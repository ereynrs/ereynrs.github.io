# Data Quality Framework

## WHAT? 
Three Stages
* Detection -> Is there a problem?
* Analysis -> Is it possible to solve it?
* Action -> How to solve it?

## WHERE?
Two Levels
* Technical level: In ODL. I.e.: verify pipelines, data types, etc.
* Knowledge level: In KG. Is the data aligned with the SME's point of view?

## WHEN?
Two options: a) ad-hoc, and b) guard mode. Guard mode seems to be the most appropiate one.
From resource link number 2:

> When should you use guard mode versus ad hoc validation?
> If we’re building a Knowledge Graph over many systems, each of which owns its data entry interfaces, then we will run ICV asynchronously on our own auditing schedule.
> For applications built to store data directly in the graph, guard mode helps move complex business logic from application software to declarative data in the Knowledge Graph.

## Use cases
### Linking Substances to Targets from CTL, by joining Biomart & ChEMBL Data
Overview:
Biomart data depicts samples without ChEMBL codes for targets, even when those ChEMBL codes are available in the ChEMBL data source. This situation could be originated by out of sync release of the different data sources.
*Note: Data with this lack of info account for less than 2% of the overall cases (almost 47k of more than 2.4M cases).*

Query to retrieve examples of this issue:
```
SELECT DISTINCT
csifc.md_chembl_id as substance_chembl_id
,csifc.standard_inchi_key as inchi_key
,beeu.chembl_id as target_chembl_id
FROM
chembl_sample_info_from_ci csifc
INNER JOIN chembl_assays ca ON
ca.assay_id = csifc.assay_chembl_id
INNER JOIN biomart_ens_entrez_uniprot beeu ON
beeu.uniprotkb_swissprot_id = ca.uniprot_id
AND beeu.organism = ca.organism
WHERE beeu.chembl_id IS NULL
```

Let's focus in one example:
substance_chembl_id |	inchi_key |	target_chembl_id 
------------------- |-----------|-----------------
CHEMBL4097563 | AAAAJHGLNDAXFP-VNKVACROSA-N | NULL
CHEMBL4097563 | AAAAJHGLNDAXFP-VNKVACROSA-N | CHEMBL1293222
CHEMBL4097563 | AAAAJHGLNDAXFP-VNKVACROSA-N | CHEMBL1293266

Two semantics are possible:
1. There is no attribute value. E.g.: a given molecule does not have a ChEMBL code assigned yet.
2. The attribute value is unknown, but it actually exists. E.g.: The data source does not know the ChEMBL for a given Therapeutic Target.

**But the "right" semantics can be only established by the domain experts. In consequence, to define a unified approach to tackle the NULL values cases is not an option.
Instead, a domain-dependent approach is requiered.**

### Resources
1. https://www.stardog.com/platform/features/data-quality-shacl/
2. https://www.stardog.com/blog/data-quality-with-icv/

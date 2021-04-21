# Data Quality Framework

## WHAT? 
**Three Stages**
* *Detection* -> Is there a problem?
* *Analysis* -> Is it possible to solve it?
* *Action* -> How to solve it?

## WHERE?
**Two Levels**
* *Technical level*: In ODL. I.e.: verify pipelines, data types, etc.
* *Knowledge level*: In KG. Is the data aligned with the SME's point of view?

## WHEN?
**Two options**
* *ad-hoc*: Manual or semi-automated inspection of the ingested data.
* *guard mode*: Data quality assessment at ingestion time. It's ingested only data passing the predefined validations.

## Use cases
### Linking Substances to Targets from CTL, by joining Biomart & ChEMBL Data
*Overview:*

Biomart data depicts samples without ChEMBL codes for targets, even when those ChEMBL codes are available in the ChEMBL data source. This situation could be originated by out of sync release of the different data sources.

*Note: Data with this lack of info account for less than 2% of the overall cases (almost 47k of more than 2.4M cases).*

Running the following query against the *ctl_consum* database allows to retrieve samples of this issue:
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

Let's focus in one sample:

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


### STEP 1. DETECTION
**Simplest strategy to tackle the issue: ad-hoc *(when)* detection *(what)* of knowledge gap *(where)*.**

Then, the technical implementation should highlight the knowledge gap:
* *Option 1*. Not to ingest linkages between substances and targets. But it hides the issue under the rug instead of highlight it.
* *Option 2*. Ingest the linkages. But ChEMBL code is used for minting the entity ID:
  * *2-A*. To create entities with randomly generated IDs of a new *kind* of Targets: the *incomplete or suspicious* ones.
  * *2-B*. To create target entities represented by blank nodes.

#### Insights
* **The Option 2-A mixes validation issues and domain modeling.**
* **Instead, the Option 2-B is aligned with the semantics of RDF blank nodes, and simultaneously, it allows to easily recognize suspicious entities.**

### STEP 2. ANALYSIS
**Simplest strategy to adress the issue: ad-hoc *(when)* analysis *(what)* of knowledge gap *(where)*.**

Running the following query against the *ctl_consum* database allows to retrieve more details of the sample before depicted:
```
SELECT DISTINCT
beeu.ensembl_gene_id
,beeu.entrez_gene_id
,beeu.gene_name
,beeu.organism
,beeu.uniprotkb_swissprot_id
FROM
chembl_sample_info_from_ci csifc
INNER JOIN chembl_assays ca ON
ca.assay_id = csifc.assay_chembl_id
INNER JOIN biomart_ens_entrez_uniprot beeu ON
beeu.uniprotkb_swissprot_id = ca.uniprot_id
AND beeu.organism = ca.organism
WHERE csifc.md_chembl_id = 'CHEMBL4097563' and beeu.chembl_id is NULL
```

Results:
ensembl_gene_id | entrez_gene_id | gene_name | organism | uniprotkb_swissprot_id
--------------- | -------------- | --------- | -------- | ----------------------
ENSG00000162711 | 114548 | NLRP3 | HOMO.SAPIENS | Q96P20

But reviewing the official ChEMBL data, the target seems to have a ChEMBL code assigned to it.
[This page](https://www.ebi.ac.uk/chembl/g/#browse/targets/filter/_metadata.related_compounds.all_chembl_ids%3A(%22CHEMBL4097563%22)%20AND%20_metadata.protein_classification.l1%3A(%22Unclassified%20protein%22)) depicts the targets for the compound under analysis (CHEMBL4097563), and the first of them (CHEMBL1741208) have the same values for the attributes depcited on the previous query's output, as can be seen [on this page](https://www.ebi.ac.uk/chembl/target_report_card/CHEMBL1741208/). 

#### Analysis insights
Same values on the *ensemble gene id*, *gene name*, *organism*, and *uniprot_swissprot_id* attributes suggest that the ChEMBL official data defines a ChEMBL code for the *NULL* target depicted in the *ctl_consum* database.

## Resources
1. https://www.stardog.com/platform/features/data-quality-shacl/
2. https://www.stardog.com/blog/data-quality-with-icv/

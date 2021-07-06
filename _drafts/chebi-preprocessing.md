# How to split ChEBI ontology between schema and actual substances

## ChEBI overview
The **Ch**emical **E**ntities of **B**iological **I**nterest (**ChEBI**) provides a structured classification
of molecular entities of biological interest focusing on _small_ chemical compounds.

A freely available dictionary of molecular entities focused on _small_ chemical compounds.
The term _molecular entity_ refers to any constitutionally or isotopically distinct atom, molecule, ion, ion pair,
radical, radical ion, complex, conformer, etc., identifiable as a separately distinguishable entity.
The molecular entities in question are either products of nature or synthetic products used to intervene in the processes of living organisms.

ChEBI incorporates an ontological classification, whereby the relationships between molecular entities or classes of
entities and their parents and/or children are specified.

ChEBI uses nomenclature, symbolism and terminology endorsed by the following international scientific bodies:
* International Union of Pure and Applied Chemistry (IUPAC)
* Nomenclature Committee of the International Union of Biochemistry and Molecular Biology (NC-IUBMB)

Molecules directly encoded by the genome (e.g. nucleic acids, proteins and peptides derived from proteins by cleavage) are not as a rule included in ChEBI.

All data in the database is non-proprietary or is derived from a non-proprietary source. It is thus freely accessible and available to anyone.
In addition, each data item is fully traceable and explicitly referenced to the original source.

The data on the [ChEBI homepage](https://www.ebi.ac.uk/chebi/init.do) is available under the Creative Commons License (CC BY 4.0).

## Goal
_**To split the ontology schema from the actual substances.**_

## Approach
The ChEBI ontology depicts all the information about substances in terms of classes and annotation properties. E.g., the _nintedanib_ substance is depicted as follows (excerpt):

````ttl
###  http://purl.obolibrary.org/obo/CHEBI_85164
<http://purl.obolibrary.org/obo/CHEBI_85164> rdf:type owl:Class ;
                                             rdfs:subClassOf <http://purl.obolibrary.org/obo/CHEBI_25248> ,
                                                             <http://purl.obolibrary.org/obo/CHEBI_33860> ,
                                                             <http://purl.obolibrary.org/obo/CHEBI_38459> ,
                                                             <http://purl.obolibrary.org/obo/CHEBI_46845> ,
                                                             <http://purl.obolibrary.org/obo/CHEBI_47989> ,
                                                             <http://purl.obolibrary.org/obo/CHEBI_62732> ,
                                                             <http://purl.obolibrary.org/obo/CHEBI_62733> ,
                                                             [ rdf:type owl:Restriction ;
                                                               owl:onProperty <http://purl.obolibrary.org/obo/RO_0000087> ;
                                                               owl:someValuesFrom <http://purl.obolibrary.org/obo/CHEBI_35610>
                                                             ] ,
                                                             [ rdf:type owl:Restriction ;
                                                               owl:onProperty <http://purl.obolibrary.org/obo/RO_0000087> ;
                                                               owl:someValuesFrom <http://purl.obolibrary.org/obo/CHEBI_38637>
                                                             ] ,
                                                             [ rdf:type owl:Restriction ;
                                                               owl:onProperty <http://purl.obolibrary.org/obo/RO_0000087> ;
                                                               owl:someValuesFrom <http://purl.obolibrary.org/obo/CHEBI_48422>
                                                             ] ,
                                                             [ rdf:type owl:Restriction ;
                                                               owl:onProperty <http://purl.obolibrary.org/obo/RO_0000087> ;
                                                               owl:someValuesFrom <http://purl.obolibrary.org/obo/CHEBI_63457>
                                                             ] ,
                                                             [ rdf:type owl:Restriction ;
                                                               owl:onProperty <http://purl.obolibrary.org/obo/RO_0000087> ;
                                                               owl:someValuesFrom <http://purl.obolibrary.org/obo/CHEBI_65207>
                                                             ] ,
                                                             [ rdf:type owl:Restriction ;
                                                               owl:onProperty <http://purl.obolibrary.org/obo/chebi#is_conjugate_base_of> ;
                                                               owl:someValuesFrom <http://purl.obolibrary.org/obo/CHEBI_85172>
                                                             ] ;
                                             <http://purl.obolibrary.org/obo/IAO_0000115> "A member of the class of oxindoles that is a kinase inhibitor used (in the form of its ethylsulfonate salt) for the treatment of idiopathic pulmonary fibrosis and cancer."^^xsd:string ;
                                             <http://purl.obolibrary.org/obo/chebi/charge> "0"^^xsd:string ;
                                             <http://purl.obolibrary.org/obo/chebi/formula> "C31H33N5O4"^^xsd:string ;
                                             <http://purl.obolibrary.org/obo/chebi/inchi> "InChI=1S/C31H33N5O4/c1-34-15-17-36(18-16-34)20-27(37)35(2)24-12-10-23(11-13-24)32-29(21-7-5-4-6-8-21)28-25-14-9-22(31(39)40-3)19-26(25)33-30(28)38/h4-14,19,32H,15-18,20H2,1-3H3,(H,33,38)/b29-28-"^^xsd:string ;
                                             <http://purl.obolibrary.org/obo/chebi/inchikey> "XZXHXSATPCNXJR-ZIADKAODSA-N"^^xsd:string ;
                                             <http://purl.obolibrary.org/obo/chebi/mass> "539.62480"^^xsd:string ;
                                             <http://purl.obolibrary.org/obo/chebi/monoisotopicmass> "539.25325"^^xsd:string ;
                                             <http://purl.obolibrary.org/obo/chebi/smiles> "COC(=O)c1ccc2\\C(=C(\\Nc3ccc(cc3)N(C)C(=O)CN3CCN(C)CC3)c3ccccc3)C(=O)Nc2c1"^^xsd:string ;
                                             <http://www.geneontology.org/formats/oboInOwl#hasDbXref> "CAS:656247-17-5"^^xsd:string ,
                                                                                                      "Drug_Central:4903"^^xsd:string ,
                                                                                                      "KEGG:D10481"^^xsd:string ,
                                                                                                      "PDBeChem:XIN"^^xsd:string ,
                                                                                                      "PMID:24657398"^^xsd:string ,
                                                                                                      "PMID:25299232"^^xsd:string ,
                                                                                                      "PMID:25338318"^^xsd:string ,
                                                                                                      "PMID:25430078"^^xsd:string ,
                                                                                                      "PMID:25474320"^^xsd:string ,
                                                                                                      "PMID:25478720"^^xsd:string ,
                                                                                                      "PMID:25527450"^^xsd:string ,
                                                                                                      "PMID:25534294"^^xsd:string ,
                                                                                                      "PMID:25571784"^^xsd:string ,
                                                                                                      "PMID:25745043"^^xsd:string ,
                                                                                                      "Reaxys:12016593"^^xsd:string ,
                                                                                                      "Wikipedia:Nintedanib"^^xsd:string ;
                                             <http://www.geneontology.org/formats/oboInOwl#hasExactSynonym> "methyl (3Z)-3-[(4-{methyl[(4-methylpiperazin-1-yl)acetyl]amino}anilino)(phenyl)methylidene]-2-oxo-2,3-dihydro-1H-indole-6-carboxylate"^^xsd:string ;
                                             <http://www.geneontology.org/formats/oboInOwl#hasOBONamespace> "chebi_ontology"^^xsd:string ;
                                             <http://www.geneontology.org/formats/oboInOwl#hasRelatedSynonym> "BIBF 1120"^^xsd:string ,
                                                                                                              "BIBF-1120"^^xsd:string ,
                                                                                                              "BIBF1120"^^xsd:string ,
                                                                                                              "nintedanib"^^xsd:string ;
                                             <http://www.geneontology.org/formats/oboInOwl#id> "CHEBI:85164"^^xsd:string ;
                                             <http://www.geneontology.org/formats/oboInOwl#inSubset> <http://purl.obolibrary.org/obo/chebi#3_STAR> ;
                                             rdfs:label "nintedanib"^^xsd:string .

[ rdf:type owl:Axiom ;
   owl:annotatedSource <http://purl.obolibrary.org/obo/CHEBI_85164> ;
   owl:annotatedProperty <http://www.geneontology.org/formats/oboInOwl#hasDbXref> ;
   owl:annotatedTarget "CAS:656247-17-5"^^xsd:string ;
   <http://www.geneontology.org/formats/oboInOwl#source> "ChemIDplus"^^xsd:string
 ] .

[ rdf:type owl:Axiom ;
   owl:annotatedSource <http://purl.obolibrary.org/obo/CHEBI_85164> ;
   owl:annotatedProperty <http://www.geneontology.org/formats/oboInOwl#hasDbXref> ;
   owl:annotatedTarget "CAS:656247-17-5"^^xsd:string ;
   <http://www.geneontology.org/formats/oboInOwl#source> "KEGG DRUG"^^xsd:string
 ] .

[ rdf:type owl:Axiom ;
   owl:annotatedSource <http://purl.obolibrary.org/obo/CHEBI_85164> ;
   owl:annotatedProperty <http://www.geneontology.org/formats/oboInOwl#hasDbXref> ;
   owl:annotatedTarget "Drug_Central:4903"^^xsd:string ;
   <http://www.geneontology.org/formats/oboInOwl#source> "DrugCentral"^^xsd:string
 ] .
````

Then, a strategy to distinguish between schema and actual substances is required.
A straighforward option to distinguish them is to split the ontology between _leaf_ and _no-leaf_ classes. 

The next SPARQL query retrieves the ChEBI codes of every _leaf_ node:
````sparql
prefix owl: <http://www.w3.org/2002/07/owl#>
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select ?chebi
where { ?leaf rdf:type owl:Class
        filter NOT EXISTS {?child rdfs:subClassOf ?leaf }
        ?leaf <http://www.geneontology.org/formats/oboInOwl#id> ?chebi .
}
````

The next SPARQL query retrieves the ChEBI codes of every _no-leaf_ node:
````sparql
prefix owl: <http://www.w3.org/2002/07/owl#>
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select ?chebi
where { ?leaf rdf:type owl:Class
        filter EXISTS {?child rdfs:subClassOf ?leaf }
        ?leaf <http://www.geneontology.org/formats/oboInOwl#id> ?chebi .
}
````

## Tools
ROBOT


## Splitting process
### Step 1. Serialize the OWL ontology as a set of triples
For a sake of readability I prefer to work with ontologies serialized as triples. Running the following command will generate the _ttl_ version of the ontology:
````bash
robot convert --input chebi.owl --output chebi.ttl
````

### Step 2. Retrieve a list of ChEBI codes of _leaf_ nodes
Running the following command will generate a _csv_ file listing the ChEBI codes of the _leaf_ nodes:
````bash
robot query --input chebi.ttl --tdb true --query getleafschebis.sparql leafschebis.csv
````
The query used is depicted as the first query in the **Approach** section.

### Step 3. Retrieve a list of ChEBI codes of _no-leaf_ nodes
Running the following command will generate a _csv_ file listing the ChEBI codes of the _no-leaf_ nodes:
````bash
robot query --input chebi.ttl --tdb true --query getnoleafschebis.sparql noleafschebis.csv
````
The query used is depicted as the second query in the **Approach** section.

### Step 4. Get an ontology of _leaf_ nodes
````bash
robot filter --input chebi.ttl --term-file leafschebis.csv --select self --axioms all --signature false --trim false --output leafs.ttl
````

### Step 5. Get an ontology of _no-leaf_ nodes
````bash
robot filter --input chebi.ttl --term-file noleafschebis.csv --select self --axioms all --signature false --trim false --output noleafs.ttl
````














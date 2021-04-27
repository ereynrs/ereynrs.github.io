# Medical Subject Headings (MeSH) 

## Summary
The Medical Subject Headings (MeSH) thesaurus is a controlled and hierarchically-organized vocabulary. It is used for indexing, cataloging, and searching for biomedical and health-related information.
MeSH can be used for information enrichment of the free text labels identified as entities of interest by the NER pipeline.
MeSH can be used as a source for text summarization and topic detection, by using the MeSH entities identified on the free text by the NER pipeline as input for the topic detection and text summarization algorithms.
MeSH RDF can be stored on an internal platform. The daily updates require automated ontology/taxonomy management capabilities, which are not available in BI at the time of writing this document.
MESH RDF latest vocabulary version - stored and managed by US NLM - can be accessed by using the SPARQL and RESTful API available endpoints.

## MeSH Overview
The Medical Subject Headings (MeSH) thesaurus is a controlled and hierarchically-organized vocabulary produced by the *US National Library of Medicine (US NLM)*. It is used for indexing, cataloging, and searching for biomedical and health-related information.
### MeSH Record Types
MeSH contains several different types of terms.

#### Descriptors (main headings)
They characterize the subject matter or content. This record type plays a central role in MeSH vocabulary as a unit of Indexing and retrieval. The descriptors are organized into a numbered tree structure or hierarchy that allows users to browse in an orderly fashion from broader to narrower topics. 
The MeSH ID for Descriptors begins with the letter 'D', e.g., the *'Stroke'* Descriptor has the MeSH ID *'D020521'*.

#### Qualifiers
They are used with descriptors and afford a means of grouping together those documents concerned with a particular aspect of a subject. For example, *Liver/drug effects* indicate that the article or book is not about the liver in general, but about the effect of drugs on the liver.
The MeSH ID for Qualifiers begin with the letter 'Q', e.g., the *'diagnosis'* Qualifier has the MeSH ID *'Q000175'*.

#### Supplementary Records also called Supplementary Chemical Records (SCRs)
They are used to index chemicals, drugs, and other concepts such as rare diseases. Unlike Descriptors, SCRs are not organized in a tree hierarchy. Instead, each SCR is linked to one or more Descriptors by the Heading Mapped To (HM) field in the SCR. They also include an Indexing Information (II) field that is used to refer to other descriptors that are from related topics.
The MeSH ID for Qualifiers begin with the letter 'C', e.g., the *'Aeron'* SCR has the MeSH ID *'C025735'*.
Four classes of SCRs exist.
* *Class 1 Supplementary Records - Chemicals.* These records are dedicated to chemicals and are primarily heading mapped to the D tree descriptors.
* *Class 2 Supplementary Records - Protocols.* These records are dedicated to Chemotherapy Protocols. They are heading mapped to the MeSH heading "Antineoplastic Combined Chemotherapy Protocols" and to chemicals used in the protocols found in D tree descriptors.
* *Class 3 Supplementary Records - Diseases.* These records are dedicated to diseases and are primarily heading mapped to the C tree descriptors and anatomical headings found in the A tree.
* *Class 4 Supplementary Records - Organisms.* These records are dedicated to organisms (e.g., viruses) and are primarily heading mapped to the B tree organism descriptors.

#### Entry terms: 
Sometimes called "See cross-references", are synonyms, near-synonyms alternate forms, and other closely related terms in a MeSH record. 

Entry terms are not always strictly synonymous with the preferred term in the record or with each other. However, Entry terms are generally used interchangeably with the preferred term and are equivalent to the preferred term for purposes of cataloging, indexing, and retrieval. For example, the Entry terms for the preferred term *Heart Arrest* range from *Arrest, Heart* to a simple inversion or less obvious synonyms such as *Heart Arrest* and *Asystole*. They are also a means by which the MeSH thesaurus can be enriched.

The MeSH ID for Entry terms begin with the letter 'T', e.g., the *'Stroke'* SCR has the MeSH ID *'T007470'*.

There are also informative references suggesting other MeSH Descriptors that relate to the subject and that may be useful in indexing, cataloging, or searching a particular topic.

The *See Related (also used as See Also)* references are used for a variety of relationships between Descriptor records where a user of one Descriptor is reminded of another Descriptor which may be of interest. For example, the relationship may be between a disease and its cause: *Factor XIII Deficiency see related Factor XIIIa*.
The *Consider Also* references to other Descriptors having related linguistic roots, for example, *Brain consider also terms at CEREBR- and ENCEPHAL-*.

## MeSH Descriptors Tree Structure
MeSH descriptors are organized in 16 categories, identified by letters as depicted following:

* A - Anatomy
* B - Organisms
* C - Diseases
* D - Chemicals and Drugs
* E - Analytical, Diagnostic and Therapeutic Techniques, and Equipment
* F - Psychiatry and Psychology
* G - Phenomena and Processes
* H - Disciplines and Occupations
* I - Anthropology, Education, Sociology, and Social Phenomena
* J - Technology, Industry, and Agriculture
* K - Humanities
* L - Information Science
* M - Named Groups 
* N - Health Care 
* V - Publication Characteristics
* Z - Geographicals
Within each subcategory, descriptors are arranged hierarchically from most general to most specific in up to 13 hierarchical levels. 

**Each MeSH descriptor appears in at least one place in the trees and may appear in as many additional places as may be appropriate.**

Those who use MeSH should find the most specific MeSH descriptor that is available to represent each concept of interest.

## MeSH Concept Structure
Terms in a MeSH record which are strictly synonymous with each other are grouped in a category called a *Concept*. 
Each MeSH Descriptor consists of one or more Concepts, and each Concept consists of one or more synonymous terms. Within each Concept, the terms are synonymous with each other. In contrast, the terms in one Concept are not strictly synonymous with terms in another Concept, even in the same record. 
Each Concept has a Preferred Term, which is also said to be the name of the Concept. And each record has a Preferred Concept. The Descriptor name - the term most often used to refer to the Descriptor - is the Preferred Term of the preferred Concept. *Scope Notes* are properly attributed to Concepts, while the *Annotation* applies to the record level.
Example:
<block>
  Cardiomegaly [Descriptor]
     Cardiomegaly [Concept, Preferred]
          Cardiomegaly [Term, Preferred]
          Enlarged Heart [Term]
          Heart Enlargement [Term]
     Cardiac Hypertrophy [Concept, Narrower]
          Cardiac Hypertrophy [Term, Preferred]
          Heart Hypertrophy [Term]
 <block>
   
## MeSH Updates
New concepts are constantly emerging, old concepts are in a state of flux, and terminology and usage are modified accordingly. To accommodate these changes, descriptors must be added to, changed, or deleted from MeSH with adjustments in the related hierarchies,
* **Descriptors** and **Qualifiers** are released annually to coincide with the calendar year. Typically the next year's data is released in late November of the prior year to allow time for MeSH users to update their systems.
* **Supplemental Records (SCRs)** are updated continually and released daily, Monday through Friday throughout the year. A yearly cycle of SCR updates is also done to synchronize SCRs with additions, subtractions, and changes made to Descriptor and Qualifiers.
Generally, only Supplementary Concept Records (SCRs) are regularly modified but if changes in Descriptors and Qualifiers are made, these are also included in the daily updates.

## MeSH and Semantic Technologies
MeSH RDF is a linked data representation of the MeSH biomedical vocabulary. It is produced by the US NLM, including:
1- a downloadable file in RDF N-Triples format, 
2- a SPARQL query editor (web interface), 
3 - a SPARQL endpoint (API), 
4 - and a RESTful interface for retrieving MeSH data.
The RDF downloadable file (item 1) allows storing the MeSH vocabulary on an internal platform. However, the daily updates detailed in Section 1.4 implies such a platform requires ontology/taxonomy management capabilities to automate the version control and automatic ingestion of changes processes. On the other side, SPARQL and RESTful API endpoints allow getting access to the latest vocabulary version stored and managed by US NLM.

## MeSH and NLP
There are two ways the GIHK NLP pipeline can leverage the MeSH vocabulary:
### For information enrichment
Performing information enrichment on the free text labels identified as entities of interest by the NER pipeline.

* *Step 1.* NER pipeline identifies the entities of interest.
* *Step 2.* NER pipeline links the entities to MeSH IDs.
* *Step 3.* Additional information is retrieved from MeSH vocabulary through the MeSH IDs

### As a source for text summarization and topic detection
MeSH entities identified on the free text by the NER pipeline are used as input for the topic detection and text summarization algorithms.
* *Step 1.* NER pipeline identifies the entities of interest.
* *Step 2.* NER pipeline links the entities to MeSH IDs.
* *Step 3.* Additional information is retrieved from MeSH vocabulary through the MeSH IDs
* *Step 4.* MeSH IDs and enriched information (outputs of *Step 3*) are used as input for the topic detection and text summarization algorithms.






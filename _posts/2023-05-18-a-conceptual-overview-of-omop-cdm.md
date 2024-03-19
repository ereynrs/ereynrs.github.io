---
layout: post
title: A Conceptual Overview of OMOP CDM
---
*Written by [Emiliano Reynares](https://www.linkedin.com/in/ereynrs/)*

The Observational Medical Outcomes Partnership (OMOP) Common Data Model (CDM) is a foundation for harmonizing and standardizing observational data, paving the way for efficient analyses and generating reliable evidence in healthcare research. Grasping its core principles and components enables the design of more robust data models, crafting effective data integration strategies, and working towards data quality and interoperability across diverse healthcare systems. 

In this post I share my understanding of OMOP CDM by exploring its core concepts and uncovering the pivotal role played by standardized vocabularies.

## Who produces and supports OMOP CDM?
A notable example of collaboration in observational research was the Observational Medical Outcomes Partnership (OMOP). OMOP was a public-private partnership, chaired by the US Food and Drug Administration, administered by the Foundation for the National Institutes of Health, and funded by a consortium of pharmaceutical companies that collaborated with academic researchers and health data partners to establish a research program that sought to advance the science of active medical product safety surveillance using observational healthcare data.

Recognizing the technical challenges of conducting research across disparate observational databases in both a centralized environment and a distributed research network, the team designed the OMOP Common Data Model (CDM) as a mechanism to standardize the structure, content, and semantics of observational data and to make it possible to write statistical analysis code once that could be re-used at every data site. The OMOP experiments demonstrated it was feasible to establish a common data model and standardized vocabularies that could accommodate different data types from different care settings and represented by different source vocabularies in a manner that could facilitate cross-institutional collaboration and computationally-efficient analytics.

When the OMOP project had been completed, having fulfilled its mandate to perform methodological research to inform the FDA’s active surveillance activities, the team recognized the end of the OMOP journey needed to become the start of a new journey together. From this insight, OHDSI was born.

Observational Health Data Sciences and Informatics (OHDSI), pronounced as "Odyssey", is an open-science community dedicated to enhancing health outcomes by enabling the collaborative generation of evidence for informed healthcare decisions and improved patient care. OHDSI conducts methodological research to establish best practices in utilizing observational health data, develops open-source analytics software to implement these practices consistently, transparently, and reproducibly, and applies these tools and practices to address clinical inquiries and generate evidence for healthcare policy and patient care guidance.

Since its establishment in 2014, OHDSI has grown to include over 2,500 collaborators from diverse stakeholders, including academia, the medical product industry, regulators, government, payers, technology providers, health systems, clinicians, patients, and experts from various disciplines such as computer science, epidemiology, statistics, biomedical informatics, health policy, and clinical sciences.

As of August 2019, OHDSI has established a data network consisting of more than 100 healthcare databases from over 20 countries. This distributed network approach, facilitated by the OMOP CDM (Observational Medical Outcomes Partnership Common Data Model), enables research without the need to share patient-level data between individuals or organizations. Instead, research questions are formulated as study protocols accompanied by analysis code, generating aggregated summary statistics. Only these summary statistics are shared among collaborating partners. Each data partner retains autonomy over their patient-level data and adheres to the data governance policies of their respective institutions.

The OHDSI developer community has created a comprehensive library of open-source analytics tools based on the OMOP CDM. Additionally, OHDSI developers have built applications to facilitate OMOP CDM adoption, assess data quality, and support OHDSI network studies. All OHDSI tools are open source and publicly accessible via Github.

## Why a common data model?
The observational databases differ in both purpose and design. For instance, Electronic Medical Records (EMR) are aimed at supporting clinical practice at the point of care, while administrative claims data are built for the insurance reimbursement processes. Since they are collected for different purposes, they result in diverse logical organizations and physical formats and the terminologies used to describe medicinal products and clinical conditions vary from source to source. 
The CDM allows accommodating both administrative claims and EHR, which enables users to generate evidence by transforming the data contained within those databases into a common data model as well as using common terminologies, vocabularies, and coding schemes, with the end goal of performing systematic analyses through a library of standard analytic routines. There are three main use cases for those systematic analyses.

Firstly, the CDM allows for improving clinical characterization for a comprehensive understanding of a disease's natural history, treatment utilization, and quality improvement.

Secondly, population-level effect estimation becomes achievable through the application of causal inference methods for medical product safety surveillance and comparative effectiveness. By transforming data from different sources into the CDM's standardized format, researchers can conduct reliable analyses and assess the impact of medical interventions on a larger scale, ensuring patient safety and informed decision-making.

Lastly, the CDM empowers patient-level prediction using machine learning algorithms for precision medicine and disease interception. 

Traditionally, extracting data for the analysis required complex access controls and strict data use agreements. However, with a common data standard like the CDM, the need for data extraction is alleviated. Instead, analytics can be executed directly on the data in its native environment, reducing complexities and streamlining the research process.

OHDSI provides a variety of resources and open-source tools for converting datasets into the OMOP CDM. Once converted, researchers can generate evidence using standardized analytics, such as data quality assessment, medical product safety surveillance, comparative effectiveness analysis, quality of care evaluation, and patient-level predictive modeling.

## The OMOP CDM
CDM is purposefully designed to cater to typical observational research requirements. Its key objectives include (1) identifying patient populations based on healthcare interventions and outcomes, (2) characterizing these populations across various parameters, (3) predicting individual patient outcomes, and (4) estimating the effects of interventions on a population. To achieve these goals, the CDM adheres to the essential design principles outlined below.

Firstly, the CDM prioritizes suitability for analysis over operational needs, ensuring that data is organized optimally for research purposes rather than the day-to-day requirements of healthcare providers or payers (__suitability for purpose__ principle). Moreover, the model emphasizes data protection by limiting sensitive information that could compromise patient identities, with exceptions when the research expressly requires more detailed information (data protection principle).

The CDM's domain design follows a person-centric relational data model, capturing essential details such as the person's identity and a corresponding date for each record (__design of domains__ principle). Domains are identified and separately defined in an entity-relationship model if they serve specific analytical use cases and possess attributes not applicable elsewhere. For other data, the CDM adopts an observation table structured in an entity-attribute-value format (__rationale for domain__ principle).

To standardize the content of records, the CDM relies on Standardized Vocabularies that encompass all necessary healthcare concepts (__standardized vocabularies__ principle). Whenever possible, these concepts are derived from existing national or industry standardization initiatives (__reuse existing vocabularies__ principle). 

Additionally, the CDM preserves the source codes alongside the mapped standardized codes to ensure comprehensive information retention (__maintaining source codes__ principle).

The CDM exhibits technology neutrality, allowing its implementation in any relational database without imposing specific technological requirements (__technology neutrality__ principle). It is designed for scalability, efficiently handling data sources of varying sizes, including databases with millions of persons and billions of clinical observations (__scalability__ principle).

Backward compatibility is a key consideration, with clear documentation of changes from previous versions of the CDM available in the [GitHub repository](https://github.com/OHDSI/CommonDataModel). This ensures that older versions can be easily recreated from the current version, preserving all previously present information (__backward compatibility__ principle).

## Domains, Concepts, and Vocabularies
Domains are used to classify events. Then, the events are stored in domain-specific tables and depicted by Standard Concepts which in turn are defined by the Standardized Vocabularies.

A Concept is linked to at most a single Domain, and such a linkage defines which table they are recorded in. Even though the correct Domain assignment is subject to debate in the community, this strict correspondence rule assures that there is always an unambiguous location for any Concept.

### Using Domains and Concepts to represent the content
Each concept is linked to a single domain to guarantee each of them has a clearly defined and unambiguous location within the CDM. For instance, concepts related to signs, symptoms, and diagnoses belong to the Condition Domain and are recorded in the corresponding event recording table.
The concepts are represented by the Standard Concepts defined by the Standardized Vocabularies, which ensures consistency and uniformity in the depiction of events.
The event recording tables depict the concepts by referencing the CONCEPT table, which serves as a universal reference table for all CDM instances by providing detailed information about each concept, e.g.: name, domain, and class. Standardized Vocabularies also house information about Concept Relationships, Concept Ancestors, and other related aspects of Concepts.

The CDM encompasses a total of 30 different domains, each of which is associated with a specific number of concepts. An overview of these domains and the corresponding number of related concepts are depicted in this [table](https://ohdsi.github.io/TheBookOfOhdsi/CommonDataModel.html#domains).

### Relationships between concepts
Concepts in the OMOP CDM can have defined relationships, regardless of their domain or vocabulary. The nature of these relationships is indicated by a unique alphanumeric ID and is symmetrical, meaning that for each relationship, an equivalent relationship exists. For example, the "Maps to" relationship has an opposite relationship called "Mapped from."

#### Mapping relationships
Mapping relationships play a crucial role in harmonizing how clinical events are represented in the OMOP CDM because they facilitate crosswalks between equivalent concepts. There are two of them. (1) "Maps to" and "Mapped from", where Standard Concepts are mapped to themselves, while non-standard concepts are mapped to Standard Concepts. "Maps to value" and "Value mapped from" that map a concept representing a value. This allows splitting values for OMOP CDM tables using an entity-attribute-value (EAV) model. The "Maps to" relationship maps the source to the attribute concept, while the "Maps to value" relationship maps to the value concept.

#### Equivalence relationships
"Equivalent concepts" denote concepts with the same meaning, and their hierarchical descendants cover the same semantic space. If an equivalent concept is unavailable and the concept is not Standard, it is mapped to a broader concept ("up-hill mappings"). Up-hill mappings are used when the loss of information is considered irrelevant for standard research use cases.

In cases where the source concept is a pre-coordinated combination of two conditions, it may be mapped to more than one Standard Concept. For example, the ICD9CM code "070.43" for "Hepatitis E with hepatic coma" is mapped to both SNOMED codes "235867002" for "Acute hepatitis E" and "72836002" for "Hepatic Coma."

#### Hierarchical relationships
Hierarchical relationships are defined through the "Is a" - "Subsumes" relationship pair. These relationships establish hierarchies where child concepts inherit all attributes of the parent concept and may have additional attributes or more precise definitions. Concepts can have multiple parents and multiple child concepts. Currently, comprehensive hierarchies exist primarily for the drug and condition domains, while the procedure, measurement, and observation domains are still under construction. Hierarchical ancestry is particularly useful in the drug domain for browsing drugs based on ingredients or drug classes, regardless of country of origin or brand name.

#### In- and inter-vocabularies relationships
Internal vocabulary relationships are typically provided by the vocabulary provider and define relationships between clinical events, aiding in information retrieval. Relationships between concepts from different vocabularies are labeled as "Vocabulary A - Vocabulary B equivalent." These relationships are either supplied by the original vocabulary source or built by the OHDSI Vocabulary team. While they can serve as approximate mappings, they are generally less precise than well-curated mapping relationships. High-quality equivalence relationships are always duplicated using the "Maps to" relationship.

## The Standardized Vocabularies
Vocabularies are controlled terminologies, hierarchies, or ontologies agreed upon by healthcare communities to capture, classify, and analyze patient data. They are maintained by public and government agencies, e.g.: World Health Organization (WHO) manages the International Classification of Disease (ICD) and country-specific versions are produced by local governments, e.g.: ICD10CM (USA), ICD10GM (Germany). Private sector entities also use vocabularies for EHR systems and medical insurance claim reporting.

This myriad of vocabularies across countries, regions, healthcare systems, and institutions hinders system interoperability. Standardization plays a vital role in enabling patient data exchange, facilitating global health data analysis, and supporting systematic and standardized research.

The OHDSI initiative developed the OMOP CDM as a global standard for observational research. Within the CDM, the OMOP Standardized Vocabularies serve two main purposes: (1) to act as a common repository of vocabularies used in the community, and (2) to provide standardization and mapping for research purposes. The OMOP Standardized Vocabularies are freely available and serve as the mandatory reference table for every OMOP CDM instance.

The OMOP CDM supports a wide range of vocabularies, with each vocabulary having a unique alphanumeric ID. OHDSI currently supports 111 vocabularies, of which 78 are adopted from external sources, while the rest are OMOP-internal vocabularies. OHDSI prioritizes the adoption of existing vocabularies rather than constructing new ones due to their widespread use in observational data and the complexity of constructing and maintaining comprehensive vocabularies. OHDSI mainly produces internal administrative vocabularies, such as Type Concepts, except for the RxNorm Extension, which covers drugs used outside the United States.

In the OMOP CDM, all clinical events are expressed as concepts utilized to represent various aspects of a patient's healthcare experience, including conditions, procedures, drug exposures, and administrative information related to healthcare systems. Each concept is assigned a unique concept ID and has an associated name, which is typically in English. If a source vocabulary includes multiple names for a concept, the most expressive one is selected, but the remaining names  - including non-English versions - are also stored. Whilst the source vocabularies tend to combine codes of different domains, each concept is assigned to a domain, which is identified by a short alphanumeric string; e.g., "Condition," "Drug," "Procedure". The concept-to-domain assignment is a heuristic-based process performed during vocabulary ingestion.

To address the evolution of concepts the CDM also includes concept lifespan information, since there are a few vocabularies that reuse concept codes instead of deprecating them.

Some vocabularies organize concepts into classes. Concept classes can represent vertical divisions or horizontal levels within hierarchical structures. While a single concept is designated as the standard for a given clinical event, non-standard and classification concepts are included in the Standardized Vocabularies because they are found in the source data - that’s why they are called Source Concepts-, and they can be used to perform hierarchical queries, respectively.

<img src="/_assets/hierarchy_concepts.png" alt="Standard, non-standard, and classification concepts." width="100%">
<div class ="caption">
Standard, non-standard source and classification concepts and their hierarchical relationships in the condition domain. SNOMED is used for most standard condition concepts (with some oncology-related concepts derived from ICDO3), MedDRA concepts are used for hierarchical classification concepts, and all other vocabularies contain non-standard or source concepts, which do not participate in the hierarchy. (Image source: The Book of OHDSI)
</div>

A concept is designated as a standard, non-standard, or classification concept by analyzing each domain at the vocabulary level according to the quality of the concepts, the built-in hierarchy, and the declared purpose of the vocabulary. In consequence, there is no standard vocabulary since not all concepts of a vocabulary are used as Standard Concepts.

## A OMOP CDM instantiation example
Let’s delve into one patient's experience with endometriosis, a painful condition characterized by the presence of uterine lining cells outside the uterus. This example will illustrate how such a case can be represented within the CDM. You can explore Lauren's firsthand account at [Lauren's Story](https://endometriosis-uk.org/laurens-story) to get a more comprehensive understanding of the patient's story, including their journey and personal insights.

Endometriosis is known to cause significant discomfort and is associated with various complications, such as infertility, as well as bowel and bladder issues. This real-life case study sheds light on [how their medical data can be captured](https://ohdsi.github.io/TheBookOfOhdsi/CommonDataModel.html#running-example-endometriosis) within the Common Data Model.

## Glossary
#### Clinical event
A clinical event refers to any occurrence or incident related to a patient's healthcare experience. It encompasses various types of data that capture different aspects of the event. The specific data included in a clinical event can vary depending on the context and purpose of the analysis.

Some common examples of data that may be part of a clinical event include Conditions, Procedures, Drug Exposures, Observations, Visits, Care Sites, and Other Administrative Information - administrative details about the healthcare system, such as insurance claims, billing codes, or healthcare provider information-.

These are just some examples, and the scope of a clinical event can vary depending on the specific analysis or research question being addressed.

#### Observation
A clinical fact about a Person obtained in the context of examination, questioning, or a procedure. 

#### Observational data
Patient-level data captured during the routine course of clinical care to cater to these purposes:
1. To facilitate research, often in the form of survey or registry data
2. to support the conduct of healthcare (EHR), or 
3. to manage the payment for healthcare, i.e.:claims data. 
All three are routinely used for clinical research, the latter two as secondary use data, and all three types have their unique formatting and encoding of the content.

#### Observational study
A study where the researcher has no control over the intervention.

## Resources
* [The Book of OHDSI](https://ohdsi.github.io/TheBookOfOhdsi/).
* CDM v5.4 [diagram](https://ohdsi.github.io/CommonDataModel/cdm54erd.html). The interactive version is available [here](http://omop-erd.surge.sh/).
* OMOP CDM [GitHub repository](https://github.com/OHDSI/CommonDataModel).
* [Athena](http://athena.ohdsi.org/). The OHDSI standardized vocabularies lookup tool.

##### Visit my [LinkedIn Profile](https://www.linkedin.com/in/ereynrs/)
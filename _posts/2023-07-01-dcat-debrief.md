---
layout: post
title: A Data Catalog Vocabulary (DCAT) Debrief
---
*Written by [Emiliano Reynares](https://www.linkedin.com/in/ereynrs/)*

Sharing data resources across organizations, researchers, governments, and citizens necessitates the use of metadata, regardless of data openness. DCAT (Data Catalog Vocabulary) is an RDF vocabulary designed for publishing data catalogs on the Web. 

DCAT enables publishers to describe datasets and data services using a standardized model and vocabulary. This promotes discoverability and federated search across multiple catalogs, increasing accessibility to datasets and data services.

In this post, I debrief the DCAT v3 specification by highlighting its core features.

## What's DCAT?
DCAT (Data Catalog Vocabulary) is an RDF vocabulary designed for publishing data catalogs on the Web. Initially developed by the [Digital Enterprise Research Institute (DERI)](http://www.deri.ie/), refined by the [eGov Interest Group](http://www.w3.org/egov/), and standardized in 2014 by the [W3C Government Linked Data (GLD) Working Group](http://www.w3.org/2011/gld/). DCAT 2, a recommended revision, was later developed by the [W3C Dataset Exchange Working Group](https://www.w3.org/2017/dxwg/).
DCAT enables publishers to describe datasets and data services using a standardized model and vocabulary. This promotes discoverability and federated search across multiple catalogs, increasing accessibility to datasets and data services. DCAT 3, a major revision, introduces enhancements such as the checksum property, versioning support, and representation of the Dataset Series.

Since data may be accessed through services allowing the selection of specific data subsets or combinations, DCAT also facilitates the inclusion of data access service descriptions in a catalog. DCAT does not assume any specific data serialization format (e.g., Excel, XML, RDF), but it distinguishes between the abstract dataset and its various distributions.

DCAT can be complemented with other vocabularies, like VoID, to provide additional format-specific details. For instance, VoID properties can express statistics about an RDF dataset when used within DCAT.
To explore the latest version (V3 Working Draft) and associated resources, visit the official [DCAT website](https://www.w3.org/TR/vocab-dcat-3/).

:thinking:
_At the moment of analyzing a DCAT implementation, the first point to decide is about the DCAT version to apply. Recommended one (v2) or the latest WD (v3)?_

The specification also establishes the criteria a data catalog must meet to ensure conformity with DCAT:

1. It must organize data into datasets, distributions, and data services.
2. Both the catalog and the cataloged resources and distributions must be described by an RDF artifact.
3. Every relevant metadata field about the catalog, resources, and distributions is described by the RDF artifact through the appropriate DCAT classes and properties. If a specific class or property is missing, an alternative approach is used.
4. Every class and property defined in DCAT is utilized in a manner consistent with the declared semantics in the specification.

DCAT-compliant catalogs have the flexibility to incorporate non-DCAT metadata fields and additional RDF data in the catalog's RDF description.

DCAT profiles, also known as Application Profiles in the [Dublin Core](https://www.dublincore.org/about/) community, introduce additional constraints to DCAT to satisfy requirements in specific domains. In some cases, a profile extends another DCAT profile by adding classes and properties for metadata fields not covered.

Some of the more relevant DCAT profiles are: 
* DCAT-AP: The DCAT application profile for data portals in Europe,
* GeoDCAT-AP: Geospatial profile of DCAT-AP, and
* StatDCAT-AP: Statistical profile of DCAT-AP.
A catalog conforming to a profile is considered to conform to DCAT as well. Profile constraints may include:
* Cardinality requirements specifying a minimum set of mandatory metadata fields.
* Sub-classes and sub-properties of standard DCAT classes and properties.
* Classes and properties for additional metadata fields not covered in the DCAT vocabulary specification.
* Controlled vocabularies or sets of Internationalized Resource Identifiers (IRIs) as acceptable property values.
* Requirements for specific access mechanisms (RDF syntaxes, protocols) to the catalog's RDF description.

## The DCAT model
DCAT is based on seven classes:
* The _Catalog_ class is a dataset in which each individual item is a metadata record describing some resource, i.e.: collections of metadata about datasets, data services, or other resource types.
* An instance of the _Resource_ class represents a dataset, a data service, or any other resource that may be described by a metadata record in a catalog. Resources in a catalog should be instances of one of their child classes - i.e.: _Dataset_, _DataService_, _Catalog_, or any other sub-class defined in a DCAT profile or application.
* A _Dataset_ represents a collection of data, published or curated by a single agent or identifiable community. The notion of the DCAT dataset is broad and inclusive, with the intention of accommodating resource types arising from all communities.
* A _Distribution_ represents an accessible form of a dataset such as a downloadable file.
* A _DataService_ represents a collection of operations accessible through an interface (API) that provides access to one or more datasets or data processing functions.
* A _DatasetSeries_ is a dataset that represents a collection of datasets that are published separately, but share some characteristics that group them.
* A _CatalogRecord_ represents a metadata record in the catalog, primarily concerning provenance information - the registration information, such as who added the record and when-. Its use is considered optional.

<img src="/_assets/dcat_model.svg" alt="DCAT model." width="100%">
<div class ="caption">
Overview of the DCAT model, showing the classes of resources that can be members of a Catalog and the relationships between them. Except where specifically indicated, DCAT does not provide cardinality constraints. (Image source: W3C DCAT WD)
</div>

While the diagram uses UML-style class notation it should be interpreted following the usual RDF open-world assumptions around the presence/absence of properties, relationships, and their cardinality. A basic DCAT Catalog serialization is shown [here](https://www.w3.org/TR/vocab-dcat-3/#basic-example). The properties of the previously mentioned classes, and some additional ones, are fully specified [here](https://www.w3.org/TR/vocab-dcat-3/#vocabulary-specification). DCAT supports inverse properties but they MAY be used only in addition to those described in the section above linked and MUST NOT be used to replace them; because the purpose is to ensure interoperability with systems not making use of OWL reasoning. The supported inverse properties are described in [this table](https://www.w3.org/TR/vocab-dcat-3/#inverse-properties).

### Identifiers
In the scientific and data provider communities, various identifiers are used for publications, authors, and data. DCAT relies primarily on persistent HTTP IRIs (Internationalized Resource Identifiers) as a powerful means of making identifiers actionable. Many identifier schemes can be represented as dereferenceable HTTP IRIs, and some of them even provide machine-readable metadata, such as [DOI (Digital Object Identifiers)](https://www.doi.org/) and [ORCID (Open Researcher and Contributor IDs)](https://orcid.org/).

However, there are cases where data providers may still need to refer to legacy identifiers, non-HTTP dereferenceable identifiers, and identifiers obtained from local or third-party sources. In such situations, [Dublin Core](https://www.dublincore.org/about/) and [Asset Description Metadata Schema (ADMS)](https://www.w3.org/TR/vocab-adms/) terms can be helpful.

The property [dcterms:identifier](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_identifier) in DCAT explicitly allows for the inclusion of both HTTP IRIs and legacy identifiers. On the other hand, the property [adms:identifier](https://www.w3.org/TR/vocab-adms/#adms-identifier) can be used to express other locally minted or externally provided identifiers, as long as they are globally unique and stable. There are some examples [here](https://www.w3.org/TR/vocab-dcat-3/#dereferenceable-identifiers), and also [best practices](https://www.w3.org/TR/dwbp/#DataIdentifiers) for data identifiers and [how to manage duplicates](https://joinup.ec.europa.eu/release/dcat-ap-how-manage-duplicates).

If identifiers are not HTTP dereferenceable, common identifier types can be represented as RDF datatypes or custom OWL datatypes, ensuring compatibility with the RDF data model.

### Qualified relationships
Qualified relationships play a crucial role in enhancing the description of datasets and data services within DCAT. While DCAT already provides elements to capture various aspects, there are situations where additional information is needed to fully express the semantics of certain relationships. However, introducing a multitude of sub-properties under existing properties like [dcterms:relation](https://www.w3.org/TR/vocab-dcat-3/#Property:relationship_relation) or [dcterms:contributor](http://purl.org/dc/terms/contributor) would result in an overwhelming number of properties, making it impractical. Additionally, the complete range of potential roles and relationships remains unknown.

The typical approach to address such a challenge is to introduce additional resources that carry parameters to qualify the relationships. These qualified forms enable the inclusion of additional roles within a relationship and the attachment of other aspects, such as applicable time intervals when describing the relationship using specific nodes.

The DCAT vocabulary incorporates additional forms to fill gaps in qualified terms, drawing from the [Provenance Ontology](https://www.w3.org/TR/prov-o/). The figure below provides a summary of these additional forms. You can find specific examples illustrating the usage of these qualified relationships [here](https://www.w3.org/TR/vocab-dcat-3/#qualified-forms).

<img src="/_assets/qualified_relationships.svg" alt="DCAT qualified relationships." width="100%">
<div class ="caption">
Qualified relationships support an extensible set of roles relating resources to agents or to other resources (Source: W3C DCAT WD)
</div>

### Temporal and spatial properties
DCAT provides a framework for describing various temporal and spatial properties of resources. Here's an overview of the properties supported.

Temporal Properties:
1. [dcterms:issued](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_release_date) is used to indicate the release time of a resource. Typically, the value is represented as an [xsd:date](https://www.w3.org/TR/xmlschema11-2/#date).
2. [dcterms:modified](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_update_date) is used to denote the revision or update time of a resource. The value is usually encoded as an [xsd:date](https://www.w3.org/TR/xmlschema11-2/#date).
3. [dcterms:accrualPeriodicity](https://www.w3.org/TR/vocab-dcat-3/#Property:dataset_frequency) is used to specify the update schedule for a resource. It is recommended to use a value from a controlled vocabulary like the [Dublin Core Collection Description Frequency Vocabulary](http://www.dublincore.org/specifications/dublin-core/collection-description/frequency/).
4. [dcat:temporalResolution](https://www.w3.org/TR/vocab-dcat-3/#Property:dataset_temporal_resolution) indicates the minimum temporal separation between items in a dataset. The value is represented as an xsd:duration. The combination of update schedule and temporal resolution allows for describing different types of time-series data effectively.
5. [dcterms:temporal](https://www.w3.org/TR/vocab-dcat-3/#Property:dataset_temporal) is used to define the temporal extent of a dataset. The value is expressed as a [dcterms:PeriodOfTime](http://purl.org/dc/terms/PeriodOfTime).

Spatial Properties:
1. [dcat:spatialResolutionInMeters](https://www.w3.org/TR/vocab-dcat-3/#Property:dataset_spatial_resolution) allows specifying the minimum spatial separation between items in a dataset. The value is a decimal number, i.e.: [xsd:decimal](https://www.w3.org/TR/xmlschema11-2/#decimal).
2. [dcterms:spatial](https://www.w3.org/TR/vocab-dcat-3/#Property:dataset_spatial) is used to indicate the spatial extent of a dataset. The value is represented as a [dcterms:Location](http://purl.org/dc/terms/Location). Various options for expressing location details are recommended in the [Class:Location](https://www.w3.org/TR/vocab-dcat-3/#Class:Location).

By utilizing these temporal and spatial properties, DCAT enables comprehensive and standardized descriptions of resources with regard to their release time, update schedule, temporal and spatial resolutions, and extents.

### Dataset series
A dataset series refers to a collection of interrelated data that are published separately. The creation and organization of dataset series vary across domains, lacking common rules and criteria. DCAT acknowledges this diversity and encourages the adoption of guidance, domain-specific practices, and community standards. Typically, datasets are grouped into series based on factors such as their size, characteristics, publishing process, and usage.
DCAT elevates dataset series to a prominent position within data catalogs by introducing the [dcat:DatasetSeries](https://www.w3.org/TR/vocab-dcat-3/#Class:Dataset_Series) class as a subclass of [dcat:Dataset](https://www.w3.org/TR/vocab-dcat-3/#Class:Dataset). The relationship between datasets and dataset series is established using the [dcat:inSeries](https://www.w3.org/TR/vocab-dcat-3/#Property:dataset_in_series) property. Dataset series can also have hierarchical structures, with one series being a member of another.

Over time, dataset series may expand by including new datasets. DCAT provides properties such as [dcat:first](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_first), [dcat:prev](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_previous), [dcat:next](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_previous), and [dcat:last](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_last) to indicate the ordering of datasets within a series.

Properties related to dataset series can be categorized into two groups: (1) those that describe the series itself and (2) those that reflect dimensions described in child dataset metadata. The latter follows upstream inheritance, where property values of child datasets are inherited by their parent, the dataset series. Consequently, the dataset series encompasses the union of properties specified in its child datasets. For example, the temporal and spatial coverage of the series is derived from the temporal and spatial coverage of all datasets within the series.

DCAT also recommends the following practices for dataset series: (1) setting the creation date ([dcterms:created](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/terms/created/)) of the series as the creation date of its first dataset, (2) setting the publication date ([dcterms:issued](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_release_date)) of the series as the publication date of its first dataset, and (3) setting the update date ([dcterms:modified](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_update_date)) of the series as the publication or update date of its last dataset.

## DCAT and Schema.org
Alignment with [schema.org](https://www.w3.org/TR/vocab-dcat-3/#bib-schema-org) is crucial for ensuring the visibility and discoverability of datasets and services through general-purpose web search services like Google's [Dataset Search](https://g.co/datasetsearch). These services heavily rely on metadata, incorporating both schema.org and DCAT structured descriptions.

Data providers and catalog publishers who desire their datasets and services to be exposed through these indexes are particularly interested in understanding the relationship between DCAT and schema.org.

To facilitate this alignment, a recommended mapping from the revised DCAT to schema.org version 3.4 [is available in an RDF file](https://w3c.github.io/dxwg/dcat/rdf/dcat-schema.ttl). This mapping is axiomatized using predicates such as [rdfs:subClassOf](https://www.w3.org/TR/2014/REC-rdf-schema-20140225/#ch_subclassof), [rdfs:subPropertyOf](https://www.w3.org/TR/2014/REC-rdf-schema-20140225/#ch_subpropertyof), [owl:equivalentClass](https://www.w3.org/TR/2012/REC-owl2-syntax-20121211/#Equivalent_Classes), owl:equivalentProperty, [skos:closeMatch](https://www.w3.org/2009/08/skos-reference/skos.html#closeMatch), along with the annotation properties [sdo:domainIncludes](https://schema.org/domainIncludes) and [sdo:rangeIncludes](https://schema.org/rangeIncludes). These mappings ensure semantic coherence between DCAT and schema.org, enabling effective integration and interoperability between the two vocabularies.

## Licenses
The licensing of resources in DCAT is categorized into three main situations:
1. Resources explicitly declared as 'licenses' have a statement associated with them. The [dcterms:license](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/#http://purl.org/dc/terms/license) property is used to reference the canonical IRI of well-known licenses, such as [those defined by Creative Commons](https://creativecommons.org/share-your-work/licensing-types-examples/).
2. Resources associated with statements denoting access rights, such as whether data can be accessed by anyone or only authorized parties. The [dcterms:accessRights](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/#http://purl.org/dc/terms/accessRights) property is used, and values from taxonomies like the [Named Authority List: Access rights](https://publications.europa.eu/en/web/eu-vocabularies/at-dataset/-/resource/dataset/access-right) can be used to specify the access permissions.
3. All other cases that do not pertain to licensing conditions or access rights, such as copyright statements. While the simplest approach is to use the [dcterms:rights](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/#http://purl.org/dc/terms/rights) property, a more comprehensive solution for expressing rights is provided by the [Open Data Rights Statement Vocabulary (ODRS)](https://schema.theodi.org/odrs/). ODRS extends [Dublin Core](https://www.dublincore.org/about/) and offers properties to specify various aspects, including copyright statements and notices.

In the case of expressing rights through [ODRL policies](https://www.w3.org/TR/odrl-vocab/), it is recommended to use the [odrl:hasPolicy](https://www.w3.org/TR/odrl-vocab/#term-hasPolicy) property to link the description of the cataloged resource or distribution to the corresponding ODRL policy. The Open Digital Rights Language (ODRL) is a policy expression language that provides a flexible and interoperable [information model](https://www.w3.org/TR/odrl-model/), [vocabulary](https://www.w3.org/TR/odrl-vocab/), and encoding mechanisms for representing statements about usage (i.e., permissions, prohibitions, and obligations) of content and services.

## Versioning
Versions are often used to denote relationships between a resource and its derived forms, such as revisions, editions, adaptations, or translations. DCAT allows describing versions resulting from revisions during the life cycle of a resource. This is achieved by leveraging existing vocabularies, including the versioning component of the [Provenance, Authoring, and Versioning (PAV)](https://pav-ontology.github.io/pav/) ontology, [terms from Dublin Core](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/), [OWL 2](https://www.w3.org/TR/owl2-overview/), and [ADMS](https://www.w3.org/TR/vocab-adms/). The DCAT approach complements existing versioning practices used in specific domains and communities.

DCAT supports the following types of relationships between versions:
1. Version chain and hierarchy: This includes properties such as [dcat:previousVersion](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_previous_version) (equivalent to [pav:previousVersion](https://pav-ontology.github.io/pav/#d4e459)), [dcat:hasVersion](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_has_version) (equivalent to [pav:hasVersion](https://pav-ontology.github.io/pav/#d4e395)), and [dcat:hasCurrentVersion](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_has_current_version) (equivalent to [pav:hasCurrentVersion](https://pav-ontology.github.io/pav/#d4e359) and sub-property of [dcat:hasVersion](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_has_version)). The [dcat:previousVersion](https://pav-ontology.github.io/pav/#d4e459) property constructs a version chain that allows navigation from a given version to its predecessors. The [dcat:hasVersion](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_has_version) property establishes a version hierarchy by linking an abstract resource to its specific versions. The [dcat:hasCurrentVersion](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_has_current_version) property connects an abstract resource to a snapshot representing the current version.
2. Version replacement: This is indicated by reusing the [dcterms:replaces](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_replaces) property. This relationship is necessary because version history-related properties alone do not imply a strict version chain, as a version may not necessarily replace its immediate predecessor.

To provide additional information about versioned resources, the following properties can be used:
* [dcat:version](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_version) (equivalent to pav:version) for specifying the version name or identifier.
* [dcterms:issued](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_release_date) for indicating the release date of the version.
* [adms:versionNotes](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_version_notes) for providing a textual description of the changes, including any backward compatibility issues with the previous version.

For specifying life-cycle statuses, DCAT utilizes the [adms:status](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_status) property along with appropriate time-related properties ([dcterms:created](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/terms/created/), [dcterms:dateSubmitted](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/terms/dateSubmitted/), [dcterms:dateAccepted](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/terms/dateAccepted/), [dcterms:dateCopyrighted](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/terms/dateCopyrighted/), [dcterms:issued](https://www.w3.org/TR/vocab-dcat-3/#Property:resource_release_date), [dcterms:modified](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/terms/modified/), [dcterms:valid](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/terms/valid/)). However, DCAT does not prescribe a specific set of life-cycle statuses but defers to existing standards and community practices that are suitable for the relevant application scenario. The [dataset statuses](https://op.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/dataset/dataset-status) and [concept statuses](https://op.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/dataset/concept-status) vocabularies from the EU Vocabularies registry can also be employed.

## Data citation
To facilitate data citation, a dataset description should include essential elements such as the dataset identifier, dataset creator(s), dataset title, dataset publisher, and dataset publication or release date. These elements align with the requirements of the [DataCite](https://schema.datacite.org/) metadata schema, which is used for associating persistent identifiers, e.g.: DOIs, with research data.

DCAT 2 introduced dereferenceable identifiers and the ability to specify creators of cataloged resources. The remaining essential properties for data citation have been available since DCAT 1.

## Quality information
Since it’s needed to have a basic understanding of relevant quality-related vocabularies and standards, this section just redirects interested readers to the [source document](https://www.w3.org/TR/vocab-dcat-3/#quality-information) for further exploration. It provides detailed information and examples on two aspects: (1) documenting the quality of DCAT first-class entities such as datasets and distributions, and (2) representing the degree of conformance to a stated quality standard along with the associated conformance tests.

## Examples
Examples showing the usage of a DCAT to describe a loosely structured catalog, the provenance of a dataset, how to link datasets and publications, how to describe data services, and how to depict compressed and packaged distributions are available [here](https://www.w3.org/TR/vocab-dcat-3/#collection-of-examples).7

## Resources
* Data Catalog Vocabulary (DCAT) - [Version 3. W3C Working Draft](https://www.w3.org/TR/vocab-dcat-3/).
* Using DCAT (Data Catalogue Vocabulary) to support metadata catalogue integration ([YoutTube talk](https://youtu.be/Pb53L77_nS8))
* [Dublin Core Metadata Initiative (DCMI)](https://www.dublincore.org/about/)
* [DataCite Metadata Schema](https://schema.datacite.org/)
* [Data Documentation Initiative (DDI)  - Cross Domain Integration (DDI-CDI)](https://ddialliance.org/Specification/ddi-cdi)

##### Visit my [LinkedIn Profile](https://www.linkedin.com/in/ereynrs/)

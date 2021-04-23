# A briefing on PIDs

This document presents some notes related to Persistent Identifiers (PIDs) approaches and technologies. 

Part 1 presents the “[Identifiers for the 21st century: How to design, provision, and reuse persistent identifiers to maximize utility and impact of life science data](http://dx.doi.org/10.1101/117812)” article’s highlights.

Part 2 presents the DOI’s approach and technology to resource identification, as depicted in the [DOI Handbook](https://www.doi.org/hb.html) and [DOI Factsheets](https://www.doi.org/factsheets.html).

 
## Part 1 
### Abstract
Drawing on our experience and on work by other groups, we outline ten lessons we have learned about the identifier qualities and best practices that facilitate large-scale data integration. Specifically, we propose actions that identifier practitioners (database providers) should take in the design, provision and reuse of identifiers; we also outline important considerations for those referencing identifiers in various circumstances, including by authors and data generators. While the importance and relevance of each lesson will vary by context, there is a need for increased awareness about how to avoid and manage common identifier problems, especially those related to persistence and web-accessibility/resolvability. We focus strongly on web-based identifiers in the life sciences; however, the principles are broadly relevant to other disciplines.  
 
### Introduction
While the Internet has revolutionized the efficiency of retrieving sources, the same can not be said for reliability: it is well established that a significant percentage of cited web addresses go “dead”. This process is commonly referred to as link rot because availability of cited works decays with time. Although link rot threatens to erode the utility and reproducibility of scholarship, it is not inevitable: link persistence has been the recognized solution since the dawn of the Internet. However, this problem, as we will discuss, is not at all limited to referencing journal articles. The life sciences have changed a lot over the past decade as the data have evolved to be ever larger, more distributed, more interdependent, and more natively web-based. This transformation has fundamentally altered what it even means to “reference” a resource; it has diversified both the actors doing the referencing and the entities being referenced. 

#### Definition 1: Anatomy of a persistent identifier

An identifier is a sequence of characters that identifies an entity. The term “persistent identifier” is usually used in the context of digital objects that are accessible over the Internet. Typically, such an identifier is not only persistent but also actionable: it is a Uniform Resource Identifier (URI), usually of type http/s, that you can paste in a web browser address bar and be taken to the identified source. 

An example of an exemplary URI is below; it is comprised of ASCII characters and follow a pattern that starts with a fixed set of characters (URI pattern). That URI pattern is followed by a Local ID -an identifier which, by itself, is only guaranteed to be locally unique within the database or source. A local ID is sometimes referred to as an “accession”.

![](https://i.imgur.com/dNyUxVa.png)

_Figure. URI structure (by example)_

Formally breaking down a URI into into these two components (URI pattern and local ID) makes it possible for meta resolvers to “resolve” entities to their source. This practice also facilitates representation of a URI as a compact URI (CURIE), an identifier comprised of <Prefix>:<Local ID> wherein prefix is deterministically convertible to a URI pattern and vice-versa. For instance, the above URI could be represented as uniprot:A0A022YWF9. This deterministic conversion makes it easy for meta resolvers as well, e.g., http://identifiers.org/uniprot:A0A022YWF9.

This article seeks to provide pragmatic guidance and examples for how actors in life science research lifecycle should handle identifiers. Optimizing web-based persistent identifiers is harder than it appears; there are a number of approaches that may be used for this purpose, but no single one is perfect: Identifiers are reused in different ways for different reasons, by different consumers. 

The problem of identifier management is hardly unique to the life sciences; it afflicts every discipline from astronomy to law. Towards this end, several groups  have been converging on identifier standards that are broadly applicable. Building on these efforts and drawing on our experience in integrating and accessing data from a large number of sources, we outline the identifier qualities and best practices we consider particularly important in the context of large-scale data integration in the life sciences. In Lessons 1-9 we propose actions for data providers when designing new identifiers, maintaining existing identifiers, as well as when reusing and referencing identifiers from other datasets. In Lesson 10, we conclude with guidance for data integrators and redistributors on how best to reference multiple identifiers from diverse sources. More often than not, life science data providers often invent or organically grow their own identifier systems without a firm grasp of the lasting implications. Data providers are urged to take a long-term view of the scope and lifecycle of data and the identifiers that they issue, and to consider using existing identifier platforms and services where appropriate.

_Table 2. Desirable characteristics for database identifiers in the life sciences._


<table>
  <tr>
   <td><strong>Characteristics</strong>
   </td>
   <td><strong>Definition</strong>
   </td>
   <td><strong>General rationale/impact on data integration</strong>
   </td>
   <td><strong>Specific example of a possible ramification due to no-adherence</strong>
   </td>
  </tr>
  <tr>
   <td>Unambiguous
   </td>
   <td>One Local ID must be associated to no more than one entity locally.
<p>
One URI must be associated to no more than one entity globally
   </td>
   <td>Avoids collisions that result in integrating on the wrong entity.
   </td>
   <td>A physician makes a wrongful diagnosis.
   </td>
  </tr>
  <tr>
   <td>Unique
   </td>
   <td>One entity should ideally be identified by no more than one URI
   </td>
   <td>1) Eliminates the cost of maintaining public mappings between equivalent identifiers
<p>
2) Avoids false negatives if data integrators do not
<p>
leverage or know about a mapping.
   </td>
   <td>A researcher fails to make a pathway discovery because she
<p>
does not realize that
<p>
http://mydb.org/ 234567 and
<p>
http://mydb.org/q?=1234567 are
<p>
in fact the same.
   </td>
  </tr>
  <tr>
   <td>Stable (identifier)
   </td>
   <td>The URI, and by extension the local ID should, wherever possible stay the same over time.
   </td>
   <td>Avoids link rot.
   </td>
   <td>A researcher is unable to reproduce an experiment because the link to a record is dead.
   </td>
  </tr>
  <tr>
   <td>Stable (entity)
   </td>
   <td>Identifier must NOT be reassigned to an altogether different entity, though the original entity may evolve provided a change history is documented.
   </td>
   <td>Avoids integrating on the wrong entity.
   </td>
   <td>A chemist uses the wrong chemical in a reaction.
   </td>
  </tr>
  <tr>
   <td>Version-documented
   </td>
   <td>If the entity’s definition or essential metadata changes substantially,
<p>
(Lesson 7) the identifier should, wherever possible be versioned and/or change history documented
   </td>
   <td>Avoids integrating on the wrong entity state (specified through version)
   </td>
   <td>A given experiment is not reproducible because the specific build version of a gene sequence was not specified.
   </td>
  </tr>
  <tr>
   <td>Persistent
   </td>
   <td>The identifier must NOT be deleted (but may be deprecated)
   </td>
   <td>Avoids link rot
   </td>
   <td>Information about a gene model is completely lost
   </td>
  </tr>
  <tr>
   <td>Web-resolvable
   </td>
   <td>The URI must be resolvable to a web address where the data or information about the entry can be accessed
   </td>
   <td>Avoids the unnecessary
<p>
proliferation of resolvable identifiers issued by third parties (for entities that are not resolvable and/or not identified in their native context). See also surrogate identifier.
   </td>
   <td>A dozen different third party pr oviders mint identifiers for entities that are not actually under their control. Harmonization between these off-brand identifiers is painful.
   </td>
  </tr>
  <tr>
   <td>Convertible
   </td>
   <td>The local ID and its URI counterpart must be inter-convertible by applying the URI pattern to the local ID. Note that in some communities (eg.
<p>
ontologies), the local ID is often a CURIE by default.
   </td>
   <td>Avoids the need for special handling of edge cases when integrating data at scale.
   </td>
   <td>Data integrators spend time cleaning identifiers and handling
<p>
edge-cases instead of doing science.
   </td>
  </tr>
  <tr>
   <td>Defined
   </td>
   <td>The total set of assignable identifiers
<p>
for the database must be describable through a formal pattern (regular expression)
   </td>
   <td>Facilitates validation and extraction from scientific text, thus the pattern should be as tightly specified as possible (see Lesson 3).
   </td>
   <td>Identifiers can not be validated and a provider may find it hard to assess their impact in the literature.
   </td>
  </tr>
  <tr>
   <td>Web-friendly
   </td>
   <td>The local ID should wherever possible be of a format that does not
<p>
need special handling when used in URLs and common exchange formats (e.g. XML).
   </td>
   <td>Avoids potential points of failure due to malformed URL, XML, etc.
   </td>
   <td>Use of the identifier produces malformed XML and / or requires
<p>
special detection and encoding.
   </td>
  </tr>
  <tr>
   <td>Free-to-assign
   </td>
   <td>The identifier should ideally be assigned at no cost to individuals
<p>
depositing data in a repository
   </td>
   <td>Lowers barriers for data generators to deposit data.
   </td>
   <td>Data generators become reluctant to deposit data in order to minimize costs.
   </td>
  </tr>
  <tr>
   <td>Open access and use
   </td>
   <td>The identifier and its label should be able to be transparently referenced and actioned (e.g. in a public index or search) anywhere by
<p>
anyone and for any reason.
<p>
Restrictions on associated data may
<p>
apply but are not recommended.
   </td>
   <td>Enables integration on the basis of scientific merit, rather than on the restrictions of the
<p>
license.
   </td>
   <td>When there are license
<p>
restrictions on the identifier and/or label (not just the content)
<p>
it thwarts meaningful reuse and redistribution of whole datasets.
   </td>
  </tr>
  <tr>
   <td>Documented
   </td>
   <td>The identifier scheme should be documented.
   </td>
   <td>Encourages consistent use of existing identifiers by others and reduces the number of ways identifiers are
<p>
represented.
   </td>
   <td>Inconsistent informal approaches to eferencing are difficult to harmonize post-hoc. By extension, impact is harder to assess.
   </td>
  </tr>
</table>




#### Lesson 1. Credit any derived content using its original identifier.
If you manage an online database (repository, registry, or knowledgebase), consider its role in identifying and referencing the knowledge that it publishes. We advise that you only create your own identifiers for new knowledge. Wherever you are referring to existing knowledge, do so using existing identifiers (Lesson 10): otherwise, wherever the 1:1 relationship of identifier:entity breaks down, costly mapping problems arise. Whether or not you create a new ID, it is vital to credit any derived content using its indigenous identifiers; to facilitate data integration, all such identifiers should be machine processable and transparently mapped.

![](https://i.imgur.com/2CFFU9z.png)

_Figure. Contributions and roles related to content as they correspond to identifier creation vs reuse._

The decision about whether to create a new identifier, or reuse an existing one depends on the role you play in the creation, editing, and republishing of content; for certain roles (and when several roles apply) that decision is a judgement call. Asterisks convey cases in which the best course of action is often to correct/improve the original record in collaboration with the original source; the guidance about ID creation versus reuse is meant to apply only when such collaboration is not practicable (and an alternate record is created). It is common that a given actor may have multiple roles along this spectrum; for instance, a given record in monarchinitiative.org may reflect a combination of a) corrections Monarch staff made in collaboration with the original data source, b) post-ingest curation by Monarch staff, b) expanded content integrated from multiple sources.

#### Lesson 2. Help local identifiers travel well: document prefix and patterns.
If you reference other's data, or anticipate your data being referenced by others, consider how you document your identifiers. Note that you may not know a priori how your data may be used. Data does not thrive in silos: it is most useful when reused, broken into parts and integrated with other data, for instance in database cross references (“db xrefs”). In spite of how important identifiers are to this process, the confusion with identifiers often starts with the basics, including what the “identifier” even is. A local ID is an identifier guaranteed only to be unique in a given local context (eg. a single provider, a single collection, etc.), and sometimes only within a specific version; as such, it is poorly suited to facilitate data integration because it can collide when considered in a more global landscape of many such identifiers. Local IDs therefore need to be contextualized in order to be understood and accessed (resolved) on the web. This is often accomplished through the use of a prefix, which should be documented. 

The http/s URI is the best available identifier form for machine-driven global data integration because a) the http URI is a widely adopted IETF standard and b) the http URI’s uniqueness is ensured by a single well-established name-granting process (DNS). However, the length of URIs can make them unwieldy for tasks involving human readability even within structured machine-parsable documents. Compact URIs (CURIEs) are a mature W3C standard that is well established in some contexts (e.g. JSON-LD and RDFa) as they enable URIs to be understood and conveniently expressed. 

#### Lesson 3. Opt for simple, durable web resolution.
A core component of persistent identification is redirection, the absence of which makes it extremely difficult to provide stable identifiers. When designing (or refining) your http URI strategy:

*   **Consider a resolution provider before doing it yourself.** If you are a database provider, you must implement an http URI pattern (Figure 1 panel B) for local IDs to be resolvable to a web page. If you choose to outsource to a resolver service, use an approach that adheres to best practice and be mindful of your constraints regarding cost, metadata ownership, turnaround time, etc. Some of these resolver services can even provide content negotiation for different encodings of your data and make it easier to provide direct access to data, metadata, and persistence statements.
*   **Avoid inclusion of anything that is likely to change or lapse**, including administrative details (e.g. grant name) or implementation details such as file extensions (“resource.html”), query strings (“param=value”), and technology choices (“.php”). Never embed the local ID in the query part of a URI eg. [http://example.com/explore?record=A123456](http://example.com/explore?record=A123456).
*   **Omit trailing characters after the local ID.** In all cases, the URI pattern must include the protocol (e.g. https://) and, if applicable, trailing slash or other delimiters. Trailing characters after the local ID are discouraged as they unnecessarily increase the variability with which the identifier is represented and also complicate straightforward appending of the local ID.
*   **Avoid unnecessary detail.** Detail in “persistent” identifiers creates complexity that must be managed in perpetuity. Make every attempt to limit the degree of path nestedness.

#### Lesson 4. Avoid embedding meaning, or relying on it for uniqueness.
When designing new local IDs or http URIs, avoid embedding meaning or relying on it for uniqueness. The structure and scope of collections evolve, as does scientific understanding; minimizing the meaning embedded in identifiers makes them less vulnerable to obsoletion.  Meaning should only be embedded if it is indisputable, unchangeable and also useful to the data consumer (e.g. computer-processable). For instance, the type of entity imparts meaning to users and may fulfill these three criteria.  In any case, if you opt to include type in the identifiers you issue, avoid relying on type for uniqueness:  it should never be recycled for another entity, even an entity of a different type.

If you need the ability to convey meaning in a dense character space, you don’t need to do so in the identifier itself; consider instead implementing an entity label.  Labels are for human readability only; even if they are deemed durable, labels should not be treated as identifiers, nor should they appear within http URIs. URI patterns, if type-specific, require a corresponding type-specific prefix.  Dual approaches can be helpful to different kinds of consumers: type-agnostic resolution is useful in cases such as data citation in the literature where a) the type of the identified entity is not of primary importance, or b) the type of the entity is already conveyed contextually, and/or c) where resolution is done systematically at scale and/or involves many and varied or volunteer contributors that may be difficult to coordinate. Type-specific resolution is useful in cases like bioinformatic research pipelines where embedded type may facilitate the human-led debugging process. If you support both kinds of resolution, it is best to document a) whether you intend for both to be treated as persistent b) what mapping support you provide.

Whether or not your URIs or your local IDs include type, you should provide other ways for humans and machines to determine the type of entity that is being identified; this is most often achieved via webservices, but ideally also within metadata landing pages if provided.

#### Lesson 5. Design new identifiers for diverse uses by others.
Pre-existing identifiers should be referenced without modifications. However, if you create new local IDs, there are some design decisions that can facilitate their use in diverse contexts.

*   **Avoid problematic characters.** Local IDs should, wherever possible comprise only letters, numbers and URL-safe delimiters. Omission of other special characters guards against corruption and mistranscription in many contexts; however, it is acceptable that the local ID be in CURIE format since modern browsers resolve colons without having to encode them. For the same reason, local IDs should ideally not contain “.” except to denote version where appropriate.
*   **Define a formal pattern and stick to it.** Local IDs must adhere to a formal pattern (regular expression); this facilitates the validation of URIs and improves the accuracy of mining identifiers from scientific text. Consider a fixed length of 8-16 characters (according to the anticipated number of required local IDs). A pattern may be extended if all available identifiers are issued, but existing identifiers should not be changed. The regular expression should include a fixed, documented case convention. In most cases, it is advised that identifiers not rely on case for their uniqueness.
*   **Avoid problematic patterns.** Consider using both letters and numbers in the local ID. This avoids misinterpretation as numeric data. A historically common, if thorny, identifier pattern is that “_” and “:” are often interconverted and it has come to be understood as compact notation, delimiting the prefix from the rest of the identifier. Therefore “_” or “:” should a) occur no more than once per identifier and b) should only be used if local ID are intended to be deterministically expanded to a resolvable http URI. Whatever pattern you adopt, document which variations you support resolution of, if any.
 
#### Lesson 6. Implement a version-management policy.
Whether you produce original data, or reference others’ data, consider the impact of changes. The nature, extent, and speed of data changes impact how data can be referenced and used. Document your chosen version management practice: If you issue identifiers, the change history for the entity should be either documented or queryable. Alternatively, the identifier itself can be versioned whether or not change history is also supported.

Explicit identifier versioning is recommended if the prevailing use of an unversioned identifier results in “breaking changes”. However, if new information about the entity emerges slowly and the changes are “non-breaking”, it is reasonable to instead maintain a machine-actionable change history wherein the changes are listed, and where they may also be categorized (eg. minor versus major changes). Versioning and change history work well together, especially when multiple types of changes overlap. Even where previous records are entirely removed, the URI should continue to resolve, but to a “tombstone” page (Lesson 7). A resource should communicate clearly what a version change refers to. 

There are two approaches to versioning, record-level and release-level; the latter is more common in the life sciences. Release-level versioning is usually performed for defined data releases. However, use cases vary; some user communities need to resolve individual archived entities via a deterministically-versioned URI pattern.

If you version identifiers at the level of the individual record, you should version in the local ID after the “dot”, this provides continuity in your site and also enables a single prefix to be used with any version: UniProt:P12345.3 → [http://www.uniprot.org/uniprot/P12345](http://www.uniprot.org/uniprot/P12345).3. If you do record-level versioning but dot suffixing is not practicable, we strongly recommend providing a transparent mapping between identifiers together with a mechanism for obtaining the latest version of the record (e.g. by inserting “/latest/” in the URI path). 

_Table 3. Recommendation for versioning._

<table>
  <tr>
   <td>
   </td>
   <td><strong>Recommendation</strong>
   </td>
  </tr>
  <tr>
   <td rowspan="4" >General versioning practices
   </td>
   <td>Primary versioning strategy.
   </td>
  </tr>
  <tr>
   <td>Past versions are accessible.
   </td>
  </tr>
  <tr>
   <td>Release versioning available.
   </td>
  </tr>
  <tr>
   <td>Documentation exists regarding what kinds of record changes prompt a new version to be issued.
   </td>
  </tr>
  <tr>
   <td rowspan="7" >URL versions
   </td>
   <td>The base identifier (the one with no explicit version) should resolve (302 redirect) to most recent version.
   </td>
  </tr>
  <tr>
   <td>Base identifier should be deterministically convertible from any other version.
   </td>
  </tr>
  <tr>
   <td>Older versions must resolve.
   </td>
  </tr>
  <tr>
   <td>Illegal or invalid version should produce an informative http error code and a HTML page explaining the error.
   </td>
  </tr>
  <tr>
   <td>A list of all previous versions should be available.
   </td>
  </tr>
  <tr>
   <td>Link from older vers.ion to current version should ideally be provided.
   </td>
  </tr>
  <tr>
   <td>Two versions (or dates) should ideally be comparable.
   </td>
  </tr>
</table>

 
#### Lesson 7. Do not reassign or delete identifiers.
Identifiers that you have exposed publicly, whether as http URIs or via APIs may be deprecated but must never be deleted or reassigned to another record. If you issue identifiers, consider their full lifecycle: there is a fundamental difference between identifiers which point to experimental datasets  and identifiers which point to a current understanding of a biological concept. While experimental records are less likely to change, concept descriptions may evolve rapidly; even the nature and number of the relevant metadata fields change over time. Moreover, the very notion of identity is often strongly impacted by relationships (e.g., between concepts or processes).

_Table 4. Recommendations for identifier lifecycle management._

<table>
  <tr>
   <td><strong>Recommended handling</strong>
   </td>
   <td><strong>Example</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Obsoletion:</strong> If an entry has been removed or deprecated, the original identifier must still resolve to a “tombstone page”. Reasons for obsolescence should be indicated. If the obsoleted ID is replaced by another ID, the replacement must be present and also described as automatic or suggested, preferably using the ontology properties iao:replaced_by and obo:consider, respectively. 
<p>
The obsoleted ID must never be reassigned to another entity. A list of obsoleted IDs should be maintained.
   </td>
   <td>Single obsoleted identifier:
<p>
<a href="http://www.uniprot.org/uniprot/A0AV18">http://www.uniprot.org/uniprot/A0AV18</a>
<p>
List of obsoleted identifiers:
<p>
<a href="http://uniprot.org/help/deleted_accessions">http://uniprot.org/help/deleted_accessions</a>
   </td>
  </tr>
  <tr>
   <td><strong>Merging</strong>: When two or more identifiers are merged, a new recipient identifier should be designated as the primary (citable) one and should contain information about the legacy identifiers it encompasses. Any legacy identifiers should continue to resolve via redirection to the primary identifier.
   </td>
   <td>UniProt entries Q57339 and O08022 have been merged into Q00626. Q57339 and O08022 are redirected to Q00626.
   </td>
  </tr>
  <tr>
   <td><strong>Splitting</strong>: If an identifier is split (demerged) into two or more new ones, new identifiers should be assigned to all the new entries. The legacy identifier must be marked as obsolete, but must also still resolve, providing a warning and pointers to the new ones as per above.
   </td>
   <td>UniProt entry P29358 has been split into P68250 and P68251. P29358 displays a warning and links to the demerged entries:
<p>
<a href="http://www.uniprot.org/uniprot/P29358">http://www.uniprot.org/uniprot/P29358</a>
   </td>
  </tr>
</table>

#### Lesson 8. Make URIs clear and findable.
Persistent URIs almost always differ from the ephemeral URLs to which users are ultimately directed (Figure 2). Therefore, whether you produce original data, or reference others’ data, make persistent URIs obvious to users.
 
#### Lesson 9. Document the identifiers you issue and use.
The global-scale identification cycle is a shared responsibility and provider/consumer roles often overlap in the context of data integration. Whether you issue your own identifiers or just reference those of others, you should document your identifier policies. Documentation should be published alongside and/or included together in a dataset description, for instance, as outlined in the recommendations for Dataset Descriptions developed by the W3C Semantic Web in the Health Care and Life Sciences Interest Group.

#### Lesson 10. Reference and display responsibly.
When external entities are referenced in narrative online text, they should be hyperlinked to their URIs or to pages/metadata containing their URIs. Access URLs are volatile (see Lesson 4) and must not be used for referencing or linking in any context intended to persist.

## Part 2
 
### About identifiers and registries
An identifier registry is a compilation of unique identifiers, with some information on each item so identified, registered through an organization which maintains it. The registry follows a syntax specification for the identifiers (typically a formal standard), and the agency provides a means of registering identifiers. 

Usually the agency focuses on a particular field of interest and the items registered are of one content type, so the resulting registry achieves scale as the recommended, definitive, or most widely used list of identified entities of that content type. A good example is the ISBN for books (ISO 2108:2005; the ISBN registration authority is [www.isbn-international.org](http://www.isbn-international.org/)). 

Less commonly, sometimes a standard identifier syntax is defined without an implementation to build a registry. Without a single registry these schemes are less likely to be comprehensive and are not as useful for interoperable applications [[DOI System and Standard Identifier Schemes](https://www.doi.org/factsheets/DOIIdentifiers.html)]. 
 
### An introduction to the DOI system
The digital object identifier (DOI) system provides an infrastructure for persistent unique identification of objects of any type [[DOI HandBook. Chap. 1](https://www.doi.org/doi_handbook/1_Introduction.html)]. 

DOI is an acronym for "digital object identifier", meaning a "digital identifier of an object" rather than an "identifier of a digital object". The Digital Object Identifier (DOI) was conceived as a generic framework for managing identification of content over digital networks, recognising the trend towards digital convergence and multimedia availability. Its key features include persistence, network accessibility, interoperability with other identifiers, shared fault-tolerant infrastructure, and the ability to resolve the identifiers in multiple forms. DOI is standardized as ISO 26324.

The DOI system is implemented by Registration Agencies who provide domain-specific identifiers for various applications using the underlying DOI framework. For example, Crossref manages DOIs for the scientific publishing industry, DataCite provides DOIs for referencing and sharing scientific datasets, and the Entertainment ID Registry (EIDR) provides identifiers and associated metadata that are used in the commercial film and video industry, from post-production through broadcast, digital distribution, and reporting.

Because the DOI system is designed for network awareness and interoperability, it is easy to build a wide variety of modern applications using DOIs. For example, the DOI system is used in internal processes in multiple industries, for publishing and reporting across corporate and national boundaries, and in the emerging field of semantic web applications. 

The DOI system is designed to work over the Internet. A DOI name is permanently assigned to an object to provide a resolvable persistent network link to current information about that object, including where the object, or information about it, can be found on the Internet. While information about an object can change over time, its DOI name will not change. A DOI name can be resolved within the DOI system to values of one or more types of data relating to the object identified by that DOI name, such as a URL, an e-mail address, other identifiers and descriptive metadata.

The DOI system enables the construction of automated services and transactions. Applications of the DOI system include but are not limited to managing information and documentation location and access; managing metadata; facilitating electronic transactions; persistent unique identification of any form of any data; and commercial and non-commercial transactions.

The content of an object associated with a DOI name is described unambiguously by DOI metadata, based on a structured extensible data model that enables the object to be associated with metadata of any desired degree of precision and granularity to support description and services. The data model supports interoperability between DOI applications.

The scope of the DOI system is not defined by reference to the type of content (format, etc.) of the referent, but by reference to the functionalities it provides and the context of use. The DOI system provides, within networks of DOI applications, for unique identification, persistence, resolution, metadata and semantic interoperability.


### The DOI functionality 
[[DOI System and Internet Identifiers Factsheet](https://www.doi.org/factsheets/DOIIdentifierSpecs.html)]

*   _Neutral as to implementation._ DOI allows but does not require http or other protocols. The design principle is that DOIs are not specific to the web or any other implementation. DOI is designed to be applicable in any environment on the Internet. 
*   _Flexible as to implementation._ The DOI system has been designed around a data and transaction model that can work in a wide variety of environments. The current implementation works well with, but does not require, http or other web protocols, and can be used in any environment on the Internet.
*   _Granularity of naming and administration at the object level._ Allows but does not mandate coarser level granularity tools such as domain names. Specifically, DOI resolution in native resolver form does not require the use of the DNS (Domain Name System). 
*   _Neutral as to language/character set._ Compatible with, but not restricted to, the ascii character set. DOI names can use the Unicode capability of the Handle System to develop DOI names in Japanese, Chinese, etc., characters. The current DOI syntax restricts initial implementations to ascii simply for ease of adoption, but is intended to be widened (backward compatibility) to Unicode in a future revision. 
*   _Multiple resolution to typed data offers the possibility of expressing semantic relationships. _
*   _Social infrastructure_ providing persistence through organizational backup, data integrity measures, etc. 

### The DOI system components
The DOI system provides a ready-to-use packaged system of several components: 

*   _DOI name syntax:_ a specified standard numbering syntax. A DOI<sup> </sup>name differs from commonly used Internet pointers to material such as the URL, because it identifies an object as a first-class entity, not simply the place where the object is located. 
*   _DOI name resolution:_ a resolution service. The Handle System is a technology specification for assigning, managing, and resolving persistent identifiers for digital objects and other resources on the Internet. It is the underlying resolution component for the DOI System. The Handle System is the most appropriate persistent identifier management system for the DOI System [[DOI System and Internet Identifiers Factsheet](https://www.doi.org/factsheets/DOIIdentifierSpecs.html)]. Specifications and the source code for reference implementations for the servers and protocols can be found [here](http://www.handle.net/prefix.html) [[Handle System Wikipedia entry](https://en.wikipedia.org/wiki/Handle_System)].
*   _DOI data model:_ a data model incorporating a data dictionary. The DOI system uses an interoperable data dictionary built from an underlying ontology. The data dictionary component is designed to ensure maximum interoperability with existing metadata element sets; the framework allows the terms to be grouped in meaningful ways so that certain types of DOI names all behave predictably in an application through association with specified services. This provides a means of integrating the features of handle resolution with a structured data approach. DOI names need not make extensive use of this data model, but it is envisaged that many will.  
*   _DOI system implementation:_ An implementation mechanism through a social infrastructure of organizations, policies and procedures for the governance and registration of DOI names. The DOI system is implemented through a federation of Registration Agencies which use policies and tools developed through a parent body, the International DOI Foundation (IDF). Through use of the DOI system in commercial systems, individual Registration Agencies have an incentive to allocate their own resources to the development of new features, or to collaborate with other RAs to develop features that may be shared with the wider DOI name user community [[DOI Handle System Factsheet](https://www.doi.org/factsheets/DOIHandle.html)]. 

### Differences between DOI System and most standard identifier registries
While the DOI System is a registry, it has significant differences compared to traditional single content-type registries [[DOI Identifiers Factsheet](https://www.doi.org/factsheets/DOIIdentifiers.html)]:



*   _Purpose_: The purpose of an identifier registry is to manage a given collection of identifiers; the primary purpose of the DOI System, on the other hand, is to make a collection of identifiers actionable and interoperable, where that collection can include identifiers from many other controlled collections. 
*   _Coverage_: A registry aims to be definitive and comprehensive across its content type; the DOI System is not intended to be a comprehensive identifier registry of all items falling potentially within its scope, but any such item may be registered as a DOI name. 
*   _Scope_: The scope of a standard registry is defined and fixed by content type (e.g. books and ISBN). The scope of the DOI System is potentially any resource involved in an intellectual property transaction; hence the coverage of DOI names is extensible (actual use expands continually as new areas of application are created): _"The scope of the DOI system is not defined by reference to the type of content (format, etc) of the referent, but by reference to the functionalities it provides and the context of use"._ (ISO 26324 FDIS, Introduction) 
*   _Granularity_: The granularity of a content registry is typically defined and fixed by content type. The DOI System may be applied at any desired level of functional granularity, which may be modified by creating supersets or sub sets (including related types). 

    Registries may be of many different types, and are used in many applications, so it is difficult to generalize across all cases.

### DOI System Data Model
[[DOI Handbook. Chap. 4: Data Model.](https://www.doi.org/doi_handbook/4_Data_Model.html)]

Without metadata, an identifier is of very little value. Metadata, which may be defined in this context as information about an identified Referent, provides human beings or machines with the data they need to enable them to make use of that identified Referent. Metadata may include names, identifiers, descriptions, types, classifications, locations, times, measurements, relationships and any other kind of information related to a Referent.

DOI system policy places no restrictions on the form and content of an RA's input and service metadata declarations, except insofar as input metadata must support the minimum requirements implicit in the _DOI Kernel_. RAs may specify their own metadata schemes and messages, or use any existing schemes in whole or part for their input and service metadata declarations. 

DOI data model policy is concerned with the internal management and exchange of metadata between RAs within the "RA network", and is designed to achieve two aims:

1. To promote _interoperability_ within the network of DOI system users, 
2. To ensure minimum standards of quality of _administration_ of DOI names by Registration Agencies, and facilitate the administration of the DOI system as a whole. 

The DOI data model has **_three tools_** to support its metadata policy:

1. The DOI Kernel Metadata Declaration 
2. DOI Referent Metadata Declaration schemas for data interchange between RAs 
3. The Data Dictionary ("DD") 

The responsibilities of RAs can be summarized in these three statements:

1. An RA must be capable of producing a Kernel Metadata Declaration for each DOI name issued. 
2. Metadata exchanged between RAs supporting DOI system services should be exchanged using an agreed DOI Referent Metadata Declaration ("RMD") for the Referent or Service type. 
3. Proprietary terms (data elements and values) used by RAs in Kernel and Referent Metadata Declarations should be registered in the IDF's data dictionary ("DD"). 

The first aim of DOI data model policy is to promote interoperability within the network of DOI name users. Standardization of any kind is driven by a need for interoperability. If an RA is issuing DOI names for Referents for use within a private domain where that RA is able to command all aspects of metadata gathering and output, then it has no need for standardization or conformance with DOI data model obligations. The RA will lay out its schemas and declarations, and its providers and users will, hopefully, conform to them. Such a situation is described as _restricted use of the DOI system_, and applies typically where an organization becomes an RA for the specific purpose of issuing DOI names for use only within its own private organization. However, such isolation is unusual. Normally, when a DOI name is issued to a Referent, one fundamental assumption may be made about interoperability: _the RA or the Referent provider may wish (now or in the future) that the DOI name should be available for use in services provided by other Ras_.

Any DOI name which is _intended for interoperability_ — that is, which has the possibility of use in services outside of the direct control of the issuing RA — is subject to DOI data model policy. The aim of metadata interoperability can therefore be expressed in these two objectives: 

1. To ensure that metadata held by different RAs is _not fundamentally inconsistent_, and 
2. To ensure that an _efficient and extensible means of interchange_ exists for transporting metadata between RAs (and in future other service providers). 

An identifier such as a DOI is of no value without some related metadata describing what it is that is being identified. The DOI's approach to metadata has two aspects: first, the DOI standard mandates a particular minimum set of metadata (the "Kernel" metadata) to describe the referent of a DOI name, supported by an XML Schema; secondly, to promote interoperability and assist RA's in the creation of their own schemas the IDF provides a Data Dictionary or ontology of all terms used in the Kernel, and other terms registered by Registration Agencies, and supports a mapping tool called the Vocabulary Mapping Framework.

The "DOI Kernel" is a minimum metadata set with two aims: **recognition** and **interoperability**. 

"Recognition" in this context means that the Kernel metadata should be sufficient to show clearly what _kind_ of thing which is the DOI referent (by various classifications), and allow a user to identify with reasonable accuracy the _particular_ thing (by various names, identifiers and relationships). These two are complementary, for it is possible to know that something is (for example) a movie or a DVD without knowing that it is "Casablanca", and vice versa. Recognition is required for the discovery of referents, and also to provide information to a user when a referent is discovered, whether by intent or accident. The user of metadata may be a person or a machine. The structure of the Kernel is often but not always sufficient to provide a unique description of a referent ("disambiguation"), and further specialized metadata elements may be required in some cases. A unique description can in fact always be achieved by adding additional descriptive text to a referentName, but this is not a satisfactory way if the additional text is being used in place of a formal classification, measurement, identifier, time or other structured contextual metadata, as it undermines the second goal of interoperability. 

"Interoperability" in this context means that Kernel metadata from different DOI Registration Agencies may be combined or queried by the same software without requiring semantic mapping or transformation. Interoperability is achieved when data elements or their values are common to diverse metadata schemas. The Kernel provides this directly by mandating a common set of core elements and classifications, but this of course supports only limited interoperability.

The assignment of a DOI name requires that the registrant provide metadata describing the object to which the DOI name is being assigned. At minimum, this metadata shall consist of a DOI Kernel Metadata Declaration (also known as the DOI Kernel).

For other elements and sub-elements beyond the DOI Kernel, values as needed may be developed. Such value sets shall be registered in the data dictionary under the responsibility of the IDF (the ISO 26324 Registration Authority) in order to facilitate the integration of DOI data from different sources by a common application. Tables depicting the descriptive and administrative elements of the DOI Kernel can be found in [[DOI Handbook. Chap. 4: Data Model.](https://www.doi.org/doi_handbook/4_Data_Model.html)].

A "schema" includes any item of software or documented set of metadata elements designed for a specific purpose to support the use of DOIs. Typically this may be an XML schema, and RDF schema or a set of defined vocabulary terms for use in some process. There is increasing consideration among RAs of the benefits of implementing some common machine-readable DOI metadata schemas, especially as "Linked Data". There are currently two DOI metadata schemas: The DOI Data Dictionary, including Allowed Value Sets for use in messages; and The Metadata Kernel. 

All elements and allowed values used in the Kernel are included in the DOI Data Dictionary, a hierarchical ontology created to support the orderly development of DOI metadata. The introduction to the Dictionary contains further information on its scope, structure and maintenance.

The DOI Data Dictionary is implemented and maintained as a managed namespace within the Vocabulary Mapping Framework (VMF).  The IDF supports and recommends the Vocabulary Mapping Framework (VMF) to promote interoperability between Registration Agency schemas, and other schemas and ontologies from outside the DOI domain such as ONIX or DDEX message standards. The IDF hosts the VMF web site and is also part of the governance structure of VMF. VMF is a downloadable tool that provides support for semantic interoperability across communities by providing extensive and authoritative mapping of vocabularies from major content metadata standards, to support interoperability across communities.

Users need not understand the underlying concepts and construction of the Data Dictionary in order to make use of it. Key features of the dictionary are: 

*   extensible to whatever level of detail and granularity is required; 
*   neutral as to business model; 
*   independent of any implementation technology; 
*   allows use of any existing metadata scheme; 
*   multiple, different, specialized views can be made available; 
*   local terms can be included: an RA can add all its own local data elements and names into the ontology, and use only those terms it needs; 
*   can include different terms from different internal systems and map them together; 
*   incorporates external and standard schemes such ISO territory, currency and language codes, and sector specific external schemes, allowing them to be treated seamlessly alongside local terms; 
*   any public terms are accessible to all IDF Registration Agencies. 

A fundamental role of the IDF is to provide assurance to users that the work has been peer-reviewed, tested in practical implementations, and is based on sound principles. The methodology of the data dictionary has been validated against the W3C ontology language OWL-DL. The data dictionary uses an underlying ontology widely accepted and used in a variety of major metadata schemes, having its origins in the indecs (interoperability of data in e-commerce) framework.

The IDF recognises that the **automated integration of metadata** is the key to realising the full potential of the DOI as tool for digital commerce and culture. 

Kernel declaration, DOI Data Dictionary and VMF exist to provide a basis of good practice and a starting point for the integration of metadata for different DOI referents. Initiatives such as Linked Open Data provide further essential infrastructure, but only in technology and syntax: they do not provide solutions at the level of **shared meaning** ("semantic alignment") for the automated integration of different datasets which can allow services from different RAs and other parties to interact fully without human intervention or a plethora of one-to-one "silo" solutions.  The key to this is the development of well-structured metadata schemas and of services which make use of the semantic mapping capabilities of tools such as the DOI Data Dictionary and the VMF. IDF will provide support to its RAs where they choose to co-operate in the development of such services.

The DOI data model is built on a contextual ontology approach shared with many other applications. The IDF ensures that the data model is maintained and made available for further extension and application; RAs and application developers do not need to access the contextual ontology in order to use the DOI system, though they may do so if they wish; an illustrative graphic of the high level concept model is provided following, and further documentation is available as part of the Vocabulary Mapping Framework and related materials.

![](https://i.imgur.com/jQFh4l5.png)

_Figure. [Contextual ontology overview](https://www.doi.org/doi_handbook/DataModelUnderlyingOntology.pdf)_

### DOI applications
[[DOI Handbook. Chap. 5: Applications.](https://www.doi.org/doi_handbook/5_Applications.html)]

All of the data that is associated with a DOI name is stored in the DOI system in type/value pairs, each with its own unique ID. This is extremely flexible and open: new types can be added at any time, and the values can be of arbitrary complexity. Type/value pairs can be administrative (e.g., creation date, permissions, etc.), well known and standardized (e.g., URL, email, etc.), or created by an RA for a specific application purpose (e.g., a custom type with a value that is binary data or a specially formatted string). There can be many type/value pairs associated with a DOI name, and multiples of the same type, but every DOI name has at least one "URL" type, and its value is a URL related to the entity.

At the most basic level, all DOI applications query the DOI system by resolving a DOI name. The request can be structured to ask for all of the data associated with that DOI name; all of the data of a specific type; or data with a specific identifier.  Applications are built to understand one or more of the types, and parse, evaluate, and take action based on the corresponding values.

Developers may choose to put all or most of the information provided by the service in the DOI system itself, such that the DOI system is the primary service provider, or alternatively, use the DOI system to point to one or more external services to provide the desired information and functions. For example, the DOI system may be used to store all of the data required to display a simple menu of labeled options for a user to pick from, but redirecting to an external service that stores large quantities of scientific data for visualization tools, or multiple image files, might be preferable to storing that data in the DOI system.

Developing applications that store, use, and share high-quality machine-readable data in a standardized format is a further design consideration. DOI applications can be designed to conform to the best practices of Linked Data, a well-known concept of exposing data in machine-readable form, using the content negotiation feature of the standard HTTP web protocol.

An example of a simple service that has been built to examine the type/value pairs returned from handle resolution and provide specific information is available from the doi.org Proxy System. This service returns the name of the DOI Registration Agency (RA) responsible for a specific DOI, or group of DOIs. When a DOI name is appended to the string "https://doi.org/doiRA/", a resolution (HTTP GET) of that URL will return a bit of JSON specifying the name of the RA. Resolving [https://doi.org/doiRA/10.5240/B1FA-0EEC-C316-3316-3A73-L](https://doi.org/doiRA/10.5240/B1FA-0EEC-C316-3316-3A73-L) will return:

```
[  \
	{  \
	"DOI": "10.5240/B1FA-0EEC-C316-3316-3A73-L", \
	"RA": "EIDR" 

	}  \
] 
```

Using commas in the URL string to delineate multiple DOIs will return multiple results in one request.

### DOI multiple resolution applications
[[DOI Handbook. Chap. 5: Applications.](https://www.doi.org/doi_handbook/5_Applications.html)]

The most basic DOI application is resolution of a DOI to a "URL" type, a simple, single point resolution. The proxy resolves the DOI name, sees the URL type and knows that the associated value can be returned to the end user client, likely a web browser, as an HTTP redirect. 

Multiple resolution allows an entity to be resolved to multiple pieces of data or entities. Multiple type/value pairs, not limited to URL types, can be returned to clients which can then evaluate the data to determine the next action.  Associating XML, or other machine readable data with a DOI name, expands the utility of multiple resolution even further, adding features, and offering more options for negotiating content, and facilitating the creation of Linked Data applications.

Multiple resolution is therefore useful for building applications where the required evaluation is to choose from a set of possible outcomes based on some criteria. An application developer could create a type in which to store values used by a client to generate a menu of options for a user when the user resolves a DOI name. In the case of a document, the user might be given the option to view the document, view document metadata, share the document by emailing the URL to an associate, visit the author's blog, etc. For a dataset, the choices offered to the user on the landing page may include viewing the complete dataset, viewing only selected data, or some other choice of interaction with the dataset based on the information made available to a client via resolution of the DOI name.

Clients that do not implement the multiple resolution approach can simply use the single level of redirection using the URL type. All older DOI clients of which we are aware behave in this way. This allows adding new features to DOI resolution without breaking the millions of existing DOI names.

Note that even if this approach were to be applied to millions of an RA's existing DOI names, it is not necessary to alter all the DOI records. The DOI system's infrastructure is designed such that any information that applies to all DOI names under a given DOI prefix can be recorded at the DOI prefix record that identifies the service responsible for that prefix, and clients will be able to carry that information all the way through the resolution process. Thus, should an RA at some point need to change their approach to linked data and point to a different service or use different parameters, the change could be made to a single DOI prefix and it would apply to all of the millions of DOI names automatically.

For example, scholarly journal articles are assigned one DOI name, but they can be available from multiple web sites, and readers may wish to download an article from a service to which they are subscribed. The service is enabled by using type "10320/loc", an established type that specifies a machine-readable XML-formatted value that contains a list of "locations" which provide a client with a choice of actions, or 'choose-by' options, for selecting the action to take when resolving the DOI name. It provides a standardized mechanism to select a specific URL or other information from a number of available choices. Resolvers can apply known selection methods, in order, to choose a location based on the resolver's context (the HTTP request in the case of the proxy server) and the attributes of each location. 

The figure below shows values associated with DOI name 10.1525/bio.2009.59.5.9. When this DOI name was registered it had a single URL value. A 10320/loc type was added to provide instructions for offering other resolution options. When this record is returned to the proxy, it recognizes the 10320/loc type and, if asked to do so, performs an evaluation of the location values based on a given criteria.

![](https://i.imgur.com/Zl7mEp1.png)
_Figure. XML Data for Type 10320/loc_

A client may be configured to:

*   Ignore type 10320/loc and resolve 10.1525/bio.2009.59.5.9 and select the URL value, http://www.jstor.org/stable/25502450; 
*   Resolve 10.1525/bio.2009.59.5.9, and construct a page that shows the user both of the URLs in the cr_type attributes in the 10320/loc value, letting the user choose the next action; 
*   Add a paramater and resolve the string 10.1525/bio.2009.59.5.9?locatt="country:uk", in which case the proxy would determine the user's geographic location from their IP address, and based on the value in the "country" element in type 10320/loc, select the BioOne URL, http://www.bioone.org/doi/full/10.1525/bio.2009.59.5.9, for users in the UK. A random selection would be made for all others; 
*   Resolve the string 10.1525/bio.2009.59.5.9?locatt=id:1, in which case the Crossref metadata service at http://mr.crossref.org/iPage?doi=10.1525%2Fbio.2009.59.5.9 would be selected. 

DOI names can be grouped into _Application Profiles (APs)_, which define the services, including metadata services, which are available for that set of DOI names. Each DOI name can be a member of one or more APs. By default, for example, every DOI name is a member of an Application Profile which resolves the DOI name to some network location that contains at least the minimal kernel metadata for that DOI name. Application Profiles can be useful as a conceptual grouping, but may also be technically expressed in the DOI data model as part of the resolution mechanism. Mechanisms for formally registering APs and including them as part of the structured data returned with each DOI resolution, e.g., as a label, will depend on application requirements; potential users should consult with IDF.

### DOI and Linked Data
[[DOI Handbook. Chap. 5: Applications.](https://www.doi.org/doi_handbook/5_Applications.html)]

A prominent use of the 'choose-by' mechanism described above is redirection to Linked Data services. Linked data is the general term for a set of best practices for exposing data in machine-readable form using the content-negotiation feature of the standard HTTP web protocol. These best practices support the development of tools to link and make use of data from multiple web sources without the need to deal with many different proprietary and incompatible application programming interfaces (APIs), and use of HTTP to request data in structured form meant for machines instead of human-readable displays.

The DOI proxy at doi.org is enabled to support content negotiation for DOI names. In Linked Data applications, evaluation of the HTTP request that comes in to the proxy service determines if it is a request for content of form application/rdf+xml, or one of a few similar types that are commonly understood to be a request for Linked Data. These requests for special content types would come from automated processes or special 'linked data' browsers and would not normally come from end users. The utility of this, of course, is that it allows outside developers to query the extensive and reliable set of metadata records held by IDF RAs to build value-added services.

Content negotiation is an accepted method of reaching the goal of linking data. To a web server, and to the DOI proxy servers, content negotiation means 'what should I send back'. It allows a user to request a particular representation of a web resource. For example, DOI resolvers can use content negotiation to provide different representations of metadata associated with DOI names. A content negotiated request to a DOI resolver is much like a standard HTTP request, except server-driven negotiation will take place based on the list of acceptable content types a client provides.

Some DOI RAs are using this approach for all of their DOI names, offering services that return metadata in a common machine-readable format. A significant advantage of applying Linked Data principles and technologies to DOI-registered material is that it is 'data worth linking to': it is curated, value-added, data, which is managed, corrected, updated and consistently maintained by Registration Agencies. It is also persistent, so avoiding 'bit-rot'. In practice, the quality of Linked data implementations is only as good as the data you are linking to, and the meaning and contextualisation of the link you use. The DOI system can offer "curated data", i.e. consistent, managed, linking so you can link to other "quality data" with confidence, while still using the standard Linked Data technologies.

### Expressing relationships between DOI names
[[DOI Handbook. Chap. 5: Applications.](https://www.doi.org/doi_handbook/5_Applications.html)]

The DOI system can provide further support in the development of Linked Data or other mechanisms for relating entities identified with DOI names, by offering the resolution capability combined with simple, useful and interoperable semantics using the [Vocabulary Mapping Framework](https://www.doi.org/VMF/index.html) (VMF) to define (or map from existing schemes) specific relationships. Potential users of this facility are strongly encouraged to discuss this with the IDF community. 

The following set of Relators is recommended for use by Registration Agencies in devising typed relationships ("this DOI name is related to this other DOI name by a relationship of the defined type…").  The relevant semantics here are the "relators" or (in OWL/RDF terms) "properties" which join two DOI names representing resources or parties. IDF has added a small number of "key" relators to its data dictionary, in its own namespace, representing the most common and important relationships between Resources and Parties in existing content standards and databases:

*   _doi:IsDerivedFrom _
*   _doi:IsManifestationOf _
*   _doi:HasSubject _
*   _doi:IsReplacementFor _
*   _doi:IsPartOf _
*   _doi:HasContributor _

These are then recommended and available to RA's to use within their schemas. This list is a starting point, based on content standards and databases: each represents a key concept in VMF. However, RAs will often have some of their own existing relators, or will want to use more specialized Relators. They can be added, as needed by RAs, to the DOI Data Dictionary. For obvious interoperability reasons, the structural relators make use of the RDF/OWL standards (eg owl:equivalentProperty, rdfs:subPropertyOf). This structure provides a small but powerful hierarchical and equivalence Relator Ontology to allow RAs to create and harvest linked data involving DOI names, supported by basic inference to allow for translation into their preferred or required terms. 

In some cases, applications may require the identification of fragments of an entity, rather than the full entity. Each such fragment may be assigned a separate DOI name if it is practical and useful to do so (for example, if a specific table within a book is likely to be re-used many times). However, this may not always be possible: there are also cases where one wishes to identify in principle "any fragment of this entity" as it becomes needed "on the fly". For such cases, use may be made of [Template handles](http://www.handle.net/tech_manual/HN_Tech_Manual_8.pdf): a single template DOI handle can be included in the form of a base formula that allows any number of extensions to that base to be resolved as full DOI handles, according to a pattern, without each such handle being individually registered. This would allow, for example, the use of DOI names to reference an unlimited number of ranges within a video without each potential range having to be registered with a separate handle. If the pattern needs to be changed, e.g., the video moves or a different kind of server is used to deliver the video clips, only the single base DOI name (handle) needs to be changed to allow an unlimited number of previously constructed extensions to continue to resolve properly. 

### DOI and Content Negotiation
[[DOI Handbook. Chap. 5: Applications.](https://www.doi.org/doi_handbook/5_Applications.html)]

Content negotiation is being implemented by DOI Registration Agencies for their DOI names, specifically to offer value-added metadata representations for users. A number of citation styles, some of which are common across RAs, are being supported. Using content negotiation, it is possible to make a request that favours content types specific to a particular RA but which will also degrade to respond with a more standard content type for other RAs. Requests to these services would come from automated processes or special 'linked data' browsers and would not normally come from end users. The utility of this is that it allows outside developers to query the extensive and reliable set of metadata records held by IDF RAs to build value-added services. As examples, content negotiated DOI names can be accessed in [http://data.crossref.org](http://data.crossref.org/), [http://data.datacite.org](http://data.datacite.org/), and [http://data.medra.org](http://data.medra.org/).

For normal browser requests in which the HTML page header includes GET "Accept: text/html", the DOI name is resolved and the user is redirected to the publisher's landing page URL. For example, the DOI "10.1126/science.169.3946.635" redirects to a landing page describing the article, "The Structure of Ordinary Water". Normal browser requests or explicit requests for text/html redirect to the content's landing page is shown following [[Crosscite DOI Content Negotiation](https://citation.crosscite.org/docs.html)].

![](https://i.imgur.com/Q3dqNF2.png)
_Figure. Normal browser requests or explicit requests for text/html redirect to the content's landing page._

Making a content negotiated request instead requires the use of an HTTP header, "Accept", where the GET includes other known content types that are acceptable to the client (those that it knows how to parse) and that are commonly understood to be a request for linked data. For browser requests for which the header says GET "Accept: application/ref+sml, resolving the same DOI name referenced above will redirect the user to the RA's metadata service. Requests for a data type redirect to a registration agency's metadata service is shown following [[Crosscite DOI Content Negotiation](https://citation.crosscite.org/docs.html)].

![](https://i.imgur.com/Y66CpcV.png)
_Figure. Requests for a data type redirect to a registration agency's metadata service._

Further, a client that wishes to receive citeproc JSON if it is available, but which can also handle RDF XML if citeproc JSON is unavailable, would make a request with an Accept header listing both "application/citeproc+json" and "application/rdf+xml". The proxy will return the "Vary: Accept" HTTP header whenever a DOI is resolved which has separate resolutions for content negotiation requests. Developers can use this to determine which DOIs offer content negotiation support. Shown below is an Accept header for the content negotiated request, listing both "application/citeproc+json" and "application/rdf+xml", and the metadata returned by the service.

![](https://i.imgur.com/ZpT4UJH.png)
_Figure. HTTP “Accept” Header_

### The Handle System
It provides a general-purpose global name service enabling secure name resolution over the Internet, designed to enable a broad set of communities to use the technology to identify digital content independent of location. The DOI system utilizes the Handle System as one component in building an added value application, for the persistent, semantically interoperable, identification of intellectual property entities. The Handle System license provides the reference implementation of Handle System, the database component of which was not designed to scale above a few million handles. The DOI system employs a much more robust database implementation capable of scaling to any number of handles [[DOI Handle System Factsheet](https://www.doi.org/factsheets/DOIHandle.html)].

*   www.acme.com is a domain name, which DNS resolves to an IP address, while this: http://www.acme.com/BigChart is not a domain name - it is a URL invented for hyperlinking. It relies on DNS resolution as the first step to find the IP address for an http server. DNS is an excellent resolution mechanism for domain names. This does not make it a resolution mechanism of any kind for other names or identifiers until you add something else. So using DNS and URLs for identifiers requires that you design some approach to using them consistently and coherently. In the same way that DNS and http URLs have not replaced databases but give you an easy way to reference databases, they will not replace well-structured identifier systems but can give you an easy way to reference those identifier systems. 
*   The Handle System was designed as a resolution system for digital objects and it serves as a level of indirection to any sort of current state data that you care to associate with the object through the identifier resolution mechanism. The Handle System provides a way to use DNS and URLs for identifiers, which simultaneously provides an identifier that can be resolved without using DNS and URLs, if you choose to use it like that. 
*   Most uses of the Handle System involve DNS, either as a way to get common web browser clients to communicate with handle servers, e.g. https://doi.org/10.1037/0003-066X.59.1.29 or as the current state data returned from that resolution (e.g. http://psycnet.apa.org/?&fa=main.doiLanding&doi=10.1037/0003-066X.59.1.29). Although the scope of DOI is any object (physical, digital, or abstract), this is consistent with the Handle System's stated scope of digital objects, since any non-digital object can be represented by a digital object "avatar." 

### DOI names persistance

Handle System software may be implemented by anyone who agrees to basic licensing terms; there is no requirement that a user's implementation be persistent. The Handle System technology provides persistence if used with appropriate social infrastructure. The International DOI Foundation (IDF) builds on the technical infrastructure of the Handle System a social structure guaranteeing persistence. Persistence is a function of organizations, not of technology; a persistent identifier system requires a persistent organization and defined processes [[DOI Handle System Factsheet](https://www.doi.org/factsheets/DOIHandle.html)].

### DOI relationships expressions

Multiple resolution allows one entity to be resolved to multiple other entities; this can be used to embody relationships, e.g., a parent-child relationship, or any other relationship. Handle System technology allows this; the DOI system provides a framework to achieve it: 

*   Multiple resolution is a feature of the Handle System technology, but the Handle System per se (deliberately) has no pre-existing constraints to make a useful framework (think of it as like spreadsheet software): the DOI system is an application of the Handle System which adds this constraint (think of it as like a spreadsheet application already written for you to add data to). 
*   In the DOI system the constraints come from the metadata which defines the entities, which is the data dictionary approach: hence IDF's role in MPEG-21 RDD and the indecs Data Dictionary. That enables one to express relationships.

### Semantic interoperability

Handles (including DOI names) will be resolved by the Handle System, but there is no requirement in the Handle System for declaring what is being identified, or for ensuring semantic interoperability across several identified resources. The DOI system adds this facility, specifically designed for its area of applications, which is now being implemented and will be a feature of advanced applications of the DOI system.



*   The DOI system provides a kernel of structured data upon which extended metadata schemes can be built if required, and a means of precisely specifying the entity identified. 
*   The DOI system provides an optional tool to map an existing metadata scheme through a structured standard ontology, thereby ensuring semantic interoperability so that DOI names from different sources may be used as the key in building multi-component media objects or managing multiple assets. 
*   The DOI Data Model embraces both a data dictionary and a framework for applying it. The data dictionary component is designed to ensure maximum interoperability with existing metadata element sets; the framework allows the terms to be grouped in meaningful ways (through Application Profiles) so that certain types of DOI names all behave predictably in an application through association with specified Services. This provide a means of integrating the features of handle resolution with the structured data approach. 
*   IDF maintains the indecs data dictionary, the underlying tool for semantic interoperability, which integrates with the standard ISO MPEG21 Rights Data Dictionary (RDD). The IDF is also the chosen candidate to become the MPEG Registration Authority to manage the RDD. [[DOI Handle System Factsheet](https://www.doi.org/factsheets/DOIHandle.html)] 
 
### Additional comments

#### DOI on Digital Object Architecture

An optional path which has been followed by some DOI implementations is to use the DOI System with the CNRI [Cordra](http://cordra.org/) software which provides complementary functionality. Both the Handle System and Cordra are part of the wider [Digital Object Architecture](http://hdl.handle.net/4263537/5041). Some customization of the basic technologies may be required: this could be done by CNRI or others. This technology is not part of the DOI System and not managed by the IDF, but is endorsed for use with DOI.

The Digital Object (DO) Registry software was developed by [CNRI](http://www.cnri.reston.va.us/) to complement the Handle System (used in the DOI System). The current version, [Cordra](http://www.cordra.org/), integrates the DO Registry with CNRI's DO Repository software. It provides the reverse-lookup function, analogous to a card catalog or telephone directory, such that an identifier can be located by searching the attributes that have been registered. Cordra is open and freely available and has been used in a number of different areas, including in DOI application (e.g. by the movie and television industries for the Entertainment ID Registry, [EIDR](http://www.eidr.org/)). Registries can be built relatively cheaply and relatively quickly using existing systems in place and in daily use today. The DO technologies were built by CNRI, largely supported by government funding, and are freely available at little or no cost. 


:bulb: **What about Linked Data and DOI identifiers?** It seems a **_bad idea_**, see [[CrossRef Blog. Linked Data](https://www.crossref.org/categories/linked-data/)].

### Other IDs proposals

#### IRI
The Internationalized Resource Identifier (IRI, RFC 3987) - is an internet protocol standard which extends the ASCII characters subset of the URI protocol. It was defined by the Internet Engineering Task Force (IETF) in 2005 as a new internet standard to extend the existing URI scheme.

While URIs are limited to a subset of the ASCII character set, IRIs may contain characters from the Universal Character Set (Unicode/ISO 10646), including Chinese, Japanese kanji, Korean, and Cyrillic characters [[Wikipedia article](https://en.wikipedia.org/wiki/Internationalized_Resource_Identifier)]. 


#### URI
Uniform Resource Identifier (RFC 3986) provides an extensible means for identifying a resource within the World Wide Web. Each URI begins with a scheme name that refers to a specification for assigning identifiers within that scheme; each scheme's specification may further restrict the syntax and semantics of identifiers using that scheme. 

URI specification defines (1) an implementation to access a location on a file server, commonly accessed using the http protocol though other protocols are allowed; (2) a syntax for referencing, through which e.g., ISBNs can be specified as URIs. The network path of the URI is implicitly DNS based; the formal URI specification that allows the URI to be opaque following the scheme name, e.g., 'http:' or 'mailto:', has been generally overtaken by practical usage which assumes that the initial URI parser will look for meaningful characters (such as dot and slash). 

The use of URIs as identifiers that don't actually identify network resources (for example, they identify an abstract object, or a physical object) was recognised as an unanswered problem in RFC 3305. This usage is important in any semantic application. To address this, the info URI scheme (RFC 4452: [http://info-uri.info](http://info-uri.info/)) was developed by library and publishing communities for "URIs of information assets that have identifiers in public namespaces but have no representation within the URI allocation". OpenURL adopted it and was the key motivation for it. The InfoURI registry was closed to further "info" namespace registrations due to evolving practices in resource resolution and identification, but the registry continues to be supported. DOI was registered in the infoURI scheme [[DOI System and Internet Identifier Specifications FactSheet](https://www.doi.org/factsheets/DOIIdentifierSpecs.html)].


#### URN
Uniform Resource Name (RFC 2141) is a specification for defining names (identifiers) of resources for use on the Internet [[DOI System and Internet Identifiers Factsheet](https://www.doi.org/factsheets/DOIIdentifierSpecs.html)]. Locations are assumed to be independent of names. URN resolution is still an active topic of discussion, especially in the library community (e.g. for treatment of National Bibliography Numbers as URN in RFC 3188). RFC 2141 defines (1) a formal registration process as a urn namespace, and (2) accompanying specifications to implement a series of functional requirements for such namespaces. Existing identifiers may thereby be specified as a URN: e.g. an ISBN as urn:isbn:9789521061547; such identifiers may be implemented using a specially written URN plug-in and resolved to URLs: functionally this gives nothing beyond that achieved by coherent management of the corresponding URLs. Note that an active IETF Working Group, "[Uniform Resource Names, Revised](http://datatracker.ietf.org/wg/urnbis/charter/)", has undertaken the task of updating the key URN RFCs, including RFC 2141, which date from 1997-2001, to reflect the URN implementation experience gained since that time. Proposed changes include updating the syntax specification, a formal IANA registration for the 'urn' URI scheme, revised URN examples, and updated descriptions of how URNs are resolved based on current practices. 

URN architecture assumes a DNS-based Resolution Discovery Service (RDS) to find the service appropriate to the given URN scheme. However no such widely deployed RDS schemes currently exist: browsers cannot action URN strings without some additional programming in the form of a "plug-in". These carry no guarantee of ready interoperability with other deployments, which may require a different plug-in for each implementation and may use conflicting data approaches. Therefore most existing URN implementations embed the URN as a http URI which contains the URL of the relevant resolution service (e.g. for the URN form of the ISBN shown above, resolved via the Finnish national URN service http://urn.fi, the actionable form of the URN is [http://urn.fi/URN:ISBN:978-952-10-6154-7](http://urn.fi/URN:ISBN:978-952-10-6154-7)). There is no global service aware of national and/or regional URN resolution services, but there are some proposals to provide one (e.g. [http://www.persid.org](http://www.persid.org/)).

The set of URNs, of the form "urn: nid: nnnnnn", is a URN namespace. ("nid" is here a URN namespace identifier, neither a "URN scheme", nor a "URI scheme.") The official IANA list of registered NIDs at [http://www.iana.org/assignments/urn-namespaces](http://www.iana.org/assignments/urn-namespaces) lists 40 registered NIDs; many of these are not widely used as URNs (e.g., ISSN, ISBN).

To enable the use of DOIs in workflows that have already standardized on URNs, the DOI proxy servers understand the substitution of a colon in place of the initial slash in a DOI name. DOI names may therefore be expressed as URNs in the doi.org domain by writing, for example, the DOI name 10.123/456 in the form https://doi.org/urn:doi:10.123:456. Note, however, that a DOI suffix is allowed to contain other slashes, and where these occur they must be percent-encoded rather than replaced with a colon: for example, the DOI name 10.123/456ABC/zyz would become https://doi.org/urn:doi:10.123:456ABC%2Fzyz, with the final slash character encoded as %2F.

DOI is not registered as a URN namespace, despite fulfilling all the functional requirements, since URN registration appears to offer no advantage to the DOI System. It requires an additional layer of administration for defining DOI as a URN namespace (the string urn:doi:10.1000/1 rather than the simpler doi:10.1000/1) and an additional step of unnecessary redirection to access the resolution service, already achieved through either http proxy or native resolution. If RDS mechanisms supporting URN specifications become widely available, DOI will be registered as a URN.


#### URL
Uniform Resource Locator (RFC 1738) is a location on a file server in the WWW; redefined in RFC 3986 as "a type of URI that identifies a resource via a representation of its primary access mechanism (e.g., its network "location"), rather than by some other attributes it may have". In this view "URL is a useful but informal concept" (RFC 3305) [[DOI System and Internet Identifiers Factsheet](https://www.doi.org/factsheets/DOIIdentifierSpecs.html)]. In practice, it identifies a single location, and therefore is widely used incorrectly as a (mutable) identifier of the resource at that location (so the same resource at two URLs would have two URL "identifiers"). This bad practice arose from the failure to distinguish name and location in early WWW development. Adding to the problem, URLs carry semantics of the Domain Name they are based on and are therefore unsuitable as opaque identifiers; they may also be contextually qualified. URLs are pervasive as the foremost mechanism of location specification throughout the WWW, but less useful outside it. 

Attempts to circumvent the problem of using URLs as citable identifiers by developing persistent identifier alternatives are well documented (PURL, DOI, ARK, etc.).

A DOI name may be represented as a URL (http string) by prefacing the string with https://doi.org/ (or the supported but deprecated http://dx.doi.org) to the DOI of the document (e.g., to resolve the DOI name 10.1000/182, enter into a browser the address: https://doi.org/10.1000/182). Web pages or other hypertext documents can include hypertext links in this form.


#### LSID
Life Science Identifiers (LSID) are a way to name and locate pieces of information on the web. Essentially, an LSID is a unique identifier for some data, and the LSID protocol specifies a standard way to locate the data (as well as a standard way of describing that data). They are a little like DOIs used by many publishers. 

An LSID is represented as a uniform resource name (URN) with the following format: 

*   urn:lsid:<Authority>:<Namespace>:<ObjectID>[:<Version>]

The _lsid:_ namespace, however, is not registered with the Internet Assigned Numbers Authority (IANA), and so these are not strictly URNs or URIs.

LSIDs may be resolved in URLs, e.g. [http://zoobank.org/urn:lsid:zoobank.org:pub:CDC8D258-8F57-41DC-B560-247E17D3DC8C](http://zoobank.org/urn:lsid:zoobank.org:pub:CDC8D258-8F57-41DC-B560-247E17D3DC8C) 

There has been a lot of interest in LSIDs in both the bioinformatics and the biodiversity communities, with the latter continuing to use them as a way of identifying species in global catalogues. However, the use of LSIDs as identifiers has been criticized as violating the Web Architecture good practice of reusing existing URI schemes. Nevertheless, the explicit separation of data from metadata; specification of a method for discovering multiple locations for data-retrieval; and the ability to discover multiple independent sources of metadata for any identified thing were crucial parts of the LSID and its resolution specification that have not successfully been mimicked by an HTTP-only approach. 

The World Wide Web provides a globally distributed communication framework bu several limits and inadequacies were thought to exist. One of them was the inability to programmatically identify locally named objects that may be widely distributed over the network. This perceived shortcoming would have limited the ability to integrate multiple knowledgebases, each of which gives partial information of a shared domain. LSID and LSID Resolution System (LSRS) were designed to provide simple and elegant solutions to this problem. However, it has been pointed out that some of these perceived shortcomings are not intrinsic to HTTP URIs, and much (though not all) of the functionality that LSIDs provide can be obtained using properly crafted HTTP URIs.

#### ARK
Archival Resource Key (ARK) is a Uniform Resource Locator (URL) that provides a multi-purpose identifier given to information objects of any type. ARKs contain the label ark: in the URL, which sets the expectation that the URL terminated by '?' returns a brief metadata record, and the URL terminated by '??' returns metadata that includes a commitment statement from the current service provider [[DOI System and Internet Identifiers Factsheet](https://www.doi.org/factsheets/DOIIdentifierSpecs.html), [ARK Wikipedia entry](https://en.wikipedia.org/wiki/Archival_Resource_Key)].


#### PURL
A persistent uniform resource locator (PURL) is a Uniform Resource Locator (URL) that does not directly describe the location of the resource to be retrieved but instead describes an intermediate (more persistent) location which, when retrieved, results in redirection (e.g. via a 302 HTTP status code) to the current location of the final resource. PURLs are said to be "an interim measure, while Uniform Resource Names (URNs) are being mainstreamed, to solve the problem of transitory URIs in location-based URI schemes like HTTP" [[DOI System and Internet Identifiers Factsheet](https://www.doi.org/factsheets/DOIIdentifierSpecs.html)].


#### Using Domain Names as Persistent Identifiers
Using DNS strings as persistent identifiers brings the advantages of DNS (universal deployment, simplicity and conformance with existing web site management structures) but also the problems of domain names [[DOI System and Internet Identifiers Factsheet](https://www.doi.org/factsheets/DOIIdentifierSpecs.html)]:

*   Including a domain name in an identifier string embeds a piece of intelligence into the identifier. But a domain name is the wrong intelligence to embed in an identifier string since the referent and domain name are not guaranteed to have the same ownership or management in the future. 
*   The attempt to link identifiers with domain names is often based on the assumption that in some way this conveys information about rights. However this assumption conflates several things implied by "ownership" of an ID string: registration of the identifier, management of the identifier, management of the referent, commercial brand, rights associated with the referent, etc. Rights are complex, subject to change, and not usually expressible as one simple string (e.g. consider multiple or shared rights, different territorial rights, time limited rights, dependent rights, etc.) Rights should be expressed through metadata, not in the string itself. 
*   Persistent identifier schemes have among other requirements: 
    *   _Functional granularity:_ it should be possible to identify an entity whenever it needs to be separately distinguished. Since functionality is not pre-determined, hierarchical relationships with other entities are best expressed though metadata, not in the identifier string itself. 
    *   _First class naming:_ it should be possible to assign an identifier which has no embedded dependency on other separately identified entities. It is not possible to satisfy both of these requirements with a DNS naming scheme. 
*   The impression of persistence is as important as actual persistence. Since DNS http strings are widely viewed as not persistent (subject to 404 errors), expressing identifiers as URIs brings the problem that even if well maintained there may be a lack of trust in PIDS expressed as URIs because they look like fragile URLs. 

DOI names are not defined in terms of domain names: the ISO Standard (ISO 26324) is an abstract specification. DOI names can be optionally expressed using domain names (URN, URI), usually expressed in the form including the http proxy doi.org/ (or dx.doi.org/). Propagating these identifiers in this form brings the advantages but also the problems of domain names: http-form DOI names expressed through other proxies would have more fragility. The distinction between the DOI name itself and an expression of it in another form should preferably be clearly maintained. 

DOI name resolution in native resolver form does not require the use of the DNS (Domain Name System), though does of course when used with the proxy resolver. The Handle System is more appropriate for large numbers of digital objects than DNS, and the DNS administrative model argues against using it as a general-purpose name system (DNS administration typically requires a network administrator, and has no provision for administration per name by anyone other than a network administrator). DNS also has well-recognised problems of security and updating which suggest that it will not be sufficient to assume that existing DNS technology should be adapted to deal with new requirements, rather than inventing something new: peer-to-peer networks already presage this.

URLs are grouped by domain name and then by some sort of hierarchical structure, originally based on file trees, now possibly unconnected from that but still a hierarchy. DOI names offer a more finely grained approach to naming where each name stands on its own, unconnected to any DNS or other hierarchy. This offers beneficial flexibility, especially over time, as the document origins reflected in that hierarchy lose meaning, such as a change in ownership which is reflected in DNS.

Functionality such as URL partial redirection and relative URLs (which assume as "known" or inherited a part of a URL/domain name address) make a lot of sense in the context of URLs. However since DOI names/handles deliberately have a more finely grained approach to naming things, functionality such as partial redirection is dealt with through tools which capitalise on the finer granularity made available through DOI names. 


#### Some (negative) considerations about using URN, URL, and URIs as permanent identifiers (PIDs)

Historically there was ambiguity and confusion in the use of these terms. RFC 3986 (2005) aimed to end this by stating that a URI can be classified as a locator, a name, or both. In this view, the term URL refers to the subset of URIs that, in addition to identifying a resource, provide a means of locating the resource; the term URN has been used historically to refer to both URIs under the "urn" scheme (RFC 2141) which are required to remain globally unique and persistent even when the resource ceases to exist or becomes unavailable, and to any other URI with the properties of a name [[DOI System and Internet Identifiers Factsheet](https://www.doi.org/factsheets/DOIIdentifierSpecs.html)]. 

RFC 3986 requires that the terms URL and URN be deprecated. This brings a uniformity to the technical treatment of all URIs; however the risk of confusion remains, from:

1. cited documents which rely on earlier, now superseded, statements of the position; 
2. the use of one simple top level term (URI) may hide useful distinctions which some users, e.g., librarians, may wish to make between a unique name and a location, for example when a named resource is available at multiple locations; 
3. considerations of how widely used non-web identifiers (such as ISBNs, RFIDs, social security numbers, etc) relate to URIs, which can lead to: 
    *   confusions re identifier, representation, and access mechanism; 
    *   lack of appreciation of identifier usage outside the WWW; 
    *   use for non-digital referents; and 
    *   the requirement to perceive the web as only part of the Internet and the Internet as only part of information. 

In the view now considered by RFC 3986 to be obsolete, URIs have two subclasses: URN (identifying names) and URL (identifying single locations). In the RFC 3986 view, web-identifier schemes are all URI schemes, as a given URI scheme may define subspaces; some of these may be access mechanisms (e.g., "http:") whilst others may be namespaces (e.g., "urn:").

:bulb: **The vulnerability of any digital material to unexpected or unintended changes in Internet domain name assignment, and hence to the outcome of domain name resolution, is widely [recognised](http://www.w3.org/2001/tag/2011/12/dnap-workshop/report.html). The fact that domain names are not permanently assigned is regularly cited as one of the main reasons why http: URIs cannot be regarded as persistent identifiers over the long term.**

### Editor notes
It’s essential to differentiate the following notions: 

1. **_entity_** e.g. a gene,
2.  **_entity name_** - the gene’s name,
3. **_entity identifier_** - e.g. the gene identifier,
4. **_entity localization_** - e.g. where to find gene’s (meta)data, information, and knowledge , and
5. **_entity representation_** - e.g. the different “formats” adopted/available for the gene’s (meta). 

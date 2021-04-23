# SHACL (Shapes Constraint Language)

## Resources:
* [W3C SHACL Standard](https://www.w3.org/TR/shacl/)
* TopQuadrant blog post: [An Overview of SHACL features and specificatons.](https://www.topquadrant.com/shacl-features-and-specifications/)
* SPIN website blog post: [SHACL and OWL Compared](https://spinrdf.org/shacl-and-owl.html)
* SHACL contraint extension namespace: [DASH](http://datashapes.org/)

## Introduction
On 2017 the [Shapes Constraint Language (SHACL)](https://www.w3.org/TR/shacl/) became an official W3C Recommendation, lifting it into the same league as other RDF-based standards including RDF Schema, OWL, SPARQL, Turtle and JSON-LD. This article examines the similarities and differences between SHACL and the Web Ontology Language (OWL). It explains that OWL has been designed for classification tasks (inferencing in an "open world"), while SHACL covers data validation (in a "closed world") similar to traditional schema languages. Given this division of roles, both technologies can be used together or individually. Further, SHACL can also be used for general purpose rule-based inferencing.

## RDF, RDFS, and OWL
The base specification of the W3C semantic technology stack is RDF, a simple and flexible graph-based data model based on URIs and literals that form triples. Parallel to the first version of RDF, work was underway to produce schema and ontology languages leading to the publication of the RDF Schema and Web Ontology Language (OWL) specifications. RDF Schema introduced a core vocabulary to declare classes (using `rdfs:Class`) and properties (using `rdf:Property`), and built-in properties to associate properties with classes (`rdfs:domain`) and to declare their value types (using `rdfs:range`). RDF Schema is not a schema language in a traditional sense (like for example XML Schema). It is a very minimalistic language designed for inferencing rather than to enforce adherence of data to a schema.

The Web Ontology Language (OWL) was designed as an extension of RDF Schema. OWL added properties such as `owl:maxCardinality` to express so-called restrictions. A very important thing to note is that, like RDFS, OWL was designed for inferencing. OWL restrictions are not actually data constraints, but rather describe inferences to be applied based on them. When the addition of new facts results in logical contradictions, then the processor will report an error. However, the error is rarely traceable to the original data statement(s) that have caused the contradictions.

Another consequence of the Open-World Assumption is that an application should not assume that the absence of a certain statement means that the statement is false. This situation is not reported as an error by an OWL processor, because more data may appear at any time to satisfy that restriction under the Open-World Assumption.

## Data Validation prior to SHACL
Although data validation is an important practical use case for the RDF stack, until SHACL came around, there was no W3C standard mechanism for defining data constraints. 

A practical solution that has emerged over the years is writing SPARQL queries that test an RDF data graph against conditions expressed in a SPARQL WHERE clause. SPARQL does not make any assumptions about inferences being present or not. Instead, a SPARQL processor simply queries the triples that are actually asserted in the data.

## Creation of SHACL
Recognizing the lack of a suitable standard to express constraints and schemas with a Closed World Assumption, the W3C launched the [RDF Data Shapes Working Group](https://www.w3.org/2014/data-shapes/wiki/Main_Page) in 2014. The group was using [SPIN](https://spinrdf.org/) and other member submissions such as IBM's [Resource Shapes](https://www.w3.org/Submission/shapes/) as input, and the term shape was established to mean a collection of constraints that apply to targetted RDF resources. After a couple of years of intense discussions, the Shapes Constraint Language (SHACL) was standardized as a W3C Recommendation in 2017.

SHACL provides a high-level vocabulary and a fallback mechanism that allows users to express basically any constraint using SPARQL or (using the SHACL-JS extension) in JavaScript. The high-level vocabulary makes SHACL also a schema/ontology language, allowing tools to examine the structure of a class to, for example, suggest how instances of a class should be presented on an input form. Since OWL performs (limited, as described above) data validation through inferencing, it has no separation between data validation and reasoning. _**SHACL separates checking data validity from inferring new facts. Both, however, are possible with SHACL**_.

## OWL and SHACL comparison
### Class Hierarchies
The backbone of most data models are classes, arranged in a sub-class or sub-set hierarchy. Both OWL and SHACL rely on RDF Schema vocabulary for the basic mechanism.

OWL defines its own metaclass owl:Class which can be used instead of rdfs:Class. SHACL treats all these classes in the same way, assuming the graph contains triples that declare owl:Class (or any other metaclass) as a sub-class of rdfs:Class. Some SHACL features such as sh:class and sh:targetClass are defined to automatically "walk" up the superclass hierarchy based on the rdfs:subClassOf relationship. So overall this central concept of schemas and ontologies is handled very similarly between OWL and SHACL.

### Defining constraints
OWL is operating on classes, which are understood as sets of instances that satisfy the same restrictions. OWL includes the metaclass `owl:Restriction` which is typically used as an anonymous superclass of the named class that the restriction is about.

OWL sentences to state that a `ex:Person` cannot have more than one `ex:hasFather` and this value must be an instance of `ex:Person` is shown following. It apply to all instances of the class and its subclasses
```owl
ex:Person
	a owl:Class ;
	rdfs:subClassOf [
		a owl:Restriction ;
		owl:onProperty ex:hasFather ;
		owl:maxCardinality 1 ;
	] ;
	rdfs:subClassOf [
		a owl:Restriction ;
		owl:onProperty ex:hasFather ;
		owl:allValuesFrom ex:Person ;
	] .
```
The direct SHACL equivalent of the OWL example above would be:
```shacl
ex:Person
	a owl:Class, sh:NodeShape ;
	sh:property [
		sh:path ex:hasFather ;
		sh:maxCount 1 ;
		sh:class ex:Person ;
	] .
```
But it is also possible to decouple the class definition and its "shape" and to use`sh:targetClass` to link the shape to its target class(es). This design is sometimes cleaner if shape definitions should be reused in multiple classes or if they are developed by different people, in different files or namespaces.
```shacl
ex:Person
	a owl:Class .

ex:PersonShape
	a sh:NodeShape ;
	sh:targetClass ex:Person ;
	sh:property [
		sh:path ex:hasFather ;
		sh:maxCount 1 ;
		sh:class ex:Person ;
	] .
```
:bulb: A difference between OWL and SHACL is the presence of global property axioms such as `owl:FunctionalProperty` in OWL. SHACL does not have such global constraints but they can be expressed using shapes that apply to all places where a property is present, for example using `sh:targetObjectsOf`. SHACL shapes are not limited to only target instances of a class. In addition, the SHACL vocabulary includes terms to state that a shape applies to all subjects or objects that have values for a certain property. SHACL also offers a way to state that a shape applies to a specific resource or a group of resources by listing their URIs. The [custom targets](https://www.w3.org/TR/shacl-af/#targets) take this further and even support the selection of target nodes based on SPARQL queries or JavaScript code. This makes is possible to precisely state, based on some criteria, which nodes a given shape should apply. Overall, SHACL is vastly more flexible here than OWL.

### Built-in constraint types
[This](https://www.w3.org/TR/shacl/#core-components) section of the standard defines the built-in SHACL Core constraint components that must be supported by all SHACL Core processors, and [here](https://spinrdf.org/shacl-and-owl.html) is presented a table comparing OWL and SHACL-based constraints.

:bulb: SHACL offers its extension mechanisms allowing anyone to create their own constraint types. Such extension namespaces can be published on the web, for anyone to reuse. An example of such an extension namespace is the [DASH Data Shapes Vocabulary](http://datashapes.org/).

:bulb: With some notable exceptions, the expressivity of OWL is comparable to the SHACL Core vocabulary. A syntactic translation between OWL and SHACL is straight-forward in most cases. 

There are some use cases that cannot be covered by OWL. For example, to state that values of `schema:address` can be either resources of type `schema:PostalAddress` or literals with datatype `xsd:string` is not possible in OWL because object and datatype properties are disjoint. In SHACL it would be:
```shacl
schema:Person
	a owl:Class, sh:NodeShape ;
	sh:property [
		sh:path schema:address ;
		sh:or (
			[ sh:class schema:PostalAddress ]
			[ sh:datatype xsd:string ]
		)
	] .
```

### Additional SHACL Validation Features
A key differentiator between SHACL and OWL is that SHACL is extensible ([SHACL-SPARQL](https://www.w3.org/TR/shacl/#sparql-constraints) and [SHACL-JS](https://www.w3.org/TR/shacl-js/)) while OWL is limited to exactly the features that have been specified by the OWL committee. In particular, SPARQL includes the concept of variables which has no equivalent in OWL and therefore makes it impossible to express many real-world use cases in OWL alone. 

SHACL includes a very rich [results vocabulary](https://www.w3.org/TR/shacl/#validation-report) in which the results of the validation process are returned. For example it includes both human-readable and machine-readable pointers to specific data that violates SHACL constraints, the specific constraint type that was violated, and it distinguishes errors from warnings and informational results.

Individual SHACL shapes can be switched off using `sh:deactivated`, which greatly improves the reusability of other people's data shapes.

Not specific to validation, SHACL also includes [standard properties](https://www.w3.org/TR/shacl/#nonValidation) to represent annotations that may be used to drive user interfaces, in particular forms, including sh:group, sh:order and sh:defaultValue. 

### Inferencing
OWL has been designed to support inferencing, but it is only practically applicable for certain kinds of inferencing. As a result, users have been supplementing OWL with SWRL rules, SPIN rules or simply with a set of SPARQL CONSTRUCT queries. Although SHACL has been originally designed to focus on data validation, it also includes support for rule-based inferencing. The SHACL features described in this section are not part of the SHACL W3C Recommendation but have been published by the W3C working group in the [WG Note SHACL Advanced Features](https://www.w3.org/TR/shacl-af).

There are notable differences between the types of inferences supported by both languages. OWL is centered around classification problems, i.e. it can be used to find subclass relationships (`rdfs:subClassOf` triples), instance-of relationships (`rdf:type`) and find equivalent (`owl:sameAs`) or different (`owl:differentFrom`) individuals. However, this is basically all that OWL inferencing is capable of. SHACL can be used to infer arbitrary triples, including the above use cases.

### Why and when to use OWL, SHACL or both
During the 13 years since its becoming a standard OWL has not achieved a critical mass of adoption outside of academia. Maybe the main reason for this lays in the design principles of OWL. Despite large amounts of education material, newcomers to OWL continue to run into the same stumbling blocks as in 2004:
* Confusion about the meaning of restrictions - in particular that OWL does not constrain anything but rather describes inferences.
* The built-in nature of the Open World Assumption and the Unique Name Assumption - it contradicts established approaches from schema languages and makes the meaning of certain statements (e.g., cardinality) different from what most modelers expect.

SHACL comes with much more traditional semantics and offers a flexible approach to data modeling on graph structures. With SHACL's inferencing features, for most projects there is really no strong reason to stay with OWL.

:bulb: **However, a number of useful OWL ontologies have been built and people may need to leverage them or interact with them. One option is a port to SHACL. Another option is co-existence of OWL and SHACL models. We now examine some techniques on how to deal with this situation.*

#### Combining SHACL and OWL by means of RDF Graphs.
The shared base technology of both OWL and SHACL is RDF, which is built on top of URIs. Anyone is allowed to add statements about such URIs. Some of these statements will be interpreted by OWL engines, others will only carry meaning for SHACL processors. Humans can read and understand both.

RDF is also built on the concept of graphs, which are typically represented by individual files or databases. RDF graphs are sets of triples. Such sets can be merged into larger sets, forming union graphs. Tools can then operate on RDF triples merged from multiple sources.

Based on URIs and graphs, it is perfectly fine for any URI resource to carry different triples for different purposes. If you define a class with the URI `ex:Customer` then you may attach both OWL axioms and SHACL constraints to the same class. In order to keep things tidy and organized, it is advisable to create multiple graphs in multiple files.

![](https://i.imgur.com/A2GusgL.png)

As a result of this design, applications can dynamically decide which features of the data model they need for their specific task, by merging the sub-graphs that they require. A SHACL validator does not need the OWL axioms and vice versa.

Then, it is recommended the use of individual graphs when RDF/OWL/SHACL files are published. Note that SHACL includes two properties `sh:shapesGraph` and `sh:suggestedShapesGraph` that can be used to link from an RDF Schema or instances file to shapes graphs. And then there is `rdfs:seeAlso`.

:bulb: **In some use cases, SHACL's closed-world assumption does not work well. Your data may have links to external RDF resources that are not part of the graph that is being validated. For example, your sales database may reference a customer by a URI via `ex:customer ex:JohnDoe` but there is no triple stating that `ex:JohnDoe` is actually an instance of `ex:Customer`. In such cases, using a closed-world `sh:class ex:Customer` constraint would be a poor choice. Such cases can typically be covered with `rdfs:range` statements, which provide tools with a hint of the nature of values without being overly strict. A SHACL shape may limit itself to stating that the values must be URIs, using `sh:nodeKind sh:IRI.`**

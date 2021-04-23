# LPG vs RDF

This documents compares pros and cons of the two more widely-known graph data models.
***Source:*** _Knowledge Graphs versus Property Graphs: Similarities, Differences, and Some Guidance on Capabilities. TopQuadrant WhitePaper._ 

## Property Graphs
### Overview
They are also known as Labeled Property Graphs (LPG). In general, the property grah data model consists of three elements:
1. **Nodes** (or vertices) are the entities of the graph. They can be tagged with zero to many text labels representing their types.
2. **Edges** (or relationships) are the directed links between nodes. Each edge has a type. Altough they are directed, they can be navigated and queried in either direction.
3. **Properties** are the key-value pairs associated with a node or an edge. Propery values can have data types.

![](https://i.imgur.com/boZHQC2.png)
_Figure. Property Graph excerpt with information about artists and their works._

### Remarks
* While there are core commonalities in property graphs implementations, there is no true standard property graph data model. The most well-known implementation, which popularized property graphs as a concept, is the Neo4J graph database.
* No industry standard query language exists for property graphs. Instead, each database offers its own, unique query langague that is incompatible with others. Neo4J Cypher (aldo known as CQL) is the more widely-known. Apache TinkerPop is an open source graph computing framework that is integrated with some LPG and RDF graph databases. It offers the Gremlin languag, which is more an API language than a query language.   
* Nodes and edges have IDs, which are assigned by a database and are internal to such database. Referencing is done by using text strings - node labels, relationship types, and property names.
* There is no standard serialization format.

### Highlightings
* **No standard data model**
* **No standard query language**
* **No standard serialization format**
* **No global IDs, just database scoped ones.**

## Resource Descrition Framework
### Overview
The RDF graph data model basically consists of two elements:
* **Nodes** or vertices. They can be resources with ID or literal (string, number, etc.)
* **Edges**, also known as predicates or properties. Two nodes connected by a predicate form a subject-pbject-predicate which is known as a _Triple_ or _Triple statement_. Altough edges are directed, they can be navigated or queried in either direction.
Everything in RDF graph is a resource. An "edge" or "node" is just the role a resource play in a given statement. 

![](https://i.imgur.com/djZdCoS.png)
_Figure. An RDF graph representing the same information that the previous Property Graph._

### Remarks
* RDF graphs use the standard graph data model managed by the W3C.
* RDF Graphs capture data (instances) and semantics (model). They capture meaning or semantics of data, including rich constraints and highly expressive rules. All information can be stored in a graph and is available for query and any other algorithm  (logic reasoning, graph analytics - node centrality, node similarity, shortest path, clustering, etc) able to reason and discover new knowledge.
* There is a standard query language named SPARQL, wich is both a query language and a HTTP protocol making it possible to send query requests to endpoints over HTTP.
* RDG Graph can be partitioned in Named Graphs. A Named Graph offers a way to group a sub-set of the triple statements, by assigning it a uniquely identifying name and associate to it any other information. A single statement can belong to many Named Graphs. In some way, it's similar to _views_ in relational databases.
* There is a set of standard serializations, i.e., Turtle, JSON-LD, etc. 
* Literal values nodes, from the data structure perspective, are part of the graph just like any other node. The only difference is that they can only be used as the target of a relation -i.e., they cannot be the source or subject of a triple-. 
* Every non-literal node is assigned an ID (typically an URI/IRI). Local identifiers are also possible, but they are rarely used because they are not interoperable. The URI identifying nodes are commonly shown as qualified names (Qname), which presents the namespace part of the URI as a prefix. E.g., the prefix `rdf:` represents the built-in standard namespace w3.org/1999/02/22-rdf-syntax-ns#. These namespaces define the semantics (the model behind) the graph model. In the case of the `rdf:` namespace, such semantics is defined in the standard. The part of the Qname after the prefix is called local name, and it can be generated applying any strategy able to uniquely identify a resource within a namespace.
* SHACL is a language for defining rules and contraints for RDF Graphs. It offers an approach to ensure the integrity of RDF data and model. For example, it allows (1) to consult a graph to find out what properties are appropiate for specific resource, (2) define contraints also known as rich data quality/validity rules, and (3) define rich inference rules able to generate new facts from the fact in the graph.  
* RDF Statements can be used to state properties about other properties, as shown in following figure. See also further standards RDF* (aka RDF Star and RDF Plus) and SPARQL* ([W3C Workshop](https://www.w3.org/Data/events/data-ws-2019/index.html) and [blog post](https://blog.liu.se/olafhartig/2019/01/10/position-statement-rdf-star-and-sparql-star/))
![](https://i.imgur.com/w96mrLF.png)
_Figure. RDF Graph showing making a statement about another statement to attach information on an edge between two specific nodes._

### Highlighting
* **Standard data model**
* **Standard query language**
* **Standard language to define rules and contraints.**
* **Standard serialization format**
* **Global IDs, just database scoped ones.**
* **Partitionable into Named Graphs.**
* **N-ary relationships trough RDF Statements.**

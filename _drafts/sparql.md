# SPARQL

Standards
*   SPARQL 1.1 Overview. [https://www.w3.org/TR/sparql11-overview/](https://www.w3.org/TR/sparql11-overview/)

Tutorials

*   Introduction to SPARQL. Stardog tutorial. [https://www.stardog.com/tutorials/getting-started-1/](https://www.stardog.com/tutorials/getting-started-1/)
*   Six degrees of Kevin Bacon. Stardog tutorial. [https://www.stardog.com/tutorials/getting-started-2/](https://www.stardog.com/tutorials/getting-started-2/)
*   SPARQL. Stardog tutorial. [https://www.stardog.com/tutorials/sparql/](https://www.stardog.com/tutorials/sparql/)

## Introduction
SPARQL 1.1 is a set of specifications that provide languages and protocols to query and manipulate RDF graph content on the Web or in an RDF store. The standard comprises the following specifications:
*   [SPARQL 1.1 Query Language](http://www.w3.org/TR/sparql11-query/). A query language for RDF.
*   [SPARQL 1.1 Query Results JSON Format](http://www.w3.org/TR/sparql11-results-json/) and [SPARQL 1.1 Query Results CSV and TSV Formats](http://www.w3.org/TR/sparql11-results-csv-tsv/), apart from the standard SPARQL Query Results XML Format.
*   [SPARQL 1.1 Federated Query](http://www.w3.org/TR/sparql11-federated-query/). A specification defining an extension of the SPARQL 1.1 Query Language for executing queries distributed over different SPARQL endpoints.
*   [SPARQL 1.1 Entailment Regimes](http://www.w3.org/TR/sparql11-entailment/). A specification defining the semantics of SPARQL queries under entailment regimes such as RDF Schema, OWL, or RIF.
*   [SPARQL 1.1 Update Language](http://www.w3.org/TR/sparql11-update/). An update language for RDF graphs.
*   [SPARQL 1.1 Protocol for RDF](http://www.w3.org/TR/sparql11-protocol/). A protocol defining means for conveying arbitrary SPARQL queries and update requests to a SPARQL service.
*   [SPARQL 1.1 Service Description](http://www.w3.org/TR/sparql11-service-description/). A specification defining a method for discovering and a vocabulary for describing SPARQL services.
*   [SPARQL 1.1 Graph Store HTTP Protocol](http://www.w3.org/TR/sparql11-http-rdf-update/). As opposed to the full SPARQL protocol, this specification defines minimal means for managing RDF graph content directly via common HTTP operations.
> [name=ereynrs][color=yellow] Read carefully this spec, could be useful to implement RDF-based microservices architectures.]
*   [SPARQL 1.1 Test Cases](http://www.w3.org/2009/sparql/docs/tests/). A suite of tests, helpful for understanding corner cases in the specification and assessing whether a system is SPARQL 1.1 conformant.

## SPARQL queries
Most forms of SPARQL query contain a set of triple patterns called a _basic graph pattern_. Triple patterns are like RDF triples except that each of the subject, predicate and object may be a variable. A basic graph pattern _matches_ a subgraph of the RDF data when RDF terms from that subgraph may be substituted for the variables and the result is RDF graph equivalent to the subgraph.
Results of SPARQL queries can be exchanged in machine-readable form. SPARQL supports four common exchange formats, namely the Extensible Markup Language, the JavaScript Object Notation, Comma Separated Values, and Tab Separated Values.

## SPARQL syntax highlightings
### Predicate-Object Lists
Triple patterns with a common subject can be written so that the subject is only written once and is used for more than one triple pattern by employing the ";" notation.

```
?x  foaf:name  ?name ;
    foaf:mbox  ?mbox .
```

This is the same as writing the triple patterns:

```
?x  foaf:name  ?name .
?x  foaf:mbox  ?mbox .
```

### Object Lists
If triple patterns share both subject and predicate, the objects may be separated by ```,```.

```
?x foaf:nick  "Alice" , "Alice_" .
```

is the same as writing the triple patterns:

```
?x  foaf:nick  "Alice" .
?x  foaf:nick  "Alice_" .
```

Object lists can be combined with predicate-object lists:

```
?x  foaf:name ?name ; foaf:nick  "Alice" , "Alice_" .
```

is equivalent to:

```
?x  foaf:name  ?name .
?x  foaf:nick  "Alice" .
?x  foaf:nick  "Alice_" .
```
 
### Collections
RDF collections can be written in triple patterns using the syntax ```(element1 element2 ...)```. The form ```()``` is an alternative for the IRI http://www.w3.org/1999/02/22-rdf-syntax-ns#nil. When used with collection elements, such as ```(1 ?x 3 4)```, triple patterns with blank nodes are allocated for the collection. The blank node at the head of the collection can be used as a subject or object in other triple patterns. The blank nodes allocated by the collection syntax do not occur elsewhere in the query.

```
(1 ?x 3 4) :p "w" .
```

is syntactic sugar for (noting that ```b0```, ```b1```, ```b2``` and ```b3``` do not occur anywhere else in the query):

```
_:b0 rdf:first 1 ;
    rdf:rest _:b1 .

_:b1 rdf:first ?x ;
    rdf:rest _:b2 .

_:b2 rdf:first 3 ;
    rdf:rest _:b3 .

_:b3 rdf:first 4 ;
    rdf:rest rdf:nil .

_:b0 :p "w" . 
```

RDF collections can be nested and can involve other syntactic forms:

```
(1 [:p :q] ( 2 ) ) .
```

is syntactic sugar for:

```
_:b0 rdf:first 1 ;
    rdf:rest _:b1 .

_:b1 rdf:first _:b2 .
    _:b2 :p :q .

_:b1 rdf:rest _:b3 .

_:b3 rdf:first _:b4 .

_:b4 rdf:first 2 ;
    rdf:rest rdf:nil .

_:b3 rdf:rest rdf:nil .
```

### Blank node labels in query results
Query results can contain blank nodes. Blank nodes in the example result sets in this document are written in the form ```_:``` followed by a blank node label. 
Blank node labels are scoped to a result set or, for the ```CONSTRUCT``` query form, the result graph. Use of the same label within a result set indicates the same blank node. There need not be any relation between a label ```_:a``` in the result set and a blank node in the data graph with the same label. An application writer should not expect blank node labels in a query to refer to a particular blank node in the data.
Blank nodes in graph patterns act as variables, not as references to specific blank nodes in the data being queried.
Blank nodes are indicated by either the label form, such as ```_:abc```, or the abbreviated form ```[]```. A blank node that is used in only one place in the query syntax can be indicated with ```[]```. A unique blank node will be used to form the triple pattern. Blank node labels are written as ```_:abc``` for a blank node with label ```abc```. The same blank node label cannot be used in two different basic graph patterns in the same query.
The ```[:p :v]``` construct can be used in triple patterns. It creates a blank node label which is used as the subject of all contained predicate-object pairs. The created blank node can also be used in further triple patterns in the subject and object positions.
The following two forms:

* ```[ :p "v" ] .```
* ```[] :p "v" .```

allocate a unique blank node label (here ```b57```) and are equivalent to writing: ```_:b57 :p "v" .```

This allocated blank node label can be used as the subject or object of further triple patterns. For example, as a subject:

```[ :p "v" ] :q "w" .```

which is equivalent to the two triples:

```
_:b57 :p "v" .
_:b57 :q "w" .
```

and as an object:
```
:x :q [ :p "v" ] .
```

which is equivalent to the two triples:
```
:x  :q _:b57 .
_:b57 :p "v" .
```

Abbreviated blank node syntax can be combined with other abbreviations for common subjects and common predicates.
```
[ foaf:name  ?name ;
    foaf:mbox  <mailto:alice@example.org> ]
```

This is the same as writing the following basic graph pattern for some uniquely allocated blank node label, ```b18```:
```
_:b18  foaf:name  ?name .
_:b18  foaf:mbox  <mailto:alice@example.org> .
```

### Relative URIs
Relative IRIs are combined with base IRIs as per Uniform Resource Identifier (URI): Generic Syntax of RFC3986, using only the basic algorithm in section 5.2. Neither Syntax-Based Normalization nor Scheme-Based Normalization (described in sections 6.2.2 and 6.2.3 of RFC3986) are performed.

The following fragments are some of the three different ways to write the same IRI:

* <http://example.org/book/book1>
* BASE <http://example.org/book/>
<book1>
* PREFIX book:<http://example.org/book/>
book:book1

## Graph Patterns
SPARQL is based around graph pattern matching. More complex graph patterns can be formed by combining smaller patterns in various ways:
*   _Basic Graph Patterns_, where a set of triple patterns must match
*   _Group Graph Pattern_, where a set of graph patterns must all match
*   _Optional Graph patterns_, where additional patterns may extend the solution
*   _Alternative Graph Pattern_, where two or more possible patterns are tried
*   _Patterns on Named Graphs_, where patterns are matched against named graphs

This section describes the two forms that combine patterns by conjunction: basic graph patterns, which combine triples patterns, and group graph patterns, which combine all other graph patterns.

### Basic Graph Patterns
Basic graph patterns are sets of triple patterns. SPARQL graph pattern matching is defined in terms of combining the results from matching basic graph patterns.

A sequence of triple patterns, with optional filters, comprises a single basic graph pattern. Any other graph pattern terminates a basic graph pattern.

### Group Graph Patterns
In a SPARQL query string, a group graph pattern is delimited with braces: ```{}```. For example, this query's query pattern is a group graph pattern of one basic graph pattern.
```
PREFIX foaf: &lt;http://xmlns.com/foaf/0.1/>
SELECT ?name ?mbox
	WHERE {
        ?x foaf:name ?name .
		?x foaf:mbox ?mbox .
    }
```
The same solutions would be obtained from a query that grouped the triple patterns into two basic graph patterns. For example, the query below has a different structure but would yield the same solutions as the previous query: 
```
PREFIX foaf: &lt;http://xmlns.com/foaf/0.1/>
	SELECT ?name ?mbox
	WHERE 	{ ?x foaf:name ?name . }
			{ ?x foaf:mbox ?mbox . }
```

## Subqueries
Subqueries are a way to embed SPARQL queries within other queries, normally to achieve results which cannot otherwise be achieved, such as limiting the number of results from some sub-expression within the query.

Due to the bottom-up nature of SPARQL query evaluation, the subqueries are evaluated logically first, and the results are projected up to the outer query.

Note that only variables projected out of the subquery will be visible, or in scope, to the outer query.

## Query forms
SPARQL has four query forms. These query forms use the solutions from pattern matching to form result sets or RDF graphs. The query forms are:
* **_SELECT_**: Returns all, or a subset of, the variables bound in a query pattern match.
* **_CONSTRUCT_**: Returns an RDF graph constructed by substituting variables in a set of triple templates. 
* **_ASK_**: Returns a boolean indicating whether a query pattern matches or not. 
* **_DESCRIBE_**: Returns an RDF graph that describes the resources found. 

### SELECT
The ```SELECT``` form of results returns variables and their bindings directly. It combines the operations of projecting the required variables with introducing new variable bindings into a query solution.

Specific variables and their bindings are returned when a list of variable names is given in the ```SELECT``` clause. The syntax ```SELECT *``` is an abbreviation that selects all of the variables that are in-scope at that point in the query. It excludes variables only used in ```FILTER```, in the right-hand side of MINUS, and takes account of subqueries.  Use of ```SELECT *``` is only permitted when the query does not have a ```GROUP BY``` clause.

### CONSTRUCT
The ```CONSTRUCT``` query form returns a single RDF graph specified by a graph template. The result is an RDF graph formed by taking each query solution in the solution sequence, substituting for the variables in the graph template, and combining the triples into a single RDF graph by set union.

The solution modifiers of a query affect the results of a ```CONSTRUCT``` query. In this example, the output graph from the ```CONSTRUCT``` template is formed from just two of the solutions from graph pattern matching.
If any such instantiation produces a triple containing an unbound variable or an illegal RDF construct, such as a literal in subject or predicate position, then that triple is not included in the output RDF graph. The graph template can contain triples with no variables (known as ground or explicit triples), and these also appear in the output RDF graph returned by the ```CONSTRUCT``` query form.

A template can create an RDF graph containing blank nodes. The blank node labels are scoped to the template for each solution. If the same label occurs twice in a template, then there will be one blank node created for each query solution, but there will be different blank nodes for triples generated by different query solutions.

Using ```CONSTRUCT```, it is possible to extract parts or the whole of graphs from the target RDF dataset. This first example returns the graph (if it is in the dataset) with IRI label http://example.org/aGraph; otherwise, it returns an empty graph.
```
CONSTRUCT { ?s ?p ?o } 
WHERE { GRAPH <http://example.org/aGraph> { ?s ?p ?o } . }
```
The access to the graph can be conditional on other information. For example, if the default graph contains metadata about the named graphs in the dataset, then a query like the following one can extract one graph based on information about the named graph:
```
PREFIX dc:<http://purl.org/dc/elements/1.1/>
PREFIX app:<http://example.org/ns#>
PREFIX xsd:<http://www.w3.org/2001/XMLSchema#>

CONSTRUCT { ?s ?p ?o } WHERE
     {
       GRAPH ?g { ?s ?p ?o } .
       ?g dc:publisher <http://www.w3.org/> .
       ?g dc:date ?date .
       FILTER ( app:customDate(?date) > "2005-02-28T00:00:00Z"^^xsd:dateTime ) .
     }
```
A short form for the ```CONSTRUCT``` query form is provided for the case where the template and the pattern are the same and the pattern is just a basic graph pattern (no ```FILTER```s and no complex graph patterns are allowed in the short form). The keyword ```WHERE``` is required in the short form.

Following examples show the _complete_ and _short_ form of a query.

* Complete form
```
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
CONSTRUCT { ?x foaf:name ?name }
WHERE
    { ?x foaf:name ?name }
```
* Short form
```
PREFIX foaf:<http://xmlns.com/foaf/0.1/>
CONSTRUCT WHERE { ?x foaf:name ?name } 
```
### ASK
Applications can use the ```ASK``` form to test whether or not a query pattern has a solution. No information is returned about the possible query solutions, just whether or not a solution exists.

### DESCRIBE
The ```DESCRIBE``` form returns a single result RDF graph containing RDF data about resources. This data is not prescribed by a SPARQL query, where the query client would need to know the structure of the RDF in the data source, but, instead, is determined by the SPARQL query processor. The query pattern is used to create a result set. The ```DESCRIBE``` form takes each of the resources identified in a solution, together with any resources directly named by IRI, and assembles a single RDF graph by taking a _description_ which can come from any information available including the target RDF Dataset. The description is determined by the query service. The syntax ```DESCRIBE *``` is an abbreviation that describes all of the variables in a query.

The resources to be described can also be taken from the bindings to a query variable in a result set. If the query pattern has multiple solutions, the RDF data for each is the union of all RDF graph descriptions. This enables description of resources whether they are identified by IRI or by blank node in the dataset:
```
PREFIX foaf:<http://xmlns.com/foaf/0.1/>
DESCRIBE ?x
WHERE { ?x foaf:mbox <mailto:alice@org> }
```

## Filter expressions
Graph pattern matching produces a solution sequence, where each solution has a set of bindings of variables to RDF terms. SPARQL ```FILTER```s restrict solutions to those for which the filter expression evaluates to ```TRUE```.

A constraint, expressed by the keyword ```FILTER```, is a restriction on solutions over the whole group in which the filter appears. The following patterns all have the same solutions:
```
{   ?x foaf:name ?name .
    ?x foaf:mbox ?mbox .
    FILTER regex(?name, "Smith")
}
```
```
{   FILTER regex(?name, "Smith")
    ?x foaf:name ?name .
    ?x foaf:mbox ?mbox .
}
```
```
{   ?x foaf:name ?name .
    FILTER regex(?name, "Smith")
    ?x foaf:mbox ?mbox .
}
```
SPARQL ```FILTER``` functions like regex can test RDF literals. regex matches only string literals. regex can be used to match the lexical forms of other literals by using the str function. The regular expression language is defined by XQuery 1.0 and XPath 2.0 Functions and Operators and is based on XML Schema Regular Expressions.

### Effective Boolean Value (EBV)
Effective boolean value is used to calculate the arguments to the logical functions logical-and, logical-or, and ```fn:not```, as well as evaluate the result of a ```FILTER``` expression.

The XQuery Effective Boolean Value rules rely on the definition of XPath's ```fn:boolean```. The following rules reflect the rules for ```fn:boolean``` applied to the argument types present in SPARQL queries:

*   The EBV of any literal whose type is ```xsd:boolean``` or numeric is false if the lexical form is not valid for that datatype (e.g. ```"abc"^^xsd:integer```).
*   If the argument is a typed literal with a datatype of ```xsd:boolean```, and it has a valid lexical form, the EBV is the value of that argument.
*   If the argument is a plain literal or a typed literal with a datatype of ```xsd:string```, the EBV is false if the operand value has zero length; otherwise the EBV is true.
*   If the argument is a numeric type or a typed literal with a datatype derived from a numeric type, and it has a valid lexical form, the EBV is false if the operand value is NaN or is numerically equal to zero; otherwise the EBV is true.
*   All other arguments, including unbound arguments, produce a type error.

An EBV of true is represented as a typed literal with a datatype of ```xsd:boolean``` and a lexical value of ```TRUE```; an EBV of false is represented as a typed literal with a datatype of ```xsd:boolean``` and a lexical value of ```false```.
 
### Testing values
SPARQL expressions are constructed according to the grammar and provide access to functions (named by IRI) and operator functions (invoked by keywords and symbols in the SPARQL grammar). SPARQL operators can be used to compare the values of typed literals:

```
PREFIX a:<http://www.w3.org/2000/10/annotation-ns#>
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX xsd:<http://www.w3.org/2001/XMLSchema#>
SELECT ?annot
WHERE { ?annot  a:annotates <http://www.w3.org/TR/rdf-sparql-query/> .
        ?annot  dc:date      ?date .
        FILTER ( ?date > "2005-01-01T00:00:00Z"^^xsd:dateTime ) }
```

In addition, SPARQL provides the ability to invoke arbitrary functions, including a subset of the XPath casting functions. These functions are invoked by name (an IRI) within a SPARQL query. For example:
```
FILTER ( xsd:dateTime(?date) xsd:dateTime("2005-01-01T00:00:00Z") )
```

### Function Forms
Operators and functions introduced by the SPARQL Query language are presented following.

#### BOUND
```
xsd:boolean  BOUND (variable var)
```
Returns true if var is bound to a value. Returns false otherwise. Variables with the value ```NaN``` or ```INF``` are considered bound.
#### IF
```
rdfTerm  IF (expression1, expression2, expression3)
```
The ```IF``` function form evaluates the first argument, interprets it as a effective boolean value, then returns the value of ```expression2``` if the EBV is true, otherwise it returns the value of ```expression3```. Only one of ```expression2``` and ```expression3``` is evaluated. If evaluating the first argument raises an error, then an error is raised for the evaluation of the ```IF``` expression.

#### COALESCE
```
rdfTerm  COALESCE(expression, .…)
```
The ```COALESCE``` function form returns the RDF term value of the first expression that evaluates without error. In SPARQL, evaluating an unbound variable raises an error. If none of the arguments evaluates to an RDF term, an error is raised. If no expressions are evaluated without error, an error is raised.

#### EXIST AND NOT EXIST
```
xsd:boolean  EXISTS { pattern }
```
```
xsd:boolean  NOT EXISTS { pattern }
```
```EXISTS``` returns true/false depending on whether the pattern matches the dataset given the bindings in the current group graph pattern, the dataset and the active graph at this point in the query evaluation. No additional binding of variables occurs. The ```NOT EXISTS``` form translates into ```fn:not(EXISTS{...}```).
 
#### OR
```
xsd:boolean  xsd:boolean left || xsd:boolean right
```
Returns a logical ```OR ```of left and right. It operates on the effective boolean value of its arguments.

#### AND
```
xsd:boolean  xsd:boolean left && xsd:boolean right
```
Returns a logical ```AND``` of left and right. It operates on the effective boolean value of its arguments.

#### EQUAL
```
xsd:boolean  RDF term term1 = RDF term term2
```
Returns ```TRUE``` if ```term1``` and ```term2``` are the same RDF term, produces a type error if the arguments are both literal but are not the same RDF term; returns ```FALSE``` otherwise. term1 and term2 are the same if any of the following is true:

*   term1 and term2 are equivalent IRIs.
*   term1 and term2 are equivalent literals.
*   term1 and term2 are the same blank node.

Invoking RDFterm-equal on two typed literals tests for equivalent values. An extended implementation may have support for additional datatypes. An implementation processing a query that tests for equivalence on unsupported datatypes (and non-identical lexical form and datatype IRI) returns an error, indicating that it was unable to determine whether or not the values are equivalent. For example, an unextended implementation will produce an error when testing either ```"iiii"^^my:romanNumeral = "iv"^^my:romanNumeral``` or ```"iiii"^^my:romanNumeral != "iv"^^my:romanNumeral```.

#### SAME
```
xsd:boolean  sameTerm (RDF term term1, RDF term term2)
```
Returns ```TRUE``` if ```term1``` and ```term2``` are the same RDF term, returns ```FALSE``` otherwise. Unlike RDFterm-equal, sameTerm can be used to test for non-equivalent typed literals with unsupported datatypes.

#### IN
```
boolean  rdfTerm IN (expression, …)
```
The ```IN``` operator tests whether the RDF term on the left-hand side is found in the values of list of expressions on the right-hand side. The test is done with ```=``` operator. A list of zero terms on the right-hand side is legal. Errors in comparisons cause the ```IN expression to raise an error if the RDF term being tested is not found elsewhere in the list of terms.

<table>
  <tr>
   <td> 2 IN (1, 2, 3)
   </td>
   <td>TRUE
   </td>
  </tr>
  <tr>
   <td>2 IN ()
   </td>
   <td>FALSE
   </td>
  </tr>
  <tr>
   <td>2 IN (<http://example/iri>, "str", 2.0)
   </td>
   <td>TRUE
   </td>
  </tr>
  <tr>
   <td>2 IN (1/0, 2)
   </td>
   <td>TRUE
   </td>
  </tr>
  <tr>
   <td>2 IN (2, 1/0)
   </td>
   <td>TRUE
   </td>
  </tr>
</table>
        
#### NOT IN
```
boolean  rdfTerm NOT IN (expression, …)
```
The ```NOT IN``` operator tests whether the RDF term on the left-hand side is not found in the values of list of expressions on the right-hand side. The test is done with ```!=```. A list of zero terms on the right-hand side is legal. Errors in comparisons cause the ```NOT IN``` expression to raise an error if the RDF term being tested is not found to be in the list elsewhere in the list of terms.

### Functions on RDF Terms
#### isIRI
```
xsd:boolean isIRI(RDFterm term)
```
```
xsd:boolean isURI(RDFterm term)
```
Returns ```TRUE``` if ```term``` is an IRI. Returns false otherwise. ```isURI``` is an alternate spelling for the``` isIRI``` operator.

#### isBlank
```
xsd:boolean isBlank(RDFterm term)
```
Returns ```TRUE``` if ```term``` is a blank node. Returns false otherwise.

#### isLiteral
```
xsd:boolean isLiteral(RDFterm term)
```
Returns ```TRUE``` if ```term``` is a literal. Returns false otherwise.

### isNumeric
```
xsd:boolean isNumeric(RDFterm term)
```
Returns ```TRUE``` if ```term``` is a numeric value. Returns false otherwise. ```term``` is numeric if it has an appropriate datatype and has a valid lexical form, making it a valid argument to functions and operators taking numeric arguments. 

<table>
  <tr>
   <td>isNumeric(12)
   </td>
   <td>TRUE
   </td>
  </tr>
  <tr>
   <td>isNumeric("12")
   </td>
   <td>FALSE
   </td>
  </tr>
  <tr>
   <td>isNumeric("12"^^xsd:nonNegativeInteger)
   </td>
   <td>TRUE
   </td>
  </tr>
  <tr>
   <td>isNumeric("1200"^^xsd:byte)
   </td>
   <td>FALSE
   </td>
  </tr>
  <tr>
   <td>isNumeric(<http://example/>)
   </td>
   <td>FALSE
   </td>
  </tr>
</table>

#### str
```
simple literal STR(literal ltrl)
```
```
simple literal STR(IRI rsrc)
```
Returns the lexical form of ```ltrl``` (a literal); returns the codepoint representation of rsrc (an IRI). This is useful for examining parts of an IRI, for instance, the host-name.

#### lang
```
simple literal LANG(literal ltrl)
```
Returns the language tag of ```ltrl```, if it has one. It returns ```""``` if ```ltrl``` has no language tag. Note that the RDF data model does not include literals with an empty language tag.

#### datatype
```
iri DATATYPE(literal literal)
```
Returns the datatype IRI of a literal. 

*   If the literal is a typed literal, return the datatype IRI.
*   If the literal is a simple literal, return ```xsd:string```.
*   If the literal is literal with a language tag, return ```rdf:langString```.

In SPARQL 1.0, the ```DATATYPE``` function was not defined for literals with a language tag. Therefore, an unextended implementation would raise an error when ```DATATYPE``` was called with a literal with a language tag. Operator extensibility allows implementations to return a result rather than raise an error. SPARQL 1.1 defines the result of ```DATATYPE``` applied to a literal with a language tag to be ```rdf:langString```. 
 
#### IRI
```
iri IRI(simple literal)
```
```
iri IRI(xsd:string)
```
```
iri IRI(iri)
```
```
iri URI(simple literal)
```
```
iri URI(xsd:string)
```
```
iri URI(iri)
```
The IRI function constructs an IRI by resolving the string argument. The IRI is resolved against the base IRI of the query and must result in an absolute IRI. The URI function is a synonym for IRI. If the function is passed an IRI, it returns the IRI unchanged. Passing any RDF term other than a simple literal, ```xsd:string``` or an IRI is an error.

#### BNODE
```
blank node BNODE()
```
```
blank node BNODE(simple literal)
```
```
blank node BNODE(xsd:string)
```
The ```BNODE``` function constructs a blank node that is distinct from all blank nodes in the dataset being queried and distinct from all blank nodes created by calls to this constructor for other query solutions. If the no argument form is used, every call results in a distinct blank node. If the form with a simple literal is used, every call results in distinct blank nodes for different simple literals, and the same blank node for calls with the same simple literal within expressions for one solution mapping. This functionality is compatible with the treatment of blank nodes in SPARQL ```CONSTRUCT``` templates.

#### STRDT
```
literal  STRDT(simple literal lexicalForm, IRI datatypeIRI)
```
The ```STRDT``` function constructs a literal with lexical form and type as specified by the arguments.

#### STRLANG
```
literal STRLANG(simple literal lexicalForm, simple literal langTag)
```
The ```STRLANG``` function constructs a literal with lexical form and language tag as specified by the arguments.

#### UUID
```
iri  UUID()
```
Return a fresh IRI from the ```UUID URN``` scheme. Each call of ```UUID()``` returns a different ```UUID```. It must not be the ```"nil" UUID``` (all zeroes). The variant and version of the UUID is implementation dependent.

#### STRUUID
```
simple literal STRUUID()
```
Return a string that is the scheme specific part of UUID. That is, as a simple literal, the result of generating a UUID, converting to a simple literal and removing the initial ```urn:uuid:```. 

### Functions on strings
Certain functions  take a string literal as an argument and accept (1) a simple literal, (2) a plain literal with language tag, or (3) a literal with datatype ```xsd:string```. They then act on the lexical form of the literal. The term string literal is used in the function descriptions for this. Use of any other RDF term will cause a call to the function to raise an error.

Some functions take two arguments. These arguments must be compatible otherwise invocation of one of these functions raises an error. Compatibility of two arguments is defined as: 

*   The arguments are simple literals or literals typed as ```xsd:string```.
*   The arguments are plain literals with identical language tags.
*   The first argument is a plain literal with language tag and the second argument is a simple literal or literal typed as ```xsd:string```.

Almost all the functions that return a string literal do so with the string literal of the same kind as the first argument (simple literal, plain literal with same language tag, xsd:string). However the function ```CONCAT``` returns a string literal based on the details of all its arguments.

#### STRLEN
```
xsd:integer STRLEN(string literal str)
```
It corresponds to the XPath ```fn:string-length``` function and returns an ```xsd:integer``` equal to the length in characters of the lexical form of the literal. 

#### SUBSTR
```
string literal SUBSTR(string literal source, xsd:integer startingLoc)
```
```
string literal SUBSTR(string literal source, xsd:integer startingLoc, xsd:integer length)
```
It corresponds to the XPath ```fn:substring``` function and returns a literal of the same kind (simple literal, literal with language tag, xsd:string typed literal) as the source input parameter but with a lexical form formed from the substring of the lexcial form of the source.

The arguments startingLoc and length may be derived types of ```xsd:integer```. The index of the first character in a strings is 1.

#### UCASE
```
string literal UCASE(string literal str)
```
It corresponds to the XPath ```fn:upper-case``` function. It returns a string literal whose lexical form is the upper case of the lexical form of the argument.

#### LCASE
```
string literal LCASE(string literal str)
```
It corresponds to the XPath ```fn:lower-case``` function. It returns a string literal whose lexical form is the lower case of the lexical form of the argument.

#### STRSTARTS
```
xsd:boolean STRSTARTS(string literal arg1, string literal arg2)
```
The ```STRSTARTS``` function corresponds to the XPath ```fn:starts-with``` function. It returns true if the lexical form of arg1 starts with the lexical form of arg2, otherwise it returns false.

#### STRENDS
```
xsd:boolean STRENDS(string literal arg1, string literal arg2)
```
The ```STRENDS``` function corresponds to the XPath ```fn:ends-with``` function. It returns true if the lexical form of arg1 ends with the lexical form of arg2, otherwise it returns false.
 
#### CONTAINS
```
xsd:boolean CONTAINS(string literal arg1, string literal arg2)
```
The ```CONTAINS``` function corresponds to the XPath ```fn:contains```. It returns ```TRUE``` if the lexical form of ```arg1``` contains the lexical forms of ```arg2```, otherwise it returns ```FALSE```.
        
#### STRBEFORE
```
literal  STRBEFORE(string literal arg1, string literal arg2)
```
The ```STRBEFORE``` function corresponds to the XPath fn:substring-before function. For compatible arguments, if the lexical part of the second argument occurs as a substring of the lexical part of the first argument, the function returns a literal of the same kind as the first argument ```arg1```. The lexical form of the result is the substring of the lexical form of ```arg1``` that precedes the first occurrence of the lexical form of ```arg2```. If the lexical form of ```arg2``` is the empty string, this is considered to be a match and the lexical form of the result is the empty string.  If there is no such occurrence, an empty simple literal is returned. 

#### STRAFTER
```
literal  STRAFTER(string literal arg1, string literal arg2)
```
The ```STRAFTER``` function corresponds to the XPath fn:substring-after function.  For compatible arguments, if the lexical part of the second argument occurs as a substring of the lexical part of the first argument, the function returns a literal of the same kind as the first argument ```arg1```. The lexical form of the result is the substring of the lexical form of ```arg1``` that follows the first occurrence of the lexical form of ```arg2```. If the lexical form of ```arg2``` is the empty string, this is considered to be a match and the lexical form of the result is the lexical form of ```arg1```. If there is no such occurrence, an empty simple literal is returned. 

#### ENCODE_FOR_URI
```
simple literal  ENCODE_FOR_URI(string literal ltrl)
```
The ```ENCODE_FOR_URI``` function corresponds to the XPath ```fn:encode-for-uri``` function. It returns a simple literal with the lexical form obtained from the lexical form of its input after translating reserved characters according to the ```fn:encode-for-uri``` function.

#### CONCAT
```	
string literal  CONCAT(string literal ltrl<sub>1</sub> ... string literal ltrl<sub>n</sub>)
```
The ```CONCAT``` function corresponds to the XPath ```fn:concat``` function. The function accepts string literals as arguments. The lexical form of the returned literal is obtained by concatenating the lexical forms of its inputs. If all input literals are typed literals of type ```xsd:string```, then the returned literal is also of type ```xsd:string```, if all input literals are plain literals with identical language tag, then the returned literal is a plain literal with the same language tag, in all other cases, the returned literal is a simple literal.

#### langMatches
``` 	
xsd:boolean  langMatches (simple literal language-tag, simple literal 			language-range)
```
Returns true if language-tag (first argument) matches language-range (second argument). language-range is a basic language range. A language-range of "*" matches any non-empty language-tag string. Example:
```
PREFIX dc: &lt;http://purl.org/dc/elements/1.1/>
SELECT ?title
WHERE { ?x  dc:title  "That Seventies Show"@en ;
            dc:title  ?title .
FILTER langMatches( lang(?title), "FR" ) }
```
The idiom ```langMatches( lang( ?v ), "*" )``` will not match literals without a language tag as ```lang( ?v )``` will return an empty string.

#### REGEX
```
xsd:boolean  REGEX (string literal text, simple literal pattern)
```
```
xsd:boolean  REGEX (string literal text, simple literal pattern, simple literal flags)
```
Invokes the XPath ```fn:matches``` function to match text against a regular expression pattern. The regular expression language is defined in [XQuery 1.0 and XPath 2.0 Functions and Operators section](http://www.w3.org/TR/xpath-functions/#regex-syntax).

#### REPLACE
```
string literal  REPLACE (string literal arg, simple literal pattern, simple literal replacement )
```
```
string literal  REPLACE (string literal arg, simple literal pattern, simple literal replacement,  simple literal flags)
```
The ```REPLACE``` function corresponds to the XPath fn:replace function. It replaces each non-overlapping occurrence of the regular expression pattern with the replacement string. Regular expession matching may involve modifier flags.

### Functions on numerics 
#### ABS
```
numeric  ABS (numeric term)
```
Returns the absolute value of ```arg```. An error is raised if ```arg``` is not a numeric value.

#### ROUND
```
numeric  ROUND (numeric term)
```
Returns the number with no fractional part that is closest to the argument. If there are two such numbers, then the one that is closest to positive infinity is returned. An error is raised if arg is not a numeric value.

#### CEIL
```
numeric  CEIL (numeric term)
```
Returns the smallest (closest to negative infinity) number with no fractional part that is not less than the value of arg. An error is raised if arg is not a numeric value.

#### FLOOR
``` 	
numeric  FLOOR (numeric term)
```
Returns the largest (closest to positive infinity) number with no fractional part that is not greater than the value of arg. An error is raised if arg is not a numeric value.
 
#### RAND
```
xsd:double  RAND ( )
```
Returns a pseudo-random number between 0 (inclusive) and 1.0e0 (exclusive). Different numbers can be produced every time this function is invoked. Numbers should be produced with approximately equal probability.
 
### Functions on dates and times

#### NOW
```
xsd:dateTime NOW ()
```
Returns an ```XSD dateTime``` value for the current query execution. All calls to this function in any one query execution must return the same value. The exact moment returned is not specified.

#### YEAR
``` 	
xsd:integer YEAR (xsd:dateTime arg)
```
Returns the year part of arg as an integer.

#### MONTH
```
xsd:integer MONTH (xsd:dateTime arg)
```
Returns the month part of arg as an integer.

#### DAY
```
xsd:integer DAY (xsd:dateTime arg)
```
Returns the day part of arg as an integer.

#### HOURS
```
xsd:integer HOURS (xsd:dateTime arg)
```
Returns the hours part of arg as an integer. The value is as given in the lexical form of the ```XSD dateTime```.

#### MINUTES
```
xsd:integer MINUTES (xsd:dateTime arg)
```
Returns the minutes part of arg as an integer. The value is as given in the lexical form of the ```XSD dateTime```.
 
#### SECONDS
```
xsd:integer SECONDS (xsd:dateTime arg)
```
Returns the seconds part of arg as an integer. The value is as given in the lexical form of the ```XSD dateTime```.
 
#### TIMEZONE
```
xsd:dayTimeDuration TIMEZONE (xsd:dateTime arg)
```
Returns the timezone part of arg as an ```xsd:dayTimeDuration```. Raises an error if there is no timezone.

#### TZ
```
simple literal TZ (xsd:dateTime arg)
```
Returns the timezone part of arg as a simple literal. Returns the empty string if there is no timezone.

### Hash functions
#### MD5
```
simple literal MD5 (simple literal arg)
```
```
simple literal MD5 (xsd:string arg)
```
Returns the MD5 checksum, as a hex digit string, calculated on the UTF-8 representation of the simple literal or lexical form of the xsd:string. Hex digits SHOULD be in lower case.

#### SHA1
```
simple literal SHA1 (simple literal arg)
```
```
simple literal SHA1 (xsd:string arg)
```
Returns the SHA1 checksum, as a hex digit string, calculated on the UTF-8 representation of the simple literal or lexical form of the ```xsd:string```. Hex digits SHOULD be in lower case.

#### SHA256
```
simple literal SHA256 (simple literal arg)
```
```
simple literal SHA256 (xsd:string arg)
```
Returns the SHA256 checksum, as a hex digit string, calculated on the UTF-8 representation of the simple literal or lexical form of the ```xsd:string```. Hex digits SHOULD be in lower case.

#### SHA384
```
simple literal SHA384 (simple literal arg)
```
```
simple literal SHA384 (xsd:string arg)
```
Returns the SHA384 checksum, as a hex digit string, calculated on the UTF-8 representation of the simple literal or lexical form of the ```xsd:string```. Hex digits SHOULD be in lower case.

#### SHA512
```
simple literal SHA512 (simple literal arg)
```
```
simple literal SHA512 (xsd:string arg)
```
Returns the SHA512 checksum, as a hex digit string, calculated on the UTF-8 representation of the simple literal or lexical form of the ```xsd:string```. Hex digits SHOULD be in lower case.

### Optional values
Basic graph patterns allow applications to make queries where the entire query pattern must match for there to be a solution. For every solution of a query containing only group graph patterns with at least one basic graph pattern, every variable is bound to an RDF Term in a solution. However, regular, complete structures cannot be assumed in all RDF graphs. It is useful to be able to have queries that allow information to be added to the solution where the information is available, but do not reject the solution because some part of the query pattern does not match. Optional matching provides this facility: if the optional part does not match, it creates no bindings but does not eliminate the solution. 

Syntax: 
```pattern OPTIONAL { pattern }```
```{ OPTIONAL { pattern } }``` and ```{ { } OPTIONAL { pattern } }``` are equivalent.

The ```OPTIONAL``` keyword is left-associative: ```pattern OPTIONAL { pattern } OPTIONAL { pattern }``` is the same as ```{ pattern OPTIONAL { pattern } } OPTIONAL { pattern }```.

Example:
```
PREFIX foaf:<http://xmlns.com/foaf/0.1/>
SELECT ?name ?mbox
WHERE   {   ?x foaf:name  ?name .
            OPTIONAL { ?x  foaf:mbox  ?mbox }
        }
```

Constraints can be given in an optional graph pattern. Example:
```
PREFIX  dc:<http://purl.org/dc/elements/1.1/>
PREFIX  ns:<http://example.org/ns#>
SELECT  ?title ?price
WHERE   { ?x dc:title ?title .
          OPTIONAL { ?x ns:price ?price . 
          FILTER (?price < 30) }
        }
```

## Matching alternatives
SPARQL provides a means of combining graph patterns so that one of several alternative graph patterns may match. If more than one of the alternatives matches, all the possible pattern solutions are found. Pattern alternatives are syntactically specified with the ```UNION``` keyword. The ```UNION``` pattern combines graph patterns; each alternative possibility can contain more than one triple pattern. Examples:
```
PREFIX dc10:<http://purl.org/dc/elements/1.0/>
PREFIX dc11:<http://purl.org/dc/elements/1.1/>
SELECT ?title
	WHERE  { 
    { ?book dc10:title  ?title } UNION { ?book dc11:title  ?title}
    }
```
To determine exactly how the information was recorded, a query could use different variables for the two alternatives:
```
PREFIX dc10:<http://purl.org/dc/elements/1.0/>
PREFIX dc11:<http://purl.org/dc/elements/1.1/>
SELECT ?x ?y
WHERE   { { ?book dc10:title ?x } UNION { ?book dc11:title  ?y } 
        }
```

## Negation
The SPARQL query language incorporates two styles of negation, one based on filtering results depending on whether a graph pattern does or does not match in the context of the query solution being filtered, and one based on removing solutions related to another pattern.
 
### Testing for the absence of a pattern
The ```NOT EXISTS``` filter expression tests whether a graph pattern does not match the dataset, given the values of variables in the group graph pattern in which the filter occurs. It does not generate any additional bindings. Example:
```
PREFIX  rdf:<http://www.w3.org/1999/02/22-rdf-syntax-ns#> 
PREFIX  foaf:<http://xmlns.com/foaf/0.1/> 
SELECT ?person
WHERE   { ?person rdf:type  foaf:Person .
        FILTER NOT EXISTS { ?person foaf:name ?name }
        } 
``` 

### Testing for the presence of a pattern
The filter expression ```EXISTS``` is also provided. It tests whether the pattern can be found in the data; it does not generate any additional bindings. Example:
```
PREFIX  rdf:<http://www.w3.org/1999/02/22-rdf-syntax-ns#> 
PREFIX  foaf:<http://xmlns.com/foaf/0.1/> 
SELECT ?person
WHERE 
	{ ?person rdf:type  foaf:Person .
	    FILTER EXISTS { ?person foaf:name ?name }
	}
```

### Removing possible solutions
The other style of negation provided in SPARQL is ```MINUS``` which evaluates both its arguments, then calculates solutions in the left-hand side that are not compatible with the solutions on the right-hand side.
```
PREFIX :<http://example/>
PREFIX foaf:<http://xmlns.com/foaf/0.1/>
SELECT DISTINCT ?s
WHERE {
	   ?s ?p ?o .
	   MINUS {
	      ?s foaf:givenName "Bob" .
	   }
}
```
 
### Differences between NOT EXISTS and MINUS
```NOT EXISTS``` and ```MINUS``` represent two ways of thinking about negation, one based on testing whether a pattern exists in the data, given the bindings already determined by the query pattern, and one based on removing matches based on the evaluation of two patterns. In some cases they can produce different answers.

Using the following data:
```
@prefix :<http://example/> .
:a :b :c .
```
Example about variables sharing:
```
SELECT *
	{ ?s ?p ?o
	  FILTER NOT EXISTS { ?x ?y ?z }
	}
```
evaluates to a result set with no solutions because ```{ ?x ?y ?z }``` matches given any ```?s ?p ?o```, so ```NOT EXISTS { ?x ?y ?z }``` eliminates any solutions. Whereas with ```MINUS, there is no shared variable between the first part ```(?s ?p ?o)``` and the second ```(?x ?y ?z)``` so no bindings are eliminated.
```
SELECT *
	{ ?s ?p ?o    
    MINUS { ?x ?y ?z }
	}
```
It is important to keep in mind this differences in scope between ```FILTER``` and ```MINUS```, especially when both expressions are combined (See [Inner Filter](https://www.w3.org/TR/2013/REC-sparql11-query-20130321/#idp899488))

Another case is where there is a concrete pattern (no variables) in the example:
```
PREFIX :<http://example/>
SELECT * 
	{ ?s ?p ?o 
	  FILTER NOT EXISTS { :a :b :c }
	}
```
evaluates to a result set with no query solutions. Whereas,
```
PREFIX :<http://example/>
SELECT * 
	{ ?s ?p ?o 
	  MINUS { :a :b :c }
	}
```
evaluates to result set with one query solution, because there is no match of bindings and so no solutions are eliminated.
 
## Property Paths
A property path is a possible route through a graph between two graph nodes. A trivial case is a property path of length exactly 1, which is a triple pattern. The ends of the path may be RDF terms or variables. Variables can not be used as part of the path itself, only the ends. 

Property paths allow for more concise expressions for some SPARQL basic graph patterns and they also add the ability to match connectivity of two resources by an arbitrary length path. 

Connectivity between the subject and object by a property path of arbitrary length can be found using the "zero or more" property path operator, ```*```, and the "one or more" property path operator, ```+```. There is also a "zero or one" connectivity property path operator, ```?```. 

Each of these operators uses the property path expression to try to find a connection between subject and object, using the path step a number of times, as restricted by the operator. Such connectivity matching does not introduce duplicates (it does not incorporate any count of the number of ways the connection can be made) even if the repeated path itself would otherwise result in duplicates. The graph matched may include cycles. Connectivity matching is defined so that matching cycles does not lead to undefined or infinite results. 

### Syntax
In the table below, ```iri``` is either an IRI or the keyword ```a```. ```elt``` is a path element, which may itself be composed of path constructs. 

![](https://i.imgur.com/lJjhLU6.png)


The order of IRIs, and reverse IRIs, in a negated property set is not significant and they can occur in a mixed order. Precedence is left-to-right within groups. The precedence of the syntax forms is, from highest to lowest: 
*   IRI, prefixed names
*   Negated property sets
*   Groups
*   Unary operators ```*```, ```?``` and ```+```
*   Unary ```^``` inverse links
*   Binary operator ```/```
*   Binary operator ```|```

## Assignment
The value of an expression can be added to a solution mapping by binding a new variable to the value of the expression, which is an RDF term. The variable can then be used in the query and also can be returned in results.

Three syntax forms allow this: the ```BIND``` keyword, expressions in the ```SELECT``` clause and expressions in the ```GROUP BY``` clause. The assignment form is ```(expression AS ?var)```. 

If the evaluation of the expression produces an error, the variable remains unbound for that solution but the query evaluation continues. Data can also be directly included in a query using ```VALUES``` for inline data.
 
### BIND. Assigning values to variables
The ```BIND``` form allows a value to be assigned to a variable from a basic graph pattern or property path expression. Use of ```BIND``` ends the preceding basic graph pattern. The variable introduced by the ```BIND``` clause must not have been used in the group graph pattern up to the point of use in ```BIND```.

Example:
```
PREFIX  dc:<http://purl.org/dc/elements/1.1/>
PREFIX  ns:<http://example.org/ns#>
SELECT  ?title ?price
	{  ?x ns:price ?p .
	   ?x ns:discount ?discount
	   BIND (?p*(1-?discount) AS ?price)
	   FILTER(?price < 20)
	   ?x dc:title ?title . 
	}
```
 
### VALUES. Providing inline data
Data can be directly written in a graph pattern or added to a query using ```VALUES```. ```VALUES``` provides inline data as a solution sequence which are combined with the results of query evaluation by a join operation. It can be used by an application to provide specific requirements on query results and also by SPARQL query engine implementations that provide federated query through the ```SERVICE``` keyword to send a more constrained query to a remote query service. 

In the following example, there is a table of two variables, ```?x``` and ```?y```. The second row has no value for ```?y```.
```
VALUES (?x ?y) {(:uri1 1)(:uri2 UNDEF)}
```
And a single variable and some values: ```VALUES ?z { "abc" "def" }```, which is the same as using the general form: ```VALUES (?z) { ("abc") ("def") }```.

Example:
```
PREFIX dc:<http://purl.org/dc/elements/1.1/> 
PREFIX :<http://example.org/book/> 
PREFIX ns:<http://example.org/ns#> 
SELECT ?book ?title ?price
	{
		?book dc:title ?title ;
		ns:price ?price .
		VALUES (?book ?title)
   		{ 	(UNDEF "SPARQL Tutorial")
     		(:book2 UNDEF)
   		}
	}
```
Which is the same as:
```
PREFIX dc:<http://purl.org/dc/elements/1.1/> 
PREFIX :<http://example.org/book/> 
PREFIX ns:<http://example.org/ns#> 
SELECT ?book ?title ?price
	{
	   ?book dc:title ?title ;
		ns:price ?price .
	}
	VALUES (?book ?title)
	{	(UNDEF "SPARQL Tutorial")
  		(:book2 UNDEF)
	}
```
## Aggregates
Aggregates apply expressions over groups of solutions. By default a solution set consists of a single group, containing all solutions. Grouping may be specified using the ```GROUP BY``` syntax. Aggregates defined in version 1.1 of SPARQL are ```COUNT```, ```SUM```, ```MIN```, ```MAX```, ```AVG```, ```GROUP_CONCAT```, and ```SAMPLE```. Aggregates are used where the querier wishes to see a result which is computed over a group of solutions, rather than a single solution. For example the maximum value that a particular variable takes, rather than each value individually.

It should be noted that as per functions, aggregate expressions are required to be aliased (again, similar to the ```BIND``` clause, using the keyword ```AS```) in order to project them from queries or subqueries. In the example above this is done using the variable ```?totalPrice```. It is an error for aggregates to project variables with a name already used in other aggregate projections, or in the ```WHERE``` clause.

### GROUP BY
In order to calculate aggregate values for a solution, the solution is first divided into one or more groups, and the aggregate value is calculated for each group.

If aggregates are used in the query level in ```SELECT```, ```HAVING``` or ```ORDER BY``` but the ```GROUP BY``` term is not used, then this is taken to be a single implicit group, to which all solutions belong.

Within ```GROUP BY``` clauses the binding keyword, ```AS```, may be used, such as ```GROUP BY (?x + ?y AS ?z)```. This is equivalent to ```{ ... BIND (?x + ?y AS ?z) } GROUP BY ?z```.

For example, given a solution sequence ```S```, ```( {?x→2, ?y→3}, {?x→2, ?y→5}, {?x→6, ?y→7} )```, we might wish to group the solutions according to the value of ```?x```, and calculate the average of the values of ```?y``` for each group.

### HAVING
```HAVING``` operates over grouped solution sets, in the same way that ```FILTER``` operates over un-grouped ones. ```HAVING``` expressions have the same evaluation rules as projections from grouped queries, as described in the following section. An example that returns average sizes, grouped by the subject, but only where the mean size is greater than 10.
```
PREFIX :<http://data.example/>
SELECT (AVG(?size) AS ?asize)
WHERE {
	  ?x :size ?size
	}
	GROUP BY ?x
	HAVING(AVG(?size) > 10)
```

## Querying RDF datasets
Many RDF data stores hold multiple RDF graphs and record information about each graph, allowing an application to make queries that involve information from more than one graph.

A SPARQL query is executed against an _RDF Dataset_ which represents a collection of graphs. An RDF Dataset comprises one graph, the default graph, which does not have a name, and zero or more named graphs, where each named graph is identified by an IRI. A SPARQL query can match different parts of the query pattern against different graphs.

An RDF Dataset may contain zero named graphs; an RDF Dataset always contains one default graph. A query does not need to involve matching the default graph; the query can just involve matching named graphs.

The definition of RDF Dataset does not restrict the relationships of named and default graphs. Information can be repeated in different graphs; relationships between graphs can be exposed. Two useful arrangements are:
*   to have information in the default graph that includes provenance information about the named graphs
*   to include the information in the named graphs in the default graph as well.

### FROM and FROM NAMED
A SPARQL query may specify the dataset to be used for matching by using the ```FROM``` clause and the ```FROM NAMED``` clause to describe the RDF dataset. If a query provides such a dataset description, then it is used in place of any dataset that the query service would use if no dataset description is provided in a query. The RDF dataset may also be specified in a SPARQL protocol request, in which case the protocol description overrides any description in the query itself. A query service may refuse a query request if the dataset description is not acceptable to the service.

The ```FROM``` and ```FROM NAMED``` keywords allow a query to specify an RDF dataset by reference; they indicate that the dataset should include graphs that are obtained from representations of the resources identified by the given IRIs (i.e. the absolute form of the given IRI references). The dataset resulting from a number of ```FROM``` and ```FROM NAMED``` clauses is:
*   a default graph consisting of the RDF merge of the graphs referred to in the ```FROM``` clauses, and
*   a set of ```(IRI, graph)``` pairs, one from each ```FROM NAMED``` clause.
If there is no ```FROM``` clause, but there is one or more ```FROM NAMED``` clauses, then the dataset includes an empty graph for the default graph.

#### Specifying the default graph
Each ```FROM``` clause contains an IRI that indicates a graph to be used to form the default graph. This does not put the graph in as a named graph. If a query provides more than one FROM clause, providing more than one IRI to indicate the default graph, then the default graph is the RDF merge of the graphs obtained from representations of the resources identified by the given IRIs.

#### Specifying named graphs
A query can supply IRIs for the named graphs in the RDF Dataset using the ```FROM NAMED``` clause. Each IRI is used to provide one named graph in the RDF Dataset. Using the same IRI in two or more ```FROM NAMED``` clauses results in one named graph with that IRI appearing in the dataset.

### FROM, FROM NAMED and GRAPH
The ```FROM``` clause and ```FROM NAMED``` clause can be used in the same query. The RDF Dataset for the example query contains a default graph and two named graphs. 

**Default graph**
```
@prefix dc:<http://purl.org/dc/elements/1.1/> .
<http://example.org/bob> dc:publisher "Bob Hacker" .
<http://example.org/alice> dc:publisher "Alice Hacker" .
```

**Named graph: http://example.org/bob**
```
@prefix foaf:<http://xmlns.com/foaf/0.1/> .
_:a foaf:name "Bob" .
_:a foaf:mbox <mailto:bob@oldcorp.example.org> .
```

**Named graph: http://example.org/alice**
```
@prefix foaf:<http://xmlns.com/foaf/0.1/> .
_:a foaf:name "Alice" .
_:a foaf:mbox <mailto:alice@work.example.org> .
```
```
PREFIX foaf:<http://xmlns.com/foaf/0.1/>
PREFIX dc:<;http://purl.org/dc/elements/1.1/>
SELECT ?who ?g ?mbox
WHERE
    { ?g dc:publisher ?who .
    GRAPH ?g { ?x foaf:mbox ?mbox }
    }
```
The actions required to construct the dataset are not determined by the dataset description alone. If an IRI is given twice in a dataset description, either by using two ```FROM``` clauses, or a ```FROM``` clause and a ```FROM NAMED``` clause, then it does not assume that exactly one or exactly two attempts are made to obtain an RDF graph associated with the IRI. Therefore, no assumptions can be made about blank node identity in triples obtained from the two occurrences in the dataset description. In general, no assumptions can be made about the equivalence of the graphs. 

When querying a collection of graphs, the ```GRAPH``` keyword is used to match patterns against named graphs. ```GRAPH``` can provide an IRI to select one graph or use a variable which will range over the IRI of all the named graphs in the query's RDF dataset.

The use of ```GRAPH``` changes the active graph for matching graph patterns within that part of the query. Outside the use of ```GRAPH```, matching is done using the default graph.
 
## Federated queries
The growing number of SPARQL query services offer data consumers an opportunity to merge data distributed across the Web. This specification defines the syntax and semantics of the ```SERVICE``` extension to the SPARQL 1.1 Query Language. This extension allows a query author to direct a portion of a query to a particular SPARQL endpoint. Results are returned to the federated query processor and are combined with results from the rest of the query. 

## Solution sequences and modifiers
Query patterns generate an unordered collection of solutions. These solutions are then treated as a sequence in no specific order; any sequence modifiers are then applied to create another sequence. Finally, this latter sequence is used to generate one of the results of a SPARQL query form (```SELECT```, ```CONSTRUCT```, ```ASK```, or ```DESCRIBE```).

A solution sequence modifier is one of the following list, and they are applied in the order given by the list above:
*   Order modifier (```ORDER```): put the solutions in order
*   Projection modifier (```SELECT```): choose certain variables
*   Distinct modifier (```DISTINCT```): ensure solutions in the sequence are unique
*   Reduced modifier(```REDUCED```): permit elimination of some non-distinct solutions
*   Offset modifier (```OFFSET```): control where the solutions start from in the overall sequence of solutions
*   Limit modifier (```LIMIT```): restrict the number of solutions

### ORDER
The ```ORDER BY``` clause establishes the order of a solution sequence. Following the ```ORDER BY``` clause is a sequence of order comparators, composed of an expression and an optional order modifier: ```ASC()``` or ```DESC()```.

SPARQL does not define a total ordering of all possible RDF terms. Here are a few examples of pairs of terms for which the relative order is undefined:

*   ```"a"``` and ```"a"@en_gb``` (a simple literal and a literal with a language tag)
*   ```"a"@en_gb``` and ```"b"@en_gb``` (two literals with language tags)
*   ```"a"``` and ```"1"^^xsd:integer``` (a simple literal and a literal with a supported datatype)
*   ```"1"^^my:integer``` and ```"2"^^my:integer``` (two unsupported datatypes)
*   ```"1"^^xsd:integer``` and ```"2"^^my:integer``` (a supported datatype and an unsupported datatype)

Using ```ORDER BY``` on a solution sequence for a ```CONSTRUCT``` or ```DESCRIBE``` query has no direct effect because only ```SELECT``` returns a sequence of results. Used in combination with ```LIMIT``` and ```OFFSET```, ```ORDER BY``` can be used to return results generated from a different slice of the solution sequence. An ```ASK``` query does not include ```ORDER BY```, ```LIMIT``` or ```OFFSET```. 

### SELECT
The solution sequence can be transformed into one involving only a subset of the variables. For each solution in the sequence, a new solution is formed using a specified selection of the variables using the ```SELECT``` query form.

### DISTINCT / REDUCED
A solution sequence with no ```DISTINCT``` or ```REDUCED``` query modifier will preserve duplicate solutions.

The ```DISTINCT``` solution modifier eliminates duplicate solutions. Only one solution solution that binds the same variables to the same RDF terms is returned from the query.

While the ```DISTINCT``` modifier ensures that duplicate solutions are eliminated from the solution set, ```REDUCED``` simply permits them to be eliminated. The cardinality of any set of variable bindings in a ```REDUCED``` solution set is at least one and not more than the cardinality of the solution set with no ```DISTINCT``` or ```REDUCED``` modifier.

### OFFSET
```OFFSET``` causes the solutions generated to start after the specified number of solutions. An ```OFFSET``` of zero has no effect.

### LIMIT
The ```LIMIT``` clause puts an upper bound on the number of solutions returned. If the number of actual solutions, after OFFSET is applied, is greater than the limit, then at most the limit number of solutions will be returned. A limit may not be negative.

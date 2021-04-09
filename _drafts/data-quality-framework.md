# Data Quality Framework

## WHAT? 
Stages
* Quality checking
* Actions
  * Flag issues on Knowledge Graph GUI (metaphactory)

## WHERE?
On the Knowledge Graph

## WHEN?
Two options: a) ad-hoc, and b) guard mode. Guard mode seems to be the most appropiate one.
From resource link number 2:

> When should you use guard mode versus ad hoc validation?
> If we’re building a Knowledge Graph over many systems, each of which owns its data entry interfaces, then we will run ICV asynchronously on our own auditing schedule.
>For applications built to store data directly in the graph, guard mode helps move complex business logic from application software to declarative data in the Knowledge Graph.

## Use cases
### NULL values on relational tables
Two possible semantics:
1. There is no attribute value. E.g.: a given molecule does not have a ChEMBL code assigned yet.
2. The attribute value is unknown, but it actually exists. E.g.: The data source does not know the ChEMBL for a given Therapeutic Target.

The "right" semantics can be only established by the domain experts. In consequence, to define a unified approach to tackle the NULL values cases is not an option.
Instead, a domain-dependent approach is requiered.

### Resources
1. https://www.stardog.com/platform/features/data-quality-shacl/
2. https://www.stardog.com/blog/data-quality-with-icv/

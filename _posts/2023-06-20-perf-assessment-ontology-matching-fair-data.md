---
layout: post
title: Takeaways on "Performance assessment of ontology matching systems for FAIR data" article
---
*Written by [Emiliano Reynares](https://www.linkedin.com/in/ereynrs/)*  

The [Performance assessment of ontology matching systems for FAIR data](https://doi.org/10.1186/s13326-022-00273-5) article depicts an experimental study assessing the performance of ontology matching systems in the context of a real-life application from the rare disease domain. It also presents a method for analyzing top-level classes to improve precision. 

The study is rooted in the assumption that multiple data sources can use different ontologies for annotating their data and, thus, creating the need for dynamic ontology matching services. 

The performance - assessed using reference alignments from UMLS and BioPortal- of three ontology matching systems was assessed:
* [AgreementMaker Light 2.0](https://ceur-ws.org/Vol-1272/paper_151.pdf).
* [FCA-Map](https://github.com/icgw/FCA-Map).
* [LogMap 2.0](https://www.nature.com/articles/npre.2011.6670.1).

The authors suggest trying machine learning-based approaches as further work and using the MELT/MELT-ML frameworks. More info on this in the following articles:
* [Biomedical ontology alignment: an approach based on representation learning](https://doi.org/10.1186/s13326-018-0187-8)
* [Supervised Ontology and Instance Matching with MELT](https://arxiv.org/abs/2009.11102).

They also mentioned two APIs that would be interesting to review:
* [OWL API](https://github.com/owlcs/owlapi).
* [Alignment API](https://www.w3.org/2001/sw/wiki/Alignment_API).

##### Visit my [LinkedIn Profile](https://www.linkedin.com/in/ereynrs/)

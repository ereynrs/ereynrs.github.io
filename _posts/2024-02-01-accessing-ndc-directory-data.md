---
layout: post
title: Accessing NDC Directory data
---
*Written by [Emiliano Reynares](https://www.linkedin.com/in/ereynrs/)*

FDA’s National Drug Code (NDC) Directory contains information about finished drug products, unfinished drugs and compounded drug products. _**The NDC Directory is updated daily**_ ([FDA NDC basics](https://www.fda.gov/drugs/drug-approvals-and-databases/national-drug-code-directory)).

FDA provides an Elasticsearch-based API that serves public FDA data about nouns like drugs, devices, and foods. ([openFDA basics](https://open.fda.gov/apis/)). There’s a specific endpoint to retrieve drug -related data from the NDC Directory ([openFDA drug endpoint](https://open.fda.gov/apis/drug/ndc/)).

openFDA is open source and its source code is available on GitHub, so it’s possible to create a private instance of openFDA ([openFDA GitHub](https://github.com/FDA/openfda/)).

It’s also possible to download the data for any openFDA endpoint in JSON format. Because of the size of some categories, the downloads are broken into several zipped JSON files. _**To keep updated the downloaded files it’s needed to download ALL available data files for the endpoint of interest.**_ The downloading process can be automated ([openFDA data download](https://open.fda.gov/apis/downloads/)).

##### Visit my [LinkedIn Profile](https://www.linkedin.com/in/ereynrs/)
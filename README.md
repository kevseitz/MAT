# MAT

Title: Prototype Semantics in Knowledge Graphs - A study of different Graph Centrality Measures on the example of DBpedia

Scope: Master's thesis

Chair: Data and Web Science Group

Date: Feb - July 2022

## Introduction

> Write introduction, link DBpedia etc.

The experiments carried out in this work are spread over different Jupyter notebooks. They are listed now and discussed in the next steps:
- Questionnaire_preprocessing
- Questionnaire_evaluation
- DBpedia_centrality_computation
- GCM_analysis
- GCM_linear_regression
- (GCM_ordinal_regression)


## Questionnaire_preprocessing

Preprocessing of the raw survey data to create a gold standard. Steps include:

- Splitting and cleaning the original survey answers (given as 3 comma-separated answers per question and participant)
- Translating answers where necessary for certain categories via the [DeepL API](https://www.deepl.com/pro-api). This was done using the [deep-translator](https://github.com/nidhaloff/deep-translator) package.
- Matching the DBpedia URI via [Lookup API](https://lookup.dbpedia.org/). GitHub repository for more information: [dbpedia-lookup](https://github.com/dbpedia/dbpedia-lookup). Additionally, to using the top result the [Levenshtein distance](https://pypi.org/project/python-Levenshtein/) of the survey answer and the results was used to identify the best hit.
  - Saving the results into distinct .csv files per survey question (Category in DBpedia). They can be found in [data/survey_single_cat_files/sv_prep_uri](data/survey_single_cat_files/sv_prep_uri)
  - To further improve the gold standard each URI was checked manually. This was done with great care in order to not alter the results. They can be found in [data/survey_single_cat_files/sv_prep_uri/improved_uri](data/survey_single_cat_files/sv_prep_uri/improved_uri)

Link to survey: https://forms.gle/TiY3p9WSkGV8mNKk6

## Questionnaire_evaluation

Analysing the survey results per category in the gold standard by:
- Counting, normalization and scaling of the unique survey results
- Test on normal distribution with scipy [normaltest](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.normaltest.html) and [KS-test](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kstest.html)
- Test on Kurtosis and Skewness
- Plotting the data

## DBpedia_centrality_computation

Takes DBpedia data and computes Graph Centrality Measures. In oder to recreate the data, the "Instance-types" and "Mappingbased-objects" listed below need to be downloaded first.

```
Instance types
Version: 2021.12.01/ 24-Jan-2022 00:21
Specific: 24-Jan-2022 00:20            44905249
Transitive: 24-Jan-2022 00:20           145896226

Mapping based objects
Version: 2021.12.01/ 21-Apr-2022 23:33
24-Jan-2022 00:34           184053203
```

- Both are accessable as Turtle files (.ttl) via: https://downloads.dbpedia.org/repo/dbpedia/mappings/
- Before executing, the files need to be merged into one, for instance by concatenating them in the shell like:
    > cat mappingbased-objects_lang=en.ttl instance-types_lang=en_specific.ttl > mappingbased-objects_instance-types.ttl

- For creating, handling, modifying and analysing the graph data in python the following packages are used:
  - [RDFLib](https://rdflib.dev)
  - [NetworkX](https://networkx.org)
  - [iGraph](https://igraph.org/python)
  
TBD: GCMs, max min node, nodes edges pendants, data: ig-gcm.csv

## GCM_analysis
TBD

## GCM_linear_regression
TBD

## (GCM_ordinal_regression)
TBD

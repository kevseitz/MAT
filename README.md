# MAT

**Title:** Prototype Semantics in Knowledge Graphs - A study of different Graph Centrality Measures on the example of DBpedia

**Scope:** Master's thesis

**Date:** Feb - July 2022

## Introduction

> This thesis presents the concepts of prototype semantics in open knowledge graphs with the idea of identifying former by studying different graph centrality measures (GCM) on the example of the DBpedia database. Prototype semantics are a special semantic derived from prototype theory, that combine insights from psychology as well as linguistics and describe the quantitative gradation of the belonging of entities to categories. According to prototype theory, each linguistic concept in a language has a prototypical representation defined by its features. A pipeline for processing the distinct data sources is developed. This includes the preprocessing and analysis of survey data into a gold standard dataset as well as the nourishment of the DBpedia category data with measures computed from different graph centrality algorithms. Algorithms include degree centrality, eigenvector centrality and PageRank centrality based on Google's algorithm for assessing the importance of a webpage. Challenging hereby, is the task of identifying records in the knowledge graph which refer to the same real-world object identified as a prototype by the gold standard.

The open knowledge graph [DBpedia](https://www.dbpedia.org) is one of the central data hubs in the [Linked Open Data Cloud](https://lod-cloud.net/dataset/dbpedia). It is a large-scale knowledge graph derived from the [Wikipedia encyclopedia](https://www.wikipedia.org) and sourced as a community project. 

The experiments carried out in this work are spread over different Jupyter notebooks. They are listed now and discussed in the next steps:
- [Questionnaire_preprocessing](https://github.com/kevseitz/MAT#questionnaire_preprocessing)
- [Questionnaire_evaluation](https://github.com/kevseitz/MAT#questionnaire_evaluation)
- [DBpedia_centrality_computation](https://github.com/kevseitz/MAT#dbpedia_centrality_computation)
- [GCM_analysis](https://github.com/kevseitz/MAT#gcm_analysis)
- [GCM_linear_regression](https://github.com/kevseitz/MAT#gcm_linear_regression)
- [(GCM_ordinal_regression)](https://github.com/kevseitz/MAT#gcm_ordinal_regression)


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

Plots for every category are in [plots/plots_survey](/plots/plots_survey).

## DBpedia_centrality_computation

Takes DBpedia data and computes Graph Centrality Measures. In oder to recreate the data, the "Instance-types" and "Mappingbased-objects" listed below need to be downloaded first. Both are accessable as Turtle files (.ttl) via: https://downloads.dbpedia.org/repo/dbpedia/mappings/.


```
Instance types
Version: 2021.12.01/ 24-Jan-2022 00:21
Specific: 24-Jan-2022 00:20            44905249
Transitive: 24-Jan-2022 00:20           145896226

Mapping based objects
Version: 2021.12.01/ 21-Apr-2022 23:33
24-Jan-2022 00:34           184053203
```

For creating, handling, modifying and analysing the graph data in python the following packages are used:
- [RDFLib](https://rdflib.dev)
- [NetworkX](https://networkx.org)
- [iGraph](https://igraph.org/python)

Before executing, the files need to be merged into one, for instance by concatenating them in the shell like:
```bash
cat mappingbased-objects_lang=en.ttl instance-types_lang=en_specific.ttl > mappingbased-objects_instance-types.ttl
```

Loading and transforming the data into a knowledge graph takes around 1,5h. The network has 8.243.333 nodes and 28.263.172 edges. The network density is 8,318504610951633e-07. GCMs computed are _Degree Centrality_, _Eigenvector Centrality_ and _PageRank Centrality_. The output is saved as .csv-files and can befound in [data/gcm_computed](data/gcm_computed). GCM files exist computed with NetworkX (nx-gcm.csv) and iGraph (ig-gcm.csv). In the next steps the iGraph data is used.

## GCM_analysis

This notebook is about basic analysis of the performance of the computed graph centrality measures. As the GCM computation was on the whole DBpedia data, in this step a division into the individual categories is made. The categories are queried with SPARQL and pulled from the graph based on the URI. The categories are congruent with those of the survey/gold standard.

The GCM results are analysed by checking on:
- Normal distribution
- Rank correlation between the different measures (Pearson and Spearman correlation)
- Precision at k
- ...

## GCM_linear_regression

In order to test the hypothesized correlation between the ranks of GCMs and the prototypicality of an entity in a category, linear regression was performed and verified against the gold standard. The [statsmodels](https://www.statsmodels.org/stable/index.html) library was used for this. The results can be found in the Jupyter notebook and in the thesis. Plots for every category are in [plots/plots_linearRegression](/plots/plots_linearRegression).

## (GCM_ordinal_regression)

Another experiment for classification of prototypes could be made with an ordinal regression model. Hereby, for the algorithms can be tested if they can predict the belonging to a predefined camp instead of the actual ranking of an entity. A camp for instance would be a split of a categories results into the top and low ranking 50\% or 33\%. The model can predict whether the entity is prototypical based on those camps and then verified with the gold standard. A Jupyter notebook has been implemented for this as well - but the crucial analysis has not been done yet.

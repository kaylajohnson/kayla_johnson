---
title: "Robust normalization and transformation techniques for constructing gene coexpression networks from RNA-seq data"
subtitle: ""
excerpt: "Research article published in _Genome Biology_"
date: 2022-01-03
author: "Kayla A Johnson, Arjun Krishnan"
draft: false
tags:
- research
categories:
- research
# layout options: single or single-sidebar
layout: single
links:
- icon: newspaper
  icon_pack: far
  name: article
  url: https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02568-9/
- icon: github
  icon_pack: fab
  name: code
  url: https://github.com/krishnanlab/RNAseq_coexpression/
- icon: door-open
  icon_pack: fas
  name: website
  url: https://krishnanlab.github.io/RNAseq_coexpression/
---

#### Check out the links to the article, code on GitHub, and Rmarkdown website I made for the battery of extra results above!

### Abstract
#### Background
Constructing gene coexpression networks is a powerful approach for analyzing high-throughput gene expression data towards module identification, gene function prediction, and disease-gene prioritization. While optimal workflows for constructing coexpression networks, including good choices for data pre-processing, normalization, and network transformation, have been developed for microarray-based expression data, such well-tested choices do not exist for RNA-seq data. Almost all studies that compare data processing and normalization methods for RNA-seq focus on the end goal of determining differential gene expression.

#### Results
Here, we present a comprehensive benchmarking and analysis of 36 different workflows, each with a unique set of normalization and network transformation methods, for constructing coexpression networks from RNA-seq datasets. We test these workflows on both large, homogenous datasets and small, heterogeneous datasets from various labs. We analyze the workflows in terms of aggregate performance, individual method choices, and the impact of multiple dataset experimental factors. Our results demonstrate that between-sample normalization has the biggest impact, with counts adjusted by size factors producing networks that most accurately recapitulate known tissue-naive and tissue-aware gene functional relationships.

#### Conclusions
Based on this work, we provide concrete recommendations on robust procedures for building an accurate coexpression network from an RNA-seq dataset. In addition, researchers can examine all the results in great detail at https://krishnanlab.github.io/RNAseq_coexpression to make appropriate choices for coexpression analysis based on the experimental factors of their RNA-seq dataset.





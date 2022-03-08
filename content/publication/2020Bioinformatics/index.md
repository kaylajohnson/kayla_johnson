---
title: "Supervised learning is an accurate method for network-based gene classification"
subtitle: ""
excerpt: "Research article published in _Bioinformatics_"
date: 2020-04-14
author: "Renming Liu, Christopher A Mancuso, Anna Yannakopoulos, Kayla A Johnson, Arjun Krishnan"
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
  url: https://academic.oup.com/bioinformatics/article/36/11/3457/5780279/
- icon: github
  icon_pack: fab
  name: code
  url: https://github.com/krishnanlab/GenePlexus/
---

#### Check out the links to the article and code above!

### Abstract
#### Background
Assigning every human gene to specific functions, diseases and traits is a grand challenge in modern genetics. Key to addressing this challenge are computational methods, such as supervised learning and label propagation, that can leverage molecular interaction networks to predict gene attributes. In spite of being a popular machine-learning technique across fields, supervised learning has been applied only in a few network-based studies for predicting pathway-, phenotype- or disease-associated genes. It is unknown how supervised learning broadly performs across different networks and diverse gene classification tasks, and how it compares to label propagation, the widely benchmarked canonical approach for this problem.

#### Results
In this study, we present a comprehensive benchmarking of supervised learning for network-based gene classification, evaluating this approach and a classic label propagation technique on hundreds of diverse prediction tasks and multiple networks using stringent evaluation schemes. We demonstrate that supervised learning on a gene’s full network connectivity outperforms label propagaton and achieves high prediction accuracy by efficiently capturing local network properties, rivaling label propagation’s appeal for naturally using network topology. We further show that supervised learning on the full network is also superior to learning on node embeddings (derived using node2vec), an increasingly popular approach for concisely representing network connectivity. These results show that supervised learning is an accurate approach for prioritizing genes associated with diverse functions, diseases and traits and should be considered a staple of network-based gene classification workflows.






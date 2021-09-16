---
permalink: /research/
title: "Research"
classes: wide
---

## Uncertainty in protein-protein interaction networks

Protein-protein interaction data is obtained through a range of experimental techniques, each of which is subject to experimental error. When we build and analyse protein interaction networks using such data, we often ignore the uncertainty associated with it. It is therefore not always clear whether the conclusions we draw from such analysis are biologically relevant, or whether they are artifacts of the data. I am generally interested in studying the robustness of network analysis pipelines to data uncertainty.

## Publication bias in protein-protein interaction networks

PPI networks are usually built using data collected from a range of primary sources, including both low- and high-throughput studies. This data is subject to centralised curation and standardisation efforts such as [BioGRID](https://thebiogrid.org/). Data curation generally aims to minimise false positives i.e. erroneous interaction records, but little is done to account for false negatives and missing data.

To date, nearly a quarter of PPI data comes from focussed, low-throughput studies. This means parts of the proteome that are of particular research interest, e.g. protein kinases, are overrepresented in interaction databases. Meanwhile, even in model organisms such as *S. cerevisiae* many protein pairs do not cooccur in a single study and so may have never been screened for interaction.

This publication bias has a structural effect on PPI networks, as proteins of known function and specific research interest will have denser neighbourhoods. I work on developing methods for assessing the effect of this bias on downstream network analysis.

## Robust construction of gene co-expression networks

Gene co-expression, or the highly correlated abundance of gene products in the cell, is indicative of shared genetic function. This is why we use gene co-expression networks in order to do predict gene function, or to identify genes of interest, e.g. in a disease. However, there is no single standard way of building such networks: data processing pipelines and in particular how co-expression values are calculated vary from study to study. I am interested in how we prioritise network construction methodologies, especially in cases where there is insufficient external validation data.

---
title:  "Preprint: A role for BCL2L13 and autophagy in germline purifying selection of mtDNA"
categories:
  - Blog
tags:
  - R
  - research
  - publication
  - mtDNA
---

[Laura Kremer](https://staff.ki.se/people/laura-kremer) from Nils-Göran Larsson's [group](https://ki.se/en/mbb/nils-goran-larsson-group) and I recently wrote a [paper](https://www.biorxiv.org/content/10.1101/2022.09.02.506367v1) on the role of autophagy in purifying mitochondrial DNA (mtDNA) selection. This was a really fun collaboration, and my first co-first authorship! Below is a brief outline of our work. Apologies for all the biology I am inevitably about to botch.

## Some background

Most mammalian cells have hundreds to thousands of copies of mtDNA, which are continuously replicated and degraded. This means it is possible for mutant and wildtype mtDNA to coexist in the same sample, a state known as heteroplasmy. Pathogenic mtDNA mutations are often heteroplasmic, and higher heteroplasmy levels (i.e. higher proportions of mutant mtDNA) can be associated with worse phenotype.

Curiously, despite mtDNA being inherited matrilineally and without any recombination, offspring can have _very_ different heteroplasmy levels compared to their mother. This has obvious clinical implications: a woman can be a healthy low-heteroplasmy mutation carrier, and her children may nevertheless develop severe mitochondrial disease. [Mitochondrial donation treatment](https://www.hfea.gov.uk/treatments/embryo-testing-and-treatments-for-disease/mitochondrial-donation-treatment/), a form of IVF where mitochondria from a healthy donor are used to create "three-parent babies", is one possible therapeutic intervention to avoid passing on mitochondrial disease. A better understanding of heteroplasmy and how and why it changes over time, as well as from mother to offspring, could one day result in new and more widely available treatment avenues.

We understand some of the mechanisms involved in mtDNA inheritance, but a lot remains unknown ([Zhang _et al._, 2018](https://portlandpress.com/essaysbiochem/article/62/3/225/78534/The-mitochondrial-DNA-genetic-bottleneck)). Selection against an mtDNA mutation could be the product of a wide range of biological processes, both at the organellar and at the cellular level. In this paper we wanted to investigate how autophagy, in the form of preferential degradation of mutant mtDNA, might play a role in purifying selection of mtDNA in the germline. To do so, Laura came up with a genetic knockout experiment focussing on four different autophagy genes.

## Experimental setup

There are broadly two types of targeted autophagy of mitochondria: ubiquitin- and adapter-dependent mitophagy. The former involves ubiquitilation of outer mitochondrial membrane proteins, and the latter depends on specific mitophagy receptors. PARKIN, which is a E3 ubiquitin ligase, plays an important and well-studied role in the former. As an example of the latter, Laura picked BCL2L13, which is relatively poorly understood, but which is a homolog of a well-known mitophagy receptor in yeast. She also selected on ULK1 and ULK2, which are involved in forming the autophagosome in the first place. It is worth pointing out here that ULK1 and ULK2 have high sequence similarity and functional redundancy but that there exists evidence for tissue-specific differences between them as well.

In order to test whether any of these proteins play a role in germline selection, Laura introduced homozygous knockouts of the _Parkin_, _Bcl2l13_, _Ulk1_ and _Ulk2_ genes in female heteroplasmic mice carrying the m.5024C>T mutation. These m.5024C>T mice are a standard model organism. From previous studies we know they exhibit germline selection, i.e. that high-heteroplasmy m.5024C>T mothers tend to have on average lower-heteroplasmy offspring ([Zhang _et al._, 2021](https://pubmed.ncbi.nlm.nih.gov/34878831/)). Hence, if a particular gene contributes to germline selection and we knock it out in female mice, we would expect their pups to have higher heteroplasmy compared to pups born to control m.5024C>T mice.

To see whether this happens for our four autophagy genes, Laura and colleagues developed six separate mouse colonies: one each with homozygous gene knockouts of _Parkin_, _Bcl2l13_, _Ulk1_ and _Ulk2_, as well as two control colonies with no knockouts. They then measured heteroplasmy of all 40 mothers and their 959 pups from ear biopsies. This is the point at which I joined the project, trying to make sense of the resulting numbers.

## So what did we find?

Above I implied we would be comparing knockout pups to controls, which is not quite right. Instead of comparing the pups, what we wanted to compare were the mother-to-pup changes. We know control pups have on average lower heteroplasmy than their mothers. We wanted to know whether the same change is observed for the knockout mice. To do this we worked with a quantity called normalised heteroplasmy shift ([Burgstaller _et al._, 2018](https://www.nature.com/articles/s41467-018-04797-2)):

$$nH = logit(H_{pup}) - logit(H_{mother})$$.

This normalised shift allows us to fairly compare the heteroplasmy of pups born to different mothers. If normalised shifts are consistently below zero in a particular genotype group, this suggests selection pressure against the mutation. The closer $$nH$$ is to zero, the weaker the evidence of selection. Predictably, we saw $$nH < 0$$ in our control groups. This means purifying selection in those groups happened as expected. We also saw $$nH < 0$$ in the _Parkin_ knockout group. However, the mean normalised heteroplasmy shift was much closer to zero in the other three knockout groups, for _Bcl2l13_, _Ulk1_, and _Ulk2_. Is this evidence these genes are involved in selection? Or is the significance of these results spurious?

Despite there being nearly a thousand pups in the study, these came from a relatively small pool of mothers. The same mother heteroplasmy measurement could easily contribute to twenty or thirty different data points when we calculated normalised shift. This would be fine if heteroplasmy measurements were accurate, but technical and biological variability (we think in the ballpark of 1-3%) can result in badly propagating errors. This meant we could not treat our observations as independent. To complicate things further, we have reason to believe both measurement error and heteroplasmy log-odds are generally non-normal, so I was not that keen on hierarchical linear-type models for this study.

Instead, we performed subsampling experiments to check whether knockout and control distributions were comparable if we varied the control mothers. When we did this, the _Bcl2l13_ effect remained detectable, but the _Ulk1_ and _Ulk2_ signals were not consistently observed. We also subsampled pup populations down, to make sure _Bcl2l13_ did not appear to have a significant effect simply because there was more data in this genotype group (239 pups compared to around 150 in other groups). This was not the case: the difference between controls and the _Bcl2l13_ knockout was consistently significant even when we sampled down to 120 pups.

All in all, we showed robust evidence that BCL2L13, a so-far poorly studied homolog of the well-known yeast mitophagy receptor ATG32, is involved in germline selection against m.5024C>T. More broadly, this indicates autophagy can play a role in purifying mtDNA selection in the germline.

## An open science approach

We made a concerted effort to ensure our work was as widely available and transparent as possible. You can find the preprint on bioRxiv:

Kremer, L.S., Bozhilova, L.V., Rubalcava-Gracia, D., Filograna, R., Upadhyay, M., Koolmeister, C., Chinnery, P.F. and Larsson, N.G., 2022. [A role for BCL2L13 and autophagy in germline purifying selection of mtDNA](https://www.biorxiv.org/content/10.1101/2022.09.02.506367v1.full). bioRxiv.

All data and code (down to how to get superscripts in `ggplot2` figure legends!) are available on GitHub: [https://github.com/lbozhilova/bcl2l13-mtdna-selection](https://github.com/lbozhilova/bcl2l13-mtdna-selection).

Finally, I recently made a (somewhat aesthetically impaired) poster describing this work, which you can check out [here](/assets/pdfs/poster_LyubaBozhilova_MBUSymp_2022.pdf).

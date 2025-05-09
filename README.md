# SIN3B Downstream analysis 

This repository contains the code used for the analysis of TCGA SKCM data. The analysis is divided into several sections, each corresponding to a specific aspect of the study. Below is a brief description of each section:

## Summary 

### Cell line transcriptome analysis

Gene Set Enrichment Analysis was performed using the clusterProfiler R package (Yu et al., 2012) to identify enriched biological processes associated with differential gene expression in SIN3B depletion. To analyze enriched gene terms and pathways, the results from differential expression analysis of SIN3B KO vs control cells were used as input to GSEA. The analysis used the complete ranked gene list based on fold changes. Gene annotation was performed using the org.Hs.eg.db database for human genes. The analysis was conducted using Biological Process (BP) from Gene Ontology, with a minimum set size of 5 genes, a significance threshold of nominal p-value < 0.05 and FDR < 0.05. The Benjamini-Hochberg (BH) method was used for multiple testing correction. Hallmark Pathway Analysis was performed using msigdbr (Dolgalev, 2022) for accessing MSigDB Hallmark gene set collection and fgsea (Korotkevich et al., 2019) with 1000 permutations and filtering on nominal p-value < 0.05.  Spearman's rank correlation coefficient was used to evaluate correlations between key genes (SIN3B, SIN3A, POLE4, BRAF, and STK11). Visualisations were created using ggplot2 (Wickham et al., 2016)


### Survival Analysis of TCGA SKCM dataset

To investigate the correlation between SIN3B expression and overall survival, an examination of the TCGA SKCM dataset (n = 472) was conducted, with separate analyses for primary tumours (n = 103) and metastases (n = 369). Data on expression and clinical details were obtained using TCGAbiolinks (v2.32.0) (Colaprico et al., 2015) and cBioportalData (Ramos et al., 2020). The count data underwent VST transformation via DESeq2 (Love et al., 2014) and subsequent scaling. Patients were categorised into SIN3B High/Low groups using three cut-off points: median, 66th, and 75th percentiles. Kaplan-Meier curves were generated and log-rank tests were computed for assessing between-group significance. Hazard ratios were calculated using Cox PH regression models. Both univariate and multivariate analyses were conducted, adjusting for clinical covariates including age, sex, stage, Breslow depth, and BRAF V600E mutation status.  We also investigated potential interactions between SIN3B and both POLE4 and STK11.

### Kaplan Meier Survival Analysis

In metastatic melanomas, significant survival differences were observed between SIN3B expression groups (66th percentile: p = 0.012, 75th percentile: p= 0.012, median: p = 0.054). In primary tumors, no significant associations were found across any classification threshold (median: p = 0.82; 66th percentile: p = 0.27; 75th percentile: p = 0.58).

### COX Proportional Hazards Models

To assess potential gene interactions with SIN3B and their influence on survival, whilst accounting for clinical variables, Cox Proportional Hazards Models were utilised. Expression data for SIN3B, POLE4, and STK11 were evaluated using scaled vst values. 

The 66th percentile classification was applied. In the primaries dataset, model instability and wide confidence intervals occurred when testing interactions. LASSO Cox regression identified stage as the only significant predictor, with SIN3B not retained in the final model. In metastatic melanomas, the univariate analysis showed high SIN3B expression was associated with worse overall survival (HR = 1.50, p = 0.0044). This association remained significant in multivariate analysis after adjusting for clinical covariates and BRAF mutation status. There were moderate correlations between BRAF and POLE4 (R= -0.32, p = 2.6e-10), SIN3B and BRAF (R = -0.21, p = 3.8e-05), SIN3B and POLE4 (R = 0.16, p = 0.0021), and SIN3B and STK11 (R = 0.37, p = 1.7e-13), suggesting interconnected effects. Therefore, separate interaction models for each gene pair were used to assess the effects of SIN3B with additional adjustments. 

In interaction models, neither POLE4 nor STK11 significantly modified the effect of SIN3B on survival (interaction p = 0.394 and p = 0.4269, respectively). POLE4 showed a trending association with worse survival (HR = 1.18, p = 0.074), while STK11 showed no significant association (HR = 0.94, p = 0.549). Significant predictors included stage (HR = 1.394, p = 0.002), Breslow depth (HR = 1.027, p = 0.030), and age (HR = 1.019 per year, p = 0.003). The complete case analysis included 360 patients with 193 events (univariate), and 251 patients with 143 events for multivariate. Breslow depth was the most frequently missing variable (27.4% of cases).




## References

-Yu G, Wang LG, Han Y, He QY. clusterProfiler: an R package for comparing biological themes among gene clusters. OMICS. 2012 May;16(5):284-7. doi: 10.1089/omi.2011.0118. Epub 2012 Mar 28. PMID: 22455463; PMCID: PMC3339379.

-Dolgalev I (2022). msigdbr: MSigDB Gene Sets for Multiple Organisms in a Tidy Data Format. R package version 7.5.1, https://CRAN.R-project.org/package=msigdbr.

-G. Korotkevich, V. Sukhov, A. Sergushichev. Fast gene set enrichment analysis. bioRxiv (2019), doi:10.1101/060012

-Love, M.I., Huber, W., Anders, S. Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2 Genome Biology 15(12):550 (2014)

-Therneau T (2024). A Package for Survival Analysis in R. R package version 3.7-0, https://CRAN.R-project.org/package=survival.

-Terry M. Therneau, Patricia M. Grambsch (2000). Modeling Survival Data: Extending the Cox Model. Springer, New York. ISBN 0-387-98784-3.

-Colaprico A, Silva TC, Olsen C, Garofano L, Cava C, Garolini D, Sabedot T, Malta TM, Pagnotta SM, Castiglioni I, Ceccarelli M, Bontempi G, Noushmehr H (2015). “TCGAbiolinks: An R/Bioconductor package for integrative analysis of TCGA data.” Nucleic Acids Research. doi:10.1093/nar/gkv1507, http://doi.org/10.1093/nar/gkv1507.

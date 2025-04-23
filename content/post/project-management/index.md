---
title: 🧰 Quantile-Quantile Plots (QQ-plots)
summary: GWAS Toolbox for generating a QQ-plot & Lambda
date: 2025-04-18
authors:
  - admin
tags:
  - Bioinformatics
  - Statistical Genetics
  - Genome-wide Association Studies
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com)'
---

📈Q-Q plot is an essential tool for detecting problems (such as unrecognized population structure, analytical approach, genotyping artifacts, etc.) in a Genome-wide association study (GWAS). 

## Introduction

* A Quantile-Quantile (QQ) plot (in general) plots the observed quantiles of one distribution versus another, OR plots the observed quantiles of a distribution versus the quantiles of the ideal distribution.

* In GWAS, we use a QQ plot to plot our the quantile distribution of observed p-values (on the y-axis) versus the quantile distribution of expected p-values. In an ideal situation, where there ARE NO causal polymorphisms, the QQ-plot will be a line. 

* The reason is that we will observe a uniform distribution of p-values from such a case and in our QQ, we are plotting this observed distribution of p-value versus the expected distribution of p-values: a uniform distribution (where both have been -log transformed).

** Note that if your GWAS analysis is correct but you do not have enough power to detect positions of causal polymorphisms, this will also be your result (!!)-> it is a way to assess whether you can detect any hits in your GWAS.

### To plot a QQ-plot

One way to do it is by using the [qqman package](https://cran.r-project.org/web/packages/qqman/vignettes/qqman.html) in R. 

<div class="highlight" style="padding: 1.5rem;">
<pre class="chroma">
<code>
  install.packages('qqman')
  library(qqman)
  qq(result$PVALUE, main = "QQ Plot of {{Project}}")  
</code>
</pre>
</div>

### Lambda (λ)
When making a QQ-plot, it is important to calculate lambda (also called the genomic inflation factor, often written as λGC).

* λ quantifies how much the observed test statistics deviate from what you’d expect under the null hypothesis (i.e., no association). It helps assess whether your test statistics are inflated due to technical or population structure issues.
* Detect inflations or deflations of P-values

📈 If your QQ plot shows a systematic upward curve and λ is >>1 (e.g., 1.2 or higher), it suggests inflation, possibly from:
* Population stratification
* Cryptic relatedness
* Batch effects
* Genotyping errors

📈 If λ is <1, it might signal deflation, often due to:
* Overcorrection
* Conservative test statistics
* Sparse data

<div class="highlight" style="padding: 1.5rem;">
<pre class="chroma">
<code>
  chisq <- qchisq(1 - result$PVALUE, df = 1)
  lambda <- median(chisq, na.rm = TRUE) / qchisq(0.5, df = 1)
  legend("topleft", legend = bquote(lambda == .(round(lambda, 3))), bty = "n")
</code>
</pre>
</div>

To read more, see my [GitHub repository](https://github.com/ruuhsu/QQ_Plot_lambda)

## References
1. _[Statistical Horizons](https://statisticalhorizons.com/wp-content/uploads/2022/04/SG-Sample-Materials-1.pdf)_
2. _Ehret GB, Curr Hypertens Rep. 2010 Feb;12(1):17–25. doi: [10.1007/s11906-009-0086-6](https://pmc.ncbi.nlm.nih.gov/articles/PMC2865585/)_

## Did you find this page helpful? Consider sharing it 🙌

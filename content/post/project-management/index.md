---
title: Quantile, Quantile-Quantile, and GWAS
summary: Q-Q plot
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
## Quantile (分位數/切位點）
" In statistics and probability, quantiles are cut points dividing the range of a probability distribution into continuous intervals with equal probabilities or dividing the observations in a sample in the same way." - Wikipedia

A quantile is the cutoff value at a certain proportion of the data. If you divide your dataset into _k_ equal groups, then you get _k_ inequalities.
So: 

* 2 quantiles → Median (中位數)
Splits the distribution into 2 halves.

* 4 quantiles → Quartiles (四分位數)
Splits the distribution into 4 equal parts (Q1, Q2 [= median], Q3).

* 100 quantiles → Percentiles (百分位數)
Splits the distribution into 100 equal parts.

## Quantile-Quantile Plot (QQ-plot/分位數對分位數圖)

### Purpose:
* Compare the distribution of two datasets.
* Assess whether a dataset follows a specified theoretical distribution (e.g., normal distribution).

{{% callout tip %}}
A QQ-plot provides a visual comparison of quantiles, offering a nuanced view of how a dataset deviates from a theoretical distribution. It allows one to see patterns of deviation, such as skewness or heavy tails. In contrast, a Goodness-of-Fit test is a formal statistical procedure that evaluates whether a sample comes from a specified distribution (e.g., normal, Poisson, chi-square). While it yields a clear statistical decision, it typically requires a sufficiently large sample size and does not indicate where or how the deviations occur.
{{% /callout %}}


### How it works:
* When comparing the distribution of two datasets.

1. We sort our data based on order statistics (順序統計量)

$$
\begin{align}
X1,X2,X3......Xn -> X(1)​≤X(2)​≤X(3)​≤⋯≤X(n)​
\end{align}
$$

$$
\begin{align}
Y1,Y2,Y3......Yn -> Y(1)​≤Y(2)​≤Y(3)​≤⋯≤Y(n)​
\end{align}
$$

3. To compare empirical data with a theoretical distribution (e.g., in a QQ-plot), you map each order statistic 𝑋(𝑖) to a quantile probability.

$$
\begin{align}
p_i &= \frac{i}{n+1} \\
Y_{(i)} &= F^{-1}(p_i)
\end{align}
$$

$X_{(i)}$ → the $i^{\text{th}}$ ordered sample value.  

$p_i$ → its plotting position (approximate quantile level).  


* Plots the quantiles of one dataset against the quantiles of another (or against a theoretical distribution).


Q-Q plot is an essential tool for detecting problems (such as unrecognized population structure, analytical approach, genotyping artifacts, etc.) in a Genome-wide association study (GWAS). 

## Q-Q plot and GWAS
* Q-Q plots the observed quantiles of one distribution versus another, OR plots the observed quantiles of a distribution versus the quantiles of the ideal distribution.

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

To read more, see [GitHub repository](https://github.com/ruuhsu/QQ_Plot_lambda)

## References
1. _[Statistical Horizons](https://statisticalhorizons.com/wp-content/uploads/2022/04/SG-Sample-Materials-1.pdf)_
2. _Ehret GB, Curr Hypertens Rep. 2010 Feb;12(1):17–25. doi: [10.1007/s11906-009-0086-6](https://pmc.ncbi.nlm.nih.gov/articles/PMC2865585/)_


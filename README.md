VLTimeCausality: Variable-Lag Time Series Causality Inference Framework
===========================================================
[![minimal R version](https://img.shields.io/badge/R%3E%3D-3.5.0-6666ff.svg)](https://cran.r-project.org/)
[![CRAN Status Badge](https://www.r-pkg.org/badges/version-last-release/VLTimeCausality)](https://cran.r-project.org/package=VLTimeCausality)
[![Download](https://cranlogs.r-pkg.org/badges/grand-total/VLTimeCausality)](https://cran.r-project.org/package=VLTimeCausality)
[![arXiv](https://img.shields.io/badge/cs.LG-arXiv%3A2002.00208-B31B1B.svg)](https://arxiv.org/abs/2002.00208)
[![](https://img.shields.io/badge/doi-10.1145%2F3441452-yellow)](https://doi.org/10.1145/3441452 )
[![License](https://img.shields.io/badge/License-GPL%203-orange.svg)](https://spdx.org/licenses/GPL-3.0-only.html)

A framework to infer causality on a pair of time series of real numbers based on Variable-lag Granger causality (VL-Granger) and transfer entropy (VL-Transfer Entropy).

Typically, Granger causality and transfer entropy have an assumption of a fixed and constant time delay between the cause and effect. However, for a non-stationary time series, this assumption is not true. For example, considering two time series of velocity of person A and person B where B follows A. At some time, B stops tying his shoes, then running to catch up A. The fixed-lag assumption is not true in this case.

We propose a framework that allows variable-lags between cause and effect in Granger causality and transfer entropy to allow them to deal with variable-lag non-stationary time series. 

Installation
------------

You can install our package from CRAN

```r
install.packages("VLTimeCausality")
```

For the newest version on github, please call the following command in R terminal.


``` r
remotes::install_github("DarkEyes/VLTimeSeriesCausality")
```
This requires a user to install the "remotes" package before installing VLTimeSeriesCausality.

Example: Inferred VL-Granger causality time series
----------------------------------------------------------------------------------
In the first step, we generate time series  TS$X and TS$Y where TS$X causes TS$Y with variable-lags.
``` r
library(VLTimeCausality)
# Generate simulation data
TS <- VLTimeCausality::SimpleSimulationVLtimeseries()
```

We can plot time series using the following function.
```r
VLTimeCausality::plotTimeSeries(TS$X,TS$Y)
```
A sample of generated time series pair that has a causal relation is plotted below:  

<img src="https://github.com/DarkEyes/VLTimeSeriesCausality/blob/master/man/FIG/TSsample.png" width="550">

We use the following function to infer whether X causes Y.
``` r
# Run the function
out<-VLTimeCausality::VLGrangerFunc(Y=TS$Y,X=TS$X)
```
The result of VL-Granger causality is below:

```r
out$BICDiffRatio
[1] 0.8882051

out$XgCsY
[1] TRUE
```

If out$XgCsY is true, then it means that X VL-Granger-causes Y. The value out$BICDiffRatio is a BIC difference ratio. If out$BICDiffRatio>0, it means that X is a good predictor of Y behaviors. The closer out$BICDiffRatio to 1, the stronger we can claim that X VL-Granger-causes Y.

For the comparison between normal Granger test with our VL-Granger test, we recommend to use the F-test decision criterion the same as typical Granger test criterion.

Below are the results of VL-Granger Causality using F-test.

```r
library(lmtest)
data(ChickEgg)
ChickEgg <- as.data.frame(ChickEgg)

#============ The the egg causes chicken. 
out_test1 <- VLTimeCausality::VLGrangerFunc(X=ChickEgg$egg,Y=ChickEgg$chicken)
out_test1$p.val
[1] 0.004980847
out_test1$XgCsY_ftest
[1] TRUE 

#============ The reverse direction has no causal relation
out_test2 <- VLTimeCausality::VLGrangerFunc(Y=ChickEgg$egg,X=ChickEgg$chicken)
out_test2$p.val
[1] 1
out_test2$XgCsY_ftest
[1] FALSE
```


Citation
----------------------------------------------------------------------------------
Chainarong Amornbunchornvej, Elena Zheleva, and Tanya Berger-Wolf (2021). Variable-lag Granger Causality and Transfer Entropy for Time Series Analysis. ACM Transactions on Knowledge Discovery from Data (TKDD), 15(4), 1-30.  https://doi.org/10.1145/3441452 

Contact
----------------------------------------------------------------------------------
- Developer: C. Amornbunchornvej<div itemscope itemtype="https://schema.org/Person"><a itemprop="sameAs" content="https://orcid.org/0000-0003-3131-0370" href="https://orcid.org/0000-0003-3131-0370" target="orcid.widget" rel="noopener noreferrer" style="vertical-align:top;"><img src="https://orcid.org/sites/default/files/images/orcid_16x16.png" style="width:1em;margin-right:.5em;" alt="ORCID iD icon">https://orcid.org/0000-0003-3131-0370</a></div>
- <a href="https://www.nectec.or.th">Strategic Analytics Networks with Machine Learning and AI (SAI)</a>, <a href="https://www.nectec.or.th/en/">NECTEC</a>, Thailand
- Homepage: <a href="https://sites.google.com/view/amornbunchornvej/home">Link</a>

# Getting Started with Genomic Prediction
Julin Maloof  
November 22, 2016  





# Genomic Selection Intro

## Goal: Predictive Breeding
<!-- .slide: style="text-align: left;"> --> 

* Want to breed plants for better performance, resistance, etc.
* Use genetic / genomic data to predict best plants for breeding

## Old school: Marker assisted selection

* Use QTL or GWAS to find a few markers linked to trait of interest
* Genotype at those markers to "pyramid" during breeding
* Problem: ignores the many, many small loci that contribute to the trait

## Genomic Selection

* Instead of focusing on a few main loci, try to predict the effects of all loci
* QTL and GWAS fit a separate regression model for each location being considered
$ hyp \sim \beta1*M1 $
$ hyp \sim \beta2*M2 $
$ hyp \sim \beta3*M3 $
...
$ hyp \sim  \beta * M250000 $

* Genomic Selection fits a model to all markers simultaneously

$ hyp \sim \beta1*M1 + \beta2*M2 + \beta3*M3 + ... +\beta250000*M250000 $

## Genomic Selection

* Genomic Selection fits a model to all markers simultaneously

$ hyp \sim \beta1*M1 + \beta2*M2 + \beta3*M3 + ... +\beta250000*M250000 $

* Once this model is fit then we can predict the performance of other strains
* Challenge: How to fit a regression model with hundreds of thousands of predictors?

## Penalized Regression

* Standard regression models do not perform well with many predictors
* If we expect that most predictors will have an effect size of ~ 0 (we do in this case), use _penalized regression_
* Can be done using frequentist framework:
  * "Penalize" models for non-zero coefficients
  * Lasso Regression, Ridge Regression, Elastic Net
* Can be done using Bayesian framework:
  * Use priors strongly biased towards zero
  * Horseshoe, Laplace, Student's T, etc.
  
# Today's goal

## Tool exploration

* The KIAT project aims to use Genomic Selection techniques for predicitive breeding
* My goal was to start becoming familiar with the available tools in R
* Use hypocotyl data set from Filiaut and Maloof (2012)
    * 169 Arabidopsis natural accessions
    * 250,000 SNPs
* Train on 120 and try to predict remaining 49
                              
## get the data




# bigRR

Package bigRR (big Ridge Regression) uses optimized code to fit penalized Ridge Regression models.  
Enables separate shrinkage parameters to be used for each marker, in a two-step process



## compare models with uniform and variable shrinkage

using all data.  Plot predicted marker effects



![](Genome_Prediction_files/figure-revealjs/unnamed-chunk-5-1.png)

## plot predicted versus actual (using all data)

![](Genome_Prediction_files/figure-revealjs/unnamed-chunk-6-1.png)

```
##            observed     bigRR bigRR.HME
## observed  1.0000000 0.9916559 0.9970304
## bigRR     0.9916559 1.0000000 0.9976740
## bigRR.HME 0.9970304 0.9976740 1.0000000
```
fits the model data well

## predict new observations



![](Genome_Prediction_files/figure-revealjs/unnamed-chunk-8-1.png)

```
##            observed     bigRR bigRR.HME
## observed  1.0000000 0.3966104 0.4061482
## bigRR     0.3966104 1.0000000 0.9928781
## bigRR.HME 0.4061482 0.9928781 1.0000000
```

On to something else...

# rrBLUP

similar to bigRR, not included



fit model


predict from model


# BGLR

BGLR is a package that implements many different Bayesian approached to genomics selection.

Two versions will be explored here
* Bayesian Lasso
* Bayes C

## Fit a Bayesian Lasso model



Fit a Bayesian Lasso model, with K



Fit a BayesC model with



Fit a BayesC model with K






## Bayesian Lasso, no structure


```r
plot_fitted_observed(bglr.bl1)
```

![](Genome_Prediction_files/figure-revealjs/unnamed-chunk-17-1.png)

```
## [1] 0.9670503
```

```r
plot_predicted_observed(bglr.bl1)
```

![](Genome_Prediction_files/figure-revealjs/unnamed-chunk-17-2.png)

```
##           [,1]
## [1,] 0.3450679
```

## Bayesian Lasso, with structure


```r
plot_fitted_observed(bglr.bl2)
```

![](Genome_Prediction_files/figure-revealjs/unnamed-chunk-18-1.png)

```
## [1] 0.9688608
```

```r
plot_predicted_observed(bglr.bl2)
```

![](Genome_Prediction_files/figure-revealjs/unnamed-chunk-18-2.png)

```
##           [,1]
## [1,] 0.3698421
```

## BayesC, no structure


```r
plot_fitted_observed(bglr.bc1)
```

![](Genome_Prediction_files/figure-revealjs/unnamed-chunk-19-1.png)

```
## [1] 0.9803918
```

```r
plot_predicted_observed(bglr.bc1)
```

![](Genome_Prediction_files/figure-revealjs/unnamed-chunk-19-2.png)

```
##           [,1]
## [1,] 0.3746115
```

## BayesC, with structure


```r
plot_fitted_observed(bglr.bc2)
```

![](Genome_Prediction_files/figure-revealjs/unnamed-chunk-20-1.png)

```
## [1] 0.9817013
```

```r
plot_predicted_observed(bglr.bc2)
```

![](Genome_Prediction_files/figure-revealjs/unnamed-chunk-20-2.png)

```
##           [,1]
## [1,] 0.4140168
```




# To Do

* Check BGLR convergence
* Compare to traditional GWAS
    * Marker effects
    * Predictions

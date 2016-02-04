# Chapter-9
Joey and Sean  

## Plan for today
- PCA
- PCoA
- NMDS
- Correspondence analysis


## Load packages and functions


```r
# (vegan must be loaded after ade4 to avoid some conflicts)
library(ade4)
library(vegan)
library(gclus)
library(ape)
```

## Load additional functions

```r
# (files must be in the working directory)
source("evplot.R")
source("cleanplot.pca.R")
source("PCA.R")
source("CA.R")
```

## Import data

```r
# (files must be in the working directory)
spe <- read.csv("DoubsSpe.csv", row.names=1)
env <- read.csv("DoubsEnv.csv", row.names=1)
spa <- read.csv("DoubsSpa.csv", row.names=1)
# Remove empty site 8
spe <- spe[-8,]
env <- env[-8,]
spa <- spa[-8,]
```

## Intro to CA
-CA is well suited to the analysis of species abundance data without pre-transformation. 

-data submitted to CA must be frequencies or frequency-like, dimensionally homogeneous and non-negative (as is the case of species counts or presence–absence data)


## CA of the raw species dataset (original species abundances)

```r
# Compute CA
spe.ca <- cca(spe)
spe.ca
```

```
## Call: cca(X = spe)
## 
##               Inertia Rank
## Total           1.167     
## Unconstrained   1.167   26
## Inertia is mean squared contingency coefficient 
## 
## Eigenvalues for unconstrained axes:
##    CA1    CA2    CA3    CA4    CA5    CA6    CA7    CA8 
## 0.6010 0.1444 0.1073 0.0834 0.0516 0.0418 0.0339 0.0288 
## (Showed only 8 of all 26 unconstrained eigenvalues)
```

```r
summary(spe.ca)		# default scaling 2
```

```
## 
## Call:
## cca(X = spe) 
## 
## Partitioning of mean squared contingency coefficient:
##               Inertia Proportion
## Total           1.167          1
## Unconstrained   1.167          1
## 
## Eigenvalues, and their contribution to the mean squared contingency coefficient 
## 
## Importance of components:
##                         CA1    CA2     CA3     CA4     CA5     CA6     CA7
## Eigenvalue            0.601 0.1444 0.10729 0.08337 0.05158 0.04185 0.03389
## Proportion Explained  0.515 0.1237 0.09195 0.07145 0.04420 0.03586 0.02904
## Cumulative Proportion 0.515 0.6388 0.73069 0.80214 0.84634 0.88220 0.91124
##                           CA8     CA9    CA10    CA11     CA12     CA13
## Eigenvalue            0.02883 0.01684 0.01083 0.01014 0.007886 0.006123
## Proportion Explained  0.02470 0.01443 0.00928 0.00869 0.006760 0.005250
## Cumulative Proportion 0.93594 0.95038 0.95965 0.96835 0.975100 0.980350
##                           CA14     CA15     CA16     CA17     CA18
## Eigenvalue            0.004867 0.004606 0.003844 0.003067 0.001823
## Proportion Explained  0.004170 0.003950 0.003290 0.002630 0.001560
## Cumulative Proportion 0.984520 0.988470 0.991760 0.994390 0.995950
##                           CA19     CA20      CA21      CA22      CA23
## Eigenvalue            0.001642 0.001295 0.0008775 0.0004217 0.0002149
## Proportion Explained  0.001410 0.001110 0.0007500 0.0003600 0.0001800
## Cumulative Proportion 0.997360 0.998470 0.9992200 0.9995900 0.9997700
##                            CA24      CA25      CA26
## Eigenvalue            0.0001528 8.949e-05 2.695e-05
## Proportion Explained  0.0001300 8.000e-05 2.000e-05
## Cumulative Proportion 0.9999000 1.000e+00 1.000e+00
## 
## Scaling 2 for species and site scores
## * Species are scaled proportional to eigenvalues
## * Sites are unscaled: weighted dispersion equal on all dimensions
## 
## 
## Species scores
## 
##          CA1       CA2      CA3       CA4       CA5       CA6
## CHA  1.50075 -1.410293  0.26049 -0.307203  0.271777 -0.003465
## TRU  1.66167  0.444129  0.57571  0.166073 -0.261870 -0.326590
## VAI  1.28545  0.285328 -0.04768  0.018126  0.043847  0.200732
## LOC  0.98662  0.360900 -0.35265 -0.009021 -0.012231  0.253429
## OMB  1.55554 -1.389752  0.80505 -0.468471  0.471301  0.225409
## BLA  0.99709 -1.479902 -0.48035  0.079397 -0.105715 -0.332445
## HOT -0.54916 -0.051534  0.01123 -0.096004 -0.382763  0.134807
## TOX -0.18478 -0.437710 -0.57438  0.424267 -0.587150  0.091866
## VAN  0.01337 -0.095342 -0.57672  0.212017  0.126668 -0.389103
## CHE  0.01078  0.140577 -0.34811 -0.538268  0.185286  0.167087
## BAR -0.33363 -0.300682 -0.04929  0.170961 -0.157203  0.103068
## SPI -0.38357 -0.255310 -0.20136  0.374057 -0.385866  0.239001
## GOU -0.32152 -0.034382 -0.07423 -0.031236  0.014417 -0.156351
## BRO -0.26165  0.187282  0.00617  0.183771  0.295142 -0.262808
## PER -0.28913  0.121044 -0.18919  0.367615  0.218087 -0.163675
## BOU -0.60298 -0.057369  0.20341  0.214299 -0.050977  0.211926
## PSO -0.58669 -0.082467  0.21198  0.050175 -0.120456  0.108724
## ROT -0.61815  0.124733  0.13339  0.147190  0.317736 -0.340380
## CAR -0.57951 -0.110732  0.20173  0.308547  0.006854  0.153224
## TAN -0.37880  0.138023 -0.07825  0.095793  0.256285 -0.029245
## BCO -0.70235  0.011155  0.40242  0.211582  0.138186  0.132297
## PCH -0.73238 -0.009098  0.55678  0.321852  0.281812  0.172271
## GRE -0.69300  0.038971  0.37688 -0.183965 -0.051945 -0.011126
## GAR -0.44181  0.176915 -0.23691 -0.345104  0.129676 -0.043802
## BBO -0.70928  0.032317  0.40924  0.030224  0.049050  0.114560
## ABL -0.63114  0.053594  0.15204 -0.661381 -0.414796 -0.206611
## ANG -0.63578 -0.041894  0.30093  0.224044  0.030444  0.203160
## 
## 
## Site scores (weighted averages of species scores)
## 
##         CA1       CA2        CA3      CA4      CA5      CA6
## 1   2.76488  3.076306  5.3657489  1.99192 -5.07714 -7.80447
## 2   2.27540  2.565531  1.2659130  0.87538 -1.89139 -0.13887
## 3   2.01823  2.441224  0.5144079  0.79436 -1.03741  0.56015
## 4   1.28485  1.935664 -0.2509482  0.76470  0.54752  0.10579
## 5   0.08875  1.015182 -1.4555434  0.47672  2.69249 -2.92498
## 6   1.03188  1.712163 -0.9544059  0.01584  0.91932  0.39856
## 7   1.91427  2.256208 -0.0001407  0.39844 -1.07017  0.32127
## 9   0.25591  1.443008 -2.5777721 -3.41400  2.36613  2.71741
## 10  1.24517  1.526391 -1.9635663 -0.41230  0.69647  1.51859
## 11  2.14501  0.110278  1.6108693 -0.82023  0.53918  1.01153
## 12  2.17418 -0.251649  1.5845397 -0.81483  0.52623  1.05501
## 13  2.30944 -2.034439  1.9181448 -0.60481  0.64435 -0.14844
## 14  1.87141 -2.262503  1.1066796 -0.80840  1.09542  0.11038
## 15  1.34659 -1.805967 -0.6441505 -0.52803  0.76871 -0.67165
## 16  0.70214 -1.501167 -1.9735888  0.98502 -0.93585 -1.27168
## 17  0.28775 -0.836803 -1.2259108  0.73302 -1.57036  0.57315
## 18  0.05299 -0.647950 -0.9234228  0.35770 -0.95401  0.77738
## 19 -0.20584 -0.007252 -1.0154343  0.07041 -1.03450  0.51442
## 20 -0.57879  0.042849 -0.3660551 -0.15019 -0.61357  0.10115
## 21 -0.67320  0.038875  0.1194956  0.17256 -0.14686 -0.12018
## 22 -0.71933  0.014694  0.2204186  0.13598  0.09459 -0.02068
## 23 -0.70438  0.735398 -0.6546250 -6.61523 -2.49441 -1.73215
## 24 -0.83976  0.390120  0.5605295 -4.38864 -2.56916 -0.96702
## 25 -0.68476  0.418842 -0.2860819 -2.80336 -0.37540 -3.93791
## 26 -0.75808  0.210204  0.5894091 -0.70004 -0.01880 -0.10779
## 27 -0.75046  0.100869  0.5531191 -0.12946  0.29164  0.11280
## 28 -0.77878  0.088976  0.7379012  0.05204  0.40940  0.43236
## 29 -0.60815 -0.203235  0.5522726  0.43621  0.15010  0.25618
## 30 -0.80860 -0.019592  0.6686542  0.88136  0.52744  0.16456
```

```r
summary(spe.ca, scaling=1) 
```

```
## 
## Call:
## cca(X = spe) 
## 
## Partitioning of mean squared contingency coefficient:
##               Inertia Proportion
## Total           1.167          1
## Unconstrained   1.167          1
## 
## Eigenvalues, and their contribution to the mean squared contingency coefficient 
## 
## Importance of components:
##                         CA1    CA2     CA3     CA4     CA5     CA6     CA7
## Eigenvalue            0.601 0.1444 0.10729 0.08337 0.05158 0.04185 0.03389
## Proportion Explained  0.515 0.1237 0.09195 0.07145 0.04420 0.03586 0.02904
## Cumulative Proportion 0.515 0.6388 0.73069 0.80214 0.84634 0.88220 0.91124
##                           CA8     CA9    CA10    CA11     CA12     CA13
## Eigenvalue            0.02883 0.01684 0.01083 0.01014 0.007886 0.006123
## Proportion Explained  0.02470 0.01443 0.00928 0.00869 0.006760 0.005250
## Cumulative Proportion 0.93594 0.95038 0.95965 0.96835 0.975100 0.980350
##                           CA14     CA15     CA16     CA17     CA18
## Eigenvalue            0.004867 0.004606 0.003844 0.003067 0.001823
## Proportion Explained  0.004170 0.003950 0.003290 0.002630 0.001560
## Cumulative Proportion 0.984520 0.988470 0.991760 0.994390 0.995950
##                           CA19     CA20      CA21      CA22      CA23
## Eigenvalue            0.001642 0.001295 0.0008775 0.0004217 0.0002149
## Proportion Explained  0.001410 0.001110 0.0007500 0.0003600 0.0001800
## Cumulative Proportion 0.997360 0.998470 0.9992200 0.9995900 0.9997700
##                            CA24      CA25      CA26
## Eigenvalue            0.0001528 8.949e-05 2.695e-05
## Proportion Explained  0.0001300 8.000e-05 2.000e-05
## Cumulative Proportion 0.9999000 1.000e+00 1.000e+00
## 
## Scaling 1 for species and site scores
## * Sites are scaled proportional to eigenvalues
## * Species are unscaled: weighted dispersion equal on all dimensions
## 
## 
## Species scores
## 
##          CA1      CA2      CA3      CA4      CA5      CA6
## CHA  1.93586 -3.71167  0.79524 -1.06393  1.19669 -0.01694
## TRU  2.14343  1.16888  1.75759  0.57516 -1.15306 -1.59651
## VAI  1.65814  0.75094 -0.14555  0.06277  0.19306  0.98127
## LOC  1.27267  0.94983 -1.07661 -0.03124 -0.05385  1.23887
## OMB  2.00654 -3.65761  2.45774 -1.62244  2.07523  1.10190
## BLA  1.28617 -3.89487 -1.46646  0.27497 -0.46548 -1.62514
## HOT -0.70838 -0.13563  0.03428 -0.33249 -1.68537  0.65900
## TOX -0.23836 -1.15198 -1.75354  1.46935 -2.58533  0.44908
## VAN  0.01724 -0.25092 -1.76067  0.73427  0.55774 -1.90211
## CHE  0.01391  0.36998 -1.06276 -1.86417  0.81585  0.81679
## BAR -0.43036 -0.79135 -0.15048  0.59208 -0.69219  0.50384
## SPI -0.49478 -0.67194 -0.61472  1.29546 -1.69904  1.16834
## GOU -0.41473 -0.09049 -0.22662 -0.10818  0.06348 -0.76431
## BRO -0.33751  0.49290  0.01884  0.63645  1.29957 -1.28472
## PER -0.37296  0.31857 -0.57758  1.27315  0.96028 -0.80011
## BOU -0.77780 -0.15099  0.62098  0.74218 -0.22446  1.03599
## PSO -0.75678 -0.21704  0.64715  0.17377 -0.53039  0.53149
## ROT -0.79737  0.32828  0.40724  0.50976  1.39905 -1.66393
## CAR -0.74752 -0.29143  0.61585  1.06858  0.03018  0.74903
## TAN -0.48862  0.36325 -0.23888  0.33176  1.12847 -0.14296
## BCO -0.90598  0.02936  1.22855  0.73277  0.60846  0.64673
## PCH -0.94471 -0.02395  1.69979  1.11466  1.24087  0.84214
## GRE -0.89392  0.10256  1.15059 -0.63712 -0.22872 -0.05439
## GAR -0.56990  0.46561 -0.72328 -1.19519  0.57099 -0.21413
## BBO -0.91492  0.08505  1.24936  0.10467  0.21598  0.56002
## ABL -0.81412  0.14105  0.46416 -2.29054 -1.82642 -1.01000
## ANG -0.82011 -0.11026  0.91871  0.77593  0.13405  0.99313
## 
## 
## Site scores (weighted averages of species scores)
## 
##         CA1       CA2        CA3       CA4       CA5      CA6
## 1   2.14343  1.168878  1.7575907  0.575155 -1.153061 -1.59651
## 2   1.76398  0.974804  0.4146591  0.252762 -0.429551 -0.02841
## 3   1.56461  0.927572  0.1684981  0.229368 -0.235605  0.11459
## 4   0.99607  0.735478 -0.0821999  0.220804  0.124347  0.02164
## 5   0.06880  0.385730 -0.4767740  0.137649  0.611487 -0.59835
## 6   0.79995  0.650556 -0.3126227  0.004573  0.208785  0.08153
## 7   1.48401  0.857273 -0.0000461  0.115048 -0.243045  0.06572
## 9   0.19839  0.548288 -0.8443683 -0.985772  0.537369  0.55588
## 10  0.96530  0.579970 -0.6431806 -0.119049  0.158175  0.31065
## 11  1.66289  0.041901  0.5276521 -0.236838  0.122453  0.20692
## 12  1.68551 -0.095617  0.5190277 -0.235278  0.119512  0.21582
## 13  1.79036 -0.773009  0.6283025 -0.174635  0.146337 -0.03037
## 14  1.45079 -0.859665  0.3625011 -0.233420  0.248779  0.02258
## 15  1.04393 -0.686198 -0.2109962 -0.152467  0.174580 -0.13740
## 16  0.54432 -0.570386 -0.6464636  0.284420 -0.212539 -0.26014
## 17  0.22308 -0.317953 -0.4015561  0.211655 -0.356642  0.11725
## 18  0.04108 -0.246196 -0.3024740  0.103285 -0.216664  0.15902
## 19 -0.15958 -0.002755 -0.3326130  0.020330 -0.234943  0.10523
## 20 -0.44870  0.016281 -0.1199041 -0.043366 -0.139347  0.02069
## 21 -0.52189  0.014771  0.0391417  0.049826 -0.033352 -0.02458
## 22 -0.55765  0.005583  0.0721997  0.039264  0.021482 -0.00423
## 23 -0.54606  0.279423 -0.2144273 -1.910111 -0.566501 -0.35434
## 24 -0.65102  0.148231  0.1836056 -1.267193 -0.583479 -0.19782
## 25 -0.53085  0.159144 -0.0937082 -0.809453 -0.085257 -0.80556
## 26 -0.58769  0.079869  0.1930653 -0.202133 -0.004271 -0.02205
## 27 -0.58178  0.038326  0.1811782 -0.037380  0.066234  0.02307
## 28 -0.60374  0.033808  0.2417050  0.015027  0.092978  0.08844
## 29 -0.47146 -0.077222  0.1809010  0.125954  0.034090  0.05241
## 30 -0.62686 -0.007444  0.2190226  0.254487  0.119787  0.03366
```

```r
## things to note: the first axis has a very large eigenvalue. In CA, a value over 0.6 indicate a very strong gradient in the data. Note that the eigenvalues are the same in both scalings. The scaling affects the eigenvectors, not the eigenvalues. 
```
## Scaling options
- when scaling = 1 the distances among objects approximate their x2 distances (i.e. object points close together are similar in their species frequencies). Any object near the point representing a species is likely to contain a high contribution of that species. 

- when scaling = 2, ordination of species. Species points close to one another are likely to have similar relative frequencies among objects. 



## Plot eigenvalues and % of variance for each axis

```r
ev2 <- spe.ca$CA$eig
```
## plot 

```r
evplot(ev2)
```

![](Chap-9-presentation_files/figure-html/unnamed-chunk-6-1.png) 
-things to note: the first axis is extremely dominant. 

## CA biplots

```r
par(mfrow=c(1,2))
# Scaling 1: sites are centroids of species
plot(spe.ca, scaling=1, main="CA fish abundances - biplot scaling 1")
# Scaling 2 (default): species are centroids of sites
plot(spe.ca, main="CA fish abundances - biplot scaling 2")
```

![](Chap-9-presentation_files/figure-html/unnamed-chunk-7-1.png) 
## A posteriori projection of environmental variables in a CA
-envfit finds vectors or factor averages of environmental variables. [...] The projections of points onto vectors have maximum correlation with corresponding environmental variables, and the factors show the averages of factor levels”
	
## CA biplot (scaling 2) of the Doubs fish abundance data with a posteriori projection of environmental variables


```r
# The last plot produced (CA scaling 2) must be active
plot(spe.ca, main="CA fish abundances - biplot scaling 2")
(spe.ca.env <- envfit(spe.ca, env))
```

```
## 
## ***VECTORS
## 
##          CA1      CA2     r2 Pr(>r)    
## das -0.94801 -0.31825 0.6889  0.001 ***
## alt  0.81141  0.58448 0.8080  0.001 ***
## pen  0.73753  0.67531 0.2976  0.006 ** 
## deb -0.92837 -0.37166 0.4440  0.002 ** 
## pH   0.50723 -0.86181 0.0908  0.212    
## dur -0.71728 -0.69678 0.4722  0.001 ***
## pho -0.99897  0.04533 0.1757  0.086 .  
## nit -0.94906 -0.31511 0.4510  0.002 ** 
## amm -0.97495  0.22241 0.1762  0.079 .  
## oxy  0.93352 -0.35854 0.6263  0.001 ***
## dbo -0.94094  0.33857 0.2237  0.040 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## Permutation: free
## Number of permutations: 999
```

```r
plot(spe.ca.env)
# Plot significant variables with a different colour
plot(spe.ca.env, p.max=0.05, col=3)
```

![](Chap-9-presentation_files/figure-html/unnamed-chunk-8-1.png) 

## Species data table ordered after the CA result


```r
vegemite(spe, spe.ca)
```

```
##                                   
##      2322222222211  11 1 11  11 1 
##      40867235190985976604547312231
##  PCH .53122..14...................
##  BBO 155244..3421.................
##  BCO .53234..3411.................
##  GRE 255454.135211................
##  ANG .54232..24211..1.............
##  ABL 5555552355532..2.............
##  ROT .52112.12221.2...............
##  BOU .54233..35322..1.............
##  PSO 134233..25211..11............
##  CAR .54123..23111..11............
##  HOT 111113..22221..1.............
##  GAR 255455115555254211...........
##  SPI .51111..23323..41............
##  TAN .54354..4342131112.11........
##  BAR .33245..45423..32...21.......
##  GOU 154345.2554422.1211121.......
##  PER .52114..342134.211.2.........
##  BRO .43243.1352114.111.2.1.1.....
##  TOX .21.12..22233..44............
##  CHE 23423411243132522221311.11...
##  VAN .32123.1232225.3512.3.1......
##  LOC ..1111..11253234554554551432.
##  BLA .........1.11..25...43.....2.
##  VAI ........11133314344545454445.
##  CHA ............1..12...33..12.2.
##  OMB .........1..1..1....24..12.3.
##  TRU .........1..12.23314455535553
##   sites species 
##      29      27
```
## CA using CA() function


```r
spe.CA.PL <- CA(spe)
biplot(spe.CA.PL, cex=1)
```

![](Chap-9-presentation_files/figure-html/unnamed-chunk-10-1.png) 

```r
# Ordering of the data table following the first CA axis
# The table is transposed, as in vegemite() output
summary(spe.CA.PL)
```

```
##               Length Class  Mode     
## total.inertia   1    -none- numeric  
## eigenvalues    26    -none- numeric  
## rel.eigen      26    -none- numeric  
## rel.cum.eigen  26    -none- numeric  
## U             702    -none- numeric  
## Uhat          754    -none- numeric  
## F             754    -none- numeric  
## Fhat          702    -none- numeric  
## V             702    -none- numeric  
## Vhat          754    -none- numeric  
## site.names     29    -none- character
## sp.names       27    -none- character
## color.sites     1    -none- character
## color.sp        1    -none- character
## call            2    -none- call
```

```r
t(spe[order(spe.CA.PL$F[,1]), order(spe.CA.PL$V[,1])])
```

```
##     24 30 28 26 27 22 23 25 21 29 20 19 18 5 9 17 16 6 10 4 15 14 7 3 11
## PCH  0  5  3  1  2  2  0  0  1  4  0  0  0 0 0  0  0 0  0 0  0  0 0 0  0
## BBO  1  5  5  2  4  4  0  0  3  4  2  1  0 0 0  0  0 0  0 0  0  0 0 0  0
## BCO  0  5  3  2  3  4  0  0  3  4  1  1  0 0 0  0  0 0  0 0  0  0 0 0  0
## GRE  2  5  5  4  5  4  0  1  3  5  2  1  1 0 0  0  0 0  0 0  0  0 0 0  0
## ANG  0  5  4  2  3  2  0  0  2  4  2  1  1 0 0  1  0 0  0 0  0  0 0 0  0
## ABL  5  5  5  5  5  5  2  3  5  5  5  3  2 0 0  2  0 0  0 0  0  0 0 0  0
## ROT  0  5  2  1  1  2  0  1  2  2  2  1  0 2 0  0  0 0  0 0  0  0 0 0  0
## BOU  0  5  4  2  3  3  0  0  3  5  3  2  2 0 0  1  0 0  0 0  0  0 0 0  0
## PSO  1  3  4  2  3  3  0  0  2  5  2  1  1 0 0  1  1 0  0 0  0  0 0 0  0
## CAR  0  5  4  1  2  3  0  0  2  3  1  1  1 0 0  1  1 0  0 0  0  0 0 0  0
## HOT  1  1  1  1  1  3  0  0  2  2  2  2  1 0 0  1  0 0  0 0  0  0 0 0  0
## GAR  2  5  5  4  5  5  1  1  5  5  5  5  2 5 4  2  1 1  0 0  0  0 0 0  0
## SPI  0  5  1  1  1  1  0  0  2  3  3  2  3 0 0  4  1 0  0 0  0  0 0 0  0
## TAN  0  5  4  3  5  4  0  0  4  3  4  2  1 3 1  1  1 2  0 1  1  0 0 0  0
## BAR  0  3  3  2  4  5  0  0  4  5  4  2  3 0 0  3  2 0  0 0  2  1 0 0  0
## GOU  1  5  4  3  4  5  0  2  5  5  4  4  2 2 0  1  2 1  1 1  2  1 0 0  0
## PER  0  5  2  1  1  4  0  0  3  4  2  1  3 4 0  2  1 1  0 2  0  0 0 0  0
## BRO  0  4  3  2  4  3  0  1  3  5  2  1  1 4 0  1  1 1  0 2  0  1 0 1  0
## TOX  0  2  1  0  1  2  0  0  2  2  2  3  3 0 0  4  4 0  0 0  0  0 0 0  0
## CHE  2  3  4  2  3  4  1  1  2  4  3  1  3 2 5  2  2 2  2 1  3  1 1 0  1
## VAN  0  3  2  1  2  3  0  1  2  3  2  2  2 5 0  3  5 1  2 0  3  0 1 0  0
## LOC  0  0  1  1  1  1  0  0  1  1  2  5  3 2 3  4  5 5  4 5  5  4 5 5  1
## BLA  0  0  0  0  0  0  0  0  0  1  0  1  1 0 0  2  5 0  0 0  4  3 0 0  0
## VAI  0  0  0  0  0  0  0  0  1  1  1  3  3 3 1  4  3 4  4 5  4  5 4 5  4
## CHA  0  0  0  0  0  0  0  0  0  0  0  0  1 0 0  1  2 0  0 0  3  3 0 0  1
## OMB  0  0  0  0  0  0  0  0  0  1  0  0  1 0 0  1  0 0  0 0  2  4 0 0  1
## TRU  0  0  0  0  0  0  0  0  0  1  0  0  1 2 0  2  3 3  1 4  4  5 5 5  3
##     12 2 13 1
## PCH  0 0  0 0
## BBO  0 0  0 0
## BCO  0 0  0 0
## GRE  0 0  0 0
## ANG  0 0  0 0
## ABL  0 0  0 0
## ROT  0 0  0 0
## BOU  0 0  0 0
## PSO  0 0  0 0
## CAR  0 0  0 0
## HOT  0 0  0 0
## GAR  0 0  0 0
## SPI  0 0  0 0
## TAN  0 0  0 0
## BAR  0 0  0 0
## GOU  0 0  0 0
## PER  0 0  0 0
## BRO  0 0  0 0
## TOX  0 0  0 0
## CHE  1 0  0 0
## VAN  0 0  0 0
## LOC  4 3  2 0
## BLA  0 0  2 0
## VAI  4 4  5 0
## CHA  2 0  2 0
## OMB  2 0  3 0
## TRU  5 5  5 3
```

## NMDS
- the objective is to plot dissimilar objects far apart in the ordination space and similar objects close to one another.
- can use any distance matrix
- can deal with missing data


## NMDS applied to the fish species - Bray-Curtis distance matrix


```r
spe.nmds <- metaMDS(spe, distance="bray")
```

```
## Run 0 stress 0.07477822 
## Run 1 stress 0.1110707 
## Run 2 stress 0.122174 
## Run 3 stress 0.1242998 
## Run 4 stress 0.0737622 
## ... New best solution
## ... procrustes: rmse 0.01939253  max resid 0.09464461 
## Run 5 stress 0.08696388 
## Run 6 stress 0.1222431 
## Run 7 stress 0.08892174 
## Run 8 stress 0.1116308 
## Run 9 stress 0.08841678 
## Run 10 stress 0.08797477 
## Run 11 stress 0.1118887 
## Run 12 stress 0.1104531 
## Run 13 stress 0.1159169 
## Run 14 stress 0.1111187 
## Run 15 stress 0.08841667 
## Run 16 stress 0.07376253 
## ... procrustes: rmse 0.000184464  max resid 0.0008864944 
## *** Solution reached
```

```r
spe.nmds
```

```
## 
## Call:
## metaMDS(comm = spe, distance = "bray") 
## 
## global Multidimensional Scaling using monoMDS
## 
## Data:     spe 
## Distance: bray 
## 
## Dimensions: 2 
## Stress:     0.0737622 
## Stress type 1, weak ties
## Two convergent solutions found after 16 tries
## Scaling: centring, PC rotation, halfchange scaling 
## Species: expanded scores based on 'spe'
```

```r
spe.nmds$stress
```

```
## [1] 0.0737622
```

```r
plot(spe.nmds, type="t", main=paste("NMDS/Bray - Stress =", 
	round(spe.nmds$stress,3)))
```

![](Chap-9-presentation_files/figure-html/unnamed-chunk-11-1.png) 






## Slide with Plot

![](Chap-9-presentation_files/figure-html/unnamed-chunk-12-1.png) 

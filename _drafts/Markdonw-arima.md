---
title: "Arima"
date: 2020-09-13T15:34:30-04:00
categories:
  - blog
tags:
  - Template
toc: true
toc_label: "En ésta página"
related: true
---

## GitHub Documents

This is an R Markdown format used for publishing markdown documents to
GitHub. When you click the **Knit** button all R code chunks are run and
a markdown file (.md) suitable for publishing to GitHub is generated.

## Declarando las Variables

You can include R code in the document as follows:

``` r
library(tseries)
```

``` r
library(readxl)
mydata<- read_excel("C:/Users/malva/OneDrive/intereses/R/Proyectos/ARIMA models/time-series.xlsx")


y<- mydata$ppi
d.y<- diff(y)
t<- mydata$yearqrt
```

## Conociendo la data

### Summary de la data y

``` r
summary(y)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
    ##   25.24   29.66   77.00   64.68   93.27  110.43

``` r
#grafico vs. tiempo

plot(t,y)
```

![](/myweb/assets/images/Markdonw-arima_files/figure-gfm/data%20y-1.png)<!-- -->

### Summary de la data d.y

``` r
summary(d.y)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
    ## -3.2100  0.0300  0.3000  0.4643  0.9625  3.0800

``` r
plot(d.y)
```

![](/myweb/assets/images/Markdonw-arima_files/figure-gfm/datad.y-1.png)<!-- -->

## Buscando estacionalidad

``` r
adf.test(y,alternative="stationary",k=0)
```

    ##
    ##  Augmented Dickey-Fuller Test
    ##
    ## data:  y
    ## Dickey-Fuller = -0.79306, Lag order = 0, p-value = 0.9604
    ## alternative hypothesis: stationary

``` r
adf.test(y,alternative="explosive",k=0)
```

    ##
    ##  Augmented Dickey-Fuller Test
    ##
    ## data:  y
    ## Dickey-Fuller = -0.79306, Lag order = 0, p-value = 0.03964
    ## alternative hypothesis: explosive

## ACF y PACF

``` r
acf(y)
```

![](/myweb/assets/images/Markdonw-arima_files/figure-gfm/acf&pacf-1.png)<!-- -->

``` r
pacf(y)
```

![](/myweb/assets/images/Markdonw-arima_files/figure-gfm/acf&pacf-2.png)<!-- -->

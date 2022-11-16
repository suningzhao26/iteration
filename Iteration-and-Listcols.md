Iteration and Listcols
================
Suning Zhao
2022-11-15

## Lists

``` r
l = list(
vec_numeric = 5:8,
vec_logical = c(TRUE, TRUE, FALSE, TRUE, FALSE, FALSE),
mat = matrix(1:8, nrow = 2, ncol = 4),
summary = summary(rnorm(100))
)
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE  TRUE FALSE  TRUE FALSE FALSE
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -2.06694 -0.65860 -0.11817 -0.06698  0.62089  2.06853

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## ‘for’ loop

Creat a new list.

``` r
list_norm = 
  list(
    a = rnorm(20, mean = 3, sd = 1),
    b = rnorm(20, mean = 0, sd = 5),
    c = rnorm(20, mean = 10, sd = .2),
    d = rnorm(20, mean = 3, sd = 1)
  )
```

``` r
list_norm
```

    ## $a
    ##  [1] 3.818473 1.998151 2.842421 2.460209 3.442681 3.803582 2.609579 1.978859
    ##  [9] 1.486025 2.298646 1.998232 2.816765 4.182767 5.253067 1.960492 2.607239
    ## [17] 2.158208 3.868763 4.108924 2.473577
    ## 
    ## $b
    ##  [1] -3.99795097  9.04847714 12.22863648 -4.26267085  2.14783400 -2.29966194
    ##  [7]  1.26512787  2.48604627  3.07994611 -2.69217800 -6.21010401 -1.76572171
    ## [13]  3.22783703  2.38966862 -7.30031484 -2.88483011 -0.98823709 -2.72652537
    ## [19]  1.82536827  0.08363423
    ## 
    ## $c
    ##  [1]  9.781009  9.640493 10.146864 10.371284  9.988354 10.054494  9.876760
    ##  [8] 10.078160 10.053712 10.054832  9.705873 10.027340 10.127420  9.925560
    ## [15] 10.029546  9.738943 10.221474  9.934492  9.862190 10.103658
    ## 
    ## $d
    ##  [1] 2.591137 2.520766 4.113230 3.769837 3.067122 1.937846 3.540266 3.026895
    ##  [9] 2.665971 4.037224 1.756710 2.342378 2.723681 2.526987 3.203515 4.374960
    ## [17] 2.868695 4.266524 2.805349 4.284907

Pause and get my old function.

``` r
mean_and_sd = function(x){
  if (!is.numeric(x)){
    stop("Input must be numeric")
  }
  if(length(x) < 3){
    stop("Input must have at least three numbers")
  }
  mean_x = mean(x)
  sd_x = sd(x)
  tibble(
    mean = mean_x,
    sd = sd_x
  )
}
```

I can apply that function to each list element.

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.91 0.983

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.133  4.76

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.99 0.181

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.12 0.797

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4){
output[[i]] = mean_and_sd(list_norm[[i]])
}
```

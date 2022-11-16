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
    ## -2.96691 -0.70972  0.04735 -0.07644  0.49533  1.85364

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
    ##  [1] 1.839513 3.842633 2.734568 4.277319 2.922318 3.173530 2.570089 1.575615
    ##  [9] 2.828413 2.473051 4.558448 3.539450 3.206847 2.709835 1.755471 2.749333
    ## [17] 3.510904 3.577486 1.894668 3.238136
    ## 
    ## $b
    ##  [1] -4.48007943 -1.71671667 -2.58351798  9.92774537 -9.95302340  0.02857857
    ##  [7]  0.97224901  2.89334568 -6.63013002  3.35726959  0.73559322 -3.17985388
    ## [13]  0.87117233  7.34288806 -6.25355632 -3.21036530  1.05685201  2.67972994
    ## [19]  1.52383097 -4.58586221
    ## 
    ## $c
    ##  [1]  9.958758 10.265511 10.210699  9.900229 10.308471  9.799592 10.328414
    ##  [8]  9.678810 10.086557 10.294253  9.936622 10.185182 10.072212 10.070662
    ## [15]  9.690759  9.311840 10.283931  9.761526  9.962619  9.886714
    ## 
    ## $d
    ##  [1] 3.326836 3.110652 1.960362 2.387662 1.170706 5.447804 3.407247 2.169500
    ##  [9] 1.766473 2.791772 2.582990 4.652810 1.912548 3.661268 1.029771 2.455094
    ## [17] 3.634373 3.335424 2.502118 6.259459

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
    ## 1  2.95 0.814

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.560  4.77

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.265

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.98  1.33

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4){
output[[i]] = mean_and_sd(list_norm[[i]])
}
```

## Let’s try map!

``` r
output = map(list_norm, mean_and_sd)
```

What if you want a different function..?

``` r
output = map(list_norm, median)
```

``` r
output = map(list_norm, IQR)
```

``` r
output = map_dbl(list_norm, IQR)
```

``` r
output = map_df(list_norm, mean_and_sd, .id = "input")
```

## List columns!

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norm
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>%  pull(samp)
```

    ## $a
    ##  [1] 1.839513 3.842633 2.734568 4.277319 2.922318 3.173530 2.570089 1.575615
    ##  [9] 2.828413 2.473051 4.558448 3.539450 3.206847 2.709835 1.755471 2.749333
    ## [17] 3.510904 3.577486 1.894668 3.238136
    ## 
    ## $b
    ##  [1] -4.48007943 -1.71671667 -2.58351798  9.92774537 -9.95302340  0.02857857
    ##  [7]  0.97224901  2.89334568 -6.63013002  3.35726959  0.73559322 -3.17985388
    ## [13]  0.87117233  7.34288806 -6.25355632 -3.21036530  1.05685201  2.67972994
    ## [19]  1.52383097 -4.58586221
    ## 
    ## $c
    ##  [1]  9.958758 10.265511 10.210699  9.900229 10.308471  9.799592 10.328414
    ##  [8]  9.678810 10.086557 10.294253  9.936622 10.185182 10.072212 10.070662
    ## [15]  9.690759  9.311840 10.283931  9.761526  9.962619  9.886714
    ## 
    ## $d
    ##  [1] 3.326836 3.110652 1.960362 2.387662 1.170706 5.447804 3.407247 2.169500
    ##  [9] 1.766473 2.791772 2.582990 4.652810 1.912548 3.661268 1.029771 2.455094
    ## [17] 3.634373 3.335424 2.502118 6.259459

``` r
listcol_df %>% 
  filter(name == "a")
```

    ## # A tibble: 1 × 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

Let’s try some operations.

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.95 0.814

Can I just map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.95 0.814
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.560  4.77
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.265
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.98  1.33

So … can I add a list column?

``` r
listcol_df = 
listcol_df %>% 
  mutate(
    summary = map(samp, mean_and_sd),
    median = map_dbl(samp, median))
```

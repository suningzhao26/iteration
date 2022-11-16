Iteration
================
Suning Zhao
2022-11-15

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec))/sd(x_vec)
```

    ##  [1] -0.72721168 -0.04654560 -1.38394976 -0.73157315  0.38830750 -0.15577319
    ##  [7]  2.34180518 -0.51700358  0.73051820 -0.77907178  0.98581095 -0.91538855
    ## [13] -1.63718319  0.32654240  0.13352229 -1.35833905  0.03382127 -1.34024368
    ## [19]  1.01749606  1.65458960  1.14743999  0.24622333 -0.43685842  1.07446547
    ## [25]  0.14360384  1.29440233 -0.35333674 -0.97672328  0.76034125 -0.91968799

I want a function to compute z-scores

``` r
z_scores = function(x){
  if (!is.numeric(x)){
    stop("Input must be numeric")
  }
  if(length(x) < 3){
    stop("Input must have at least three numbers")
  }
  z = (x - mean(x))/sd(x)
  return(z)
}

z_scores(x_vec)
```

    ##  [1] -0.72721168 -0.04654560 -1.38394976 -0.73157315  0.38830750 -0.15577319
    ##  [7]  2.34180518 -0.51700358  0.73051820 -0.77907178  0.98581095 -0.91538855
    ## [13] -1.63718319  0.32654240  0.13352229 -1.35833905  0.03382127 -1.34024368
    ## [19]  1.01749606  1.65458960  1.14743999  0.24622333 -0.43685842  1.07446547
    ## [25]  0.14360384  1.29440233 -0.35333674 -0.97672328  0.76034125 -0.91968799

Try my function on some other things.These should give errors

``` r
z_scores(3)
```

    ## Error in z_scores(3): Input must have at least three numbers

``` r
z_scores("my name is jeff")
```

    ## Error in z_scores("my name is jeff"): Input must be numeric

``` r
z_scores(mtcars)
```

    ## Error in z_scores(mtcars): Input must be numeric

``` r
z_scores(c(TRUE,TRUE,FALSE,TRUE))
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)): Input must be numeric

## Multiple outputs

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

Check that function works

``` r
mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.57  2.83

## Multiple Inputs

``` r
sim_data = 
  tibble(
    x = rnorm(n = 100, mean = 4, sd = 3)
  )

sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.37  3.15

``` r
sim_mean_sd = function(samp_size, mu = 3, sigma = 4){
  sim_data = 
  tibble(
    x = rnorm(n = samp_size, mean = mu, sd = sigma)
  )

sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
}

sim_mean_sd(samp_size = 100, mu = 6, sigma = 3)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.01  2.89

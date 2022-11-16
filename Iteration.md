Iteration
================
Suning Zhao
2022-11-15

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec))/sd(x_vec)
```

    ##  [1]  0.001864697  1.208976047 -0.424760103 -0.341547375  0.211977113
    ##  [6]  1.360786486 -1.449695818 -0.628752657  0.927470972  2.071673359
    ## [11] -0.456406959  0.697419704 -2.635775621  0.178033967 -0.133015857
    ## [16] -0.017957812 -1.381274994  1.388225136 -0.744657027  0.856356897
    ## [21]  0.656605394  1.274405635 -0.604729209 -0.134817050  0.019310842
    ## [26]  0.575642414 -0.474456084 -0.689836909 -1.179787018 -0.131278170

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

    ##  [1]  0.001864697  1.208976047 -0.424760103 -0.341547375  0.211977113
    ##  [6]  1.360786486 -1.449695818 -0.628752657  0.927470972  2.071673359
    ## [11] -0.456406959  0.697419704 -2.635775621  0.178033967 -0.133015857
    ## [16] -0.017957812 -1.381274994  1.388225136 -0.744657027  0.856356897
    ## [21]  0.656605394  1.274405635 -0.604729209 -0.134817050  0.019310842
    ## [26]  0.575642414 -0.474456084 -0.689836909 -1.179787018 -0.131278170

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

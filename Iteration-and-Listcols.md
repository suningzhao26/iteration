Iteration and Listcols
================
Suning Zhao
2022-11-16

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
    ## -2.50251 -0.61402 -0.07307  0.06057  0.73577  2.09721

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
    ##  [1] 3.565973 1.643824 4.666910 4.106612 3.727416 2.472584 2.622332 1.564283
    ##  [9] 4.547189 3.090038 2.289840 1.863859 3.747976 1.889763 3.684982 1.771610
    ## [17] 3.432353 3.420300 1.609042 2.762414
    ## 
    ## $b
    ##  [1] -2.86117093 -2.11334558 -1.64884936 -1.69650455 -2.69480567 -1.36107722
    ##  [7]  5.33224269 -8.57204241 -0.80455097 -3.39363193  6.02990901 -5.32151968
    ## [13]  0.09091672  0.46958517 -2.99548823  1.19482213 -0.36269423 -7.46735059
    ## [19] -3.34050419 -3.85293143
    ## 
    ## $c
    ##  [1] 10.264359  9.584183  9.982542  9.810810 10.153425 10.179935  9.964797
    ##  [8] 10.002780  9.958920 10.135333  9.988620  9.873567 10.008221  9.663096
    ## [15] 10.102560  9.988052 10.041501 10.346622 10.134409  9.879929
    ## 
    ## $d
    ##  [1] 3.790529 4.956248 4.189173 2.661878 3.286309 1.787337 2.867279 5.282224
    ##  [9] 2.121930 2.881647 2.153552 2.277365 3.734766 3.841111 3.092265 2.988888
    ## [17] 1.475834 1.408610 3.024641 2.671650

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
    ## 1  2.92  1.01

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -1.77  3.53

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.185

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.02  1.05

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
    ##  [1] 3.565973 1.643824 4.666910 4.106612 3.727416 2.472584 2.622332 1.564283
    ##  [9] 4.547189 3.090038 2.289840 1.863859 3.747976 1.889763 3.684982 1.771610
    ## [17] 3.432353 3.420300 1.609042 2.762414
    ## 
    ## $b
    ##  [1] -2.86117093 -2.11334558 -1.64884936 -1.69650455 -2.69480567 -1.36107722
    ##  [7]  5.33224269 -8.57204241 -0.80455097 -3.39363193  6.02990901 -5.32151968
    ## [13]  0.09091672  0.46958517 -2.99548823  1.19482213 -0.36269423 -7.46735059
    ## [19] -3.34050419 -3.85293143
    ## 
    ## $c
    ##  [1] 10.264359  9.584183  9.982542  9.810810 10.153425 10.179935  9.964797
    ##  [8] 10.002780  9.958920 10.135333  9.988620  9.873567 10.008221  9.663096
    ## [15] 10.102560  9.988052 10.041501 10.346622 10.134409  9.879929
    ## 
    ## $d
    ##  [1] 3.790529 4.956248 4.189173 2.661878 3.286309 1.787337 2.867279 5.282224
    ##  [9] 2.121930 2.881647 2.153552 2.277365 3.734766 3.841111 3.092265 2.988888
    ## [17] 1.475834 1.408610 3.024641 2.671650

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
    ## 1  2.92  1.01

Can I just map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.92  1.01
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -1.77  3.53
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.185
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.02  1.05

So … can I add a list column?

``` r
listcol_df = 
listcol_df %>% 
  mutate(
    summary = map(samp, mean_and_sd),
    median = map_dbl(samp, median))
```

## Weather Data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: C:\Users\SUNING~1\AppData\Local/Cache/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2022-10-12 22:30:14 (8.428)

    ## file min/max dates: 1869-01-01 / 2022-10-31

    ## using cached file: C:\Users\SUNING~1\AppData\Local/Cache/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2022-10-12 22:30:24 (1.703)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: C:\Users\SUNING~1\AppData\Local/Cache/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2022-10-12 22:30:28 (0.954)

    ## file min/max dates: 1999-09-01 / 2022-10-31

Get our list columns..

``` r
weather_nest = 
  weather_df %>% 
  nest(data = date:tmin)
```

``` r
weather_nest %>% pull(name)
```

    ## [1] "CentralPark_NY" "Waikiki_HA"     "Waterhole_WA"

``` r
weather_nest %>% pull(data)
```

    ## [[1]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows
    ## # ℹ Use `print(n = ...)` to see more rows
    ## 
    ## [[2]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # … with 355 more rows
    ## # ℹ Use `print(n = ...)` to see more rows
    ## 
    ## [[3]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # … with 355 more rows
    ## # ℹ Use `print(n = ...)` to see more rows

``` r
weather_nest$data[[1]]
```

    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows
    ## # ℹ Use `print(n = ...)` to see more rows

Suppose i want to regress `tmax` on `tmin` for each station.

This works..

``` r
lm(tmax ~ tmin, data = weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest$data[[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

Let’s write a function.

``` r
weather_lm = function(df){
  lm(tmax ~ tmin, data = df)
}

output = vector("list",3)

for (i in 1:3){
  output[[i]] = weather_lm(weather_nest$data[[i]])
}
```

What about a map…!?

``` r
map(weather_nest$data, weather_lm)
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

What about a map in a list column?

``` r
weather_nest =
weather_nest %>% 
  mutate(
    models = map(data, weather_lm)
    )

weather_nest$models
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

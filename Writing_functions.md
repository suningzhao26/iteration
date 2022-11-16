Iteration
================
Suning Zhao
2022-11-15

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec))/sd(x_vec)
```

    ##  [1] -0.18647535 -1.34628603  0.15259745  0.89745081 -0.44228872 -0.20183683
    ##  [7]  0.74316835  1.94114275 -0.95799706 -0.13194809 -1.59454187  1.03966857
    ## [13]  0.19040938  0.31673823  0.02397247 -1.01685309 -1.71169389  1.66526642
    ## [19] -0.23101763  0.24347801  0.68808511  1.48355536  0.92606588  1.01740136
    ## [25] -0.24502220 -0.96541754  0.83175625 -1.44915240 -1.31932624 -0.36089946

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

    ##  [1] -0.18647535 -1.34628603  0.15259745  0.89745081 -0.44228872 -0.20183683
    ##  [7]  0.74316835  1.94114275 -0.95799706 -0.13194809 -1.59454187  1.03966857
    ## [13]  0.19040938  0.31673823  0.02397247 -1.01685309 -1.71169389  1.66526642
    ## [19] -0.23101763  0.24347801  0.68808511  1.48355536  0.92606588  1.01740136
    ## [25] -0.24502220 -0.96541754  0.83175625 -1.44915240 -1.31932624 -0.36089946

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
    ## 1  6.32  3.05

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
    ## 1  3.64  2.81

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
    ## 1  5.97  3.19

## Let’s review Napoleon Dynamite

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

What about the next page of reviews… Let’s turn that code into a
function

``` r
read_page_reviews = function(url) {
  html = read_html(url)

review_titles = 
  html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text = 
  html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
reviews
}
```

Let me try my function.

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"
read_page_reviews(dynamite_url)
```

    ## # A tibble: 10 × 3
    ##    title                                      stars text                        
    ##    <chr>                                      <dbl> <chr>                       
    ##  1 Classic Movie                                  5 "Love this movie. Everyone …
    ##  2 Goofy movie                                    5 "I used this movie for a mi…
    ##  3 Lol hey it’s Napoleon. What’s not to love…     5 "Vote for Pedro"            
    ##  4 Still the best                                 5 "Completely stupid, absolut…
    ##  5 70’s and 80’s Schtick Comedy                   5 "…especially funny if you h…
    ##  6 Amazon Censorship                              5 "I hope Amazon does not cen…
    ##  7 Watch to say you did                           3 "I know it's supposed to be…
    ##  8 Best Movie Ever!                               5 "We just love this movie an…
    ##  9 Quirky                                         5 "Good family film"          
    ## 10 Funny movie - can't play it !                  1 "Sony 4k player won't even …

Let’s read a few pages of reviews.

``` r
dynamite_url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="

dynamite_urls = str_c(dynamite_url_base, 1:5)

all_reviews =
bind_rows(
read_page_reviews(dynamite_urls[1]),
read_page_reviews(dynamite_urls[2]),
read_page_reviews(dynamite_urls[3]),
read_page_reviews(dynamite_urls[5])
)
```

## Mean scoping example

``` r
f = function(x){
  z = x + y
  z
}

x = 1
y = 2

f(x = y)
```

    ## [1] 4

## Functions as arguments

``` r
my_summary = function(x, summ_func){
  summ_func(x)
  
}

x_vec = rnorm(100, 3, 7)

mean(x_vec)
```

    ## [1] 3.226816

``` r
median(x_vec)
```

    ## [1] 3.520999

``` r
my_summary(x_vec, mean)
```

    ## [1] 3.226816

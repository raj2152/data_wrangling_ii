Reading from Web
================

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.4     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(httr)

knitr::opts_chunk$set(
  fig.width = 6, 
  fig.asp = .6,
  out.width = "90%"
)
```

## NSDUH data

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)

drug_use_html
```

    ## {html_document}
    ## <html lang="en">
    ## [1] <head>\n<link rel="P3Pv1" href="http://www.samhsa.gov/w3c/p3p.xml">\n<tit ...
    ## [2] <body>\r\n\r\n<noscript>\r\n<p>Your browser's Javascript is off. Hyperlin ...

``` r
drug_use_df =
  drug_use_html %>%
  html_table() %>% 
  first() %>% 
  slice(-1)
```

## Star Wars

``` r
sw_url = "https://www.imdb.com/list/ls070150896/"

sw_html = 
  read_html(sw_url)

sw_titles = 
  sw_html %>% 
  html_elements(".lister-item-header a") %>% 
  html_text()

sw_revenue = 
  sw_html %>% 
  html_elements(".text-small:nth-child(7) span:nth-child(5)") %>% 
  html_text()

sw_df = 
  tibble(
    title = sw_titles,
    revenue = sw_revenue)
```

## Napoleon Dynamite

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = 
  read_html(dynamite_url)

dynamite_review_titles =
  dynamite_html %>% 
  html_elements(".a-text-bold span") %>% 
  html_text()


dynamite_stars =
  dynamite_html %>% 
  html_elements("#cm_cr-review_list .review-rating") %>% 
  html_text()

dynamite_df = 
  tibble(
    title = dynamite_review_titles,
    stars = dynamite_stars)
```

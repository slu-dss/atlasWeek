Accessible Data Analysis: Data Exploration
================
Christy Garcia and Chris Prener
(April 11, 2018)

Introduction
------------

This is the second of three notebooks for our ATLAS week session.

Dependencies
------------

The following packages are required for this notebook:

``` r
# tidyverse packages
library(dplyr)  # data wrangling
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(readr)  # data import

# other packages
library(here)    # file paths
```

    ## here() starts at /Users/chris/GitHub/DSS/atlasWeek

``` r
library(janitor) # frequency tables
```

    ## Warning: package 'janitor' was built under R version 3.4.4

``` r
library(skimr)   # descriptive statistics
```

    ## 
    ## Attaching package: 'skimr'

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     contains, ends_with, everything, matches, num_range, one_of,
    ##     starts_with

Loading Data
------------

We need the same two data tables we used in the first notebook for this notebook as well:

``` r
lead <- read_csv(here("data", "stlLead.csv"))
```

    ## Parsed with column specification:
    ## cols(
    ##   geoID = col_double(),
    ##   tractCE = col_double(),
    ##   nameLSAD = col_character(),
    ##   countTested = col_double(),
    ##   pctElevated = col_double(),
    ##   totalPop = col_double(),
    ##   totalPop_MOE = col_double(),
    ##   white = col_double(),
    ##   white_MOE = col_double(),
    ##   black = col_double(),
    ##   black_MOE = col_double(),
    ##   povertyTot = col_double(),
    ##   povertyTot_MOE = col_double(),
    ##   povertyU18 = col_double(),
    ##   povertyU18_MOE = col_double()
    ## )

``` r
murders <- read_csv(here("data", "stlMurders.csv"))
```

    ## Parsed with column specification:
    ## cols(
    ##   id = col_double(),
    ##   fullDate = col_character(),
    ##   year = col_double(),
    ##   month = col_double(),
    ##   day = col_double(),
    ##   time = col_double(),
    ##   descrip = col_character(),
    ##   district = col_double(),
    ##   nhood = col_double(),
    ##   address = col_double(),
    ##   street = col_character()
    ## )

Exploring Data
--------------

### Review

In the first notebook, we used the `glimpse()` function from `dplyr` to get a list of all variables present in our data frame:

``` r
glimpse(murders)
```

    ## Observations: 1,394
    ## Variables: 11
    ## $ id       <dbl> 1988, 2712, 3276, 3400, 4689, 4722, 5567, 6258, 6295,...
    ## $ fullDate <chr> "12jan2008", "17jan2008", "20jan2008", "21jan2008", "...
    ## $ year     <dbl> 2008, 2008, 2008, 2008, 2008, 2008, 2008, 2008, 2008,...
    ## $ month    <dbl> 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,...
    ## $ day      <dbl> 12, 17, 20, 21, 30, 30, 4, 9, 9, 11, 12, 16, 21, 22, ...
    ## $ time     <dbl> 1237, 400, 2119, 1738, 1534, 1919, 1745, 1730, 2259, ...
    ## $ descrip  <chr> "HOMICIDE", "HOMICIDE", "HOMICIDE", "HOMICIDE", "HOMI...
    ## $ district <dbl> 7, 7, 7, 1, 8, 8, 9, 7, 6, 4, 4, 8, 7, 6, 6, 1, 7, 6,...
    ## $ nhood    <dbl> 50, 48, 50, 16, 0, 55, 29, 48, 71, 61, 36, 55, 50, 68...
    ## $ address  <dbl> 5738, 5356, 5826, 3859, 4100, 2418, 1646, 5617, 5001,...
    ## $ street   <chr> "TERRY AVE", "PAGE BLVD", "ROOSEVELT PL", "OHIO AVE",...

This is an easy way to get some basic information about each variable.

### Descriptive Statistics

The `skimr` package provides some numeric summaries of the data present in a given data frame. The `skim()` function can be used to generate a report that breaks down variables by type, and provides the most relevant descriptive statistics based on that distinction:

``` r
skim(murders)
```

    ## Skim summary statistics
    ##  n obs: 1394 
    ##  n variables: 11 
    ## 
    ## Variable type: character 
    ##  variable missing complete    n min max empty n_unique
    ##   descrip       0     1394 1394   8   8     0        1
    ##  fullDate       0     1394 1394   9   9     0     1053
    ##    street       0     1394 1394   3  30     0      514
    ## 
    ## Variable type: numeric 
    ##  variable missing complete    n      mean        sd   p0       p25
    ##   address      15     1379 1394   3826.45   1951.67    0   2428   
    ##       day       0     1394 1394     15.71      8.75    1      8   
    ##  district       0     1394 1394      5         1.99    0      4   
    ##        id       0     1394 1394 269215.58 146364.73 1988 140206.5 
    ##     month       0     1394 1394      6.78      3.33    1      4   
    ##     nhood       0     1394 1394     51.43     21.35    0     37   
    ##      time       0     1394 1394   1382.94    794.31    1    620.75
    ##      year       0     1394 1394   2012.2       2.73 2008   2010   
    ##    median   p75   p100     hist
    ##    4029    5038  10900 ▃▃▆▇▃▁▁▁
    ##      16      23     31 ▇▆▇▆▆▇▆▆
    ##       5       6      9 ▃▁▃▃▆▇▃▂
    ##  283341.5 4e+05 489370 ▅▅▅▅▅▅▇▇
    ##       7      10     12 ▆▃▇▅▃▇▃▇
    ##      56      69     85 ▂▃▁▂▅▆▇▂
    ##    1640    2100   2359 ▆▂▂▂▃▃▆▇
    ##    2012    2015   2016 ▇▃▃▃▃▅▅▅

Now you try generating some descriptive statistics for the `lead` data:

``` r
skim(lead)
```

    ## Skim summary statistics
    ##  n obs: 106 
    ##  n variables: 15 
    ## 
    ## Variable type: character 
    ##  variable missing complete   n min max empty n_unique
    ##  nameLSAD       0      106 106  17  20     0      106
    ## 
    ## Variable type: numeric 
    ##        variable missing complete   n      mean      sd    p0       p25
    ##           black       0      106 106   1429.87 1038.31    35    633.25
    ##       black_MOE       0      106 106    288.38  148.84    38    183.25
    ##     countTested       0      106 106    737.37  414.18    23    411   
    ##           geoID       0      106 106  3e+10    8132.85 3e+10  3e+10   
    ##     pctElevated       0      106 106     10.16    6.01     0      4.59
    ##      povertyTot       0      106 106    786.94  528.61   158    408.5 
    ##  povertyTot_MOE       0      106 106    270.5   156.11    65    160.25
    ##      povertyU18       0      106 106    249.75  262.6      0     66.5 
    ##  povertyU18_MOE       0      106 106    140.08  107.48     3     61.25
    ##        totalPop       0      106 106   2998.58 1263.14   620   2025   
    ##    totalPop_MOE       0      106 106    353.35  140.3     94    249.5 
    ##         tractCE       0      106 106 113385.92 8132.85 1e+05 106425   
    ##           white       0      106 106   1371.72 1293.4      1     88.5 
    ##       white_MOE       0      106 106    201.38  145.21     4     60.5 
    ##     median       p75      p100     hist
    ##    1334.5    2029      4572    ▇▇▅▅▃▂▁▁
    ##     269       379.25    760    ▅▆▇▆▅▁▁▁
    ##     694       923.25   2116    ▃▇▆▆▃▁▂▁
    ##   3e+10     3e+10     3e+10    ▇▇▅▆▇▃▃▇
    ##       9.48     14.38     23.28 ▂▇▅▆▃▂▃▂
    ##     651.5     955.75   2801    ▇▇▅▂▁▁▁▁
    ##     241.5     337.5    1049    ▇▇▅▁▁▁▁▁
    ##     173.5     341.75   1320    ▇▃▂▁▁▁▁▁
    ##     120.5     194.75    595    ▇▇▃▂▁▁▁▁
    ##    2911.5    3784.5    7069    ▃▇▇▇▅▂▁▁
    ##     333.5     439.5     832    ▂▇▆▅▃▂▁▁
    ##  112350    119101.75 127600    ▇▇▅▆▇▃▃▇
    ##    1290      2168.25   6128    ▇▂▃▂▁▁▁▁
    ##     212.5     299.75    751    ▇▃▇▃▂▁▁▁

### Pipes

All of the `tidyverse` packages and other associated packages, like `janitor` (introduced below), use the pipe operator `%>%`. The advantage of using the pipe is that it makes the code easier to read by "piping" a series of functions together in a way that can be read by a paragraph:

1.  We take the `murders` data, **then**
2.  we create a new variable named `northSide`, **then**
3.  we create a new variable named `greaterVille`, and
4.  assign the results back onto the `murders` data frame.

``` r
murders %>%
  mutate(northSide = ifelse(nhood > 45, "North City", "South City")) %>%
  mutate(greaterVille = ifelse(nhood >= 50 & nhood <= 59, TRUE, FALSE)) -> murders
```

Pipes don't necessarily make our code more efficient, but they make it more human readable.

### Frequency Tables

The `janitor` package makes it easy to create frequency tables for string and factor data. We use the `tabyl()` function to accomplish this. At its simplest, `tably()` takes just the variable name when included at the end of a pipe:

``` r
murders %>%
  tabyl(year)
```

    ##  year   n    percent
    ##  2008 168 0.12051650
    ##  2009 153 0.10975610
    ##  2010 145 0.10401722
    ##  2011 122 0.08751793
    ##  2012 124 0.08895265
    ##  2013 129 0.09253945
    ##  2014 166 0.11908178
    ##  2015 192 0.13773314
    ##  2016 195 0.13988522

We can add "adornments", including a total row at the bottom and well-formatted percent symbols:

``` r
murders %>%
  tabyl(year) %>%
  adorn_totals() %>%
  adorn_pct_formatting()
```

    ##   year    n percent
    ##   2008  168   12.1%
    ##   2009  153   11.0%
    ##   2010  145   10.4%
    ##   2011  122    8.8%
    ##   2012  124    8.9%
    ##   2013  129    9.3%
    ##   2014  166   11.9%
    ##   2015  192   13.8%
    ##   2016  195   14.0%
    ##  Total 1394  100.0%

Now its your turn. Create a frequency table of the `northSide` variable using pipes and `janitor`:

``` r
murders %>%
  tabyl(northSide) %>%
  adorn_totals() %>%
  adorn_pct_formatting()
```

    ##   northSide    n percent
    ##  North City 1016   72.9%
    ##  South City  378   27.1%
    ##       Total 1394  100.0%

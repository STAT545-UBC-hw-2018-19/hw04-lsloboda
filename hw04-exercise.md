hw04-exercise
================

Using dplyr, ggplot2 to explore data
====================================

Initialize the data
-------------------

-   Load the gapminder, tidyverse and knitr libraries:

``` r
suppressPackageStartupMessages(library(tidyverse))
suppressPackageStartupMessages(library(gapminder))
suppressPackageStartupMessages(library(knitr))
```

-   The knitr library is useful for improving the appearance of tables using the kable() command, described in more detail [here](https://www.rdocumentation.org/packages/knitr/versions/1.20/topics/kable).
-   We will take a quick look at the data to *sanity check* that the data and variables appear as we expect:

``` r
(head(gapminder))
```

    ## # A tibble: 6 x 6
    ##   country     continent  year lifeExp      pop gdpPercap
    ##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ## 1 Afghanistan Asia       1952    28.8  8425333      779.
    ## 2 Afghanistan Asia       1957    30.3  9240934      821.
    ## 3 Afghanistan Asia       1962    32.0 10267083      853.
    ## 4 Afghanistan Asia       1967    34.0 11537966      836.
    ## 5 Afghanistan Asia       1972    36.1 13079460      740.
    ## 6 Afghanistan Asia       1977    38.4 14880372      786.

-   Everything looks as expected, so let's start exploring the data

Task 1
------

*Get the max and min GDP per capita for all continents.*

### Method

-   Reduce the data subset using select and group\_by
-   Find the minimum and maximum values

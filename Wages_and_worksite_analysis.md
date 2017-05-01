Lowest prevailing wage for which h1b was filed each year
========================================================

    library(dplyr)

    ## Warning: package 'dplyr' was built under R version 3.3.1

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    Lowest <-
    h1b %>%
    group_by(YEAR) %>%
    summarize(LOWEST_PREVAILING_WAGE = min(PREVAILING_WAGE, na.rm = TRUE))
    Lowest

    ## # A tibble: 6 x 2
    ##    YEAR LOWEST_PREVAILING_WAGE
    ##   <int>                  <dbl>
    ## 1  2011                      0
    ## 2  2012                      0
    ## 3  2013                   7799
    ## 4  2014                      0
    ## 5  2015                      0
    ## 6  2016                      0

Notes: We see that the lowest prevailing wage for most years is $0. This
could be because of incorrectly populated data. We will now eliminate
the zeroes and see if this changes.

    Lowest <-
    h1b %>%
    group_by(YEAR) %>%
    summarize(LOWEST_PREVAILING_WAGE = min(PREVAILING_WAGE, na.rm = TRUE))
    Lowest

    ## # A tibble: 6 x 2
    ##    YEAR LOWEST_PREVAILING_WAGE
    ##   <int>                  <dbl>
    ## 1  2011                      0
    ## 2  2012                      0
    ## 3  2013                   7799
    ## 4  2014                      0
    ## 5  2015                      0
    ## 6  2016                      0

Notes: We again notice that the PREVAILING\_WAGE values are lower than
the acceptable rates for filing H1Bs. To explain this odd data, let us
look at what are the job titles of the employees with this prevailing
wage

    LowestWithout0 <-
    subset(h1b,!(PREVAILING_WAGE == 0)) %>%
    group_by(YEAR) %>%
    slice(which.min(PREVAILING_WAGE))
    LowestWithout0 [,c('YEAR','PREVAILING_WAGE','JOB_TITLE')]

    ## Source: local data frame [6 x 3]
    ## Groups: YEAR [6]
    ## 
    ##    YEAR PREVAILING_WAGE              JOB_TITLE
    ##   <int>           <dbl>                 <fctr>
    ## 1  2011           63.45     PROGRAMMER ANALYST
    ## 2  2012           15.16         SOUND ENGINEER
    ## 3  2013         7799.00    TELEVISION ENGINEER
    ## 4  2014        10899.00 ASSISTANT SOCCER COACH
    ## 5  2015           52.00                   ADSF
    ## 6  2016           35.00      CULTURAL DIRECTOR

Notes: For the year 2015, the lowest PREVAILING\_WAGE was for a sales
position. This can be explained by the fact that the person would have
been paid on a commission basis and that wouldn't reflect on paper.
Extending the reasoning to other years as well, we can say that most of
these jobs paid on a commission/on-delivery basis, that wouldn't be
reflected under PREVAILING\_WAGE.

What should be the next approach? We cannot use merely a single
PREVAILING\_WAGE. We will have to group these jobs into categories (such
as tech, sales, others) to better understand how wages impact h1b
petitions.

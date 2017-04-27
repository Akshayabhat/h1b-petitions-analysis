Read file into dataframe:
=========================

    h1b_initial <- read.csv("h1b_kaggle.csv")

Notes: There are 3002458 rows and 11 variables

Variables in the dataset and their types:
=========================================

    str(h1b_initial)

    ## 'data.frame':    3002458 obs. of  11 variables:
    ##  $ X                 : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ CASE_STATUS       : Factor w/ 7 levels "CERTIFIED","CERTIFIED-WITHDRAWN",..: 2 2 2 2 7 2 2 2 2 7 ...
    ##  $ EMPLOYER_NAME     : Factor w/ 236014 levels "'K' LINE LOGISTICS USA INC",..: 219680 86104 163491 81046 158424 33454 32942 84992 70183 121781 ...
    ##  $ SOC_NAME          : Factor w/ 2133 levels "<FONT><FONT>CARPINTEROS</FONT></FONT>",..: 177 276 276 276 276 276 276 276 276 276 ...
    ##  $ JOB_TITLE         : Factor w/ 287550 levels "'ACCOUNTANT",..: 163571 43293 43368 187285 164917 88219 43293 43336 164842 164842 ...
    ##  $ FULL_TIME_POSITION: Factor w/ 2 levels "N","Y": 1 2 2 2 2 2 2 2 2 2 ...
    ##  $ PREVAILING_WAGE   : num  36067 242674 193066 220314 157518 ...
    ##  $ YEAR              : int  2016 2016 2016 2016 2016 2016 2016 2016 2016 2016 ...
    ##  $ WORKSITE          : Factor w/ 18622 levels "# 19100 DIV CD 19124 PLANO, TEXAS",..: 1006 13056 8200 4528 15700 10372 7743 14519 10181 17217 ...
    ##  $ lon               : num  -83.7 -96.7 -74.1 -105 -90.2 ...
    ##  $ lat               : num  42.3 33 40.7 39.7 38.6 ...

Examining the first few rows:
=============================

    head (h1b_initial)

    ##   X         CASE_STATUS
    ## 1 1 CERTIFIED-WITHDRAWN
    ## 2 2 CERTIFIED-WITHDRAWN
    ## 3 3 CERTIFIED-WITHDRAWN
    ## 4 4 CERTIFIED-WITHDRAWN
    ## 5 5           WITHDRAWN
    ## 6 6 CERTIFIED-WITHDRAWN
    ##                                                 EMPLOYER_NAME
    ## 1                                      UNIVERSITY OF MICHIGAN
    ## 2                                      GOODMAN NETWORKS, INC.
    ## 3                                   PORTS AMERICA GROUP, INC.
    ## 4 GATES CORPORATION, A WHOLLY-OWNED SUBSIDIARY OF TOMKINS PLC
    ## 5                                   PEABODY INVESTMENTS CORP.
    ## 6                                     BURGER KING CORPORATION
    ##                        SOC_NAME
    ## 1 BIOCHEMISTS AND BIOPHYSICISTS
    ## 2              CHIEF EXECUTIVES
    ## 3              CHIEF EXECUTIVES
    ## 4              CHIEF EXECUTIVES
    ## 5              CHIEF EXECUTIVES
    ## 6              CHIEF EXECUTIVES
    ##                                                      JOB_TITLE
    ## 1                                 POSTDOCTORAL RESEARCH FELLOW
    ## 2                                      CHIEF OPERATING OFFICER
    ## 3                                        CHIEF PROCESS OFFICER
    ## 4                                  REGIONAL PRESIDEN, AMERICAS
    ## 5                                 PRESIDENT MONGOLIA AND INDIA
    ## 6 EXECUTIVE V P, GLOBAL DEVELOPMENT AND PRESIDENT, LATIN AMERI
    ##   FULL_TIME_POSITION PREVAILING_WAGE YEAR                WORKSITE
    ## 1                  N         36067.0 2016     ANN ARBOR, MICHIGAN
    ## 2                  Y        242674.0 2016            PLANO, TEXAS
    ## 3                  Y        193066.0 2016 JERSEY CITY, NEW JERSEY
    ## 4                  Y        220314.0 2016        DENVER, COLORADO
    ## 5                  Y        157518.4 2016     ST. LOUIS, MISSOURI
    ## 6                  Y        225000.0 2016          MIAMI, FLORIDA
    ##          lon      lat
    ## 1  -83.74304 42.28083
    ## 2  -96.69889 33.01984
    ## 3  -74.07764 40.72816
    ## 4 -104.99025 39.73924
    ## 5  -90.19940 38.62700
    ## 6  -80.19179 25.76168

Count of missing values:
========================

    sapply(h1b_initial, function(x) sum(is.na(x)))

    ##                  X        CASE_STATUS      EMPLOYER_NAME 
    ##                  0                 13                 45 
    ##           SOC_NAME          JOB_TITLE FULL_TIME_POSITION 
    ##              17733                 38                 15 
    ##    PREVAILING_WAGE               YEAR           WORKSITE 
    ##                 85                 13                  0 
    ##                lon                lat 
    ##             107242             107242

Notes: As we see, latitude and longitude columns have many missing
values. On the outset, we might not need these values for our analysis
as we have the worksite to identify the location. We will not worry
about this for now. We also notice that a lot of employees don't have a
SOCName aka SOCCode for their positions.13 of the case statuses are
missing. Let us look at these to understand these cases better.

    subset(h1b_initial, is.na(CASE_STATUS))

    ##               X CASE_STATUS EMPLOYER_NAME SOC_NAME JOB_TITLE
    ## 3002446 3002446        <NA>          <NA>     <NA>      <NA>
    ## 3002447 3002447        <NA>          <NA>     <NA>      <NA>
    ## 3002448 3002448        <NA>          <NA>     <NA>      <NA>
    ## 3002449 3002449        <NA>          <NA>     <NA>      <NA>
    ## 3002450 3002450        <NA>          <NA>     <NA>      <NA>
    ## 3002451 3002451        <NA>          <NA>     <NA>      <NA>
    ## 3002452 3002452        <NA>          <NA>     <NA>      <NA>
    ## 3002453 3002453        <NA>          <NA>     <NA>      <NA>
    ## 3002454 3002454        <NA>          <NA>     <NA>      <NA>
    ## 3002455 3002455        <NA>          <NA>     <NA>      <NA>
    ## 3002456 3002456        <NA>          <NA>     <NA>      <NA>
    ## 3002457 3002457        <NA>          <NA>     <NA>      <NA>
    ## 3002458 3002458        <NA>          <NA>     <NA>      <NA>
    ##         FULL_TIME_POSITION PREVAILING_WAGE YEAR
    ## 3002446               <NA>              NA   NA
    ## 3002447               <NA>              NA   NA
    ## 3002448               <NA>              NA   NA
    ## 3002449               <NA>              NA   NA
    ## 3002450               <NA>              NA   NA
    ## 3002451               <NA>              NA   NA
    ## 3002452               <NA>              NA   NA
    ## 3002453               <NA>              NA   NA
    ## 3002454               <NA>              NA   NA
    ## 3002455               <NA>              NA   NA
    ## 3002456               <NA>              NA   NA
    ## 3002457               <NA>              NA   NA
    ## 3002458               <NA>              NA   NA
    ##                            WORKSITE        lon      lat
    ## 3002446 BERKLEY HEIGHTS, NEW JERSEY  -74.43105 40.68087
    ## 3002447     SCHENECTADYÂ , NEW YORK  -73.93957 42.81424
    ## 3002448    MOUTAIN VIEW, CALIFORNIA -122.08385 37.38605
    ## 3002449          ST.PAUL, MINNESOTA  -93.08996 44.95370
    ## 3002450      NEW TOWN, PENNSYLVANIA  -74.93226 40.22834
    ## 3002451      WESTMINISTER, COLORADO -105.03720 39.83665
    ## 3002452        FREEMONT, CALIFORNIA -121.98857 37.54827
    ## 3002453         LAVERGNE, TENNESSEE  -86.58194 36.01562
    ## 3002454               NYC, NEW YORK  -74.00594 40.71278
    ## 3002455           SOUTH LAKE, TEXAS  -97.13418 32.94124
    ## 3002456         CLINTON, NEW JERSEY  -74.90989 40.63677
    ## 3002457       OWINGS MILL, MARYLAND  -76.78025 39.41955
    ## 3002458            ALTANTA, GEORGIA  -84.38798 33.74900

Notes: We see that all other columns, except for the worksite, have
missing values. These records will not help us in our analysis. Hence,
we can safely drop these.

    h1b <- subset(h1b_initial, !is.na(CASE_STATUS))

Notes: We will use dataframe h1b for further analysis

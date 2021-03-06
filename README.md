riem
====

[![CRAN\_Status\_Badge](http://www.r-pkg.org/badges/version/riem)](http://cran.r-project.org/package=riem) [![Travis-CI Build Status](https://travis-ci.org/ropensci/riem.svg?branch=master)](https://travis-ci.org/ropensci/riem) [![Build status](https://ci.appveyor.com/api/projects/status/jl8sxr77bi8jnqrm?svg=true)](https://ci.appveyor.com/project/ropensci/riem) [![codecov](https://codecov.io/gh/ropensci/riem/branch/master/graph/badge.svg)](https://codecov.io/gh/ropensci/riem)
[![](https://badges.ropensci.org/39_status.svg)](https://github.com/ropensci/onboarding/issues/39)

This package allows to get weather data from ASOS stations (airports) via the awesome website of the [Iowa Environment Mesonet](https://mesonet.agron.iastate.edu/request/download.phtml?network=IN__ASOS).

Installation
============

Install the package with:

``` r
install.packages("riem")
```

Or install the development version using [devtools](https://github.com/hadley/devtools) with:

``` r
library("devtools")
install_github("ropensci/riem")
```

Get available networks
======================

``` r
library("riem")
riem_networks() 
```

    ## # A tibble: 266 × 2
    ##        code                      name
    ##       <chr>                     <chr>
    ## 1  AE__ASOS United Arab Emirates ASOS
    ## 2  AF__ASOS          Afghanistan ASOS
    ## 3  AG__ASOS  Antigua and Barbuda ASOS
    ## 4  AI__ASOS             Anguilla ASOS
    ## 5   AK_ASOS               Alaska ASOS
    ## 6   AL_ASOS              Alabama ASOS
    ## 7  AL__ASOS              Albania ASOS
    ## 8  AM__ASOS              Armenia ASOS
    ## 9  AN__ASOS Netherlands Antilles ASOS
    ## 10 AO__ASOS               Angola ASOS
    ## # ... with 256 more rows

Get available stations for one network
======================================

``` r
riem_stations(network = "IN__ASOS") 
```

    ## # A tibble: 117 × 4
    ##       id                   name      lon      lat
    ##    <chr>                  <chr>    <dbl>    <dbl>
    ## 1   VEAT       AGARTALA         91.24045 23.88698
    ## 2   VIAG       AGRA (IN-AFB)    77.96089 27.15583
    ## 3   VAAH       AHMADABAD        72.63465 23.07724
    ## 4   VAAK       AKOLA AIRPORT    77.05863 20.69901
    ## 5   VIAH       ALIGARH          78.06667 27.88333
    ## 6   VIAL       ALLAHABAD (IN-AF 81.73387 25.44006
    ## 7   VIAR       AMRITSAR         74.86667 31.63333
    ## 8   VAOR                Arkonam 79.69120 13.07120
    ## 9   VOAR                Arkonam 79.69120 13.07120
    ## 10  VAAU Aurangabad Chikalthan  75.39810 19.86270
    ## # ... with 107 more rows

Get measures for one station
============================

Possible variables are (copied from [here](https://mesonet.agron.iastate.edu/request/download.phtml), see also the [ASOS user guide](http://www.nws.noaa.gov/asos/pdfs/aum-toc.pdf))

-   station: three or four character site identifier

-   valid: timestamp of the observation (UTC)

-   tmpf: Air Temperature in Fahrenheit, typically @ 2 meters

-   dwpf: Dew Point Temperature in Fahrenheit, typically @ 2 meters

-   relh: Relative Humidity in %

-   drct: Wind Direction in degrees from north

-   sknt: Wind Speed in knots

-   p01i: One hour precipitation for the period from the observation time to the time of the previous hourly precipitation reset. This varies slightly by site. Values are in inches. This value may or may not contain frozen precipitation melted by some device on the sensor or estimated by some other means. Unfortunately, we do not know of an authoritative database denoting which station has which sensor.

-   alti: Pressure altimeter in inches

-   mslp: Sea Level Pressure in millibar

-   vsby: Visibility in miles

-   gust: Wind Gust in knots

-   skyc1: Sky Level 1 Coverage

-   skyc2: Sky Level 2 Coverage

-   skyc3: Sky Level 3 Coverage

-   skyc4: Sky Level 4 Coverage

-   skyl1: Sky Level 1 Altitude in feet

-   skyl2: Sky Level 2 Altitude in feet

-   skyl3: Sky Level 3 Altitude in feet

-   skyl4: Sky Level 4 Altitude in feet

-   presentwx: Present Weather Codes (space seperated), see e.g. [this manual](http://www.ofcm.gov/fmh-1/pdf/H-CH8.pdf) for further explanations.

-   metar: unprocessed reported observation in METAR format

``` r
measures <- riem_measures(station = "VOHY", date_start = "2000-01-01", date_end = "2016-04-22") 
knitr::kable(head(measures))
```

| station | valid               |      lon|      lat|  tmpf|  dwpf|   relh|  drct|  sknt|  p01i|   alti| mslp |  vsby|  gust| skyc1 | skyc2 | skyc3 | skyc4 |  skyl1|  skyl2|  skyl3|  skyl4| presentwx | metar                                                        |
|:--------|:--------------------|--------:|--------:|-----:|-----:|------:|-----:|-----:|-----:|------:|:-----|-----:|-----:|:------|:------|:------|:------|------:|------:|------:|------:|:----------|:-------------------------------------------------------------|
| VOHY    | 2011-08-23 00:40:00 |  78.4676|  17.4531|  73.4|  69.8|  88.51|     0|     0|    NA|  29.83| NA   |  3.11|    NA| SCT   | BKN   |       |       |   1000|  20000|     NA|     NA| HZ        | VOHY 230040Z 00000KT 5000 HZ SCT010 BKN200 23/21 Q1010 NOSIG |
| VOHY    | 2011-08-23 01:40:00 |  78.4676|  17.4531|  73.4|  69.8|  88.51|     0|     0|    NA|  29.83| NA   |  3.11|    NA| SCT   | BKN   |       |       |   2000|  20000|     NA|     NA| HZ        | VOHY 230140Z 00000KT 5000 HZ SCT020 BKN200 23/21 Q1010 NOSIG |
| VOHY    | 2011-08-23 05:10:00 |  78.4676|  17.4531|  82.4|  68.0|  61.81|   270|     7|    NA|  29.85| NA   |  3.73|    NA| SCT   | SCT   |       |       |   1500|   2500|     NA|     NA| NA        | VOHY 230510Z 27007KT 6000 SCT015 SCT025 28/20 Q1011 NOSIG    |
| VOHY    | 2011-08-23 05:40:00 |  78.4676|  17.4531|  84.2|  66.2|  54.80|   270|     9|    NA|  29.83| NA   |  3.73|    NA| SCT   | SCT   |       |       |   1500|   2500|     NA|     NA| NA        | VOHY 230540Z 27009KT 6000 SCT015 SCT025 29/19 Q1010 NOSIG    |
| VOHY    | 2011-08-23 06:40:00 |  78.4676|  17.4531|  84.2|  68.0|  58.32|   260|     5|    NA|  29.83| NA   |  3.73|    NA| SCT   | SCT   |       |       |   1500|   2500|     NA|     NA| NA        | VOHY 230640Z 26005KT 6000 SCT015 SCT025 29/20 Q1010 NOSIG    |
| VOHY    | 2011-08-23 07:40:00 |  78.4676|  17.4531|  84.2|  66.2|  54.80|   250|     7|    NA|  29.77| NA   |  3.73|    NA| SCT   | SCT   |       |       |   2000|   2500|     NA|     NA| NA        | VOHY 230740Z 25007KT 6000 SCT020 SCT025 29/19 Q1008 NOSIG    |

For conversion of wind speed or temperature into other units, see [this package](https://github.com/geanders/weathermetrics/).

Meta
----

-   Please [report any issues or bugs](https://github.com/ropenscilabs/riem/issues).
-   License: GPL
-   Get citation information for `ropenaq` in R doing `citation(package = 'riem')`
-   Please note that this project is released with a [Contributor Code of Conduct](CONDUCT.md). By participating in this project you agree to abide by its terms.

[![ropensci\_footer](http://ropensci.org/public_images/github_footer.png)](http://ropensci.org)

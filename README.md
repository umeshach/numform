numform   [![Follow](https://img.shields.io/twitter/follow/tylerrinker.svg?style=social)](https://twitter.com/intent/follow?screen_name=tylerrinker)
============


[![Build
Status](https://travis-ci.org/trinker/numform.svg?branch=master)](https://travis-ci.org/trinker/numform)
[![Coverage
Status](https://coveralls.io/repos/trinker/numform/badge.svg?branch=master)](https://coveralls.io/r/trinker/numform?branch=master)
<a href="https://img.shields.io/badge/Version-0.0.1-orange.svg"><img src="https://img.shields.io/badge/Version-0.0.1-orange.svg" alt="Version"/></a>
</p>

**numform** contains tools to assist in the formatting of numbers for
publication. Tools include the removal of leading zeros, standardization
of number of digits, and a p-value formatter.


Table of Contents
============

-   [Installation](#installation)
-   [Contact](#contact)
-   [Available Functions](#available-functions)
-   [Demonstration](#demonstration)
    -   [Load Packages](#load-packages)
    -   [Numbers](#numbers)
    -   [Abbreviated Numbers](#abbreviated-numbers)
    -   [Commas](#commas)
    -   [Percents](#percents)
    -   [Dollars](#dollars)
    -   [Tables](#tables)
    -   [Plotting](#plotting)
    -   [Modeling](#modeling)

Installation
============


To download the development version of **numform**:

Download the [zip
ball](https://github.com/trinker/numform/zipball/master) or [tar
ball](https://github.com/trinker/numform/tarball/master), decompress and
run `R CMD INSTALL` on it, or use the **pacman** package to install the
development version:

    if (!require("pacman")) install.packages("pacman")
    pacman::p_load_current_gh("trinker/numform")
    pacman::p_load(tidyverse)

Contact
=======

You are welcome to:    
- submit suggestions and bug-reports at: <https://github.com/trinker/numform/issues>    
- send a pull request on: <https://github.com/trinker/numform/>    
- compose a friendly e-mail to: <tyler.rinker@gmail.com>    


Available Functions
===================

Below is a table of available **numform** functions. Note that `f_` is
read as "format" whereas `fv_` is read as "format vector". The former
formats individual values in the vector while the latter uses the vector
to compute a calculation on each of the values and then formats them.
Additionally, all **numform** non-methods functions have a functional
return version that is prefixed with an additional `f`. For example,
`f_num` has `ff_num` which has the same arguments but returns a function
instead. This is useful for passing in to **ggplot2** `scale_x/y_type`
functions (see [Plotting](#plotting) for usage).

<!-- html table generated in R 3.3.0 by xtable 1.8-2 package -->

<!-- Wed Mar 22 20:45:20 2017 -->

<table>

<tr>

<td>

as_factor
</td>

<td>

f_mean_sd
</td>

<td>

f_ordinal
</td>

<td>

f_prop2percent
</td>

<td>

f_weekday
</td>

</tr>

<tr>

<td>

f_affix
</td>

<td>

f_mills
</td>

<td>

f_pad_zero
</td>

<td>

f_pval
</td>

<td>

fv_num_percent
</td>

</tr>

<tr>

<td>

f_bills
</td>

<td>

f_month
</td>

<td>

f_parenthesis
</td>

<td>

f_sign
</td>

<td>

fv_percent
</td>

</tr>

<tr>

<td>

f_comma
</td>

<td>

f_num
</td>

<td>

f_percent
</td>

<td>

f_suffix
</td>

<td>

fv_percent_diff
</td>

</tr>

<tr>

<td>

f_dollar
</td>

<td>

f_num_percent
</td>

<td>

f_prefix
</td>

<td>

f_thous
</td>

<td>

fv_runs
</td>

</tr>

</table>

<p class="caption">

<b><em>Available Formatting Functions</em></b>
</p>

Demonstration
=============

Load Packages
-------------

    if (!require("pacman")) install.packages("pacman")
    pacman::p_load_gh("trinker/numform")
    pacman::p_load(dplyr)

Numbers
-------

    f_num(c(0.0, 0, .2, -00.02, 1.122222, pi, "A"))

    ## Warning in f_num(c(0, 0, 0.2, -0.02, 1.122222, pi, "A")): NAs introduced by
    ## coercion

    ## [1] ".0"  ".0"  ".2"  "-.0" "1.1" "3.1" NA

Abbreviated Numbers
-------------------

    f_thous(1234)

    ## [1] "1K"

    f_thous(12345)

    ## [1] "12K"

    f_thous(123456)

    ## [1] "123K"

    f_mills(1234567)

    ## [1] "1M"

    f_mills(12345678)

    ## [1] "12M"

    f_mills(123456789)

    ## [1] "123M"

    f_bills(1234567891)

    ## [1] "1B"

    f_bills(12345678912)

    ## [1] "12B"

    f_bills(123456789123)

    ## [1] "123B"

Commas
------

    f_comma(c(1234.12345, 1234567890, .000034034, 123000000000, -1234567))

    ## [1] "1,234.123"       "1,234,567,890"   ".000034034"      "123,000,000,000"
    ## [5] "-1,234,567"

Percents
--------

    f_percent(c(30, 33.45, .1), 1)

    ## [1] "30.0%" "33.5%" ".1%"

    f_percent(c(0.0, 0, .2, -00.02, 1.122222, pi))

    ## [1] ".0%"  ".0%"  ".2%"  "-.0%" "1.1%" "3.1%"

    f_prop2percent(c(.30, 1, 1.01, .33, .222, .01))

    ## [1] "30.0%"  "100.0%" "101.0%" "33.0%"  "22.2%"  "1.0%"

Dollars
-------

    f_dollar(c(0, 30, 33.45, .1))

    ## [1] "$0.00"  "$30.00" "$33.45" "$0.10"

    f_dollar(c(0.0, 0, .2, -00.02, 1122222, pi)) %>% 
        f_comma()

    ## [1] "$0.00"         "$0.00"         "$0.20"         "$-.02"        
    ## [5] "$1,122,222.00" "$3.14"

Tables
------

    pacman::p_load(dplyr, pander)

    set.seed(10)
    dat <- data_frame(
        Team = rep(c("West Coast", "East Coast"), each = 4),
        Year = rep(2012:2015, 2),
        YearStart = round(rnorm(8, 2e6, 1e6) + sample(1:10/100, 8, TRUE), 2),
        Won = round(rnorm(8, 4e5, 2e5) + sample(1:10/100, 8, TRUE), 2),
        Lost = round(rnorm(8, 4.4e5, 2e5) + sample(1:10/100, 8, TRUE), 2),
        WinLossRate = Won/Lost,
        PropWon = Won/YearStart,
        PropLost = Lost/YearStart
    )

    thous <- ff_thous(relative = -1, prefix = '$')
    percents <- ff_prop2percent(digits = 0)

    dat %>%
        group_by(Team) %>%
        mutate(ChangeWinLoss = fv_percent_diff(WinLossRate, 0)) %>%
        ungroup() %>%
        mutate_at(vars(Won:Lost), funs(thous)) %>%
        mutate_at(vars(PropWon, PropLost), funs(percents)) %>%
        mutate(
            YearStart = f_mills(YearStart, 1, prefix = '$'),
            Team = fv_runs(Team),
            WinLossRate = f_num(WinLossRate, 1)
        ) %>%
        pander::pander(split.tables = Inf, justify = c('left', rep('right', ncol(dat))))

<table>

<colgroup>

<col width="11%" />

<col width="7%" />

<col width="12%" />

<col width="6%" />

<col width="7%" />

<col width="15%" />

<col width="10%" />

<col width="11%" />

<col width="16%" />

</colgroup>

<thead>

<tr class="header">

<th align="left">Team</th>
<th align="right">Year</th>
<th align="right">YearStart</th>
<th align="right">Won</th>
<th align="right">Lost</th>
<th align="right">WinLossRate</th>
<th align="right">PropWon</th>
<th align="right">PropLost</th>
<th align="right">ChangeWinLoss</th>
</tr>

</thead>

<tbody>

<tr class="odd">

<td align="left">West Coast</td>
<td align="right">2012</td>
<td align="right">$2.0M</td>
<td align="right">$350K</td>
<td align="right">$190K</td>
<td align="right">1.9</td>
<td align="right">17%</td>
<td align="right">9%</td>
<td align="right">0%</td>
</tr>

<tr class="even">

<td align="left"></td>
<td align="right">2013</td>
<td align="right">$1.8M</td>
<td align="right">$600K</td>
<td align="right">$370K</td>
<td align="right">1.6</td>
<td align="right">33%</td>
<td align="right">20%</td>
<td align="right">-13%</td>
</tr>

<tr class="odd">

<td align="left"></td>
<td align="right">2014</td>
<td align="right">$ .6M</td>
<td align="right">$550K</td>
<td align="right">$300K</td>
<td align="right">1.8</td>
<td align="right">87%</td>
<td align="right">48%</td>
<td align="right">11%</td>
</tr>

<tr class="even">

<td align="left"></td>
<td align="right">2015</td>
<td align="right">$1.4M</td>
<td align="right">$420K</td>
<td align="right">$270K</td>
<td align="right">1.6</td>
<td align="right">30%</td>
<td align="right">19%</td>
<td align="right">-13%</td>
</tr>

<tr class="odd">

<td align="left">East Coast</td>
<td align="right">2012</td>
<td align="right">$2.3M</td>
<td align="right">$210K</td>
<td align="right">$420K</td>
<td align="right">.5</td>
<td align="right">9%</td>
<td align="right">18%</td>
<td align="right">0%</td>
</tr>

<tr class="even">

<td align="left"></td>
<td align="right">2013</td>
<td align="right">$2.4M</td>
<td align="right">$360K</td>
<td align="right">$390K</td>
<td align="right">.9</td>
<td align="right">15%</td>
<td align="right">16%</td>
<td align="right">86%</td>
</tr>

<tr class="odd">

<td align="left"></td>
<td align="right">2014</td>
<td align="right">$ .8M</td>
<td align="right">$590K</td>
<td align="right">$ 70K</td>
<td align="right">8.4</td>
<td align="right">74%</td>
<td align="right">9%</td>
<td align="right">811%</td>
</tr>

<tr class="even">

<td align="left"></td>
<td align="right">2015</td>
<td align="right">$1.6M</td>
<td align="right">$500K</td>
<td align="right">$420K</td>
<td align="right">1.2</td>
<td align="right">30%</td>
<td align="right">26%</td>
<td align="right">-86%</td>
</tr>

</tbody>

</table>


Plotting
--------

    library(tidyverse)

    data_frame(
        revenue = rnorm(10000, 500000, 50000),
        date = sample(seq(as.Date('1999/01/01'), as.Date('2000/01/01'), by="day"), 10000, TRUE),
        site = sample(paste("Site", 1:5), 10000, TRUE)
    ) %>%
        mutate(
            dollar = f_dollar(revenue, digits = -3),
            thous = f_thous(revenue),
            thous_dollars = f_thous(revenue, prefix = '$'),
            abb_month = f_month(date),
            abb_week = as_factor(f_weekday(date, distinct = TRUE))
        ) %T>%
        print() %>%
        ggplot(aes(abb_week, revenue)) +
            geom_jitter(width = .2, height = 0, alpha = .2) +
            scale_y_continuous(label = ff_thous(prefix = '$'))+
            facet_wrap(~site) +
            theme_bw()

    ## # A tibble: 10,000 × 8
    ##     revenue       date   site  dollar thous thous_dollars abb_month
    ##       <dbl>     <date>  <chr>   <chr> <chr>         <chr>     <chr>
    ## 1  518104.4 1999-06-09 Site 5 $518000  518K         $518K         J
    ## 2  412045.7 1999-03-11 Site 4 $412000  412K         $412K         M
    ## 3  483772.8 1999-03-27 Site 2 $484000  484K         $484K         M
    ## 4  467421.9 1999-07-04 Site 4 $467000  467K         $467K         J
    ## 5  554327.6 1999-12-30 Site 3 $554000  554K         $554K         D
    ## 6  461872.8 1999-10-16 Site 1 $462000  462K         $462K         O
    ## 7  458566.9 1999-02-11 Site 3 $459000  459K         $459K         F
    ## 8  541723.7 1999-09-02 Site 1 $542000  542K         $542K         S
    ## 9  451617.4 1999-04-16 Site 3 $452000  452K         $452K         A
    ## 10 498559.2 1999-12-26 Site 2 $499000  499K         $499K         D
    ## # ... with 9,990 more rows, and 1 more variables: abb_week <fctr>

![](inst/figure/unnamed-chunk-11-1.png)

Modeling
--------

We can see its use in actual model reporting as well:

    mod1 <- t.test(1:10, y = c(7:20))

    sprintf(
        "t = %s (%s)",
        f_num(mod1$statistic),
        f_pval(mod1$p.value)
    )

    ## [1] "t = -5.4 (p < .05)"

    mod2 <- t.test(1:10, y = c(7:20, 200))

    sprintf(
        "t = %s (%s)",
        f_num(mod2$statistic, 2),
        f_pval(mod2$p.value, digits = 2)
    )

    ## [1] "t = -1.63 (p = .12)"

We can build a function to report model statistics:

    report <- function(mod, stat = NULL, digits = c(0, 2, 2)) {
        
        stat <- if (is.null(stat)) stat <- names(mod[["statistic"]])
        sprintf(
            "%s(%s) = %s, %s", 
            gsub('X-squared', '&Chi;<sup>2</sup>', stat),
            paste(f_num(mod[["parameter"]], digits[1]), collapse = ", "),
            f_num(mod[["statistic"]], digits[2]),
            f_pval(mod[["p.value"]], digits = digits[3])
        )

    }

    report(mod1)

    ## [1] "t(22) = -5.43, p < .05"

    report(oneway.test(count ~ spray, InsectSprays))

    ## [1] "F(5, 30) = 36.07, p < .05"

    report(chisq.test(matrix(c(12, 5, 7, 7), ncol = 2)))

    ## [1] "&Chi;<sup>2</sup>(1) = .64, p = .42"

This enables in-text usage as well. First set up the models in a code
chunk:

    mymod <- oneway.test(count ~ spray, InsectSprays)
    mymod2 <- chisq.test(matrix(c(12, 5, 7, 7), ncol = 2))

And then use <code class="r">`` `r report(mymod)` ``</code> resulting in
a report that looks like this: F(5, 30) = 36.07, p &lt; .05. For
Χ<sup>2</sup> using proper HTML leads to Χ<sup>2</sup>(1) = .64, p =
.42.
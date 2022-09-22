Data Manipulation with ‘dplyr’
================

### Clean Up Column Names

By Using Janitor in Litters and Pups Data

``` r
library(tidyverse)
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.3.6     ✔ purrr   0.3.4
## ✔ tibble  3.1.8     ✔ dplyr   1.0.9
## ✔ tidyr   1.2.0     ✔ stringr 1.4.1
## ✔ readr   2.1.2     ✔ forcats 0.5.2
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()

options(tibble.print_min = 3)

litters_data = read_csv("./data/FAS_litters.csv",
  col_types = "ccddiiii")
litters_data = janitor::clean_names(litters_data)

pups_data = read_csv("./data/FAS_pups.csv",
  col_types = "ciiiii")
pups_data = janitor::clean_names(pups_data)
```

# Select

Select certain columns

``` r
select(litters_data, group, litter_number, gd0_weight, pups_born_alive)
## # A tibble: 49 × 4
##   group litter_number gd0_weight pups_born_alive
##   <chr> <chr>              <dbl>           <int>
## 1 Con7  #85                 19.7               3
## 2 Con7  #1/2/95/2           27                 8
## 3 Con7  #5/5/3/83/3-3       26                 6
## # … with 46 more rows

select(litters_data, group:gd_of_birth)
## # A tibble: 49 × 5
##   group litter_number gd0_weight gd18_weight gd_of_birth
##   <chr> <chr>              <dbl>       <dbl>       <int>
## 1 Con7  #85                 19.7        34.7          20
## 2 Con7  #1/2/95/2           27          42            19
## 3 Con7  #5/5/3/83/3-3       26          41.4          19
## # … with 46 more rows
```

## Remove

Remove columns using select

``` r
select(litters_data, -pups_survive)
## # A tibble: 49 × 7
##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive pups_…¹
##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>   <int>
## 1 Con7  #85                 19.7        34.7          20               3       4
## 2 Con7  #1/2/95/2           27          42            19               8       0
## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6       0
## # … with 46 more rows, and abbreviated variable name ¹​pups_dead_birth

select(litters_data, -pups_survive, -group)
## # A tibble: 49 × 6
##   litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive pups_dead_b…¹
##   <chr>              <dbl>       <dbl>       <int>           <int>         <int>
## 1 #85                 19.7        34.7          20               3             4
## 2 #1/2/95/2           27          42            19               8             0
## 3 #5/5/3/83/3-3       26          41.4          19               6             0
## # … with 46 more rows, and abbreviated variable name ¹​pups_dead_birth
```

## Rename Using Select

New name is on left, old is on right

``` r
select(litters_data, GROUP = group, LiTtEr_NuMbEr = litter_number)
## # A tibble: 49 × 2
##   GROUP LiTtEr_NuMbEr
##   <chr> <chr>        
## 1 Con7  #85          
## 2 Con7  #1/2/95/2    
## 3 Con7  #5/5/3/83/3-3
## # … with 46 more rows
```

## Rename Using Rename

Rename variables using rename function instead of select

``` r
rename(litters_data, GROUP = group, LiTtEr_NuMbEr = litter_number)
## # A tibble: 49 × 8
##   GROUP LiTtEr_NuMbEr gd0_weight gd18_weight gd_of_birth pups_…¹ pups_…² pups_…³
##   <chr> <chr>              <dbl>       <dbl>       <int>   <int>   <int>   <int>
## 1 Con7  #85                 19.7        34.7          20       3       4       3
## 2 Con7  #1/2/95/2           27          42            19       8       0       7
## 3 Con7  #5/5/3/83/3-3       26          41.4          19       6       0       5
## # … with 46 more rows, and abbreviated variable names ¹​pups_born_alive,
## #   ²​pups_dead_birth, ³​pups_survive
```

## Other functions

starts_with(), ends_with(), contains(), everything()

everything() puts every other variable after the ones you state

``` r
select(litters_data, starts_with("gd"))
## # A tibble: 49 × 3
##   gd0_weight gd18_weight gd_of_birth
##        <dbl>       <dbl>       <int>
## 1       19.7        34.7          20
## 2       27          42            19
## 3       26          41.4          19
## # … with 46 more rows

select(litters_data, litter_number, pups_survive, everything())
## # A tibble: 49 × 8
##   litter_number pups_survive group gd0_weight gd18_wei…¹ gd_of…² pups_…³ pups_…⁴
##   <chr>                <int> <chr>      <dbl>      <dbl>   <int>   <int>   <int>
## 1 #85                      3 Con7        19.7       34.7      20       3       4
## 2 #1/2/95/2                7 Con7        27         42        19       8       0
## 3 #5/5/3/83/3-3            5 Con7        26         41.4      19       6       0
## # … with 46 more rows, and abbreviated variable names ¹​gd18_weight,
## #   ²​gd_of_birth, ³​pups_born_alive, ⁴​pups_dead_birth
```

# Relocate

Does a similar thing to select(everything())

``` r
relocate(litters_data, litter_number, pups_survive)
## # A tibble: 49 × 8
##   litter_number pups_survive group gd0_weight gd18_wei…¹ gd_of…² pups_…³ pups_…⁴
##   <chr>                <int> <chr>      <dbl>      <dbl>   <int>   <int>   <int>
## 1 #85                      3 Con7        19.7       34.7      20       3       4
## 2 #1/2/95/2                7 Con7        27         42        19       8       0
## 3 #5/5/3/83/3-3            5 Con7        26         41.4      19       6       0
## # … with 46 more rows, and abbreviated variable names ¹​gd18_weight,
## #   ²​gd_of_birth, ³​pups_born_alive, ⁴​pups_dead_birth
```

# Learning Assesment \#1

``` r
select(pups_data, litter_number, sex, pd_ears)
## # A tibble: 313 × 3
##   litter_number   sex pd_ears
##   <chr>         <int>   <int>
## 1 #85               1       4
## 2 #85               1       4
## 3 #1/2/95/2         1       5
## # … with 310 more rows
```

\###Filter

You will often filter using comparison operators (\>, \>=, \<, \<=, ==,
and !=). You may also use %in% to detect if values appear in a set, and
is.na() to find missing values. The results of comparisons are logical –
the statement is TRUE or FALSE depending on the values you compare – and
can be combined with other comparisons using the logical operators &
(and) and \| (or), or negated using !.

Some ways you might filter the litters data are:

gd_of_birth == 20 pups_born_alive \>= 2 pups_survive != 4 !(pups_survive
== 4) group %in% c(“Con7”, “Con8”) group == “Con7” & gd_of_birth == 20

A very common filtering step requires you to omit missing observations.
You can do this with filter, but I recommend using drop_na from the
tidyr package:

drop_na(litters_data) will remove any row with a missing value
drop_na(litters_data, wt_increase) will remove rows for which
wt_increase is missing.

# Learning Assessment

``` r
filter(pups_data, sex == 1)
## # A tibble: 155 × 6
##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
##   <chr>         <int>   <int>   <int>    <int>   <int>
## 1 #85               1       4      13        7      11
## 2 #85               1       4      13        7      12
## 3 #1/2/95/2         1       5      13        7       9
## # … with 152 more rows

filter(pups_data, pd_walk < 11, sex == 2)
## # A tibble: 127 × 6
##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
##   <chr>         <int>   <int>   <int>    <int>   <int>
## 1 #1/2/95/2         2       4      13        7       9
## 2 #1/2/95/2         2       4      13        7      10
## 3 #1/2/95/2         2       5      13        8      10
## # … with 124 more rows
```

### Mutate

To change columns or create new ones

``` r
mutate(litters_data,
  wt_gain = gd18_weight - gd0_weight,
  group = str_to_lower(group)
)
## # A tibble: 49 × 9
##   group litter_number gd0_weight gd18_…¹ gd_of…² pups_…³ pups_…⁴ pups_…⁵ wt_gain
##   <chr> <chr>              <dbl>   <dbl>   <int>   <int>   <int>   <int>   <dbl>
## 1 con7  #85                 19.7    34.7      20       3       4       3    15  
## 2 con7  #1/2/95/2           27      42        19       8       0       7    15  
## 3 con7  #5/5/3/83/3-3       26      41.4      19       6       0       5    15.4
## # … with 46 more rows, and abbreviated variable names ¹​gd18_weight,
## #   ²​gd_of_birth, ³​pups_born_alive, ⁴​pups_dead_birth, ⁵​pups_survive
```

# Learning Assessment

``` r
mutate(pups_data,
       sub_pd_pivot = pd_pivot - 7,
       sum_pd = pd_ears + pd_eyes + pd_pivot + pd_walk
)
## # A tibble: 313 × 8
##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk sub_pd_pivot sum_pd
##   <chr>         <int>   <int>   <int>    <int>   <int>        <dbl>  <int>
## 1 #85               1       4      13        7      11            0     35
## 2 #85               1       4      13        7      12            0     36
## 3 #1/2/95/2         1       5      13        7       9            0     34
## # … with 310 more rows
```

### Arrange

Arrange the rows in your data according to the values in one or more
columns

In SAS, similar to proc sort

``` r
head(arrange(litters_data, group, pups_born_alive), 10)
## # A tibble: 10 × 8
##    group litter_number   gd0_weight gd18_weight gd_of_…¹ pups_…² pups_…³ pups_…⁴
##    <chr> <chr>                <dbl>       <dbl>    <int>   <int>   <int>   <int>
##  1 Con7  #85                   19.7        34.7       20       3       4       3
##  2 Con7  #5/4/2/95/2           28.5        44.1       19       5       1       4
##  3 Con7  #5/5/3/83/3-3         26          41.4       19       6       0       5
##  4 Con7  #4/2/95/3-3           NA          NA         20       6       0       6
##  5 Con7  #2/2/95/3-2           NA          NA         20       6       0       4
##  6 Con7  #1/2/95/2             27          42         19       8       0       7
##  7 Con7  #1/5/3/83/3-3/2       NA          NA         20       9       0       9
##  8 Con8  #2/2/95/2             NA          NA         19       5       0       4
##  9 Con8  #1/6/2/2/95-2         NA          NA         20       7       0       6
## 10 Con8  #3/6/2/2/95-3         NA          NA         20       7       0       7
## # … with abbreviated variable names ¹​gd_of_birth, ²​pups_born_alive,
## #   ³​pups_dead_birth, ⁴​pups_survive
```

### %\>%

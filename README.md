
tidyNano <img src="man/figures/tidyNano.png" align="right" width = "120" />
===========================================================================

tidyNano is a package that imports raw NanoSight data and provides the framework to clean, analyze, and visualize nanoparticle analysis data.

Installation
------------

The latest development version can be installed from github:

``` r
# install.packages("devtools")
devtools::install_github("nguyens7/tidyNano")
```

tidyNano Example
----------------

``` r
library(tidyNano)
library(tidyverse)

file <- system.file("extdata", "beads.csv", package = "tidyNano")

data <- nanoimport(file)  

head(data)
#>   particle_size std_10000_yes_2_00 std_10000_yes_2_01 std_10000_yes_2_02
#> 1           0.5                  0                  0                  0
#> 2           1.5                  0                  0                  0
#> 3           2.5                  0                  0                  0
#> 4           3.5                  0                  0                  0
#> 5           4.5                  0                  0                  0
#> 6           5.5                  0                  0                  0
#>   std_10000_no_2_00 std_10000_no_2_01 std_10000_no_2_02 std_10000_yes_1_00
#> 1                 0                 0                 0                  0
#> 2                 0                 0                 0                  0
#> 3                 0                 0                 0                  0
#> 4                 0                 0                 0                  0
#> 5                 0                 0                 0                  0
#> 6                 0                 0                 0                  0
#>   std_10000_yes_1_01 std_10000_yes_1_02 std_10000_no_1_00
#> 1                  0                  0                 0
#> 2                  0                  0                 0
#> 3                  0                  0                 0
#> 4                  0                  0                 0
#> 5                  0                  0                 0
#> 6                  0                  0                 0
#>   std_10000_no_1_01 std_10000_no_1_02
#> 1                 0                 0
#> 2                 0                 0
#> 3                 0                 0
#> 4                 0                 0
#> 5                 0                 0
#> 6                 0                 0
```

``` r
tidy_data <- data  %>% 
  nanotidy(sep_var = c("Sample", "Dilution","Filter","Injection","Tech_rep"))

head(tidy_data)
#>   particle_size Sample Dilution Filter Injection Tech_rep Count True_count
#> 1           0.5    std    10000    yes         2        0     0          0
#> 2           1.5    std    10000    yes         2        0     0          0
#> 3           2.5    std    10000    yes         2        0     0          0
#> 4           3.5    std    10000    yes         2        0     0          0
#> 5           4.5    std    10000    yes         2        0     0          0
#> 6           5.5    std    10000    yes         2        0     0          0
```

``` r
tidy_data %>% 
  ggplot(aes(x = particle_size, y = True_count, color = Tech_rep)) +
  geom_line(size = 1) +
  facet_wrap(Injection ~ Filter)
```

![](man/figures/unnamed-chunk-3-1.png)

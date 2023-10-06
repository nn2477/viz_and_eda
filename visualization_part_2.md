visualization 2
================
Nhu Nguyen
2023-10-05

loading necessary libraries

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(patchwork)
```

## loading the weather data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USW00022534", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2021-01-01",
    date_max = "2022-12-31") |>
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USW00022534 = "Molokai_HI",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) |>
  select(name, id, everything())
```

    ## using cached file: /Users/nhunguyen/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2023-10-04 20:49:04 (8.527)

    ## file min/max dates: 1869-01-01 / 2023-10-31

    ## using cached file: /Users/nhunguyen/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00022534.dly

    ## date created (size, mb): 2023-10-04 20:49:16 (3.832)

    ## file min/max dates: 1949-10-01 / 2023-10-31

    ## using cached file: /Users/nhunguyen/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2023-10-04 20:49:20 (0.997)

    ## file min/max dates: 1999-09-01 / 2023-10-31

``` r
weather_df
```

    ## # A tibble: 2,190 × 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1  
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6
    ## # ℹ 2,180 more rows

## remember this plot?

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5)
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->
this plot is good for me to know what is going on in the graph, but if
someone else were to look at this graph, they might not know what tmin
or tmax is – might be helpful to have a caption

## labels

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) + 
  labs(
    title = "temperature plot", 
    x = "minimum daily temperature (x)",
    y = "maximum daily temperature (y)", 
    color = "location", 
    caption = "data from the rnoaa package: temperatures in 2017"
  )
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

## scales

start with the same plot, x and y scale

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) + 
  labs(
    title = "temperature plot", 
    x = "minimum daily temperature (x)",
    y = "maximum daily temperature (y)", 
    color = "location", 
    caption = "data from the rnoaa package: temperatures in 2017"
  ) + 
scale_x_continuous(
  breaks = c(-15, 0, 15),
  labels = c("-15º C", "0", "15")
) + 
scale_y_continuous(
  trans = "sqrt"
)
```

    ## Warning in self$trans$transform(x): NaNs produced

    ## Warning: Transformation introduced infinite values in continuous y-axis

    ## Warning: Removed 142 rows containing missing values (`geom_point()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

look at color scales

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) + 
  labs(
    title = "temperature plot", 
    x = "minimum daily temperature (x)",
    y = "maximum daily temperature (y)", 
    color = "location", 
    caption = "data from the rnoaa package: temperatures in 2017"
  ) + 
  scale_color_hue(h = c(100, 300))
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) + 
  labs(
    title = "temperature plot", 
    x = "minimum daily temperature (x)",
    y = "maximum daily temperature (y)", 
    color = "location", 
    caption = "data from the rnoaa package: temperatures in 2017"
  ) + 
  viridis::scale_color_viridis(
    name = "Location", 
    discrete = TRUE
  )
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

## themes

shift the legend

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) + 
  labs(
    title = "temperature plot", 
    x = "minimum daily temperature (x)",
    y = "maximum daily temperature (y)", 
    color = "location", 
    caption = "data from the rnoaa package: temperatures in 2017"
  ) + 
  viridis::scale_color_viridis(
    name = "Location", 
    discrete = TRUE) + 
  theme(legend.position = "bottom")
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

change the overall theme

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) + 
  labs(
    title = "temperature plot", 
    x = "minimum daily temperature (x)",
    y = "maximum daily temperature (y)", 
    color = "location", 
    caption = "data from the rnoaa package: temperatures in 2017"
  ) + 
  viridis::scale_color_viridis(
    name = "Location", 
    discrete = TRUE) + 
  theme_minimal() + 
  theme(legend.position = "bottom")
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

## setting options

- put at the very beginning of rmarkdown file – makes sure that every
  plot i make has the viridis color palate, the theme will be minimal,
  and the legend will be at the bottom

``` r
library(tidyverse)

knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6, 
  out.width = "90%"
)

theme_set(theme_minimal() + theme(legend.position = "bottom"))

options(
  ggplot2.continuous.colour = "viridis", 
  ggplot2.continuous.fill = "viridis"
)

scale_color_discrete = scale_color_viridis_d
scale_fill_discrete = scale_fill_viridis_d
```

## data args in `geom`

``` r
central_park_df = 
  weather_df |> 
  filter(name == "CentralPark_NY")

molokai_df = 
  weather_df |> 
  filter(name == "Molokai_HI")

ggplot(data = molokai_df, aes(x = date, y = tmax, color = name)) +
  geom_point() +
  geom_line(data = central_park_df)
```

    ## Warning: Removed 1 rows containing missing values (`geom_point()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

## `patchwork`

remember faceting?

``` r
weather_df %>% 
  ggplot(aes(x = tmin, fill = name)) + 
  geom_density(alpha=.5) + 
  facet_grid(. ~ name)
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_density()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

what happens when you want multipanel plots but can’t facet? - where
`patchwork` comes in

``` r
tmax_tmin_p =
  weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .5) +
  theme(legend.position = "none")

prcp_dens_p = 
  weather_df %>% 
  filter(prcp > 0) %>% 
  ggplot(aes(x = prcp, fill = name)) + 
  geom_density(alpha = .5) +
  theme(legend.position = "none")

tmax_date_p = 
  weather_df %>% 
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point(alpha = .5) + 
  geom_smooth(se = FALSE) +
  theme(legend.position = "bottom")

(tmax_tmin_p + prcp_dens_p) / tmax_date_p
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).
    ## Removed 17 rows containing missing values (`geom_point()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

## data manipulation

``` r
weather_df %>% 
  ggplot(aes(x = name, y = tmax, fill = name)) + 
  geom_violin(alpha = .5)
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_ydensity()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-14-1.png)<!-- --> -
for character variables, R turns the names into factors
(i.e. centralpark = 1) and then puts it in alphabetical order - in order
to put the labels in a different order, need to manipulate my data by
using factors – not a ggplot problem - mutate has turned the variable
from character to factor

control your factors

``` r
weather_df %>% 
  mutate(
    name = factor(name), 
    name = forcats::fct_relevel(name, c("Molokai_HI"))
  ) %>% 
  ggplot(aes(x = name, y = tmax, fill = name)) + 
  geom_violin(alpha = .5) 
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_ydensity()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

what if i wanted densities for tmin and tmax simultaneously?

``` r
weather_df |>
  select(name, tmax, tmin) |> 
  pivot_longer(
    tmax:tmin,
    names_to = "observation", 
    values_to = "temp"
  ) %>% 
  ggplot(aes(x = temp, fill = observation)) +
  geom_density(alpha = .5) +
  facet_grid(~ name) + 
  viridis::scale_fill_viridis(discrete = TRUE)
```

    ## Warning: Removed 34 rows containing non-finite values (`stat_density()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

## revisit the pulse

``` r
pulse_data = 
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names() %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m,
    names_to = "visit",
    names_prefix = "bdi_score_",
    values_to = "bdi") %>% 
  select(id, visit, everything()) %>% 
  mutate(
    visit = recode(visit, "bl" = "00m"), 
    visit = factor(visit, levels = str_c(c("00", "01", "06", "12"), "m"))) %>% 
  arrange(id, visit)

ggplot(pulse_data, aes(x = visit, y = bdi)) + 
  geom_point()
```

    ## Warning: Removed 879 rows containing missing values (`geom_point()`).

![](visualization_part_2_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

## revisiting pups dataset

data from the FAS study

``` r
pups_df = 
  read_csv("/Users/nhunguyen/Desktop/P8150/data_wrangling/data/FAS_pups.csv") %>% 
  janitor::clean_names() %>% 
  mutate(sex = recode(sex, `1` = "male", `2` = "female"))
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df = 
  read_csv("/Users/nhunguyen/Desktop/P8150/data_wrangling/data/FAS_litters.csv") %>% 
  janitor::clean_names() %>% 
  separate(group, into = c("dose", "day_of_tx"), sep = 3)
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
fas_df = left_join(pups_df, litters_df, by = "litter_number")

fas_df %>% 
  select(dose, day_of_tx, starts_with("pd_")) %>% 
  pivot_longer(
    pd_ears:pd_walk,
    names_to = "outcome",
    values_to = "pn_day"
  ) %>% 
  drop_na() %>% 
  mutate(outcome = forcats::fct_reorder(outcome, pn_day, median)) %>% 
  ggplot(aes(x = dose, y = pn_day)) + 
  geom_violin() +
  facet_grid(day_of_tx ~ outcome)
```

![](visualization_part_2_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

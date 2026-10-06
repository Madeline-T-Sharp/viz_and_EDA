Visualization II
================
Madeline Sharp
2026-10-01

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ readr     2.2.0
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.3     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(patchwork)
library(p8105.datasets)

data("weather_df")
```

Now we have everything we need!

Start with a scatterplot.

``` r
weather_df |>
  ggplot(aes(x = tmax, y = tmin, color = name)) +
  geom_point() + 
  labs(
    title = "Temperature (Max vs. Min)",
    x = "Max Temperature (C)",
    y = "Min Temperature (C)",
    color = "Location",
    caption = "Data from NOAA for three weather stations."
  )
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

Let’s try some other scales.

``` r
weather_df |>
  ggplot(aes(x = tmax, y = tmin, color = name)) +
  geom_point() + 
  labs(
    title = "Temperature (Max vs. Min)",
    x = "Max Temperature (C)",
    y = "Min Temperature (C)",
    color = "Location",
    caption = "Data from NOAA for three weather stations."
  ) +
  scale_x_continuous(
    breaks = c(-10, 0, 15),
    labels = c("-10 C", "0", "Fifteen")
  ) +
  scale_y_continuous(
    trans = "sqrt",
    position = "right"
  )
```

    ## Warning in transformation$transform(x): NaNs produced

    ## Warning in scale_y_continuous(trans = "sqrt", position = "right"): sqrt
    ## transformation introduced infinite values.

    ## Warning: Removed 520 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

Let’s look at color!!!

``` r
weather_df |>
  ggplot(aes(x = tmax, y = tmin, color = name)) +
  geom_point() +
  scale_colour_hue(h = c(100, 300))
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmax, y = tmin, color = name)) +
  geom_point() +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE
  )
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## Themes

``` r
weather_df |>
  ggplot(aes(x = tmax, y = tmin, color = name)) +
  geom_point() +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE
  ) +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmax, y = tmin, color = name)) +
  geom_point() +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE
  ) +
  theme_bw() +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmax, y = tmin, color = name)) +
  geom_point() +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE
  ) +
  theme_minimal() +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmax, y = tmin, color = name)) +
  geom_point() +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE
  ) +
  theme_classic() +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
weather_df |>
  ggplot(aes(x = tmax, y = tmin, color = name)) +
  geom_point() +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE
  ) +
  # order matters. If you do this in this order as opposed to the other, 
  # then the second fn will overwrite the first.
  # this is why the legend is on the right instead of the bottom
  theme(legend.position = "bottom") +
  theme_minimal()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

Update the tmax vs date plot

``` r
# my effort
ggp_weather = 
  weather_df |>
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point(aes(size = prcp), alpha = .5) +
  facet_grid(. ~ name) +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE
  ) +
  theme_bw() +
  theme(legend.position = "bottom")
```

``` r
# jeff's effort
weather_df |>
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point() +
  geom_smooth(se = FALSE) +
  labs(
    title = "Seasonal trends in Max Temp",
    x = "Date",
    y = "Max Temp",
    caption = "Max daily temp in three weather stations in 2021 and 2020",
    color = "location"
  ) +
  viridis::scale_color_viridis(
    name = "Location",
    discrete = TRUE
  ) +
  theme_minimal() +
  theme(legend.position = "bottom")
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

## Two more weird but useful plot things

``` r
central_park_df =
  weather_df |>
  filter(name == "CentralPark_NY")

molokai_df =
  weather_df |>
  filter(name == "Molokai_HI")

ggplot(molokai_df, aes(x = date, y = tmax, color = name)) +
  geom_point()
```

    ## Warning: Removed 1 row containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

``` r
# add a completely different geometry to this plot that has a different df
ggplot(molokai_df, aes(x = date, y = tmax, color = name)) +
  geom_point() +
  geom_line(data = central_park_df)
```

    ## Warning: Removed 1 row containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-13-2.png)<!-- -->

multiple panels with different plot types.

``` r
ggp_tmax_tmin =
  weather_df |>
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point() +
  theme(legend.position = "none")

ggp_prcp_density =
  weather_df |>
  filter(prcp > 0) |>
  ggplot(aes(x = prcp, fill = name)) +
  geom_density(alpha = .5) +
  theme(legend.position = "none")
  
ggp_seasonal =
  weather_df |>
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point() +
  theme(legend.position = "bottom")

# using patchwork to take these panels and put them together
# mathematical symbols show position
(ggp_tmax_tmin + ggp_prcp_density) / ggp_seasonal
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).
    ## Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](02_viz_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

## Data manipulation

Start with factors. (Factors are labels sitting on top of numbers)

boxplots!

``` r
# if you don't tell ggplot what order to put plots in before using ggplot, 
# it will automatically choose alphabetical order
weather_df |>
  ggplot(aes(x = name, y = tmax)) +
  geom_boxplot()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_boxplot()`).

![](02_viz_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

``` r
# fct_relevel makes variables factors and how you would like to order them
weather_df |>
  mutate(name = fct_relevel(name, c("Molokai_HI", "CentralPark_NY", 
                                    "Waterhole_WA"))) |>
  ggplot(aes(x = name, y = tmax)) +
  geom_boxplot()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_boxplot()`).

![](02_viz_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

``` r
weather_df |>
  # reordering is not a ggplot problem. Must be fixed outside ggplot.
  # putting things in the order you want is a factor problem; not a ggplot one
  mutate(name = fct_reorder(name, tmax)) |>
  ggplot(aes(x = name, y = tmax)) +
  geom_boxplot()
```

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `name = fct_reorder(name, tmax)`.
    ## Caused by warning:
    ## ! `fct_reorder()` removing 17 missing values.
    ## ℹ Use `.na_rm = TRUE` to silence this message.
    ## ℹ Use `.na_rm = FALSE` to preserve NAs.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_boxplot()`).

![](02_viz_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

Make that distribution plot

``` r
# looking at the min and max at each specific location
weather_df |>
  select(name, tmax, tmin) |>
  pivot_longer(
    tmax:tmin,
    names_to = "observation",
    values_to = "temp"
  ) |>
  ggplot(aes(x = temp, fill = observation)) +
  geom_density(alpha = 0.5) +
  facet_grid(. ~ name)
```

    ## Warning: Removed 34 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](02_viz_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

Make an FAS

``` r
# start with the plot you want to make and 
# think about how you want/need to get there
pups_df =
  read_csv(
    "data/FAS_pups.csv", skip = 3, na = c("", "NA", ".")) |>
  janitor::clean_names()
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
  read_csv(
    "data/FAS_litters.csv", na = c("", "NA", ".")) |>
  janitor::clean_names() |>
  separate(group, into = c("dose", "day_of_tx"), 3)
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
fas_df =
  left_join(pups_df, litters_df, by = "litter_number")

fas_df |>
  select(dose, day_of_tx, starts_with("pd")) |>
  pivot_longer(
    starts_with("pd"),
    names_to = "outcome",
    values_to = "pn_day"
  ) |>
  drop_na() |>
  ggplot(aes(x = dose, y = pn_day)) +
  geom_boxplot() +
  facet_grid(day_of_tx ~ outcome)
```

![](02_viz_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

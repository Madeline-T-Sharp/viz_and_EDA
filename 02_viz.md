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

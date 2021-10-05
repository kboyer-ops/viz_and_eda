viz\_eda\_1
================
Kaila Boyer
10/5/2021

Loading in dataset

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2021-03-22 17:09:07 (7.559)

    ## file min/max dates: 1869-01-01 / 2021-03-31

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2021-10-05 10:30:50 (1.697)

    ## file min/max dates: 1965-01-01 / 2020-02-29

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2021-10-05 10:30:53 (0.912)

    ## file min/max dates: 1999-09-01 / 2021-09-30

## Scatter plots

tmax vs. tmin

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point() 
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_and_eda_1_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

can save ggplots

``` r
gg_tmax_tmin <- weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point() 
```

## Adding things to scatter plots

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = 0.3) + 
  geom_smooth(se = FALSE) 
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_and_eda_1_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point(aes(color = name), alpha = 0.3) + 
  geom_smooth(se = FALSE) 
```

    ## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_and_eda_1_files/figure-gfm/unnamed-chunk-5-2.png)<!-- -->

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(, alpha = 0.3) + 
  geom_smooth(se = FALSE) + 
  facet_grid( . ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_and_eda_1_files/figure-gfm/unnamed-chunk-5-3.png)<!-- -->

One more scatter plot

``` r
weather_df %>% 
  ggplot(aes(x = date, y = tmax, size = prcp)) + 
  geom_point(alpha = 0.3) + 
  facet_grid(. ~ name) + 
  geom_smooth(se = FALSE) 
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](viz_and_eda_1_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

## Let’s data manipulation as part of this

``` r
weather_df %>% 
  filter(name == "CentralPark_NY") %>% 
  mutate(
    tmax = tmax * (9/5) + 32, 
    tmin = tmin * (9/5) + 32
  ) %>% 
  ggplot(aes(x = date, y = tmax)) + 
  geom_point() 
```

![](viz_and_eda_1_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

## Stack geoms

which geoms do you want?

``` r
weather_df %>% 
  ggplot(aes(x = date, y = tmax, color = name)) + 
  geom_smooth()
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

![](viz_and_eda_1_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

With hexbins

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_hex()
```

    ## Warning: Removed 15 rows containing non-finite values (stat_binhex).

![](viz_and_eda_1_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_bin_2d()
```

    ## Warning: Removed 15 rows containing non-finite values (stat_bin2d).

![](viz_and_eda_1_files/figure-gfm/unnamed-chunk-9-2.png)<!-- -->
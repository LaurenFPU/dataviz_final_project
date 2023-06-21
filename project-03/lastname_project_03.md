---
title: "Data Visualization for Exploratory Data Analysis"
output: 
  html_document:
    keep_md: true
    toc: true
    toc_float: true
---

# Data Visualization Project 03


In this exercise you will explore methods to create different types of data visualizations (such as plotting text data, or exploring the distributions of continuous variables).


## PART 1: Density Plots

Using the dataset obtained from FSU's [Florida Climate Center](https://climatecenter.fsu.edu/climate-data-access-tools/downloadable-data), for a station at Tampa International Airport (TPA) for 2022, attempt to recreate the charts shown below which were generated using data from 2016. You can read the 2022 dataset using the code below: 


```r
library(tidyverse)
weather_tpa <- read_csv("https://raw.githubusercontent.com/reisanar/datasets/master/tpa_weather_2022.csv")
# random sample 
sample_n(weather_tpa, 4)
```

```
## # A tibble: 4 × 7
##    year month   day precipitation max_temp min_temp ave_temp
##   <dbl> <dbl> <dbl>         <dbl>    <dbl>    <dbl>    <dbl>
## 1  2022     1    27             0       74       59     66.5
## 2  2022     3    21             0       85       57     71  
## 3  2022     2    14             0       68       46     57  
## 4  2022     1    11             0       73       54     63.5
```

See https://www.reisanar.com/slides/relationships-models#10 for a reminder on how to use this type of dataset with the `lubridate` package for dates and times (example included in the slides uses data from 2016).

Using the 2022 data: 

(a) Create a plot like the one below:

<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_facet.png" width="80%" style="display: block; margin: auto;" />

Hint: the option `binwidth = 3` was used with the `geom_histogram()` function.


```r
# prepare data for histogram
tpa_month <- weather_tpa %>%
  mutate(Month = lubridate::month(month, label = TRUE, abb = FALSE))

tpa_month %>% sample_n(3)
```

```
## # A tibble: 3 × 8
##    year month   day precipitation max_temp min_temp ave_temp Month   
##   <dbl> <dbl> <dbl>         <dbl>    <dbl>    <dbl>    <dbl> <ord>   
## 1  2022    10     6       0             84       67     75.5 October 
## 2  2022    11    21       0.00001       79       54     66.5 November
## 3  2022     7     5       0.01          94       81     87.5 July
```


```r
theme_set(theme_bw())

tpa_month %>%
  ggplot() +
  geom_histogram(aes(x = max_temp, fill = month), 
                 binwidth = 3, 
                 color = "white") +
  scale_fill_viridis_c() +
#  scale_x_continuous(breaks = seq(50,100,10)) + # omitted: less clutter by 20s
  facet_wrap(vars(Month)) +
  labs(x = "Maximum temperatures",
       y = "Number of Days") +
  theme(legend.position = "none",
        strip.text.x = element_text(size = 13),
        axis.title.x = element_text(size = 15),
        axis.title.y = element_text(size = 15),
        axis.text.y = element_text(size = 13),
        axis.text.x = element_text(size = 13))
```

<img src="lastname_project_03_files/figure-html/tpa_month_hist-1.png" width="80%" style="display: block; margin: auto;" />


(b) Create a plot like the one below:

<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_density.png" width="80%" style="display: block; margin: auto;" />

Hint: check the `kernel` parameter of the `geom_density()` function, and use `bw = 0.5`.


```r
theme_set(theme_minimal())

weather_tpa %>%
  ggplot() +
  geom_density(aes(x = max_temp), 
               kernel = "optcosin", bw = 0.5, 
               fill = "grey50", size = 1) +
  labs(x = "Maximum temperature") +
  theme(axis.title.x = element_text(size = 15),
        axis.title.y = element_text(size = 15))
```

```
## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
## ℹ Please use `linewidth` instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

<img src="lastname_project_03_files/figure-html/density-plot-1.png" width="80%" style="display: block; margin: auto;" />


(c) Create a plot like the one below:

<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_density_facet.png" width="80%" style="display: block; margin: auto;" />

Hint: default options for `geom_density()` were used. 


```r
theme_set(theme_bw())

tpa_month %>%
  ggplot() +
  geom_density(aes(x = max_temp, fill = Month),
               alpha = 0.6, size = 1) +
  scale_y_continuous(breaks = seq(0,0.30,0.1)) +
  facet_wrap(vars(Month)) +
  labs(title = "Density plots for each month in 2022",
       x = "Maximum temperatures") +
  theme(legend.position = "none",
        plot.title = element_text(size = 20),
        strip.text.x = element_text(size = 15),
        axis.text.x = element_text(size = 14),
        axis.title.x = element_text(size = 15),
        axis.text.y = element_text(size = 13),
        axis.title.y = element_blank())
```

<img src="lastname_project_03_files/figure-html/density-monthly-1.png" width="80%" style="display: block; margin: auto;" />


(d) Generate a plot like the chart below:


<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_ridges_plasma.png" width="80%" style="display: block; margin: auto;" />

Hint: use the`{ggridges}` package, and the `geom_density_ridges()` function paying close attention to the `quantile_lines` and `quantiles` parameters. The plot above uses the `plasma` option (color scale) for the _viridis_ palette.



```r
theme_set(theme_minimal())

library(ggridges)
```

```
## Warning: package 'ggridges' was built under R version 4.3.1
```

```r
tpa_month %>%
  ggplot(aes(x = max_temp, y = Month, fill = after_stat(x))) +
  geom_density_ridges_gradient(quantile_lines = TRUE, quantiles = 2,
                               size = 1) + # "deprecated" warning noted, but get "ignoring" warning using linewidth 
  scale_fill_viridis_c(option = "C") +
  scale_x_continuous(breaks = seq(40, 100, 10)) +
  labs(x = "Maximum temperature (in Fahrenheit degrees)",
       fill = element_blank(),
       y = element_blank()) +
  theme(axis.title.x = element_text(size = 17),
        axis.text.y = element_text(size = 13),
        axis.text.x = element_text(size = 13),
        legend.text = element_text(size = 13))
```

```
## Picking joint bandwidth of 1.93
```

```
## Warning: Using the `size` aesthetic with geom_segment was deprecated in ggplot2 3.4.0.
## ℹ Please use the `linewidth` aesthetic instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

<img src="lastname_project_03_files/figure-html/ridges-monthly-1.png" width="80%" style="display: block; margin: auto;" />



(e) Create a plot of your choice that uses the attribute for precipitation _(values of -99.9 for temperature or -99.99 for precipitation represent missing data)_.



## PART 2 

> **You can choose to work on either Option (A) or Option (B)**. Remove from this template the option you decided not to work on. 


### Option (A): Visualizing Text Data

Review the set of slides (and additional resources linked in it) for visualizing text data: https://www.reisanar.com/slides/text-viz#1

Choose any dataset with text data, and create at least one visualization with it. For example, you can create a frequency count of most used bigrams, a sentiment analysis of the text data, a network visualization of terms commonly used together, and/or a visualization of a topic modeling approach to the problem of identifying words/documents associated to different topics in the text data you decide to use. 

Make sure to include a copy of the dataset in the `data/` folder, and reference your sources if different from the ones listed below:

- [Billboard Top 100 Lyrics](https://github.com/reisanar/datasets/blob/master/BB_top100_2015.csv)

- [RateMyProfessors comments](https://github.com/reisanar/datasets/blob/master/rmp_wit_comments.csv)

- [FL Poly News Articles](https://github.com/reisanar/datasets/blob/master/flpoly_news_SP23.csv)


(to get the "raw" data from any of the links listed above, simply click on the `raw` button of the GitHub page and copy the URL to be able to read it in your computer using the `read_csv()` function)


### Option (B): Data on Concrete Strength 

Concrete is the most important material in **civil engineering**. The concrete compressive strength is a highly nonlinear function of _age_ and _ingredients_. The dataset used here is from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/index.php), and it contains 1030 observations with 9 different attributes 9 (8 quantitative input variables, and 1 quantitative output variable). A data dictionary is included below: 


Variable                      |    Notes                
------------------------------|-------------------------------------------
Cement                        | kg in a $m^3$ mixture             
Blast Furnace Slag            | kg in a $m^3$ mixture  
Fly Ash                       | kg in a $m^3$ mixture             
Water                         | kg in a $m^3$ mixture              
Superplasticizer              | kg in a $m^3$ mixture
Coarse Aggregate              | kg in a $m^3$ mixture
Fine Aggregate                | kg in a $m^3$ mixture      
Age                           | in days                                             
Concrete compressive strength | MPa, megapascals


Below we read the `.csv` file using `readr::read_csv()` (the `readr` package is part of the `tidyverse`)


```r
concrete <- read_csv("../data/concrete.csv", col_types = cols())
```


Let us create a new attribute for visualization purposes, `strength_range`: 


```r
new_concrete <- concrete %>%
  mutate(strength_range = cut(Concrete_compressive_strength, 
                              breaks = quantile(Concrete_compressive_strength, 
                                                probs = seq(0, 1, 0.2))) )
```



1. Explore the distribution of 2 of the continuous variables available in the dataset. Do ranges make sense? Comment on your findings.

2. Use a _temporal_ indicator such as the one available in the variable `Age` (measured in days). Generate a plot similar to the one shown below. Comment on your results.

<img src="https://github.com/reisanar/figs/raw/master/concrete_strength.png" width="80%" style="display: block; margin: auto;" />


3. Create a scatterplot similar to the one shown below. Pay special attention to which variables are being mapped to specific aesthetics of the plot. Comment on your results. 

<img src="https://github.com/reisanar/figs/raw/master/cement_plot.png" width="80%" style="display: block; margin: auto;" />





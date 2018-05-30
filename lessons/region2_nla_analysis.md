---
title: "2007 NLA Analysis"
editor_options:
  chunk_output_type: console
date: '2018-06-05'
output:
  html_document:
    toc: yes
    toc_depth: 3
    toc_float: true
editor_options: 
  chunk_output_type: console
---



# Purpose

The purpose of this document is to serve as motivating example (i.e. R, Markdown, and Open data are cool!), but will also serve to structure the rest of this workshop in that we will see how to work with and visualize data in R, combine code and documentation with R Markdown, and introduce [The Tidyverse](https://tidyverse.org) which is an opinionated (but effective) way to think about organizing and analyzing data in R.

# The Example

We will be using data from the 2007 National Lakes Assessment data as it provides a nice water quality relevant example and I am quite familiar with it so I already know most of the issues we will run into with it! 

## Get Data 

The data we need is available from the National Aquatic Resource Survey's website

First we can get the dataset that we have saved as a `.csv` in this repository.


```
## Error: 'gapminder_gdp.csv' does not exist in current working directory ('/var/host/media/removable/SD Card/region2_r/lessons').
```

Second, let's the get CO~2~ emissions data straight from the Google Sheets in which it is stored.


```r
gap_co2_url <- "https://docs.google.com/spreadsheets/d/1qJR55SL3lHcx1d3hMHJ1dMYqAog9sXofaPWjPkpO4QQ/pub"
gap_co2 <- gs_read(gs_url(gap_co2_url))
```

```
## 
```

## Manipulate Data

Let's tidy up these two datasets and join them together


```r
gap_gdp_tidy <- gap_gdp %>%
  gather("year","gdp",-1) %>%
  select(country = `GDP (constant 2000 US$)`, everything())
```

```
## Error in eval(lhs, parent, parent): object 'gap_gdp' not found
```

```r
gap_co2_tidy <- gap_co2 %>%
  gather("year","co2_emiss", -1) %>%
  select(country = `CO2 emission total`, everything())

gap_data <- gap_gdp_tidy %>%
  left_join(gap_co2_tidy, by = c("country", "year")) %>%
  filter(complete.cases(gdp,co2_emiss))
```

```
## Error in eval(lhs, parent, parent): object 'gap_gdp_tidy' not found
```

```r
datatable(gap_data)
```

```
## Error in inherits(x, "SharedData"): object 'gap_data' not found
```



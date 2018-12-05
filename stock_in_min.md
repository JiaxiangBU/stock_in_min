Stock price in min
================

> 这是那个分钟数据，你可以研究研究， 后面的money,avg,factor不太清楚是什么变量了 volume应该是成交量
> 价格应该/10000才是元单位 里面的open，close，high，low就是分钟级别的开盘，收盘，最高最低

见数据报告`report.html`

1.  变量有双波峰的情况
2.  QQ Plot 显示有很强的skewness
3.  数据有些问题，相关性太高

<!-- end list -->

``` r
knitr::opts_chunk$set(eval = FALSE)
library(data.table)
library(tidyverse)
```

    ## -- Attaching packages ---------------------------------------------- tidyverse 1.2.1 --

    ## √ ggplot2 3.1.0     √ purrr   0.2.5
    ## √ tibble  1.4.2     √ dplyr   0.7.8
    ## √ tidyr   0.8.2     √ stringr 1.3.1
    ## √ readr   1.2.1     √ forcats 0.3.0

    ## -- Conflicts ------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::between()   masks data.table::between()
    ## x dplyr::filter()    masks stats::filter()
    ## x dplyr::first()     masks data.table::first()
    ## x dplyr::lag()       masks stats::lag()
    ## x dplyr::last()      masks data.table::last()
    ## x purrr::transpose() masks data.table::transpose()

``` r
library(lubridate)
data <- 
    fread('minute.csv',drop = 'V1') %>% 
    mutate(time = hms(paste0(time,':00')))
```

``` r
data %>% 
    summarise_all(funs(typeof)) %>% 
    gather
```

``` r
# 生产EDA报告
library(DataExplorer)
config <- list(
  "introduce" = list(),
  "plot_str" = list(
    "type" = "diagonal",
    "fontSize" = 35,
    "width" = 1000,
    "margin" = list("left" = 350, "right" = 250)
  ),
  "plot_missing" = list(),
  "plot_histogram" = list(),
  "plot_qq" = list(sampled_rows = 1000L),
  "plot_bar" = list(),
  "plot_correlation" = list("cor_args" = list("use" = "pairwise.complete.obs")),
  # "plot_prcomp" = list(),
  "plot_boxplot" = list(),
  "plot_scatterplot" = list(sampled_rows = 1000L)
)

create_report(data
              ,config=config
              )
```

`create_report`给定 config

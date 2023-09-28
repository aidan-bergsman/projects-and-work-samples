---
title: "Athletic Wear Sales Analytics Project"
author: "Aidan Bergsman"
date: "2023-09-28"
output:
  html_document:
    theme: journal
    highlight: haddock
    toc: yes
    toc_depth: 3
    toc_float: yes
    keep_md: true
---

# Introduction

This project began as an extension of a DataCamp course on business analytics using Microsoft Excel. The course used an Excel Workbook with global sales and client data for athletic wear and taught how to create basic graphs and tables in Excel. After completing the course, I challenged myself to see if I could recreate the graphs and tables using RStudio, which is my analytics tool of choice.

After completing this challenge, I went a step further and added to my analytics by creating interactive graphs and global heat maps of sales to add even more analytics and insight to a potential colleague, boss, or investor. Below are the various graphs and tables with ample descriptions of what I created and why I thought it was important.

# Data

The data used for the following analytics comes from a DataCamp course on Microsoft Excel, which can be found [here](https://assets.datacamp.com/production/repositories/6317/datasets/2e57fe9fc676c640aab6d1c20f2ab0dfe9cd2ca3/Data%20Preparation%20in%20Excel.pdf). I completed the first chapter of the course, which walks through data cleaning, sheet renaming, and sheet reorganization, which is necessary to complete prior to following along with my analytics below (the first chapter of the DataCamp course is free to complete). After completing the course, I decided to load in the **"Orders"** sheet from the DataCamp workbook to conduct my analytics.


```r
# load in pacman
library(pacman)

# load in necessary packages
p_load(readxl,janitor)

# load in orders sheet from DataCamp Workbook
sales_data <- read_excel("~/github_repos/projects-and-work-samples/athletic_wear_sales_analytics_project_files/data/sales_data.xlsx",
                         sheet = "Orders")
sales_data
```

```
## # A tibble: 1,269 × 24
##    Order I…¹ Order…² `Order Date`        Days …³ Late_…⁴ Shipp…⁵ Categ…⁶ Custo…⁷
##        <dbl>   <dbl> <dttm>                <dbl>   <dbl> <chr>     <dbl>   <dbl>
##  1      6176    2015 2015-01-04 00:00:00       4       0 Standa…      17    3329
##  2     10384    2015 2015-01-06 00:00:00       4       1 Standa…      17     587
##  3     14551    2015 2015-01-08 00:00:00       2       0 Second…      17    2028
##  4     14574    2015 2015-01-08 00:00:00       2       1 Second…      18    6594
##  5     22924    2015 2015-01-12 00:00:00       2       1 Second…      29    9704
##  6       906    2015 2015-01-14 00:00:00       4       0 Standa…      40    7141
##  7       973    2015 2015-01-15 00:00:00       4       0 Standa…      29    5118
##  8      1077    2015 2015-01-16 00:00:00       2       1 Second…       9    8103
##  9      1105    2015 2015-01-17 00:00:00       4       1 Standa…      37    9760
## 10      1186    2015 2015-01-18 00:00:00       4       0 Standa…       9   11947
## # … with 1,259 more rows, 16 more variables: `Department Id` <dbl>,
## #   `Department Name` <chr>, Market <chr>, `Order City` <chr>,
## #   `Order State` <chr>, `Order Zipcode` <dbl>, `Order Country` <chr>,
## #   `Order Region` <chr>, `Product Category Id` <dbl>, `Product Id` <dbl>,
## #   `Product Price` <dbl>, `Product Cost` <dbl>, `Order Quantity` <dbl>,
## #   `Order Total Discount` <dbl>, Sales <dbl>, `Payment Type` <chr>, and
## #   abbreviated variable names ¹​`Order Id`, ²​`Order Year`, …
```

## Cleaning 
The data above contains all sorts of categorical and numerical data regarding individual orders of athletic wear. Lots of potential for analytics! Next, I will clean the data a bit, focusing on the column titles and making them more r-friendly (meaning lowercase letters and underscores instead of spaces). I will use the `janitor` package to quickly clean the column names using the `clean_names()` function.


```r
#clean column names
sales_data <- clean_names(sales_data)

# view data
sales_data
```

```
## # A tibble: 1,269 × 24
##    order_id order_…¹ order_date          days_…² late_…³ shipp…⁴ categ…⁵ custo…⁶
##       <dbl>    <dbl> <dttm>                <dbl>   <dbl> <chr>     <dbl>   <dbl>
##  1     6176     2015 2015-01-04 00:00:00       4       0 Standa…      17    3329
##  2    10384     2015 2015-01-06 00:00:00       4       1 Standa…      17     587
##  3    14551     2015 2015-01-08 00:00:00       2       0 Second…      17    2028
##  4    14574     2015 2015-01-08 00:00:00       2       1 Second…      18    6594
##  5    22924     2015 2015-01-12 00:00:00       2       1 Second…      29    9704
##  6      906     2015 2015-01-14 00:00:00       4       0 Standa…      40    7141
##  7      973     2015 2015-01-15 00:00:00       4       0 Standa…      29    5118
##  8     1077     2015 2015-01-16 00:00:00       2       1 Second…       9    8103
##  9     1105     2015 2015-01-17 00:00:00       4       1 Standa…      37    9760
## 10     1186     2015 2015-01-18 00:00:00       4       0 Standa…       9   11947
## # … with 1,259 more rows, 16 more variables: department_id <dbl>,
## #   department_name <chr>, market <chr>, order_city <chr>, order_state <chr>,
## #   order_zipcode <dbl>, order_country <chr>, order_region <chr>,
## #   product_category_id <dbl>, product_id <dbl>, product_price <dbl>,
## #   product_cost <dbl>, order_quantity <dbl>, order_total_discount <dbl>,
## #   sales <dbl>, payment_type <chr>, and abbreviated variable names
## #   ¹​order_year, ²​days_for_shipment_scheduled, ³​late_delivery_risk, …
```

Now the column headers are all lowercase and have underscores instead of spaces!


# Analytics Vidhya JanataHack Demand Forecasting


## Description Of Problem
Given 3 years of SKU sales data from 76 stores, the problem is to predict sales for next 12 weeks.


## Data
Data can be downloaded from the contest page [JanataHack: Demand Forecasting - Problem Statement](https://datahack.analyticsvidhya.com/contest/janatahack-demand-forecasting/#ProblemStatement)


Fields in data
- record_ID (unique ID for each row)
- week (starting date of the week)
- store_id 
- sku_id
- base_price
- total_price (in case of discount total_price < base_price)
- is_featured_sku (flag for whether sku_id is featured product of the week)
- is_display_sku (flag for whether sku_id is displayed product of the week - this will potentially mean banners , carousels etc)
- units_sold (target)
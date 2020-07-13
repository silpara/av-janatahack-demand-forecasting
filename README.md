# Analytics Vidhya JanataHack Demand Forecasting


## Description Of Problem
Given 3 years of SKU sales data from 76 stores, the problem is to predict sales for next 12 weeks.

## Evaluation
The evaluation metric was 100\*RMSLE (Root Mean Squared Log Error). This eval metric is available xgboost out-of-the-box.


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

## Models
1. [Forecasting using fbprophet](https://github.com/silpara/av-janatahack-demand-forecasting/blob/master/fbprophet-av-janatahack-demand-forecasting.ipynb) 
- **Experiment Set 1:** I used fbprophet library to fit time series model for each (store, sku) pair. This gave scores in range 785-809 (public), 710-739 (private).
- **Experiment Set 2:** Log transformed dependent variable and fitted prophet model. Score: 638.649013296102(public), 577.817887311402 (private)
- **Experiment Set 3:** Added regressors - total_price, base_price, is_featured_sku, is_display_sku to the model. Score jumped to 497.965157591909 (public), 563.605151425418 (private)
- **Experiment Set 4:** I added discounting, featured and display as special events to model. I added total price / max total price for sku as signal for discounted price for sku. This model takes ~ 30 mins to train and performed surprisingly well - 499.384415259399 (public) , 566.333485529977 (private).

	Didn't spend more time to tune prophet model/add more signals but there definitely looked scope to further improve it.

2. [Forecasting using xgboost](https://github.com/silpara/av-janatahack-demand-forecasting/blob/master/xgb-av-janatahack-demand-forecasting.ipynb)
 Additional features added to the xgb models were week_of_year, week_num (from starting of data), max total price for sku and total price/ max total price for sku. Target variable was log transformed sales.
- **Experiment Set 1:** xgb model performed will right from the start. I added store and sku as features to the model since test set had the same stores and sku. The baseline model score was 564.961275573224 (public),569.959964232437 (private)
- **Experiment Set 2:** I created model for each store independently but it didn't do well -- probably there was a bug?
- **Experiment Set 3:** Added lagged units_sold as features - that didn't do well either -- probably a bug in code which I didn't get into.
- **Experiment Set 4:** Tuned the first xgb model to get the final model. Additional features added to the xgb models were week_of_year, week_num (from starting of data), max total price for sku and total price/ max total price for sku. The **final model** gives score **386.025005238819 (public), 424.879932387956 (private)** with **rank 6** overall in the competition.


## Best Performing Model Approach
The best performing model for me was xgboost. The training process is as follows:
### Train-Eval Split
- Last 12 weeks of data for each store and sku was used as eval set and rest was training set.
- Trained xgboost with early stopping on eval set.
- Trained final model on full data with best iterations learned in previous step.



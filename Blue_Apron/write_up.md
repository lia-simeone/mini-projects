###Summary
Raw user age is not a strong predictor of whether a user place an order in the upcoming week. For a fixed population, the number of orders placed each week will continuously decline (at least for the first 10 weeks). This pattern makes user age a poor predictor as we *know* some older users are going to place an order. However, business intuition suggests the older users most likely to place an order in the upcoming week are the ones who have placed repeated orders in the past. A model built with a basic representation of this user history has good predictive power which could be improved with additional data.

###Modeling details
#####Selected method
I used decision trees as my primarily modeling tool. I made this choice because I only had 1-2 input variables, one of which had only 10 possible outcomes, so more sophisticated modeling tools seemed like overkill. I did try a gradient boosting model, but as expected, it had identifical performance to the decision tree. For the models with my user history variable, I split  user_fulfillment_history into an 80/20 test and train.

#####Target variable
I modeled the was_shipped indicator from the user_fulfillment_history data.

####Caveats
I treated each week of records as independent which is of course not true as every customer has 10 weeks of data. For exploratory analysis violating this assumption is usually okay, but I would want to make a more careful choice before modeling in production. For the model evaluation, I compared the models built with my user history variable and user age on a training subset from user_fulfillment_history and scored on a separate test subset. The model built _only_ on user age was fit and scored on the entire data. Therefore, the comparison is not ideal, but again,is acceptable for exploratory analysis. I would want to make them consistent before making any definitive claims about the difference in model performance.

####Other considered methods
When I first saw the problem, a time series technique seemed like an obvious choice given the weekly nature of the data. I considered ARIMA, but discarded it because it's not designed for binary targets and requires a seasonality component that wasn't available. Surivial could be a good fit for the binary target, but users can "live" to place another order after their first one so that didn't seem like a good fit either. Gradient boosting is another tool I use for quick exploratory modeling, but as I said above, I didn't use it because of the limited input variables.

###Suggestions for additional analysis
Because of the availability of the was_shipped variable, I only used user_fulfillment_history for modeling. Therefore, my model has only been exposed to users aged 1-10. To build a more robust model consistent with the real user population, I would want to build on a much broader set of user ages.

Seasonality data jumped immediately to mind for predicting weekly order volumes. Holiday weekends may see less orders as users travel out of town and for the fish business specifically, I imagine more orders are placed in summer vs. winter. Geographic data could also be interesting. Zip-9 is highly predictive of income which could help separate customers who can afford to become repeated customers vs. those who are just trying the service but won't use it after an initial promotion.

###Data exploration
I performed basic exploration of the datasets given. user_fulfillment_history had 10 weeks of data for all users with no missings. weekly_user_population had duplicates, so I removed those (although I didn't end up using this dataset), 

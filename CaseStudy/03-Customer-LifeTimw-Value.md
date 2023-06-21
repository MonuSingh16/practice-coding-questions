Problem
----------
How would you calculate the customer lifetime value (CLV) of Amazon customers?

Solution
---------
[Candidate] ​Before talking about the solution, I’d like to first frame the problem. Based on my experience, I believe customer lifetime value is defined by the following:

1.Lifetime history (i.e. 6 months, 9 months, 3 years and e.t.c.)
2.Prediction point (i.e. CLV prediction based on the first week of sale or first month of sale)
3.Customer segment (i.e. all Amazon customers or subset?)

Are my assumptions valid?

[Interviewer] ​They are all reasonable assumptions about CLV. Let’s discuss them more in details one by one. What do you believe would be a reasonable lifetime history?

[Candidate]​ Well, I believe this depends on the data history available. For instance, if I have three months worth of sales data per customer, I won’t be able to build a CLV model with greater than 3 months of forecast horizon. Also, it’s not possible to build a “lifetime” value of acustomer given churn and limited data history. So, it needs to be confined to some months or years. To start, perhaps I can design a system that predicts the CLV of 1 year.

[Interviewer] ​Sounds good. Now, let’s talk about how you would predict 1 year.

[Candidate] ​To calculate the 1-year CLV of a customer, I need their transaction history. But, it needs to be designed based on two different approaches. 
Design 1 - Predict 1 year based on the first X weeks/months/years of the customer lifetime value. An approach could be: predict 1 year given the customers’ first month of purchasing goods on Amazon.
Design 2 - Moving window approach. Regardless of whatever stage a customer is on, continue to predict the next 12 months of sales given the past X weeks/months/years sales patterns. Forinstance, predict sales of the next 12 months given the past 3 months of spending patterns.Update the model on a monthly basis.

[Interviewer]​ Now, let’s talk about variables. What do you believe are relevant signals for predicting the CLV of customers? Let’s say you have access to the customer’s purchase patterndata.

[Candidate]​ I think I could come up with a couple. I could apply apply a window-based functions aggregated at the customer level to derive the following features:
1.Average sales the past 1 days
2.Average sales the past 7 days
3.Average sales the past 30 days
4.Average sales the past 60 days
5.Average sales the past 120 days
6.Count sales the past 1 days
7.Count sales the past 7 days
8.Count sales the past 30 days
9.Count sales the past 60 days
10.Count sales the past 120 days
11.# of days since first purchase
12.# of logins this week

I could also apply the same strategy aggregated at the customer x department level to extract additional features. For instance, the average sales in grocery the past 7 days. Using this strategy, I could literally create hundreds of signals.

[Interviewer] ​Great. Now, one quick question on that. It seems that the signals could be highly correlated, how would you address that?

[Candidate] ​There are various statistical techniques to handle multicollinearity. One of them isusing the Lasso model to remove collinearity in the feature set.

[Interviewer]​ Sounds good. Now, what’s your next step toward designing the solution?

[Candidate]​ With the features select, I can start with a simple benchmark model such as random forest with hyper-parameter tuning on N trees, depths, # of samples per terminal node and e.t.c.

[Interviewer] ​If you were to perform hyper-parameter tuning, how much should it improve your model performance?

[Candidate] ​I’d say it’s reasonable to gain 1 to 5% improvement using hyper-paramter tuning.It’s quite rare for the improvement to happen beyond that unless the default parameter setting was overfitting the data to begin with.

[Interviewer]​ Great, one quick question on the granularity. Would you want to forecast the CLV of 1 year in a single data-point of prediction or 12 months?
[Candidate]​ I suppose that depends on what the business stakeholders want. Sometimes,single data-point suffice. However, in many cases, the business wants more granularity. So,forecasting the 12 month in a month-to-month granularity could work as well.

[Interviewer] ​Great. Now, let’s talk about productionization. How would you deploy your model?
[Candidate] ​If the offline model performance is decent (let’s say that MAPE is <10%), then I can deploy a model that predicts real-time. Meaning for every customer that has just completed the 1 month as a customer customer, predict their CLV for the next 12 months. I would deploy monitoring such as data validations, unit-testing and performance tracking to ensure that the model is performing quite well.
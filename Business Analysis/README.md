v# Business Analysis

## Overview
This project focuses on analyzing metrics in an ad campaign. The data that we had in hand were marketing expenses, server logs from June 2017 to May 2018, and orders for the period. Given these data, we would like to know how effective is our advertisement strategy and how much revenue it generates. We would also like to answer related questions such as know how people use the product, when they start to buy, how much money each
customer brings, and when they pay off.

Our analysis started with the data preprocessing step that included replacing missing values, removing duplicates, and converting data types.
We got the three tables `visit`, `orders`, `cost` resulting from data cleaning. 

The `visit` table contains user's id, logs of when users started logging in and whe they ended the session, and from which ad source they came to our site.

```python
visit.sample(5)
```
<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/visit.png" alt="visit" width="60%"/>
</p>

The `order` table shows users transaction, date of purchase and the revenue they generated

```python
order.sample(5)
```
<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/order.png" alt="order" width="40%"/>
</p>

Lastly, the `cost` table shows the marketing expenses spent for each ad source,

```python
cost.sample(5)
```
<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/cost.png" alt="cost" width="40%"/>
</p>

***
#### Daily, Weekly, Monthly Average Users
The first question we would like to know is how many people use it every day, week, and month. By grouping the `visit` dataset by its unique users, we can create a daily average users table.

```python
# Grouping the dataframe on week and calculating the number of unique users
dau = visits.groupby(['year', 'month', 'day']).agg({'uid':'nunique'}).reset_index()

```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/dau.png" alt="percent_growth" width="75%"/>
</p>


The table shows that visitors came in more frequently towards the end of the year, starting in October 2017 until the first quarter of the new year in March 2018. On Black Friday, 24 November 2017, the customer visit reaches the highest peak with 3319 visitors. On the other hand, there was a moment on 31 March 2018 where the number suddenly dropped 
to only one visit.

In general, the total number of visitors per day is 908.0 visitors.

Using the same apprach, we coould also created the average weekly user table,

```python
# Grouping the dataframe on week and calculating the number of unique users
wau = visits.groupby(['year', 'week']).agg({'uid':'nunique'}).reset_index()

```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/wau.png" alt="dau" width="60%"/>
</p>

On average, we can expect 5716 visits every week. However, we can see the same occurrence at the 47th week in 2017, where the visit suddenly jumped out. The number of visits stays high throughout the end of 2017. There was an increase in the 12th week of 2018, which coincides with school spring break until it drops to not much more than 5740 visits per week.

Finally the monthly average user,

```python
# Grouping the dataframe on week and calculating the number of unique users
mau = visits.groupby(['year', 'month']).agg({'uid':'nunique'}).reset_index()
```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/mau.png" alt="mau" width="70%"/>
</p>


On average, we can expect 23,228 visitors per month. The WAU graphs agree with the weekly trend we discussed earlier. Toward the end of the year, the number of visitors increased. The graphs peaked in November 2017, where Black Friday takes place.

#### Sessions per Day
One user can visit the website more than once. With that in mind, we would like to know how many sessions per day from all users. 

```python
session = visits.groupby(['year', 'month', 'day']).agg({'uid':'count'}).reset_index()
```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/daily_sessions.png" alt="daily_sessions" width="70%"/>
</p>

The number of sessions per day is 987.4

Previously we found out that the number of unique daily users is 908 visitors. On the other hand, the average daily session is 987.4. We can calculate the ratio of n_unique visitors and the daily session and see how it behaves throughout the year.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/daily_sessions_ratio.png" alt="daily_sessions_ratio" width="70%"/>
</p>

Generally, the ratio of sessions over the visit is 1.08. In other words, eight users visit the website two times in one hundred users. This is a pretty small amount, so we can confidently say that users only see the site once on regular days. However, during high season the percentage could reach 1.22.

Looking at the heatmap above, we see that ad source #3, #4, and #5 brought the most visitors to the site. At the end of the year, source #4 made the highest visit with 13144 sessions. On the other hand, source #7 performed poorly by attracting not more than nine sessions in a month.
<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/sessions_per_source.png" alt="sessions_per_source" width="70%"/>
</p>

#### How Often Do Users Come Back (Retention Rate)

The retention rate is defined as the number of active users at different periods divided by the initial 
size of the same group. This metric is particulary useful if we offer a subscribtion based service. It can give
us hint of how loyal our customers are

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/retention.png" alt="retention" width="70%"/>
</p>


The second-week retention for the first six weeks shows a relatively stable trend, despite the drop in Jul-17, with at least a 7.7% rate. At the beginning of the new year, the retention rate dropped around 2 percent lower to the level between 4.2-6.0%. The end of the festive season might cause this decrease where people returned to their normal daily activities.

The retention rate heatmap shows us that most cohorts increased their rate in the 2nd month. Additionally, although not valid for every cohort, generally, the retention rate rises steadily, especially for end-of-year cohorts where the number of retention improves.

#### When Do People Start Buying (Conversion Rate)

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/user_conversion.png" alt="user_conversion" width="70%"/>
</p>


#### How Many Orders In A Given Period of Time?

```python
# Calculating number of purchase per customer
(print('The average order per customer is {:.2f}'
       .format(orders.groupby('uid').count()['buy_ts'].sum()/ 
               orders.groupby('uid').count()['buy_ts'].shape[0])))


```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/average_order.png" alt="average_order" width="70%"/>
</p>
The average order per customer is 1.38

The daily heatmap shows that daily orders are usually at around 1.08-1.10 rate. However, there are days that the rate increased to above 1.20 in August-17 and March-18.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/daily_order.png" alt="daily_order" width="70%"/>
</p>


The daily heatmap shows that daily orders are usually at around 1.08-1.10 rate. However, there are days that the rate increased to above 1.20 in August-17 and March-18.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/weekly_order.png" alt="weekly_order" width="70%"/>
</p>



The weekly heatmap shows the peak, 48.11 orders per customer at the 8th week of 2018. That occurred at Feb 19-25, 18.
<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/monthly_order.png" alt="monthly_order" width="70%"/>
</p>

#### Lifetime Value (LTV)

LTV is the total amount of money a customer brings to the company on average by making purchases. When calculating LTV, analysts usually consider the gross profit from a customer per period.


<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/LTV.png" alt="LTV" width="70%"/>
</p>


#### How much money was spent? Overall/per source/over time
This time we are looking at the `marketing` table

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/monthly_costs.png" alt="monthly_costs" width="70%"/>
</p>

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/source_costs.png" alt="source_source" width="70%"/>
</p>

#### How much did customer acquisition from each of the sources cost (Customer Acquisition Costs)?

CAC Source

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/CAC_source.png" alt="CAC_source" width="70%"/>
</p>

CAC Cohort

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/CAC_cohort.png" alt="CAC_cohort" width="70%"/>
</p>


#### How worthwhile where the investments? (ROMI)

To find the ROI, or rather, ROMI (Return On Marketing Investment), we merge two dataframes that have cac and ltv columns which are cac and source dataframes,

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/ROMI_cohort.png" alt="ROMI_cohort" width="70%"/>
</p>

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/ROMI_source.png" alt="ROMI_source" width="70%"/>
</p>


***

Conclusion
In this step, we found the following findings:

Regarding user's visit:
On average, there are 908 unique visits per day, 5716 per week, and 23228 per month.
On the other hand, we can expect 987 sessions per day.
There was a significant increase in a visit during the end-of-year holiday, particularly on Black Friday
Most customers came to the site through ad source #3, #4, #5, and most of them using the desktop-based web interface.
On average, users spends 10.7 minutes on the site
There was a sudden drop in session length and daily visit indicating a problem on our end, possibly a server problem
The cohorts formed at the end of the year, especially Dec-17, have the best user retention rate.
Regarding sales:
Among all visitors, only 16% converted into buying customers. Mor than half of them converted at 0 days, and the highest cohort that has 0d conversion was the Oct-17 cohort
In terms of as source, source #3 and #4 had the highest 0d conversion
The Jun-17 cohort consistently produces a high volume of orders even until its 9th month. Another cohort that potentially able to match them is the Mar-18 cohort.
Ad source #1 and #2 produced large orders when compares to other sources.
The cohort that has the highest LTV is the Jun-17 followed by Sep-17 cohort, while source #1 and source #2 also generated high LTV score with 320 and 360, respectively
Regarding marketing:
The overall marketing cost in a year is USD329131. Most of them are used by ad source #3.
By evaluating the LTVs and CACs, we found out that ad source #1 and #2 are the most profitable, with ROMI of USD339 and USD1818, respectively.
Step 4: General Conclusion
We have learned from the analyses done in the previous step that the most profiting ad source is source #1. The source generated the highest return of investment (ROMI), USD339.94, with the lowest Customer Acquisition Costs (CAC), USD0.09 per user, and produced large volume orders. Ad source #2 also has a similar performance as #1, although on a smaller scale. On the other hand, source #3 has not been performing very well. Source #3 generated USD3.2 per user CAC, with the LTV only USD22, far below source #1 and #2.

No cohorts that have ROMI greater than 1 which indicate we haven't generated enough revenue to cover the marketing costs. Only two cohorts, Jun-17 and Sep-17 have the highest ROMI 0.53 and 0.59 respectively.

Hence, we recommend that marketing experts focus on ad sources #1 and #2 to increase revenue. It would be advisable to reallocate the marketing budget from source #3 to allow source v #1 to attract more customers.

We would also like to suggest that the best time to launch the marketing campaign is from October to March, where user visits peaked.

Black Friday is a unique date. In many of our metrics, the value around and on this day increased almost three times. So it is worthwhile to give extra attention to attract more users on this day




# Business Analysis


## Overview


This project focused on analyzing metrics in an ad campaign. The data that we had in hand were marketing expenses, server logs from June 2017 to May 2018, and orders for the period. Given these data, we would like to know how effective was our advertisement strategy and how much revenue it generated. We would also like to answer related questions such as know how people usede the product, when they start to buy, how much money each
customer brings, and when they pay off.

Our analysis started with the data preprocessing step that included replacing missing values, removing duplicates, and converting data types.
We got the three tables `visit`, `orders`, `cost` resulting from data cleaning. 

The `visit` table contained user's id, logs of when users started logging in, when they ended the session, and from which ad source they came to the site.

```python
visit.sample(5)
```
<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/visit.png" alt="visit" width="60%"/>
</p>

The `order` table showed users transaction, date of purchase and the revenue they generated

```python
order.sample(5)
```
<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/order.png" alt="order" width="40%"/>
</p>

Lastly, the `cost` table showed the marketing expenses spent for each ad source,

```python
cost.sample(5)
```
<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/cost.png" alt="cost" width="40%"/>
</p>

***
#### Daily, Weekly, Monthly Average Users
The first question we would like to know was how many people visited per day, week, and month. By grouping the `visit` dataset by its unique users, we created a daily average users table,

```python
# Grouping the dataframe on week and calculating the number of unique users
dau = visits.groupby(['year', 'month', 'day']).agg({'uid':'nunique'}).reset_index()

```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/dau.png" alt="percent_growth" width="75%"/>
</p>


The table shows that visitors came in more frequently towards the end of the year, starting in October 2017 until the first quarter of the new year in March 2018. On Black Friday, 24 November 2017, the customer visit reached the highest peak with 3319 visitors. There was a moment on 31 March 2018 where the number suddenly dropped 
to only one visit.

In general, the total number of visitors per day was 908.0 visitors.

Using the same apprach, we could also find the average weekly user table,

```python
# Grouping the dataframe on week and calculating the number of unique users
wau = visits.groupby(['year', 'week']).agg({'uid':'nunique'}).reset_index()

```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/wau.png" alt="dau" width="60%"/>
</p>

On average, we expected 5716 visits every week. However, we saw the same occurrence at the 47th week in 2017, where the visit suddenly jumped out. The number of visits stayed high throughout the end of 2017. There was an increase in the 12th week of 2018, which coincides with school spring break until it dropped to not much more than 5740 visits per week.

Finally, the monthly average user,

```python
# Grouping the dataframe on week and calculating the number of unique users
mau = visits.groupby(['year', 'month']).agg({'uid':'nunique'}).reset_index()
```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/mau.png" alt="mau" width="60%"/>
</p>

On average, we expected 23,228 visitors per month. The WAU graphs agreed with the weekly trend we discussed earlier. Toward the end of the year, the number of visitors increased. The graphs peaked in November 2017, where Black Friday takes place.

***
#### Sessions per Day
One user can visit the website more than once. With that in mind, we would like to know how many sessions per day from all users. 

```python
session = visits.groupby(['year', 'month', 'day']).agg({'uid':'count'}).reset_index()
```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/daily_sessions.png" alt="daily_sessions" width="70%"/>
</p>

The number of sessions per day is 987.4. On the previous step, we found out that the number of unique daily users was 908 visitors. On the other hand, the average daily session was 987.4. We calculated the ratio of `n_unique` visitors and the daily session and saw how it behaved throughout the year,

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/daily_sessions_ratio.png" alt="daily_sessions_ratio" width="70%"/>
</p>

Generally, the ratio of sessions over the visit was 1.08. In other words, eight users visited the website two times in one hundred users. This was a pretty small amount, so we could confidently say that users only saw the site once on regular days. However, during high season the percentage could reach 1.22.

Looking at the heatmap above, we saw that ad source #3, #4, and #5 brought the most visitors to the site. At the end of the year, source #4 made the highest visit with 13144 sessions. On the other hand, source #7 performed poorly by attracting not more than nine sessions in a month.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/sessions_per_source.png" alt="sessions_per_source" width="70%"/>
</p>

***

#### How Often Do Users Come Back (Retention Rate)

Retention rate is defined as the number of active users at different periods divided by the initial 
size of the same group. This metric is particularly useful in a subscription based service. It can give
us hint of how loyal our customers are.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/retention.png" alt="retention" width="70%"/>
</p>

The second-week retention for the first six weeks showed a relatively stable trend, despite the drop in Jul-17, with at least a 7.7% rate. At the beginning of the new year, the retention rate dropped around 2 percent lower to the level between 4.2-6.0%. The end of the festive season might caused this decrease where people returned to their normal daily activities.

The retention rate heatmap also showed us that most cohorts increased their rate in the 2nd month. Although not true for every cohort, generally, the retention rate rose steadily, especially for end-of-year cohorts where the number of retention improved.

***

#### When Do People Start Buying (Conversion Rate)
Conversion rate is the measure of how many users actually purchase our service over the number of users who visited the site.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/user_conversion.png" alt="user_conversion" width="70%"/>
</p>

The above graph showed that 25039 users put their orders right away after their first visit. One thousand nine hundred sixty-six users waited one day before making their purchase. Similarly, 58 users came back after a month, 41 users after 60 days, and so on.
The fact that most of the users converted on day one indicates that we had a strong product offering that attract users.
We broke down the metrics even more to see which ad source that brought the most revenue in.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/conversion_per_source.png" alt="user_conversion" width="70%"/>
</p>

Users that came to the site from ad source #3 and #4 are the group that most converted.

#### How Many Orders In A Given Period of Time?
It is also interesting to found out how many orders users made daily, weekly and monthly,

```python
# Calculating number of purchase per customer
(print('The average order per customer is {:.2f}'
       .format(orders.groupby('uid').count()['buy_ts'].sum()/ 
               orders.groupby('uid').count()['buy_ts'].shape[0])))


```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/daily_order.png" alt="daily_order" width="70%"/>
</p>

The average order per customer is 1.38. The daily heatmap showed that daily orders were usually at around 1.08-1.10 rate. However, there were days that the rate increased to above 1.20 in August-17 and March-18.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/weekly_order.png" alt="weekly_order" width="70%"/>
</p>

The weekly heatmap showed the peak, 48.11 orders per customer at the 8th week of 2018. That occurred at Feb 19-25, 2018. 

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/monthly_order.png" alt="monthly_order" width="70%"/>
</p>

The month with the highest orders per customer was May-18.

***

#### Lifetime Value (LTV)

LTV is the total amount of money a customer brings to the company on average by making purchases. 
In this case we calculated the LTV by dividing the revenue with the size of each cohort. 
LTV commonly presented in cumulative units to express how much revenue each cohort bring over time.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/LTV.png" alt="LTV" width="70%"/>
</p>

The cumulative LTV showed that cohort Jun-17 and Sep-17 had the highest score. It means users on each cohort brought 5.94 and 6.72 revenues respectively.

We could also evaluate the LTV per ad source,

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/LTV_per_source.png" alt="LTV" width="70%"/>
</p>

Ad source #1 and #2 brought the most revenue with USD320 and USD363 per user, respectively. There was also a peak on source #2 in Dec-2017, where the LTV was USD94 per user.

#### How much money was spent? Overall/per source/over time
We found out how much we money spent per month or per ad source as a step to calculate the customer acquisition cost (CAC), which we will calculate after this.
This time we are looking at the `marketing` table

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/monthly_costs.png" alt="monthly_costs" width="70%"/>
</p>


We can see above the marketing cost spent for each month. The overall cost for the whole year was USD329,131. 
The highest expenses paid were on October, November, and December, possibly to anticipate autumn and winter school breaks, as well as the Black Friday, Christmas, and New Year holidays.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/source_costs.png" alt="source_source" width="70%"/>
</p>

We paid USD141,321 on source #3, almost double the amount of money spent on other channels and take almost 43% of the marketing expenses. On the other hand, we only spent USD5500-5800 on source #9, #10.


#### How much did customer acquisition from each of the sources cost (Customer Acquisition Costs)?

The Customer Acquisition Cost tells us how much we spent on marketing budget to get a single customer. 
The value is the ratio between marketing costs and the number of customers who made an order over the same period.
It is an indication of how effective our spending strategy.

In the same way as the above analyses, we evaluated the CAC based on cohort and ad source,

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/CAC_cohort.png" alt="CAC_cohort" width="70%"/>
</p>


The heatmap showed the CAC over period of one year evaluated every month. 
On the 12th month source #3 absorbed USD141321.63 and only gained USD46766 to get the CAC of USD38.4 per user. 
Compare this to source #1 where it attracted 248662 users at the same period for only USD20833.27.

Calculating the CAC per ad source,

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/CAC_source.png" alt="CAC_source" width="70%"/>
</p>

From the chart above, we saw that ad source #3 has the highest CAC. We spent USD3 to get a single customer. The most effective channel is source #1, where we needed only to pay less than 1 cent to attract new customers.

#### How worthwhile where the investments? (ROMI)

Finally, we came to the most important metric in this analysis, the return of investment or in this case the return of marketing investment (ROMI).
It is the amount of LTV divided over CAC. Using this metric we can conclude whether our business activity is a worthwhile investments or not. 
Ideally we would like to see the ROMI of greater than 1.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/ROMI_cohort.png" alt="ROMI_cohort" width="70%"/>
</p>

From the ROMI heatmap we can conclude that no cohort that indicates a good investment, since most ROMI are below 1.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/ROMI_source.png" alt="ROMI_source" width="70%"/>
</p>

The table suggested that focusing our effort on ad source #1 and # 2 is a key to increase our revenue in the future. 
The ROMI values of these source far exceed the other sources where source #1 scored 21.97 and source #2 16.74. 
Other source that is also worthwhile to be considered is #5 where it scored 6.27 showing a relatively strong performance.


***

## Conclusion
In this step, we presented the following findings:

Regarding user's visit:

* On average, there were 908 unique visits per day, 5,716 per week, and 23,228 per month.
* On the other hand, we expected 987 sessions per day.
* There was a significant increase in a visit during the end-of-year holiday, particularly on Black Friday.
* Most customers came to the site through ad source #3, #4, #5, and most of them using the desktop-based web interface.
* On average, users spent 10.7 minutes on the site.
* There was a sudden drop in session length and daily visit indicating a problem on our end, possibly a server problem.
* The cohorts formed at the end of the year, especially Dec-17, have the best user retention rate.

Regarding sales:

* Among all visitors, only 16% converted into buying customers. More than half of them converted at 0 days, and the highest cohort that has 0d conversion was the Oct-17 cohort
* In terms of as source, source #3 and #4 had the highest 0d conversion
* The Jun-17 cohort consistently produced high volume orders even until its 9th month. Another cohort that potentially able to match them was the Mar-18 cohort.
* Ad source #1 and #2 produced large orders when compares to other sources.
* The cohort that had the highest LTV is the Jun-17 followed by Sep-17 cohort, while source #1 and source #2 also generated high LTV score with 320 and 360, respectively.


Regarding marketing:

* The overall marketing cost in a year was USD329131. Most of them are used by ad source #3.
* By evaluating the LTVs and CACs, we found out that ad source #1 and #2 are the most profitable, with ROMI of USD339 and USD1818, respectively.


We have learned from the analyses above that the most profiting ad source is source #1. The source generated the highest return of investment (ROMI), USD339.94, with the lowest Customer Acquisition Costs (CAC), USD0.09 per user, and produced large volume orders. Ad source #2 also had a similar performance as #1, although on a smaller scale. On the other hand, source #3 had not been performing very well. Source #3 generated USD3.2 per user CAC, with the LTV only USD22, far below source #1 and #2.

No cohorts had ROMI greater than 1 which indicated we haven't generated enough revenue to cover the marketing costs. Only two cohorts, Jun-17 and Sep-17 had high ROMI of 0.53 and 0.59 respectively.

Hence, we recommended that marketing experts *focus on ad sources #1 and #2* to increase revenue. It would be advisable to reallocate the marketing budget from source #3 to allow source #1 to attract more customers.

We also suggested that the best time to *launch the marketing campaign was from October to March*, where user visits peaked.

Black Friday is a unique date. In many of our metrics, the value around and on this day increased almost three times. So it is worthwhile to give extra attention to attract more users to spend on *Black Friday*.




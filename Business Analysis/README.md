# Business Analysis

## Overview
This project focuses on analyzing metrics in an ad campaign. The data that we had in hand were marketing expenses, server logs from June 2017 to May 2018, and orders for the period. Given these data, we would like to know how effective is our advertisement strategy and how much revenue it generates. We would also like to answer related questions such as know how people use the product, when they start to buy, how much money each
customer brings, and when they pay off.

Our analysis started with the data preprocessing step that included replacing missing values, removing duplicates, and converting data types.
We got the three tables `visit`, `orders`, `cost` resulting from data cleaning. 

The `visit` table contains logs of when users started logging in and when they ended the session, their user's id, and from which ad source they came to our site.

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


***
One user maight have more than one session. We need to count how many sessions are there per day

One importaant metric to find out is how often do users come back?
		
####		Sales
    * 		When do people start buying? (In KPI analysis, we're usually interested in knowing the time that elapses between registration and conversion — when the user becomes a customer. For example, if registration and the first purchase occur on the same day, the user might fall into category Conversion 0d. If the first purchase happens the next day, it will be Conversion 1d. You can use any approach that lets you compare the conversions of different cohorts, so that you can determine which cohort, or marketing channel, is most effective.)
    * 		How many orders do they make during a given period of time?
    * 		What is the average purchase size?
    * 		How much money do they bring? (LTV)	

####		Marketing
    * 		How much money was spent? Overall/per source/over time
    * 		How much did customer acquisition from each of the sources cost?
    * 		How worthwhile where the investments? (ROI)
* 		
Plot graphs to display how these metrics differ for various devices and ad sources and how they change in 
time.


Step 3. Write a conclusion: advise marketing experts how much money to invest and where.
What sources/platforms would you recommend? Back up your choice: what metrics did you focus on? Why? What conclusions did you draw after finding the metric values?

#### Sticky Factor per Week
```python
# Merging dau with wau
sticky_factor_week = pd.merge(dau_with_week, wau, on=['year','week'])

sticky_factor_week['sf_per_week'] = (sticky_factor_week['n_user_day']/sticky_factor_week['n_user_week'])*100
sticky_factor_week.head()

```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/sticky_factors_week.png" alt="sticky_factors_week" width="80%"/>
</p>


#### Sticky Factor per Month
```python
# Sticky factor per week (SFPW)
sticky_factor_month = pd.merge(dau, mau, on='month')

sticky_factor_month['sf_per_month'] = (sticky_factor_month['n_user_day']/sticky_factor_month['n_user_month'])*100
sticky_factor_month.head()
```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/sticky_factors_week.png" alt="sticky_factors_month" width="80%"/>
</p>


#### Retention Rate
In this section, we want to know the retention rate of our users. 

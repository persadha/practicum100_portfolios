# Making Business Decision Based On Data

## Introduction

The first analysis we would like to find out is how many people visited our site and how the user spend their time in the site.

#### Daily Average Users
```python
# Grouping the dataframe on week and calculating the number of unique users
dau = visits.groupby(['year', 'month', 'day']).agg({'uid':'nunique'}).reset_index()

# Renaming the columns
dau.columns = ['year', 'month', 'day', 'n_user_day']
dau.head()
```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/dau.png" alt="percent_growth" width="80%"/>
</p>


The heat map shows how many users visited the website every day for one year period. In general, visitors 
came in more frequently towards the end of the year, starting in October 2017 until the first quarter of 
the new year in March 2018. On Black Friday, 24 November 2017, the customer visit reaches the highest peak 
with 3319 visitors. On the other hand, there is a moment on 31 March 2018 where the number suddenly dropped 
to only one visit.

In general the total number of visitors per day is 908.0 visitors

#### Weekly Average Users
```python
# Grouping the dataframe on week and calculating the number of unique users
wau = visits.groupby(['year', 'week']).agg({'uid':'nunique'}).reset_index()

# Renaming the columns
wau.columns = ['year', 'week', 'n_user_week']
wau.head()
```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/wau.png" alt="dau" width="80%"/>
</p>

The graphs show the weekly average users of the website. On average, we can expect 5716 visits every week. However, we can see the same occurrence at the 47th week in 2017 where the visit suddenly jumped out. The number of visits stays high throughout the end of 2017. There was an increase in the 12th week of 2018, which coincides with school spring break until it drops to not much more than 5740 visits per week.


#### Monthly Average Users
```python
# Grouping the dataframe on week and calculating the number of unique users
mau = visits.groupby(['year', 'month']).agg({'uid':'nunique'}).reset_index()

# Renaming the columns
mau.columns = ['year', 'month', 'n_user_month']
mau.head()
```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Business%20Analysis/images/mau.png" alt="mau" width="80%"/>
</p>


On average, we can expect 23,228 visitors per month. The WAU graphs agree with the weekly trend we 
discussed earlier. Toward the end of the year, the number of visitors increased. The graphs peaked in 
November 2017, where Black Friday takes place.

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
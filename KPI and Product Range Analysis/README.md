# Evaluating Sales KPI and Product Range Analysis
#### The final project of the Practicum100 Germany Data Analyst Bootcamp. 

### Description
In this project, we were given sales data of 4,223 items in a store for a year period (2018-2019).
The dataset contains of 541,909 rows with 25,900 unique invoice numbers, item quantities, their unit prices, among other 
features. We were asked to assess the sales performance by selecting relevant key performance indicators (KPI) then 
evaluate how they developed. For this purpose we selected the following metrics:
- Monthly Revenue
- Monthly Sales Growth
- Average Check
- Average Revenue Per Paying User (ARPPU)
- Average Revenue Per Unit (ARPU)
- Product Performance

Then in the second part of the project, we evaluated the product range by grouping the items into several categories 
then find which products that were usually sold by themselves or in bundles. We used a technique commonly used in NLP 
called Topic Modelling to make the categories. Finally, we also wanted to find out whether the items usually sold in bundles 
generate more revenues than those usually sold individually. The last part of the project was to build a simple recommender system that can suggest customers what items they should 
buy next based on their past selections.

### KPI Analysis
#### Monthly Revenue

Monthly Revenue is the amount of money that we made from sales 
every month. We got the figure by multiplying the number of items 
sold on each invoice per month by the item's unit price.
~~~python
# Finding the monthly revenue
revenue = clean_data.groupby(['year', 'month']).agg({'revenue':'sum', 'invoice_date':'min'}).reset_index()
revenue['label'] = revenue['invoice_date'].dt.strftime('%Y-%m')
revenue
~~~

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/monthly_revenue.png" alt="monthly_revenue" width="100%"/>
</p>

From November 2018 to December 2019 the revenue grew around 3% with the highest groeth in November 2019 
where the sales made $1,483,942 revenue. There were some periods where revenues were underperformed however the biggest 
decline would be on December 2019 with -0.66% loss to $507470 which is a lso the current state of this metric.

#### Percent Growth
Percent Growth shows how much revenue changes every month in percentage compared to the previous month's. 
A positive value means that we earn more income than last month. In a same way, a negative value means 
we lost money. The figure is calculated by the change of revenue each month divided by the previous 
month's income.

~~~python
# Calculating percent growth
revenue['growth'] = revenue['revenue'].pct_change().fillna(0)
revenue['color'] = revenue['growth'].apply(lambda x: 'firebrick' if x < 0 else 'forestgreen')
revenue
~~~

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/percent_growth.png" alt="percent_growth" width="100%"/>
</p>

Despite having an average of 1% growth, the store saw 5.39% as the highest growth in December 2018. Currently the growth is
at -0.66% level.

#### Average check

The average check is the number of invoices in a month divided by the number of paying customers. 
This indicator shows how many transactions per person made in a particular month.
To find the number of invoices in amount, we first group the data by year, month, and invoice_no and 
count only the number of invoices with quantity at least one.

~~~python
# Merging the dataframes
check = pd.merge(invoice, customer, on=['year', 'month','label', 'invoice_date'], how='left')
check.columns = ['year', 'month', 'n_invoice', 'invoice_date', 'label', 'n_customer']

# Calculating the average check
check['check'] = check['n_invoice']/check['n_customer']
check
~~~

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/average_check.png" alt="average_check" width="100%"/>
</p>

The average check for the sales is 2.3 and currently at 2.3. The highest value recorded is on November 2019 with 3.71 

#### ARPPU

Average Revenue Per Paying User (ARPPU) is the metric that shows how much money a single user makes. The figure is calculated by dividing the revenue by the number of paying customers per month.


~~~python
arppu = pd.merge(revenue, customer, on=['year', 'month', 'invoice_date', 'label'], how='left')
arppu.columns = ['year', 'month', 'revenue', 'invoice_date', 'label', 'growth', 'color', 'n_customer']
arppu['arppu'] = arppu['revenue']/arppu['n_customer']
arppu
~~~

#### ARPU

Average Revenue Per Unit is the amount of money we can expect to receive from selling a single product. It is calculated by dividing the monthly revenue by the number of products sold in a month.
~~~python
# Merging the dataframes and calculating the ARPU
arpu = pd.merge(quantity, revenue, on=['year', 'month', 'invoice_date', 'label'], how='left')
arpu['arpu'] = arpu['revenue']/arpu['quantity']
arpu
~~~

#### Product Performance
chart 1
chart 2
chart 3

~~~python
# Merging the dataset
kpi = pd.merge(arppu, check, on=['year', 'month', 'invoice_date', 'n_customer', 'label'], how='left')
kpi = pd.merge(kpi, arpu, on=['year', 'month', 'invoice_date', 'label', 'growth', 'color', 'revenue'], how='left')

# Rearranging the columns
kpi = kpi[['year', 
             'month', 
             'invoice_date', 
             'label', 
             'n_customer', 
             'revenue', 
             'growth', 
             'n_invoice', 
             'quantity', 
             'check', 
             'arppu', 
             'arpu']]
kpi.head()
~~~


We have calculated the monthly revenue, monthly growth, average check, average revenue per paying user (ARPPU), average revenue per unit (ARPU), and product performance. From the indicators, we learned that the sales performed very well; it scored 5.3% growth in December 2018 and kept growing until November 2019 where it reached the highest revenue of 1,483,943. The average revenue per paying unit (ARPPU) also scored the highest this month, with 1891 points. However, the following month showed an interesting phenomenon where sales dropped -0.66% and only earned 507,470. The average check and ARPPU also present a similar trend. The ARPU, on the other hand, is relatively steady throughout the year, with an average of 2.21 points. We have also displayed the top 3, 10, and 25 products based on revenue in the last section of this analysis.

In November 2019, most of our customers bought Christmas decorations, which boost revenue. Furthermore, it also explains why the revenue dropped drastically in the following month.


## Product Range Analysis

![pyLDAviz](pyLDAviz_img.png "Product Range")


[Dashboard](https://public.tableau.com/app/profile/widianto.persadha/viz/KPI_16275523309270/Dashboard?publish=yes)



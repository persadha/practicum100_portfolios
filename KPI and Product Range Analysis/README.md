# Evaluating Sales KPI and Product Range Analysis
#### The final project of the Practicum100 Germany Data Analyst Bootcamp. 
***
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

***
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
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/monthly_revenue.png" alt="monthly_revenue" width="90%"/>
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
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/percent_growth.png" alt="percent_growth" width="90%"/>
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
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/average_check.png" alt="average_check" width="90%"/>
</p>

The average check for the sales is 2.3 and currently at 2.3. The highest value recorded is on November 2019 with 3.71 

#### ARPPU

Average Revenue Per Paying User (ARPPU) is the metric that shows how much money a single user makes. 
The figure is calculated by dividing the revenue by the number of paying customers per month.

~~~python
arppu = pd.merge(revenue, customer, on=['year', 'month', 'invoice_date', 'label'], how='left')
arppu.columns = ['year', 'month', 'revenue', 'invoice_date', 'label', 'growth', 'color', 'n_customer']
arppu['arppu'] = arppu['revenue']/arppu['n_customer']
arppu
~~~

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/arppu.png" alt="arppu" width="90%"/>
</p>

The average ARPPU is 1115 and the highest ARPPU was found on November 2019 with 1883.

#### ARPU

Average Revenue Per Unit is the amount of money we can expect to receive from selling a single product. 
It is calculated by dividing the monthly revenue by the number of products sold in a month.


~~~python
# Merging the dataframes and calculating the ARPU
arpu = pd.merge(quantity, revenue, on=['year', 'month', 'invoice_date', 'label'], how='left')
arpu['arpu'] = arpu['revenue']/arpu['quantity']
arpu
~~~

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/arpu.png" alt="arpu" width="75%"/>
</p>

The sales generated a relatively stable revenue per unit, which on average is 1.82. There was a 
relatively high figure in December 2018, where the ARPU was 2.21 points.

#### Product Performance
The product performnace is basically showing the number of top n products based on revenue.

~~~python
def show_top(n_highest):
    '''
    Description:
    Display of top n products based on revenue.
    '''
    result = (clean_data
              .groupby(['stock_code', 'description'])
              .sum()['revenue']
              .sort_values(ascending=False)
              .head(n_highest).reset_index()
             )
    fig, ax = plt.subplots(figsize=(10,round(n_highest*0.5)), dpi=75)
    sns.barplot(data=result, y=result.description.map(lambda x: x.title()), x='revenue', color='teal', orient='h')
    
    for row in result.itertuples():
        ax.text(row.revenue+10000,
                row.Index,
                s=round(row.revenue, 2), 
                horizontalalignment='center',
                verticalalignment='bottom')
    plt.title('The Top {} Products Based on Revenue'.format(n_highest), fontdict={'size':16})
    plt.xlabel('Revenue')
    plt.xlim(0, result.revenue.max()+25000)
    plt.ylabel('Item Name')
    plt.show()

# Showing the top 25 products
show_top(25)
~~~

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/product_performance.png" alt="product_performance" width="100%"/>
</p>

The above figures shows the 3, 10, and 25 top products based on the revenue. Among the top performers 
are `Regency Cakestand 3 Tier`, `Paper Craft`, `Little Birdie`, and `White Hanging Heart T-Light Holder`.

Three KPIs show that the most profitable month is November 2019, where it scored the highest 
revenue, check, and ARPPU. After looking deeper into the data, we found that this trend was driven by 
Christmas related items. Customers started to make Christmas decorations for gifts in November and bought 
the supplies at this time of the year. 
This also explains why the revenue in December 2018 dropped. All decorations and gifts supposedly were already handed out and fewer people searched for these supplies.

***
## Product Range Analysis

![pyLDAviz](pyLDAviz_img.png "Product Range")


[Dashboard](https://public.tableau.com/app/profile/widianto.persadha/viz/KPI_16275523309270/Dashboard?publish=yes)



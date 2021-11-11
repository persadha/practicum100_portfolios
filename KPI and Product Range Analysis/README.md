Evaluating Sales KPI and Product Range Analysis
The final project of the Practicum100 Germany Data Analyst Bootcamp. 

The project's main task was to do a KPI and project range analysis to see how sales are performing in a year. In this project, I used different techniques to clean and visualize the dataset. As a part of the task, we were asked to put the product items into larger groups. I used the Topic Modelling techniques to find the most common product categories using the item's description as an input. The final number of categories was chosen by evaluating the coherence value of a given range of the possible number of topics. I also did hypothesis testing to check the statistical significance between two groups in the dataset. Finally, recommendations were presented in the last section of the notebook.


![pyLDAviz](pyLDAviz_img.png "Product Range")

#### Revenue
~~~python
# Finding the monthly revenue
revenue = clean_data.groupby(['year', 'month']).agg({'revenue':'sum', 'invoice_date':'min'}).reset_index()
revenue['label'] = revenue['invoice_date'].dt.strftime('%Y-%m')
revenue
~~~

#### Percent Growth

#### Average check
~~~python
# Finding the number of invoices per month
invoice = (clean_data
           .groupby(['year', 'month', 'invoice_no'])
           .agg({'quantity':'count', 'invoice_date':pd.Series.min})
           .reset_index()
           .query('quantity > 0')
           .groupby(['year', 'month'])
           .agg({'invoice_no':'count', 'invoice_date':'min'})
           .reset_index()
        )
invoice['label'] = invoice['invoice_date'].dt.strftime('%Y-%m')
invoice
~~~


~~~python
# Finding the number of customers per month
customer = (clean_data
            .groupby(['year', 'month', 'customer_id'])
            .agg({'invoice_no':'count', 'invoice_date':'min'})
            .reset_index()
            .query('invoice_no > 0')
            .groupby(['year', 'month'])
            .agg({'customer_id':'count', 'invoice_date':'min'})
            .reset_index()
            )
customer['label'] = customer['invoice_date'].dt.strftime('%Y-%m')
customer
~~~

~~~python
# Merging the dataframes
check = pd.merge(invoice, customer, on=['year', 'month','label', 'invoice_date'], how='left')
check.columns = ['year', 'month', 'n_invoice', 'invoice_date', 'label', 'n_customer']

# Calculating the average check
check['check'] = check['n_invoice']/check['n_customer']
check
~~~

#### ARPPU
~~~python
arppu = pd.merge(revenue, customer, on=['year', 'month', 'invoice_date', 'label'], how='left')
arppu.columns = ['year', 'month', 'revenue', 'invoice_date', 'label', 'growth', 'color', 'n_customer']
arppu['arppu'] = arppu['revenue']/arppu['n_customer']
arppu
~~~

#### ARPU
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

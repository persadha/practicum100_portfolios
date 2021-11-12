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
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/monthly_revenue.png" alt="monthly_revenue" width="80%"/>
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
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/percent_growth.png" alt="percent_growth" width="80%"/>
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
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/average_check.png" alt="average_check" width="80%"/>
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
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/arppu.png" alt="arppu" width="80%"/>
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
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/arpu.png" alt="arpu" width="80%"/>
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
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/product_performance.png" alt="product_performance" width="80%"/>
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
### Finding Items Commonly Sold In Bundles
For this task, we use the fact that there were already keywords in the product's name that indicated 
if an item are sold in group. Those words are `set of`, `box of`, `pack of`, `bundle of`.

```python
# Creating a boolean column to find a row that consist of the keywords
keywords = ['box of', 'set of', 'pack of', 'bundle of']

clean_data['is_bundle'] = (clean_data['description']
                           .apply(lambda x: True if any(w in x for w in keywords) else False))


from collections import Counter
Counter(clean_data.query('is_bundle == True')['description']).most_common(5)
```

<p align="left">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/bundles.png" alt="bundles" width="50%"/>
</p>


### Find Product Categories

The dataset provided us with the item names under the `description` column. There was, however, no other 
columns in the dataset that gave information which category an item belongs to. Consequently, we had to 
consider other methods to help us in grouping the items.

In the Natural Language Processing domain, a technique called Topic Modelling aims to discover the topic 
in a collection of documents, which is in this case quite similar to our objective. The model uses the 
method known as Latent Dirichlet Allocation (LDA), which assumes that a document consists of a collection 
of topics in a certain proportion. And each topic is a collection of keywords in a certain proportion. 
The method groups keywords based on the given suggested number of topics and calculate how much each 
keyword contributes to a particular topic.

We implemented Topic Modelling in this analysis with the help of a library called Gensim. We also used 
In addition, we pyLDAviz for visualizing the categories produced by Gensim. 

~~~python
# Importing Gensim library
import gensim
from gensim import corpora, models
from gensim.utils import simple_preprocess

# Importing pyLDAviz for visualization
import pyLDAvis
import pyLDAvis.gensim_models
~~~

The steps that we took in the analysis were the following:
1. Preprocessed the input text by tokenized the text into a smaller unit, removed the stop words, stemmed, and 
lemmatized the remaining words.
2. Created a Gensim dictionary and a corpus as a main input for the LDA model.
3. Ran the LDA model by giving the model an initial guess of the number of topics, the dictionary, 
and the corpus.
4. Visualized the model output using pyLDAviz.
5. Evaluated the model output by calculating the coherence model.
6. Repeated the modeling part using a different number of topics until we found the best coherence model.

To achieve the expected result we defined a function as follows:

~~~python
def get_corpus(df):
    words = [preprocess(line) for line in df.description]
    id2word = gensim.corpora.Dictionary(words)
    
    # Filter out any tokens that occur less than 10 times and no more that 35% of the corpus size
    id2word.filter_extremes(no_below=10, no_above=0.35)
    
    # Removing any gaps
    id2word.compactify()
    
    # Creating the corpus
    corpus = [id2word.doc2bow(text) for text in words]
    return corpus, id2word, words
~~~

Then feeded the item decriptions from the cleaned dataset as input to `get_corpus`.

~~~python
np.random.seed(0)

# Creating a corpus and dictionary from the input text
corpus, id2word, words = get_corpus(clean_data)
~~~

Finally, we ran the LDA model using the corpus and dictionary id2word from get_corpus. The model requires 
prior knowledge of the number of topics the document might have. As an initial guess, we assumed that 
there were 10 product categories.

~~~python
# Running the LDA model
lda_model =  gensim.models.LdaMulticore(corpus, num_topics=10, id2word = id2word, passes=2, workers=2)

from pprint import pprint

# Visualizing outputs from the model
for idx, topic in lda_model.print_topics(-1):
    pprint('Topic: {} Words: {}'.format(idx, topic))
~~~

The LDA model gives out ten topics with the probability of words that makes up each topic. As an example, 
in Topic 0 the 'light' keyword contributes 11.7% to the topic, 'heart' contributes 9%, 'decor' 8.5%, and 
so on.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/gensim_output.png" alt="gensim_output" width="80%"/>
</p>

One important thing to note is that the LDA model does not assign any title to the output topics. 
Therefore, we would need to name them manually later.

We used pyLDAviz to visualize the results.

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/pyLDAviz.png" alt="pyLDAviz_img.png" width="80%"/>
</p>

The visualization showed several bubbles and bars charts in the main window. Each bubble represents a 
topic; the larger the bubble, the higher percentage of the words in the corpus is about that topic. 
Blue bars represent the overall frequency of each word in the corpus. Red bars give the estimated number 
of times a given topic generates a given term. For example, there are 40,000 counts of the word 'heart' 
in the corpus, as indicated by the blue bar. Topic 4 generates around 22,000 of them and Topic 2 around 
18,000, as shown by the red bars.

Additionally, the further the bubbles are away from each other, the more different they are.

The model gave us ten bubbles of the same size. Some bubbles intersected with the other, indicating that 
there are keywords found in more than one bubble.

To evaluate how good our selection of the number of topics is, we use a coherence model metric that came 
with the Gensim library.

```python
from gensim.models import CoherenceModel
```

<p align="left">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/coherence_score.png" alt="coherence_score.png" width="80%"/>
</p>

Our selection of 10 topics gave us around 58% coherence. To find the best number of categories, we 
can repeat the modeling process with different value for number of topic and checked the resulting
coherence value. In this case we started with the initial 2 topics then repeated the process until
40 topics.

```python
np.random.seed(0)

def compute_coherence_values(dictionary, corpus, texts, limit, start=2, step=3):
    """
    Description:
    Compute c_v coherence for various number of topics

    Inputs:
    dictionary : Gensim dictionary
    corpus : Gensim corpus
    texts : List of input texts
    limit : Max num of topics

    Ouputs:
    model_list : List of LDA topic models
    coherence_values : Coherence values corresponding to the LDA model with respective number of topics
    """
    coherence_values = []
    model_list = []
    for num_topics in range(start, limit, step):
        model = gensim.models.LdaMulticore(corpus=corpus, num_topics=num_topics, id2word=id2word)
        model_list.append(model)
        coherencemodel = CoherenceModel(model=model, texts=texts, dictionary=dictionary, coherence='c_v')
        coherence_values.append(coherencemodel.get_coherence())

    return model_list, coherence_values

model_list, coherence_values = compute_coherence_values(dictionary=id2word, corpus=corpus, texts=words, start=2, limit=40, step=4)
```

Plotting the coherence values,

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/coherence_plot.png" alt="coherence_plot.png" width="80%"/>
</p>

<p align="left">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/coherence_highest.png" alt="coherence_highest.png" width="50%"/>
</p>

Our iterative process suggested that selecting 26 topics give the best coherence score of around 60%. However, 
since this value was not so much different that the one we got initially (58%) we will stick 10 product categories. 
Surely it would help customers a lot by not confusing them with too many product groups.

The final step is to assign the category to each item in the description column. We can use the model and corpus that 
have been trained to assign the groupings.

```python
def topic_prediction(item):
    '''
    Description:
    Find the topic the input text most associated with
    
    Parameters:
    A string type text input
    
    Returns
    The Topic with the highest probability
    '''
    text = preprocess(item)
    corpus = id2word.doc2bow(text)
    output = list(lda_model[corpus])
    topics = sorted(output, key=lambda x: x[1], reverse=True)
    return topics[0][0]

clean_data['category'] = clean_data['description'].apply(topic_prediction)
clean_data
```

<p align="center">
<img src="https://github.com/persadha/practicum100_portfolios/blob/main/KPI%20and%20Product%20Range%20Analysis/images/product_categories.png" alt="product_categories.png" width="90%"/>
</p>

Even though we picked 10 categories for the product range, our selection was far from perfect. 
There were still many ways to improve our model, such as choosing a different hyperparameter, applying the 
TF-IDF method, utilizing the Gensim LDA Mallet model as suggested by some references, or using the output 
categories as target labels and run a supervised machine learning method to the dataset. However, due to 
time restrictions, we kept these options as future recommendations. One caveat of the technique is that 
the model does not assign the naming to the output topics/categories.

[Dashboard](https://public.tableau.com/app/profile/widianto.persadha/viz/KPI_16275523309270/Dashboard?publish=yes)


## Hypothesis Testing
This section focused on finding out 
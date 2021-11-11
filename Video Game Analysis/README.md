# Video Games Analysis

The main objective of the project is to analyze the sales data of video game titles sold in north america, europe,
and other regions in the world. We would like to answer interesting business questions such as:
- What is the best-selling games of all time in all platforms?
- Which platforms that are leading sales? 
- Are there any correlations between total sales and critic's reviews?
- What is the most profitable genre?

The dataset consisted of 16715 rows by 11 columns, and six of them had missing values. 
The column titles are `Name`, `Platform`, `Year_of_Release`, `Genre`, `NA_sales`, `EU_sales`,`JP_sales`, `Other_sales`, 
`Critic_Score`, `User_Score`, and `Rating`. We used an external dataset from _GameRankings.com_ to help us filled in the missing data. 
Additional data preprocessing brought us to a conclusion that only 43% of the original data that were usable for analysis.

![Preview](https://github.com/persadha/practicum100_portfolios/blob/main/Video%20Game%20Analysis/data_snapshot.png)

<img src="https://github.com/persadha/practicum100_portfolios/blob/main/Video%20Game%20Analysis/data_snapshot.png" alt="drawing" width="50%"/>

Our data exploration showed us that PlayStation2 was the platform that has the highest selling figures and made the
highest revenue, with USD973 million. This figure is followed by XBox 360 and Nintendo 3DS. The top three games that 
gave PlayStation 2 the highest revenue were 'Grand Theft Auto: San Andreas' (released in 2004), 'Grand Theft Auto: Vice City' (2002), and Gran Turismo 3: A-Spec' (2001).  


![Preview](https://github.com/persadha/practicum100_portfolios/blob/main/Video%20Game%20Analysis/total_sales_per_platform.png)

We also found out that even though PlayStation 2 was the top-selling platform, PlayStation3 has the highest growth with 
around 1.5 growth above the average of all platforms. We also discovered other interesting insights such as how
critic's and user's reviews affect the total sales, how the same title perform in different platforms, what is the 
most profitable genre and how different platforms contributes to the sales of each genres.

Then we compared the sales in North America, Europe and Japan. XBox led the sales in North America with $359 million, 
followed by PlayStation 3 and Nintendo Wii. In Europe and Japan, on the other hand, sales were dominated by PlayStation 3
with $211 million and $46 million respectively. Xbox 360 came at the second position in Europe with $170 million and 
Nintendo 3DS in Japan with $34 million.

![Preview](https://github.com/persadha/practicum100_portfolios/blob/main/Video%20Game%20Analysis/sales_in_different_regions.png)


Customers in North America and Europe prefer Action, Shooter, and Sports games as those dominate the market almost with 60% share. 
In Japan, Role-Playing Games are the most popular, followed by Action with the total combined of 50% market share.

![Preview](https://github.com/persadha/practicum100_portfolios/blob/main/Video%20Game%20Analysis/enres_in_different_regions.png)

In the last section, we would lie to compare the average user's review score between XBox and PC, 
and between the action and sport genres. Using hypothesis testing between the two distribution means, we found out that 
there were no significant difference between the Xbox user's reviews to that of PC's. On the other hand, 
there were significant evidence of different user's review score mean between the action and sport genres.

Finally, we drew some business recommendations based on our analysis for the following year (2017) forecast:

- PlayStation2 is the best-selling platform of all time. However, we didn't include it in our analysis for 2017 since the platform has been discontinued.
- We focused the analysis more on PlayStation 4 because, among the current platforms generating sales, PlayStation 4 is the only platform that is still active. The other platforms, PlayStation 3, Xbox 360, and Nintendo Wii, either have been officially discontinued or reaching the end of their lifespan.
- We need to pay attention to expert review since there is a moderate correlation to total sales.
- To gain more sales, we need to emphasize ins selling games from the Action, Shooter, and Role Playing genres as they are the top-selling genre in all regions.
- Additionally, the hypothesis test indicates that the user's reviews between the Xbox One and PC genres are the same, and the user's reviews between the Action and Sports genres are different.

The complete notebook could be found here [here](/Users/w.persadha/Documents/GitHub/practicum100_portfolios/Video Game Analysis/data_snapshot.png)

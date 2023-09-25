# Exploring the Relationship Between Connotation of News Articles and Company Revenue Using Sentiment Analysis
Group 10 <br />
**Project 1 Leader:** Sujith Panchumarthy <br />
**Members:** Carson Crenshaw, Zoe Averill <br />
DS 4002 <br />
Last Updated: 09/24/2023

## Description

MI3 deliverable for the first project cycle of DS 4002. <br />

The objective of this project is to test whether there is an inverse relationship between the sentiment orientation of news articles and the profit revenues of news organizations, hypothesizing that more negative articles will lead to higher revenues. This project group will attempt to fulfill this objective by answering the questions _To what extent do negative news headlines correlate with revenue generation? and Can the average yearly directionality of sentiment for a company’s news articles predict their annual revenue generation?_ The overall sentiment of a news company’s articles will be calculated to compare directional attitude scores with the overall pattern of revenue growth for the company through both a sentiment analysis and a predictive regression model. 

## Contents of the Repository

The contents of this repository include the source code, data, figures, license, and references for MI3. 

## [SRC](https://github.com/C-Crenshaw/Project1_DS4002/tree/b1542e32aed9d06d4183589f6be21cb49bb49c3e/SRC)

This section contains all the source code for the project. 

### Installing/Building Code

* How/where to download your program
* Any modifications needed to be made to files/folders

### Usage

* How to run the program
* Step-by-step bullets
```
code blocks for commands
```

## [Data](https://github.com/C-Crenshaw/Project1_DS4002/tree/9814e54ec5d0e0119e89d199a6f0073ec55778e4/DATA)

This section contains all of the data for this project. Data dictionaries are provided below and are organized by dataset. Relevant notes about use of data are also included. 

The original dataset ('nyt-metadata.csv') is pulled from @aryansingh0909 on [Kaggle](https://www.kaggle.com/datasets/aryansingh0909/nyt-articles-21m-2000-present). This dataset contains a comprehensive collection of over 2.1 million articles from The New York Times spanning from January 1, 2000 to the present day [1]. The original dataset’s size is approximately 5 GB which was too large to handle and manipulate in any meaningful ways in a reasonable amount of time. As such, a random sample of roughly 1% was used which reduced the size to 6.12 MB. Due to the size of the original population, this is still a valid and representative sample size. The other major contributor to the size reduction was removing unnecessary columns which left 10 columns from the original 20. 

The updated dataset ('nyt-metadata-SAMPLE.csv') is a cleaned version of the original and is largely used throughout the analysis. Within the python file it is used/uploaded in excel format, but can also be downloaded as a .csv when cleaned. 

**nyt-metadata-SAMPLE.csv**
| 	Column Name	 | 	Description	 | 	Data Type	 |  
| 	:-----:	 | 	:-----:	 | 	:-----:	 |
| 	headlines	| 	Title or heading of the article	| 	String	 | 
| 	abstract	| 	Brief summary or description of the article's content	| 	String	 | 
| 	lead_paragraph	| 	Introductory paragraph of the article	| 	String	 | 
| 	print_section	| 	Section in the print version of the newspaper where the article was published	| 	String/Integer	 | 
| 	print_page	| 	Page number in the print version of the newspaper where the article was published	| 	Integer	 | 
| 	pub_date	| 	Date the article was published	| 	Datetime	 | 
| 	news_desk	| 	Department responsible for the article	| 	String	 | 
| 	section_name	| 	Section or category of the article	| 	String	 | 
| 	type_of_material	| 	Type of article	| 	String	 | 
| 	word_count	| 	Number of words in the article	| 	Integer	 | 

After running the sentiment analysis on the sample data, a new dataset is generated ('nyt-sentiment.csv'). <br /> 

**nyt-sentiment.csv**
| 	Column Name	 | 	Description	 | 	Data Type	 |  
| 	:-----:	 | 	:-----:	 | 	:-----:	 |
| 	headlines	| 	Title or heading of the article	| 	String	 | 
| 	abstract	| 	Brief summary or description of the article's content	| 	String	 | 
| 	lead_paragraph	| 	Introductory paragraph of the article	| 	String	 | 
| 	print_section	| 	Section in the print version of the newspaper where the article was published	| 	String/Integer	 | 
| 	print_page	| 	Page number in the print version of the newspaper where the article was published	| 	Integer	 | 
| 	pub_date	| 	Date the article was published	| 	Datetime	 | 
| 	news_desk	| 	Department responsible for the article	| 	String	 | 
| 	section_name	| 	Section or category of the article	| 	String	 | 
| 	type_of_material	| 	Type of article	| 	String	 | 
| 	compound_sentiment	| 	Aggregate degree of headline sentiment	| 	Integer	 | 
| 	sentiment_direction	| 	Directionality of headline sentiment	| 	Integer	 | 
| 	pub_year	| 	Publication year of the article	| 	Integer	 | 

Annual New York Times revenue data ('New York Times Revenue.xlsx') is pulled from [StockAnalysis.com](https://stockanalysis.com/stocks/nyt/revenue/). This dataset is downloaded directly and stored in an excel formatting. <br />

**New York Times Revenue.xlsx**
| 	Column Name	 | 	Description	 | 	Data Type	 |  
| 	:-----:	 | 	:-----:	 | 	:-----:	 |
| 	Year	| 	End of fiscal year	| 	Datetime	 | 
| 	Revenue (in Billions)	| 	NYT revenue generated that year	| 	Integer	 | 
| 	Change (in Millions)	| 	Change in revenue between the present and previous year	| 	Integer	 | 
| 	Growth	| 	Percentage change between present and previous year	| 	Integer	 | 
| 	CPI	| 	Consumer Price Index	| 	Integer	 | 
| 	Revenue_Adjusted (in 2022 dollars)	| 	NYT revenue adjusted for inflation	| 	Integer	 | 

The final NYT revenue and headline sentiment dataset ('nyt_sentiment_revenue.csv') is assembled from the previous datasets. Combining the NYT revenue data and the sentiment analysis run on the sample data, the new dataset is created and will be used in a regression analysis. This dataset will includes the year, NYT revenue, average sentiment score for each component, and the direction of attitude for each year (2000-2022). <br />

**nyt_sentiment_revenue.csv**
| 	Column Name	 | 	Description	 | 	Data Type	 |  
| 	:-----:	 | 	:-----:	 | 	:-----:	 |
| 	Year	| 	End of fiscal year	| 	Integer	 | 
| 	compound_sentiment	| 	Aggregate degree of headline sentiment	| 	Integer	 | 
| 	sentiment_direction	| 	Directionality of headline sentiment	| 	Integer	 | 
| 	Adjusted 2022 Revenue in Billions USD	| 	NYT revenue adjusted for inflation	| 	Integer	 | 


## [Figures](https://github.com/C-Crenshaw/Project1_DS4002/tree/31af060ae50a6ab3334985967c7f2793b55dfb41/FIGURES)

This section contains all of the figures generated by the project. A complete list and summary description of each figure is organized in the markdown table below. _Currently in progress but will be finished during MI4._ 

Note: This section is currently in progress. The regression equation has been run and our code is complete, but our project group still needs to graphically illustrate how our prediction equation compares with our original data. Additionally, if the regression analysis seems sufficient, we will use the equation to predict the New York Time's revenue for 2023 using the headline sentiment scores. 

| 	Figure Title	 | 	Description	 | 
| 	:-----:	 | 	:-----:	 |
| 	Scatterplot of all Sentiment Scores	| 	TBD	| 
| 	Regression Comparison between Training and Test Data	| 	TBD	|
| 	Predicting Revenue for 2023	| 	TBD	| 

## [License](https://github.com/C-Crenshaw/Project1_DS4002/blob/19a904af31fb17d7c6a334f728b05b2a784e7304/LICENSE)

This project is licensed under the MIT License. See the LICENSE.md file for details. 

## References
[Link-to-MI1-Doc](https://docs.google.com/document/d/1utsMtB2bDIQX3fz7LK_I9ves920YAxxiQElVlCmu9ys/edit?usp=sharing)

[Link-to-MI2-Doc](https://github.com/matiassingers/awesome-readme](https://docs.google.com/document/d/1RaTlaMyAfhypTplMEkluhVozaZ-1d3DvAZYYkxy1Y_M/edit?usp=sharing)https://docs.google.com/document/d/1RaTlaMyAfhypTplMEkluhVozaZ-1d3DvAZYYkxy1Y_M/edit?usp=sharing)

[1] 	A. Singh, “NYT Articles: 2.1M+ (2000-Present) Daily Updated,” kaggle.com, Sept., 2023. [Online]. Available: https://www.kaggle.com/datasets/aryansingh0909/nyt-articles-21m-2000-present. [Accessed Sept. 9, 2023]. <br />

[2] 	S. Panchumarthy, “DS4002/Project 1,” Sept., 2023. [Online]. Available: https://github.com/vsp2fcs/DS4002/tree/main/Project%201. [Accessed Sept. 17, 2023]. 

[3] 	“NLTK Sentiment Analysis Tutorial for Beginners,” datacamp.com, para. 1, Mar., 2023. [Online]. Available: https://www.datacamp.com/tutorial/text-analytics-beginners-nltk. [Accessed Sept. 15, 2023]. 

[4] 	“The New York Times Company (NYT),” stockanalysis.com, Sept., 2023. [Online]. Available: https://stockanalysis.com/stocks/nyt/revenue/. [Accessed Sept. 12, 2023]. 

[5] 	Z. Blumenfeld, “The Definitive Guide to Building a Predictive Model in Python, neo4j.com, Jun. 16, 2023. [Online]. Available: https://neo4j.com/blog/build-predictive-model-python/. [Accessed Sept. 17, 2023].  

## Acknowledgments
This README structure is adopted from [@DomPizzie](https://gist.github.com/DomPizzie) on Github. 

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

This section contains all the source code for the project. All code has been consolidated in one python/jupyter notebook file. The original source code can be found [online](https://colab.research.google.com/drive/1YPt-yQZR_OffM-WwLsKOsho0I5PVS_6A?usp=sharing). 

### Installing/Building Code

* The code can be downloaded from the SRC file in either python or jupyter notebook format. The finalized and most updated version of the code file can be found online on Google Colaboratory. The link to this file is shared above.
* The code file will generate all updated/cleaned dataframes outside of the original 'nyt-metadata.csv' and 'New York Times Revenue.xlsx' files that must be downloaded. Ensure that all file paths are updated in the downloaded file. 

### Usage

The usage section focuses on the most complex aspects of the final code, highlighting the most important aspects of the final code. For brevity, this section largely leaves out the aspects of the file that involve cleaning the data. 

* All necessary imports are loaded into the 'Source_Code.py' file. These necessary packages are repeated below. These imports include the Natural Language Toolkit (NLTK) which is utilized for our sentiment analysis and the SK Learn package which is used for the regression model.  
```
#Standard imports
import pandas as pd
import numpy as np
import random

#Natural Language Toolkit (NLTK)
import nltk
nltk.download('all')

#Other NLKT imports
from nltk.sentiment.vader import SentimentIntensityAnalyzer
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.stem import WordNetLemmatizer

#ML imports
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
from sklearn.preprocessing import MinMaxScaler
```

* The sentiment analysis is run through a text processing function. The function codes for lexical and syntactic features of the news headlines (such as positive or negative words) and assigns each article a compound sentiment score. This score indicates the strength of emotion within each article headline. We then create an additional variable which codes the directionality of the sentiment score. This numerical score will falls between -1 and 1, with zero indicating neutrality.
```
#Text preprocessing function
def textPreprocess(string):
  #Tokenizing text
  tokens = word_tokenize(string.lower())

  #Removing stop words
  filteredTokens = [token for token in tokens if token not in stopwords.words("english")]

  #Lemmatizing tokens
  lemmatizer = WordNetLemmatizer()
  lemmatizedTokens = [lemmatizer.lemmatize(token) for token in filteredTokens]

  #Joining tokens back together
  processedString = ' '.join(lemmatizedTokens)

  return processedString

#Initializing NLTK sentiment analyzer
analyzer = SentimentIntensityAnalyzer()

#Sentiment fetching function
def sentimentFetch(string):
  scores = analyzer.polarity_scores(string)
  sentiment = scores["compound"]
  return sentiment

#Process headlines and create new sentiment column
nyt["compound_sentiment"] = nyt["headlines"].apply(textPreprocess).apply(sentimentFetch)
#26s runtime

#Positive, neutral, or negative sentiment definer function
def compoundToDirection(score):
  if score >= 0.05:
    return 1
  elif score <= -0.05:
    return -1
  return 0
```

* Machine learning is used to build a regression model based on sentiment and yearly revenue. Machine learning functions as a result of splitting the final dataset into training and test data. 
```
#Divide into X & y
X = nyt_ml.drop(columns=["Adjusted 2022 Revenue in Billions USD"])
y = nyt_ml["Adjusted 2022 Revenue in Billions USD"]

#Find averages of MSE, RMSE, and beta coefficients for 1000 models - 6s runtime
mse_avg = 0
rmse_avg = 0
beta1_arr = []
beta2_arr = []
intercept_avg = 0
for i in range(1000):
  #Split data
  X_train, X_test, y_train, y_test = train_test_split(X,y,test_size=0.3)
  #Create and train model
  mult_reg = LinearRegression()
  mult_reg.fit(X_train,y_train)

  #Generate Predictions
  predicted = mult_reg.predict(X_test)
  actual = np.array(y_test)

  #MSE & RMSE
  mse = mean_squared_error(predicted,actual)
  mse_avg += mse
  rmse = mean_squared_error(predicted,actual,squared=False)
  rmse_avg += rmse

  #Beta Coefficients
  coef = mult_reg.coef_
  beta1_arr.append(coef[0])
  beta2_arr.append(coef[1])
  intercept = mult_reg.intercept_
  intercept_avg += intercept

mse_avg /= 1000
rmse_avg /= 1000
print("Average MSE is", round(mse_avg,5), "and Average RMSE is", round(rmse_avg,5))

#Average multiple linear regression equation
beta1_avg = np.mean(beta1_arr)
beta2_avg = np.mean(beta2_arr)
intercept_avg /= 1000

print("Adjusted 2022 Revenue in Billions USD =", round(beta1_avg,3),"*compound_sentiment +", round(beta2_avg,3),"*sentiment_direction +", round(intercept_avg,3))
```

* As a final note, the model is used to predict the adjusted revenue for the New York Times in 2023 based on the current sentiment scores of articles presently being published in 2023. 
```
#2023 sentiment (previously recorded in "ML Dataset Creation" section)
sentiment2023

predicted2023Revenue = beta1_avg * sentiment2023["compound_sentiment"][2023] + beta2_avg * sentiment2023["sentiment_direction"][2023] + intercept_avg
print("The predicted adjusted 2022 revenue for 2023 is $", '{:,}'.format(round(predicted2023Revenue*1000000000,2)), u"\u00B1 $", '{:,}'.format(round(rmse_avg*1000000000,2)))
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
| 	compound_sentiment	| 	Aggregate degree of headline sentiment (Absolute Value)	| 	Integer	 | 
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
| 	compound_sentiment	| 	Aggregate degree/strength of headline sentiment	| 	Integer	 | 
| 	sentiment_direction	| 	Directionality of headline sentiment	| 	Integer	 | 
| 	Adjusted 2022 Revenue in Billions USD	| 	NYT revenue adjusted for inflation	| 	Integer	 | 


## [Figures](https://github.com/C-Crenshaw/Project1_DS4002/tree/31af060ae50a6ab3334985967c7f2793b55dfb41/FIGURES)

This section contains all of the figures generated by the project. A complete list and summary description of each figure is organized in the markdown table below.

| 	Figure Title	 | 	Description	 | 
| 	:-----:	 | 	:-----:	 |
| 	[Scatterplot of Revenue and Compound Scores](https://github.com/C-Crenshaw/Project1_DS4002/blob/802342d33f7bc2e72eb56e8420bbb7c720e30b95/FIGURES/Revenue_Compound_Sentiment.jpg)	| 	Raw comparison between NYT yearly revenue and average compound scores. 	| 
| 	[Scatterplot of Revenue and Sentiment Direction](https://github.com/C-Crenshaw/Project1_DS4002/blob/3d267a9034bdf2ba91683ffd7f5c38f0b637e1b1/FIGURES/Revenue_Sentiment_Direction.png)	| 	Raw comparison between NYT yearly revenue and average directionality of sentiment.	| 
| 	[Final Presentation](https://myuva-my.sharepoint.com/:p:/g/personal/zta9cq_virginia_edu/ESr049fEGHVGkGc37L0qty8BgAHzCUCwH2mPdILFwx6wLg?e=zXLJI1)	| 	Link to the final Project 1 powerpoint presentation.	|

## [License](https://github.com/C-Crenshaw/Project1_DS4002/blob/19a904af31fb17d7c6a334f728b05b2a784e7304/LICENSE)

This project is licensed under the MIT License. See the LICENSE.md file for details. 

## References
[Link-to-MI1-Doc](https://docs.google.com/document/d/1utsMtB2bDIQX3fz7LK_I9ves920YAxxiQElVlCmu9ys/edit?usp=sharing)

[Link-to-MI2-Doc](https://github.com/matiassingers/awesome-readme](https://docs.google.com/document/d/1RaTlaMyAfhypTplMEkluhVozaZ-1d3DvAZYYkxy1Y_M/edit?usp=sharing)https://docs.google.com/document/d/1RaTlaMyAfhypTplMEkluhVozaZ-1d3DvAZYYkxy1Y_M/edit?usp=sharing)

[1] 	A. Singh, “NYT Articles: 2.1M+ (2000-Present) Daily Updated,” kaggle.com, Sept., 2023. [Online]. Available: https://www.kaggle.com/datasets/aryansingh0909/nyt-articles-21m-2000-present. [Accessed Sept. 9, 2023]. <br />

[2] 	“NLTK Sentiment Analysis Tutorial for Beginners,” datacamp.com, para. 1, Mar., 2023. [Online]. Available: https://www.datacamp.com/tutorial/text-analytics-beginners-nltk. [Accessed Sept. 15, 2023]. 

[3] 	“The New York Times Company (NYT),” stockanalysis.com, Sept., 2023. [Online]. Available: https://stockanalysis.com/stocks/nyt/revenue/. [Accessed Sept. 12, 2023]. 

[4] 	Z. Blumenfeld, “The Definitive Guide to Building a Predictive Model in Python, neo4j.com, Jun. 16, 2023. [Online]. Available: https://neo4j.com/blog/build-predictive-model-python/. [Accessed Sept. 17, 2023].  

## Acknowledgments
This README structure is adopted from [@DomPizzie](https://gist.github.com/DomPizzie) on Github. 

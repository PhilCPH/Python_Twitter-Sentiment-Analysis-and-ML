# Social Data Science Semester 2020 - Group Assignment
## From informal to formal: Using Twitter Sentiments to predict stock prices
### Philipp, Merle, Kevin
In this project we have chosen to predict stock development based on a Twitter sentiment analysis which is sourced by COVID-19 related keywords and relate it to COVID-19 infections and deaths over the same timeframe.

Our problem statement was:
> Can COVID-19 related sentiments, measured via Twitter, in combination with actual case and death counts predict current and future stock market development in the US?

In this project we’re working with three datasets: 
- COVID-19 cases in the US (https://github.com/nytimes/covid-19-data)
- a Twitter dataset based on COVID-19 related keywords (https://zenodo.org/record/3723940#.X6xa7y2X-34)
- S&P 500 data (Yahoo Finance)

Our plan of attack looked as follows:

![PlanofAttack image](https://github.com/PhilCPH/Twitter-Sentiment-Analysis-and-ML/blob/main/images/PlanofAttack.JPG)

### Data preprocessing
We have decided to analyse Q1 of 2020 to visualize the first economic drop due to COVID-19.

The COVID-19 dataset was the easiest to handle, as its only date, cases and deaths. The date-category was transferred to a datetime format.

The Tweet dataset needed to be hydrated first as in this state it only contains tweet-ids due to Twitter's TOS. Another challenge in this dataset was the size. During the middle of March COVID-19 became a bigger part of the public discussion and thus, the frequency of related tweets increased. In addition to that, the scraper of this dataset switched from 2 to 13 keywords, increasing the size even more. For our sample, the increase can be seen in this graph.

![Nrtweets image](https://github.com/PhilCPH/Twitter-Sentiment-Analysis-and-ML/blob/main/images/tweet_increase.JPG)

After hydrating, tweets that have been since deleted will result in NaN's. After dropping all missed values, duplicates and tweets not written in English, 43,111 tweets remain.

We've used "Tweet Preprocessor" to remove Twitter-specific parts (e.g. hashtags, mentions) and "Spacy" to identify stop words and punctuation. Spacy then transforms the words into tokens and leematizes them (e.g. running converts to run). Then words with less than 4 characters are removed. By loading NLTK-dictionaries, each tweet can be related to a polarity, depending on how many positive/neutral/negative words were mentioned. This polarity ranges from -1 to +1. The mean polarity over our time frame can be seen in the next figure.

![Polarity image](https://github.com/PhilCPH/Twitter-Sentiment-Analysis-and-ML/blob/main/images/MeanPol.JPG)

The financial data uses S&P 500 data for a proxy to the US market. The financial data is interpolated in order to relate Twitter-data to non-trading days as well.

The financial dataset comprises two parts: individidual stock returns and clustering based on fundamental data for all companies that are included in the index.

### Results

After pre-processing the tweets, we can plot the polarity of the tweets against the subjectivity.

![Polsub image](https://github.com/PhilCPH/Twitter-Sentiment-Analysis-and-ML/blob/main/images/Polarity.JPG)

This results in a funnel-shape, meaning that the more negative/positive a tweet is, the more subjective it becomes (excluding a few outliers).

### PCA/UMAP

Measures to try and cluster the S&P 500 data with PCA/UMAP by industries or return (positive/negative) did not provide satisfying results.

![Cluster1 image](https://github.com/PhilCPH/Twitter-Sentiment-Analysis-and-ML/blob/main/images/PCA.JPG)
![Cluster2 image](https://github.com/PhilCPH/Twitter-Sentiment-Analysis-and-ML/blob/main/images/UMAP.JPG)

### LDA

This tool helps us to get a grip of what the data set looks like. Not surprisingly, it showed that "coronavirus" is the most frequent term.

![LDA image](https://github.com/PhilCPH/Twitter-Sentiment-Analysis-and-ML/blob/main/images/LDA.JPG)

### WordClouds
The WordClouds have been used to visualize the most frequent words for each sentiment. As a bonus, we used a mask as an overlay.

Positive Sentiment

![Positive image](https://github.com/PhilCPH/Twitter-Sentiment-Analysis-and-ML/blob/main/images/Positive.png)

Neutral Sentiment

![Neutral image](https://github.com/PhilCPH/Twitter-Sentiment-Analysis-and-ML/blob/main/images/Neutral.png)

Negative Sentiment

![Negative image](https://github.com/PhilCPH/Twitter-Sentiment-Analysis-and-ML/blob/main/images/Negative.png)

The positive segment seems to be more related to instill hope and cooperation in the community by mentioning “good”, “right”, “time” and “help”. The neutral keywords have a higher representation of COVID-19 related keywords but also mention the political situation with “trump”, “china”, “pandemic” and “outbreak”. Negative keywords, such as curse words in combination with “crisis”, “china”, “kill” and “trump” seem to be more of an outlet of frustration in this time of crisis.

### Stock prediction

The supervised learning models are used to predict

1. the actual stock price return each day, i.e. the daily percentage change of the S&P 500 adjusted closing price, and
2. the direction of the index’s movements, i.e. if its adjusted closing price is going up or down that day.

This is done for

a. the index price of the same date as the tweets and COVID-19 cases as well as for
b. the index price one day later.

With that, a correlative as well as a predictive relationship are examined. In order to do that, the data sets are merged on the date with the US COVID-19 cases and deaths, the mean sentiments, and the index price and percentage change.

The exploration of the financials showed that companies in the industry of Gold, Biotech, Wireless Telecommunication Services, and Systems Software made gains during this timeframe. On the other hand, industries such as Airlines and Hotels and Resorts were hit the worst.

![Industries image](https://github.com/PhilCPH/Twitter-Sentiment-Analysis-and-ML/blob/main/images/industries.JPG)

### Performance

We used cross validation for the prediction. As the tweets should influence the stock development of tomorrow, we included a prediction with a time lag of one day. The mean scores as well as the standard deviation for 10-fold Repeated Stratified KFold cross-validation are reported in the figure below.

![Classresult image](https://github.com/PhilCPH/Twitter-Sentiment-Analysis-and-ML/blob/main/images/classres.JPG)

The model's accuracy is pictured below.

![Accresult image](https://github.com/PhilCPH/Twitter-Sentiment-Analysis-and-ML/blob/main/images/accres.JPG)

### Conclusion

Unfortunately, the used specification of sentiments were not enough to conclude in a precise prediction. The COVID-19 related Twitter-database only covers a one-dimensional perspective. Stock development transcends this perspective and seems to be influenced by a greater number of parameters.

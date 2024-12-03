# Tail Me - Team 46 ML Final Report

## Introduction / Background

Unlike professional sports, college football is a collection of nearly 150 teams - many of whom don't receive national attention. For many of these programs, local media outlets dominate their news coverage. In the context of sports betting, lines and odds are set by outsiders on a national scale. Thus, much of the underlying information about a team may be captured by local media and kept a "secret" to these oddsmakers. Additionally, key team statistics—such as returning players, offensive and defensive metrics, and overall roster continuity—often go underexplored at the national level. We aim to explore how capturing local news sentiments, combined with these underexplored team statistics, can provide context that national coverage misses and lead to more accurate predictions of a college football team's season. While others have explored social media and individual-level data to predict outcomes, the concept of analyzing local news coverage is still relatively untapped in this space ([IEEE Explore](https://ieeexplore.ieee.org/document/8530517) and [MDPI](https://www.mdpi.com/2624-831X/1/2/14)).

For our first model, we took a fresh approach to predict college football team performance by combining local sentiment with national betting odds. For every FBS team, we scraped local news articles to get a sense of how writers felt about their team's chances before the 2023 season. Using advanced tools, we analyzed the tone of these articles—whether positive, negative, or neutral—and turned that sentiment into a numerical score. We then used this score to create a simple prediction: whether bettors should take the over or under on a team's projected wins for the season. To check how accurate our model was, we compared it against national win projections from [Sports Odds History](https://www.sportsoddshistory.com/cfb-win/?y=2023&sa=cfb&t=win&o=t). By mixing local coverage with national odds, we developed a new way to approach sports betting predictions.


For our next model, we decided to try something a little different by using a clustering technique to find groups of teams that had the best chances of winning based on similar stats. We started by collecting all the team statistics from the 2022 season using a detailed sports database [College Football Team Data](https://www.sports-reference.com/cfb/years/2022-team-offense.html), along with data on how many starters each team was bringing back for the 2023 season. We knew that teams with more returning starters often perform better, so we turned this into a percentile score to highlight the strength of each team’s roster. Then, we combined all of this information and used a method called KMeans clustering, which groups teams with similar profiles together. This allowed us to categorize teams into clusters based on their overall strengths and weaknesses. By analyzing these clusters, we were able to spot patterns that helped predict which teams were more likely to outperform or underperform their expected win totals for the season. This approach gave us a fresh perspective on how different team types might fare, offering sports bettors valuable insights into team success.


Finally, to take our predictions to the next level, we combined both models into one powerful approach. By using a Random Forest classifier, we connected the insights from sentiment analysis with the team clusters, allowing us to predict whether a team would exceed or fall short of their expected win total. Random Forest works by creating many "decision trees" that each make their own prediction, and then combining the results to give a final, more accurate prediction. Think of it like having a group of experts weigh in on the same question, with the final decision being the one most experts agree on. We plotted key data points together to predict a team’s win differential for the season. This merger of models gave us a much clearer picture of team performance, turning our predictions into actionable insights. In the end, these three models provided a strong foundation for accurately predicting which teams will outperform expectations, helping bettors make smarter, data-backed decisions for the season ahead.

## Problem Definition
### Problem
Predicting college football team performance can be challenging, as national odds often overlook both the local sentiment surrounding a team and key team statistics. We aim to explore whether combining insights from local news articles—capturing the emotions and opinions of writers close to the team—with overlooked team metrics like returning players and offensive/defensive statistics that can provide valuable predictions for the upcoming season.

### Motivation
Local news and underexplored team statistics often contain critical information that is underutilized by national oddsmakers, especially for smaller programs. By analyzing both the narratives in local reporting and metrics like roster continuity and past performance, we aim to uncover insights that can give sports bettors a significant edge in predicting team success.

## Methods

### Data Preprocessing Methods
- We start by gathering the dataset, done by webscraping local news articles in a specified format for all division one teams, aiming for around 10 articles per team. We extract the title and article content of each of these links and export it to a csv.
- Next, we **filter out bad articles**
  *   First we **embed all of the titles** of the articles using a hugging face sentence embedding model. We then compare it against a set query and use cosine simililarty of the two embeddings in order to determine whether to drop the article or not. It is worth noting we artifically bump up some of the titles that contain certain keywords (i.e preview, predictions) in order to not incorrectly drop some of the articles.
  *   We also drop all articles that could not be parsed correctly
 - We then webscrape the odds database for all the teams, concatenating it with the dataframe of the articles to create a singular dataframe

### Models
There are two main models we implemented so far.
1. Sentiment Analysis
- We take all of the valid articles and their contents and score them from -1 to 1 on sentiment using a hugging face sentiment analysis model. We do this for each article for each team, followed by averaging them out for each team. The goal of this is to capture the overall sentiment of local news for the given team. Sentiment analysis is a supervised NLP technique that (as the name implies) captures the sentiment of the given text. No preprocessing of the text was required as it is a transformer based model that takes capitalization, punctuation, stopwords, etc. into account; as long as the article content was correctly scraped we passed it in as is. 
2. Linear regression
- We then apply linear regression, using the sentiment scores for each team as the independent variable and the win differential (calculated as actual wins minus predicted wins) as the dependent variable. We do this method with a continuous outcome rather than simply predicting over and under in order to get a more accurate model. Using over/under as the outcome (binary, would have used logisitic regression) would have ignored many of the naunces associated with each data point. Thus, we opted to use a continuous variable as the outcome and then afterwards convert this to a binary over/under prediction for comparison in order to be more precise.


## Results and Discussion

**Scores:**
- **Mean Squared Error:** 3.3171764072001624
- **R-squared:** -0.02545207213662004
- **Accuracy:** 0.5135135135135135

![Linear Regression Basic](linear_regression_basic.png)
- This is a basic plot of our linear regression model (sentiment of articles versus actual-predicted win loss difference)
- Trendline is relatively steep, and the datapoints are at a similar level. Correlation is very subtle.
- However, a steep trendline doesn't necessarily mean a good fit, indicated by the negative R-squared and relatively high MSE.

![Linear Regression Residual](linear_regression_residual.png)
- This is a residual plot for the linear regression model.
- There seems to be little to no clear pattern in the graph except for outliers towards the high end of the win total. Overall, this is a good sign there is no clear section of bias in our model and is without being influenced by any specific bias.

![Accuracy Confusion Matrix](accuracy_confusion_matrix.png)
- The model is ever so slightly predicting over more than under, but overall pretty even. There is little trend here as there should be. This is good to know as we likely do not have to adjust the threshold to predict over or under (derived from predicted wins).

### Conclusions
The model is doing slightly better than guessing (51.3% over 50%) which is to be expected given that sentiment analysis of articles is likely to be no more than a small trend. This, coupled with the use of a basic model such as linear regression, was bound to only give a marginal increase over guessing, especially as sports betting is unfavorable to begin with. However, coupling this with other paramters could yield a much stronger model which we will explore in the future.

## Link to gantt chart
https://docs.google.com/spreadsheets/d/1bbKUSDxJFoK4ky9VcbNaSTmDr1u1k4chZWNa7AIZ_jU/edit?gid=1538336270#gid=1538336270



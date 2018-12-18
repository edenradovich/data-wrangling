## Table of Contents
- [Introduction](#intro)
- [Data Wrangling Summary](#wrangle)
- [Data Analysis Summary](#analysis)

<a id='intro'></a>
# Introduction

This project was focused primarily on data wrangling. I have gathered, assessed and cleaned data from the WeRateDogs twitter account, @dog_rates. Starting with basic data provided by Twitter directly, I have also wrangled additional data from Twitter’s API to perform an interesting and entertaining analysis on dog ratings.

<a id='wrangle'></a>
# Data Wrangling Summary

In this project, I have wrangled data from the `WeRateDogs` twitter account, @dog_rates. Starting with basic data provided by Twitter directly such as tweet ID, timestamp, text, etc., I have collected additional data from Twitter’s API, which shows retweet count and favorite count as well. 

After acquiring all the data, I identified the below issues, separated by Tidiness and Quality:
*Tidiness Issues:*
1.	The original twitter data should be combined with the newly acquired API data; once combined, there will be duplicated columns which need to be removed (such as text) 
>Solution: using pd.merge to combine the two dataframes
2.	There are four columns for dog stage (i.e. dogger, floofer, pupper, puppo), which should be combined into one column, as these are values.
>Solution: using .melt to combine into one column and cleaning from there

*Quality Issues:*
1.	The "id" columns in all the tables have different names (tweet_id vs id)
>Solution: using .rename to change column names
2.	Missing dog stage information should be null not "None"
>Solution: using .replace to np.nan
3.	We are only interested in original tweets, not retweets  
>Solution: using .notnull on “retweeted_status_id” to see which tweets were retweets then using .drop to remove them
4.	Incorrect data types: tweet_id should be an object, not integer
>Solution: using .astype(object) to correct data type
5.	There are columns with null data 
>Solution: there are dog names and dog stages with null data, however this will not affect my analysis
6.	There are dog names that are not real names (i.e. incorrectly tagged)
>Solution: dog names will not be a part of my analysis so I did not focus on correcting inccorect dog names
7.	The max rating numerator is 1776; the min rating numerator is 0
>Solution: using .query to locate the large and small values to assess whether accurate or not
8.	The max rating denominator is 170; the min rating denominator is 0
>Solution: using .query to locate the large and small values to assess whether accurate or not

After identifying and defining the above issues, I addressed them line by line (with some getting fixed through fixing another issue). 

<a id='analysis'></a>
# Data Analysis Summary

After wrangling and cleaning the data, I asked the following questions:

1. Do higher ratings yield higher retweet and favorite count? Is retweet count correlated with favorite count?
2. Which dog stage receives the highest rating?
3. which dog breed receives the highest rating?

**Question 1:** 

To answer Question 1, I assessed correlations with `rating_numerator` and the other variables to see any relationships. Surprisingly, there were not strong correlations with ratings and retweet or favorite count. However, retweet count and favorite count were very highly correlated with a coefficient of 0.93. This tells us that retweets and favorites might be a higher indicator of popularity than rating.

**Question 2:** 

dog_stage refers to the WeRateDogs dog stage defintion: 
- doggo: bigger, possibly older dog
- pupper: smaller, possibly younger dog
- puppo: transitional phase between pupper and doggo
- floofer: particular dogs with more hair

With these determinations in mind, I looked to see how many dogs had a `dog_stage` classification and how they were dispersed. Here was the breakdown:
- doggo:       75
- floofer:     10
- pupper:     234
- puppo:       25

Then I calculated the mean rating of each dog_stage. The results came out to be:
- doggo:      11.853333
- floofer:    11.800000
- pupper:     10.820513
- puppo:      12.080000

*It appears that "puppo" had the highest average rating, although if you look above at how many dogs were in the puppo sample, 25 is not very high. Therefore, it is difficult to make any sort of concrete conclusion. Pupper scored the lowest and had the highest sample of 234.*

**Question 3:**

To assess which dog breed received the highest rating, I chose the top ten dog breeds by the number of times they were tweeted. Here were the top ten dog breeds:

- golden_retriever:      138
- labrador_retriever:     95
- pembroke:               88
- chihuahua:              79
- pug:                    54
- chow:                   41
- samoyed:                40
- toy_poodle:             38
- pomeranian:             38
- malamute:               29

Here were the results of the average ratings each of the above dog breeds received:

- pug:                   10.240741
- chihuahua:             10.708861
- malamute:              10.896552
- toy_poodle:            11.105263
- pembroke:              11.443182
- chow:                  11.609756
- samoyed:               11.700000
- pomeranian:            12.868421
- golden_retriever:      13.130435
- labrador_retriever:    13.905263

*Golden Retriever appears to be the most tweeted dog, while Labrador Retriever received the highest ratings.*

Here is a visualization representing the above numbers in graphical form. The bars are ordered by number of tweets.

![Top 10 Breeds](/Top10Breeds.png)

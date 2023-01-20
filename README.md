# Project 3: Between `r/cars` and `r/electricvehicles` - Rivian Automotive's Classification Problem

## Problem Statement

[`Rivian`](https://rivian.com/), a leading electric vehicle manufacturer in the United States, has hired me to perform a sentiment analysis of two `subreddits` - `r/cars` and `r/electricvehicles` - which it actively monitors on the popular Reddit platform. The problem is that the data previously collected before I was hired were not properly labelled, and so we do not know which of those unlabelled posts came from either subreddit. We do not wish to discard the unlabelled data because they are a substantial number, and due to some issues with [Pushshift's](https://github.com/pushshift/api) API, it was no longer possible to get data earlier than November. My boss has therefore asked me to do the following:

1. Collect more data from the two subreddits, using [Pushshift's](https://github.com/pushshift/api) API.
2. Use Natural Language Processing to train a classifier on which subreddit a given post came from. This is a binary classification problem and I attempt several models and ensembles.
3. In future, I can then use my best model to classify the unlabelled posts we have as belonging to either the `r/cars` or `r/electricvehicles` subreddits, and then carry out a sentiment analysis. (This third point is not covered in this project). 
4. Make a presentation to him and the rest of the team outlining my process and findings.

## Metrics for Evaluation
Before I begin, my boss informs me that my model would only be used if it satisfies one or both of the following conditions:

1. **Has an accuracy of at least 0.90**. Accuracy is a key metric for this project. According to my boss, we ideally do not want the model to misclassify more than 1 in 10 posts otherwise we would not be confident of the model's usefulness.
2. **Has an F1 score of at least 0.90**. The F1 score is important for two reasons. First, we are as interested in correctly predicting posts from the `r/cars` subreddit as we are in correctly predicting posts from the `r/electricvehicles` subreddit. Neither is more important to us. Second, it turned out that our two classes were slightly unbalanced with the `r/cars` subreddit having the majority - 58% - of observations. As a result of this imbalance, accuracy is not a perfect metric. For these reasons, a strong F1 score is useful.

## General Information on the two subreddits
|**Name of Subreddit**|**Description**|**Created**|**Membership as at Jan 07, 2023**|
|:-|:-|:-|:-|
|[`r/cars`](https://www.reddit.com/r/cars/)|`r/Cars` is the largest automotive enthusiast community on the Internet. We are Reddit's central hub for vehicle-related discussion including industry news, reviews, projects, videos, DIY guides, stories, and more.|Mar 20, 2008|4.3m|
|[`r/electricvehicles`](https://www.reddit.com/r/electricvehicles/)|The future of sustainable transportation is here! This is the Reddit community for EV owners and enthusiasts. Discuss evolving technology, new entrants, charging infrastructure, government policy, and the ins and outs of EV ownership right here.|Apr 20, 2009|218k|

## Dataset used for analysis 

My target was to use the Pushshift API to gather 5,000 posts from each of the subreddits, making a total of 10,000 posts. I reckoned that since the two subreddits are pretty similar in theory as they both deal with cars, my models would need substantial data in order to be able to differentiate between posts from either.

However, there were data collection challenges. Following some recent updates with the Pushshift API, its data only went as far back as November 2022. This was not a problem for the `r/cars` subreddit where I was able to pull 5000 posts. But for the `r/electricvehicles` subreddit - which has almost 20 times fewer members than its `r/cars` counterpart, there wasn't up to 5000 posts dating back to November. Hence, I was only able to collect 3660 posts from there. Combined, this brought my uncleaned dataset to 8660 rows. In the data folder for this project, I have the following files:

* `cars_unclean.csv` - which contains the 5000 rows collected from the cars subreddit. 
* `ev_unclean.csv` - which contains the 3660 rows collected from the electricvehicles subreddit.  
* `ev_cars_unclean.csv` - which contains the 8660 combined rows from the two aforementioned docs. 
* `ev_cars_clean.csv` - which contains the 7447 rows that remained after cleaning the dataset. This is the file that was used for my EDA, analysis and modeling. 

## Data Dictionary

|S/No|Feature Name|Type|Description|
|---|---|---|---|
|1|`subreddit`|object|One of two classes - `cars` or `electricvehicles` - into which all observations in the dataset fall|
|2|`title`|object|The title of the post| 
|3|`id`|object|ID of each unique post|
|4|`created_utc`|object|[Unix/Epoch time](https://en.wikipedia.org/wiki/Unix_time), useful in the data collection stage as it is used to note the posting time of the last post pulled from the API, and then subsquently used as a benchmark when pulling more data.|  

## Summary
In the three notebooks in this repo, I go through the data collection process, data cleaning, exploratory data analysis, data preprocessing, and modeling. 
- As stated earlier, my data collection process was through the Pushshift API. 8660 observations were collected in total from the two subreddits.
- In my cleaning process, I removed a single row without a title, removed some inexplicable classes which showed up in the dataset, removed non-english alphabets, dropped duplicate rows, among other steps. At the end of my cleaning process, I was left with 7447 rows in total. My `r/cars` subreddit was the majority class, and its proportion - 58% - became my baseline accuracy. The baseline represents something of a naive accuracy, where if a person simply predicts the majority class every single time, such a person would be correct up to the baseline which in this case is 58%. 
- I explored the data, comparing the distributions of the character lengths of the two subreddits as well as the word counts of the two subreddits; I considered some summary statistics which basically buttressed my observations from the visuals I had created.
- I also explored the data after vectorizing it. I defined custom functions for stemming and preprocessing. I dropped stopwords - which are words that are so commonly used that they do not have much use for my modeling. I looked at the top words in the combined datasets, the top words in each of the separate classes, and the words that overlapped at the top of both classes. I did the same for bigrams. 
- I also briefly did EDA for my employer, Rivian, looking at how often it ppeared in the different classes, and compared the frequency of its mentions with those of its main competitors in the EV market. 
- Before I went into the meat of the modeling, I made sure to train-test split my dataset. Then I made several attempts to build a competent classifier for the problem before me, bearing in mind the evaluation metrics. 

Below are the models I build, alongside their performances for the metrics chosen for this task. 
|Model Number|Model Type|Test Accuracy Achieved|Misclassification Rate|F1 Score|
|---|---|---|---|---|
|1|Logistic Regression|86.0%|14.0%|0.88|
|2|Random Forest|80.2%|19.8%|0.85|
|3|Multinomial Naive Bayes|85.3%|14.7%|0.87|
|4|Bagged Classifier with LR base estimator|86.8%|13.2%|0.89|
|5|Bagged Classifier with RF base estimator|86.5%|13.5%|0.89|
|6|Gradient Boosting|86.4%|13.6%|0.89|
|7|Voting Classifier|87.1%|12.9%|0.89|
|8|Stacked Classifier|87.4%|12.6%|0.89|
|9|Stacked Classifier with character length feature|87.9%|12.1%|0.90|

## Conclusions & Recommendations

Recall that my assignment was to:
1. Collect more data from the two subreddits, using [Pushshift's](https://github.com/pushshift/api) API.
**Outcome: I collected enough data from the two subreddits. The number was high enough to give me confidence about my models' abilities to solve the task.** 

2. Use Natural Language Processing to train a classifier on which subreddit a given post came from. 
**Outcome: I trained several high performing predictive models with accuracies far above the baseline. I was informed that before the model can be put into production, it had to either achieve 90% accuracy or an F1 score of 90%. While none of my models managed to reach 90% accuracy, one of my models achieved an F1 score of 90%. That model was a stacked classifier comprising of LogisticRegression, Multinomial Naive Bayes, RandomForestClassifier, and Bagging Classifier as the first level estimators; and a Logisticregression model as the final estimator.** 

3. Make a presentation outlining my process and findings.
**A powerpoint presentation is included in this project and would be delivered as requested.** 

**My recommendations are as follows:**
- I have submitted my findings as requested, but if there is more time I would love to incorporate the texts of the posts as well. I only used the titles in this instance, but I believed that adding the texts could further help my models in differentiating between the classes. 
- I recommend that my Stacked Classifier which got the highest F1 score be put into production as it meets the 0.90 F1 score criteria. Further its other metrics, including an accuracy of 88% and a misclassification rate of 12 out of 100 is solid enough to proceed with. 


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

* [`cars_unclean.csv`]('./datasets/cars_unclean.csv') - which contains the 5000 rows collected from the cars subreddit. 
* [`ev_unclean.csv`]('./datasets/ev_unclean.csv') - which contains the 3660 rows collected from the electricvehicles subreddit.  
* [`ev_cars_unclean.csv`](''./datasets/ev_cars_unclean.csv) - which contains the 8660 combined rows from the two aforementioned docs. 
* [`ev_cars_clean.csv`](./datasets/ev_cars_clean.csv) - which contains the 7447 rows that remained after cleaning the dataset. This is the file that was used for my EDA, analysis and modeling. 

## Data Dictionary

|S/No|Feature Name|Description|
|---|---|---|
|1|`subreddit`|One of two classes - `cars` or `electricvehicles` - into which all observations in the dataset fall|
|2|`title`|The title of the post| 
|3|`id`|ID of each unique post|
|4|`created_utc`|[Unix/Epoch time](https://en.wikipedia.org/wiki/Unix_time), useful in the data collection stage as it is used to note the posting time of the last post pulled from the API, and then subsquently used as a benchmark when pulling more data.|  


**Make a presentation of my findings at the next meeting of their association**
A powerpoint presentation is included in this project and would be delivered as requested. 


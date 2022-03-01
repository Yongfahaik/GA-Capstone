# Winner Winner Chickie Dinner?
## Winner Prediction for Fortnite

### Contents
- [Overview](#overview)
- [Problem Statement](#probstate)
- [Executive Summary](#executivesummary)
- [Data Dictionary](#datadictionary)
- [Model Evaluation](#evaluation)
- [Future Steps](#future)
- [Conclusion](#conclusion)

***

<div id="overview"></div>

## Overview

Electronic sports (or e-sports) is a form of competition using video games, and takes the format of organized multiplayer video game competitions held between professional players, either as an individual or as a team. The video game genres normally associated with e-sports are Multiplayer Online Battle Arena (MOBA), First-Person Shooter (FPS), Battle Royale, Real-Time Strategy (RTS), Fighting, Collectible Card Games (CCG) and Sports & Racing.

Based on the reports by Newzoo (games and esports analytics group), the global revenue for esports for year 2020 reached 1 billion dollars and the global audience reached 495 million. ([Source](https://venturebeat.com/2020/02/25/newzoo-global-esports-will-top-1-billion-in-2020-with-china-as-the-top-market/)). Looking at the historical data from Esports Earnings, considering how the prize money for tournaments is over 200 million dollars for the years 2019 and 2021, and 125 million dollars in 2020 despite the COVID-19 pandemic, it comes as no surprise that there is an increasing focus on sports in the recent years. ([Source](https://www.esportsearnings.com/history)).

In the case of Singapore, as the host of first Asian edition of Gamescom (The world's largest video game industry event) as well as the host for the Global Esport Games in the year 2021 last year, it is without doubt that the e-sports sector will become increasingly more important, with Singapore being the epicenter for the South-East Asia region. ([Source](https://www.straitstimes.com/business/companies-markets/singapore-sets-sights-on-becoming-world-force-in-e-sports)).

<div id="probstate"></div>

## Problem Statement

With how popular the e-sports sector is, it comes without a doubt that the most popular form of entertainment is going to be imminent, the act of guessing the winner. As such, this project will be tackling the subject of winner prediction.

Looking at the various genres of video games, team sports such as MOBA, FPS and RTS tends to be more popular and easier to guess the winner since the matches are between two teams. As such, this project will be undertaking the task of predicting the winner of a match of a Battle Royale game. In this case, the focus will be on Fortnite, the current third highest prize-awarding game. ([Source](https://www.esportsearnings.com/games)).

Fortnite by itself as a battle royale has various match modes that cater to the preferences of the player that are divided into the number of people in same group fighting for the winner spot. In this case, they are 'Solo' for 1v1 matches, 'Duos' for teams of 2, 'Trios' for teams of 3 and 'Squads' for teams of 4.

To simplify this project, the main task of this project will be the development of a classification-based model to handle the prediction of the winner of a solo match in Fortnite. Since the act of winner prediction is usually before the beginning of the match, this task will be based off the historical player statistics.

<div id="executivesummary"></div>

## Executive Summary

Firstly, web scraping was conducted using the Requests library, Selenium Webdriver and Fortnite Tracker API to gather the data from the website [FortniteTracker](https://fortnitetracker.com/) to obtain two sets of data, match statistics and player statistics. (Saved as matches_stats_solo.csv and players_stats.csv)

Secondly, data cleaning was performed for the two datasets (i.e. matches_stats_solo and players_stats). For the match statistics, this included removing the various columns that are not useful as well as rows with faulty data. For the player statistics, this meant removing columns with too many missing values, removing columns with only zero values and correcting columns to have the correct data types. Feature Engineering is also done for the `Lifetime` section to consolidate `Top` statistics as well as matching derivative columns with the match modes sections such as the various `average` statistics.

Thirdly, EDA was performed to check for possible relationships between player statistics, match statistics and the target class of winner.

For the modelling stage, five model types (Logistic Regression, K Neighbors Classifier, Random Forest Classifier, Light GBM Classifier and Keras Neural Network Classifier) were considered in total. The intention was to ensure an encompassing production modelling process through the evaluations of model types with various mechanisms in ascertaining the optimal model. Multiple iterations of tuning predictor features and grid search were carried out to ensure that the best hyperparameters were selected. 

<div id="datadictionary"></div>

## Data Dictionary

The data dictionary for all features is as follows (Note that only player statistics are used in the production model):
(Note that only the datasets used for the Part 2 and later Parts for the Project are available here. If you require the datasets in the webscraping process, kindly find them in the [google drive here](https://drive.google.com/drive/folders/1iz_ih0jQcpEd59D7Bk9OER5MBZF84suU?usp=sharing).)

|Feature|Type|Origin|Description|
|---|---|---|---|
|**eliminations**|*float*|matches_stats_solo.csv|The number of eliminations or kills made by the player during the match|
|**points**|*float*|matches_stats_solo.csv|A score statistics calculated by the game based on the number of eliminations and placement of the player for the match|
|**sessionid**|*object*|matches_stats_solo.csv|The unique id value used by Fortnite Tracker to identify the match| 
|**timealive**|*float*|matches_stats_solo.csv|The period of time alive the player is alive during the match counted in seconds. This is based off the time the player enters the match map and the time the player dies or the time the match ends if the player wins|
|**name**|*object*|matches_stats_solo.csv|The unique id value used by the player to designate as his name|
|**[prefix]_avgtimeplayed**|*float*|players_stats.csv|The average time the player played during the match for the match mode in seconds|
|**[prefix]_kd**|*float*|players_stats.csv|The kills to death ratio of the player for the match mode|
|**[prefix]_kills**|*float*|players_stats.csv|The cumulative number of kills the player has for the match mode|
|**[prefix]_kpg**|*float*|players_stats.csv|The average number of kills per match the player has for the match mode|
|**[prefix]_kpm**|*float*|players_stats.csv|The average number of kills per minute the player has for the match mode|
|**[prefix]_matches**|*float*|players_stats.csv|The cumulative number of matches the player has played for the match mode|
|**[prefix]_minutesplayed**|*float*|players_stats.csv|The cumulative time the player has played for the match mode in minutes|
|**[prefix]_score**|*float*|players_stats.csv|The cumulative score the player has for the match mode based off the points earned from each match|
|**[prefix]_scorepermatch**|*float*|players_stats.csv|The average score per match the player has earned for the match mode|
|**[prefix]_scorepermin**|*float*|players_stats.csv|The average score per minute the player has earned for the match mode|
|**[prefix]_winratio**|*float*|players_stats.csv|The ratio of wins over the number of matches played the player has for the match mode|
|**[prefix]_top1**|*float*|players_stats.csv|The cumulative number of wins the player has for the match mode|
|**lifetime_wins**|*float*|players_stats.csv|The cumulative number of wins the player has for all match modes|
|**solo_top10**|*float*|players_stats.csv|The cumulative number of times the player has reached the placement of top 10 in solo|
|**solo_top25**|*float*|players_stats.csv|The cumulative number of times the player has reached the placement of top 25 in solo|
|**duos_top5**|*float*|players_stats.csv|The cumulative number of times the player has reached the placement of top 5 in duos|
|**duos_top12**|*float*|players_stats.csv|The cumulative number of times the player has reached the placement of top 12 in duos|
|**trios_top3**|*float*|players_stats.csv|The cumulative number of times the player has reached the placement of top 3 in trios|
|**trios_top6**|*float*|players_stats.csv|The cumulative number of times the player has reached the placement of top 6 in trios|
|**squads_top3**|*float*|players_stats.csv|The cumulative number of times the player has reached the placement of top 3 in squads|
|**squads_top6**|*float*|players_stats.csv|The cumulative number of times the player has reached the placement of top 6 in squads|
|**lifetime_top3/5/10**|*float*|Engineered Feature|The overall cumulative number of times the player has reached the second tier of rankings |
|**lifetime_top6/12/25**|*float*|Engineered Feature|The overall cumulative number of times the player has reached the third tier of rankings |

The prefixes used above are as follows: 
|Prefix|Description|
|---|---|
|`solo`|The match mode where the team is of only 1 person to compete with the maximum of 100 players per match|
|`duos`|The match mode where the team is of a maximum of 2 persons to compete with the maximum of 100 players per match|
|`trios`|The match mode where the team is of a maximum of 3 persons to compete with the maximum of 100 players per match|
|`squads`|The match mode where the team is of a maximum of 4 persons to compete with the maximum of 100 players per match|
|`lifetime`|The overall statistics that is the total of the match modes `solo`, `duos`, `trios` and `squads`|

<div id="evaluation"></div>

## Model Evaluation

Table summarizing relative model performance:  

| model | GridSearch_score | train_score | test_score | accuracy | recall | precision | roc_auc_score | f1score | average_precision |
|---|---|---|---|---|---|---|---|---|---|
|**Dummy Classifier**|0.0144|0.0144|0.0144|0.9856|0.0000|0.0000|0.5000|0.0000|0.0144|
|**Logistic Regression**|0.0327|0.0347|0.0246|0.6611|0.5839|0.0247|0.6230|0.0473|0.0204|
|**K Neighbors Classifier**|0.1520|0.2769|0.1559|0.6201|0.6510|0.0244|0.6353|0.0471|0.0209|
|**Random Forest Classifier**|0.1945|0.3222|0.1861|0.8310|0.4899|0.0419|0.6629|0.0771|0.0279|
|**Light GBM Classifier**|0.2177|0.4424|0.2230|0.9275|0.4497|0.0913|0.6921|0.1518|0.0490|
|**Keras Classifier**|0.1543|0.2896|0.2005|0.7551|0.5906|0.0344|0.6741|0.0650|0.0262|

In order to choose the best model among the 6 models above, our evaluation will be based on the following scoring metrics:
- Average Precision: The Area Under the Precision-Recall Curve, the indicator of how well the model can identify all of the target classes without incorrectly marking too many of the non-target classes as the target. This is chosen over ROC AUC as the positive class is more important.
- Accuracy: The percentage of predictions being correct. However, as the most basic of scoring metrics, accuracy is useless by itself when the class is imbalanced and the model ends up being unable to predict the target class.
- Sensitivity: The percentage of the actual winnners that are correctly predicted. In this case, minimizing False Negatives is quite important as the ability to accurately predict the winners is important for anyone using this model to judge the match.
- F1 Score: The weighted average of Recall and Precision to determine how precise and robust the classifier is. As the data is imbalanced, this evaluates the model better than just accuracy alone as it takes into account the positive.

Based on those criteria, the model chosen is the Light GBM Classifier Model. With the highest score in Mean Average Precision at 0.049, and the highest score in Accuracy at 0.9275, as well as the highest F1 score at 0.1518, the performance of this model came at a cost, with the lowest score in Recall at 0.4497. Even so, taking all this into account, the Light GBM Classifier Model is the one that performs the best at this point.

The parameters for the production model are as follow: 
- **Model type: Light GBM Classifier**
- Boosting Type: Gradient Boosting Decision Trees
- Scale Pos Weight: 99  
- Max Bin: 200
- N Estimators: 200
- Learning Rate: 0.01
- Max Depth: 25
- Num Leaves: 255
- Min Child Samples: 200
- Colsample by Tree: 0.9
- Subsample: 0.9
- Subsample Freq: 2

Regrettably, the production model falls short as none of the evaluation metrics are above 0.5 except for accuracy. Most importantly, the primary indictator of average precision is at 0.049, which can be seen as slightly better than the baseline score of 0.0144. This can be attributed to the narrow scope of the predictor variables being purely based on historical player statistics and no match statistics being utilized at all. Recommendations on improvements to the model are covered in the prior section. 

<div id="future"></div>

## Future Steps

Based on the evaluation metrics for all of the models above, it can clearly be seen that none of the models are able to accurately predict the winner at a significant level. In fact, even the best model only has the `Mean Average Precision score of 0.049`, which can be seen as being slightly better than the baseline score of `0.0144`. As this is the limitations of the data currently available, the obvious step is to try to mitigate this issue through the gathering of more data. 

In this case, the first thing to look at is the fact that to limit the scale of the project, only the data for 1,000 matches are taken out of the original 5,888 matches. As such, if we were to utilize all of the available data, the increase in the amount of data available to manipulate and analyse might gives us a better result. Though it should be mentioned here that since the original scraping of data for the statistics of over 33,000 players had already taken over 16 hours, the scraping of data for the estimated 200,000 players would take at least 96 hours or 4 days. This might not be that feasible in the current context, but it is something that can be considered.

Another thing to look is to gather other forms of player statistics that are just game-based that may be useful. In this case, it may be something related to the ingame currency, the item shop inside the game Fortnite and the ownerships of said items. Though these may ultimately be of little use to the model, there may exist some unknown of relationship as player motivation can affect the performance of the player.

---

Of course, another future endeavour is to make use of the match statistics `eliminations`, `points` and `timealive` to assist in the modelling process. Though the actual model would turn out to be way better, the use of the match statistics ultimately means that the model would not be that useful since the act of winner prediction is usually done before the match starts. Of course, other variables such as `inserttime` and `geoidentities` may also be relevant as well.

In this case though, instead of winner prediction, the project can instead move to recommendations that increase the probability of being the winner, such as the number of `eliminations` needed, the minimum `timealive` and the optimal `inserttime`. The `geoidentities` would be useful in checking the ping time which is the lag that may affect the survival of the player.

There are also other match statistics that are not gathered that may be useful, like statistics regarding to objects destroyed or built. Though in this case, the difficulty would be trying to gather these information that are not readily available.

---

Ultimately, in the future, if a classifier model can be designed and be sufficient enough to predict the winner in a ready fashion for the Battle Royale game Fortnite, the next step is apply this model to other Battle Royale games, such as PlayerUnknown's Battlegrounds, Apex Legends and Garena Free Fire.

<div id="conclusion"></div>

## Conclusion

Nevertheless, despite the poor performance of the model, just by looking at the feature importances, we are able to partially address the goal set in the problem statement, which is the prediction of the winner. By looking at the variables `winratio` and `scorepermin`, especially with regards to the statistics relating to the `solo` match mode, it is possible to do a preliminary look at determining the potential winner of the solo match.


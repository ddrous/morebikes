# MoreBikes 2021
#### Rental bikes predictions using machine learning algorithms

## Introduction

This repository is for this [Kaggle competition](https://www.kaggle.com/c/morebikes2021/overview) by __Roussel__ and __Subin__. Our goal is to predict the number of bicycles at rental stations 3 hours in advance. We have written three notebooks to tackle different angles of the assignment.

To begin with, we need a better understanding of the given dataset. The data contain 4 station features, 8 time features, 7 weather features, 1 task-specific feature and 4 profile features plus 1 target variable. The target variable is 'bikes' and it is a non-negative integer representing the median number of available bikes during the respective hour in the respective rental station.

The assignment consists of three Phases. At Phase 1, we predict availability for each station for the next 3 months using the given data of 75 stations (Station 201 to Station 275) for 1 month. Two approaches are suggested: (a) a separate model for each station, (b) a single model on all stations together and to be compared based on MAE scores.

At Phase 2, we are given a set of linear models trained on other stations (Station 1 to Station 200) with data from a whole year. We are supposed to make use of the similarity among different stations and to find how to predict the stations in Phase 1 by only using these trained models. 

Finally, at Phase 3, we blend Phases 1 and 2 to achieve better performance.

Our code is available on [GitHub](https://github.com/desmond-rn/morebikes) and as Kaggle notebooks (see links above in the Experiments section) and completely reproducible.


## Experiments overview

We approach this problem by designing a simple model and interpreting its preliminary outputs. Firstly, we examine the correlation between 22 predictor variables and our target variable, after which we omit 5 time and weather features to focus on other variables with a higher correlation value. To deal with data drifting, we use data for the 25 first days of the month for training, and the remaining 6 days for validation.

After comparing the performance of 10 learning algorithms, we observe that when excluding random forests and [deep neural networks](https://www.kaggle.com/desmondrn/morebikes-deep-neural-network), linear regression models (with Ridge regularization) often achieved the lowest MAE score, with Poisson regression models often having the highest score. Hence, we choose a linear regression model as our main model. Linear regression models are easy to use, compute and interpret with a good performance. 

Next, we compare MAE scores of given models with 6 different sets of features: `short`, `short_temp`, `full`, `full_temp`, `short_full`, and `short_full_temp`, and find out that the difference is insignificant. MAE scores are 2.57 for all all-stations models and range from 2.45 to 2.47 for per-station models. Thus, we use `short_full_temp` features for following experiments.

To design a better-performing model, we combine our predictions with the given 200 pre-trained linear models by averaging and voting. For all instances, we find the weight our predictions should be assigned in order to minimize the combined MAE. Once the (optimal) weight is known, we duplicate our predictions before combining with the others.

## Experiments

### [MoreBikes - Analysis & Per-Station Training](https://www.kaggle.com/desmondrn/morebikes-analysis-per-station-training)
The goal of this notebook is to analyse the dataset and identify the best features and the best per-station machine learning algorithms to use in the next phases of the competition. Moreover, this analysis justifies splitting the training data to account for data drifting.

### [MoreBikes - All-Stations Training & Comparison](https://www.kaggle.com/desmondrn/morebikes-all-stations-training-comparison)
This second notebook deals with Phase 1 of the Kaggle competition. Firstly, we build 75 different models for each station for the task (a). Secondly, then we build one model for all stations together for the task (b). Finally, we compare these two approaches and discuss some variables that affect results in both cases.

### [MoreBikes - Peter's Models & Comparison](https://www.kaggle.com/desmondrn/morebikes-peter-s-models-comparison)
This last notebook addresses Phases 2 and 3. It builds on the results from the previous notebook to compare our models with the 200 linear models provided. We then combined those models' predictions with ours, hence reusing the knowledge learned from the whole year's data and the knowledge from appropriate stations.


## Results

Our results show that training multiple models for each station generally achieves better performance than training a single model for all stations. This is especially obvious when the training data is representative of the whole dataset with shuffling. When not shuffling the data, we observe exceptions with models such as decision trees and random forest that perform better when trained on all stations together. Considering the high computational cost of these models, we use per-station models trained without shuffling data. 

Furthermore, we observe that the MAE score improves when combining predictions from the 200 provided models and our custom-built ones. Once we find the optimal weights for combining our model with the given models, we make new predictions for the competition. With 50% of the test data, one of our models achieved a 2.47 MAE score at the [leaderboard](https://www.kaggle.com/c/morebikes2021/leaderboard).

## Conclusion

This assignment is a good practice to apply the knowledge from Machine Learning Paradigms. Our experiments reflect our own approaches to the assignment from understanding the dataset and the difference among machine learning algorithms, investigating the predictions of pre-trained models, making predictions with the optimal weights for model ensemble and drawing conclusions based on our own models.

We learned that linear regression models are efficient for this machine learning task. We also notice that models trained for each station with unshuffled data often make better predictions than a single model trained with all stations. That said, some of our highest scoring classifiers for the competition were trained on all stations together, highlighting the necessity to explore both techniques, especially when data drifting is a concern.

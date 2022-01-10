# MoreBikes 2021
## Rental bikes predictions using machine learning algorithms

## Introduction

This repository is for this [Kaggle competition](https://www.kaggle.com/c/morebikes2021/overview) by __Roussel__ and __Subin__. Our goal is to predict the number of bicycles at rental stations 3 hours in advance. We have written three notebooks to tackle 3 phases of the assignment.

To begin with, we need a better understanding of the given dataset. The data contain 4 station features, 8 time features, 7 weather features, 1 task-specific feature and 4 profile features plus 1 target variable. The target variable is 'bikes' and it is a non-negative integer representing the median number of available bikes during the respective hour in the respective rental station.

The assignment consists of 3 phases. At Phase 1, we predict availability for each station for the next 3 months using the given data of 75 stations (Station 201 to Station 275) for the period of one month. Two approaches are suggested: (a) a separate model for each station, (b) a single model on all stations together and to be compared based on MAE scores.

At Phase 2, we are given a set of linear models trained on other stations (Station 1 to Station 200) with the training data from a whole year. We are supposed to make use of the similarity among different stations and to find how to predict the stations in Phase 1 by only using these trained models. 

Finally, at Phase 3, we challenge to achieve better performance by our own models.


## Experiments overview

We approach this problem by designing a simple model and interpreting its preliminary outputs. Firstly, we examine the correlation between 22 predictor variables and our target variable and omitted 5 time and weather features to focus on other variables with a higher correlation value. After comparing the performance of 8 learning algorithms such as kNN, Random Forest and different regression models, we observe that the linear regression model achieved the lowest MAE score, while Poisson regression model having the highest score, when predicting the number of bikes at station 255. Hence, we choose a linear regression model as our main model. Linear regression models are easy to use, compute and interpret with a great performance. 

Next, we compare MAE scores of given 6 models with different features: short, short_temp, full, full_temp, short_full, and short_full_temp, and find out that the difference is insignificant. MAE scores are 2.57 for all all-stations models and ranges from 2.45 to 2.47 for per-station models. Thus, we conclude that per-station models perform bettern than all-stations models and we use short_full_temp features for following experiments.

To design a better-performing model, we combine our predictions of the given 200 pre-trained linear models by averaging and voting. For each instance, we find the weight our prediction should be assigned in order to minimize the combined MAE. Once the optimal weight is known, we duplicate our prediction by dupcount before combining with the others.

## Experiments

### [MoreBikes - Analysis & Per-Station Training](https://www.kaggle.com/desmondrn/morebikes-analysis-per-station-training)
The goal of this notebook is to analyse the dataset and identify the best features and the best per-station machine learning algorithms to use in the next phases of the competition. Moreover, this analysis justifies splitting the training data in order to account for data drifting.

### [MoreBikes - All-Stations Training & Comparison](https://www.kaggle.com/desmondrn/morebikes-all-stations-training-comparison)
This second notebook deals with Phase 1 of the Kaggle competition. Firstly, we build 75 different models for each station for task (a). Secondly, then we build one model for all stations together for task (b). Finally, we compare these two approaches and discuss some variables that affect the results in both cases.

### [MoreBikes - Peter's Models & Comparison](https://www.kaggle.com/desmondrn/morebikes-peter-s-models-comparison)
This last notebook addresses Phases 2 and 3 of the Kaggle competition. This work builds on the results from the previous notebook to compare our models with the 200 linear models provided. We then combined those models' predictions with ours, hence reusing the knowledge learned from the whole year's data and the knowledge from appropriate stations.


## Results

Our results show that training multiple models for each station generally achieves better performance than training a single model for all stations. This is especially obvious when the training data is representative of the whole dataset with shuffling. When not shuffling the data, we observe exceptions with some high-variance models such as Decision Trees and Random Forest that perform better when trained on all stations together. Considering the high computational cost of these models, we use per-station models trained wihout shuffling data. 

Furthermore, we observe that MAE score improves when combining predictions from the 200 provided models and our custom-built ones. Once we find the optimal weights for combining our model with the given models, we make new predictions for the competition. 

With 50% of the test data, our model achieves 2.47 MAE score at the [leaderboard](https://www.kaggle.com/c/morebikes2021/leaderboard).

## Conclusion


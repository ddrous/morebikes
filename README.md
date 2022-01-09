# MoreBikes 2021

This repository is for this [Kaggle competition](https://www.kaggle.com/c/morebikes2021/overview) for __'Roussel'__ and __'Subin'__. The goal is to predict the number of bicycles at rental stations 3 hours in advance. We have written three notebooks that deal with seperate aspects of the asignment.

### [MoreBikes - Analysis & Per-Station Training](https://www.kaggle.com/desmondrn/motorbikes-analysis-per-station-training)
The goal of this notebook is to analyse the dataset and identify the best features and the best per-station machine learning algorithms to use in the next phases of the competition. Moreover, this analysis justifies splitting the training data in order to to account for data drifting.

### [MoreBikes - All-Stations Training & Comparison](https://www.kaggle.com/desmondrn/motorbikes-all-stations-training-comparison)
This second notebook deals with Phase 1 of the Kaggle competition. Firstly, we build 75 different models for each station for task (a), then we build one model for all stations together for task (b). After that, we compare these two approaches and discuss some variables that affect the results in both cases.

### [MoreBikes - Peter's Models & Comparison](https://www.kaggle.com/desmondrn/motorbikes-peter-s-models-comparison)
This last notebook addresses Phases 2 and 3 of the Kaggle competition. This work builds on the results from the previous notebook to compare our models with 200 pre-trained linear models that were provided. We then combined those models' predictions with ours from Phase 1, hence reusing the knowledge learned from the whole year's data and the knowledge from appropriate stations.

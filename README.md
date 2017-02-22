# Capstone Project
## Welcome to the repo for my Capstone Project as Galvanize San Francisco.

The project focuses on predicting DotA 2 match outcomes by looking at the first 10 minutes of each match. Here are the goals:

1. Work on a set of 50,000 ranked matches in DotA 2 before the infamous 7.00 patch, and later expand into an even larger dataset offered by yasp.co on Academic Torrents.
2. Data for each game contains massive amounts of information. However, no standalone features is provided. I have to create my feature matrix from scratch by aggregating information over multiple datasets.
3. Start by focusing on the first 10 minutes of each game, find the influence of hero drafting and player information within that timeframe over the outcome.
4. There are many ways I could aggregate player information within the first 10 minutes of the game, I will try multiple feature engineering strategies to find the best set of features.
5. Investigate the interaction between player(hero) roles and match specific information to see if I could improve prediction accuracy.
6. Plot accuracy of my model over varying observed timeframe of a match to see if there are generic deciding moments in matches. 

For those of you interested in the dataset I used, here's the link:

https://www.kaggle.com/devinanzelmo/dota-2-matches

## Directory

### 1. SRC
This folder contains all python scripts I used for feature engineering and data visualization.
### 2. img
This folder contains all useful/insightful graphs for my project.
### 3. DotA2_presentation_explanatory
This is the verbose version of my project presentation. You will find explanation for my decisions and findings in there.
### 4. DotA2_Explore_Analyze
This is Jupyter Notebook version of my data exploration and analysis, if you want see more beyond the powerpoint.

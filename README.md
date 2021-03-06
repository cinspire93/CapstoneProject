# DotA 2 Match Prediction

## Objective:
One of the most nagging issue in the gaming industry is game balance. Imagine a 40-minute game, whose outcome can be predicted reliably at the 10-minute mark every single time. Would you play this game? My guess is no. Any predictable game is, for the lack of a better word, lame. In gaming terms, such is game is called unbalanced. Game imbalance is almost always caused by some overpowered mechanics that could single-handedly decide the outcome of a game. My goal is to investigate whether DotA 2 is a balanced game.

DotA 2, or Defense of the Ancients 2, is a multiplayer online battle arena(MOBA) game, in which 10 players get split into two teams of 5, called the radiant and the dire, who then try to destroy each other's ancient. It is currently the second most popular MOBA game in the industry, netting over 200 millions per year for its developer--Valve. It should not come as a surprise that DotA 2's balance is of utmost interest to its developer and players.

Since game balance is directly tied to predictability, I am checked the balance of DotA 2 by predicting the outcome after observing only the first 10 minutes of any given match. The higher my prediction accuracy get, the more imbalanced the game would be. This main objective also allowed me to accomplish two side goals simultaneously. First, an accurate prediction model can offer solid strategic advice to players looking to maximize their win rates. The features in my model will help players better study the game mechanics. Second, in-game wagering systems that allow players to bet the outcome of their current match in the first minute can benefit a lot from a good prediction model as well.

## Case Study:
### Preprocessing
Unlike any data I have dealt with before, this dataset was very widely distributed in terms of content. There were 19 tables in total, each of them representing a different concentration of data. Luckily, the data was relatively well-behaved, considering that all the data came directly from the DotA 2 api. All the missing values I had to deal with could be filled in uniformly with 0 while conforming to each feature's definition. There was a slight hiccup, however, with the coding of hero_id. The original hero_names.csv skipped 24 as a hero_id. This problem was also present in players.csv. I fixed them with a correction mapping.

### Methodology
The target variable (outcome of the match) is very balanced. The heavy lifting was in feature engineering. Although the dataset was super rich, it did not offer any useful in-place features that I could use immediately. This motivated me to create every one of my features from scratch using domain knowledge.

#### Feature Engineering
1. Hero Selection: Every player has to select a different hero for each game. DotA 2 heroes do not just look different, they also serve different purposes. It is pretty intuitive that certain hero compositions will help players win a game. There were 112 heroes in total. I doubled the roster to create a list for both the radiant and the dire team, and gave 1 to selected heroes and 0 otherwise.
2. Net worth at the 10-minute mark: Net worth represents the total amount of gold (in terms of both item assets and gold itself). The higher the net worth, the better items a hero could get. Good items both amplify a hero's power and cover its deficiency. Thus, heroes with higher net worth will be more likely to win the game. I got this feature by referencing the player_time.csv in the dataset.
3. Others: Other features that I engineered include net death counts from team fights before 10 minutes, team composition of hero roles and the interaction between each hero's role and its net worth. Higher team fight death counts implies that a team is more likely to lose. Having a bad composition of roles (too many or few of one role) may put a team at a disadvantage. A non-core hero doing extremely well in terms of net worth may indicate that a team is dominating the game. Sadly, these features did not help much.

### Results
I ended up using logistic regression as my model and hero selection + net worth at the 10-minute mark as my feature matrix. This model gave me an accuracy score of 68.5%. There are two questions: Why did I use logistic regression instead of any other classifier model? How do I make sense of the near 70% accuracy? Allow me to address them one by one.

The first question is valid in the sense that boosting and random forest classifiers are known to be excellent models that almost always outperform logistic regression. However, my dataset provided an exception. My logistic regression model consistently bested boosting and random forest classifiers in terms of accuracy. My hypothesis for such bizarre phenomenon is the sparsity of information. When every feature in my feature matrix contributes somewhat equally to the game outcome, it is hard for classifiers that rely on information gain to generate accurate results. In other words, boosting and random forest classifiers will struggle with finding the best features that predict the outcome of the game because every feature is useful. I manipulated the random subsetting of features to be 80% to 100% (non-stochastic boost or bagged forest) in hopes of avoiding missing important features. The results still did not improve much. I was careful with my application of logistic regression as well. Logistic regression required that each game should be somewhat independent of each other. This is in general not true, because one player can play multiple games, often with the same people. Fortunately, the player information indicated that players in general played no more than 100 games in a sample of 50000 games. This made the independence assumption tangible, and by extension, logistic regression reasonable.

The second question is more interesting. A 70% accuracy in prediction was by no means low. It was very tempting to conclude that DotA 2 is an unbalanced game. However, I decided to dig more into the issue. Since I was looking at the net worth information at the 10-minute mark, my model was in fact absorbing a time slice. I could collect all the net worth information at 60-second intervals within the first 10 minutes, retrain my model at each of those time marks and check how accuracy stacks up against time.

![Accuracy vs. Time](/img/accuracy_vs_time.png)

What I found was a linear relationship between the time passed and the accuracy of my model. The rate at which game-deciding information increased was constant. This meant that there was no point in time within the first 10 minutes that could offer significant insight into how the game would end. In terms of time, the game is actually balanced because early game should not decide the outcome. Notice, however, that my model's accuracy did not start from 50% at time 0. This implied that the other part of my feature matrix, or hero selection, actually mattered. Bad strategic decisions when picking heroes can actually cost a team the game.

### Conclusion
In conclusion, the game is balanced with respect to time, but no so much with respect to hero selection. However, I believe that the ability to out-draft one's enemy team is a necessary skill for any good player. The accuracy of my model is not so high to cause DotA 2 developers panic, but it is decent enough to make my model a solid hero strategy recommender. Moreover, the in-game wagering system could also benefit quite a lot from my model.

### Future Work
1. Reference the official DotA 2 api and parse the rich data myself and see if I could get more useful features that would improve my model
2. Look at game information from a player perspective, predicting the outcome of any given match based on player skills

## Questions:
Feel free to reach out to me at __cchen9331@gmail.com__

## Directory:
1. _**img**_ folder contains all graphs that I found useful for my analysis
2. _**src**_ folder contains my python files that I used for cleaning up my data, feature engineering and plotting
3. _**DotA2_Explore_Analyze**_ is the Jupyter notebook version of my analysis, and offers sneakpeak of the dataset
4. _**DotA2_Presentation**_ is the verbose version of my presentation on DotA 2 project

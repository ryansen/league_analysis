
# Top Laner's Individual Performance to Win Analysis

An analysis of the critical factors for success in the top lane role of League of Legends esports.

---

By Ryan Lee

## Introduction
League of Legends is an incredibly popular 5 v 5 multiplayer online battle arena (MOBA) video game. This analysis will be done on one of the 5 positions in the game, the "Top Laner". The data I will be working with is from the 2022 esport season of League of Legends and has been developed by the Oracle's Elixer. This means all of the data is pro-player centered. 

A common sentiment among many Top Lane players is that their role does not matter or that it is very hard for a good Top Laner to consistently convert their skill to win a match. This is due to the fact that League of Legends is a team game with 5 players on each side. Top Laners face-off in the 'Top Lane' of the map which is very isolated from the rest of the 8 players on the map. 

The question that I want to answer is **Which stats of a Top laner are best suited to predicting a win?**

## Columns 

This data set of pro-player matches contains necessary post-game data combined with match wins and losses. This analysis will only be examining the stats of individual Top Lane players, meaning there are 2 rows of data per game as each team in a matchup will have a Top Laner. This results in the current Data Frame containing 21244 rows. 

- **`gameid`**: A unique identifier for each game, typically formatted as a combination of tournament, date, and match number 

- **`result`**: Indicates the outcome of the game for the team.
  - `True`: Win
  - `False`: Loss

- **`firstbloodkill`**: Indicates whether the team secured the first kill of the game.
  - `True`: Yes, the player got the first blood.
  - `False`: No, the player did not get first blood. 

- **`xpdiffat15`**: The players experience point difference at the 15-minute mark.
  - Positive values indicate an advantage, and negative values indicate a deficit.

- **`golddiffat15`**: The player's gold difference at the 15-minute mark.
  - Positive values indicate a gold lead, and negative values indicate a gold deficit.

- **`damageshare`**: The percentage of total team damage dealt by the player(s) in this role during the game.

- **`side`**: The side of the map the team/player started on:
  - `Blue`: Blue side
  - `Red`: Red side

- **`earnedgold`**: The total amount of gold earned by the player during the game.

- **`kills`**: The total number of kills secured by the player during the game.

- **`deaths`**: The total number of deaths incurred by the layer during the game.

- **`assists`**: The total number of assists the player earned during the game.

- **`dpm`**: Damage per minute dealt by the player.

- **`cspm`**: Creep score per minute achieved by the player.

- **`gamelength`**: The length in time of each game that is played.

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning

First I set the index to 'gameid', 

I kept the relevant columns to a Top Laner's stats that measure their individual performance and other relevant columns on the game state:
`result`, `firstbloodkill`, `xpdiffat15`, `golddiffat15`, `damageshare`, `side`, `earnedgold`, `kills`, `deaths`, `assists`, `dpm`, `cspm`, `gamelength`

I also filtered the data to only keep individual Top Laner's stats. Rows that contained missing data were removed based on a `datacompleteness` column that was provided in the data set because there would be up to 5 missing cells per row with missing data. Furthermore, `gamelength` column was converted to datetime objects. 

| gameid              | result   | firstbloodkill   |   xpdiffat15 |   golddiffat15 |   damageshare | side   |   earnedgold |   kills |   deaths |   assists |      dpm |   cspm |
|:--------------------|:---------|:-----------------|-------------:|---------------:|--------------:|:-------|-------------:|--------:|---------:|----------:|---------:|-------:|
| ESPORTSTMNT01_2690210 | False    | False            |        345   |          391   |      0.278784 | Blue   |        7164   |       2 |        3 |         2 | 552.2942 | 8.0911 |
| ESPORTSTMNT01_2690210 | True     | False            |       -345   |         -391   |      0.218428 | Red    |        6231   |       1 |        1 |        12 | 611.3835 | 8.0210 |
| ESPORTSTMNT01_2690219 | False    | False            |       -652   |        -1484   |      0.159184 | Blue   |        6488   |       0 |        5 |         2 | 269.1769 | 6.9536 |
| ESPORTSTMNT01_2690219 | True     | False            |        652   |         1484   |      0.315704 | Red    |       13289   |       2 |        2 |         6 | 670.7285 | 9.6783 |
| ESPORTSTMNT01_2690227 | True     | False            |         85   |          904   |      0.192858 | Blue   |        9771   |       3 |        2 |         7 | 395.3550 | 9.2495 |

### Univariate Analysis

<iframe
  src="assets/xp_diff_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This plot describes the distribution of XP Difference at 15 minutes among Top Laners. It displays a normal distribution as it is symmetric. 

### Bivariate Analysis

<iframe
  src="assets/earned_gold_vs_result.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This is a box plot that shows the distribution of 'earned gold 'by whether a game was won or not. This plot indicates that 'earned gold' by a top laner is higher when they win a match as the spread is higher. 


<iframe
  src="assets/dpm_vs_gold.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This scatter plot shows the relation between 'damage per minute' and 'earned gold' which appears to be a linear correlation.

### Interesting Aggregates

| result   | golddiffat15 (False) | golddiffat15 (True) | xpdiffat15 (False) | xpdiffat15 (True) |
|:---------|----------------------:|--------------------:|-------------------:|------------------:|
| False    |          -381.674055 |         478.299539 |         -249.027279 |        127.038402 |
| True     |           248.728519 |        1103.597793 |          190.440831 |        569.063190 |


This pivot table summarizes the mean values of 'golddiffat15' and 'xpdiffat15' ,
grouped by two categorical variables: 'result' (win/loss) and 'firstbloodkill' (whether the first blood kill occurred). This table implies that 'golddiffat15' and 'xpdiffat15' greatly increase when a Top Laner gets the first kill of the match. 

Oftentimes a first blood by a Top Laner is a 'solo kill' (no teammates are present) meaning that a first blood is a good indicator of a Top Laner's skill over the enemy Top Laner. 

## Assessment of Missingness

### NMAR Analysis

I believe that the 'dpm' column is NMAR there is two missing values. This is due to the fact that is the 'damage per minute' and it is possible that damage dealt by a player could not be calculated in the case of a pro match where there is tech issues and the game was not able to properly start. There would be no damage to calculate or time to divide that damage by. 

### Missingness Dependency

This part of the analysis focuses on testing the missingness of `'golddiffat15'` based on other columns. The columns being tested for whether `'golddiffat15'` is dependent on them being missing are `'datacompleteness'` and `'gamelength'`.

I will use **Total Variance Distance (TVD)** as the test statistic, and the significance level will be set at 0.01.

#### Hypotheses
- **Null Hypothesis**: The distribution of missing `'golddiffat15'` values is independent of `'datacompleteness'` and `'gamelength'`.
- **Alternative Hypothesis**: The distribution of missing `'golddiffat15'` values depends on `'datacompleteness'` and `'gamelength'`.

To test the Missingness at Random (MAR), I conducted a permutation test by shuffling `'gamelength'` to determine if the missingness of `'golddiffat15'` is dependent on it, repeating this process 500 times. I then calculated the proportion of simulated TVDs greater than or equal to the observed TVD to obtain the p-value. The resulting p-value was `0.0`, meaning that we reject the null hypothesis at a 0.01 significance level.

Next, I performed the same procedure with `'datacompleteness'` and obtained a p-value of `1.0`. This means we fail to reject the null hypothesis at a 0.01 significance level, indicating that the missingness of `'golddiffat15'` is not dependent on the `'datacompleteness'` column.

## Hypothesis Testing

### Hypothesis Testing Results

In this analysis, we tested the relationship between the game outcome (`result`) and three factors: `'xpdiffat15'`, `'kills'`, and `'damageshare'`. The goal was to determine whether these factors significantly influence the outcome of a game using a permutation test.

#### Hypotheses

- **Null Hypothesis (H₀)**: There is no association between the factor and the game outcome (`result`), meaning any observed differences are due to random chance.
- **Alternative Hypothesis (Hₐ)**: There is an association between the factor and the game outcome (`result`), meaning the observed differences are not solely due to random chance.

#### Test Statistic and Significance Level

- **Test Statistic**: The observed difference in means of the factor values between games that resulted in a win (`result=True`) and games that resulted in a loss (`result=False`).
- **Significance Level (α)**: 0.05

#### Results

1. **Factor: `'xpdiffat15'`**
   - **Observed Difference**: `451.96`
   - **p-value**: `0.0`
   - **Conclusion**: The p-value is less than 0.05, so we reject the null hypothesis. This suggests that `'xpdiffat15'` is likely associated with the game outcome.

2. **Factor: `'kills'`**
   - **Observed Difference**: `1.97`
   - **p-value**: `0.0`
   - **Conclusion**: The p-value is less than 0.05, so we reject the null hypothesis. This suggests that `'kills'` is likely associated with the game outcome.

3. **Factor: `'damageshare'`**
   - **Observed Difference**: `-0.0058`
   - **p-value**: `1.0`
   - **Conclusion**: The p-value is greater than 0.05, so we fail to reject the null hypothesis. This suggests that `'damageshare'` is not significantly associated with the game outcome.

#### Justification of Methodology

- **Permutation Test**: This method was chosen as it does not rely on assumptions about the distribution of the data, making it suitable for the dataset.
- **Significance Level (0.05)**: A commonly used threshold in statistical testing, which balances the risk of Type I and Type II errors.
- **Test Statistic**: The difference in means is an intuitive measure of the association between the factor and the outcome.

#### Limitations

- These tests do not prove causation, as they are based on observed data and not randomized controlled experiments.
- The conclusions are based on the significance level and the p-value, which are inherently probabilistic measures.

#### Visualization

The empirical distribution of differences obtained from the permutations for `'xpdiffat15'` showed that the observed difference is far in the tail, consistent with a low p-value. A similar pattern was observed for `'kills'`, while `'damageshare'` demonstrated no such pattern, aligning with the high p-value.

These findings highlight that `'xpdiffat15'` and `'kills'` are important factors to consider in understanding game outcomes, while `'damageshare'` appears to have a weaker or negligible association.

## Framing a Prediction Problem

The goal of this analysis is to predict whether a Top Laner will win their game based on their individual post-game performance stats. This is a **binary classification** problem, as the target variable (`result`) has two possible outcomes: `True` (win) or `False` (loss).

#### Response Variable
- **Response Variable**: `result` (whether a Top Laner wins or loses their game).
- **Reason for Choosing `result`**: This variable directly reflects the outcome of the game, which aligns with the objective of understanding the relationship between individual performance and winning. By focusing on this variable, we can evaluate the importance of Top Lane stats in determining game success.

#### Features Used
We are using **post-game stats** to train the model, including:
- **`golddiffat15`**: Represents the gold difference at 15 minutes. This is a critical indicator of how much advantage the Top Laner was able to achieve in the early game, typically in a 1v1 scenario.
- Other features such as `kills`, `deaths`, `assists`, and `damageshare`, which collectively capture individual contributions to the game's outcome.

#### Justification for Feature Selection
All features used in the model are **available at the time of prediction** as they are derived from post-game stats. The goal is to evaluate whether these stats "matter" in determining the Top Laner's ability to contribute to a win.

#### Evaluation Metric
- **Metric Used**: Accuracy
  - **Reason**: Accuracy is a straightforward metric that measures the proportion of correct predictions out of all predictions. Since the dataset is not heavily imbalanced (i.e., both wins and losses are reasonably represented), accuracy is a reliable metric for this problem.
  - **Other Metrics Considered**: Metrics like F1-score were considered, but since the primary goal is to assess the overall correctness of the model in predicting wins or losses, accuracy was deemed most suitable.

#### Context and Limitations
- This analysis focuses on **post-game data**, meaning the stats are only available after the game is completed. The goal is not to predict the outcome in real-time but rather to understand how individual Top Lane performance contributes to winning.
- Metrics like `golddiffat15` are particularly valuable as they capture early-game performance in a largely isolated 1v1 context before teamplay dynamics take over. This allows for a focused analysis of the Top Laner's individual impact.

By leveraging a **Decision Tree Classifier**, we can evaluate the significance of individual stats in determining game outcomes and interpret the model to identify the most influential factors.

## Baseline Model

### Model Description

#### Features in the Model
The features used in this decision tree model are as follows:

1. **Quantitative Features** (6):
   - `firstbloodkill`: A binary variable indicating whether the player got the first blood kill.
   - `golddiffat15`: The gold difference at 15 minutes, indicating the player's early-game performance.
   - `damageshare`: The proportion of the total team damage dealt by the player.
   - `earnedgold`: The total gold earned by the player in the game.
   - `dpm` (Damage Per Minute): The amount of damage dealt by the player per minute.
   - `cspm` (Creep Score Per Minute): The player's creep score per minute.

2. **Nominal Features** (0): None in this specific model, as no categorical variables like "side" were included.

3. **Ordinal Features** (0): None in this specific model, as the features are either continuous or binary.

---

#### Preprocessing and Encodings
- **Scaling**: All quantitative features were scaled using `StandardScaler` to normalize the data and ensure that the decision tree's splits are not biased by differing feature magnitudes.
- **No Encoding Needed**: Since all features are quantitative, no encoding for categorical variables was performed.

---

#### Model Performance
- **Training Accuracy**: The model achieved an accuracy of **69%** on the training set. 
- **Test Performance**: The model achieved an accuracy of **69%** on the test set. 

---

#### Assessment of Model Quality
1. **Strengths**:
   - The model captures early-game and individual performance metrics that are highly relevant for predicting a player's contribution to the game's outcome.
   - Decision trees are interpretable, making it easy to understand how features contribute to the model's predictions.

2. **Weaknesses**:
   - The training accuracy of **69%** suggests that the model may not capture all patterns in the data and could benefit from additional features or more complex modeling approaches (e.g., random forests or gradient boosting).
   - The use of a depth-limited decision tree (`max_depth=2`) might lead to underfitting, as the tree may not be complex enough to capture all important splits.

3. **Conclusion**:
   - While the model provides insights into feature importance and relationships, the performance indicates that there is room for improvement. Testing with a more flexible tree depth or using ensemble methods could enhance its predictive capability.
   - Additional features, such as `side` or other game statistics, could further improve the model's performance.

---

### Next Steps
- Evaluate the model's performance on the test set and compute metrics such as precision, recall, and F1-score.
- Experiment with hyperparameter tuning (e.g., increasing `max_depth` or using ensemble methods like Random Forest).
- Include nominal or ordinal features such as the `side` of the player and encode them appropriately to enrich the model.

## Final Model

### Model Description and Feature Selection

#### Features Added
1. **`earnedgold`** (Quantile Transformed):
   - **Reason for Inclusion**: This feature represents the total gold earned by a player, which is a direct indicator of resource efficiency and contributions to the game. Transforming it using a quantile transformer ensures that its skewed distribution is normalized, making it more comparable across players.
   - **Impact on Model**: Helps the model better evaluate resource accumulation differences among players.

2. **`damageshare`** (Binarized):
   - **Reason for Inclusion**: Damage share reflects how much of the team's total damage the player contributes. Binarizing it around a threshold of 0.25 allows the model to distinguish high contributors from average or below-average contributors.
   - **Impact on Model**: Adds clarity to the role of significant damage contributors without noise from smaller variations.

3. **`side`** (One-Hot Encoded):
   - **Reason for Inclusion**: This categorical variable represents whether the player was on the Blue or Red side of the map. The side influences strategic advantages, such as vision placement and early-game pathing.
   - **Impact on Model**: Helps capture map-based discrepancies that can affect performance.

---

### Modeling Algorithm and Hyperparameter Selection
1. **Modeling Algorithm**:
   - A **Decision Tree Classifier** was chosen for its interpretability and ability to handle mixed feature types. This model structure is ideal for identifying key splits in performance metrics that correlate with game outcomes.

2. **Hyperparameter Selection**:
   - The following hyperparameters were tuned using **GridSearchCV**:
     - `max_depth`: Controls the depth of the tree to prevent overfitting.
     - `min_samples_split`: Ensures that splits are meaningful by requiring a minimum number of samples.
     - `criterion`: Evaluates splits using either Gini impurity or entropy.

3. **Best Hyperparameters**:
   - `max_depth`: `5`
   - `min_samples_split`: `20`
   - `criterion`: `entropy`

4. **Selection Method**:
   - **GridSearchCV** with 5-fold cross-validation was employed to evaluate combinations of hyperparameters. This method ensures that the model generalizes well to unseen data by testing on multiple folds.

---

### Model Performance

#### Baseline Model
- The baseline model used a limited feature set and a fixed depth decision tree (`max_depth=2`), achieving a training accuracy of **69%**.
  
#### Final Model
- The final model incorporates additional features (`earnedgold`, `damageshare`, `side`) and uses hyperparameter tuning for optimization.
- **Performance Metrics**:
  - **Training Accuracy**: **82%**
  - **Test Accuracy**: **78%**
  - **Precision Score**: **0.75**

#### Improvement Over Baseline
- The final model shows a significant improvement in accuracy and precision over the baseline model due to:
  - Inclusion of relevant features derived from the game's data-generating process.
  - Hyperparameter tuning to optimize splits and prevent overfitting.

---

### Confusion Matrix
Below is an interactive confusion matrix that visualizes the final model's performance:

<iframe
  src="confusion_matrix.html" 
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

### Conclusion
The final model outperforms the baseline by leveraging features that reflect the underlying dynamics of individual performance in games. It captures early-game metrics (`golddiffat15`), team contributions (`damageshare`), and structural factors (`side`) to make more accurate predictions. The inclusion of feature transformations and hyperparameter optimization ensures the model is both interpretable and robust.

## Fairness Analysis
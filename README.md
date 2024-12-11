
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

### Missingness Dependency

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
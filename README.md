
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

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning


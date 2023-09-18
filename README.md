Kaggle Notebook: https://www.kaggle.com/ethanplee/nfl-big-data-bowl-2022

# A Metric for Identifying Individual Performance in Punt Coverage

### Ethan Lee (University of California, Irvine)

The focus on the 2021 - 2022 Big Data Bowl is special teams. This notebook submission attempts to create a new special teams metric and subsequently rank teams and players on punt coverage plays

# Introduction: Player Distance to Returner at Punt Receive

- On NFL punt plays, the punt unit will (1) block for the punter and (2) sprint down the field to get to the ball. In order to limit return yards, punt coverage teams want to get down the field as fast as possible to be in the best possible position to force a fair catch, tackle the returner, or down the ball where it lands.
- Thus, the "closeness" of the distance between punt coverage players and the returner at the exact moment the ball is received can be a valuable metric to predict how many yards the returner might gain.

# "Close" Players

- Given the data, we predict that a "close" player is a punt coverage player who has the effect of lowering return yards by attempting to zone out or tackle the returner.
- We will establish the threshold of a "close" player being within 10 yards of the returner at the moment the ball is caught.
- Now we will find the number of "close" coverage players for every punt return.
- By finding the Pearson Correlation Coefficient between "close" players and return yardage, we find that there is an approximately 0% likelihood that the true value of r is 0. That is to say, it is very unlikely that there is no correlation between "close" players and return yardage. We can conclude that as the number of "close" players increases on a punt return, the return yardage will tend to be lower. 
- Here we will predict the Expected Return Yards using a simple linear model between the number of "close" players and the return yardage. For example, if there are 3 "close" players within the returner at the moment the ball is received, then the returner is expected to gain about 6.2 yards. Then we can find Return Yards Over Expected by calculating the actual return yards minus the expected.

![Expected Return Yards by Number of "Close" Players](https://github.com/ELtrebolt/2022-NFL-Big-Data-Bowl/assets/44250460/b7ee9f8a-5871-4298-a123-6dd0f9a6f821)

# Correlation Between "Close" Players and Return Yardage

- We can now take the data for every unique punt return play and group by team to rank the performance of special teams units in 2018. 
- We want to cover:
  * Punt Coverage - how well do teams get close to the opposing returner... versus the Opposing Return Yardage
  * Punt Return Coverage - how well do teams' opponents get close to the returner... versus Return Yardage
- To identify the most relevant data points from above, we will use a heatmap to test correlations between 2 variables. What sticks out in particular is the correlation between the percentage of plays where there is 1+ "close" coverage players versus the % of plays where the opposing return yardage is 5 or less: there is a pearson correlation coefficient of .55 with a 0.001% chance that r = 0.
- Of course, we must note that the number of "Close" Players can be unimportant if a coverage player is "Close" to the returner, but is being blocked by an opposing player. There are many factors that lead to the all-important variable of punt return yardage, and we will simply look at its correlation with the "Close" Players variable.

![Heatmap](https://github.com/ELtrebolt/2022-NFL-Big-Data-Bowl/assets/44250460/b41d9151-eeca-4d66-937b-f30555823e17)

# Visualize Data

Visualization templates from: https://gist.github.com/Deryck97/dff8d33e9f841568201a2a0d5519ac5e

## Team Rankings: Punt Coverage

- First we will graph teams based on the average number of players "close" to the punt returner on punt coverages
- In other words, every time team X punts, about how many of their special teams players will be within 10 feet of the returner at the moment the returner catches the ball?
- Based on the graph, we expect the Colts and Falcons in particular to have good punt coverage (thus having the least amount of opposing avg. return yards) while the Ravens and Rams are expected to have poor punt coverage (the highest amount of opposing avg. return yards). 

![Average Number of "Close" Players Per Punt Coverage](https://github.com/ELtrebolt/2022-NFL-Big-Data-Bowl/assets/44250460/89234459-4254-4e84-8cf3-a2eb69d94686)

In the below scatterplot, we see that while the Colts do have the lowest opposing average punt return, teams like the Broncos and Browns do not have the predicted effect of a lower opposing return yardage through a higher number of "close" players. This suggests that the Colts' "close" players prevented the returner from gaining more yards, while the Broncos and Browns number of "close" players did not help decrease the opposing return yardage.

![Average Opponent Return Yards vs Average Number of "Close" Players](https://github.com/ELtrebolt/2022-NFL-Big-Data-Bowl/assets/44250460/ed60a1f1-9974-42ec-8d35-09bd3d155433)

Here we analyze the metrics with the strongest correlation found in the heatmap earlier:

![Percentage of Plays with 1+ "Close" Players versus Percentage of Plays with Opponent Returns of <5 Yards](https://github.com/ELtrebolt/2022-NFL-Big-Data-Bowl/assets/44250460/33f2502a-4a06-4dba-88bf-4142af302e38)

## Team Rankings: Punt Return Coverage

The idea here is the same - as there are more "close" opponent players to the returner, there should be lower return yardage. Or, as there are less "close" opposing players to the returner, the returner will tend to gain more yardage. Based off of the graph, we expect the 49ers and Giants to have the least punt return yardage and the Panthers and Falcons to have the highest return yardage.

![Average Number Of "Close" Opposing Players Per Punt Return](https://github.com/ELtrebolt/2022-NFL-Big-Data-Bowl/assets/44250460/83395c35-7832-418f-8975-d022e8a90936)

While the 49ers and Giants do have low average return yards, the Falcons and Panthers also have a below-average average return yards. This suggests that even though the Falcons and Panthers had less "close" players to their returner, they could not gain more yardage on punt returns as expected. There seems to be little correlation between the average return yards and the average number of opposing "close" players.

![Average Return Yards vs Average Number of "Close" Opposing Players](https://github.com/ELtrebolt/2022-NFL-Big-Data-Bowl/assets/44250460/7a7b086a-95c0-430c-b4a6-ee6c68b1c157)

## Individual Gunner Rankings

Here we rank the top 20 gunners by the percentage of plays where they were "close" to the returner on punt receive (minimum 10 plays).

![Average Opposing Return Yards vs Percentage of "Close" Plays Per Gunner](https://github.com/ELtrebolt/2022-NFL-Big-Data-Bowl/assets/44250460/d46104b9-5d45-45be-a46e-2f19565d87b9)

![Gunners Who Were Most Often "Close" To The Returner](https://github.com/ELtrebolt/2022-NFL-Big-Data-Bowl/assets/44250460/0f512832-6e9e-49a2-9ba6-f4f2603d7cf1)

![Non-Gunners Who Were Most Often "Close" To The Returner](https://github.com/ELtrebolt/2022-NFL-Big-Data-Bowl/assets/44250460/4550c9eb-24db-4359-9d48-8c0e68f5fbb2)

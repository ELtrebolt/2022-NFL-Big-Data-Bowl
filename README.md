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

![Expected Return Yards by Number of "Close" Players](https://www.kaggleusercontent.com/kf/84681128/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..zyKEhwiTfLeNE9GQJZslOA.dUgJdjPXGB7BTkjou6cm4RVmBhYZiM69H5YF1JksNMWlciiyXULtQENX2kTXBfJMOmcMwtEQPDu2AkcF6mR7_MT0aRECFNLsNdnOH_qvhFrJlxAAtEfQOM8UiyEVuNuh4AUXPR24f2-pSHxqxW9xC4WPUeFDtNNkmowMHmJ32L6WDJK6K-FZYfL0Hs0JTtvL5CWYqnZPbkgfunmq7jc-MxR3E9whG-m9OhkIO6rm9xv-jz-WBk5uQdqSU1o3Y8jA1YfQA84p31Wnggw_QGv06mHQ9kYE2JHiuPZ_rgoiO_94wR3cTdO-aHlPCy1RNRdX-cNv3UwtluGQmJZUp0Bf5teAJEs5Rjn28nGue0tWo0fXrVxsrsI9jihXzSOE_ciCK-DcDqD7cQf9ZAR_FrhhBCePLJWbB9SQRjiJCzoLe39xXFBIcYbxOWP_iLE-6qKqQRgDMi7IzDEdV2AKt6eNUiBGvmhy953ilXAAGJOnx1EFv1EnXZlUAqqEytg2jMXILhU7HPmIDBu86rC3fGAymZR8zIwm_yPKAZhzJSdzMQPlvjF3vJAZfGAMZCKcRu3gs8pUTipcoNKk5BDuIdmPlsnErmYImaJBuM854E2dVPj2zFtNVopZrP5000_88vI9GUj7O37FqqCTin-bUnN9jFYzJF3qgQNDTg7y6fj32uw.NBDvxZ0j3sCtcFPt7iafRw/__results___files/__results___24_0.png)

# Correlation Between "Close" Players and Return Yardage

- We can now take the data for every unique punt return play and group by team to rank the performance of special teams units in 2018. 
- We want to cover:
  * Punt Coverage - how well do teams get close to the opposing returner... versus the Opposing Return Yardage
  * Punt Return Coverage - how well do teams' opponents get close to the returner... versus Return Yardage
- To identify the most relevant data points from above, we will use a heatmap to test correlations between 2 variables. What sticks out in particular is the correlation between the percentage of plays where there is 1+ "close" coverage players versus the % of plays where the opposing return yardage is 5 or less: there is a pearson correlation coefficient of .55 with a 0.001% chance that r = 0. 

![Expected Return Yards by Number of "Close" Players](https://www.kaggleusercontent.com/kf/84681128/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..zyKEhwiTfLeNE9GQJZslOA.dUgJdjPXGB7BTkjou6cm4RVmBhYZiM69H5YF1JksNMWlciiyXULtQENX2kTXBfJMOmcMwtEQPDu2AkcF6mR7_MT0aRECFNLsNdnOH_qvhFrJlxAAtEfQOM8UiyEVuNuh4AUXPR24f2-pSHxqxW9xC4WPUeFDtNNkmowMHmJ32L6WDJK6K-FZYfL0Hs0JTtvL5CWYqnZPbkgfunmq7jc-MxR3E9whG-m9OhkIO6rm9xv-jz-WBk5uQdqSU1o3Y8jA1YfQA84p31Wnggw_QGv06mHQ9kYE2JHiuPZ_rgoiO_94wR3cTdO-aHlPCy1RNRdX-cNv3UwtluGQmJZUp0Bf5teAJEs5Rjn28nGue0tWo0fXrVxsrsI9jihXzSOE_ciCK-DcDqD7cQf9ZAR_FrhhBCePLJWbB9SQRjiJCzoLe39xXFBIcYbxOWP_iLE-6qKqQRgDMi7IzDEdV2AKt6eNUiBGvmhy953ilXAAGJOnx1EFv1EnXZlUAqqEytg2jMXILhU7HPmIDBu86rC3fGAymZR8zIwm_yPKAZhzJSdzMQPlvjF3vJAZfGAMZCKcRu3gs8pUTipcoNKk5BDuIdmPlsnErmYImaJBuM854E2dVPj2zFtNVopZrP5000_88vI9GUj7O37FqqCTin-bUnN9jFYzJF3qgQNDTg7y6fj32uw.NBDvxZ0j3sCtcFPt7iafRw/__results___files/__results___28_1.png)

# Visualize Data

Visualization templates from: https://gist.github.com/Deryck97/dff8d33e9f841568201a2a0d5519ac5e

## Team Rankings: Punt Coverage

- First we will graph teams based on the average number of players "close" to the punt returner on punt coverages
- In other words, every time team X punts, about how many of their special teams players will be within 10 feet of the returner at the moment the returner catches the ball?
- Based on the graph, we expect the Colts and Falcons in particular to have good punt coverage (thus having the least amount of avg. return yards) while the Ravens and Rams are expected to have poor punt coverage (the highest amount of avg. return yards). 

![Average Number of "Close" Players Per Punt Coverage](https://www.kaggleusercontent.com/kf/84681128/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..zyKEhwiTfLeNE9GQJZslOA.dUgJdjPXGB7BTkjou6cm4RVmBhYZiM69H5YF1JksNMWlciiyXULtQENX2kTXBfJMOmcMwtEQPDu2AkcF6mR7_MT0aRECFNLsNdnOH_qvhFrJlxAAtEfQOM8UiyEVuNuh4AUXPR24f2-pSHxqxW9xC4WPUeFDtNNkmowMHmJ32L6WDJK6K-FZYfL0Hs0JTtvL5CWYqnZPbkgfunmq7jc-MxR3E9whG-m9OhkIO6rm9xv-jz-WBk5uQdqSU1o3Y8jA1YfQA84p31Wnggw_QGv06mHQ9kYE2JHiuPZ_rgoiO_94wR3cTdO-aHlPCy1RNRdX-cNv3UwtluGQmJZUp0Bf5teAJEs5Rjn28nGue0tWo0fXrVxsrsI9jihXzSOE_ciCK-DcDqD7cQf9ZAR_FrhhBCePLJWbB9SQRjiJCzoLe39xXFBIcYbxOWP_iLE-6qKqQRgDMi7IzDEdV2AKt6eNUiBGvmhy953ilXAAGJOnx1EFv1EnXZlUAqqEytg2jMXILhU7HPmIDBu86rC3fGAymZR8zIwm_yPKAZhzJSdzMQPlvjF3vJAZfGAMZCKcRu3gs8pUTipcoNKk5BDuIdmPlsnErmYImaJBuM854E2dVPj2zFtNVopZrP5000_88vI9GUj7O37FqqCTin-bUnN9jFYzJF3qgQNDTg7y6fj32uw.NBDvxZ0j3sCtcFPt7iafRw/__results___files/__results___34_0.png)

In the below scatterplot, we see that while the Colts do have the lowest average punt return, teams like the Broncos and Browns do not have the predicted effect of lowering return yardage through a higher number of "close" players.

![Average Opponent Return Yards vs Average Number of "Close" Players](https://www.kaggleusercontent.com/kf/84681128/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..zyKEhwiTfLeNE9GQJZslOA.dUgJdjPXGB7BTkjou6cm4RVmBhYZiM69H5YF1JksNMWlciiyXULtQENX2kTXBfJMOmcMwtEQPDu2AkcF6mR7_MT0aRECFNLsNdnOH_qvhFrJlxAAtEfQOM8UiyEVuNuh4AUXPR24f2-pSHxqxW9xC4WPUeFDtNNkmowMHmJ32L6WDJK6K-FZYfL0Hs0JTtvL5CWYqnZPbkgfunmq7jc-MxR3E9whG-m9OhkIO6rm9xv-jz-WBk5uQdqSU1o3Y8jA1YfQA84p31Wnggw_QGv06mHQ9kYE2JHiuPZ_rgoiO_94wR3cTdO-aHlPCy1RNRdX-cNv3UwtluGQmJZUp0Bf5teAJEs5Rjn28nGue0tWo0fXrVxsrsI9jihXzSOE_ciCK-DcDqD7cQf9ZAR_FrhhBCePLJWbB9SQRjiJCzoLe39xXFBIcYbxOWP_iLE-6qKqQRgDMi7IzDEdV2AKt6eNUiBGvmhy953ilXAAGJOnx1EFv1EnXZlUAqqEytg2jMXILhU7HPmIDBu86rC3fGAymZR8zIwm_yPKAZhzJSdzMQPlvjF3vJAZfGAMZCKcRu3gs8pUTipcoNKk5BDuIdmPlsnErmYImaJBuM854E2dVPj2zFtNVopZrP5000_88vI9GUj7O37FqqCTin-bUnN9jFYzJF3qgQNDTg7y6fj32uw.NBDvxZ0j3sCtcFPt7iafRw/__results___files/__results___36_0.png)

Here we analyze the metrics with the highest correlations found in the heatmap earlier:

![Percentage of Plays with 1+ "Close" Players versus Percentage of Plays with Opponent Returns of <5 Yards](https://www.kaggleusercontent.com/kf/84681128/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..zyKEhwiTfLeNE9GQJZslOA.dUgJdjPXGB7BTkjou6cm4RVmBhYZiM69H5YF1JksNMWlciiyXULtQENX2kTXBfJMOmcMwtEQPDu2AkcF6mR7_MT0aRECFNLsNdnOH_qvhFrJlxAAtEfQOM8UiyEVuNuh4AUXPR24f2-pSHxqxW9xC4WPUeFDtNNkmowMHmJ32L6WDJK6K-FZYfL0Hs0JTtvL5CWYqnZPbkgfunmq7jc-MxR3E9whG-m9OhkIO6rm9xv-jz-WBk5uQdqSU1o3Y8jA1YfQA84p31Wnggw_QGv06mHQ9kYE2JHiuPZ_rgoiO_94wR3cTdO-aHlPCy1RNRdX-cNv3UwtluGQmJZUp0Bf5teAJEs5Rjn28nGue0tWo0fXrVxsrsI9jihXzSOE_ciCK-DcDqD7cQf9ZAR_FrhhBCePLJWbB9SQRjiJCzoLe39xXFBIcYbxOWP_iLE-6qKqQRgDMi7IzDEdV2AKt6eNUiBGvmhy953ilXAAGJOnx1EFv1EnXZlUAqqEytg2jMXILhU7HPmIDBu86rC3fGAymZR8zIwm_yPKAZhzJSdzMQPlvjF3vJAZfGAMZCKcRu3gs8pUTipcoNKk5BDuIdmPlsnErmYImaJBuM854E2dVPj2zFtNVopZrP5000_88vI9GUj7O37FqqCTin-bUnN9jFYzJF3qgQNDTg7y6fj32uw.NBDvxZ0j3sCtcFPt7iafRw/__results___files/__results___38_0.png)

## Team Rankings: Punt Return Coverage

The idea here is the same - as there are more "close" opponent players to the returner, there should be lower return yardage. Or, as there are less "close" opposing players to the returner, the returner will tend to gain more yardage. Based off of the graph, we expect the 49ers and Giants to have the least punt return yardage and the Panthers and Falcons to have the highest return yardage.

![Average Number Of "Close" Opposing Players Per Punt Return](https://www.kaggleusercontent.com/kf/84681128/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..zyKEhwiTfLeNE9GQJZslOA.dUgJdjPXGB7BTkjou6cm4RVmBhYZiM69H5YF1JksNMWlciiyXULtQENX2kTXBfJMOmcMwtEQPDu2AkcF6mR7_MT0aRECFNLsNdnOH_qvhFrJlxAAtEfQOM8UiyEVuNuh4AUXPR24f2-pSHxqxW9xC4WPUeFDtNNkmowMHmJ32L6WDJK6K-FZYfL0Hs0JTtvL5CWYqnZPbkgfunmq7jc-MxR3E9whG-m9OhkIO6rm9xv-jz-WBk5uQdqSU1o3Y8jA1YfQA84p31Wnggw_QGv06mHQ9kYE2JHiuPZ_rgoiO_94wR3cTdO-aHlPCy1RNRdX-cNv3UwtluGQmJZUp0Bf5teAJEs5Rjn28nGue0tWo0fXrVxsrsI9jihXzSOE_ciCK-DcDqD7cQf9ZAR_FrhhBCePLJWbB9SQRjiJCzoLe39xXFBIcYbxOWP_iLE-6qKqQRgDMi7IzDEdV2AKt6eNUiBGvmhy953ilXAAGJOnx1EFv1EnXZlUAqqEytg2jMXILhU7HPmIDBu86rC3fGAymZR8zIwm_yPKAZhzJSdzMQPlvjF3vJAZfGAMZCKcRu3gs8pUTipcoNKk5BDuIdmPlsnErmYImaJBuM854E2dVPj2zFtNVopZrP5000_88vI9GUj7O37FqqCTin-bUnN9jFYzJF3qgQNDTg7y6fj32uw.NBDvxZ0j3sCtcFPt7iafRw/__results___files/__results___40_0.png)

While the 49ers and Giants do have low average return yards, the Falcons and Panthers also have a below-average average return yards. There seems to be little correlation that the average return yards is affected by the average number of opposing "close" players.

![Average Return Yards vs Average Number of "Close" Opposing Players](https://www.kaggleusercontent.com/kf/84681128/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..zyKEhwiTfLeNE9GQJZslOA.dUgJdjPXGB7BTkjou6cm4RVmBhYZiM69H5YF1JksNMWlciiyXULtQENX2kTXBfJMOmcMwtEQPDu2AkcF6mR7_MT0aRECFNLsNdnOH_qvhFrJlxAAtEfQOM8UiyEVuNuh4AUXPR24f2-pSHxqxW9xC4WPUeFDtNNkmowMHmJ32L6WDJK6K-FZYfL0Hs0JTtvL5CWYqnZPbkgfunmq7jc-MxR3E9whG-m9OhkIO6rm9xv-jz-WBk5uQdqSU1o3Y8jA1YfQA84p31Wnggw_QGv06mHQ9kYE2JHiuPZ_rgoiO_94wR3cTdO-aHlPCy1RNRdX-cNv3UwtluGQmJZUp0Bf5teAJEs5Rjn28nGue0tWo0fXrVxsrsI9jihXzSOE_ciCK-DcDqD7cQf9ZAR_FrhhBCePLJWbB9SQRjiJCzoLe39xXFBIcYbxOWP_iLE-6qKqQRgDMi7IzDEdV2AKt6eNUiBGvmhy953ilXAAGJOnx1EFv1EnXZlUAqqEytg2jMXILhU7HPmIDBu86rC3fGAymZR8zIwm_yPKAZhzJSdzMQPlvjF3vJAZfGAMZCKcRu3gs8pUTipcoNKk5BDuIdmPlsnErmYImaJBuM854E2dVPj2zFtNVopZrP5000_88vI9GUj7O37FqqCTin-bUnN9jFYzJF3qgQNDTg7y6fj32uw.NBDvxZ0j3sCtcFPt7iafRw/__results___files/__results___42_0.png)

## Individual Gunner Rankings

Here we rank the top 20 gunners by the percentage of plays where they were "close" to the returner on punt receive (minimum 10 plays).

![Average Opposing Return Yards vs Percentage of "Close" Plays Per Gunner](https://www.kaggleusercontent.com/kf/84681128/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..zyKEhwiTfLeNE9GQJZslOA.dUgJdjPXGB7BTkjou6cm4RVmBhYZiM69H5YF1JksNMWlciiyXULtQENX2kTXBfJMOmcMwtEQPDu2AkcF6mR7_MT0aRECFNLsNdnOH_qvhFrJlxAAtEfQOM8UiyEVuNuh4AUXPR24f2-pSHxqxW9xC4WPUeFDtNNkmowMHmJ32L6WDJK6K-FZYfL0Hs0JTtvL5CWYqnZPbkgfunmq7jc-MxR3E9whG-m9OhkIO6rm9xv-jz-WBk5uQdqSU1o3Y8jA1YfQA84p31Wnggw_QGv06mHQ9kYE2JHiuPZ_rgoiO_94wR3cTdO-aHlPCy1RNRdX-cNv3UwtluGQmJZUp0Bf5teAJEs5Rjn28nGue0tWo0fXrVxsrsI9jihXzSOE_ciCK-DcDqD7cQf9ZAR_FrhhBCePLJWbB9SQRjiJCzoLe39xXFBIcYbxOWP_iLE-6qKqQRgDMi7IzDEdV2AKt6eNUiBGvmhy953ilXAAGJOnx1EFv1EnXZlUAqqEytg2jMXILhU7HPmIDBu86rC3fGAymZR8zIwm_yPKAZhzJSdzMQPlvjF3vJAZfGAMZCKcRu3gs8pUTipcoNKk5BDuIdmPlsnErmYImaJBuM854E2dVPj2zFtNVopZrP5000_88vI9GUj7O37FqqCTin-bUnN9jFYzJF3qgQNDTg7y6fj32uw.NBDvxZ0j3sCtcFPt7iafRw/__results___files/__results___46_0.png)

![Gunners Who Were Most Often "Close" To The Returner]
(https://www.kaggleusercontent.com/kf/84681128/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..zyKEhwiTfLeNE9GQJZslOA.dUgJdjPXGB7BTkjou6cm4RVmBhYZiM69H5YF1JksNMWlciiyXULtQENX2kTXBfJMOmcMwtEQPDu2AkcF6mR7_MT0aRECFNLsNdnOH_qvhFrJlxAAtEfQOM8UiyEVuNuh4AUXPR24f2-pSHxqxW9xC4WPUeFDtNNkmowMHmJ32L6WDJK6K-FZYfL0Hs0JTtvL5CWYqnZPbkgfunmq7jc-MxR3E9whG-m9OhkIO6rm9xv-jz-WBk5uQdqSU1o3Y8jA1YfQA84p31Wnggw_QGv06mHQ9kYE2JHiuPZ_rgoiO_94wR3cTdO-aHlPCy1RNRdX-cNv3UwtluGQmJZUp0Bf5teAJEs5Rjn28nGue0tWo0fXrVxsrsI9jihXzSOE_ciCK-DcDqD7cQf9ZAR_FrhhBCePLJWbB9SQRjiJCzoLe39xXFBIcYbxOWP_iLE-6qKqQRgDMi7IzDEdV2AKt6eNUiBGvmhy953ilXAAGJOnx1EFv1EnXZlUAqqEytg2jMXILhU7HPmIDBu86rC3fGAymZR8zIwm_yPKAZhzJSdzMQPlvjF3vJAZfGAMZCKcRu3gs8pUTipcoNKk5BDuIdmPlsnErmYImaJBuM854E2dVPj2zFtNVopZrP5000_88vI9GUj7O37FqqCTin-bUnN9jFYzJF3qgQNDTg7y6fj32uw.NBDvxZ0j3sCtcFPt7iafRw/__results___files/__results___48_0.png)

![Non-Gunners Who Were Most Often "Close" To The Returner]
(https://www.kaggleusercontent.com/kf/84681128/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..zyKEhwiTfLeNE9GQJZslOA.dUgJdjPXGB7BTkjou6cm4RVmBhYZiM69H5YF1JksNMWlciiyXULtQENX2kTXBfJMOmcMwtEQPDu2AkcF6mR7_MT0aRECFNLsNdnOH_qvhFrJlxAAtEfQOM8UiyEVuNuh4AUXPR24f2-pSHxqxW9xC4WPUeFDtNNkmowMHmJ32L6WDJK6K-FZYfL0Hs0JTtvL5CWYqnZPbkgfunmq7jc-MxR3E9whG-m9OhkIO6rm9xv-jz-WBk5uQdqSU1o3Y8jA1YfQA84p31Wnggw_QGv06mHQ9kYE2JHiuPZ_rgoiO_94wR3cTdO-aHlPCy1RNRdX-cNv3UwtluGQmJZUp0Bf5teAJEs5Rjn28nGue0tWo0fXrVxsrsI9jihXzSOE_ciCK-DcDqD7cQf9ZAR_FrhhBCePLJWbB9SQRjiJCzoLe39xXFBIcYbxOWP_iLE-6qKqQRgDMi7IzDEdV2AKt6eNUiBGvmhy953ilXAAGJOnx1EFv1EnXZlUAqqEytg2jMXILhU7HPmIDBu86rC3fGAymZR8zIwm_yPKAZhzJSdzMQPlvjF3vJAZfGAMZCKcRu3gs8pUTipcoNKk5BDuIdmPlsnErmYImaJBuM854E2dVPj2zFtNVopZrP5000_88vI9GUj7O37FqqCTin-bUnN9jFYzJF3qgQNDTg7y6fj32uw.NBDvxZ0j3sCtcFPt7iafRw/__results___files/__results___50_0.png)

# 2022 Pro League Analysis Between Bot and Mid
## Introduction
The following is an analysis of the 2022 Professional League of Legends match data. Specifically, it is looking at the difference in performance,
in order to see if Bot lane, other wise known as AD Carry, truly does carry the game over Midlane, a role which traditionally receives a large part of attention, 
and resources, in no small part due to the famous players that occupy the role. This analysis looking at Kills, Deaths, and Assists, and an adjusted_deaths column to calculate 
KDA as well as total Team Kills, proportion of Team Kills, cspm, or creep score per minute, roughly indactive of gold economy, and dpm, damage per minute into order to provide 
a data friven approach towards what role truly ends up "Carrying" games. This condenses our dataset into 49800 columns of mid or bot players, and 10 columns of the afformentioned 
statistics, and the following analysis looks into which role ends up returning the most results, especially when resources are invested. 

## Cleaning and EDA

### Cleaning
The first step to look at the difference in performance in role, was to condense our CSV to only look at the statistics in those roles. This removes 
jungle, support, and top  as you would expect but also two team roles, representing an aggregation of total stats and team wide objectives such as towers or drakes.
The next step involves isolating the columns we are interested in: position, kills, deaths, assists, teamkills, dpm, and csmp. We also need to create a new column
in order to calculate KDA (Kills, Deaths, Assists). My calculation of KDA involves treating a kill as worth one kill, and an assist as worth half. However, some players 
have deathless games, leading to a division by zero error, which is corrected by the adjusted deaths columns, where any zero death game is treated as a one for the 
purposes of calculating KDA. A similar process is undertaken for the proprotion of kills on the team, where in most cases it is cacluatled as kills divided by team kills, 
however the case wherethe team overall doesn't get a kill, that player's proportion is also treated as a zero. Below is a small sample of what the cleaned DataFrame looks like.

|    | position   |   kills |   deaths |   assists |   teamkills |     dpm |    cspm |   adjusted_deaths |   kda |   proportion_kills |
|---:|:-----------|--------:|---------:|----------:|------------:|--------:|--------:|------------------:|------:|-------------------:|
|  2 | mid        |       2 |        2 |         3 |           9 | 499.405 |  6.7601 |                 2 |  1.75 |           0.222222 |
|  3 | bot        |       2 |        4 |         2 |           9 | 389.002 |  7.9159 |                 4 |  0.75 |           0.222222 |
|  7 | mid        |       6 |        3 |        12 |          19 | 724.693 |  7.5657 |                 3 |  4    |           0.315789 |
|  8 | bot        |       8 |        2 |        10 |          19 | 934.746 | 11.1734 |                 2 |  6.5  |           0.421053 |
| 14 | mid        |       2 |        4 |         0 |           3 | 655.118 | 10.5298 |                 4 |  0.5  |           0.666667 |   0.666667 |

### Exploration of Role Statistics
One area of interest is the comparison between the KDA between the two roles. A probability histogram between the two roles shows that the ADC is more likely to have a higher
KDA, as demonstrated by the blue bars representing Mid being taller when KDA is lower, and smaller when KDA is higher, meaning ADC's are more likely to have higher KDAs.

<iframe src="assets/kda.html" width=800 height=600 frameBorder=0></iframe>

Another area we can see this is in the Damage Per Minute Distribution. Mid and ADC are roughly around the same towards the lower end of damage per minute, however ADC players
have higher DPM stats than Midplayers once we cross the 700 mark. 

<iframe src="assets/dpm.html" width=800 height=600 frameBorder=0></iframe>

We can also see this trend when comparing two variables as well. An analysis of the Damage Per Minute in proprotion to team kills shows a trend of higher DPM from the 
ADC resulting in a greater proportion of the total teamwide kills, meaning that when dealing more damage, ADC players also contribute to a greater portion of their team's
total kill count.  

<iframe src="assets/dpmprop.html" width=800 height=600 frameBorder=0></iframe>

Here is a cleaner version of the data showing only the trendlines instead of all the scatterplot points as well. 

<iframe src="assets/cleaned_dpm.html" width=800 height=600 frameBorder=0></iframe>

This also gets interesting when comparing CS Per Minute to the proportion of Teamwide Kills. Towards the lower end of CS, Midlane provides a greater proportion of kills however as
CS Per minute rises, the ADC begins to pull ahead, indicating that skilled farming allows the ADC to rapidly contribute to team fights, as the gold provided seems to provide
a greater impact to the overall team than the time spent missing when farming. 

<iframe src="assets/csprop.html" width=800 height=600 frameBorder=0></iframe>

Again, the cleaned trendline data from the scatter plots. 

<iframe src="assets/cleaned_cspm.html" width=800 height=600 frameBorder=0></iframe>

### Aggregate Statistics
This trend also continues when looking at the aggregate statistics. Here, teamwide kills are dropped as it is the same for both mid and ADC as there is a mid and ADC player on every
team. Looking at both Mean and Median shows that the previous charts where not a fluke, as ADC averages greater kills, DPM< CSPM, KDA, and Proportion of Teamwide kills. One area
where Mid outclasses ADC is in assists, however this can be explianed by the greater kill volume as a player cannot obtain a kill and assist from the same elimination. 

### Mean

| position   |   kills |   deaths |   assists |   teamkills |     dpm |    cspm |   adjusted_deaths |     kda |   proportion_kills |
|:-----------|--------:|---------:|----------:|------------:|--------:|--------:|------------------:|--------:|-------------------:|
| bot        | 4.2588  |  2.54763 |   5.37153 |     14.4853 | 560.685 | 8.80589 |           2.68394 | 4.14882 |           0.284844 |
| mid        | 3.50145 |  2.65614 |   5.89317 |     14.4853 | 545.908 | 8.28479 |           2.78008 | 3.7535  |           0.240355 |
### Median

 position   |   kills |   deaths |   assists |   teamkills |     dpm |   cspm |   adjusted_deaths |   kda |   proportion_kills |
|:-----------|--------:|---------:|----------:|------------:|--------:|-------:|------------------:|------:|-------------------:|
| bot        |       4 |        2 |         5 |          14 | 531.121 | 8.9888 |                 2 |  2.75 |           0.277778 |
| mid        |       3 |        2 |         5 |          14 | 518.949 | 8.2966 |                 2 |  2.5  |           0.227273 |


Overall, it seems that despite their stardom of Midlane, ADCs perform better than the midlane when they're given a chance to shine. 


## Assessment of Missingness 
As an aside, the CSV provided contains lots of very interesting data, however there are several that seem to be Not Missing at Random, or NMAR. One example could be the Double Kill statistic
The reasoning being is that whenever a column is labelled as partially complete, it's missing data about several columns such as the double/triple/quardra/penta kill statistics and the gold/xp difference at 10 minutes. These pieces of data are consistently missing in partial incomplete games, leading to the suggestion that these values are "Not Missing at Random" One piece of additional data that could perhaps explain the missingness is to see where the data is collected, for example if certain tournament organizers or leagues not providing the same information, or the lack of recorded results due to something about the game going 
wrong, like the data collection malfunction due to something in the game, or the game being incomplete. These all serve as suggestions for additional information that could explain why 
the data in these columns are missing. 

Looking at this missingness further, we can see that the missingness of for example, double kills does depend on kills but does not seem to depend on the overall winrate. The first
step of this process involves finding the absolute difference in means between the columns of kills and results when missing doublekill information as compared to without. We then need
to see how rare such a result truly is, leading to a permutation test. This invovles shuffling around the columns that are missing the double kill statistic in order to see if the 
difference in means. We can see that in the case of Mean Kills, the observed test statistic of absolute difference being -0.25 is extremely rare, and unlikely to have come from random chance while in terms of a randomized winrate, of the observed test statistc of the variation in winrate between missing and non missing being 0, the result is extremely common. This suggests that mean kills perhaps is influenced by the lack of double kills, perhaps because the lack of information on killstreaks means that mean kills skew lower while winrate remains untouched as it seems to be collected even if the extra information is not. Or perhaps there is something deeper at play, relating to the reasoning behind why the data was missing in the first place. Conversely, Win Rate seems to always be collected, so the missingness does not seem to depend on the Results column representing wins/losses. 

<iframe src="assets/kills.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/wins.html" width=800 height=600 frameBorder=0></iframe>



## Hypothesis Testing
Finally, the test looking at the results of the question asked in the beginning. Adressing wheter or not ADC performs better than Mid Lane. One question that could be raised is whether or
not this difference could stem from random chance. In order to help answer this, I ran a permuatation test. Our null hypothsis is that these two roles perform the same, or that mid lane 
performs better, while our alternative hypothesis suggests that ADC performs better. I analyzed this looking at the difference between in mean KDA for Bot - Mid, and used a significance value of 
0.05. Our observed test statistic was  0.395, or 0.4 when rounded. After conducting a shuffle 1000 times, the permutation test, returned a result of 0.0, leading to me rejecting the null 
at the 0.05 significance level. This random set of permutations suggests that the difference in KDA observed between Bot and Mid lane was extremely unlikely to come from random chance, as none of the results in the 1000 random shuffles were close to our observed statistic.  The graph below serves as a visual demonstration of this in action.  Overall, this seems to suggest that the better performance from
ADC we foundearlier is unlikely to have come from random chance, lending credence to the idea that AD Carry really does seem to live up to it's name. 

<iframe src="assets/hypoth.html" width=800 height=600 frameBorder=0></iframe>

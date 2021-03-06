---
layout: post
title: Home Court Advantage
output: html_document
fig_width: 6
fig_weight: 3
---

by: *Jesse Zlotoff*

Conventional sports wisdom holds that a team is more likely to win when playing at home.  I examine this idea using data from the Wizards' 2016-2017 regular season.[^1]  The goal is to determine *whether* playing at home made the Wizards more likely to win, not *why* playing at home might (or might not) affect their performance.  

There are 82 games in a regular NBA season with half played at home.  The table below shows that the Wizards won almost three out of four games at home (73%) but won less than half (46%) on the road.

```{r, echo=FALSE}
suppressPackageStartupMessages(library(kableExtra))

load("analysis_results.rdata")
wiz_summary_k
```

***
<p><b><font color="green">Optional Math Break <a href="#Block1" data-toggle="collapse">Show/Hide</a></font></b></p>
<div id="Block1" class="collapse"> 
I run a chi-squared test on wins at home vs away and find that the difference is significant at the 95% confidence level.
````{r, echo=FALSE}
print(t)
paste("p-value",round(chi2$p.value, digits=3))
````
</div>
***

Not all games are equal, however.  The likelihood of winning depends on the opponent.  I use each team's percent of games won over the season as a simple metric for opponent strength.  Taking this into account, the Wizards still won more at home than away.[^2]

```{r, echo=FALSE}
wiz_bars
```

I use a simple regression model to measure how much of a boost the Wizards get from playing at home.

***
<p><b><font color="green">Optional Math Break <a href="#Block2" data-toggle="collapse">Show/Hide</a></font></b></p>
<div id="Block2" class="collapse"> 
Here are the results of the regression.  I also controlled for game number to measure potential changes over the course of the season but that variable is not significant.

````{r, echo=FALSE}
regr_output_all_k
````
</div>
***

````{r, echo=FALSE}
wiz_trend
````

The wizards are more likely to win against any team if they play at home.  But the difference is larger for mid-level teams.  This makes intuitive sense: the Wizards should beat poor teams, and struggle against strong teams, no matter where they play.

The table below, with the predicted chance of winning against each team at home or away, follows the same pattern. 

````{r, echo=FALSE}
wiz_predicted_k %>%
    kable_styling(full_width=F, position="left")
````

***
#### Further analysis
This model assumes that the "better" team should always win.  Further analysis could add the minutes played by a team's starters, or days since the last game, to account for the varying skill of a team on a given night.

Another analysis could extend the current model across every team in the 2016-2017 season, and then across seasons, to see if home court advantage exists outside of this particular example.

***
#### Acknowledgements
Special thanks to Gus Warren

[^1]: Data from [Basketball Reference](https://www.basketball-reference.com/teams/WAS/2017_games.html)
[^2]: Bins with only one game excluded from the chart

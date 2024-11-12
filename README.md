# Tail Me - Team 46 ML Midterm

## Problem Definition

Unlike professional sports, college football is a collection of nearly 150 teams - many of whom don't receive national attention. For many of these programs, local media outlets dominate their news coverage. In the context of sports betting, lines and odds are set by outsiders on a national scale. Thus, much of the underlying information about a team may be captured by local media and kept a "secret" to these oddsmakers. We aim to explore how capturing local news sentiments can help us provide better context unknown at the national level and more accurately predict the outcome of a college football team's season. Others have tried approaches at the individual level (i.e., social media) to predict outcomes for sports and similar topics; however, the concept of looking one level at local news is relatively unexplored [CITE: IEEE Explore](https://ieeexplore.ieee.org/document/8530517) and [MDPI](https://www.mdpi.com/2624-831X/1/2/14).

To do this, we examine local news articles about division 1 college football teams before a season and compare it against national odds predicting how well that team will do that year. We web scrape local articles and compare them against a dataset of odds for the 2023 season available at [Sports Odds History](https://www.sportsoddshistory.com/cfb-win/?y=2023&sa=cfb&t=win&o=t).

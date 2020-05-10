
-----

output: github\_document

-----

<!-- README.md is generated from README.Rmd. Please edit that file -->

# rockr <img src='hex/rockr_hex.png' align="right" height="150" />

<!-- badges: start -->

<!-- badges: end -->

The goal of rockr is to:

Given a series of Twitter polls on best album of the year, with
sequential polls for each year, where some bands are reoccuring response
options across polls

1.  Munge data into an analysis-ready format

2.  Produce multiple scalings and subsets of data to permit different
    ways of conceptualising poll responses

3.  Render animated bar charts to visualise the cumulative and aggregate
    sentiment across the series of polls

\[(Attempts at) British spelling are in honour of Nick Moberly (Exeter,
UK), whose @nickmoberly Twitter polls were the motivation for and
contributing source data used in the illustrative example.\]

<p align="center">

<img src=https://media.giphy.com/media/cD00Ukp6FfXuU/giphy.gif>

</p>

## Installation

Ideally rockr would be a package in itself with generalizable functions
and integrated dependencies. However, at present it is simply a code and
data repository with script tailored to one particular dataset.

The analyses described herein requires the installation and loading of
the following R packages:

`tidyverse`, `ggplot2`, `gganimate`, `png`

All R code required for this project can be found in [script/Animated
Bar
Chart.Rmd](https://github.com/MarkLaVenia/rockr/blob/master/script/Animated%20Bar%20Chart.Rmd)

## Analytic premises and assumptions (…however questionable)

  - A band’s status within the ranking of best bands is a function of
      - the number of albums the band has that qualify as one of the
        best albums of the year and
      - the proportion of people who vote the band’s albums as the best
        album of the year.
  - Poll percentages weighted equally across years smooths over
    variation in response rate across polls;
      - however, sum of votes may (in part) be an indicator of
        enthusiasm for a given a band or album– and therefore may also
        be a valid metric for ranking bands.
      - That said, early polls averaged fewer responses than later
        polls; therefore, it appears reach of the poll increased over
        time–likely giving an upward bias for bands in later polls when
        using vote sums.
  - Constraining the date to only the final poll for each year avoids
    the problem of needing to account for albums that appeared on both
    qualifying and final polls;
      - however, on the premise that the magnitide of voter response is
        an indicator of enthusiasm for a given band or album, summing
        across bonus, qualifying final polls polls–constituting the
        total sum of votes cast for a band or album given the
        opportunity to vote for that band or album–may yield some
        insight.

## Data preparation

Raw data for this project were hand entered from Twitter, structured in
a long format.

### Precision

Twitter polls provide data on the total number of votes and the
percentage of votes per response option rounded to the first decimal
place, Accordingly, using the `mutate()` command we calculated

  - the number of votes per response option (which is a metric of
    interest in itself) and
  - the percentage of votes per response option without truncation due
    to rounding.

### Selection

We then reduce the dataframe to the observations of interest.

  - For all analyses, we drop observations for polls coded as invalid.
      - In the example data, one poll was conducted as an alternate
        final.
  - For two analyses we retain observation for only final polls
    (excluding data for bonus and qualifying polls);
      - whereas, for a third analysis we retained data for all valid
        polls (bonus, qualifying, and final polls).

### Aggregation

To remedy instances where a given band appeared more than once in a
given year

  - we use the `group_by()` and `summarise_at()` commands to sum
    percentages or vote counts for each band per year.
  - This occured in the 1970 final poll, where Black Sabbath had two
    albums that year;
      - other scenarios for this occur when analyzing bonus, qualifying,
        and final polls jointly.

### Structure

Ultimately we want a file in a long format, with each band having a row
for every year in the data set regardless of whether the band has poll
data for that year. There is probably a more efficiet what of doing
this; but short of figuring that out,

  - I first used the `pivot_wider()` command, then the `pivot_longer()`
    command to accomplish this.
  - A more efficient approach would evaluate which years were unobserved
    for given bands, then insert rows for those missing observations.
    (Suggestions on improvements on this procedure are welcome).

### Computation

To calculate rolling averages and sums, we use

  - the `mutate(cummmean())` command with the `poll_percent` variable
    and
  - the `mutate(cummsum())` command with the `album_votes` variable.

### Format

The final step before plotting the data is to format by calling

  - the `group_by()` and `mutate(rank())` commands to rank order the
    bands with each year and
  - the `group_by()` and `filter()` commands to constrain the data to
    the top 10 ranked bands for any given year.

## Rendering animated bar charts

### Static and animated plots

The first step to making an animated bar chart is to make static bar
charts using the `ggplot()` command.

  - Dissatisfied with the default colors, I create a custom array of
    colors and called it using the `scale_colour_manual()` and
    `scale_fill_manual()` commands.
  - Using the `unique()` command we can generate the list of bands in
    the plot for which a color is needed.

Then we use the `transition_states()` command to stitch together the
individual static plots.

  - And the final step is rendering the animated plots usng the
    `animate()` command.

### Best album cumulative percentage of votes aggregated by band, based on final polls

This plot uses the `poll_percent` variable as the plotted metric.

<p align="center">

<img src="plots/album_poll_final_percentage.gif" alt="reviewer">

</p>

### Best album cumulative votes aggregated by band, based on final polls

This plot uses the `album_votes` variable as the plotted metric.

<p align="center">

<img src="plots/album_poll_final_sum.gif" alt="reviewer">

</p>

### Best album cumulative votes aggregated by band, based on all polls

This plot uses the `album_votes` variable as the plotted metric.

<p align="center">

<img src="plots/album_poll_all_sum.gif" alt="reviewer">

</p>

### Source code and guidance

Credit to AbdulMajedRaja RS
<https://towardsdatascience.com/create-animated-bar-charts-using-r-31d09e5841da>
for source code and guidance referenced for these animated bar charts.
See also Stack Overflow
<https://stackoverflow.com/questions/53162821/animated-sorted-bar-chart-with-bars-overtaking-each-other>
for guidance and discussion.

<p align="center">

<img src=https://media.giphy.com/media/xT9DPiSrihyxZnarbG/giphy.gif>


-----

output: github\_document

-----

<!-- README.md is generated from README.Rmd. Please edit that file -->

![](https://img.shields.io/badge/cool-useless-green.svg) [![Open Source
Love](https://badges.frapsoft.com/os/v1/open-source.png?v=103)](https://github.com/ellerbrock/open-source-badges/)
[![Ask Me Anything
\!](https://img.shields.io/badge/Ask%20me-anything-1abc9c.svg)](https://GitHub.com/Naereen/ama)

<a id="top of page"></a>

# rockr <img src='hex/rockr_hex.png' align="right" height="150" />

<!-- badges: start -->

<!-- badges: end -->

The (current) goal of `rockr` is:

Given a series of Twitter polls on best album of the year, with
sequential polls for each year, where some bands are reoccuring response
options across polls

1.  Munge data into an analysis-ready format

2.  Produce multiple scalings and subsets of data to permit different
    ways of conceptualising poll results

3.  Render animated bar charts to visualise the cumulative and aggregate
    response across the series of polls

<br>

> (Attempts at) British spelling are in honour of Nick Moberly (Exeter,
> UK), whose @nickmoberly Twitter polls were the motivation for and
> contributing data used in the illustrative example.

<br>

<p align="center">

<img src=https://media.giphy.com/media/cD00Ukp6FfXuU/giphy.gif>

</p>

## Packages, scripts, and data

Although `rockr` aspires to be a full blown package contributing
generalizable functions useful for a variety of applications, at present
it is simply a code and data repository with script tailored to one
particular dataset. See section, [Future development of
`rockr`](#Future%20development%20of%20rockr), for thoughts on what
`rockr` might be when it grows up.

The analyses described herein requires the installation and loading of
the following `R` packages:

`tidyverse`, `ggplot2`, `gganimate`, `png`

All `R` code required for this project can be found in [script/Animated
Bar
Chart.Rmd](https://github.com/MarkLaVenia/rockr/blob/master/script/Animated%20Bar%20Chart.Rmd),
with raw data found in
[data/raw-data/raw\_twitter\_poll\_data.csv](https://github.com/MarkLaVenia/rockr/tree/master/data/raw-data/raw_twitter_poll_data.csv)

## Analytic premises and assumptions (*…however questionable*)

  - A band’s status within the ranking of best bands is a function of
      - the number of albums a band has that qualify as one of the best
        albums of the year and
      - the proportion of people who vote a band’s albums as the best
        album of the year.
  - Poll percentages weighted equally across years smooths over
    variation in response rate across polls;
      - however, the sum of votes may (in part) be an indicator of
        enthusiasm for a given a band or album–and therefore may also be
        a valid metric for ranking bands.
      - That said, early polls averaged fewer responses than later
        polls; therefore, it appears reach of the polls increased over
        time–likely giving an upward bias for bands in later polls when
        using vote sums as the metric of analysis.
  - Constraining the date to only the final poll for each year avoids
    the problem of needing to account for albums that appeared on both
    qualifying and final polls;
      - however, on the premise that the magnitide of voter response is
        an indicator of enthusiasm for a given band or album, summing
        across bonus, qualifying, and final polls–constituting the total
        sum of votes cast for a band or album given the opportunity to
        vote for that band or album–may yield some insight.
  - Lastly, regarding these Twitter polls, it is worth noting that these
    are not scientifically derived samples–just Nick’s Twitter mates :)

## Data preparation

Raw data for this project were hand-entered from Twitter, structured in
a long-format entry log.

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
      - In the example data, one poll coded as invalid was conducted as
        an alternate final.
  - For two analyses we retain data for only final polls, excluding data
    for bonus and qualifying polls (see [Bar Chart 1](#Bar%20Chart%201)
    and [Bar Chart 2](#Bar%20Chart%202)); for a third analysis we
    retained data for all valid polls, inclusive of bonus, qualifying,
    and final polls (see [Bar Chart 3](#Bar%20Chart%203)).

### Aggregation

To remedy instances where a given band appeared more than once in a
given year

  - we use the `group_by()` and `summarise_at()` commands to sum
    percentages or vote counts for each band per year.
  - This scenario occured in the 1970 final poll, where *Black Sabbath*
    had two albums that year;
      - other scenarios for this occur when analyzing bonus, qualifying,
        and final polls jointly.

### Structure

Ultimately we want a file in a long format, with each band having a row
for every year in the data set regardless of whether the band had poll
data for that year. There is probably a more efficient what of doing
this; but short of figuring that out,

  - I first used the `pivot_wider()` command, followed by the
    `pivot_longer()` command to accomplish this.
  - A more efficient approach would evaluate which years were unobserved
    for given bands, then insert rows for those missing observations.
      - Suggestions on improved approaches to this are welcome.

### Computation

To calculate rolling averages and sums, we use

  - the `mutate(cummmean())` command with the `poll_percent` variable
    and
  - the `mutate(cummsum())` command with the `album_votes` variable.

### Format

The final step before plotting is to format the data for analysis by
calling

  - the `group_by()` and `mutate(rank())` commands to rank order the
    bands with each year and
  - the `group_by()` and `filter()` commands to constrain the data to
    the top ranked bands for any given year.
      - In this example we filter to the top 10 ranked bands.

## Rendering animated bar charts

### Static and animated plots

The first step to making an animated bar chart is to plot a series of
static bar charts using the `ggplot()` command.

  - Dissatisfied with the default colors, I create a custom array of
    colors and called it in using the `scale_colour_manual()` and
    `scale_fill_manual()` commands.
      - After all, *Black Sabbath* has to be *black* and *Deep Purple*
        has to be *purple*, *right*?
  - Using the `unique()` command we can generate the list of bands in
    the plot for which colors are needed.

Then we use the `transition_states()` command to stitch together the
individual static plots.

  - And the final step is rendering the animated plots usng the
    `animate(gifski_renderer())` command. <br><br>
    <a id="Bar Chart 1"></a>

#### Bar Chart 1. *Best album cumulative percentage of votes aggregated by band*

This plot uses a rolling average of the `poll_percent` variable as the
plotted metric, based on the *final* polls.

<p align="center">

<img src="plots/album_poll_final_percentage.gif" alt="reviewer">

</p>

<br><br><br> <a id="Bar Chart 2"></a>

#### Bar Chart 2. *Best album cumulative votes aggregated by band*

This plot uses a rolling sum of the `album_votes` variable as the
plotted metric, based on the *final* polls.

  - This and the following plot that uses vote sums as the plotted
    metric has the annoying quirk of occasionally having ties where bars
    overlap–making the band name difficult to read.
      - Suggested visualization remedies to the overlapping of bars are
        welcome.
        <p align="center">
        <img src="plots/album_poll_final_sum.gif" alt="reviewer">
        </p>
        <br><br><br> <a id="Bar Chart 3"></a>

#### Bar Chart 3. *Best album cumulative votes aggregated by band*

This plot uses uses a rolling sum of the `album_votes` variable as the
plotted metric, based on *all* polls.

<p align="center">

<img src="plots/album_poll_all_sum.gif" alt="reviewer">

</p>

<br>

-----

### Source code and guidance

> Credit to [AbdulMajedRaja
> RS](https://towardsdatascience.com/create-animated-bar-charts-using-r-31d09e5841da)
> for source code and guidance referenced for these animated bar charts.
> See also related [Stack Overflow
> posts](https://stackoverflow.com/questions/53162821/animated-sorted-bar-chart-with-bars-overtaking-each-other)
> for guidance and discussion.

<br> <a id="Future development of rockr"></a>

### Future development of `rockr`

1.  The most untuitive further development of `rockr` is to integrate
    web scraping into the workflow to more efficiently gather the data
    from the Twitter polls.

2.  We could also draw upon existing datasets, such as [JarbasAI’s Metal
    Dataset](https://github.com/OpenJarbas/metal_dataset) on GitHub: a
    vast curation of metal bands, songs, and lyrics sorted by sub-genre.
    Existing uses of this dataset unclude the [Metal
    Generator](https://ai-jarbas.gitbook.io/jarbasai/projects/metal-generator)
    / [pymetal](https://github.com/OpenJarbas/pymetal) `Python` package
    for generating new band names, song names, and lyrics.
    
      - One extension of this could be to create an `R` cousin of
        `pymetal`, to where `rmetal` is a sub-command in a more
        comprehensive `rockr`package.
      - Of course there are plenty of other great uses for [JarbasAI’s
        Metal Dataset](https://github.com/OpenJarbas/metal_dataset)
        worth exploring and possibly integrating into a `rockr` package.

3.  [Alberto Acerbi’s genre
    analysis](https://github.com/albertoacerbi/mxm_genres_analysis)
    constitutes an interesting sentiment analysis of lyrics, which
    infers the postive and negative emotional tone of music, by genre,
    and over time. Certainly other conceptual frameworks and dimension
    operationalizations could be applied to explore alternate
    interpretations of the data. Albert Acerbi makes use of the
    [musixmatch](https://www.musixmatch.com/) repository of song lyrics
    for his analysis, which could be drawn upon for replication and
    extension of this line of inquiry.
    
      - One variation on this analytic strategy includes taking a more
        holistic approach to categorizing positive and negative valence,
        such as keying by word phrases rather than individual words–or
        even clustering lyrics by song to allow for an evaluation of
        individual songs over the entire arc of their lyrics.
      - Further, additional data on the sonics, harmonic esthetics,
        tempo, dynamics, etc. of the instrumentation and vocals could be
        brought to bear to evaluate the auditory effect of songs
        holistically–placing the lyrics within the broader context of
        the song as a unit of analysis.
          - Of course, this would require additional data, of which I am
            not aware of as existing at present.

4.  The [Bound by Metal Interactive Metal Genres
    Graph](https://www.boundbymetal.com/en/common/metal-genres-graph)
    represents an excellent data visualization for up- and down-stream
    influences between sub-genres. This kind of network analysis can be
    useful for interogating the ontology of and relationships between
    sub-genres.
    
      - However, I would love greater transparency around the source
        data and decision rules. Also, I’d love to be able toggle the
        unit of anlysis to visualize the network connections by band–or
        even account for and visualize how bands may vary in style over
        time, evolving across sub-genres. A `rockr` package might be
        designed to do just that.

<br>

> Consider this as an open invitation for all collaborators interested
> in pursuing any of these or other development ideas for a prospective
> `rockr` package.

<br> [top of page](#top%20of%20page) <br><br>

-----

<br>

<p align="center">

<img src=https://media.giphy.com/media/xT9DPiSrihyxZnarbG/giphy.gif>

</p>

<br>

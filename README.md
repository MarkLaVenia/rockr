
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

1.  Wrangle data into an analysis-ready format

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

The following R packages need to be installed and loaded.

\[h/t stevenworthington
(<https://gist.github.com/stevenworthington/3178163>) for ipak.R
function\]

``` r

# check to see if packages "tidyverse", "gganimate", "gifki", and "png" are installed. Install them if they are not, then load them into the R session.

ipak <- function(pkg){
new.pkg <- pkg[!(pkg %in% installed.packages()[, "Package"])]
if (length(new.pkg)) 
    install.packages(new.pkg, dependencies = TRUE)
sapply(pkg, require, character.only = TRUE)
}

# usage
packages <- c("tidyverse", "gganimate", "gifki", "png")
ipak(packages)
```

## Analytic remises and assumptions (…however questionable)

  - a band’s status within the ranking of best bands is a function of
      - the number of albums the band has that qualify as one of the
        best albums of the year and
      - the proportion of people who vote the band’s albums as the best
        album of the year.

## Data preparation

## Rendering animated bar charts

<p align="center">

<img src="plots/album_poll_final_percentage.gif" alt="reviewer">

</p>

<p align="center">

<img src=https://media.giphy.com/media/xT9DPiSrihyxZnarbG/giphy.gif>

</p>

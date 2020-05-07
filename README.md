
-----

output: github\_document

-----

<!-- README.md is generated from README.Rmd. Please edit that file -->

# rockr <img src='hex/rockr_hex.png' align="right" height="150" />

<!-- badges: start -->

<!-- badges: end -->

The goal of rockr is to:

Given a series of Twitter polls on best album of the year, with polls
for each year, where some bands are reoccuring response options across
polls

1.  Wrangle data into an analysis-ready format

2.  Produce multiple scalings and subsets of data to permit different
    ways of conceptualising poll responses

3.  Render animated bar charts to visualise the cumulative and aggregate
    sentiment across the series of polls

<p align="center">

<img src="plots/album_poll_final_percentage.gif" alt="reviewer">

</p>

## Installation

You can install rockr by first going to your Developer Settings and
creating a new [Personal Access Token
(PAT)](https://github.com/settings/tokens). Once you have the new token,
run this:

``` r

# install.packages("devtools")

devtools::install_github("bmgf-k12/edreportr", auth_token = "PAT")
```

## Setup

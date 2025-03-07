
<!-- README.md is generated from README.Rmd. Please edit that file -->

# str.rdev

Provides functions for downloading and wrangling Enron data from an
external source. This package also contains a Shiny app that can be used
to explore this downloaded data.

<!-- badges: start -->

[![R-CMD-check](https://github.com/dsobolew/str.rdev/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/dsobolew/str.rdev/actions/workflows/R-CMD-check.yaml)
<!-- badges: end -->

The goal of `str.rdev` is to download and process the Enron data set.

## Installation

You can install the development version of `str.rdev` from
[GitHub](https://github.com/) with:

``` r
# install.packages("remotes")
remotes::install_github("StreamlineDataScience/str.rdev")
```

## Example

This is a basic example which shows you how to solve a common problem:

``` r
library(str.rdev)
## basic example code to launch shiny app

shiny::runApp(system.file("app", package = "str.rdev"))
```

## Limitations and Robustness

### `enron_download_data`

For this use case I kept the function very simple due to the bespoke
nature of the request. The function will accept a new link if the file
is moved but will default to the provided location. The function will
store the data in a temporary directory on the user’s local machine for
retrieval by the `enron_process_data` function. If additional files are
uploaded with the expectation that they will be combined, this function
will need to be updated.

### `enron_process_data`

This file is likely created by hand and displays inconsistencies in
spacing. Tables with the data we need are vertically stacked, and the
number of excel rows between tables differ. Each table has a consistent
format with a top row containing day of week values, a line for each
client, two rows for totals, and a blank row in the header. I used these
consistencies to build a for loop that will search for these tables
based on their header, calculate the number of rows based on how many
locations are present, and wrangle these individual table chunks into
one list item.

This setup requires some hard coded information to identify the rows and
columns to keep. Since there is no configuration table or other place to
retrieve this information, I added them directly into the function.
These would need to be manually updated if additional clients are added
in the future. The code is robust enough to locate the vertically
stacked tables that we need, regardless of the spacing that exists
between them, but if these tables change in format, the function will
need to be updated as well.

### Shiny App

I followed the instructions exactly and included one line plot to
display receipts for each location over time. This visualization is very
busy when looking at all clients, so I made it a Plotly graph to add
extra hover and click functionality to alleviate this. For more
legibility, I would either aggregate receipts into a wider time frame,
such as weeks, or use a rolling average to smooth out these lines if
that was acceptable. I might also facet these plots by location to make
it easier to view individual locations if the number was not subject to
change.

This dataset and use case is small enough where performance was not
something I focused on. For larger datasets or more enterprise use cases
I would focus more on [caching
functionality](https://shiny.rstudio.com/articles/caching.html).

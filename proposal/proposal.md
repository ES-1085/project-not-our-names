Project proposal
================
Not our names

``` r
library(tidyverse)
library(broom)
library(readr)
```

## 1. Introduction

Guiding Question: How much have Cod landings declined since 1950 in the
Atlantic and the Pacific?

For our final project, we have chosen to analyze a dataset titled
“Atlantic and Pacific Cod Landings (1950-2021). Using this dataset, we
plan to create representations of historical Cod landings in the
Atlantic and the Pacific (each individually), so we are able to
visualize and interpret the changing trends present in this dataset.
These data were compiled by NOAA, and collected from various different
sources listed in their own column in our dataset. The variables for our
dataset originally include: Year, State, NMFS Name, \# Pounds, \# Metric
Tons, \# Dollars, Collection, Scientific Name, \# TSN, and Source.

## 2. Data

[cod-landings.csv](https://github.com/ES-1085/project-not-our-names/files/10580941/cod-landings.csv)

``` r
cod_landings <- read_csv("../data/cod-landings.csv")
```

    ## Rows: 1196 Columns: 10
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (5): State, NMFS Name, Collection, Scientific Name, Source
    ## dbl (2): Year, Tsn
    ## num (3): Pounds, Metric Tons, Dollars
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
cod_landings <- cod_landings 
  colnames(cod_landings)[3] = "Common_Name"
  colnames(cod_landings)[5] = "Metric_Tons"
  colnames(cod_landings)[6] = "US_Dollars"
  colnames(cod_landings)[7] = "Fishing_Types"
  colnames(cod_landings)[8] = "Scientific_Name"
  colnames(cod_landings)[9] = "Taxonomic_Serial_Number"
  colnames(cod_landings)[10] = "Fishing_Companies"
glimpse(cod_landings)
```

    ## Rows: 1,196
    ## Columns: 10
    ## $ Year                    <dbl> 2021, 2021, 2021, 2021, 2021, 2021, 2021, 2021…
    ## $ State                   <chr> "ALASKA", "CONNECTICUT", "CONNECTICUT", "DELAW…
    ## $ Common_Name             <chr> "COD, PACIFIC", "COD, ATLANTIC", "COD, ATLANTI…
    ## $ Pounds                  <dbl> 330404171, 2277, 7921, 18, 1171, 47311, 120113…
    ## $ Metric_Tons             <dbl> 149870, 1, 4, 0, 1, 21, 545, 166, 21, 21, 41, …
    ## $ US_Dollars              <dbl> 116766003, 5031, NA, NA, NA, 128833, 2619925, …
    ## $ Fishing_Types           <chr> "Commercial", "Commercial", "Recreational", "R…
    ## $ Scientific_Name         <chr> "Gadus macrocephalus", "Gadus morhua", NA, NA,…
    ## $ Taxonomic_Serial_Number <dbl> 164711, 164712, 164712, 164712, 164712, 164712…
    ## $ Fishing_Companies       <chr> "AKFIN", "ACCSP", "MRIP", "MRIP", "MRIP", "ACC…

This glimpse represents our dataset with modified names to allow better
a better understanding of each variable.

## 3. Data analysis plan

Q1 - How do the weights of recorded recreational and commercial Cod
landings of the Atlantic and Pacific compare to one another between the
years 1950 and 2021? Plan Q1 - Summarize the data through a ridgeline
density plot of weight of landings, colored by “Fishing_Type” and
faceted by “Common_Name”. Also include an animated plot of the same type
to better show the fluctuation in landings avery 10 years.

Q2 - How are different states economically benefiting from the cod
fishery? Plan Q2 - Use a density map of the US to represent the money
made from the landings (commercial fishing) in different states over a
specific year. Possibility to aslo compare different years (eg: lowest
vs highest income)

Q3 - How does the ratio of recreational to commercial fishing fluctuate
over time in each region? Plan Q3 - Using a stacked bar chart, explore
the fluctations in ratios of commercial and recreational landings over
the years for each location.

Other possible options to extend the analysis - Merge this data with
similar ones for the Tuna fishery between 1950 and 2021. Find data for
the same time period of time but for the cod fisehry in Canada.

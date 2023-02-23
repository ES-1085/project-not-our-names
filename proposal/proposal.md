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
names(cod_landings)[names(cod_landings) == "NMFS Name"] <- "Common_Name"
names(cod_landings)[names(cod_landings) == "Metric Tons"] <- "Metric_Tons"
names(cod_landings)[names(cod_landings) == "Dollars"] <- "US_Dollars"
names(cod_landings)[names(cod_landings) == "Scientific Name"] <- "Scientific_Name"
names(cod_landings)[names(cod_landings) == "Collection"] <- "Fishing_Types"
names(cod_landings)[names(cod_landings) == "Tsn"] <- "Taxonomic_Serial_Number"
names(cod_landings)[names(cod_landings) == "Source"] <- "Fishing_Companies"
colnames(cod_landings)
```

    ##  [1] "Year"                    "State"                  
    ##  [3] "Common_Name"             "Pounds"                 
    ##  [5] "Metric_Tons"             "US_Dollars"             
    ##  [7] "Fishing_Types"           "Scientific_Name"        
    ##  [9] "Taxonomic_Serial_Number" "Fishing_Companies"

This glimpse represents our data set with modified names to allow a
better understanding of each variable.

## 3. Data analysis plan

Q1 - How do the weights of recorded recreational and commercial Cod
landings of the Atlantic and Pacific compare to one another between the
years 1950 and 2021? Plan Q1 - Summarize the data through a ridgeline
density plot of weight of landings, colored by “Fishing_Type” and
faceted by “Common_Name”. Also include an animated plot of the same type
to better show the fluctuation in landings every 10 years.

Q2 - How are different states economically benefiting from the cod
fishery? Plan Q2 - Use a density map of the US to represent the money
made from the landings (commercial fishing) in different states over a
specific year. Possibility to also compare different years (eg: lowest
vs highest income)

Q3 - How does the ratio of recreational to commercial fishing fluctuate
over time in each region? Plan Q3 - Using a stacked bar chart, explore
the fluctuations in ratios of commercial and recreational landings over
the years for each location.

Other possible options to extend the analysis - Merge this data with
similar ones for the Tuna fishery between 1950 and 2021. Find data for
the same time period of time but for the cod fishery in Canada.

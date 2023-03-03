---
title: "Cod landings project"
author: "Kristin, Delphine, Reese"
date: "2023-02-08"
output: html_document
---




```r
#install.packages("tidyverse")
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.4.0     ✔ purrr   1.0.1
## ✔ tibble  3.1.8     ✔ dplyr   1.1.0
## ✔ tidyr   1.3.0     ✔ stringr 1.5.0
## ✔ readr   2.1.3     ✔ forcats 1.0.0
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
#install.packages("devtools")
library(devtools)
```

```
## Loading required package: usethis
```

```r
#devtools::install_github("rstudio-education/dsbox", force = TRUE)
library(dsbox)
#install.packages("ggridges")
library(ggridges)
#install.packages("gganimate")
library(gganimate)
#install.packages("janitor")
library(janitor)
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

```r
#install.packages("stringr")
library(stringr) ## String manipulation
#install.packages("plotly")
library(plotly)
```

```
## 
## Attaching package: 'plotly'
## 
## The following object is masked from 'package:ggplot2':
## 
##     last_plot
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## The following object is masked from 'package:graphics':
## 
##     layout
```

```r
#install.packages("usmap")
library(usmap)
library(ggplot2)
#install.packages("devtools")
#devtools::install_github("UrbanInstitute/urbnmapr")
library(urbnmapr)
#install.packages("leaflet")
library(leaflet) ## For leaflet interactive maps
#install.packages("sf")
library(sf) ## For spatial data
```

```
## Linking to GEOS 3.8.0, GDAL 3.0.4, PROJ 6.3.1; sf_use_s2() is TRUE
```

```r
#install.packages("RColorBrewer")
library(RColorBrewer) ## For colour palettes
#install.packages("htmltools")
library(htmltools) ## For html
#install.packages("leafsync")
library(leafsync) ## For placing plots side by side
#install.packages("kableExtra")
library(kableExtra) ## Table output
```

```
## 
## Attaching package: 'kableExtra'
## 
## The following object is masked from 'package:dplyr':
## 
##     group_rows
```

```r
#install.packages("stringr")
library(stringr) ## String manipulation
library(gganimate)
library(transformr)
```

```
## 
## Attaching package: 'transformr'
## 
## The following object is masked from 'package:sf':
## 
##     st_normalize
```

```r
#install.packages("ggimage")
library(ggimage)
```


```r
us_maps <- us_map(region = "states")

states_sf <- get_urbn_map(map = "states", sf = TRUE)
```




```r
cod_landings <- read_csv("data/cod-landings.csv")  
```

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
```

```r
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

```
##  [1] "Year"                    "State"                  
##  [3] "Common_Name"             "Pounds"                 
##  [5] "Metric_Tons"             "US_Dollars"             
##  [7] "Fishing_Types"           "Scientific_Name"        
##  [9] "Taxonomic_Serial_Number" "Fishing_Companies"
```

```r
glimpse(cod_landings)
```

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
```


```r
cod_landings$Common_Name <- str_replace(cod_landings$Common_Name,"COD, PACIFIC", "Pacific_Cod")

cod_landings$Common_Name <- str_replace(cod_landings$Common_Name,"COD, ATLANTIC", "Atlantic_Cod")

cod_landings$Scientific_Name <- str_replace(cod_landings$Scientific_Name, "Gadus macrocephalus", "Gadus_macrocephalus")

cod_landings$Scientific_Name <- str_replace(cod_landings$Scientific_Name, "Gadus morhua", "Gadus_morhua")
cod_landings
```

```
## # A tibble: 1,196 × 10
##     Year State    Commo…¹ Pounds Metri…² US_Do…³ Fishi…⁴ Scien…⁵ Taxon…⁶ Fishi…⁷
##    <dbl> <chr>    <chr>    <dbl>   <dbl>   <dbl> <chr>   <chr>     <dbl> <chr>  
##  1  2021 ALASKA   Pacifi… 3.30e8  149870  1.17e8 Commer… Gadus_…  164711 AKFIN  
##  2  2021 CONNECT… Atlant… 2.28e3       1  5.03e3 Commer… Gadus_…  164712 ACCSP  
##  3  2021 CONNECT… Atlant… 7.92e3       4 NA      Recrea… <NA>     164712 MRIP   
##  4  2021 DELAWARE Atlant… 1.8 e1       0 NA      Recrea… <NA>     164712 MRIP   
##  5  2021 MAINE    Atlant… 1.17e3       1 NA      Recrea… <NA>     164712 MRIP   
##  6  2021 MAINE    Atlant… 4.73e4      21  1.29e5 Commer… Gadus_…  164712 ACCSP  
##  7  2021 MASSACH… Atlant… 1.20e6     545  2.62e6 Commer… Gadus_…  164712 ACCSP  
##  8  2021 MASSACH… Atlant… 3.65e5     166 NA      Recrea… <NA>     164712 MRIP   
##  9  2021 NEW HAM… Atlant… 4.63e4      21 NA      Recrea… <NA>     164712 MRIP   
## 10  2021 NEW HAM… Atlant… 4.58e4      21  1.25e5 Commer… Gadus_…  164712 ACCSP  
## # … with 1,186 more rows, and abbreviated variable names ¹​Common_Name,
## #   ²​Metric_Tons, ³​US_Dollars, ⁴​Fishing_Types, ⁵​Scientific_Name,
## #   ⁶​Taxonomic_Serial_Number, ⁷​Fishing_Companies
```

Q1 - How do the weights of recorded recreational and commercial Cod landings of the Atlantic and Pacific compare to one another between the years 1950 and 2021?
Plan Q1 - Summarize the data through a ridgeline density plot of weight of landings, colored by “Fishing_Type” and faceted by “Common_Name”. Also include an animated plot of the same type to better show the fluctuation in landings every 10 years. 


```r
cod_landings <- cod_landings %>% 
  filter(Common_Name != "COD, TOOTHED" ) %>% 
  filter(Common_Name != "COD, ARCTIC" )
```


```r
cod_landings_total <- cod_landings %>%
  group_by(Year) %>%
   mutate(Total_Metric_Tons = sum(Metric_Tons, na.rm = TRUE)) %>%
  arrange(desc(Year))
view(cod_landings_total)
```


```r
cod_landings_total <- cod_landings %>%
  group_by(Year) %>%
   mutate(Total_Pounds = sum(Pounds, na.rm = TRUE)) %>%
  arrange(desc(Year))
```


```r
cod_landings_total %>% 
  group_by(Total_Pounds) %>% 
count(Total_Pounds) %>% 
  arrange(desc(Total_Pounds))
```

```
## # A tibble: 72 × 2
## # Groups:   Total_Pounds [72]
##    Total_Pounds     n
##           <dbl> <int>
##  1    732343356    23
##  2    731753057    22
##  3    725544392    18
##  4    714632302    21
##  5    703590605    22
##  6    690723678    21
##  7    688693155    24
##  8    662064941    19
##  9    648218950    22
## 10    636932442    19
## # … with 62 more rows
```


```r
cod_image <- "pacific-cod.webp"
cod_landings_total %>%
  mutate(
    image = case_when(Year %in% c(1950:2021) ~ cod_image,
                      TRUE ~ cod_image)) 
```

```
## # A tibble: 1,193 × 12
## # Groups:   Year [72]
##     Year State    Commo…¹ Pounds Metri…² US_Do…³ Fishi…⁴ Scien…⁵ Taxon…⁶ Fishi…⁷
##    <dbl> <chr>    <chr>    <dbl>   <dbl>   <dbl> <chr>   <chr>     <dbl> <chr>  
##  1  2021 ALASKA   Pacifi… 3.30e8  149870  1.17e8 Commer… Gadus_…  164711 AKFIN  
##  2  2021 CONNECT… Atlant… 2.28e3       1  5.03e3 Commer… Gadus_…  164712 ACCSP  
##  3  2021 CONNECT… Atlant… 7.92e3       4 NA      Recrea… <NA>     164712 MRIP   
##  4  2021 DELAWARE Atlant… 1.8 e1       0 NA      Recrea… <NA>     164712 MRIP   
##  5  2021 MAINE    Atlant… 1.17e3       1 NA      Recrea… <NA>     164712 MRIP   
##  6  2021 MAINE    Atlant… 4.73e4      21  1.29e5 Commer… Gadus_…  164712 ACCSP  
##  7  2021 MASSACH… Atlant… 1.20e6     545  2.62e6 Commer… Gadus_…  164712 ACCSP  
##  8  2021 MASSACH… Atlant… 3.65e5     166 NA      Recrea… <NA>     164712 MRIP   
##  9  2021 NEW HAM… Atlant… 4.63e4      21 NA      Recrea… <NA>     164712 MRIP   
## 10  2021 NEW HAM… Atlant… 4.58e4      21  1.25e5 Commer… Gadus_…  164712 ACCSP  
## # … with 1,183 more rows, 2 more variables: Total_Pounds <dbl>, image <chr>,
## #   and abbreviated variable names ¹​Common_Name, ²​Metric_Tons, ³​US_Dollars,
## #   ⁴​Fishing_Types, ⁵​Scientific_Name, ⁶​Taxonomic_Serial_Number,
## #   ⁷​Fishing_Companies
```



















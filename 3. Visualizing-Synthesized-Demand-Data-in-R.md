Food Bank Demand
================
Noelle
2025-09-28

### Visualizing food bank demand in June 2024 using R

  
  
1. Import libraries

``` r
library(dplyr)
library(readr)
library(ggplot2)
library(lubridate)
```

  
  
2. Read file into tibble

``` r
visits <- read_csv('/Users/noellepastor/Library/CloudStorage/OneDrive-UTArlington/2025-2026 Career/SKILLS/Demand Forecasting for Dallas Food Bank/Synthetic_Food_Bank_Visits.csv')
```

``` r
glimpse(visits)
```

    ## Rows: 203,434
    ## Columns: 5
    ## $ Visit_Date         <date> 2020-01-01, 2020-01-01, 2020-01-01, 2020-01-01, 20…
    ## $ Client_ID          <chr> "a0df2dbd-b41f-4f31-87de-a77bdefffcc4", "b9181ab7-6…
    ## $ Household_Size     <dbl> 6, 4, 4, 2, 3, 2, 5, 2, 4, 4, 1, 3, 3, 4, 4, 3, 1, …
    ## $ Zip_Code           <chr> "75217", "75217", "Other", "75227", "Other", "75211…
    ## $ Pounds_Distributed <dbl> 73, 51, 44, 31, 39, 34, 66, 21, 56, 55, 10, 39, 38,…

  
  
3. Get visits from June of 2024. Summarize by number of **visits per
day**.

``` r
visits_june_2024 <- visits %>% 
  filter(year(Visit_Date)==2024, month(Visit_Date)==6) %>% 
  group_by(Visit_Date) %>% 
  summarize(num_visits = n())
```

  
  
4. Plot **Visit_Date** and **num_visits** in column chart.

``` r
ggplot(visits_june_2024) +
  
  geom_col(aes(x=Visit_Date, y=num_visits, fill=num_visits)) +
  
  labs(
  x="Visit Date", y="Number of Visits",
  title = "Number of Visits in June",
  subtitle = "Noelle Pastor",
  caption = "Increased demand at end of each week and month.",
  fill="Visits") +
  
  theme(plot.title = element_text(hjust=0.5),
        plot.subtitle = element_text(hjust=0.5),
        plot.caption = element_text(hjust=0.5)
        ) 
```

![](Visualizing-Synthesized-Demand-Data-in-R_files/figure-gfm/create%20chart-1.png)<!-- -->

# Alternate Column Chart Style

Highlight days with more than 140 visits to see trends.  

``` r
ggplot(visits_june_2024) +
  
  geom_col(aes(x=Visit_Date, y=num_visits),
          fill = ifelse(visits_june_2024$num_visits>125, "darkorange", "blue")) +
  
  labs(
  x="Visit Date", y="Number of Visits",
  title = "Number of Visits in June",
  subtitle = "Noelle Pastor",
  caption = "Increased demand at end of each week and month.",
  fill="Visits") +
  
  theme(plot.title = element_text(hjust=0.5),
        plot.subtitle = element_text(hjust=0.5),
        plot.caption = element_text(hjust=0.5)
        ) 
```

![](Visualizing-Synthesized-Demand-Data-in-R_files/figure-gfm/create%20chart%202-1.png)<!-- -->

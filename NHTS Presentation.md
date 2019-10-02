---
title: "NHTS Introduction"
author: "Darshan Pandit"
output:
  beamer_presentation:
    colortheme: beaver
    fonttheme: serif
    theme: CambridgeUS
  slidy_presentation: default
  pdf_document: beamer_presentation
  html_document: beamer_presentation
---
NHTS: Let's write some code!
========================================================
author: Darshan Pandit

date: September 17, 2019

Overview
========================================================

- Installation
- NHTS: Data Organization
- Summarize NHTS Package
        + Primitive Queries
        + Generating Estimates
- Cases from the NHTS Summary Report


Setting up tools
========================================================

- Install Rstudio: [Click Here!](https://www.rstudio.com/products/rstudio/download/#download)
- Install R: [Select a mirror from the list here and proceed](https://cran.r-project.org/mirrors.html)
- If on Windows, Install Rtools found [here](https://cran.r-project.org/bin/windows/Rtools/)
    

RStudio: Quick overview
========================================================

![Layout of Rstudio](NHTS Presentation-figure/rstudio_layout.png)


Installing summarizeNHTS package
========================================================
In your Rstudio's console:
\small
```r
install.packages('devtools')
devtools::install_github('Westat-Transportation/summarizeNHTS')
```

@ Linux Users:
\small
```r
install.packages('devtools')
devtools::install_github('darshanpandit/summarizeNHTS')
```
@ MacOS Users:
\small
```code
Please contribute to my Macbook fund! :P
```

Let's verify your installation...
========================================================
- Create a new R Notebook
- Insert/Modify code chunk as follows
\small
```r
library(summarizeNHTS)
download_nhts_data("2017", exdir="C:/NHTS")
```
<sub> You may change the directory of your choice or pass a relative directory. for eg: '/NHTS' <sub>

I'M READY!!! I'M READY!!! I'M READY!!!
========================================================
![I AM READY!](NHTS Presentation-figure/spongebob.JPG)

Overview
========================================================

- Installation
- NHTS: Data Organization
- Summarize NHTS Package
        + Generating Estimates
- Cases from the NHTS Summary Report

NHTS: Data Organization
========================================================

Let's explore the variables

![NHTS Schematic Diagram](NHTS Presentation-figure/nhts_schematic.png)


NHTS: Data Organization
========================================================
\small
```r
summary(dataset$data)
```
![NHTS Schematic Diagram](NHTS Presentation-figure/tables.png)
\small
```r
dataset$data$vehicle
```

NHTS: Data Organization
========================================================
Accessing Specific Columns
\small
```r
# By position
dataset$data$vehicle[, c(1, 3)]

# By name (single variable)
dataset$data$vehicle$ANNMILES

# By name
dataset$data$vehicle[, list(HOUSEID, ANNMILES)]
```

NHTS: Data Organization
========================================================
Accessing Specific Rows
\small
```r
# By row numbers (first 5 rows)
dataset$data$vehicle[1:5, ]

# By condition
dataset$data$vehicle[VEHTYPE == "01", ]

# By condition (multiple values)
dataset$data$vehicle[VEHTYPE %in% c("01","02"), ]
```

NHTS: Data Organization
========================================================

\large
Print Vehicle make, model

Use NHTS Codebook

NHTS: Data Organization
========================================================

Print Vehicle make, model

```r
dataset$data$vehicle[, list(MAKE, MODEL)]

```
Generating Estimates
========================================================

+ Introduction to the summarize_data function
+ Understanding summarize_data parameters
+ Statistics grouped by variables
+ Exploring aggregation options
+ Estimates using a subset condition
+ Referencing the documentation

Generating Estimates: Introduction to the summarize_data
========================================================

summarizeData provides a simple interface to perform complex queries on the NHTS datasets
\small
```r
summarize_data(
  data = dataset,
  agg = "household_count"
  )
)
```
![Aggregate by household_count](NHTS Presentation-figure/summarize_1.png)


Generating Estimates: Introduction to the summarize_data
========================================================



What do these values mean?


    W - Weighted statistic.
        Count of households weighted to the population
        
    E - Standard error of the weighted statistic.
        Standard error of the weighted count of households
        
    S - Surveyed/sampled statistic (unweighted statistic).
        The count of sampled households
        
    N - Number of observations/sample size.
        The number of observations is the same as the count
        of sampled households in this example


Generating Estimates: Understanding summarize_data parameters
========================================================

\small
```r
summarize_data(
  data = dataset,
  agg = "household_count"
  )
)
```

Required parameters

    data - NHTS dataset object
          Will always be the output of read_data.
          In our example, we stored the output in the dataset object.
    
    agg - Aggregate function label. Our example used 'household_count'
          but agg could be a number of other labels.
          
Generating Estimates
========================================================

+ Introduction to the summarize_data function
+ Understanding summarize_data parameters
+ Statistics grouped by variables
+ Exploring aggregation options
+ Estimates using a subset condition
+ Referencing the documentation

Generating Estimates: Statistics grouped by variables
========================================================
\small
```r
summarize_data(
  data = dataset,
  agg = "household_count",
  by = "HOMEOWN"
)
```
![](NHTS Presentation-figure/hh_homeown.png)

Generating Estimates: Statistics grouped by variables
========================================================
You can add multiple columns!
\small
```{r}
summarize_data(
  data = dataset,
  agg = "household_count",
  by = c("HH_RACE","HOMEOWN")
)
```

Generating Estimates: Statistics grouped by variables
========================================================
Generating Frequencies/P  roportions

For any of the following Aggregate Fields:

        'household_count', 'person_count', 'trip_count', 'vehicle_count'

Parameter prop=TRUE

Generating Estimates: Statistics grouped by variables
========================================================
\small
```r
# Proportion of persons by WORKER, worker status
summarize_data(
  data = dataset,
  agg = "person_count",
  by = "WORKER",
  prop = TRUE
)
```
![](NHTS Presentation-figure/pp_count.png)

Generating Estimates
========================================================

+ Introduction to the summarize_data function
+ Understanding summarize_data parameters
+ Statistics grouped by variables
+ Exploring aggregation options
+ Estimates using a subset condition
+ Referencing the documentation

Generating Estimates: Exploring aggregation options
========================================================
Using Numeric Aggregates

Instead of Aggregate fields directly, following operands can also be passed

        'sum', 'avg', 'median'

If this is done, agg_var parameter containing the field/column must be passed

agg_var must be numeric

Missing or Invalid Values (-1,-7,-8,-9) are automagically handled!

Generating Estimates: Exploring aggregation options
========================================================

```r
summarize_data(
  data = dataset,
  agg = "avg",
  agg_var = "TRPMILES"
)
```
![](NHTS Presentation-figure/avg_tripmiles.png)

Generating Estimates: Exploring aggregation options
========================================================

Generate Median Trip Miles by Homeownership ?

![](NHTS Presentation-figure/trip_hown.png)

Generating Estimates: Exploring aggregation options
========================================================

Solution:

```r
summarize_data(
  data = dataset,
  agg = "median",
  agg_var = "TRPMILES",
  by = "HOMEOWN"
)
```

Generating Estimates: Exploring aggregation options
========================================================

You can use either of the following trip rate aggregates

    'household_trip_rate' - Daily Person Trips per Household
    'person_trip_rate' - Daily Person Trips per Person
\small
```{r}
summarize_data(
  data = dataset,
  agg = "person_trip_rate",
  by = "WORKER"
)
```
Generating Estimates
========================================================

+ Introduction to the summarize_data function
+ Understanding summarize_data parameters
+ Statistics grouped by variables
+ Exploring aggregation options
+ Estimates using a subset condition
+ Referencing the documentation

Generating Estimates: Estimates using a subset condition
========================================================

Pre-aggregation subset conditions can be specified using the subset parameter.

    Argument should be passed as a string.
    
Subsetting character variables

\small
```r
#Distribution of Leisure Travel
summarize_data(
  data = dataset,
  agg = "trip_count",
  by = "TRAVDAY",
  prop = TRUE,
  subset = "WHYTRP90 %in% c('07','08','10')"
)
```
Generating Estimates: Estimates using a subset condition
========================================================

Subsetting Numeric Variables

\small
```{r}
# Person trip rate by Sex (for millennials)
summarize_data(
  data = dataset,
  agg = "person_trip_rate",
  by = "R_SEX",
  subset = "R_AGE >= 18 & R_AGE <= 34")
```

![](NHTS Presentation-figure/trprate_gender.png)

Generating Estimates: Referencing the documentation
========================================================

```r
?summarize_data
```

Generating Estimates: Writing to CSV files
========================================================

![](NHTS Presentation-figure/trip_hown.png)

\small
```r
dataset <- read_data("2017", csv_path="NHTS")
df <- summarize_data(
  data = dataset,
  agg = "median",   
  agg_var = "TRPMILES",
  by = "HOMEOWN"
)
write.csv(df,'output.csv')
```


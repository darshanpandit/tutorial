---
output:
  pdf_document: default
  html_document: default
---

## Assignment 1: Working with NHTS
##### Total: 100 points.



### Part I

Generate the following estimates using the Summarize NHTS Package. **[5 Pts Each]**

    a. Average Person Trip Per Day grouped by Day of Week
    b. Average Person Trip Per Day grouped by Trip Purpose
    c. Median Trip Distance grouped by Trip Purpose
    d. Number of households with at least 1 child, grouped by Household's State
    e. Briefly describe why and how are the estimates generated
    f. How can Avg. Person Trip Per Day estimates be used in a 4-Step Travel demand model.

### Part II

Children's travel to school has been increasingly getting more attention. This [NHTS's briefing on Children's Travel to School](https://nhts.ornl.gov/assets/FHWA_NHTS_%20Brief_Traveltoschool_032519.pdf) summarizes several trends. One way to reproduce one of the tables from the report can be found below. Answer the following questions pertaining to this problem: **[5 Pts Each]**

a. Briefly describe the result.
b. Check out the possible values for the field 'SCHTYP' in the codebook and describe if the subset in the following query is appropriate/adequate for the analysis.
```
temp<-summarize_data(
        data = dataset_2017,
        agg = "trip_count",
        by = c("SCHTRN1"),
        subset = "SCHTYP %in% c('01','02','-9','-7')",
        prop = TRUE
      )
make_table(temp)
```

c. Can you compute a similar table using the field: 'WHYTRIP', instead of the above query?
d. Is the the method that you impemented more accurate? Please explain why.
e. Generate the required table to plot the Fig 2. in the report shared above. ( Means of Travel to School grouped by Distance Categories) Plot is not required, you can just give the query needed to generate a table that can be converted into the plot.
f. Is there a significant difference in modal shares for trips to school across students living in urban area and those living in rural area?

### Part III

Please review the dynamic visualization tool available at [this page](https://flowingdata.com/2015/12/15/a-day-in-the-life-of-americans/).

a. **[10 Pts]** Assuming we want to generate a dynamic visualization similar to that on the webpage but using NHTS 2017, What tables will be required? Can you provide a query/code?
b. **[15 Pts]** What are the main difficulties that you encoutered? and how did you handle them? (Activity Types, Time of Day, Statistical Validity)


### Part IV

Generate the query and answer the following questions pertaining to Car Ownership **[5 Pts Each]**

a. How many Urban and Rural Households with 0 vehicles are estimated using NHTS 2017? Are thesee estimates reliable? and at what geographical level?
b.  A friend of yours would like to find out if Education levels affect choice of vehicle type. He came up with the following query,  however, there is an issue with this. Can you identify the problem? ( Check the R Console message upon execution for a hint. )
```
temp<- summarize_data(
  data= dataset_2017,
  agg = "person_count",
  by = c('VEHTYPE','EDUC')
)
make_table(temp)
```
c. Can you suggest a plan to best answer his question? Just a plan will suffice. You are not required to give the exact code/query here.


---
title: "Chronic Disease Mortality - Predictive Modeling"
author: "Chetan, Augustine"
output: 
  html_document:
    keep_md: true
    df_print: paged
---
    
    

# Introduction
Using CDC's Chronic Disease Indicators dataset, we are interested in analyzing if higher prevalence of preventive care leads to lower chronic disease mortality rate. 
We will use predictive modeling to analyze the relationship between chronic disease mortality rate and prevalence of preventive care.

These are datasets we are using for our analysis:
1. CDC Chronic Disease Indicators (CDI) - https://www.cdc.gov/cdi/
2. PLACES: Local Data for Better Health - https://www.cdc.gov/places/index.html

# Research Question
**What is the relationship between chronic disease mortality rate and prevalence of preventive care?**

# Hypothesis
We hypothesize that low prevalence of preventive care leads to higher chronic disease mortality rate.

Null Hypothesis: There is no relationship between chronic disease mortality rate and prevalence of preventive care.

# Methodology
We will use predictive modeling to analyze the relationship between chronic disease mortality rate and prevalence of preventive care. We will use the following steps:
1. Mortality Rate Data: Extract mortality rate data from CDC Chronic Disease Indicators dataset.
2. Prevalence of Preventive Care Data: Extract prevalence of preventive care data from PLACES dataset.
3. Data Preprocessing: Check for missing values, data types, and unique values in the datasets.
4. Data Analysis: Analyze the relationship between chronic disease mortality rate and prevalence of preventive care.
5. Predictive Modeling: Use machine learning algorithms to predict chronic disease mortality rate based on prevalence of preventive care.
6. Model Evaluation: Evaluate the performance of the predictive models using metrics like RMSE, MAE, and R-squared.
7. Conclusion: Draw conclusions based on the predictive modeling results.

# Data

```r
library(readr)
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
# # Load the data for mortality rate from chronic diseases
# cdi_dataset <- read.csv("us_cdi_dataset.csv", stringsAsFactors = FALSE)

# # above dataset is downloaded from here: https://data.cdc.gov/Chronic-Disease-Indicators/U-S-Chronic-Disease-Indicators/hksd-2xuw/about_data
# # it's about 400MB in size, so I am not uploading it to the repository

# # From this dataset, we are only interested in extracting mortality rate for chronic diseases

# mortality_data <- cdi_dataset %>% 
#   filter(grepl("mortality", Question, ignore.case = TRUE)) %>%
#     filter(DataValueType == "Age-adjusted Rate") %>%
#         select(Topic, Question, DataValueType, DataValueUnit, StratificationCategory1, Stratification1, 
#           DataValue, DataSource, LocationDesc, LocationAbbr, GeoLocation, LocationID, YearStart, YearEnd)

# # number of rows and columns
# dim(mortality_data)

# head(mortality_data)

# # lets write this data to a csv file (we will use this data for our analysis)
# write.csv(mortality_data, "mortality_data.csv", row.names = TRUE)

# # Load the data for prevalence of preventive care
# places_dataset <- read.csv("PLACES__Local_Data_for_Better_Health__Place_Data_2023_release_20240314.csv", stringsAsFactors = FALSE)

# # above dataset is downloaded from here: https://data.cdc.gov/500-Cities-Places/PLACES-Local-Data-for-Better-Health-Place-Data-202/eav7-hnsx/about_data
# # it's about 500MB in size, so I am not uploading it to the repository

# # From this dataset, we are only interested in extracting prevalence of preventive care
# # Category = Prevention and Data_Value_Type = Age-adjusted prevalence

# preventive_care_data <- places_dataset %>% 
#   filter(Category == "Prevention") %>%
#     filter(Data_Value_Type == "Age-adjusted prevalence") %>%
#         select(Category, Short_Question_Text, Measure, Data_Value_Type, 
#             Data_Value_Unit, Data_Value, StateDesc, StateAbbr, Geolocation, LocationID, Year)

# # number of rows and columns
# dim(preventive_care_data)

# head(preventive_care_data)

# # lets write this data to a csv file (we will use this data for our analysis)
# write.csv(preventive_care_data, "preventive_care_data.csv", row.names = TRUE)
```

# Data Preprocessing

```r
# mortality data
# Load the data (row names are included in the dataset)
mortality_data <- read.csv("mortality_data.csv", stringsAsFactors = FALSE, row.names = 1)
# mortality_data <- as.data.frame(mortality_data)
head(mortality_data)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Topic"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Question"],"name":[2],"type":["chr"],"align":["left"]},{"label":["DataValueType"],"name":[3],"type":["chr"],"align":["left"]},{"label":["DataValueUnit"],"name":[4],"type":["chr"],"align":["left"]},{"label":["StratificationCategory1"],"name":[5],"type":["chr"],"align":["left"]},{"label":["Stratification1"],"name":[6],"type":["chr"],"align":["left"]},{"label":["DataValue"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["DataSource"],"name":[8],"type":["chr"],"align":["left"]},{"label":["LocationDesc"],"name":[9],"type":["chr"],"align":["left"]},{"label":["LocationAbbr"],"name":[10],"type":["chr"],"align":["left"]},{"label":["GeoLocation"],"name":[11],"type":["chr"],"align":["left"]},{"label":["LocationID"],"name":[12],"type":["int"],"align":["right"]},{"label":["YearStart"],"name":[13],"type":["int"],"align":["right"]},{"label":["YearEnd"],"name":[14],"type":["int"],"align":["right"]}],"data":[{"1":"Alcohol","2":"Chronic liver disease mortality","3":"Age-adjusted Rate","4":"cases per 100,000","5":"Race/Ethnicity","6":"Asian or Pacific Islander","7":"NA","8":"NVSS","9":"New Mexico","10":"NM","11":"POINT (-106.24058098499967 34.52088095200048)","12":"35","13":"2012","14":"2012","_rn_":"1"},{"1":"Alcohol","2":"Chronic liver disease mortality","3":"Age-adjusted Rate","4":"cases per 100,000","5":"Race/Ethnicity","6":"Hispanic","7":"6.9","8":"NVSS","9":"Massachusetts","10":"MA","11":"POINT (-72.08269067499964 42.27687047000046)","12":"25","13":"2013","14":"2013","_rn_":"2"},{"1":"Alcohol","2":"Chronic liver disease mortality","3":"Age-adjusted Rate","4":"cases per 100,000","5":"Race/Ethnicity","6":"White, non-Hispanic","7":"7.9","8":"NVSS","9":"Connecticut","10":"CT","11":"POINT (-72.64984095199964 41.56266102000046)","12":"9","13":"2012","14":"2012","_rn_":"3"},{"1":"Alcohol","2":"Chronic liver disease mortality","3":"Age-adjusted Rate","4":"cases per 100,000","5":"Gender","6":"Male","7":"12.9","8":"NVSS","9":"Ohio","10":"OH","11":"POINT (-82.40426005599966 40.06021014100048)","12":"39","13":"2010","14":"2010","_rn_":"4"},{"1":"Alcohol","2":"Chronic liver disease mortality","3":"Age-adjusted Rate","4":"cases per 100,000","5":"Race/Ethnicity","6":"American Indian or Alaska Native","7":"43.3","8":"NVSS","9":"Minnesota","10":"MN","11":"POINT (-94.79420050299967 46.35564873600049)","12":"27","13":"2013","14":"2013","_rn_":"5"},{"1":"Alcohol","2":"Chronic liver disease mortality","3":"Age-adjusted Rate","4":"cases per 100,000","5":"Race/Ethnicity","6":"Black, non-Hispanic","7":"7.6","8":"NVSS","9":"North Carolina","10":"NC","11":"POINT (-79.15925046299964 35.466220975000454)","12":"37","13":"2013","14":"2013","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
# Number of rows and columns
dim(mortality_data)
```

```
## [1] 58393    14
```

```r
# Check for missing values
missing_values <- mortality_data %>% 
  summarise_all(funs(sum(is.na(.))))
```

```
## Warning: `funs()` was deprecated in dplyr 0.8.0.
## ℹ Please use a list of either functions or lambdas:
## 
## # Simple named list: list(mean = mean, median = median)
## 
## # Auto named with `tibble::lst()`: tibble::lst(mean, median)
## 
## # Using lambdas list(~ mean(., trim = .2), ~ median(., na.rm = TRUE))
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```r
missing_values
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["Topic"],"name":[1],"type":["int"],"align":["right"]},{"label":["Question"],"name":[2],"type":["int"],"align":["right"]},{"label":["DataValueType"],"name":[3],"type":["int"],"align":["right"]},{"label":["DataValueUnit"],"name":[4],"type":["int"],"align":["right"]},{"label":["StratificationCategory1"],"name":[5],"type":["int"],"align":["right"]},{"label":["Stratification1"],"name":[6],"type":["int"],"align":["right"]},{"label":["DataValue"],"name":[7],"type":["int"],"align":["right"]},{"label":["DataSource"],"name":[8],"type":["int"],"align":["right"]},{"label":["LocationDesc"],"name":[9],"type":["int"],"align":["right"]},{"label":["LocationAbbr"],"name":[10],"type":["int"],"align":["right"]},{"label":["GeoLocation"],"name":[11],"type":["int"],"align":["right"]},{"label":["LocationID"],"name":[12],"type":["int"],"align":["right"]},{"label":["YearStart"],"name":[13],"type":["int"],"align":["right"]},{"label":["YearEnd"],"name":[14],"type":["int"],"align":["right"]}],"data":[{"1":"0","2":"0","3":"0","4":"0","5":"0","6":"0","7":"14977","8":"0","9":"0","10":"0","11":"0","12":"0","13":"0","14":"0"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
# What's the percentage of missing values?
missing_values_percentage <- missing_values / nrow(mortality_data) * 100
missing_values_percentage
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["Topic"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["Question"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["DataValueType"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["DataValueUnit"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["StratificationCategory1"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Stratification1"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["DataValue"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["DataSource"],"name":[8],"type":["dbl"],"align":["right"]},{"label":["LocationDesc"],"name":[9],"type":["dbl"],"align":["right"]},{"label":["LocationAbbr"],"name":[10],"type":["dbl"],"align":["right"]},{"label":["GeoLocation"],"name":[11],"type":["dbl"],"align":["right"]},{"label":["LocationID"],"name":[12],"type":["dbl"],"align":["right"]},{"label":["YearStart"],"name":[13],"type":["dbl"],"align":["right"]},{"label":["YearEnd"],"name":[14],"type":["dbl"],"align":["right"]}],"data":[{"1":"0","2":"0","3":"0","4":"0","5":"0","6":"0","7":"25.64862","8":"0","9":"0","10":"0","11":"0","12":"0","13":"0","14":"0"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
# preventive care data
# Load the data (row names are included in the dataset)
preventive_care_data <- read.csv("preventive_care_data.csv", stringsAsFactors = FALSE, row.names = 1)
# preventive_care_data <- as.data.frame(preventive_care_data)
head(preventive_care_data)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Category"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Short_Question_Text"],"name":[2],"type":["chr"],"align":["left"]},{"label":["Measure"],"name":[3],"type":["chr"],"align":["left"]},{"label":["Data_Value_Type"],"name":[4],"type":["chr"],"align":["left"]},{"label":["Data_Value_Unit"],"name":[5],"type":["chr"],"align":["left"]},{"label":["Data_Value"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["StateDesc"],"name":[7],"type":["chr"],"align":["left"]},{"label":["StateAbbr"],"name":[8],"type":["chr"],"align":["left"]},{"label":["Geolocation"],"name":[9],"type":["chr"],"align":["left"]},{"label":["LocationID"],"name":[10],"type":["int"],"align":["right"]},{"label":["Year"],"name":[11],"type":["int"],"align":["right"]}],"data":[{"1":"Prevention","2":"Taking BP Medication","3":"Taking medicine for high blood pressure control among adults aged >=18 years with high blood pressure","4":"Age-adjusted prevalence","5":"%","6":"55.1","7":"Minnesota","8":"MN","9":"POINT (-93.58381487 45.03287486)","10":"2741480","11":"2021","_rn_":"1"},{"1":"Prevention","2":"Core preventive services for older men","3":"Older adult men aged >=65 years who are up to date on a core set of clinical preventive services: Flu shot past year, PPV shot ever, Colorectal cancer screening","4":"Age-adjusted prevalence","5":"%","6":"42.8","7":"Alabama","8":"AL","9":"POINT (-86.6380255 31.83464701)","10":"131912","11":"2020","_rn_":"2"},{"1":"Prevention","2":"Dental Visit","3":"Visits to dentist or dental clinic among adults aged >=18 years","4":"Age-adjusted prevalence","5":"%","6":"50.7","7":"Alabama","8":"AL","9":"POINT (-87.52418026 33.8962507)","10":"112016","11":"2020","_rn_":"3"},{"1":"Prevention","2":"Core preventive services for older men","3":"Older adult men aged >=65 years who are up to date on a core set of clinical preventive services: Flu shot past year, PPV shot ever, Colorectal cancer screening","4":"Age-adjusted prevalence","5":"%","6":"37.4","7":"Alabama","8":"AL","9":"POINT (-86.74676304 31.63913459)","10":"129560","11":"2020","_rn_":"4"},{"1":"Prevention","2":"Dental Visit","3":"Visits to dentist or dental clinic among adults aged >=18 years","4":"Age-adjusted prevalence","5":"%","6":"49.2","7":"Alabama","8":"AL","9":"POINT (-88.01697154 31.15395341)","10":"111488","11":"2020","_rn_":"5"},{"1":"Prevention","2":"Core preventive services for older men","3":"Older adult men aged >=65 years who are up to date on a core set of clinical preventive services: Flu shot past year, PPV shot ever, Colorectal cancer screening","4":"Age-adjusted prevalence","5":"%","6":"42.8","7":"Alabama","8":"AL","9":"POINT (-85.3061065 32.79106472)","10":"119216","11":"2020","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
# Number of rows and columns
dim(preventive_care_data)
```

```
## [1] 281111     11
```

```r
# Check for missing values
missing_values <- preventive_care_data %>% 
  summarise_all(funs(sum(is.na(.))))
```

```
## Warning: `funs()` was deprecated in dplyr 0.8.0.
## ℹ Please use a list of either functions or lambdas:
## 
## # Simple named list: list(mean = mean, median = median)
## 
## # Auto named with `tibble::lst()`: tibble::lst(mean, median)
## 
## # Using lambdas list(~ mean(., trim = .2), ~ median(., na.rm = TRUE))
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```r
missing_values
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["Category"],"name":[1],"type":["int"],"align":["right"]},{"label":["Short_Question_Text"],"name":[2],"type":["int"],"align":["right"]},{"label":["Measure"],"name":[3],"type":["int"],"align":["right"]},{"label":["Data_Value_Type"],"name":[4],"type":["int"],"align":["right"]},{"label":["Data_Value_Unit"],"name":[5],"type":["int"],"align":["right"]},{"label":["Data_Value"],"name":[6],"type":["int"],"align":["right"]},{"label":["StateDesc"],"name":[7],"type":["int"],"align":["right"]},{"label":["StateAbbr"],"name":[8],"type":["int"],"align":["right"]},{"label":["Geolocation"],"name":[9],"type":["int"],"align":["right"]},{"label":["LocationID"],"name":[10],"type":["int"],"align":["right"]},{"label":["Year"],"name":[11],"type":["int"],"align":["right"]}],"data":[{"1":"0","2":"0","3":"0","4":"0","5":"0","6":"10403","7":"0","8":"0","9":"0","10":"0","11":"0"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
# What's the percentage of missing values?
missing_values_percentage <- missing_values / nrow(preventive_care_data) * 100
missing_values_percentage
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["Category"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["Short_Question_Text"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Measure"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Data_Value_Type"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Data_Value_Unit"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Data_Value"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["StateDesc"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["StateAbbr"],"name":[8],"type":["dbl"],"align":["right"]},{"label":["Geolocation"],"name":[9],"type":["dbl"],"align":["right"]},{"label":["LocationID"],"name":[10],"type":["dbl"],"align":["right"]},{"label":["Year"],"name":[11],"type":["dbl"],"align":["right"]}],"data":[{"1":"0","2":"0","3":"0","4":"0","5":"0","6":"3.700673","7":"0","8":"0","9":"0","10":"0","11":"0"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
# unique values for Topic and Question in mortality data
unique(mortality_data$Topic)
```

```
## [1] "Alcohol"                              
## [2] "Asthma"                               
## [3] "Chronic Kidney Disease"               
## [4] "Chronic Obstructive Pulmonary Disease"
## [5] "Cardiovascular Disease"               
## [6] "Diabetes"                             
## [7] "Overarching Conditions"
```

```r
# unique(mortality_data$Question)

# unique values for Short_Question_Text and Measure in preventive care data
unique(preventive_care_data$Short_Question_Text)
```

```
##  [1] "Taking BP Medication"                    
##  [2] "Core preventive services for older men"  
##  [3] "Dental Visit"                            
##  [4] "Colorectal Cancer Screening"             
##  [5] "Cholesterol Screening"                   
##  [6] "Mammography"                             
##  [7] "Health Insurance"                        
##  [8] "Annual Checkup"                          
##  [9] "Core preventive services for older women"
## [10] "Cervical Cancer Screening"
```

```r
# unique(preventive_care_data$Measure)
```

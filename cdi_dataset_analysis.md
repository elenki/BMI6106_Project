---
title: "Chronic Disease Indicators (CDI) - Data Exploration and Cleaning"
author: "Chetan, Augustine"
output: 
    html_document:
        theme: united
        highlight: tango
        code_folding: show
        fig_caption: true
        toc: true
        toc_depth: 3
        toc_float: true
        number_sections: true
        fig_width: 8
        fig_height: 6
        fig_align: "center"
        df_print: paged
        keep_md: true
---




```r
# Load the required libraries
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
library(ggplot2)
library(tidyr)
library(readr)
```


```r
# Load the dataset
cdi <- read.csv("us_cdi_dataset.csv", stringsAsFactors = FALSE)
```


```r
# Display the first few rows of the dataset
head(cdi)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["YearStart"],"name":[1],"type":["int"],"align":["right"]},{"label":["YearEnd"],"name":[2],"type":["int"],"align":["right"]},{"label":["LocationAbbr"],"name":[3],"type":["chr"],"align":["left"]},{"label":["LocationDesc"],"name":[4],"type":["chr"],"align":["left"]},{"label":["DataSource"],"name":[5],"type":["chr"],"align":["left"]},{"label":["Topic"],"name":[6],"type":["chr"],"align":["left"]},{"label":["Question"],"name":[7],"type":["chr"],"align":["left"]},{"label":["Response"],"name":[8],"type":["lgl"],"align":["right"]},{"label":["DataValueUnit"],"name":[9],"type":["chr"],"align":["left"]},{"label":["DataValueType"],"name":[10],"type":["chr"],"align":["left"]},{"label":["DataValue"],"name":[11],"type":["chr"],"align":["left"]},{"label":["DataValueAlt"],"name":[12],"type":["dbl"],"align":["right"]},{"label":["DataValueFootnoteSymbol"],"name":[13],"type":["chr"],"align":["left"]},{"label":["DatavalueFootnote"],"name":[14],"type":["chr"],"align":["left"]},{"label":["LowConfidenceLimit"],"name":[15],"type":["dbl"],"align":["right"]},{"label":["HighConfidenceLimit"],"name":[16],"type":["dbl"],"align":["right"]},{"label":["StratificationCategory1"],"name":[17],"type":["chr"],"align":["left"]},{"label":["Stratification1"],"name":[18],"type":["chr"],"align":["left"]},{"label":["StratificationCategory2"],"name":[19],"type":["lgl"],"align":["right"]},{"label":["Stratification2"],"name":[20],"type":["lgl"],"align":["right"]},{"label":["StratificationCategory3"],"name":[21],"type":["lgl"],"align":["right"]},{"label":["Stratification3"],"name":[22],"type":["lgl"],"align":["right"]},{"label":["GeoLocation"],"name":[23],"type":["chr"],"align":["left"]},{"label":["ResponseID"],"name":[24],"type":["lgl"],"align":["right"]},{"label":["LocationID"],"name":[25],"type":["int"],"align":["right"]},{"label":["TopicID"],"name":[26],"type":["chr"],"align":["left"]},{"label":["QuestionID"],"name":[27],"type":["chr"],"align":["left"]},{"label":["DataValueTypeID"],"name":[28],"type":["chr"],"align":["left"]},{"label":["StratificationCategoryID1"],"name":[29],"type":["chr"],"align":["left"]},{"label":["StratificationID1"],"name":[30],"type":["chr"],"align":["left"]},{"label":["StratificationCategoryID2"],"name":[31],"type":["lgl"],"align":["right"]},{"label":["StratificationID2"],"name":[32],"type":["lgl"],"align":["right"]},{"label":["StratificationCategoryID3"],"name":[33],"type":["lgl"],"align":["right"]},{"label":["StratificationID3"],"name":[34],"type":["lgl"],"align":["right"]}],"data":[{"1":"2014","2":"2014","3":"AR","4":"Arkansas","5":"SEDD; SID","6":"Asthma","7":"Hospitalizations for asthma","8":"NA","9":"","10":"Number","11":"916","12":"916","13":"","14":"","15":"NA","16":"NA","17":"Gender","18":"Male","19":"NA","20":"NA","21":"NA","22":"NA","23":"POINT (-92.27449074299966 34.74865012400045)","24":"NA","25":"5","26":"AST","27":"AST3_1","28":"NMBR","29":"GENDER","30":"GENM","31":"NA","32":"NA","33":"NA","34":"NA","_rn_":"1"},{"1":"2018","2":"2018","3":"CO","4":"Colorado","5":"SEDD; SID","6":"Asthma","7":"Hospitalizations for asthma","8":"NA","9":"","10":"Number","11":"2227","12":"2227","13":"","14":"","15":"NA","16":"NA","17":"Overall","18":"Overall","19":"NA","20":"NA","21":"NA","22":"NA","23":"POINT (-106.13361092099967 38.843840757000464)","24":"NA","25":"8","26":"AST","27":"AST3_1","28":"NMBR","29":"OVERALL","30":"OVR","31":"NA","32":"NA","33":"NA","34":"NA","_rn_":"2"},{"1":"2018","2":"2018","3":"DC","4":"District of Columbia","5":"SEDD; SID","6":"Asthma","7":"Hospitalizations for asthma","8":"NA","9":"","10":"Number","11":"708","12":"708","13":"","14":"","15":"NA","16":"NA","17":"Overall","18":"Overall","19":"NA","20":"NA","21":"NA","22":"NA","23":"POINT (-77.036871 38.907192)","24":"NA","25":"11","26":"AST","27":"AST3_1","28":"NMBR","29":"OVERALL","30":"OVR","31":"NA","32":"NA","33":"NA","34":"NA","_rn_":"3"},{"1":"2017","2":"2017","3":"GA","4":"Georgia","5":"SEDD; SID","6":"Asthma","7":"Hospitalizations for asthma","8":"NA","9":"","10":"Number","11":"3520","12":"3520","13":"","14":"","15":"NA","16":"NA","17":"Gender","18":"Female","19":"NA","20":"NA","21":"NA","22":"NA","23":"POINT (-83.62758034599966 32.83968109300048)","24":"NA","25":"13","26":"AST","27":"AST3_1","28":"NMBR","29":"GENDER","30":"GENF","31":"NA","32":"NA","33":"NA","34":"NA","_rn_":"4"},{"1":"2010","2":"2010","3":"MI","4":"Michigan","5":"SEDD; SID","6":"Asthma","7":"Hospitalizations for asthma","8":"NA","9":"","10":"Number","11":"123","12":"123","13":"","14":"","15":"NA","16":"NA","17":"Race/Ethnicity","18":"Hispanic","19":"NA","20":"NA","21":"NA","22":"NA","23":"POINT (-84.71439026999968 44.6613195430005)","24":"NA","25":"26","26":"AST","27":"AST3_1","28":"NMBR","29":"RACE","30":"HIS","31":"NA","32":"NA","33":"NA","34":"NA","_rn_":"5"},{"1":"2015","2":"2015","3":"MT","4":"Montana","5":"SEDD; SID","6":"Asthma","7":"Hospitalizations for asthma","8":"NA","9":"","10":"Number","11":"","12":"NA","13":"-","14":"No data available","15":"NA","16":"NA","17":"Race/Ethnicity","18":"Hispanic","19":"NA","20":"NA","21":"NA","22":"NA","23":"POINT (-109.42442064499971 47.06652897200047)","24":"NA","25":"30","26":"AST","27":"AST3_1","28":"NMBR","29":"RACE","30":"HIS","31":"NA","32":"NA","33":"NA","34":"NA","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
# Display the Topics in the dataset
cdi %>% select(Topic) %>% distinct()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["Topic"],"name":[1],"type":["chr"],"align":["left"]}],"data":[{"1":"Asthma"},{"1":"Cancer"},{"1":"Chronic Kidney Disease"},{"1":"Chronic Obstructive Pulmonary Disease"},{"1":"Cardiovascular Disease"},{"1":"Diabetes"},{"1":"Disability"},{"1":"Reproductive Health"},{"1":"Tobacco"},{"1":"Alcohol"},{"1":"Arthritis"},{"1":"Nutrition, Physical Activity, and Weight Status"},{"1":"Mental Health"},{"1":"Older Adults"},{"1":"Oral Health"},{"1":"Overarching Conditions"},{"1":"Immunization"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
# we are interested in understanding the mortality from chronic diseases
# how mortality specifically (as opposed to morbidity) is related to other variables in the dataset

# Filter for Question containing mortality
# Display the Question and Topic
cdi %>% filter(grepl("mortality", Question, ignore.case = TRUE)) %>% select(Question, Topic) %>% distinct()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["Question"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Topic"],"name":[2],"type":["chr"],"align":["left"]}],"data":[{"1":"Asthma mortality rate","2":"Asthma"},{"1":"Cancer of the oral cavity and pharynx, mortality","2":"Cancer"},{"1":"Cancer of the prostate, mortality","2":"Cancer"},{"1":"Invasive cancer (all sites combined), mortality","2":"Cancer"},{"1":"Cancer of the female breast, mortality","2":"Cancer"},{"1":"Cancer of the female cervix, mortality","2":"Cancer"},{"1":"Cancer of the colon and rectum (colorectal), mortality","2":"Cancer"},{"1":"Cancer of the lung and bronchus, mortality","2":"Cancer"},{"1":"Melanoma, mortality","2":"Cancer"},{"1":"Mortality with end-stage renal disease","2":"Chronic Kidney Disease"},{"1":"Mortality from heart failure","2":"Cardiovascular Disease"},{"1":"Mortality due to diabetes reported as any listed cause of death","2":"Diabetes"},{"1":"Mortality with diabetic ketoacidosis reported as any listed cause of death","2":"Diabetes"},{"1":"Chronic liver disease mortality","2":"Alcohol"},{"1":"Mortality with chronic obstructive pulmonary disease as underlying cause among adults aged >= 45 years","2":"Chronic Obstructive Pulmonary Disease"},{"1":"Mortality with chronic obstructive pulmonary disease as underlying or contributing cause among adults aged >= 45 years","2":"Chronic Obstructive Pulmonary Disease"},{"1":"Mortality from total cardiovascular diseases","2":"Cardiovascular Disease"},{"1":"Mortality from diseases of the heart","2":"Cardiovascular Disease"},{"1":"Mortality from coronary heart disease","2":"Cardiovascular Disease"},{"1":"Mortality from cerebrovascular disease (stroke)","2":"Cardiovascular Disease"},{"1":"Premature mortality among adults aged 45-64 years","2":"Overarching Conditions"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
# Mortality from chronic diseases is a broad topic
# Focusing in on 'Mortality with end-stage renal disease' for the purpose of this analysis
# Filter for Question is 'Mortality with end-stage renal disease'

cdi_renal <- cdi %>% filter(Question == "Mortality with end-stage renal disease")
```


```r
# explore cdi_renal
glimpse(cdi_renal)
```

```
## Rows: 13,497
## Columns: 34
## $ YearStart                 <int> 2017, 2014, 2012, 2015, 2017, 2011, 2019, 20…
## $ YearEnd                   <int> 2017, 2014, 2012, 2015, 2017, 2011, 2019, 20…
## $ LocationAbbr              <chr> "IA", "OH", "WY", "WY", "AZ", "CT", "IL", "A…
## $ LocationDesc              <chr> "Iowa", "Ohio", "Wyoming", "Wyoming", "Arizo…
## $ DataSource                <chr> "NVSS", "NVSS", "NVSS", "NVSS", "NVSS", "NVS…
## $ Topic                     <chr> "Chronic Kidney Disease", "Chronic Kidney Di…
## $ Question                  <chr> "Mortality with end-stage renal disease", "M…
## $ Response                  <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ DataValueUnit             <chr> "", "", "", "", "", "", "", "", "", "", "", …
## $ DataValueType             <chr> "Number", "Number", "Number", "Number", "Num…
## $ DataValue                 <chr> "1301", "4418", "391", "354", "3554", "1487"…
## $ DataValueAlt              <dbl> 1301, 4418, 391, 354, 3554, 1487, 4275, 161,…
## $ DataValueFootnoteSymbol   <chr> "", "", "", "", "", "", "", "", "", "", "", …
## $ DatavalueFootnote         <chr> "", "", "", "", "", "", "", "", "", "", "", …
## $ LowConfidenceLimit        <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ HighConfidenceLimit       <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ StratificationCategory1   <chr> "Gender", "Gender", "Overall", "Overall", "O…
## $ Stratification1           <chr> "Male", "Male", "Overall", "Overall", "Overa…
## $ StratificationCategory2   <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ Stratification2           <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ StratificationCategory3   <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ Stratification3           <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ GeoLocation               <chr> "POINT (-93.81649055599968 42.46940091300047…
## $ ResponseID                <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ LocationID                <int> 19, 39, 56, 56, 4, 9, 17, 2, 18, 4, 19, 15, …
## $ TopicID                   <chr> "CKD", "CKD", "CKD", "CKD", "CKD", "CKD", "C…
## $ QuestionID                <chr> "CKD1_0", "CKD1_0", "CKD1_0", "CKD1_0", "CKD…
## $ DataValueTypeID           <chr> "NMBR", "NMBR", "NMBR", "NMBR", "NMBR", "NMB…
## $ StratificationCategoryID1 <chr> "GENDER", "GENDER", "OVERALL", "OVERALL", "O…
## $ StratificationID1         <chr> "GENM", "GENM", "OVR", "OVR", "OVR", "GENM",…
## $ StratificationCategoryID2 <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ StratificationID2         <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ StratificationCategoryID3 <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ StratificationID3         <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
```


```r
# How many years of data do we have?
cdi_renal %>% select(YearStart, YearEnd) %>% distinct()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["YearStart"],"name":[1],"type":["int"],"align":["right"]},{"label":["YearEnd"],"name":[2],"type":["int"],"align":["right"]}],"data":[{"1":"2017","2":"2017"},{"1":"2014","2":"2014"},{"1":"2012","2":"2012"},{"1":"2015","2":"2015"},{"1":"2011","2":"2011"},{"1":"2019","2":"2019"},{"1":"2018","2":"2018"},{"1":"2010","2":"2010"},{"1":"2020","2":"2020"},{"1":"2016","2":"2016"},{"1":"2013","2":"2013"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
# How many states are represented in the dataset?
cdi_renal %>% select(LocationDesc) %>% distinct() %>% count()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["n"],"name":[1],"type":["int"],"align":["right"]}],"data":[{"1":"52"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
# What are the unique values for the DataValueType, DataValueUnit
# This column contains the type of data in the dataset
cdi_renal %>% select(DataValueType, DataValueUnit) %>% distinct()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["DataValueType"],"name":[1],"type":["chr"],"align":["left"]},{"label":["DataValueUnit"],"name":[2],"type":["chr"],"align":["left"]}],"data":[{"1":"Number","2":""},{"1":"Age-adjusted Rate","2":"cases per 100,000"},{"1":"Crude Rate","2":"cases per 100,000"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
# What are the unique values for the StratificationCategory1, Stratification1?
# This column contains the stratification category for the data
cdi_renal %>% select(StratificationCategory1, Stratification1) %>% distinct()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["StratificationCategory1"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Stratification1"],"name":[2],"type":["chr"],"align":["left"]}],"data":[{"1":"Gender","2":"Male"},{"1":"Overall","2":"Overall"},{"1":"Gender","2":"Female"},{"1":"Race/Ethnicity","2":"Asian or Pacific Islander"},{"1":"Race/Ethnicity","2":"Black, non-Hispanic"},{"1":"Race/Ethnicity","2":"White, non-Hispanic"},{"1":"Race/Ethnicity","2":"American Indian or Alaska Native"},{"1":"Race/Ethnicity","2":"Hispanic"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
# we want to aggregate by YearStart, DataValueType (filter for Crude Rate and Age-adjusted Rate) and calculate the mean DataValue
# first, lets check if there are any missing values in the DataValue column
cdi_renal %>% filter(is.na(DataValue))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["YearStart"],"name":[1],"type":["int"],"align":["right"]},{"label":["YearEnd"],"name":[2],"type":["int"],"align":["right"]},{"label":["LocationAbbr"],"name":[3],"type":["chr"],"align":["left"]},{"label":["LocationDesc"],"name":[4],"type":["chr"],"align":["left"]},{"label":["DataSource"],"name":[5],"type":["chr"],"align":["left"]},{"label":["Topic"],"name":[6],"type":["chr"],"align":["left"]},{"label":["Question"],"name":[7],"type":["chr"],"align":["left"]},{"label":["Response"],"name":[8],"type":["lgl"],"align":["right"]},{"label":["DataValueUnit"],"name":[9],"type":["chr"],"align":["left"]},{"label":["DataValueType"],"name":[10],"type":["chr"],"align":["left"]},{"label":["DataValue"],"name":[11],"type":["chr"],"align":["left"]},{"label":["DataValueAlt"],"name":[12],"type":["dbl"],"align":["right"]},{"label":["DataValueFootnoteSymbol"],"name":[13],"type":["chr"],"align":["left"]},{"label":["DatavalueFootnote"],"name":[14],"type":["chr"],"align":["left"]},{"label":["LowConfidenceLimit"],"name":[15],"type":["dbl"],"align":["right"]},{"label":["HighConfidenceLimit"],"name":[16],"type":["dbl"],"align":["right"]},{"label":["StratificationCategory1"],"name":[17],"type":["chr"],"align":["left"]},{"label":["Stratification1"],"name":[18],"type":["chr"],"align":["left"]},{"label":["StratificationCategory2"],"name":[19],"type":["lgl"],"align":["right"]},{"label":["Stratification2"],"name":[20],"type":["lgl"],"align":["right"]},{"label":["StratificationCategory3"],"name":[21],"type":["lgl"],"align":["right"]},{"label":["Stratification3"],"name":[22],"type":["lgl"],"align":["right"]},{"label":["GeoLocation"],"name":[23],"type":["chr"],"align":["left"]},{"label":["ResponseID"],"name":[24],"type":["lgl"],"align":["right"]},{"label":["LocationID"],"name":[25],"type":["int"],"align":["right"]},{"label":["TopicID"],"name":[26],"type":["chr"],"align":["left"]},{"label":["QuestionID"],"name":[27],"type":["chr"],"align":["left"]},{"label":["DataValueTypeID"],"name":[28],"type":["chr"],"align":["left"]},{"label":["StratificationCategoryID1"],"name":[29],"type":["chr"],"align":["left"]},{"label":["StratificationID1"],"name":[30],"type":["chr"],"align":["left"]},{"label":["StratificationCategoryID2"],"name":[31],"type":["lgl"],"align":["right"]},{"label":["StratificationID2"],"name":[32],"type":["lgl"],"align":["right"]},{"label":["StratificationCategoryID3"],"name":[33],"type":["lgl"],"align":["right"]},{"label":["StratificationID3"],"name":[34],"type":["lgl"],"align":["right"]}],"data":[],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
# convert DataValue to numeric
cdi_renal <- cdi_renal %>% mutate(DataValue = as.numeric(DataValue))

# convert YearStart to factor for better visualization
cdi_renal <- cdi_renal %>% mutate(YearStart = as.factor(YearStart))

# aggregate by YearStart, LocationAbbr, DataValueType and calculate the mean DataValue
cdi_renal_agg <- cdi_renal %>% 
    filter(!is.na(DataValue)) %>% 
    group_by(YearStart, LocationAbbr, DataValueType) %>% 
    summarise(mean_data_value = mean(DataValue, na.rm = TRUE))
```

```
## `summarise()` has grouped output by 'YearStart', 'LocationAbbr'. You can
## override using the `.groups` argument.
```

```r
head(cdi_renal_agg)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["YearStart"],"name":[1],"type":["fct"],"align":["left"]},{"label":["LocationAbbr"],"name":[2],"type":["chr"],"align":["left"]},{"label":["DataValueType"],"name":[3],"type":["chr"],"align":["left"]},{"label":["mean_data_value"],"name":[4],"type":["dbl"],"align":["right"]}],"data":[{"1":"2010","2":"AK","3":"Age-adjusted Rate","4":"77.36000"},{"1":"2010","2":"AK","3":"Crude Rate","4":"44.24000"},{"1":"2010","2":"AK","3":"Number","4":"170.40000"},{"1":"2010","2":"AL","3":"Age-adjusted Rate","4":"86.06667"},{"1":"2010","2":"AL","3":"Crude Rate","4":"81.88333"},{"1":"2010","2":"AL","3":"Number","4":"2177.00000"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
# with LocationAbbr as rows, YearStart as columns, calculate the mean_data_value for Crude Rate and Age-adjusted Rate
cdi_renal_agg_wide <- cdi_renal_agg %>% 
    spread(YearStart, mean_data_value)

# for each DataValueType, display the mean_data_value for each state over the years
cdi_renal_agg_wide %>% 
    filter(DataValueType == "Crude Rate") %>%
    select(LocationAbbr, `2011`, `2012`, `2013`, `2014`, `2015`, `2016`, `2017`, `2018`)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["LocationAbbr"],"name":[1],"type":["chr"],"align":["left"]},{"label":["2011"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["2012"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["2013"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["2014"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["2015"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["2016"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["2017"],"name":[8],"type":["dbl"],"align":["right"]},{"label":["2018"],"name":[9],"type":["dbl"],"align":["right"]}],"data":[{"1":"AK","2":"47.00000","3":"50.70000","4":"41.76667","5":"34.78000","6":"39.51667","7":"42.48333","8":"40.80000","9":"38.65000"},{"1":"AL","2":"88.43333","3":"83.00000","4":"78.54000","5":"61.93333","6":"72.18000","7":"72.60000","8":"62.20000","9":"68.58333"},{"1":"AR","2":"89.95000","3":"89.85000","4":"80.78000","5":"80.42000","6":"79.22000","7":"86.14000","8":"75.50000","9":"76.75000"},{"1":"AZ","2":"62.57500","3":"64.13750","4":"43.33750","5":"40.78750","6":"45.65000","7":"50.35000","8":"47.66250","9":"46.27500"},{"1":"CA","2":"85.36250","3":"86.90000","4":"57.31250","5":"56.80000","6":"60.90000","7":"63.45000","8":"63.50000","9":"63.38750"},{"1":"CO","2":"52.51250","3":"50.16250","4":"37.70000","5":"40.07500","6":"39.25714","7":"48.83750","8":"58.15000","9":"59.50000"},{"1":"CT","2":"64.75714","3":"62.78571","4":"56.61667","5":"50.51429","6":"52.11429","7":"55.55714","8":"53.55714","9":"53.97143"},{"1":"DC","2":"89.68000","3":"84.26000","4":"45.34000","5":"42.50000","6":"47.02000","7":"50.04000","8":"48.20000","9":"50.88000"},{"1":"DE","2":"95.66000","3":"88.10000","4":"71.12000","5":"76.42000","6":"72.72000","7":"81.74000","8":"81.78000","9":"87.88000"},{"1":"FL","2":"63.91429","3":"64.45714","4":"49.68571","5":"49.07143","6":"51.55714","7":"50.62500","8":"54.18571","9":"52.62500"},{"1":"GA","2":"56.02857","3":"56.84286","4":"45.25714","5":"42.48571","6":"43.90000","7":"43.74286","8":"45.35714","9":"48.14286"},{"1":"HI","2":"79.83333","3":"80.16667","4":"58.60000","5":"56.38333","6":"52.03333","7":"53.86667","8":"57.98333","9":"55.90000"},{"1":"IA","2":"86.82000","3":"79.25000","4":"54.26667","5":"61.30000","6":"55.00000","7":"58.83333","8":"64.86667","9":"56.62857"},{"1":"ID","2":"70.60000","3":"68.45000","4":"45.82000","5":"44.26000","6":"47.68000","7":"54.70000","8":"51.36000","9":"51.72000"},{"1":"IL","2":"68.38571","3":"67.84286","4":"52.84286","5":"52.64286","6":"54.41429","7":"55.35714","8":"55.25714","9":"57.98571"},{"1":"IN","2":"86.30000","3":"88.77143","4":"71.71429","5":"70.28571","6":"74.40000","7":"74.64286","8":"77.47143","9":"77.67143"},{"1":"KS","2":"76.91667","3":"78.05714","4":"59.13333","5":"58.50000","6":"63.93333","7":"61.23750","8":"74.24286","9":"67.17500"},{"1":"KY","2":"110.24000","3":"111.82000","4":"78.28000","5":"86.30000","6":"96.72000","7":"97.80000","8":"99.34000","9":"90.43333"},{"1":"LA","2":"65.65000","3":"63.10000","4":"54.60000","5":"51.28571","6":"51.17143","7":"54.61667","8":"52.15714","9":"60.36667"},{"1":"MA","2":"65.61429","3":"63.91429","4":"50.98571","5":"50.91429","6":"55.95714","7":"52.81429","8":"55.91429","9":"56.21429"},{"1":"MD","2":"66.12857","3":"64.50000","4":"45.81429","5":"45.30000","6":"47.60000","7":"47.04286","8":"53.90000","9":"56.88571"},{"1":"ME","2":"89.27500","3":"95.35000","4":"77.15000","5":"69.17500","6":"74.35000","7":"69.10000","8":"70.42500","9":"80.77500"},{"1":"MI","2":"77.82500","3":"72.38750","4":"52.82500","5":"54.81250","6":"56.61250","7":"58.35000","8":"58.41250","9":"60.12500"},{"1":"MN","2":"74.63750","3":"79.50000","4":"48.07500","5":"50.21250","6":"53.48750","7":"55.20000","8":"59.40000","9":"60.18750"},{"1":"MO","2":"74.81429","3":"74.44286","4":"58.04286","5":"60.87143","6":"61.17143","7":"63.92857","8":"66.12857","9":"67.71429"},{"1":"MS","2":"105.68000","3":"103.44000","4":"74.22000","5":"72.28000","6":"75.56000","7":"73.62000","8":"74.48000","9":"80.54000"},{"1":"MT","2":"69.44000","3":"70.86000","4":"60.18000","5":"62.74000","6":"70.18000","7":"62.02000","8":"65.52000","9":"68.46000"},{"1":"NC","2":"78.36250","3":"79.33750","4":"55.82500","5":"57.90000","6":"58.30000","7":"60.20000","8":"63.63750","9":"62.70000"},{"1":"ND","2":"116.48000","3":"121.10000","4":"85.92000","5":"76.92000","6":"81.14000","7":"80.92000","8":"83.84000","9":"89.40000"},{"1":"NE","2":"94.80000","3":"85.10000","4":"57.33333","5":"61.78333","6":"69.65000","7":"68.31667","8":"74.45000","9":"74.43333"},{"1":"NH","2":"87.65000","3":"85.17500","4":"65.75000","5":"66.67500","6":"68.92500","7":"59.67500","8":"69.97500","9":"75.02500"},{"1":"NJ","2":"77.54286","3":"75.50000","4":"57.11429","5":"56.07143","6":"56.88571","7":"58.18571","8":"60.30000","9":"63.70000"},{"1":"NM","2":"77.27143","3":"79.24286","4":"53.90000","5":"53.11429","6":"59.47143","7":"57.40000","8":"61.32857","9":"64.87143"},{"1":"NV","2":"56.11250","3":"49.38571","4":"40.92857","5":"40.90000","6":"38.81429","7":"43.91429","8":"51.72500","9":"48.51429"},{"1":"NY","2":"58.07500","3":"55.02500","4":"39.56250","5":"39.27500","6":"39.61250","7":"39.33750","8":"40.00000","9":"41.00000"},{"1":"OH","2":"86.55714","3":"87.30000","4":"57.75714","5":"58.57143","6":"59.47143","7":"62.05714","8":"62.45714","9":"60.78571"},{"1":"OK","2":"78.42500","3":"84.86250","4":"65.05714","5":"59.28750","6":"63.48571","7":"61.67500","8":"63.71250","9":"67.00000"},{"1":"OR","2":"68.90000","3":"77.50000","4":"48.10000","5":"50.87500","6":"50.53750","7":"53.16250","8":"59.52500","9":"58.31250"},{"1":"PA","2":"82.07143","3":"82.88571","4":"63.55714","5":"63.25714","6":"66.34286","7":"66.62857","8":"65.70000","9":"67.38571"},{"1":"RI","2":"81.08333","3":"80.95000","4":"63.20000","5":"61.40000","6":"62.51667","7":"65.08333","8":"68.68333","9":"68.25000"},{"1":"SC","2":"79.02857","3":"80.75714","4":"69.46667","5":"71.08333","6":"75.10000","7":"70.85714","8":"73.01429","9":"78.31429"},{"1":"SD","2":"94.98000","3":"96.76000","4":"70.58000","5":"75.76000","6":"77.26000","7":"89.98000","8":"86.50000","9":"94.40000"},{"1":"TN","2":"84.38333","3":"75.22857","4":"57.11429","5":"60.68333","6":"56.41429","7":"56.87143","8":"66.11667","9":"62.30000"},{"1":"TX","2":"68.63750","3":"73.00000","4":"54.50000","5":"55.00000","6":"56.42500","7":"56.98750","8":"61.46250","9":"62.02500"},{"1":"US","2":"84.10000","3":"84.50000","4":"62.30000","5":"62.40000","6":"64.60000","7":"65.30000","8":"67.10000","9":"68.70000"},{"1":"UT","2":"40.70000","3":"39.03333","4":"35.40000","5":"38.65000","6":"38.05000","7":"44.57143","8":"45.81429","9":"45.25714"},{"1":"VA","2":"61.35714","3":"61.54286","4":"47.58571","5":"48.50000","6":"50.01429","7":"51.14286","8":"53.00000","9":"53.12857"},{"1":"VT","2":"95.57500","3":"91.77500","4":"64.30000","5":"61.42500","6":"62.32500","7":"57.82500","8":"55.02500","9":"57.60000"},{"1":"WA","2":"75.53750","3":"75.26250","4":"55.45000","5":"55.00000","6":"57.85000","7":"54.55000","8":"58.88750","9":"56.12500"},{"1":"WI","2":"71.20000","3":"77.45000","4":"56.78750","5":"60.11250","6":"63.26250","7":"65.37500","8":"62.87500","9":"68.75000"},{"1":"WV","2":"139.00000","3":"147.54000","4":"94.58000","5":"98.48000","6":"112.32000","7":"103.10000","8":"95.38000","9":"102.62000"},{"1":"WY","2":"60.28000","3":"62.78000","4":"53.65000","5":"58.80000","6":"55.94000","7":"57.40000","8":"59.54000","9":"66.60000"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
cdi_renal_agg_wide %>% 
    filter(DataValueType == "Age-adjusted Rate") %>%
    select(LocationAbbr, `2011`, `2012`, `2013`, `2014`, `2015`, `2016`, `2017`, `2018`)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["LocationAbbr"],"name":[1],"type":["chr"],"align":["left"]},{"label":["2011"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["2012"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["2013"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["2014"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["2015"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["2016"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["2017"],"name":[8],"type":["dbl"],"align":["right"]},{"label":["2018"],"name":[9],"type":["dbl"],"align":["right"]}],"data":[{"1":"AK","2":"80.06667","3":"79.51667","4":"69.58333","5":"53.18000","6":"58.38333","7":"60.43333","8":"53.76000","9":"51.88333"},{"1":"AL","2":"91.21667","3":"84.00000","4":"73.64000","5":"59.26667","6":"64.58000","7":"64.50000","8":"55.81667","9":"61.60000"},{"1":"AR","2":"94.51667","3":"90.85000","4":"75.66000","5":"73.78000","6":"71.10000","7":"77.56000","8":"71.68333","9":"71.50000"},{"1":"AZ","2":"77.43750","3":"76.07500","4":"49.60000","5":"44.48750","6":"49.03750","7":"52.77500","8":"49.08750","9":"47.01250"},{"1":"CA","2":"93.36250","3":"92.26250","4":"58.93750","5":"56.83750","6":"59.40000","7":"60.61250","8":"59.52500","9":"57.93750"},{"1":"CO","2":"72.55000","3":"69.68750","4":"47.08571","5":"49.87500","6":"48.02857","7":"60.72500","8":"69.00000","9":"70.87500"},{"1":"CT","2":"65.75714","3":"63.61429","4":"52.63333","5":"49.34286","6":"49.14286","7":"52.01429","8":"48.00000","9":"47.65714"},{"1":"DC","2":"90.02000","3":"84.48000","4":"45.28000","5":"43.46000","6":"47.06000","7":"50.90000","8":"47.04000","9":"49.88000"},{"1":"DE","2":"89.88000","3":"81.74000","4":"63.34000","5":"67.02000","6":"61.56000","7":"67.26000","8":"67.86000","9":"70.20000"},{"1":"FL","2":"56.25714","3":"55.78571","4":"41.40000","5":"39.82857","6":"41.30000","7":"39.97500","8":"42.58571","9":"40.20000"},{"1":"GA","2":"72.24286","3":"70.71429","4":"55.32857","5":"50.71429","6":"51.71429","7":"49.80000","8":"50.27143","9":"51.68571"},{"1":"HI","2":"73.88333","3":"76.93333","4":"55.45000","5":"51.00000","6":"51.38333","7":"51.60000","8":"54.03333","9":"49.61667"},{"1":"IA","2":"83.28000","3":"89.11667","4":"55.48333","5":"54.94000","6":"55.01667","7":"58.85000","8":"67.40000","9":"62.31429"},{"1":"ID","2":"85.16667","3":"82.40000","4":"50.06000","5":"45.78000","6":"48.34000","7":"55.64000","8":"51.32000","9":"50.72000"},{"1":"IL","2":"74.74286","3":"72.05714","4":"54.65714","5":"52.64286","6":"53.62857","7":"53.82857","8":"51.37143","9":"53.34286"},{"1":"IN","2":"100.17143","3":"102.14286","4":"79.25714","5":"77.81429","6":"78.25714","7":"81.58571","8":"82.54286","9":"78.24286"},{"1":"KS","2":"88.18333","3":"94.60000","4":"63.06667","5":"61.03333","6":"66.86667","7":"69.47500","8":"79.01429","9":"72.32500"},{"1":"KY","2":"113.60000","3":"113.00000","4":"77.34000","5":"84.50000","6":"93.18000","7":"91.64000","8":"91.64000","9":"84.76667"},{"1":"LA","2":"71.60000","3":"69.34286","4":"56.98333","5":"54.35714","6":"53.58571","7":"53.68333","8":"52.00000","9":"56.28333"},{"1":"MA","2":"70.32857","3":"67.01429","4":"51.57143","5":"50.71429","6":"54.28571","7":"50.50000","8":"53.52857","9":"53.48571"},{"1":"MD","2":"72.22857","3":"68.61429","4":"47.70000","5":"45.81429","6":"47.22857","7":"45.40000","8":"51.10000","9":"52.87143"},{"1":"ME","2":"68.12500","3":"71.10000","4":"55.97500","5":"49.82500","6":"52.80000","7":"48.22500","8":"47.65000","9":"54.00000"},{"1":"MI","2":"87.50000","3":"79.26250","4":"55.06250","5":"57.26250","6":"56.87500","7":"58.70000","8":"57.83750","9":"57.60000"},{"1":"MN","2":"102.52500","3":"109.87500","4":"59.62500","5":"63.48750","6":"68.32500","7":"64.71250","8":"71.28750","9":"71.51250"},{"1":"MO","2":"81.27143","3":"79.58571","4":"60.60000","5":"60.71429","6":"59.65714","7":"63.45714","8":"63.14286","9":"64.25714"},{"1":"MS","2":"108.84000","3":"104.46000","4":"72.46000","5":"69.96000","6":"71.32000","7":"68.76000","8":"68.00000","9":"71.52000"},{"1":"MT","2":"67.56000","3":"72.04000","4":"59.44000","5":"60.94000","6":"69.34000","7":"54.30000","8":"59.06000","9":"61.40000"},{"1":"NC","2":"92.67500","3":"89.00000","4":"61.70000","5":"60.53750","6":"60.51250","7":"60.47500","8":"64.35000","9":"60.97500"},{"1":"ND","2":"118.20000","3":"125.50000","4":"87.58000","5":"82.38000","6":"82.98000","7":"77.74000","8":"83.44000","9":"89.54000"},{"1":"NE","2":"118.44286","3":"94.80000","4":"61.45000","5":"65.88333","6":"76.03333","7":"73.20000","8":"75.91667","9":"73.53333"},{"1":"NH","2":"76.70000","3":"72.70000","4":"54.40000","5":"54.40000","6":"54.47500","7":"46.72500","8":"53.30000","9":"55.35000"},{"1":"NJ","2":"79.42857","3":"75.01429","4":"55.14286","5":"52.94286","6":"52.41429","7":"53.02857","8":"53.31429","9":"55.60000"},{"1":"NM","2":"85.81429","3":"84.55714","4":"55.98571","5":"53.11429","6":"57.58571","7":"55.05714","8":"56.51429","9":"58.87143"},{"1":"NV","2":"68.28750","3":"56.11429","4":"44.11429","5":"43.42857","6":"39.70000","7":"43.12857","8":"50.17500","9":"46.65714"},{"1":"NY","2":"57.67500","3":"53.00000","4":"36.88750","5":"36.51250","6":"36.12500","7":"35.01250","8":"34.26250","9":"33.38571"},{"1":"OH","2":"89.67143","3":"89.17143","4":"56.68571","5":"56.78571","6":"57.91429","7":"59.00000","8":"58.70000","9":"55.25714"},{"1":"OK","2":"97.87500","3":"102.61250","4":"72.97143","5":"69.13750","6":"69.48571","7":"67.71250","8":"69.43750","9":"69.95000"},{"1":"OR","2":"78.82500","3":"90.25000","4":"53.85000","5":"55.96250","6":"52.46250","7":"54.16250","8":"62.18750","9":"61.43750"},{"1":"PA","2":"76.74286","3":"76.85714","4":"58.17143","5":"56.57143","6":"59.37143","7":"58.92857","8":"56.85714","9":"57.40000"},{"1":"RI","2":"75.15000","3":"73.25000","4":"58.55000","5":"53.73333","6":"53.96667","7":"55.83333","8":"57.55000","9":"57.91667"},{"1":"SC","2":"87.85714","3":"87.67143","4":"68.86667","5":"69.96667","6":"72.03333","7":"67.08571","8":"67.64286","9":"71.41429"},{"1":"SD","2":"96.72000","3":"98.30000","4":"77.00000","5":"75.02000","6":"75.54000","7":"90.48000","8":"87.12000","9":"93.92000"},{"1":"TN","2":"92.10000","3":"82.40000","4":"62.12857","5":"61.36667","6":"57.41429","7":"56.81429","8":"63.40000","9":"61.12857"},{"1":"TX","2":"89.01250","3":"93.21250","4":"66.90000","5":"65.67500","6":"66.47500","7":"65.06250","8":"69.32500","9":"68.82500"},{"1":"US","2":"77.10000","3":"76.10000","4":"55.30000","5":"54.60000","6":"55.70000","7":"55.50000","8":"56.20000","9":"56.50000"},{"1":"UT","2":"70.10000","3":"60.86667","4":"53.26667","5":"56.40000","6":"55.26667","7":"65.51429","8":"68.08571","9":"63.64286"},{"1":"VA","2":"68.30000","3":"68.61429","4":"52.11429","5":"50.77143","6":"51.50000","7":"51.24286","8":"52.05714","9":"51.21429"},{"1":"VT","2":"78.60000","3":"74.45000","4":"51.92500","5":"47.37500","6":"46.22500","7":"43.60000","8":"40.55000","9":"42.00000"},{"1":"WA","2":"96.37500","3":"92.70000","4":"67.07500","5":"64.51250","6":"65.53750","7":"60.37500","8":"68.81250","9":"60.57500"},{"1":"WI","2":"89.75000","3":"97.47500","4":"70.02500","5":"70.28750","6":"72.37500","7":"75.76250","8":"68.23750","9":"76.01250"},{"1":"WV","2":"122.82000","3":"129.94000","4":"79.84000","5":"81.22000","6":"93.90000","7":"83.18000","8":"76.46000","9":"80.14000"},{"1":"WY","2":"67.86000","3":"67.72000","4":"50.95000","5":"54.65000","6":"57.40000","7":"56.90000","8":"58.34000","9":"55.65000"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

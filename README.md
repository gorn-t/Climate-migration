# Demographic Panel Data: TVSEP
Date: 2022-10-18


## TVSEP data management (first wave: 2007)

The TVSEP is a panel dataset starting from 2007. Up until present, the datasets include 9 waves: 2007 (base year), 2008, 2010, 2011, 2013, 2016, 2017, 2019, and 2020-21. This panel survey was created to observe rural household vulnerability in Thailand and Vietnam over time. Three additional migrant surveys were created in 2010, 2018, and 2020-21 to extend the information on household members who migrated to Greater Bangkok, Thailand, and to Hanoi and Ho Chi Minh City, Vietnam

```
Relateted libraries
- library(dplyr)
- library(purr)
```

### Household member module of year 2007

``` r 
syntax

member2007<-read.csv("HH_TH_2007memclean.csv", sep =";", header = TRUE, dec = ",", na.strings = "NA") %>%      # import .csv file
        select(QID, X_x21001,X_x10001,X_x10002, X_x10003, X_x21003, X_x21004, X_x21014, X_x21016, X_x21018,
        X_x21019) %>%                                                                                           # select some variables from the data set
        rename(IND.ID = X_x21001, prov = X_x10001, dis = X_x10002, sub.dis = X_x10003, gender = X_x21003,
        age = X_x21004, occu = X_x21014, stay = X_x21016, mig_reason = X_x21018, destination = X_x21019) %>%    # rename variables                                    
        mutate(year = 2007)  %>%                                                                                # add an variable
        transform(age = as.integer(age))                                                                        # convert class(type) of data

```
|        QID| prov|  dis| sub.dis| gender| age| occu| stay| mig_reason| destination| year|
|----------:|----:|----:|-------:|------:|---:|----:|----:|----------:|-----------:|----:|
| 3110080xxx|   31| 3110|  311008|      2|  57|    1|  365|         99|          99| 2007|
| 3102060xxx|   31| 3102|  310206|      2|  18|   10|  365|         99|          99| 2007|
| 3107161xxx|   31| 3107|  310716|      2|  57|   13|  365|         99|          99| 2007|
| 3111101xxx|   31| 3111|  311110|      1|  13|   10|  365|         99|          99| 2007|
| 3123040xxx|   31| 3123|  312304|      1|  27|    5|    0|          4|          29| 2007|
| 3102050xxx|   31| 3102|  310205|      2|  61|    1|  365|         99|          99| 2007|

### Saving module of year 2007

``` r
syntax

saving2007 <-read.csv("HH_TH_2007savclean.csv", sep =";", header = T, dec = ",", na.strings = "NA") %>%              # import .csv file
                      select(QID, X_x71514) %>%                                                                 # select some variables from the data 
                      rename(QID = QID, saving = X_x71514)                                                      # rename variables 
```
|        QID| saving|
|----------:|------:|
| 3432030xxx|    108|
| 3415110xxx|    600|
| 3415160xxx|      6|
| 3401110xxx|     12|
| 3401080xxx|    600|
| 3401080xxx|    852|

```
NOTE
"%>%" operator: pass the result of one function/argument to the other one in sequence.
```

### Merging two modules (household member+saving)

``` r
syntax

merge <-list(member2007, saving2007) %>% reduce(left_join, by = "QID")          # join multiple dataframes

```
|        QID| prov|  dis| sub.dis| gender| age| occu| stay| mig_reason| destination| year| saving|
|----------:|----:|----:|-------:|------:|---:|----:|----:|----------:|-----------:|----:|------:|
| 3110080xxx|   31| 3110|  311008|      2|  57|    1|  365|         99|          99| 2007|  25000|
| 3102060xxx|   31| 3102|  310206|      2|  18|   10|  365|         99|          99| 2007|     NA|
| 3107161xxx|   31| 3107|  310716|      2|  57|   13|  365|         99|          99| 2007| 100000|
| 3111101xxx|   31| 3111|  311110|      1|  13|   10|  365|         99|          99| 2007|  50000|
| 3123040xxx|   31| 3123|  312304|      1|  27|    5|    0|          4|          29| 2007|     NA|
| 3102050xxx|   31| 3102|  310205|      2|  61|    1|  365|         99|          99| 2007|     NA|

### Creating panel data (first & second waves: 2007 & 2008)

``` r 
syntax
panel <- rbind(HH2007, HH2008) %>%                      # add observations rowwise
         arrange(QID, IND.ID, year) %>%                 # orders the rows by the values of selected columns
         distinct()                                     # select only unique/distinct rows from a data frame. In other word, remove repeated observations
```

|        QID| prov|  dis| sub.dis| gender| age| occu| stay| mig_reason| destination| year| saving|
|----------:|----:|----:|-------:|------:|---:|----:|----:|----------:|-----------:|----:|------:|
| 3102060xxx|   31| 3102|  310206|      2|  18|   10|  365|         99|          99| 2007|     NA|
| 3102060xxx|   31| 3102|  310206|      2|  18|   10|  365|         99|          99| 2008|     NA|
| 3107161xxx|   31| 3107|  310716|      2|  57|   13|  365|         99|          99| 2007| 100000|
| 3107161xxx|   31| 3107|  310716|      2|  57|   13|  365|         99|          99| 2008| 500000|
| 3110080xxx|   31| 3110|  311008|      2|  57|    1|  365|         99|          99| 2007|  25000|
| 3110080xxx|   31| 3110|  311008|      2|  57|    1|  365|         99|          99| 2008|  25000|
| 3111101xxx|   31| 3111|  311110|      1|  13|   10|  365|         99|          99| 2007|  50000|
| 3111101xxx|   31| 3111|  311110|      1|  13|   10|  365|         99|          99| 2008|  15000|





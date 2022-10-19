# Demographic Panel Data: TVSEP
Date: 2022-10-18
- Demographic data: TVSEP
- Spatial data: CRU TS version 4.05

## TVSEP data management (first wave: 2007)

The TVSEP is a panel dataset starting from 2007. Up until present, the datasets include 9 waves: 2007 (base year), 2008, 2010, 2011, 2013, 2016, 2017, 2019, and 2020-21. This panel survey was created to observe rural household vulnerability in Thailand and Vietnam over time. Three additional migrant surveys were created in 2010, 2018, and 2020-21 to extend the information on household members who migrated to Greater Bangkok, Thailand, and to Hanoi and Ho Chi Minh City, Vietnam

```
Relateted libraries
- library(dplyr)
```

### Household member module of year 2007

``` r 
syntax

memeber2007<-read.csv("HH_TH_2007memclean.csv", sep =";", header = TRUE, dec = ",", na.strings = "NA") %>%
        select(QID, X_x21001,X_x10001,X_x10002, X_x10003, X_x21003, X_x21004, X_x21014, X_x21016, X_x21018, X_x21019) %>%
        rename(IND.ID = X_x21001, prov = X_x10001, dis = X_x10002, sub.dis = X_x10003, gender = X_x21003, age = X_x21004,
        occu = X_x21014, stay = X_x21016, mig_reason = X_x21018, destination = X_x21019) %>%
        mutate(year = 2007)  %>%
        transform(age = as.integer(age))

```

### Saving module of year 2007

``` r
syntax

saving2007 <-read.csv("HH_2010savraw.csv", sep =";", header = T, dec = ",", na.strings = "NA") %>%
              select(QID, X_x71514) %>% rename(QID = QID, saving = X_x71514) 
```

```
command breakdown

 - %>%: pass the result of one function/argument to the other one in sequence.
 - select: select some variables from the data set
 - rename: rename variables
 - mutate: add an variable
 - transform: convert class(type) of data

```

### Merging two modules (household member+saving)

``` r
syntax

merge <-list(member2007, saving2007) %>% reduce(left_join, by = "QID")

```

### Creating panel data (first & second waves: 2007 & 2008)

``` r 
syntax
panel <- rbind(HH2007, HH2008) %>%
         arrange(QID, IND.ID, year) %>%
         distinct()
```

```
command breakdown

 - rbind: add cases by row
 - distinct: select only unique/distinct rows from a data frame
```



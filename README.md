# **TVSEP data management** 
Date: 2022-10-18


## Import data of year 2007 / select, rename, add variables

- library(dplyr)

```
__syntax__ 

df<-read.csv("HH_TH_2007memclean.csv", sep =";", header = T, dec = ",", na.strings = "NA") %>%
  select(QID, X_x10001, X_x10002, X_x10003) %>%
  rename(province = X_x10001, district = X_x10002, sub.district = X_x10003) %>%
  transform(year = as.integer(2007))
```

```
__comman breakdown__

 - %>%: pass the result of one function/argument to the other one in sequence.
 - select: select some variables from the data set
 - rename: rename variables
 - transform: an variable

```

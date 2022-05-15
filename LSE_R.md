# Master_Thesis_Replication
Replikere FF for å sikre at vi får samme resultater. 

## Install packages
install.packages("data.table")
library(data.table)
install.packages("zoo")
library(zoo)
install.packages("dplyr")
library(dplyr)
install.packages("tidyr")
library(tidyr)


# Market Capitalization 

## 1st cleaning in Excel
## Delete the Error columns in Excel before importing the set to R. 
## This reduces the data set to 7458

library(readxl)
MV_LSE_Raw <- read_excel("Documents/Master thesis/DATA/Ready for R/MV_LSE_Raw.xlsx")

## Change layout of the Dataset using the tidyr package
MV_LSE <- pivot_longer(MV_LSE_Raw, 2:7458, names_to = "Company", values_to = "MV")
MV_LSE <- str_remove(MV_LSE, "^.*(?=(-))") # excluding the -

## Delete the part of the security name to shorten the names 
### We also cut the code PLC to make the data sets comparable they use different endings (use the base?) 
MV_LSE$Company <- gsub(" - MARKET VALUE","",as.character(MV_LSE$Company))


## Sort the findings based on the Company name 
data <- BE_LSE[order(BE_LSE$Company),]





# Book Equity 

## Delete the Error columns in Excel before importing the set to R. 
## This reduces the data set from 8481 variables to 4803

## Import Common Equity (WC03501) from Refinitiv Eikon using Datastream 
library(readxl)
BE_LSE_Raw <- read_excel("Documents/Master thesis/DATA/Ready for R/BE_LSE_Raw.xlsx")

## Change layout of the Dataset using the tidyr package
BE_LSE <- pivot_longer(BE_LSE_Raw, 2:4803, names_to = "Company", values_to = "BE")

## Delete the part of the security name to shorten the names 
### We also cut the code PLC to make the data sets comparable they use different endings (use the base?) 
BE_LSE$Company <- gsub(" PLC - COMMON SHAREHOLDERS' EQUITY","",as.character(BE_LSE$Company))
# How should we do this? 

## Sort the findings based on the Company name 
data <- BE_LSE[order(BE_LSE$Company),]







## Merge the two datasets
data = merge.data.table(LSE_Book_Equity_data1, LSE_Book_Equity_Data2)
data = merge(LSE_Book_Equity_data1, LSE_Book_Equity_Data2, all = TRUE)

# Cleaning
## Delete Errors, NA and 0 from the dataset
index = which(is.na(data$BE))
data = data[-which(is.na(data$BE))]
data = na.omit(data)





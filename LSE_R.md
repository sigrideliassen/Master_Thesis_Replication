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





# Book Equity 

## Delete the Error columns in Excel before importing the set to R. 
## This reduces the dataset from 8481 variables to 4803

## Import Common Equity (WC03501) from Refinitiv Eikon using Datastream 
library(readxl)
BE_LSE_Raw <- read_excel("Documents/Master thesis/DATA/Ready for R/BE_LSE_Raw.xlsx")

## Change layout of the Dataset using the tidyr package
BE_LSE <- pivot_longer(BE_LSE_Raw, 2:4803, names_to = "Company", values_to = "BE")
BE_LSE$Company<-gsub(" - COMMON SHAREHOLDERS' EQUITY","",as.character(BE_LSE$Company))

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





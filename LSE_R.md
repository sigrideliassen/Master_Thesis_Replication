# Master_Thesis_Replication
Replikere FF for å sikre at vi får samme resultater. 

## Import packages needed to conduct the analysis
library(data.table)
library(zoo)
library(dplyr)

## Import Book Equity (WC05491) and Market Cap (MV) from Refinitiv 
library(readxl)
LSE_Book_Equity_data1 <- read_excel("Documents/Master thesis/DATA/LSE Book Equity data1.xlsx")
LSE_Book_Equity_Data2 <- read_excel("Documents/Master thesis/DATA/LSE Book Equity Data2.xlsx")

colnames(LSE_Book_Equity_data1) = c("Date", "Company", "BE")
colnames(LSE_Book_Equity_Data2) = c("Date", "Company", "BE")

## Merge the two datasets
data = merge.data.table(LSE_Book_Equity_data1, LSE_Book_Equity_Data2)
data = merge(LSE_Book_Equity_data1, LSE_Book_Equity_Data2, all = TRUE)

# Cleaning
## Delete Errors, NA and 0 from the dataset
index = which(is.na(data$BE))
data = data[-which(is.na(data$BE))]
data = na.omit(data)





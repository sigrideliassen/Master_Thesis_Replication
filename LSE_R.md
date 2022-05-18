# Master_Thesis_Replication
Replikere FF for å sikre at vi får samme resultater. 

# Install packages needed to conduct the analysis
install.packages("data.table")
install.packages("zoo")
install.packages("dplyr")
install.packages("tidyr")
install.packages("openxlsx")
install.packages("rio")

library(data.table)
library(zoo)
library(dplyr)
library(tidyr)
library(stringr)


# Market Capitalization 

## 1st cleaning in Excel
## Delete the Error columns in Excel before importing the set to R. 
## This reduces the data set to 6814
library(readxl)
LSE_MV_Raw <- read_excel("Documents/Master thesis/DATA/Ready for R/LSE_MV_Raw.xlsx")

## Change layout of the Dataset using the tidyr package
MV_LSE <- pivot_longer(LSE_MV_Raw, 2:6814, names_to = "ID", values_to = "MV")

## Delete the part of the security name to shorten the names (MV Code)
MV_LSE$ID <- gsub("(MV)","",as.character(MV_LSE$ID))
MV_LSE$ID <- gsub("()","",as.character(MV_LSE$ID))
### Not able to delete (). why?

## Sort the findings based on the ID
data1 <- MV_LSE[order(MV_LSE$ID),]



# Common Equity (Book Value)

## 1st cleaning in Excel
## Delete the Error columns in Excel before importing the set to R. 
## This reduces the data set to 4802
library(readxl)
LSE_CE_Raw <- read_excel("Documents/Master thesis/DATA/Ready for R/LSE_CE_Raw.xlsx")

## Change layout of the Dataset using the tidyr package
BV_LSE <- pivot_longer(LSE_CE_Raw, 2:4802, names_to = "ID", values_to = "BV")

## Delete the part of the security name to shorten the names (MV Code)
BV_LSE$ID <- gsub("(WC03501)","",as.character(BV_LSE$ID))
BV_LSE$ID <- gsub("()","",as.character(BV_LSE$ID))
### Not able to delete (). why?

## Sort the findings based on the ID
data2 <- BV_LSE[order(BV_LSE$ID),]

# Delete Errors, NA and 0 from the dataset
data1 = na.omit(data1)
data2 = na.omit(data2)


# Merge the two datasets 
data = merge.data.table(data1, data2) data = merge(data1, data2, all = TRUE)





## Merge the two datasets
data = merge.data.table(LSE_Book_Equity_data1, LSE_Book_Equity_Data2)
data = merge(LSE_Book_Equity_data1, LSE_Book_Equity_Data2, all = TRUE)



#Cancel
MV_LSE$Company <- sub("-.*", "", MV_LSE$Company)




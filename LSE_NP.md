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
## This reduces the data set to 6814 and to 7157 in the extended period
library(readxl)
LSE_MV_Raw <- read_excel("Documents/Master thesis/DATA/Ready for R/LSE_MV_Raw.xlsx")
LSE_MV_NP <- read_excel("Documents/Master thesis/DATA/Ready for R/LSE_MV_NP.xlsx")


## Change layout of the Dataset using the tidyr package
MV_LSE <- pivot_longer(LSE_MV_Raw, 2:6814, names_to = "ID", values_to = "MV")
MV_LSE_NP <- pivot_longer(LSE_MV_NP, 2:7157, names_to = "ID", values_to = "MV")

# Merge the data set with data for the extended period (1990 to 2021)
data1 = merge.data.table(MV_LSE, MV_LSE_NP, by = "ID")
data1 = merge(MV_LSE, MV_LSE_NP, all = TRUE)

## Assign names to the columns 
colnames(data1) = c("Date", "ID", "MV")

## Delete the part of the security name to shorten the names (MV Code)
data1$ID <- gsub("(MV)","",as.character(data1$ID))
data1$ID <- gsub("()","",as.character(data1$ID))
### Not able to delete (). why?

## Sort the findings based on the ID
data1 <- data1[order(data1$ID),]

# Delete Errors, NA and 0 from the dataset
data1 = na.omit(data1)





# Book Value (Equity) defined as Common Equity 

## 1st cleaning in Excel
## Delete the Error columns in Excel before importing the set to R. 
## This reduces the data set to 4802 and to 1779 in the extended period
LSE_CE_Raw <- read_excel("Documents/Master thesis/DATA/Ready for R/LSE_CE_Raw.xlsx")
LSE_BV_NP <- read_excel("Documents/Master thesis/DATA/Ready for R/LSE_BV_NP.xlsx")

## Change layout of the Dataset using the tidyr package
BV_LSE <- pivot_longer(LSE_CE_Raw, 2:4802, names_to = "ID", values_to = "BV")
BV_LSE_NP <- pivot_longer(LSE_BV_NP, 2:1779, names_to = "ID", values_to = "BV")

## Assign names to the columns 
colnames(BV_LSE) = c("Date", "ID", "BV")
colnames(BV_LSE_NP) = c("Date", "ID", "BV")

# Merge the data set with data for the extended period (1990 to 2021)
data2 = merge.data.table(BV_LSE, BV_LSE_NP, by = "ID")
data2 = merge(BV_LSE, BV_LSE_NP, all = TRUE)


## Delete the part of the security name to shorten the names (BV Code)
data2$ID <- gsub("(WC03501)","",as.character(data2$ID))
data2$ID <- gsub("()","",as.character(data2$ID))
### Not able to delete (). why?

## Sort the findings based on the ID
data2 <- data2[order(data2$ID),]

# Delete Errors, NA and 0 from the dataset
data2 = na.omit(data2)



# Merge MV and BV data 
data = merge.data.table(data1, data2, by = "ID")
data = merge(data1, data2, all = TRUE)

# Delete Errors, NA and 0 from the dataset
data = na.omit(data)

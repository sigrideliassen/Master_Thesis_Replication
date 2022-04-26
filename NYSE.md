# Master_Thesis_Replication
Replikere FF for å sikre at vi får samme resultater. 

library(data.table)
library(zoo)
library(dplyr)

# Import CRSP data 
library(readxl)
NYSE = as.data.table(read_excel("Documents/Master/Master Thesis/Data_CRSP_Compustat/NYSE_data.xlsx"))

# Cleaning 
colnames(NYSE) = c("DATE", "SHRCD", "EXCHCD", "TICKER", "DLRET", "PRC", "RET", "SHROUT", "RET_NO_DIV")
NYSE$PRC = abs(NYSE$PRC) # VI MÅ BLI ENIGE OM VI SKAL TA ABS ELLR IKKE!! Tell antall negative priser 

# Replace NA returns with delisted returns 
index = which(!is.na(NYSE$DLRET))
NYSE_DLRET = NYSE$DLRET[index]
NYSE_RET = NYSE$RET
NYSE_RET[NYSE_RET = index] <- NYSE_DLRET
NYSE$RET = NYSE_RET 

# Replace NA prices calculated from delisted returns 
index = which(!is.na(NYSE$DLRET))
NYSE_PRC = NYSE$PRC
NYSE_PRC[NYSE_PRC=index] <- NYSE_PRC[index - 1]*(1 + NYSE$RET[index])
NYSE$PRC = NYSE_PRC 

# NA prices and returns without delisiting returns - DELETING ROWS
index2 = which(is.na(NYSE$RET))
NYSE = NYSE[-which(is.na(NYSE$PRC))]
NYSE = NYSE[-which(is.na(NYSE$RET))]

# Calculating market cap
NYSE = NYSE[,MKTCAP:=abs(PRC)*SHROUT]

x[,mktcap.lag:=shift(mktcap),by=PERMNO]

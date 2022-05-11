# Master_Thesis_Replication
Replikere FF for å sikre at vi får samme resultater. 

## Import packages needed to conduct the analysis
library(data.table)
library(zoo)
library(dplyr)

## Import Book Equity (WC05491) and Market Cap (MV) from Refinitiv 
library(readxl)
Market_Cap_LSE <- read_excel("Documents/Master thesis/DATA/Market Cap LSE.xlsx")
Book_Equity_LSE <- read_excel("Documents/Master thesis/DATA/Book Equity LSE.xlsx")




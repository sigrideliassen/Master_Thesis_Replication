# Master_Thesis_Replication
Replikere FF for å sikre at vi får samme resultater. 



# Master-Thesis-SM
Fama and French 6 factor analysis

### First have to install packages in the terminal 
pip install eikon
pip install configparser
pip install cufflinks

### Import packages
import numpy as np
import pandas as pd
import cufflinks as cf
import configparser as cp

import sys
print(sys.version)

import eikon as ek
ek.__version__

### Eikon personal key
ek.set_app_key('1e658046cec74790884cac68df68d8aa2505b714')


## Retreive stocks from London Stock Exchange
### All companies listed on the exchange using SCREENER --> exchange name + country of exchange + headquarters
### Transport formulas to excel
screener_exp = 'SCREEN(U(IN(Equity(active,public,primary))),IN(TR.ExchangeCountryCode,"GB"),IN(TR.ExchangeMarketIdCode,"XLON"),IN(TR.HQCountryCode,"GB"), CURN=USD)'
instruments = [screener_exp]
fields = ["TR.CommonName","TR.HQCountryCode","TR.CompanyMarketCap"]
LSE, e = ek.get_data(instruments, fields)
LSE


### Retreive historical data for STOXX index (30.06.1990 - 01.01.2015)
#### get time series for close price for the index
RIC = ".STOXX"
stoxx = ek.get_timeseries(RIC, "CLOSE", interval = "monthly", start_date = "1990-06-30", end_date = "2015-12-31")
stoxx

#### get historical data for companies listed on EuroStoxx today for the period 1990 to 2015 monthly (company name and market cap) --> ups. have to include the ticker for dates 
##### Uses the syntax and fields from the extraction in excel 
eurostoxx_cm, err = ek.get_data(instruments=['SCREEN(U(IN(indices(91873767/*SOURCE STOXX EUROPE 600 UCITS ETF*/))), TR.CompanyMarketCap>=TR.CompanyMarketCap, CURN=USD)'],
                      fields = ["TR.CompanyMarketCap","TR.CommonName"],
                      parameters = {"SDate" : "1990-06-30", "EDate" : "2016-12-31", "Frq" : "M"})
eurostoxx_cm


## Include leavers and joiners
### Get consituent on the start date and get joiners and leavers from start date to end date 
jl, e = ek.get_data(".STOXX",
                    ["TR.IndexJLConstituentRIC.Date","TR.IndexJLConstituentRIC.Change","TR.IndexJLConstituentRIC"],
                    {"SDate":"1990-06-30","EDate":"2016-12-31","IC":"B"})
jl






## Include fields we need for portfolio analysis 
### From SCREENER --> find RIC for variables
fields = ["TR.CompanyMarketCap","TR.CommonName"]




### Shows all stocks listed on Eurostoxx per today 
eurostoxx_today = test["Instrument"].to_list()
eurostoxx_today

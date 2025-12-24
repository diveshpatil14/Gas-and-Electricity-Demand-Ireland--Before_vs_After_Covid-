## Gas & Electricity Consumption Analysis (Before and After COVID-19) - APDV (Group Project)


# Ireland Gas Demand Analysis (2019-2020) Before Covid 

#Project Overview

This project builds a full-complete data analysis workflow through Python and MongoDB to analyse Ireland's daily gas consumption.
I have store semi-structured JSON data in a database and done with cleaning and transforming the data as well as create the data visualisations and forecast model.

## Research Question
''How does Ireland's daily gas consumption vary across different customer types from 2019 to 2020 (before covid), and which sectors contributed the most to overall gas demand?''


#Dataset
## My dataset (Semi-structured JSON)
- Source: NGSD02.20251113T141152 JSON dataset
- Clean output file used: 'gas_clean.json'
- Main fields after cleaning:
    - 'date'(YYYY-MM-DD)
    - 'category' (customer type label)
    - 'demand' (daily gas demand in GWh)

> NOTE: The raw JSON was not in JSON array format it was in JSON Line format, so I have transformed into a JSON array before importing into MongoDB.


## Tools And Technologies
- Python (pandas, matplotlib, seaborn, statsmodels)
- MongoDB (NoSQL database)
- Docker (containerised MongoDB)
- Jupyter Notebook (development environment)

## Database Setup (MongoDB in Docker)

### 1) Start MongoDB container
    bash
docker run -d --name energy-mongo -p 27017:27017 -v D:\mongo_data:/data mongo

#Check files inside container

docker exec -it energy-mongo bash
ls /data

# Import cleaned JSON array into MongoDB

mongoimport --db energy_project --collection gas_clean --file /data/gas_clean.json --jsonArray


## MongoDB Database Connection in Python

from pymongo import MongoClient
import pandas as pd                      

gas_client=MongoClient('mongodb://localhost:27017/')  
database=gas_client['energy_project']          
gas_collection= database['gas_clean']  
print('Connected Successfully!')


#count all documents through MongoDB
gas_collection.count_documents({})

# Read MongoDB collection in the pandas DataFrame
raw_gas_data=pd.DataFrame(list(gas_collection.find()))

# Display First five rows
raw_gas_data.head() 



#Data Processing


The following data preprocessing steps were applied:

-After successful connection the id was automatically created, as it was unnecessary column so it was removed.
-Handling missing and duplicate values where required
-Conversion of date columns into datetime format
-From 2019 to 2020 the data was used for visualization as per the research question.
-created columns for more clear understanding of visualization
-saved the cleaned dataset back to the MongoDB .

### Key Analysis and Insights

-Trend of daily gas demand
-Compare customer type means average demand per customer category
-Monthly gas demand trend
-which category contributes the most
-Heatmap category vs month
-Gas consumption by weekday
-Total demand per year
-Category trend line plot
-Forecasting for next 30 days was used with the help of ARIMA model.


























 
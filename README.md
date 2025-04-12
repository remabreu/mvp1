
# Introduction

[Airbnb](https://www.airbnb.com.br/) is a website and app that allows people to rent out their homes and other properties to travelers. It's the world's best-known online marketplace for short-term rentals. It has more than 150 million users worldwide, and hosts over 5 million listings in more than 220 countries and 100,000 cities. 

Airbnb's initiative is to make data from the service available for some of the world's major cities. Through the [Inside Airbnb portal](http://insideairbnb.com/get-the-data.html), it is possible to download a large amount of data to develop Data Science projects and solutions. 



## Data Gathering

The Data collected is from the city of Rio de Janeiro incluinding all listings on 27th of December 2024. Thus, listings can be deleted in the Airbnb platform. so the data presented here is a snapshot of listings available at a particular time.

https://data.insideairbnb.com/brazil/rj/rio-de-janeiro/2024-12-27/data/listings.csv.gz



The Data utilizes public information compiled from the website including the availabiity calendar for 365 days in the future. 



Some hosts might not keep their calendar updated, or have it highly available even though they live in the entire home/apartment. Neighbourhood names for each listing are compiled by comparing the listing's geographic coordinates with a city's definition of neighbourhoods. 



## Objective
When thinking about AirBnB data analyisis, it is important to consider pricing, demand and customer reviews per region and sazonality. This study will not only cover sazonality due to the complexity and size of the calendar bookings dataset. 

All in all, the study will focus on:

- Explore which neighbouhoods in Rio de Janeiro have the highest revenue and more listings available

- Anaylise which type of porperties are the most rented by hosts

- Occupancy in most outstanding regions in Rio de Janeiro

- Analyse some common KPIs (RevPAR and reviews) used in hospitality compared to AirBnb dataset extracted from Rio de Janeiro.
  
  - RevPAR - Revenue per available room - The revenue generated per available room - Formula - *Total room revenue / Total available rooms*. While ADR helps you understand your property’s daily profit, RevPAR helps you understand your property’s ability to fill nights at ADR.
  
  - CSAT (reviews) - Customer Satisfaction Score - On AirBnb scope, it refers on the reviews uses leave on the site (how much it affects - revenue?)
    
    

## Data gathering and Modeling

The original [data](https://data.insideairbnb.com/brazil/rj/rio-de-janeiro/2024-12-27/data/listings.csv.gz) was downloaded as a compressed (gz) file and uncompressed manually on local host to a CSV file. The CSV file was uploaded manually to databrics into the DBFS which represents an internal fole system underneath Databricks capable to store files. Although it is not recommended to use in more complex and professional assignements, it was useful for the scope of  the MVP once it always available to be processes. Ideally, the CVS file (either the gz or the CVS) it could have been uploaded to a S3 bucket in a cloud system and have the file fetched from this location. But for simpliciy, it was chosen DBFS instead. 

The Inside AirBnb project scraps the AirBnb website frequently and releases on every 3 months the data for the last 12 months of property listings and their underlying characteristcs. 

The data is organized in a a flat-file containg 78 columns that corresponds to property listings (including amenities), negihborhood, hosts, stay durantion and reviews. The decision for not loading the flat file into a star model, for example, was due to the ...

The data catalog can be found here. It describes all columns of the flat model.

##Data Loading

Initially the CSV file as converted into a dataframe representing the bronze thier. Since the CVS file has cotes and multiple lines in some of its lines, the following parameters were used to have it properly parsed into the dataframe: 

- `header=True,` 

- `inferSchema=True`, 

- `sep=','`, 

- `quote='"'`, 

- `escape='"'`, 

- `multiLine=True`,

- `encoding='UTF-8'`

The bronze thier represent the data in raw format, as is in the extracted CSV file.

On the silver thier, the data was processed including the following steps:

1. Removeal of columns that will not be used in the assigment - The work will focus mainly on the porperty listings, their location and availability. Hosts and reviews will not be part of the study. 

2. Verification of `NULL` fileds in the most important columns such as `id`, `price`, `availabilty `and `neighbourhood`
   
3. If the `review_scores_accuracy` field is `NULL`, the review is set to 0

4. Casting of the columns that are string to their correponding type to avoid validation errors during analysis phase

5. Specially for `price` column, it contained the "$" symbol and was set as string. The symbol was removed and the type casted to Double.


Analysis

Follows the [publich notebook with Notes](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/1300206978836472/3540346726309256/8624857051788934/latest.html)







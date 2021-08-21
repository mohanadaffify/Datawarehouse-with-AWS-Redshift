#  Create Data warehouse using Amazon Redshift
-------------------------

### Introduction

A music streaming startup, Sparkify, has grown their user base and song database and want to move their processes and data onto the cloud. Their data resides in S3, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

As their data engineer, I am tasked with building an ETL pipeline that extracts their data from S3, stages them in Redshift, and transforms data into a set of dimensional tables for their analytics team to continue finding insights in what songs their users are listening to. 

### Datasets
Datasets used in this project are provided in two public **S3 buckets**. One bucket contains info about songs and artists, the second bucket has info concerning actions done by users (which song are listening, etc.. ). The objects contained in both buckets 
are *JSON* files. 

The **Redshift** service is where data will be ingested and transformed, in fact though `COPY` command we will access to the JSON files inside the buckets and copy their content on our *staging tables*.

### Database Schema
We have two staging tables which *copy* the JSON file inside the  **S3 buckets**.
#### Staging Table 
+ **staging_songs** - info about songs and artists
+ **staging_events** - actions done by users 


I createa a star schema optimized for queries on song play analysis. This includes the following tables:

#### Fact Table 
+ **songplays** - records in event data associated with song plays i.e. records with page NextSong
songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

#### Dimension Tables
+ **users** - users in the app
+ **songs** - songs in music database
+ **artists** - artists in music database
+ **time** - timestamps of records in **songplays** broken down into specific units

The database schema is shown in the figure below 
![schema](./images/db_schema.PNG)

### Data Warehouse Configurations and Setup
* Create an `IAM Role` that makes `Redshift` able to access `S3 bucket` (ReadOnly)
* Create a `security group` to authorize access to Redshift through `EC2` instance 
* Create a `RedShift Cluster` and get the `DWH_ENDPOIN(Host address)` and `DWH_ROLE_ARN` and fill the config file.
* fill the `dwh.cfg` with redshift database user and password. 

### ETL Pipeline
+ Created staging tables to store the data from `S3 buckets`.
+ copied the data from `S3 buckets` to the staging tables in the `Redshift Cluster`.
+ loaded the data from the staging tables into the `Redshift cluster`. 

### Project Structure

+ `create_tables.py` - created the fact and dimension tables for the star schema in `RedShift`
+ `etl.py` - This script load data from S3 into staging tables on Redshift and then process that data into the analytics tables on Redshift.
+ `sql_queries.py` - This file contains variables with SQL statements , which will be imported in `create_tables.py` and `etl.py`.
+ `dhw.cfg` - Configuration file used that contains info about `Redshift`, `IAM` and `S3`. the credentials needed are database username and password , Redshift cluster endpoint and the `ARN` of the IAM Role

### How to Run

1. Create tables by running `create_tables.py`.

2. Execute ETL process by running `etl.py`.








# Project: Data Lake

## Introduction
In this project we will build an ETL pipeline that extracts their data from the data lake hosted on S3, processes them using Spark which will be deployed on an EMR cluster using AWS, and load the data back into S3 as a set of dimensional tables in parquet format. 

Aim of this project is to create an ETL pipeline using the data stored in S3 buckets and process that data into respective Fact and dimentsion tables using spark and then upload the parquet files back to S3.

From this tables we will be able to find insights in what songs their users are listening to.

Prerequisite

1. Install [Python 3.x](https://www.python.org/).
2. Also install **conda** and **pyspark**

Dataset (Public S3 buckets):
Song Data: s3://udacity-dend/song_data 
Log Data Path: s3://udacity-dend/log_data 



## Method

*To run this project in local mode*, create a file `dl.cfg` in the root of this project with the following data:

```
AWS_ACCESS_KEY_ID=YOUR_AWS_ACCESS_KEY
AWS_SECRET_ACCESS_KEY=YOUR_AWS_SECRET_KEY
```

Create an S3 Bucket named `sparkify-outputs` where output results will be stored.

Finally, run the following command:

`python etl.py`



## Project structure

The files found at this project are the following:

- dl.cfg: *not uploaded to github - you need to create this file yourself* File with AWS credentials.
- etl.py: Program that extracts songs and log data from S3, transforms it using Spark, and loads the dimensional tables created in parquet format back to S3.
- README.md: Current file, contains detailed information about the project.

## ETL pipeline

1. Load credentials

2. Read data from S3

   - Song data: `s3://udacity-dend/song_data`
   - Log data: `s3://udacity-dend/log_data`

   The script reads song_data and load_data from S3.

3. Process data using spark

   Transforms them to create five different tables listed under `Dimension Tables and Fact Table`.
   Each table includes the right columns and data types. Duplicates are addressed where appropriate.

4. Load it back to S3

   Writes them to partitioned parquet files in table directories on S3.

   Each of the five tables are written to parquet files in a separate analytics directory on S3. Each table has its own folder within the directory. Songs table files are partitioned by year and then artist. Time table files are partitioned by year and month. Songplays table files are partitioned by year and month.

### Source Data

- **Song datasets**: all json files are nested in subdirectories under *s3a://udacity-dend/song_data*. A sample of this files is:
- **Log datasets**: all json files are nested in subdirectories under *s3a://udacity-dend/log_data*.Dimension Tables and Fact Table
**songplays** - Fact table - records in log data associated with song plays i.e. records with page NextSong



### Schema 

A Star Schema would be required for optimized queries on song play queries

<b>Fact Table</b>

<b>songplays_table</b> - records in event data associated with song plays i.e. records with page NextSong songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

<b>Dimension Tables</b>

<b>users_schema_table</b> - users in the app user_id, first_name, last_name, gender, level

<b>songs_schema_table</b> - songs in music database song_id, title, artist_id, year, duration

<b>artists_schema_table</b> - artists in music database artist_id, name, location, lattitude, longitude

<b>time_schema_table</b> - timestamps of records in songplays broken down into specific units start_time, hour, day, week, month, year, weekday
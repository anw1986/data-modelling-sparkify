# Project: Data Modeling with Postgres
A startup named Sparkify wants to analyze user activities using their song and user data. The current data is spread among several JSON files, making it hard to query and analyze.

This project aims to create an ETL pipeline to load song and user data to a Postgres database, making it easier to query and analyze data.

Data is currently collected for song and user activities, in two directories: data/log_data and data/song_data, using JSON files.

### Song dataset format

```json
{
  "num_songs": 1,
  "artist_id": "ARGSJW91187B9B1D6B",
  "artist_latitude": 35.21962,
  "artist_longitude": -80.01955,
  "artist_location": "North Carolina",
  "artist_name": "JennyAnyKind",
  "song_id": "SOQHXMF12AB0182363",
  "title": "Young Boy Blues",
  "duration": 218.77506,
  "year": 0
}
```

### Log dataset format

```json
{
  "artist": "Survivor",
  "auth": "Logged In",
  "firstName": "Jayden",
  "gender": "M",
  "itemInSession": 0,
  "lastName": "Fox",
  "length": 245.36771,
  "level": "free",
  "location": "New Orleans-Metairie, LA",
  "method": "PUT",
  "page": "NextSong",
  "registration": 1541033612796,
  "sessionId": 100,
  "song": "Eye Of The Tiger",
  "status": 200,
  "ts": 1541110994796,
  "userAgent": "\"Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.143 Safari/537.36\"",
  "userId": "101"
}
```

## Schema

Star schemna using Fact and dimension tables is utilized. A primary purpose of star schema is to simplify a complex normalized set of tables and consolidate data (possibly from different systems) into one database structure that can be queried in a very efficient way.

### Fact tables

It contains all the primary keys of the dimension and associated facts or measures(is a property on which calculations can be made) like quantity sold, amount sold and average sales.

#### Songplays

Records in log data associated with song plays i.e. records with `page` set to `NextSong`.

|   Column    |  Type   |
| ----------- | --------|
| songplay_id | INT     |
| start_time  | VARCHAR |
| user_id     | INT     |
| level       | VARCHAR |
| song_id     | INT     |
| artist_id   | INT     | 
| session_id  | INT     |
| location    | VARCHAR | 
| user_agent  | VARCHAR | 

Primary key: songplay_id

### Dimension tables

Dimension tables provides descriptive information for all the measurements recorded in fact table.

#### Users

Users in the app.

|   Column   |  Type   | 
| ---------- | ------- | 
| user_id    | INT     | 
| first_name | VARCHAR | 
| last_name  | VARCHAR | 
| gender     | VARCHAR | 
| level      | VARCHAR | 

Primary key: user_id

#### Songs

Songs in music database.

|  Column   |  Type   | 
| --------- | ------- | 
| song_id   | VARCHAR | 
| title     | VARCHAR | 
| artist_id | VARCHAR | 
| year      | INT     | 
| duration  | FLOAT   | 

Primary key: song_id

#### Artists

Artists in music database.

|  Column   |  Type      |  
| --------- | ---------- | 
| artist_id | VARCHAR    | 
| name      | VARCHAR    | 
| location  | VARCHAR    | 
| latitude  | FLOAT      | 
| longitude | FLOAT      | 

Primary key: artist_id

#### Time

Timestamps of records in songplays broken down into specific units.

|   Column   |  Type     | 
| ---------- | --------- | 
| start_time | VARCHAR   | 
| hour       | VARCHAR   | 
| day        | VARCHAR   | 
| week       | VARCHAR   | 
| month      | VARCHAR   | 
| year       | VARCHAR   | 
| weekday    | VARCHAR   | 

## Build

Pre-requisites:

- Python 3
- pipenv
- pyenv (optional)
- PostgreSQL Database

To install project python dependencies, you should run:

``` sh
pipenv install
```

## Database

The database can be installed locally or ran using Docker, which is the
preferred method.

To use docker to run Postgres, you should run:

``` sh
docker run --net=host --name postgres -e POSTGRES_PASSWORD=your_password -d postgres
```

### Access and user setup

To initially access the database, you should run:

``` sh
psql -h localhost -U postgres
```

You should run the following commands under psql to setup user access to
Postgres and create the initial `sparkifydb` database:

``` sql
CREATE ROLE student WITH ENCRYPTED PASSWORD 'student';
ALTER ROLE student WITH LOGIN;
ALTER ROLE student CREATEDB;
CREATE DATABASE sparkifydb OWNER student;
```

## Running

To run the project locally, use pipenv to activate the virtual environment:

``` sh
pipenv shell
```

And run the scripts to create database tables:

``` sh
./create_tables.py
```

and populate data into tables:

``` sh
./etl.py
```

Data can be verified using the provided `test.ipynb` jupyter notebook:

``` sh
jupyter notebook
```
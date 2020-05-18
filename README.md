# This project is part of Udacity's Nanodegree program "Data Engineering"

# Purpose

The purpose of this project is to support Sparkify by creating a database which enables them to perform an in-depth songplay analysis. The data source are various json files which make it hard to analyse and track user activities.

# Data sources

Source data comes in json files of the following formats:

## log_data (user attributes and activities like song plays)
```json
{ "artist":"N.E.R.D. FEATURING MALICE",
  "auth":"Logged In",
  "firstName":"Jayden",
  "gender":"M",
  "itemInSession":0,
  "lastName":"Fox",
  "length":288.9922,
  "level":"free",
  "location":"New Orleans-Metairie, LA",
  "method":"PUT","page":"NextSong",
  "registration":1541033612796.0,
  "sessionId":184,"song":"Am I High (Feat. Malice)",
  "status":200,
  "ts":1541121934796,
  "userAgent":"\"Mozilla\/5.0 (Windows NT 6.3; WOW64) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/36.0.1985.143 Safari\/537.36\"",
  "userId":"101"
}
```

## song_data (known songs and their artists)
```json
{ "num_songs": 1, 
  "artist_id": "ARMJAGH1187FB546F3", 
  "artist_latitude": 35.14968, 
  "artist_longitude": -90.04892, 
  "artist_location": "Memphis, TN", 
  "artist_name": "The Box Tops", 
  "song_id": "SOCIWDW12A8C13D406", 
  "title": "Soul Deep", 
  "duration": 148.03546, 
  "year": 1969
}
```

# Schema

We follow a star-schema with songplay table being the fact table. This enables us to run performant queries and we are also able to set up meaningful queries with ease.

## Fact table

### songplays (extracted from log files and linked to artists and songs)

|   Column    |  Type     |  Property   |
| ----------- | --------- | ----------- |
| songplay_id | integer   | Primary key |
| start_time  | timestamp |             |
| user_id     | integer   | Not Null    |
| level       | varchar   |             |
| song_id     | varchar   |             |
| artist_id   | varchar   |             |
| session_id  | integer   |             |
| location    | varchar   |             |
| user_agent  | varchar   |             |

## Dimension tables

### users (users in database)

|   Column    |  Type     |  Property   |
| ----------- | --------- | ----------- |
| user_id     | integer   | Primary key |
| first_name  | varchar   |             |
| last_name   | varchar   |             |
| gender      | varchar   |             |
| level       | varchar   |             |

### songs (known songs)

|   Column    |  Type     |  Property   |
| ----------- | --------- | ----------- |
| song_id     | varchar   | Primary key |
| title       | varchar   |             |
| artist_id   | varchar   | Not Null    |
| year        | integer   |             |
| duration    | numeric   |             |

### artists (known artists)

|   Column    |  Type     |  Property   |
| ----------- | --------- | ----------- |
| artist_id   | varchar   | Primary key |
| name        | varchar   |             |
| location    | varchar   |             |
| latitude    | numeric   |             |
| longitude   | numeric   |             |

### time (timestamps of song plays split into different units)

|   Column    |  Type     |  Property   |
| ----------- | --------- | ----------- |
| start_time  | timestamp | Primary key |
| hour        | integer   |             |
| day         | integer   |             |
| week        | integer   |             |
| month       | integer   |             |
| year        | integer   |             |
| weekday     | integer   |             |

# How to run

Open a python console and call
``` 
run create_tables.py
```
to create the db tables,
then populate the data into tables:
``` 
run etl.py
```

# Files in repository
data                | data sources (json files)
create_tables.ipnyb | functions to initially create (and drop) tables
etl.ipynb           | draft and toy examples of etl processes to transform the data within the data sources into the db schema
etl.py              | etl processes to transform the data within the data sources into the db schema
README.md           | this file
sql_queries.py      | SQL statements to drop, create tables and insert data
test.ipnyb          | Calls which are directly executed on the postgres database to see if tables were created and updated correctly
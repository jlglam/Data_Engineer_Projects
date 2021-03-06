# Data Modeling with Postgres

## Overview
In this project, we build an ETL pipeline to load the song and log dataset (in JSON format) into a postgres database `sparkifydb` to help a music streaming app startup **Sparkify**'s analytics team understand what songs users are listening to.

## Dataset
### Song Dataset
A subset of real data from the [Million Song Dataset](https://labrosa.ee.columbia.edu/millionsong/).  

File path example:  

```song_data/A/B/C/TRABCEI128F424C983.json```
```song_data/A/A/B/TRAABJL12903CDCF1A.json```

File content example:

```{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}```

### Log Dataset
Log files generated by [event simulator](https://github.com/Interana/eventsim) based on the songs in the **Song Dataset**. 

File path example:   

```log_data/2018/11/2018-11-12-events.json```
```log_data/2018/11/2018-11-13-events.json```

File content example:

```{"artist": null, "auth": "Logged In", "firstName": "Walter", "gender": "M", "itemInSession": 0, "lastName": "Frye", "length": null, "level": "free", "location": "San Francisco-Oakland-Hayward, CA", "method": "GET","page": "Home", "registration": 1540919166796.0, "sessionId": 38, "song": null, "status": 200, "ts": 1541105830796, "userAgent": "\"Mozilla\/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/36.0.1985.143 Safari\/537.36\"", "userId": "39"}```

## Database schema
Designed using the star schema optimized for queries on song play analysis, it consists of the following tables:   

### Fact Table

**songplays** - records in log data associated with song plays. 
 
```songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent```

### Dimension Tables

**users** - users in the app.  

```user_id, first_name, last_name, gender, level```

**songs** - songs in music database.  

```song_id, title, artist_id, year, duration```

**artists** - artists in music database.  

```artist_id, name, location, latitude, longitude```

**time** - timestamps of records in songplays broken down into specific units.  

```start_time, hour, day, week, month, year, weekday```

## ETL pipeline
1. **sql_query.py**: templates for dropping and creating fact and all the dimension tables. It also has insertion templates and song select query.
2. **create_tables.py**: responsible for creating the **sparkifydb** database and create all the tables using the templates from **sql_query.py**
3. **etl.py**: read and process the **Song Dataset** and **Log Dataset** and insert them into the **sparkifydb** database

## Environment set up
1. ```cd``` into ```Data_Modeling_with_Postgres``` directory
2. Set up the virtual environment using the following command: ```python -m venv env```
3. Activate the virtual environment using the following command: ```source env/bin/activate```
4. Install all the required python packages using the following command: ```pip install -r requirement.txt```

## How to run
1. Run ```python create_tables.py``` to drop and create the fact and dimision tables.
2. Run ```python etl.py``` to perform the main ETL process.
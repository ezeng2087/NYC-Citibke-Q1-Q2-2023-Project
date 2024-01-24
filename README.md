# Citibike Project

## Description

This [dataset](https://data.cityofnewyork.us/Environment/Air-Quality/c3uy-2p5r) provides information related to air quality, including various indicators measured across different geographic areas and time periods within NYC.

### Columns


| Column              | Description                                     |
|---------------------|-------------------------------------------------|
| `ride_id`           | Unique identifier for each ride.                |
| `rideable_type`     | Type of ride, e.g., "classic_bike" or "electric_bike". |
| `started_at`        | Date and time when the ride started.            |
| `ended_at`          | Date and time when the ride ended.              |
| `start_station_name`| Name of the starting station.                   |
| `start_station_id`  | Identifier for the starting station.            |
| `end_station_name`  | Name of the ending station.                     |
| `end_station_id`    | Identifier for the ending station.              |
| `start_lat`         | Latitude of the starting location.              |
| `start_lng`         | Longitude of the starting location.             |
| `end_lat`           | Latitude of the ending location.                |
| `end_lng`           | Longitude of the ending location.               |
| `member_casual`     | Indicates if the rider is a member or casual. 

# Excel

Importing the 6 separate files into a single, consolidated file,Citibike data.csv
- JC-202301-citibike-tripdata
- JC-202302-citibike-tripdata
- JC-202303-citibike-tripdata
- JC-202304-citibike-tripdata
- JC-202305-citibike-tripdata
- JC-202306-citibike-tripdata

Checking if there is any duplicated values within the Unique identifier(ride_id)


![d686ae3ed8b35df16ca56faab015073d](https://github.com/ezeng2087/Air-Quality-NYC-Project/assets/154565917/2ebcc7fd-69d4-46f4-a981-1cca6562233f)

Checking if there is any blank cells within the dataset

![1cfef7dec77022b8ac0e008c60730a7d](https://github.com/ezeng2087/Air-Quality-NYC-Project/assets/154565917/e9e76daf-9608-45a8-9178-e15be1783bfd)



# PostgreSQL

Creating a table within PostgreSQL, matching the structure of the data above.
```
CREATE TABLE citibikes_data (
    ride_id VARCHAR(50) PRIMARY KEY,
    rideable_type VARCHAR(50) NOT NULL,
    started_at TIMESTAMP NOT NULL,
    ended_at TIMESTAMP NOT NULL,
    start_station_name VARCHAR(255) NOT NULL,
    start_station_id VARCHAR(50) NOT NULL,
    end_station_name VARCHAR(255) NOT NULL,
    end_station_id VARCHAR(50) NOT NULL,
    start_lat DOUBLE PRECISION NOT NULL,
    start_lng DOUBLE PRECISION NOT NULL,
    end_lat DOUBLE PRECISION NOT NULL,
    end_lng DOUBLE PRECISION NOT NULL,
    member_casual VARCHAR(50) NOT NULL
);
```
Creating tripduration column to calculate the time spent(ended_at - started_at) with typecast
```
ALTER TABLE citibikes_data
ADD COLUMN tripduration INT;

UPDATE citibikes_data
SET tripduration = EXTRACT(EPOCH FROM (ended_at - started_at))::INT;
```

Creating another column to differentiate the members(Regular Member or Casual Member) from member_casual column

```
ALTER TABLE citibikes_data
ADD COLUMN member_type VARCHAR(50);

UPDATE citibikes_data
SET member_type = 
  CASE 
    WHEN member_casual = 'member' THEN 'Regular Member'
    WHEN member_casual = 'casual' THEN 'Casual Member'
    ELSE 'Unknown'
  END;
```

Creating another column to differentiate the bike type(Classic Bike or Electric Bike) from rideable_type column

```
ALTER TABLE citibikes_data
ADD COLUMN member_type VARCHAR(50);

UPDATE citibikes_data
SET bike_type = 
  CASE 
    WHEN rideable_type = 'classic_bike' THEN 'Classic Bike'
    WHEN rideable_type = 'electric_bike' THEN 'Electric Bike'
    ELSE 'Unknown'
  END;
```

# Tableau

Fields after connection made with final_citibike.csv

| Field Name             | Physical Table     | Remote Field Name |
|------------------------|--------------------|-------------------|
| Ride Id                | final_citibike.csv | ride_id           |
| Rideable Type          | final_citibike.csv | rideable_type     |
| Started At             | final_citibike.csv | started_at        |
| Ended At               | final_citibike.csv | ended_at          |
| Start Station Name     | final_citibike.csv | start_station_name|
| Start Station Id       | final_citibike.csv | start_station_id  |
| End Station Name       | final_citibike.csv | end_station_name  |
| End Station Id         | final_citibike.csv | end_station_id    |
| Start Station Latitude | final_citibike.csv | start_lat         |
| Start Station Longitude| final_citibike.csv | start_lng         |
| End Station Latitude   | final_citibike.csv | end_lat           |
| End Station Longitude  | final_citibike.csv | end_lng           |
| Member Casual          | final_citibike.csv | member_casual     |
| Member Type            | final_citibike.csv | member_type       |
| Tripduration           | final_citibike.csv | tripduration      |
| Bike Type              | final_citibike.csv | bike_type         |




## Overview

DRAFT  WIP

This repository is a simplified subset of files in repository `SensorProject/Garage`).
It has the essential files for displaying sensor data in a Rhino model.
The purpose is to illustrate data display and to explore the file organization.

Two steps of the data flow in `SensorProject` are illustrated:

1/ Building the SQLite database with example sensor data and
   sensor locations extracted from a Rhino `.3dm` file.

2/ Selecting data from the database and displaying it with `Grasshopper` in 
   a Rhino `.3dm` model.


### Tools Needed

- `Rhino` (tested with v8).
- `Grasshopper` plugin for `Rhino`.
- `SQLite3`.
- `python3` and python module `rhino3dm`.

`Rhino` is needed for the final display but is not need to build the database and do
python or sql queries and testing. 
`Rhino`  requires a licence and does not run on Linux. Other programs, including the python
module `rhino3dm`, are freely available for most OSs and many are preinstalled on 
MacOS and Linux system.



### Database

Files used to build the database are as follows:
- An example of sensor readings data is in file `testData.csv`. 
  This file has temperature and humidity readings identified with the sensor id.
  It has been processed from the data transmitted from wall modules that have many sensors
  plugged into sockets. That original data is identified by a module id and socket number.
  See SensorProject/Garage for the complete process.
- The sensor locations are recorded in a (Rhino) `3dm` file, for example, 
  `slab_sensors.3dm` or `garage_sensors.3dm`.
  Locations are extracted by python program `utils/extract3dmSensorLocations` and 
  written to file `sensorLocations.txt`.
- `SensorIdHash.txt` provides the mapping from module id and socket number
  to sensors id. It has already been used to create file `testData.csv` but is here so that the script 
- `ModuleIdHash.txt` is unused in Rhino but is here so that the script to .


0/ Before building the database setup the python invironment. 
In a shell (MacOS terminal) run

```
python3 -m venv  Rhino3dm
source Rhino3dm/bin/activate  # activate python environment
pip install rhino3dm          # install rhino3dm in the python environment
```

The process for building the database is as follows:

1/ To build the (SQLite) database  SensorReadings.db run

- Load `testData.csv` into the database (table `SensorData`).

```
   source Rhino3dm/bin/activate  # activate python environment if not already active
   utils/loadReadings --infile='testData.csv' --outdb=SensorReadings.db
```
This may indicates 1 bad line in 10,000.

- Extract sensor locations from (Rhino) `3dm` and write to file `sensorLocations.txt`.
```
   utils/extract3dmSensorLocations garage_sensors.3dm >sensorLocations.txt
or  older
   utils/extract3dmSensorLocations slab_sensors.3dm  >sensorLocations.txt
```

- Load sensor details (id, location, module id, module socket number)  into 
     the database (table `Sensors`) and the module descriptions into 
     the target database (table `Modules`).

```
   utils/loadSensors   --sensorLocations='sensorLocations.txt'  \
         --SensorIdHash='SensorIdHash.txt' \
         --ModuleIdHash='ModuleIdHash.txt' --outdb=SensorReadings.db
```

2/ Run some tests with `sqlite3` to check things have loaded properly:

```
   sqlite3  SensorReadings.db

      SELECT  COUNT(*) FROM sensorData;          -- should show 9999 of 10000 records
      SELECT  min(timeStamp) FROM sensorData;  
      SELECT  max(timeStamp) FROM sensorData;  

      SELECT  COUNT(*) FROM sensorData 
          WHERE (timeStamp > '2026-02-08 20:50:00')
            AND (timeStamp < '2026-02-08 21:00:00') ;

      SELECT  COUNT(*) FROM sensors;                          -- 141 sensors
      SELECT COUNT(DISTINCT(sensorData.id)) FROM SensorData ; -- sample uses only 91

      SELECT COUNT(DISTINCT(sensorData.id)) FROM SensorData 
         INNER JOIN Sensors ON sensorData.id = Sensors.id  
         WHERE  40. < temperature ;                           -- 7 sensors likely faulty

    .exit
```

... to be continued


### Data Display

Currently, displaying the data requires Rhino 8, the building model file `Garage/slab_sensors.3dm`,
the `Grasshopper` script `Garage/slab_sensor_Vis.ghx` and `python` code 
`extractReadingsSlice.py` which must be loaded into the `Grasshopper` script.


To be continued...

## License



## License

Licensed under either of

 * Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or
   http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or
   http://opensource.org/licenses/MIT)

at your option.

## Contributing

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall
be dual licensed as above, without any additional terms or conditions.

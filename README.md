## Overview

DRAFT  WIP

This repository is a simplified subset of files in repository `SensorProject/Garage`).
It has the essential files for displaying sensor data in a Rhino model.
The purpose is to illustrate data display and to explore the file organization.

Two steps of the data flow in `SensorProject` are illustrated:

1/ Building the SQLite database with example sensor data and
   sensor locations extracted from a Rhino `.3dm` file.

2/ Selecting data from the database and displaying it with `Grasshopper` in 
   the Rhino `.3dm` model.

### Database

Files used to build the database are as follows:
- An example of sensor readings data is in `testData.csv`. 
  This recorded data has temperature and humidity readings identified with the sensor id.
  It has been processed from the data transmitted from  wall module where many sensors are
  plugged into sockets. Those readings are identified by a module id and socket number.
  See SensorProject/Garage for the complete process.
- The sensor locations are recorded in a (Rhino) `3dm` file, for example, 
  `slab_sensors.3dm` or `garage_sensors.3dm`.
  Locations are extracted by python program `utils/extract3dmSensorLocations` and 
  written to file `sensorLocations.txt`.
- `SensorIdHash.txt` provides the mapping from module id and socket number
  to sensors id.
- `ModuleIdHash.txt` is unused in Rhino but is here so that the script to .

The process for building the database is as follows:


1/ To build the (SQLite) database  SensorReadings.db run

```
 utils/simpleBuildDB  slab_sensors.3dm
```

The  script does the following:

- The `testData.csv` is loaded into the target database (table `SensorData`).

- The sensor locations are extracted from (Rhino) `3dm` file by `python` 
      program `extract3dmSensorLocations` and written to file `sensorLocations.txt`.

- The sensor details (id, location, module id, module socket number) are loaded into 
     the target database (table `Sensors`) and the module descriptions are loaded into 
     the target database (table `Modules`).


2/ Then some tests to check things have loaded properly:
...


### Data Display

Currently, displaying the data requires Rhino 8, the building model file `Garage/slab_sensors.3dm`,
the `Grasshopper` script `Garage/slab_sensor_Vis.ghx` and `python` code 
`extractReadingsSlice.py` which must be loaded into the `Grasshopper` script.


To be continued...


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

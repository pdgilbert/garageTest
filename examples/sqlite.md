Below are more sqlite example queries, without the output.

```
sqlite3  SensorReadings.db

.tables

SELECT  COUNT(*) FROM sensorData;          -- show number of data records
SELECT  min(timeStamp) FROM sensorData;  
SELECT  max(timeStamp) FROM sensorData;  

SELECT printf('Number of sensors %i', COUNT(*)) FROM Sensors;
SELECT COUNT(DISTINCT(SensorData.id)) FROM SensorData ; -- sample uses only 91


SELECT COUNT(DISTINCT(sensorData.id)) FROM SensorData 
   INNER JOIN Sensors ON sensorData.id = Sensors.id  
   WHERE  40. < temperature ;                           -- 7 sensors likely faulty


SELECT printf('Number of distinct timeStamps in sample: %.i', COUNT(DISTINCT timeStamp ))
  FROM sensorData;


SELECT printf('Number of distinct timeStamps in subsample: %.i', COUNT(DISTINCT timeStamp ))
  FROM sensorData 
    WHERE (timeStamp > '2026-02-08 20:50:00')
      AND (timeStamp < '2026-02-12 21:00:00') ;

SELECT printf('Distinct sensor IDs in sensorData: %i',  COUNT(DISTINCT id )) FROM sensorData;

SELECT printf('Distinct sensor IDs in sensorData subsample: %i',  COUNT(DISTINCT id )) FROM sensorData 
    WHERE (timeStamp > '2026-02-08 20:50:00')
      AND (timeStamp < '2026-02-08 21:00:00') ;

SELECT printf('Number of distinct sensors reporting in subsample: %i', COUNT(*)) FROM sensorData 
    INNER JOIN Sensors ON sensorData.id = Sensors.id  
    WHERE (timeStamp > '2026-02-08 20:50:00')
      AND (timeStamp < '2026-02-08 21:00:00') ;
    

SELECT sensorData.id, timeStamp, temperature, x, y, z FROM sensorData 
  INNER JOIN Sensors ON sensorData.id = Sensors.id  
    WHERE (timeStamp > '2026-02-08 20:50:00')
      AND (timeStamp < '2026-02-08 21:00:00') ;

SELECT printf('min temperature %.2f', MIN(temperature)) FROM sensorData 
    WHERE (timeStamp > '2026-02-08 20:50:00')
      AND (timeStamp < '2026-02-08 21:00:00') ;

SELECT printf('max temperature %.2f', MAX(temperature)) FROM sensorData 
    WHERE (timeStamp > '2026-02-08 20:50:00')
      AND (timeStamp < '2026-02-08 21:00:00') ;



SELECT  printf('COUNT(*) %i',  COUNT(*)) FROM sensorData 
       WHERE (timeStamp > '2026-01-03 00:12:00')
         AND (timeStamp < '2026-01-03 00:14:00') ;


   

SELECT sensorData.id, timeStamp, temperature, x, y, z FROM sensorData 
    INNER JOIN Sensors ON sensorData.id = Sensors.id  
       WHERE (timeStamp > '2026-01-03 00:12:00')
         AND (timeStamp < '2026-01-03 00:14:00') 
         AND (-15.0 < z ) AND (z < 0.0) ;

SELECT  COUNT(*)  FROM Sensors WHERE  (-15.0 < z ) AND (z < -10.0) ;

SELECT   printf('z = -12.5 : %i',  COUNT(DISTINCT(sensorData.id))) FROM sensorData 
    INNER JOIN Sensors ON sensorData.id = Sensors.id  
       WHERE (timeStamp > '2026-01-01 00:12:00')
         AND (timeStamp < '2026-01-03 00:14:00') 
         AND (-12.6 < z ) AND (z < -12.4) ;

SELECT  printf('z = -10.125 : %i',  COUNT(DISTINCT(sensorData.id))) FROM sensorData 
    INNER JOIN Sensors ON sensorData.id = Sensors.id  
       WHERE (timeStamp > '2026-01-01 00:12:00')
         AND (timeStamp < '2026-01-03 00:14:00') 
         AND (-10.13 < z ) AND (z < -10.12) ;

SELECT printf('z = -3. : %i',   COUNT(DISTINCT(sensorData.id))) FROM sensorData 
    INNER JOIN Sensors ON sensorData.id = Sensors.id  
       WHERE (timeStamp > '2026-01-01 00:12:00')
         AND (timeStamp < '2026-01-03 00:14:00') 
         AND (-3.1 < z ) AND (z < -2.9) ;

PRAGMA table_info(Sensors);

SELECT * FROM Sensors;
SELECT printf('COUNT(*) %i', COUNT(*)) FROM Sensors;

SELECT COUNT(*) FROM Sensors WHERE modID IS NOT NULL ;
 
SELECT printf('COUNT(*) %i', COUNT(*)) FROM Sensors 
       WHERE (-12.6 < z ) AND (z < -12.4) ;
 
SELECT * FROM Sensors
       WHERE (-12.6 < z ) AND (z < -12.4) ;
  

SELECT COUNT(DISTINCT(sensorData.id)) FROM SensorData 
    INNER JOIN Sensors ON sensorData.id = Sensors.id  
       WHERE 40. > temperature ;

SELECT sensorData.id, timeStamp, temperature, x, y, z FROM sensorData 
    INNER JOIN Sensors ON sensorData.id = Sensors.id  
       WHERE (timeStamp > '2026-01-03 00:12:00')
         AND (timeStamp < '2026-01-03 00:14:00') 
         AND (-15.0 < z ) AND (z < 0.0) ;

SELECT printf('z < -10.0 COUNT(*) %i',  COUNT(*))  FROM Sensors 
       WHERE  (-15.0 < z ) AND (z < -10.0) ;

SELECT   printf('z = -12.5 : %i',  COUNT(DISTINCT(sensorData.id))) FROM sensorData 
    INNER JOIN Sensors ON sensorData.id = Sensors.id  
       WHERE (timeStamp > '2026-01-01 00:12:00')
         AND (timeStamp < '2026-01-03 00:14:00') 
         AND (-12.6 < z ) AND (z < -12.4) ;

SELECT  printf('z = -10.125 : %i',  COUNT(DISTINCT(sensorData.id))) FROM sensorData 
    INNER JOIN Sensors ON sensorData.id = Sensors.id  
       WHERE (timeStamp > '2026-01-01 00:12:00')
         AND (timeStamp < '2026-01-03 00:14:00') 
         AND (-10.13 < z ) AND (z < -10.12) ;

SELECT printf('z = -3. : %i',   COUNT(DISTINCT(sensorData.id))) FROM sensorData 
    INNER JOIN Sensors ON sensorData.id = Sensors.id  
       WHERE (timeStamp > '2026-01-01 00:12:00')
         AND (timeStamp < '2026-01-03 00:14:00') 
         AND (-3.1 < z ) AND (z < -2.9) ;


    .exit
```


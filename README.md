HEALTH ANALYTICS MINI CASE
----------------------------------------------------------------
1)HOW MANY UNIQUE USERS EXIST IN THE LOGS DATASET
SELECT
  COUNT(DISTINCT user)
FROM health.user_logs;
_________________________________________________________________
B)
DROP TABLE IF EXISTS user_measure_count;

CREATE TEMP TABLE user_measure_count AS
SELECT
   id,
   COUNT(*) AS measure_count,
   COUNT(DISTINCT measure) AS unique_measures
FROM health.user_logs
GROUP BY id;
---------------------------------------------------------------
2) HOW MANY TOTAL MEASUREMENTS DO WE HAVE PER USER ON AVERAGE
SELECT
  ROUND(AVG(measure_count))
FROM user_measure_count;
3) WHAT ABOUT THE MEDIAN AMOUNT OF MEASUREMENT PER USER?
   SELECT 
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY 100) AS median_value
FROM user_measure_count;
------------------------------------------------------------------
#4) HOW MANY USERS HAVE 3 OR MORE MEASUREMENTS
   SELECT
  measure, COUNT(*)
FROM user_measure_count
GROUP BY measure
HAVING measure >= 3;
------------------------------------------------------------------
5) HOW MANY USERS HAVE 1000 OR MORE MEASUREMENTS
   SELECT
  COUNT(id)
FROM user_measure_count
WHERE measure_count >= 1000;
--------------------------------------------------------------------
6) HAVE LOGGED BLOOD GLUCOSE MEASUREMENTS
   SELECT
  COUNT(DISTINCT id)
FROM health.user_logs
WHERE measure = 'blood_sugar';
---------------------------------------------------------------------
8) HAVE AT LEAST 2 TYPES OF MEASUREMENT?
SELECT
  COUNT(*)
FROM user_measure_count
WHERE unique_measures = 3;
-----------------------------------------------------------------------
9) HAVE ALL 3 MEASURES -BLOOD GLUCOSE, WEIGHT AND BLOOD PRESSURE
   SELECT
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY systolic) AS median_systolic,
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY diastolic) AS median_diastolic
FROM health.user_logs
WHERE measure = 'blood_pressure';
------------------------------------------------------------------------------


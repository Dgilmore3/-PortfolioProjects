SELECT *
FROM `my-project-378602.Cars.Cars_info`
WHERE fuel_type IS NULL

SELECT   
MIN(length) AS min_length,   
MAX(length) AS max_length 
FROM
`my-project-378602.Cars.Cars_info`
SELECT *
FROM `my-project-378602.Cars.Cars_info`
WHERE make IS NULL

SELECT *
FROM `my-project-378602.Cars.Cars_info`
WHERE body_style IS NULL

SELECT *
FROM `my-project-378602.Cars.Cars_info`
WHERE drive_wheels IS NULL

SELECT *
FROM `my-project-378602.Cars.Cars_info`
WHERE num_of_doors IS NULL

UPDATE
  ``my-project-378602.Cars.Cars_info`
SET
  num_of_doors = "four"
WHERE
  make = "dodge"
  AND fuel_type = "gas"
  AND body_style = "sedan";

SELECT
  *
FROM
  `test-369801.Cars.Cars_info`

WHERE 

  num_of_doors IS NULL;

UPDATE
  `test-369801.Cars.Cars_info`
SET
  num_of_doors = "four"
WHERE
  make = "mazda"
  AND fuel_type = "disel"
  AND body_style = "sedan";

SELECT
  DISTINCT num_of_cylinders
FROM
  `my-project-378602.Cars.Cars_info`
There are two entries for two cylinders: rows 6 and 7. But the two in row 7 is misspelled. 
UPDATE
  `my-project-378602.Cars.Cars_info`
SET
  num_of_cylinders = "two"
WHERE
  num_of_cylinders = "tow";
see if it worked
SELECT
  DISTINCT num_of_cylinders
FROM
  `my-project-378602.Cars.Cars_info`
Next, you can check the compression_ratio column. According to the data description, the compression_ratio column values should range from 7 to 23. Just like when you checked the length values , you can use MIN and MAX to check if that’s correct: 

SELECT
  MIN(compression_ratio) AS min_compression_ratio,
  MAX(compression_ratio) AS max_compression_ratio
FROM
  `my-project-378602.Cars.Cars_info`


Notice that this returns a maximum of 70. But you know this is an error because the maximum value in this column should be 23, not 70. So the 70 is most likely a 7.0. Run the above query again without the row with 70 to make sure that the rest of the values fall within the expected range of 7 to 23.
SELECT
  MIN(compression_ratio) AS min_compression_ratio,
  MAX(compression_ratio) AS max_compression_ratio
FROM
  `my-project-378602.Cars.Cars_info`

WHERE

  compression_ratio <> 70;
Now the highest value is 23, which aligns with the data description. So you’ll want to correct the 70 value. You check with the sales manager again, who says that this row was made in error and should be removed. Before you delete anything, you should check to see how many rows contain this erroneous value as a precaution so that you don’t end up deleting 50% of your data. If there are too many (for instance, 20% of your rows have the incorrect 70 value), then you would want to check back in with the sales manager to inquire if these should be deleted or if the 70 should be updated to another value. Use the query below to count how many rows you would be deleting:

SELECT

   COUNT(*) AS num_of_rows_to_delete

FROM

  `my-project-378602.Cars.Cars_info`

WHERE

   compression_ratio = 70;
Turns out there is only one row with the erroneous 70 value. So you can delete that row using this query:  
DELETE `my-project-378602.Cars.Cars_info`

WHERE compression_ratio = 70;

Question 1
 
Now the highest value is 23, which aligns with the data description. So you’ll want to correct the 70 value. You check with the sales manager again, who says that this row was made in error and should be removed. Before you delete anything, you should check to see how many rows contain this erroneous value as a precaution so that you don’t end up deleting 50% of your data. If there are too many (for instance, 20% of your rows have the incorrect 70 value), then you would want to check back in with the sales manager to inquire if these should be deleted or if the 70 should be updated to another value. Use the query below to count how many rows you would be deleting:
Finally, you want to check your data for any inconsistencies that might cause errors. These inconsistencies can be tricky to spot — sometimes even something as simple as an extra space can cause a problem.
Check the drive_wheels column for inconsistencies by running a query with a SELECT DISTINCT statement: 
SELECT
  DISTINCT drive_wheels
FROM
  `my-project-378602.Cars.Cars_info`
It appears that 4wd appears twice in results. However, because you used a SELECT DISTINCT statement to return unique values, this probably means there’s an extra space in one of the 4wd entries that makes it different from the other 4wd.
To check if this is the case, you can use a LENGTH statement to determine the length of how long each of these string variables: 

SELECT
  DISTINCT drive_wheels,
  LENGTH(drive_wheels) AS string_length
FROM
  `my-project-378602.Cars.Cars_info`
According to these results, some instances of the 4wd string have four characters instead of the expected three (4wd has 3 characters). In that case, you can use the TRIM function to remove all extra spaces in the drive_wheels column if you are using the BigQuery free trial: 
UPDATE
`my-project-378602.Cars.Cars_info`
SET drive_wheels = TRIM(drive_wheels)
WHERE TRUE;
Then, you run the SELECT DISTINCT statement again to ensure that there are only three distinct values in the drive_wheels column: 
 SELECT
  DISTINCT drive_wheels
FROM
  `my-project-378602.Cars.Cars_info`

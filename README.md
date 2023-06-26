# Case study


We want to know the __countries that don't host a megapolis__


The purpose of this exercice is to evaluate your skills in Python and SQL. You'll have to fork this repository and write a program that fetch the [dataset of the population of all cities in the world](https://public.opendatasoft.com/explore/dataset/geonames-all-cities-with-a-population-1000/export/?disjunctive.cou_name_en), stores it in a database, then perform a query that will compute: what are the countries that don't host a megapoliss (a city of more than 10,000,000 inhabitants)? 

The program will save the result (country code and country name) as a tabulated separated value file, ordered by country name. 

You should answer as if you were writting production code within your team. You can imagine that the program will be run automatically every week to update the resulting data.

Please send us the link to your github repository with the answer of the exercise. 

# the codes i used to fetch the data :
-- Create the cities table if it doesn't exist
CREATE TABLE IF NOT EXISTS cities (
    geoname_id INTEGER,
    name VARCHAR(500),
    ascii_name VARCHAR(500),
    alternate_names VARCHAR(3000),  -- Increased length to accommodate longer values
    feature_class CHAR(1),
    feature_code VARCHAR(10),
    country_code VARCHAR(2),
    country_name_en VARCHAR(500),
    country_code_2 VARCHAR(5),
    admin1_code VARCHAR(20),
    admin2_code VARCHAR(80),
    admin3_code VARCHAR(20),
    admin4_code VARCHAR(20),
    population BIGINT,
    elevation INTEGER,
    digital_elevation_model INTEGER,
    timezone VARCHAR(40),
    modification_date DATE,
    label_en VARCHAR(200),
    coordinates VARCHAR(100)
);

-- Truncate the existing data in the cities table
TRUNCATE TABLE cities;

-- Fetch the data from the URL and insert it into the cities table
COPY cities FROM PROGRAM 'curl "https://public.opendatasoft.com/explore/dataset/geonames-all-cities-with-a-population-1000/download/?format=csv&timezone=Europe/Berlin&lang=en&use_labels_for_header=true&csv_separator=%3B"' DELIMITER ';' CSV HEADER;

SELECT country_code, country_name_en
FROM cities
WHERE population <= 10000000
GROUP BY country_code, country_name_en
ORDER BY country_name_en;

COPY cities TO '/Users/hager/Downloads/file.csv' DELIMITER ';' CSV HEADER;




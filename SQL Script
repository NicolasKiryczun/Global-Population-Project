/*
===============================
Global Populations Project - SQL 
===============================
*/
 
          
 /*
 This dataset shows the Country, Country Code, Year, and Population for every country on Earth. 
 The range for the years is 10,000 BCE to 2023, but the population is only counted every year starting in 1800.
 The project consists of two parts: data cleaning then exploratory data analysis.
 
 I plan to answer these questions with my analysis:
 Are there any areas of the world with high growth rates compared to the mean? How much so?
 
 I will use this to find a region to build my dashboard on.
 
 There are many use cases for this information:
 - A governmental agency surveying the geopolitical and demographic landscape to make better informed decisions.
 - Companies looking to expand globally can choose where to invest resources into new markets.
 - Helping investors identify emerging markets with high potential.
 */


/*
===============================
START: Data Cleaning
===============================
*/


-- Create a new, cleaner version of the table
-- Create a new duplicate table titled "population_clean"

CREATE TABLE population_clean AS
SELECT * FROM population;


-- Rename columns for clarity and accessibility

ALTER TABLE population_clean
RENAME COLUMN Entity TO Country;

ALTER TABLE population_clean
RENAME COLUMN `Population` TO `Population_Size`;


-- There are only 195 countries, but this table shows 271 "Countries". We need to make this 196 so total country averages aren't skewed 

SELECT COUNT(DISTINCT Country) 
FROM population_clean;


-- Remove all continents (Select is done first before deleting to make sure no relevant countries accidentally get deleted)

DELETE FROM population_clean
	WHERE Country LIKE '%Europe%' 
	OR Country LIKE 'Africa%'
	OR Country LIKE '%Asia%'
	OR Country LIKE '%America%'
	OR Country LIKE '%Oceania%'


-- Remove other miscellaneous non-countries with independent patterns

DELETE FROM population_clean
	WHERE (Country LIKE '%(%' AND Country NOT LIKE '%(country)')
	OR Country LIKE '%America%'
	OR Country LIKE '%World%'
	OR Country LIKE '%income%';

-- Remove remaining non-countries (These did not have obvious patterns independent of real countries)

DELETE FROM population_clean
WHERE Country IN 
	('Czechoslovakia', 'East Germany', 'Netherlands Antilles', 'Serbia and Montenegro', 'USSR', 
	'West Germany','Yemen Arab Republic', 'Yugoslavia','Akrotiri and Dhekelia', 
	'American Samoa', 'Anguilla', 'Aruba', 'Bermuda','Bonaire Sint Eustatius and Saba', 'British Virgin Islands', 
	'Cayman Islands','Cook Islands', 'Curacao', 'Falkland Islands', 'Faroe Islands', 'French Guiana', 
	'French Polynesia', 'Gibraltar', 'Greenland', 'Guadeloupe', 'Guam', 'Guernsey','Hong Kong', 
	'Isle of Man', 'Jersey', 'Macao', 'Martinique', 'Mayotte', 'Montserrat','New Caledonia', 'Niue', 
	'Northern Mariana Islands', 'Puerto Rico','Reunion', 'Saint Barthelemy', 'Saint Helena', 
	'Saint Martin (French part)','Saint Pierre and Miquelon', 'Sint Maarten (Dutch part)', 'Svalbard and Jan Mayen',
	'Tokelau', 'Turks and Caicos Islands', 'United States Virgin Islands','Wallis and Futuna', 
	'Western Sahara', 'Vatican');


-- Create a new table so that we can reference our newly cleaned table with data for all years at our discretion

CREATE TABLE pop_1800_2023 AS
SELECT * FROM population_clean;


-- The data has yearly data for nearly every country starting from 1800 onwards.
-- Delete all data for years before 1800

DELETE FROM pop_1800_2023
WHERE Year < 1800;


-- Create a VIEW for country populations in the years 1800, 2000, and 2023

CREATE VIEW v_pop_1800 AS (
	SELECT * FROM pop_1800_2023 WHERE YEAR = 1800);

CREATE VIEW v_pop_2000 AS (
	SELECT * FROM pop_1800_2023 WHERE YEAR = 2000);

CREATE VIEW v_pop_2023 AS (
	SELECT * FROM pop_1800_2023 WHERE YEAR = 2023);





/*
===============================
START: Exploratory Analysis
===============================
*/


-- After looking through the cleaned and transformed datasets, here are the patterns I have found:


-- Select the top five countries with the lowest annual growth rates from the years 1800 - 2023
-- (Add a 2023 population threshold of five million to exclude microstate outliers)

SELECT 
	v_1800.Country, 
	v_1800.Year AS starting_year,
	v_2023.Year AS ending_year,
	ROUND(
		(POWER(
			(v_2023.Population_Size / v_1800.Population_Size), 1 / 223) - 1) * 100, 2) AS yearly_growth_percentage
FROM v_pop_1800 AS v_1800
LEFT JOIN v_pop_2023 AS v_2023 
ON v_1800.Country = v_2023.Country
WHERE v_2023.Population_Size > 5000000
ORDER BY yearly_growth_percentage
LIMIT 5;


/*
#1. Ireland (0.00% CAGR) 
#2. Czechia (0.30% CAGR) 
#3. France (0.37% CAGR)
#4. Slovakia (0.43% CAGR) 
#5. Hungary (0.49% CAGR)
*/

-- The list is dominated by European countries, with three of the five being located in Central Europe. 
-- Ireland's population has not grown over 200+ years.
-- Contributing factors include emigration from crises such as the Irish Famine, both world wars, and general political instability.
-- This slow growth could also be seen in the near term due to developed countries producing older populations and lowering birth rates. 


-- Select the top five countries with the highest annual growth rates from the years 2000 - 2023
-- (Add a 2023 population threshold of five million to exclude microstate outliers)

SELECT 
	v_2000.Country, 
	v_2000.Year AS starting_year,
	v_2023.Year AS ending_year,
	ROUND(
		(POWER(
			(v_2023.Population_Size / v_2000.Population_Size), 1 / 23) - 1) * 100, 2) AS yearly_growth_percentage
FROM v_pop_2000 AS v_2000
LEFT JOIN v_pop_2023 AS v_2023 
ON v_2000.Country = v_2023.Country
WHERE v_2023.Population_Size > 5000000
ORDER BY yearly_growth_percentage DESC
LIMIT 5;


/*
#1. United Arab Emirates (4.96% CAGR) 
#2. Niger (3.63% CAGR) 
#3. Chad (3.63% CAGR)
#4. Angola (3.63% CAGR) 
#5. Oman (3.77% CAGR)
*/

-- Three of the top five countries are located in Africa. 
-- This recent spike in growth rates likely has to do with improved technology for 
-- lower mortality rates, while still having high birth rates for developing countries. 

-- Two of the five countries, the UAE and Oman, are located in the Arabian Peninsula, a very particular part of the world.
-- Select the above query WITHOUT a population threshold

SELECT 
	v_2000.Country, 
	v_2000.Year AS starting_year,
	v_2023.Year AS ending_year,
	ROUND(
		(POWER(
			(v_2023.Population_Size / v_2000.Population_Size), 1 / 23) - 1) * 100, 2) AS yearly_growth_percentage
FROM v_pop_2000 AS v_2000
LEFT JOIN v_pop_2023 AS v_2023 
ON v_2000.Country = v_2023.Country
ORDER BY yearly_growth_percentage DESC
LIMIT 5;

/*
#1. Qatar (6.88% CAGR) 
#2. United Arab Emirates (4.96% CAGR) 
#3. Equatorial Guinea (4.29% CAGR)
#4. Kuwait (4.02% CAGR) 
#5. Bahrain (3.77% CAGR)
*/

-- Four of the five countries are located in the Arabian Peninsula. 
-- More specifically, these four countries are all located on the Western side of the Persian Gulf. 
-- Why is this small spot the home for almost all of the fastest-growing countries in the 21st century? 

-- The reason for this is likely due to a large growth of an expatriate population in these countries. 
-- Many citizens from Egypt, India, Pakistan, and other countries are emigrating to Arab countries for employment.
-- The Arabian Peninsula has been a hotbed for infrastructural development in the 21st century, creating an increasing demand for a large workforce.

-- Select the growth rate of each country, along with the average growth rate of all countries

WITH growth_rate_2000_2023 AS 
(
SELECT 
	v_2000.Country, 
	v_2000.Year AS starting_year,
	v_2023.Year AS ending_year,
	ROUND(
		(POWER(
			(v_2023.Population_Size / v_2000.Population_Size), 1 / 23) - 1) * 100, 2) AS yearly_growth_percentage
FROM v_pop_2000 AS v_2000
LEFT JOIN v_pop_2023 AS v_2023 
ON v_2000.Country = v_2023.Country
ORDER BY yearly_growth_percentage DESC
)
SELECT 
	Country, 
	yearly_growth_percentage AS growth_rate,
	ROUND(AVG(yearly_growth_percentage), 2) OVER() AS average_growth_rate
FROM growth_rate_2000_2023;


-- These four Arabian countries have a growth rate 3 - 4x larger than the global average



/*
===============================
START: Tableau Data Preparation
===============================
*/

-- Create a "master table" which will be converted from SQL to CSV to Excel, then uploaded onto Tableau for data visualizations

CREATE TABLE master_population_data AS 
(
SELECT
    a.Country AS Country,
    a.Year AS year,
    a.Population_Size AS population,
    b.Year AS trailing_year,
    b.Population_Size AS trailing_population,
    (a.Population_Size / b.Population_Size) - 1 AS population_growth_rate
FROM 
	global_population.pop_1800_2023 a
JOIN
    global_population.pop_1800_2023 b
    ON a.Country = b.Country AND a.Year = b.Year + 1
WHERE a.year >= 2000
);

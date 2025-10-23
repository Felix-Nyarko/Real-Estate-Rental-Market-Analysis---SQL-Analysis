# Real-Estate-Rental-Market-Analysis---SQL-Analysis
A data analytics project using SQL to explore and analyze a house rentals dataset. The project focuses on querying property data to uncover insights such as rental price trends, regional comparisons, property conditions, and amenities distribution
# Introduction
# Background
## The questions I wanted to answer through my SQL queries were;
Property Level Analysis
1. List the Top 10 most expensive properties with their name,price, bedrooms and location
2. Find the average rental price per bedroom count
3. Retrieve the properties with more than 3 bathrooms and sort them by price (descending)
4. Which category (flats, detached, etc) has the highest average floor area?
5. Show the average rental price per furnishing type (is furnished)

Location and Region Insights

6. Find te average rent by region
7. Get the top 5 localities with the highest average rental prices
8. How many properties are available in each region?
9. Find the most expensive property in Greater Accra
10. Which localities in Greater Accra have average prices above the overall dataset average?

Trends and Conditions

11. Count how many New Vrs Used properties are in the dataset
12. Find the average price per square meter (price/floor_area) for each region
13. How many properties have parking space compared to those without?
14. Which amenities appear most frequently across listings? (requires string search/processing)

# Tools I Used
I harnessed the power of several key tools;
- SQL
- My SQL Workbench
- GITHUB

# The Analysis
Each query for this project aimed at investigating specific aspects of the data analyst job market. Here's how I approached each question;

Property Level Analysis

### 1. The Top 10 most expensive properties with their name,price, bedrooms and location
This SQL query retrieves the top 10 most expensive properties from the dataset, displaying each property's name, price, number of bedrooms, and location. It helps identify high-value listings and gives insights into luxury housing trends within the market.
```sql
SELECT
	name,
	price,
	bedrooms,
	location
FROM
ghana_real_estate_rentals.house_rentals
order by price desc
LIMIT 10;
```

## 2. The Average Rental Price Per Bedroom count
This SQL query calculates the average rental price for each bedroom count across all properties. It helps reveal how rental prices scale with the number of bedrooms.
```sql
select
round(AVG(price),2) as avg_rental_price,
bedrooms
FROM
ghana_real_estate_rentals.house_rentals
group by bedrooms
order by bedrooms;
```

## 3. Retrieve the properties with more than 3 bathrooms and sort them by price (descending)
This SQL query retrieves all properties that have more than three bathrooms and orders them by price in descending order. It provides insight where larger and more premium properties often feature multiple bathrooms and command higher rental or sale prices.
```sql
SELECT
name,
bedrooms,
location,
price,
bathrooms
FROM
ghana_real_estate_rentals.house_rentals
WHERE
bathrooms > 3
order by price desc;
```

## 4.  Which category (flats, detached, etc) has the highest average floor area?
```sql
SELECT
category,
AVG(floor_area) AS average_floor_area
FROM
ghana_real_estate_rentals.house_rentals
GROUP BY
category
ORDER BY
average_floor_area DESC
LIMIT 1;
```

## 5. Show the average rental price per furnishing type (is furnished)
```sql
select
is_furnished,
round(AVG(price),2) as avg_rental_price
FROM
ghana_real_estate_rentals.house_rentals
group by
is_furnished
order by
avg_rental_price;
```
Location and Region Insights

## 6. Find the average rent by region
```sql
SELECT
region,
round(avg(price),2) as average_rental
FROM 
ghana_real_estate_rentals.house_rentals
group by
region
order by
average_rental desc;
```

## 7. Get the top 5 localities with the highest average rental prices
```sql
SELECT
locality,
avg(price) as average_rental_price
FROM
ghana_real_estate_rentals.house_rentals
group by
locality
order by
average_rental_price desc
LIMIT 5;
```

## 8. How many properties are available in each region?
```sql
select 
region,
count(*) as total_properties
from
ghana_real_estate_rentals.house_rentals
group by
region
order by
total_properties desc;
```

## 9. Find the most expensive property in Greater Accra
This SQL query identifies the highest-priced property listed in the Greater Accra region. It filters the dataset by region, sorts properties by price in descending order, and retrieves the top record. The goal is to highlight premium real estate listings and understand pricing dynamics within one of Ghana’s most active property markets.
```sql
SELECT 
    name, 
    price, 
    location
FROM 
    ghana_real_estate_rentals.house_rentals
WHERE 
    region = 'Greater Accra'
ORDER BY 
    price DESC
LIMIT 1;
```

## 10. Which localities in Greater Accra have average prices above the overall dataset average?
```sql
SELECT 
    locality,
    AVG(price) AS average_price
FROM 
   ghana_real_estate_rentals.house_rentals
WHERE 
    region = 'Greater Accra'
GROUP BY 
    locality
HAVING 
    AVG(price) > (SELECT AVG(price) FROM ghana_real_estate_rentals.house_rentals)
ORDER BY 
    average_price DESC;
```

TRENDS AND CONDITIONS

## 11. Count how category are in the dataset
```sql
SELECT 
    category,
    count(*) AS total_properties
FROM 
   ghana_real_estate_rentals.house_rentals
GROUP BY 
    category;
```

## 12. Find the average price per square meter (price/floor_area) for each region
```sql
SELECT
	region,
 round(AVG(price/floor_area), 2) as average_price_per_square_meter
FROM
	ghana_real_estate_rentals.house_rentals
group by
	region
order by
	average_price_per_square_meter desc;
```
## 13.  How many properties have parking space compared to those without?
```sql
   SELECT 
    parking_space, 
    COUNT(*) AS total_properties
FROM 
   ghana_real_estate_rentals.house_rentals
GROUP BY 
   parking_space;
```

## 14. Which amenities appear most frequently across listings? (requires string search/processing)
```sql
SELECT 
    TRIM(SUBSTRING_INDEX(SUBSTRING_INDEX(amenities, ',', n.n), ',', -1)) AS amenity,
    COUNT(*) AS frequency
FROM 
    ghana_real_estate_rentals.house_rentals
JOIN 
    (
        SELECT 1 AS n UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 
        UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9 UNION SELECT 10
    ) n
ON 
    CHAR_LENGTH(amenities) - CHAR_LENGTH(REPLACE(amenities, ',', '')) >= n.n - 1
GROUP BY 
    amenity
ORDER BY 
    frequency DESC;
```

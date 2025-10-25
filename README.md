# Introduction
This project explores a real estate rentals dataset using SQL to uncover insights into property prices, regional trends, and market dynamics. By performing structured queries, I analyzed factors such as average rent by region, rental price trends, regional comparisons, property type distribution, pricing per square meter, and amenity frequency and distribution. 

The goal of this project is to demonstrate how SQL can be used to transform raw housing data into meaningful insights that support data-driven decision-making in the real estate industry. As a real estate executive with a growing focus on data analytics, this analysis bridges my domain expertise with technical skills in data exploration and reporting.

# Background
The real estate industry generates vast amounts of data — from property listings and rental prices to amenities and regional characteristics. However, without proper analysis, these data points remain underutilized.
This project was developed to bridge that gap by applying **SQL-based data analytics** to a housing rentals dataset. The goal was to extract meaningful insights about property distribution, pricing behavior, and market trends. As a **real estate executive** transitioning into data analytics, this project reflects my interest in leveraging data to make smarter decisions in property marketing, pricing, and investment strategy.

## The questions I wanted to answer through my SQL queries were;
# Property Level Analysis
1. List the Top 10 most expensive properties with their name,price, bedrooms and location
2. Find the average rental price per bedroom count
3. Retrieve the properties with more than 3 bathrooms and sort them by price (descending)
4. Which category (flats, detached, etc) has the highest average floor area?
5. Show the average rental price per furnishing type (is furnished)

# Location and Region Insights

6. Find te average rent by region
7. Get the top 5 localities with the highest average rental prices
8. How many properties are available in each region?
9. Find the most expensive property in Greater Accra
10. Which localities in Greater Accra have average prices above the overall dataset average?

# Trends and Conditions

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

### Property Level Analysis

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
order by
	price desc
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
group by
	bedrooms
order by
	bedrooms;
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
order by
	price desc;
```

## 4.  Which category (flats, detached, etc) has the highest average floor area?
This SQL query analyzes property categories (such as flats, detached houses, and semi-detached units) to determine which type offers the largest average floor area. It helps identify which property types provide the most spacious thereby comparing value by size.
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
| Category   | Average Floor Area | 
|----------|----------|
| Mansion     | 885.6  | 
| Duplex    | 628.68   | 
| Flats    | 579.71   | 
| Townhouse    | 537.08   |
| Detached    | 511.45   |
| Semi-Detached    | 384.91   |

*Table showing the various category of property types and the average floor area*

## 5. Show the average rental price per furnishing type (is furnished)
This SQL query calculates the average rental price based on furnishing status (furnished, semi-furnished, or unfurnished). It provides insight into how interior setup and comfort level influence rental pricing, helping both property owners and tenants understand the value of furnished options in the market.
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
| Category (Furnish)   | Average Rental Price | 
|----------|----------|
| Unfurnished     | 5672.26  | 
| Semi-Furnished    | 7403.58   | 
| Furnished    | 18534.17   | 

*Table showing the various category of property states and their average rental price*


### Location and Region Insights

## 6. Find the average rent by region
This SQL query calculates the average rental price for each region, allowing for a comparison of housing costs across different geographical areas. It helps uncover regional rental trends and highlights areas with higher or lower average property prices, supporting market analysis and investment decisions.
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
| Region   | Average Rental | 
|----------|----------|
| Greater Accra     | 8209.24  | 
| Ashanti    | 3660.17   | 
| Western Region    | 3513.64   | 
| Northern Region    | 2242.75   |
| Eastern Region    | 1750.00   |
| Central Region    | 1181.18   |
| Brong Ahafo    | 1175.00   |

*Table showing the various regions and their average rental price*


## 7. Get the top 5 localities with the highest average rental prices
This SQL query identifies the top five localities with the highest average rental prices. It highlights premium neighborhoods and helps analyze location-based pricing trendS.
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
This SQL query counts the total number of properties available in each region. It helps visualize the distribution of property listings across different areas, offering insights into market supply and potential investment hotspots.
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
| Region   | Total Properties | 
|----------|----------|
| Greater Accra     | 4219  | 
| Ashanti    | 185   | 
| Central Region     | 85   | 
| Northern Region    | 57   |
| Eastern Region    | 14   |
| Western Region    | 11   |
| Brong Ahafo    | 4   |

*Table showing the various regions and their total properties*

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
This SQL query identifies localities within Greater Accra whose average rental prices exceed the overall dataset average. It provides a focused view of premium neighborhoods in the region.
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


### TRENDS AND CONDITIONS

## 11. Count how many under each category are in the dataset
This SQL query counts the total number of properties under each category (e.g., flats, detached houses, semi-detached, etc.). It provides an overview of the property type distribution within the dataset, offering insights into which housing categories dominate the market.
```sql
SELECT 
    category,
    count(*) AS total_properties
FROM 
   ghana_real_estate_rentals.house_rentals
GROUP BY 
    category;
```
| Category   | Total Properties | 
|----------|----------|
| Flats     | 2987  | 
| Detached    | 1236   | 
| Townhouse    | 92   | 
| Duplex    | 183   |
| Mansion    | 30   |
| Semi-Detached    | 47   |

*Table showing the various housing categories and their total prices*

## 12. Find the average price per square meter (price/floor_area) for each region
This SQL query calculates the average rental price per square meter for each region by dividing the property price by its floor area.
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
This SQL query counts how many properties offer parking space compared to those that do not. It helps assess the availability of a key amenity across listings, offering insights into how common parking facilities are within the housing market and how they might influence property value or demand.
```sql
   SELECT 
    parking_space, 
    COUNT(*) AS total_properties
FROM 
   ghana_real_estate_rentals.house_rentals
GROUP BY 
   parking_space;
```
| Parking Space   | Total Properties | 
|----------|----------|
| False     | 4573  | 
| True    | 2   | 

*Table showing various properties and their parking spaces*

## 14. Which amenities appear most frequently across listings? (requires string search/processing)
This SQL query analyzes the amenities column to identify which features (such as parking, gym, pool, or security) appear most frequently across all property listings.
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

# What I learned
1. **Practical SQL Application in Real Estate**
   This project deepened my understanding of how SQL can be used to explore and analyze real-world property data — from calculating averages and identifying trends to filtering high-value listings and comparing regional performance.

2. **Data-Driven Market Understanding**
   Through querying and aggregating key metrics like rental yields, price per square meter, and category distribution, I learned to interpret data in ways that support better real estate decisions and investment strategies.

3. **Enhanced Data Interpretation for Market Insights**
   I developed the ability to analyze unstructured data — such as amenities and property features — to uncover patterns that reflect tenant preferences and market trends. This skill strengthens my capacity as a real estate executive to make data-informed choices in marketing, pricing, and property positioning.


# Conclusion
This SQL real estate analysis project provided valuable hands-on experience in using data to uncover actionable market insights. Through structured queries and data exploration, I analyzed pricing patterns, regional trends, property categories, and amenity distributions to better understand the dynamics of the housing market. The project strengthened my analytical thinking and technical SQL skills while deepening my understanding of how data shapes strategic decisions in real estate. As a real estate executive with a growing focus on data analytics, this exercise reinforced the importance of leveraging data to drive smarter marketing, pricing, and investment outcomes.

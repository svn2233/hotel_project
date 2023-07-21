# Hotel Project using SQL, Excel & BI tools 
 
## Exploring data using MySQL

#### Questions to answer 

1. Is hotel revenue growing by year? 
2. Should the hotel increase parking size?
3. What trends can see see with the data? I will focus on average daily rate and guests to explore seasonality

#### There are four tables in this project named 2018, 2019, 2020, Market and Meal.

## Steps
1. By using SQL I will analyze the data and use queries to make the dataset into one single table for further cleaning in excel.
2. After doing final cleaning in excel the data will be ready to be visualized in a BI tool.
3. During the visualizing stage I will be able to answer the given questions presented
4. I will visualize the data in both PowerBI and Tableau to showcase my skill in both BI tool 

#### 1a. Using UNION to see union hotel years 2018-2020 for easier analysis and queries.
```
SELECT * FROM hotel_project.`2018`
UNION
SELECT * FROM hotel_project.`2019`
UNION
SELECT * FROM hotel_project.`2020`;
```
![](/1a.png)<!-- -->

#### 1b.Examining the two other tables
```
SELECT * FROM market;
```
![](/1bmarket.png)<!-- -->
##### This table refers to discounts giving according to market segements 

```
SELECT * FROM meal;
```
![](/1bmeal.png)<!-- -->
##### This type refers to cost of meal and type of meal.


#### 2. Create a CTE to make querying faster and less tedious 
```
WITH hotels AS (
SELECT * FROM hotel_project.`2018`
UNION
SELECT * FROM hotel_project.`2019`
UNION
SELECT * FROM hotel_project.`2020`
)
SELECT * FROM hotels;
```
![](/2..png)<!-- -->

#### 3.Finding the revenue of hotels by adding stays weeks nights and stays weekend * by the average daily rate (adr) rounded to 2 decimals places.
```
WITH hotels AS (
SELECT * FROM hotel_project.`2018`
UNION
SELECT * FROM hotel_project.`2019`
UNION
SELECT * FROM hotel_project.`2020`
)
SELECT round((stays_in_week_nights + stays_in_weekend_nights)*adr,2) revenue FROM hotels;
```
![](/3..png)<!-- -->

#### 4. Finding the revenue of hotels by year rounded to 2 decimals places
```
WITH hotels AS (
SELECT * FROM hotel_project.`2018`
UNION
SELECT * FROM hotel_project.`2019`
UNION
SELECT * FROM hotel_project.`2020`
)
SELECT arrival_date_year,
ROUND(SUM((stays_in_week_nights + stays_in_weekend_nights)*adr),2) revenue FROM hotels
GROUP BY 1;
```
![](/4..png)<!-- -->

#### 5. Finding revenue by hotel type and year rounded to 2 decimals places 
```
WITH hotels AS (
SELECT * FROM hotel_project.`2018`
UNION
SELECT * FROM hotel_project.`2019`
UNION
SELECT * FROM hotel_project.`2020`
)
SELECT arrival_date_year,
hotel,
ROUND(SUM((stays_in_week_nights + stays_in_weekend_nights)*adr),2) revenue FROM hotels
GROUP BY 1,2
ORDER BY 2;
```
![](/5..png)<!-- -->

#### 6. Examining the results 2020 data doesn't seem to be complete. Too make sure I will use this query.
```
WITH hotels AS (
SELECT * FROM hotel_project.`2018`
UNION
SELECT * FROM hotel_project.`2019`
UNION
SELECT * FROM hotel_project.`2020`
)
SELECT arrival_date_week_number,arrival_date_year,arrival_date_month
FROM hotels
WHERE arrival_date_year='2020'
ORDER BY 1 DESC,2,3;
```
![](/6..png)<!-- -->

##### Indeed 2020 dataset is incomplete as the last month is August.

#### 7. Now I will use left join CTE table hotels with table market to examine discounts given per market segment
```
WITH hotels AS (
SELECT * FROM hotel_project.`2018`
UNION
SELECT * FROM hotel_project.`2019`
UNION
SELECT * FROM hotel_project.`2020`
)
SELECT * FROM hotels
LEFT JOIN market
ON hotels.market_segment=market.market_segment;
```
![](/7..png)<!-- -->

#### 8. Now I will use a add another left join to join the meal table
```
WITH hotels AS (
SELECT * FROM hotel_project.`2018`
UNION
SELECT * FROM hotel_project.`2019`
UNION
SELECT * FROM hotel_project.`2020`
)
SELECT * FROM hotels
LEFT JOIN market
ON hotels.market_segment=market.market_segment 
LEFT JOIN 
meal ON hotels.meal=meal.meal;
```
![](/8..png)<!-- -->

#### 9. Now data is ready to be exported into a CSV file and loaded into Excel for further cleaning and then loaded into a BI tool to be visualized into a dashboard.

Tableau Link
PowerBI Link 


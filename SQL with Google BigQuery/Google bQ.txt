1	find the unique categories in the data
```
SELECT DISTINCT(c.category) FROM `dulcet-order-419702.expenses.data` d 
JOIN `dulcet-order-419702.expenses.category` c
ON d.CategoryID = c.CategoryID
```

2	Show the purchase which had the highest cost
```
SELECT * FROM `dulcet-order-419702.expenses.data` 
WHERE cost =  (SELECT MAX(Cost) FROM `dulcet-order-419702.expenses.data`)
```

3	Query to show only first 20 rows of data
SELECT * FROM `dulcet-order-419702.expenses.data` 
LIMIT 20

4	show the unique company names where money has been spent
SELECT DISTINCT(Company) FROM `dulcet-order-419702.expenses.data` ; 

5	How many unique days has money been spent in each month
SELECT COUNT(DISTINCT(EXTRACT(DAY FROM date))) AS day, EXTRACT(MONTH FROM date) AS month
FROM `dulcet-order-419702.expenses.data` 
GROUP BY month 
ORDER BY month 

6	For the above question show it in descending order 
SELECT COUNT(DISTINCT(EXTRACT(DAY FROM date))) AS day, EXTRACT(MONTH FROM date) AS month
FROM `dulcet-order-419702.expenses.data` 
GROUP BY month 
ORDER BY day DESC

7	show only data of category id 3,4
SELECT *
FROM `dulcet-order-419702.expenses.data`  
WHERE CategoryID IN (3,4)

8	What is highest categoryid of expense in march
SELECT CategoryID, SUM(Cost) AS total_cost
FROM `dulcet-order-419702.expenses.data`  
WHERE EXTRACT(MONTH FROM date) = 3 
GROUP BY CategoryID
ORDER BY SUM(Cost) DESC
LIMIT 1

9	Which store had the highest expense in may
SELECT Company, SUM(Cost) AS total_cost
FROM `dulcet-order-419702.expenses.data`  
WHERE EXTRACT(MONTH FROM date) = 5 
GROUP BY Company
ORDER BY SUM(Cost) DESC
LIMIT 1

10	Which category had the lowest total number in february
SELECT c.Category, SUM(Cost) AS total_cost
FROM `dulcet-order-419702.expenses.data`  d
JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID
WHERE EXTRACT(MONTH FROM date) = 2
GROUP BY c.Category
ORDER BY SUM(Cost) 
LIMIT 1

11	Show the data only where shop name contains the letter w
SELECT * 
FROM `dulcet-order-419702.expenses.data`   
WHERE Company LIKE "%w%"

12	find a way to get the category based on category ID
SELECT d.*, c.* 
FROM `dulcet-order-419702.expenses.data`  d
JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID

13	Is there any category ID not present in the data table
SELECT c.*, d.*
FROM `dulcet-order-419702.expenses.category`  c
LEFT JOIN `dulcet-order-419702.expenses.data`  d ON c.CategoryID = d.CategoryID
WHERE d.CategoryID IS NULL

14	show categories with expense more than 150 for the month of april
SELECT c.Category,  SUM(d.Cost) AS expenses
FROM `dulcet-order-419702.expenses.data`  d
JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID
WHERE EXTRACT(MONTH FROM d.date) = 4 
GROUP BY c.Category
HAVING SUM(d.Cost) > 150

15	Any patterns in ticket expenses over time
SELECT EXTRACT(MONTH FROM d.Date) AS Month, SUM(d.Cost) AS expenses
FROM `dulcet-order-419702.expenses.data`  d
JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID
WHERE c.Category = "Ticket"
GROUP BY EXTRACT(MONTH FROM d.Date) 

16	Which restaurant has received the maximum orders based on days
SELECT d.Company, COUNT(DISTINCT d.Date)
FROM `dulcet-order-419702.expenses.data`  d
JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID
WHERE c.Category = "Restaurant"
GROUP BY   d.Company
ORDER BY COUNT(DISTINCT d.Date) DESC
LIMIT 1

17	Calculate average spend per day for restaurants
SELECT SUM(d.Cost)/ COUNT(d.date)
FROM `dulcet-order-419702.expenses.data`  d
JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID
WHERE c.Category = "Restaurant"  


18	which day of week saw the highest spend in may
SELECT FORMAT_DATE("%A", date) AS day_of_week,SUM(cost) AS total_cost
FROM `dulcet-order-419702.expenses.data`  
WHERE EXTRACT(MONTH FROM date) = 5
GROUP BY FORMAT_DATE("%A", date)
ORDER BY total_cost  DESC
LIMIT 1


19	calculate total cost for grocery per month and show month in year and month format separated by hyphen
SELECT SUM(d.Cost), EXTRACT(MONTH FROM d.Date) AS Month, FORMAT_DATE('%Y-%m', d.Date) AS Year_Month
FROM `dulcet-order-419702.expenses.data`  d
JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID
WHERE c.Category = "Grocery"  
GROUP BY 2,3

20	Calculate total spend for shops starting with capital letter R
SELECT SUM(Cost), company
FROM `dulcet-order-419702.expenses.data`   
WHERE Company LIKE ("R%") 
GROUP BY company


21	How many unique companies exist in the shopping category
SELECT COUNT(DISTINCT(d.Company))
FROM `dulcet-order-419702.expenses.data`  d
JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID
WHERE c.Category = "Grocery"  

22	What is the spending pattern at Rewe monthwise and any insights
SELECT SUM( Cost),Company, FORMAT_DATE("%Y-%m", Date)
FROM `dulcet-order-419702.expenses.data`   
WHERE  Company = "Rewe"
GROUP BY FORMAT_DATE("%Y-%m", Date), Company

23	Any trends with respect to eating at dominos restaurant
SELECT SUM( Cost),Company, FORMAT_DATE("%Y-%m", Date)
FROM `dulcet-order-419702.expenses.data`   
WHERE  Company = "Dominos"
GROUP BY FORMAT_DATE("%Y-%m", Date), Company

24	Is there any month where grocery expense is bit different
SELECT SUM(d.Cost), FORMAT_DATE("%Y-%m", d.Date)
FROM `dulcet-order-419702.expenses.data`  d
JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID
WHERE c.Category = "Grocery" 
GROUP BY FORMAT_DATE("%Y-%m", d.Date)

25	Show only the company with highest spend in each category for april
WITH newdata AS (
  SELECT SUM(d.Cost) AS total_cost, d.Company, C.Category
  FROM `dulcet-order-419702.expenses.data`  d
  JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID
  WHERE EXTRACT(MONTH FROM d.Date) = 4
  GROUP BY  d.Company, C.Category 
)
SELECT Company, Category, RANK() OVER (PARTITION BY Category ORDER BY total_cost DESC) AS Ranking
FROM newdata
QUALIFY Ranking = 1

26	calculate % change in total cost for each month & find the month with highest % change
WITH monthlyCost2 AS(
WITH monthly_cost AS (
  SELECT SUM(Cost) AS total_cost, FORMAT_DATE("%Y-%m", Date) AS yearmonth
  FROM `dulcet-order-419702.expenses.data`  
  GROUP BY yearmonth
) 
SELECT yearmonth, total_cost, LAG(total_cost) OVER (ORDER BY monthly_cost.yearmonth)  AS pre_cost
FROM monthly_cost
)
SELECT yearmonth, ((total_cost - pre_cost)/pre_cost) * 100
FROM monthlyCost2
ORDER BY monthlyCost2.yearmonth

27	do the same as above question but only for restaurant category 
WITH monthlyCost2 AS(
WITH monthly_cost AS (
  SELECT SUM(Cost) AS total_cost, FORMAT_DATE("%Y-%m", Date) AS yearmonth
  FROM `dulcet-order-419702.expenses.data`  d
  JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID
  WHERE c.Category = "Restaurant"
  GROUP BY yearmonth
) 
SELECT yearmonth, total_cost, LAG(total_cost) OVER (ORDER BY monthly_cost.yearmonth)  AS pre_cost
FROM monthly_cost
)
SELECT yearmonth, ((total_cost - pre_cost)/pre_cost) * 100
FROM monthlyCost2
ORDER BY monthlyCost2.yearmonth


28	find the date with highest number of unique categories where money was spent
SELECT COUNT(DISTINCT c.Category), d.Date
FROM `dulcet-order-419702.expenses.data`  d
JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID 
GROUP BY d.Date
ORDER BY COUNT(DISTINCT c.Category) DESC
LIMIT 1

29	Use case statement to categorise restaurants as Indian vs non indian based on name and show total cost for June
SELECT SUM(Cost) AS Cost,
  CASE
    WHEN Company LIKE "%Jaipur%" THEN "Indian"
    ELSE "Non-Indian"
    END
    AS Restaurant_type
FROM `dulcet-order-419702.expenses.data`  
WHERE EXTRACT(MONTH FROM Date) = 6 AND CategoryID = 4
GROUP BY Restaurant_type

30	Show the ratio of total spend for restaurants vs grocery for april
SELECT SUM(IF(c.Category = "Restaurant", cost, 0))/ SUM(IF(c.Category = "Grocery", cost, 0))
FROM `dulcet-order-419702.expenses.data`  d
JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID
WHERE EXTRACT(MONTH FROM Date) = 4 

31	what is the average spend per month at inter store
SELECT AVG(Cost) AS avg_cost, EXTRACT(MONTH FROM Date) AS month 
FROM `dulcet-order-419702.expenses.data`  d
JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID
WHERE c.Category = "Grocery" AND company LIKE "Inter Store"
GROUP BY month 

32	which company in shopping category had the highest total cost
SELECT SUM(Cost) AS total_cost , d.Company
FROM `dulcet-order-419702.expenses.data`  d
JOIN `dulcet-order-419702.expenses.category`  c ON c.CategoryID = d.CategoryID
WHERE c.Category = "Shopping"  
GROUP BY  d.Company
ORDER BY total_cost DESC
LIMIT 1

33	Use union clause to show total cost for kebab shop and also panda using two different queries
WITH kebab_cost AS(
  SELECT SUM(Cost) AS total_cost
  FROM `dulcet-order-419702.expenses.data`
  WHERE Company = "Kebab Shop"
),
panda_cost AS( 
  SELECT SUM(Cost) AS total_cost
  FROM `dulcet-order-419702.expenses.data`
  WHERE Company = "Panda"
)
SELECT "Kebab Shop" AS Company, kebab_cost.total_cost
FROM kebab_cost
UNION ALL
SELECT "Panda" AS Company, panda_cost.total_cost
FROM panda_cost

34	is there any fully duplicate value in the data
WITH cte AS (
  SELECT *,
  ROW_NUMBER() OVER (PARTITION BY Date,Company,CategoryID, Cost) AS rowNumber
  FROM `dulcet-order-419702.expenses.data` 
)
SELECT * 
FROM cte
WHERE rowNumber > 1

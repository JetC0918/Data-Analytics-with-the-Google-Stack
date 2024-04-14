# SQL Formula

Formula that are used in the project

Return unique value
```
DISTINCT ()
```

Select highest cost
```
MAX()
```

Show first 20 rows
```
LIMIT 20
```

Get month from date
```
EXTRACT MONTH FROM (date)
```

Order in descending order	
```
ORDER BY day DESC
```

Show only data of category id 3,4	
```
WHERE CategoryID IN (3,4)
```

Show string contain letter "w"
```
WHERE Company LIKE "%w%"
```

Find data the is not present in table
```
WHERE d.CategoryID IS NULL
```

Show month in year and month format separated by hyphen
```
FORMAT_DATE('%Y-%m', d.Date) AS Year_Month
```

Categorise restaurants as Indian vs non indian based on name 
```
  CASE
    WHEN Company LIKE "%Jaipur%" THEN "Indian"
    ELSE "Non-Indian"
    END
    AS Restaurant_type
```

Check duplicated value
```
WITH cte AS (
  SELECT *,
  ROW_NUMBER() OVER (PARTITION BY Date,Company,CategoryID, Cost) AS rowNumber
  FROM `dulcet-order-419702.expenses.data` 
)
SELECT * 
FROM cte
WHERE rowNumber > 1
```

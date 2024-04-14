Import data from bigquery
[Import data from bigquery]([image_url](https://github.com/JetC0918/Data-Analytics-with-the-Google-Stack-/blob/main/Data%20Visualization%20with%20Google%20Looker%20Studio/1.JPG))

Connect the Google sheet data also and see how to merge them together
```
Resource -> Manage added data sources -> ADD A DATA SOURCE -> Google Sheet
SELECT chart -> BLEND DATA -> Choose Dimensions & Metrics -> Join Another Table -> Choose Table -> Choose Join Inner Join
```

Write a case statement to categorize fun and tips content as one category, others as a seperate category
```
Resource -> Manage added data sources -> EDIT -> ADD A FIELD -> ADD calculated field
CASE WHEN Category_Id IN (1,4) THEN "Fun_tips" 
ELSE 'OTHER'
END
```

Observe trends in platfroms & categories
```
INSERT table -> Add Metric -> ADD Field -> Formula to calculate new value
Chart -> style -> Metric 1 -> Decorate the data with heatmap . bar/pub
```

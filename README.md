# Case-study-Bright-Coffee-Shop-Sales
This repository contains the full analysis for the Bright Coffee Shop Sales Case Study (BRIGHTLEARN). The project explores daily transactional data to deliver insights for a new CEO aiming to grow revenue and improve product performance. Using Miro, I designed the initial data architecture and planning workflow. Data was cleaned, transformed, and processed in Snowflake, including time-bucket creation, revenue calculations, and product-level aggregations. The processed dataset was analyzed in Microsoft Excel using pivot tables and visual dashboards to uncover trends in product sales, peak sales hours, and category performance. The final business insights and recommendations were presented using PowerPoint and enhanced visuals created in Canva. This repository includes the Miro planning snapshot, SQL scripts, Snowflake transformations, Excel dashboards, and presentation materials used throughout the project.

--To check all the column names in my data.
--To check the data types in my data
SELECT*
FROM CASESTUDY.BRIGHT.COFFEE_SHOP
LIMIT 10;

---------------
-- I want to check my categorical columns
SELECT DISTINCT store_location
FROM casestudy.bright.coffee_shop;

SELECT DISTINCT product_category
FROM casestudy.bright.coffee_shop;


SELECT DISTINCT MIN (transaction_date) AS first_operating_date
FROM casestudy.bright.coffee_shop;

SELECT DISTINCT MAX (transaction_date) AS last_operating_date
FROM casestudy.bright.coffee_shop;


SELECT DISTINCT MIN (transaction_time) AS opening_hour
FROM casestudy.bright.coffee_shop;

SELECT DISTINCT MAX (transaction_time) AS opening_hour
FROM casestudy.bright.coffee_shop;


--------------------------------------------
SELECT transaction_date,
       DAYNAME(transaction_date) AS day_name,
       CASE 
        WHEN day_name IN ('Sun', 'Sat') THEN 'Weekend'
        ELSE 'Weekday'
        END AS day_classification,
       MONTHNAME (transaction_date) AS month_name,

       
       --transaction_time,
        CASE
           WHEN transaction_time BETWEEN '06:00:00' AND '11:59:59' THEN 'Morning'
           WHEN transaction_time BETWEEN '12:00:00' AND '16:59:59' THEN 'Afternoon'
           WHEN transaction_time >= '17:00:00' THEN 'Evening'
        END AS time_classification,

        HOUR (transaction_time) AS hour_of_day,

        --categorical data
        store_location,
        product_category,
        product_detail,
        product_type,

        --ID's
        COUNT (DISTINCT transaction_id) AS number_of_sales,

        --Revenue calculation
        SUM(transaction_qty*unit_price) AS revenue
      FROM casestudy.bright.coffee_shop
GROUP BY ALL;

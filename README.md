# Sales Analysis

### Table of contents
- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Data cleaning/preperation](#data-cleaning/preperation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results/Findings](#results/findings)
- [Recommendations](#recommendations)

### Project Overview
This data analysis project aims to provide insights into the sales performance of an e-commerce company over the years. By analyzing various aspects of the sales data,
we seek to identify trends, make data driven decisions and gain deeper understanding of the company's performance.

### Data Source
The primary dataset used for this analysis is the ''Sample - Superstore.xls'' file, containing the detailed information on the sales made by the company.

### Tools
- Microsoft Excel - Data cleaning
- Microsoft SQL Server - Data analysis

### Data cleaning/preperation
In the initial data preperation phase, I performed the following:
- Data loading and inspection
- Handling missing values
- Removing duplicates
- Data cleaning and formatting

### Exploratory Data Analysis
EDA involved exploring sales data to answer key questions such as:
- What are the overall sales
- What are the sales per category
- What are the profits per category
- Who are the top 3 customers

### Data Analysis
- To find the total sales per category

 ```SQL
SELECT Category, SUM(Sales) As [Total Sales]
FROM Orders
GROUP BY Category;
```

- To find the profit per category per year

```SQL
SELECT YEAR([Order Date]) AS OrderYear, Category, SUM(Profit) As [Total Profit]
FROM Orders
GROUP BY YEAR([Order Date]), Category
ORDER BY YEAR([Order Date])
```

- To find top 3 customer each year who had the highest sales amount

```SQL
WITH RankedSales AS (
    SELECT
        [Customer Name],
        [Category],
        [Sales],
        YEAR([Order Date]) AS OrderYear,
        RANK() OVER (
            PARTITION BY YEAR([Order Date])
            ORDER BY [Sales] DESC
        ) AS SalesRank,
        MAX([Sales]) OVER (
            PARTITION BY YEAR([Order Date]), [Category]
        ) AS [Max Sales],
        SUM([Sales]) OVER (
            PARTITION BY YEAR([Order Date]), [Category]
        ) AS [Total Sales]
    
    FROM
        Orders
)
SELECT
    OrderYear,
    [Customer Name],
    [Category],
    [Sales],
    [Max Sales],
    [Total Sales],
    SalesRank
FROM
    RankedSales
WHERE
    SalesRank <= 3
ORDER BY
    OrderYear,
    SalesRank;
```

### Results/Findings
The analysis results are summerized as follows:
- Office supplies and technology are the most purchased and profitable categories
- Company's sales have been increasing steadily over the years
- Sales peak during the holiday season which is November and December

### Recommendations
Based on the analysis, I recommend the following:
- Invest in marketing and promotions during the peak season to increase the sales and profits
- Focus on expanding and promoting the products in Furniture category to increase the sales
- Implement the customer segmentation strategy to target the high lifetime value customers

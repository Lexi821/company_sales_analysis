# Company Sales Analysis

## Table of Contents

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning](#data-cleaning)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results](#results)
- [Recommendations](#recommendations)

### Project Overview
---

This data analysis project aims to provide insights into the sales performance of a company in the year of 1996-1998. By analyzing various aspects of the sales data, we seek to identify trends, make data-driven recommendations, and gain a deeper understanding of the company's performance.



### Data Sources

Sales Data: The primary dataset used for this analysis is the "final_project_2M_cis467.xls" file, containing detailed information about each sale made by the company.

### Tools

- Excel - Data Cleaning
- SQL Server - Data Analysis
- Tableau - Creating reports


### Data Cleaning

In the initial data preparation phase, we performed the following tasks:
1. Data loading and inspection.
2. Handling missing values.
3. Data cleaning and formatting.

### Exploratory Data Analysis

EDA involved exploring the sales data to answer key questions, such as:

- What is the overall sales trend?
- Which category is the top seller?
- What are the peak sales periods?

### Data Analysis
1. Find the best-performing category:

```sql
WITH QuarterlySales AS (
    SELECT
        ProductID,ProductName,CategoryName,
        YEAR(STR_TO_DATE(OrderDate, '%m/%d/%Y')) AS FixedYear,
        QUARTER(STR_TO_DATE(OrderDate, '%m/%d/%Y')) AS FixedQuarter,
        SUM(Quantity) AS total_sales,
        ROW_NUMBER() OVER (PARTITION BY QUARTER(STR_TO_DATE(OrderDate, '%m/%d/%Y')) ORDER BY SUM(Quantity) DESC) AS RowNum
    FROM sales_dw
    WHERE YEAR(STR_TO_DATE(OrderDate, '%m/%d/%Y')) = 1997
    GROUP BY FixedYear,FixedQuarter,ProductID, ProductName, CategoryName
)
SELECT FixedYear,FixedQuarter,ProductID,ProductName,CategoryName,total_sales
FROM QuarterlySales
WHERE RowNum = 1;
```

2. Find the most potential region:
```sql
SELECT
  Country,
        ProductName,
        ROUND(SUM(SalePrice*Quantity*(1-Discount)),2) AS sales_revenue
FROM sales_dw
GROUP BY Country
ORDER BY sales_revenue DESC
LIMIT 10;
```

### Results

The analysis results are summarized as follows:
1. The company's sales are relatively stable, with a noticeable peak during the holiday season(Dec, Jan).
2. "Dairy products" is the best-performing category in terms of sales, quantity and number of customers.
3. The United States has the highest sales among all regions.

### Recommendations

Based on the analysis, we recommend the following actions:
- Invest in marketing and promotions during peak sales seasons to maximize revenue.
- Focus on expanding and promoting products in Dairy products.
- focus on the U.S. market and increase promotion of high-selling products in this region, such as beverages and meat..





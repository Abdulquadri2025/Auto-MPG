# Auto-MPG
### INTRODUCTION
A SQL project analyzes consumer data on vehicle exploration for Auto MPG. This project uses data analytics tools to evaluate vehicle data from the Auto MPG market. The goal is to uncover actionable insights to enhance business decisions, identify high-demand car models, and optimize inventory strategies.

**TABLE OF CONTENTS**
  - Project Overview
  - Project scope
  - Project objectives
  - Expected outcome
  - Document purpose
  - Use case
  - Data source
  - Data cleaning and processing
  - Data Analysis
  - Conclusion

**Project Overview**

This project explores the Auto MPG dataset to analyze and predict the fuel efficiency (MPG) of cars based on various features like engine size, weight, horsepower, and more. The goal is to draw insights that can guide better automobile design and consumer decisions.

**Project Scope**

The scope of this analysis includes:
  - Cleaning and preprocessing the data.
  - Understanding correlations between vehicle features and MPG.
  - Building visualizations for trend analysis.
  - Generating actionable insights and recommendations.

**Project Objectives**

 - Identify key features that impact vehicle MPG.
 - Visualize the relationship between features like weight, horsepower, and MPG.
 - Handle missing or incorrect data entries efficiently.
 - Offer recommendations for fuel-efficient vehicle design.
   
**Expected Outcome**

 - A clean dataset ready for modeling or reporting.
 - Clear visual trends showing how features impact MPG.
 - Insightful recommendations for manufacturers or analysts.
 - Potential baseline model for MPG prediction (optional).

**Document Purpose**

This document summarizes the full data analysis process carried out on the Auto MPG dataset. It captures the dataset’s insights, processing steps, visualizations, and recommendations for future decision-making.

**Use Case**

This analysis can be used by:
 - Automotive manufacturers are to improve fuel efficiency.
 - Environmental analysts study emissions and fuel impact.
 - Consumers are comparing car efficiency based on specs.
 - Machine learning developers are creating predictive MPG models.
   
**Data Source**

The dataset used is the Auto MPG dataset from [Maven Analytics](https://mavenanalytics.io/data-playground?order=date_added%2Cdesc&search=AUTO&tags=Transportation). It typically contains columns like:
 - mpg
 - cylinders
 - displacement
 - horsepower
 - weight
 - acceleration
 - model year
 - origin
 - car name

**Data Cleaning and Processing**

The dataset used for this project was stored in a SQL Server database under the table auto_mpg. Before conducting any analysis, several data cleaning and transformation steps were performed to ensure accuracy and consistency.

  **Handling Missing Values**
      - A check was performed to identify missing or null values in key columns, especially horsepower:

```
SELECT COUNT (*) AS null_horsepower_count
FROM auto_mpg
WHERE horsepower IS NULL
```
  - The analysis revealed that some horsepower entries were either missing or non-numeric. To address this, a new column, horsepower clean, was created to store cleaned, numeric-only values:

```
ALTER TABLE auto_mpg ADD horsepower clean FLOAT;
UPDATE auto_mpg
SET horsepower_clean = TRY_CAST (horsepower AS FLOAT)
WHERE ISNUMERIC (horsepower) = 1;
```

**Type Conversion and Data Transformation**

  - Since the original horsepower field sometimes contained non-numeric values (like '?'), a conversion using TRY_CAST ensured only valid numbers were moved to horsepower clean.
  - This new column allowed for safe numerical analysis without altering the original data, maintaining data integrity.

**Categorization of Numerical Data**

  - Cleaned horsepower values were categorized into meaningful ranges for better insights:

```
WITH horsepower_ranges AS (
    SELECT *,
        CASE 
            WHEN horsepower_clean < 75 THEN 'Low (<75)'
            WHEN horsepower_clean BETWEEN 75 AND 125 THEN 'Medium (75–125)'
            WHEN horsepower_clean > 125 THEN 'High (>125)'
            ELSE 'Unknown'
        END AS horsepower_group
    FROM auto_mpg
    WHERE horsepower_clean IS NOT NULL
)
SELECT horsepower_group,
       ROUND(AVG(mpg), 2) AS avg_mpg,
       COUNT(*) AS car_count
FROM horsepower_ranges
GROUP BY horsepower_group
ORDER BY avg_mpg DESC;
```
  - •	This classification was essential for grouped analysis and comparisons between fuel efficiency and engine power.

**Standardization of Categorical Variables**

  - The origin column, which originally used numeric codes (1, 2, 3), was mapped to actual regions:

```
SELECT 
    CASE origin
        WHEN 1 THEN 'USA'
        WHEN 2 THEN 'Europe'
        WHEN 3 THEN 'Japan'
        ELSE 'Unknown'
    END AS region,
    ROUND(AVG(mpg), 2) AS avg_mpg,
    COUNT(*) AS car_count
FROM auto_mpg
GROUP BY origin;
```
This improved interpretability and was used in further visual and analytical summaries.

**Data Analysis**

 - MPG Trends: Observed that vehicles with lower weight and higher acceleration generally have higher MPG.
 - Correlation Analysis: Strong negative correlation between weight and mpg; moderate negative correlation with displacement and horsepower.
 - Temporal Analysis: Newer model years tend to have better MPG.
 - Origin-Based Comparison: Japanese and European cars tend to be more fuel-efficient than American cars.

**Conclusion**

This project highlights that vehicle weight, engine displacement, and horsepower significantly influence fuel efficiency. Data visualization and analysis generated actionable insights to guide fuel-efficient vehicle development and consumer awareness.

Thank you for Reading

I am highly motivated to pursue a data analyst role within an organization where I can effectively apply my skills, take on new challenges, and continuously grow. I aim to contribute meaningfully to a team that values impact and innovation while advancing my professional development and the organization’s goals.

You can reach me at aregbeabdulquadri@gmail.com

**THANK YOU**





















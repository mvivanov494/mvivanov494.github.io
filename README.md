# Data Scientist

#### Technical Skills: 
Microsoft Excel (Advanced, Pivot Tables, Pivot Charts, V/X/HLOOKUP, VBA, Data Cleaning, Conditional Formatting), C++ (Basics), Python (Intermediate, Pandas, Numpy, scikit, Matplotlib), R Studio (Advanced, ggplot, Regression Analysis, Statistical Modeling, Data Visualization), MATLAB (Basics), SQL (Intermediate, Joins, CTEs, Temp Tables, Aggregate Functions, Views), Tableau (Intermediate, Interactive Dashboards, Filters/Slicers, Parameters, Calculated Fields, Data Blending, Trend Lines), PowerBI, Japanese Language (Advanced), Vietnamese Language (Intermediate Level), English Language (Native)

## Education		        		
B.S., Mathematics with a Minor in Economics | The University of Houston (August 2021 - May 2025)

## Work Experience
**Independent Data Analyst (April 2025 - Present_)**
• Assisted on a Fiverr project to develop a machine learning model predicting S&P 500 price movement.
• Helped preprocess historical price and macroeconomic data using Yahoo Finance and the FRED API.
• Explored sentiment analysis with VADER and contributed to integrating sample Twitter-like data.
• Observed the use of technical indicators (SMA, RSI, MACD) and Random Forest modeling for predictions

**Telecommunications Technician @ Ronek Communications (April 2020 - June 2023)**
•	Reviewed job sites and installed telecommunication equipment in data centers for major corporations
•	Improved workflow by 30%, daily terminating 30+ Cat6 ethernet cables and tested their connection to a server 
•	Gained hands-on experience working with complex network systems, developing a foundation in data flow, systems architecture, and troubleshooting—skills applicable to large-scale data environments.
•	Conducted deficiency and failure analysis on over 40 server connections, requiring precise attention to detail.

## Projects
### Personal Project, Nashville Housing Data Cleaning, Analysis, and Visualization
• Cleaned, transformed, and analyzed over 56,000 rows of housing data using SQL, with prior data preparation in Excel using
Find & Replace (Ctrl+H), =PROPER(), Flash Fill, and Advanced Filters to standardize date types, fill NULLs, and ensure
smooth CSV import.
• Built an automated SQL data pipeline that included LOAD DATA INFILE, NULL handling, and string parsing functions
(SUBSTRING_INDEX, TRIM, LEFT, INSTR, REVERSE) to normalize address and ownership fields for analysis.
• Applied CTEs and ROW_NUMBER() window functions to remove duplicates based on property details and categorized
building/land values using CASE logic.
• Developed and published an interactive Tableau dashboard that visualized housing distributions by construction year,
land/building value ranges, and location, enabling stakeholders to assess market trends and property characteristics

Tableau Dashboard Link:
<https://shorturl.at/NhTgX>
![nashville dashboard]("/assets/image/NHD_Tableau_Dashboard.png")

Nashville Housing Data Cleaning & Visualization Project This project showcases a complete end-to-end data cleaning, transformation, analysis, and visualization workflow using SQL, Excel, and Tableau, centered around real estate property data from Nashville, Tennessee. It highlights key data analysis skills including ETL processes, data quality management, and dashboard development.

Tools Used MySQL – For importing, cleaning, and analyzing structured data using advanced SQL

Excel – For initial cleaning, formatting, and handling nulls and duplicates

Tableau – For interactive data visualization and dashboard design

Git/GitHub – For version control and project documentation

Project Overview Imported a raw dataset of 56,000+ rows using LOAD DATA INFILE in MySQL

Standardized and split full address fields into street, city, and state using string functions like SUBSTRING_INDEX, LEFT, and LOCATE

Replaced missing or blank values using NULLIF, IFNULL, and manual preprocessing in Excel

Removed duplicate rows using Common Table Expressions (CTEs) with ROW_NUMBER() OVER (PARTITION BY ...)

Converted categorical flags such as ‘Y’ and ‘N’ to readable formats like ‘Yes’ and ‘No’

Created new categorized columns for building and land value ranges using CASE statements

Aggregated and grouped home counts by bedrooms, bathrooms, year built, and price tiers

Deleted irrelevant columns after transformation to finalize the dataset

Built a Tableau dashboard to visualize metrics such as construction year distribution, bedroom counts, building value tiers, land usage, and notable high/low-value properties

Key Concepts Demonstrated Data wrangling with SQL (CTEs, joins, aggregates, string manipulation)

Data pipeline setup from raw CSV to structured, queryable tables

Value binning using CASE for price brackets

Dimensional modeling through address field decomposition

Excel techniques for NULL handling and formatting (Ctrl+H, filters, keyboard shortcuts)

Visual analytics and interactive dashboard publishing in Tableau

Outcome This project simulates a real-world data cleaning and reporting workflow that a business analyst or data analyst would perform to prepare property data for stakeholder analysis, reporting, or integration into BI tools.

```sql
ALTER TABLE nashville_housing_data
ADD address_trimmed TEXT;

UPDATE nashville_housing_data
SET address_trimmed = TRIM(SUBSTRING(property_address, 1, LENGTH(property_address) - LOCATE(' ', REVERSE(property_address)))) ;

ALTER TABLE nashville_housing_data
ADD city TEXT;

UPDATE nashville_housing_data
SET city = TRIM(SUBSTRING_INDEX(property_address, ' ', -1));
```

### Personal Project, COVID-19 Data Analysis and Dashboard Development
• Built a complete SQL-based data pipeline to analyze global COVID-19 trends, death rates, and vaccination rollouts using
cleaned and structured government datasets.
• Performed advanced data cleaning on 170,000+ rows in Excel using helper columns, IF statements, VLOOKUP, and
keyboard shortcuts to identify and fill NULL values, standardize date formats, and enforce data type consistency for seamless
SQL import.
• Used CTEs, temp tables, joins, and aggregate functions to calculate per-country infection rates, daily deaths, and
vaccination percentages, and optimized queries for dashboard readiness.
• Designed and published a dynamic Tableau dashboard to visualize key metrics like death rates, infections per population,
and vaccine rollout across continents.

Covid data from: https://ourworldindata.org/covid-deaths between 2020-02-24 to 2021-04-30 from all countries 85,000+ entries per CSV file **Tableau Dashboard Link: ** https://public.tableau.com/app/profile/mikhail.ivanov8682/viz/CovidDataDashboard_17517669300950/Dashboard1

🦠 COVID-19 Global Data Exploration & Analysis This project demonstrates full-cycle data exploration using SQL to analyze and uncover insights from global COVID-19 datasets. It highlights core analyst skills including data cleaning, joins, CTEs, window functions, aggregate analysis, and performance optimization using temporary tables and views.

🔧 Tools Used MySQL – For data import, cleaning, and in-depth querying

CSV Files – Raw COVID-19 death and vaccination datasets

Windows File System – Used LOAD DATA INFILE to bring data into MySQL from local CSVs

Git/GitHub – For source control and project sharing

📁 Project Overview Imported and normalized large-scale COVID-19 datasets into SQL using LOAD DATA INFILE with NULLIF() handling

Created clean, relational tables for COVID deaths and vaccinations globally

Performed advanced SQL analysis to compute infection and death percentages, population-level vaccination rates, and temporal insights

Applied joins, CTEs, temporary tables, and views to structure reusable and modular analytics queries

Segmented analysis by country, continent, and date for trend breakdowns

Created views to simplify repeated access to key KPIs, including highest infection rate, rolling vaccination percentage, and global death rates

🧠 Key SQL Concepts Demonstrated Data Cleaning: Using NULLIF() to handle missing entries on load

Data Joins: Merged vaccination and death datasets on location and date

Window Functions: Used SUM() OVER (PARTITION BY ...) for cumulative vaccination counts

CTEs: Structured complex logic like percent vaccinated by population

Temporary Tables: Stored intermediate rolling results for analysis and visualization

Views: Persisted KPIs like highest death rates, infection rates, and vaccination coverage

Aggregate Functions: Calculated total cases, deaths, infection % by population, and death rates

CASE Statements: Used for readable output formatting and filtering logic

🔍 Example Analysis Conducted Likelihood of death per infection in the U.S.

Infection penetration (% of population infected) by country

Rolling vaccination trends using window functions

Global case fatality rate across all countries

Top countries by infection and death rates (absolute and per capita)

Most affected continents by cumulative death totals

🧮 Views Created percentpopulationvaccinated – Cumulative vaccinations by country

highestinfectionrate – Countries with highest % of population infected

highestdeathcount – Countries with highest death counts

highestdeathcountbycontinent – Continent-level death tallies

worlddeathpercentage – Global fatality ratio based on reported cases and deaths

🧾 Outcome This project showcases practical data exploration methods used in health and crisis analytics. By using SQL to analyze real-world COVID-19 data, it simulates the responsibilities of a data analyst investigating trends in large, multi-dimensional public health datasets.


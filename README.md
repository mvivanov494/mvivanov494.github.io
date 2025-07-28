## Data Scientist

### Technical Skills: 
---

Microsoft Excel (Advanced, Pivot Tables, Pivot Charts, V/X/HLOOKUP, VBA, Data Cleaning, Conditional Formatting), C++ (Basics), Python (Intermediate, Pandas, Numpy, scikit, Matplotlib), R Studio (Advanced, ggplot, Regression Analysis, Statistical Modeling, Data Visualization), MATLAB (Basics), SQL (Intermediate, Joins, CTEs, Temp Tables, Aggregate Functions, Views), Tableau (Intermediate, Interactive Dashboards, Filters/Slicers, Parameters, Calculated Fields, Data Blending, Trend Lines), PowerBI, Japanese Language (Advanced), Vietnamese Language (Intermediate Level), English Language (Native)

## Education	
B.S., Mathematics with a Minor in Economics | The University of Houston (August 2021 - May 2025)

## Work Experience
**Independent Data Analyst (April 2025 - Present)**

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
## Personal Project, Nashville Housing Data Cleaning, Analysis, and Visualization

#### Tableau Dashboard Link:
<https://shorturl.at/NhTgX>

![nashville dashboard](/assets/image/NHD_Tableau_Dashboard.png)

This project showcases a complete end-to-end data cleaning, transformation, analysis, and visualization workflow using SQL, Excel, and Tableau, centered around real estate property data from Nashville, Tennessee. It highlights key data analysis skills including ETL processes, data quality management, and dashboard development.

### Tools Used:

- MySQL – For importing, cleaning, and analyzing structured data using advanced SQL

- Excel – For initial cleaning, formatting, and handling nulls and duplicates

- Tableau – For interactive data visualization and dashboard design

- Git/GitHub – For version control and project documentation

- Project Overview Imported a raw dataset of 56,000+ rows using LOAD DATA INFILE in MySQL

### Code:

- Standardized and split full address fields into street, city, and state using string functions like SUBSTRING_INDEX, LEFT, and LOCATE
  
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

- Removed duplicate rows using Common Table Expressions (CTEs) with ROW_NUMBER() OVER (PARTITION BY ...)
  
```sql
-- Removing duplicates with a CTE
WITH RowNumCTE AS(
SELECT unique_id, 
	ROW_NUMBER() OVER (
    PARTITION BY parcel_id,
				 property_address,
                 sale_price,
                 sale_date,
                 legal_reference
                 ORDER BY
					unique_id
                    ) row_num
FROM nashville_housing_data
)

DELETE FROM nashville_housing_data
WHERE unique_id IN (SELECT unique_id FROM RowNumCTE WHERE row_num > 1);
```

- Converted categorical flags such as ‘Y’ and ‘N’ to readable formats like ‘Yes’ and ‘No’
  
```sql
-- Change Y and N to Yes and No in "Sold as Vacant' field
SELECT DISTINCT(sold_as_vacant), COUNT(sold_as_vacant)
FROM nashville_housing_data
GROUP BY sold_as_vacant
ORDER BY 2;

SELECT sold_as_vacant,
	CASE WHEN sold_as_vacant = 'Y' THEN 'Yes'
		 WHEN sold_as_vacant = 'N' THEN 'No'
		 ELSE sold_as_vacant
		 END
FROM nashville_housing_data;

UPDATE nashville_housing_data
SET sold_as_vacant = 	CASE WHEN sold_as_vacant = 'Y' THEN 'Yes'
		 WHEN sold_as_vacant = 'N' THEN 'No'
		 ELSE sold_as_vacant
		 END;
```

- Created new categorized columns for building and land value ranges using CASE statements
  
```sql
-- Looking counts of homes in each bracket of building values
SELECT
  CASE
    WHEN building_value < 100000 THEN 'Under 100k'
    WHEN building_value BETWEEN 100000 AND 200000 THEN '100k-200k'
    WHEN building_value BETWEEN 200000 AND 300000 THEN '200k-300k'
    WHEN building_value BETWEEN 300000 AND 400000 THEN '300k-400k'
    WHEN building_value BETWEEN 400000 AND 500000 THEN '400k-500k'
    WHEN building_value BETWEEN 500000 AND 600000 THEN '500k-600k'
    WHEN building_value BETWEEN 600000 AND 700000 THEN '600k-700k'
    WHEN building_value BETWEEN 700000 AND 800000 THEN '700k-800k'
    WHEN building_value BETWEEN 800000 AND 900000 THEN '800k-900k'
    WHEN building_value BETWEEN 900000 AND 1000000 THEN '900k-1M'
    WHEN building_value BETWEEN 1000000 AND 2000000 THEN '1M-2M'
    WHEN building_value BETWEEN 2000000 AND 3000000 THEN '2M-3M'
    WHEN building_value BETWEEN 3000000 AND 4000000 THEN '3M-4M'
    WHEN building_value BETWEEN 4000000 AND 5000000 THEN '4M-5M'
    WHEN building_value BETWEEN 5000000 AND 6000000 THEN '5M-6M'
    WHEN building_value BETWEEN 6000000 AND 7000000 THEN '6M-7M'
    WHEN building_value BETWEEN 7000000 AND 8000000 THEN '7M-8M'
    WHEN building_value BETWEEN 8000000 AND 9000000 THEN '8M-9M'
    WHEN building_value BETWEEN 9000000 AND 10000000 THEN '9M-10M'
    WHEN building_value BETWEEN 10000000 AND 11000000 THEN '10M-11M'
    WHEN building_value BETWEEN 11000000 AND 12000000 THEN '11M-12M'
    WHEN building_value > 12000000 THEN 'Over 12M'
    ELSE 'Missing Building Value'
  END AS building_value_range,

  COUNT(*) AS count,

  CASE
    WHEN building_value < 100000 THEN 1
    WHEN building_value BETWEEN 100000 AND 200000 THEN 2
    WHEN building_value BETWEEN 200000 AND 300000 THEN 3
    WHEN building_value BETWEEN 300000 AND 400000 THEN 4
    WHEN building_value BETWEEN 400000 AND 500000 THEN 5
    WHEN building_value BETWEEN 500000 AND 600000 THEN 6
    WHEN building_value BETWEEN 600000 AND 700000 THEN 7
    WHEN building_value BETWEEN 700000 AND 800000 THEN 8
    WHEN building_value BETWEEN 800000 AND 900000 THEN 9
    WHEN building_value BETWEEN 900000 AND 1000000 THEN 10
    WHEN building_value BETWEEN 1000000 AND 2000000 THEN 11
    WHEN building_value BETWEEN 2000000 AND 3000000 THEN 12
    WHEN building_value BETWEEN 3000000 AND 4000000 THEN 13
    WHEN building_value BETWEEN 4000000 AND 5000000 THEN 14
    WHEN building_value BETWEEN 5000000 AND 6000000 THEN 15
    WHEN building_value BETWEEN 6000000 AND 7000000 THEN 16
    WHEN building_value BETWEEN 7000000 AND 8000000 THEN 17
    WHEN building_value BETWEEN 8000000 AND 9000000 THEN 18
    WHEN building_value BETWEEN 9000000 AND 10000000 THEN 19
    WHEN building_value BETWEEN 10000000 AND 11000000 THEN 20
    WHEN building_value BETWEEN 11000000 AND 12000000 THEN 21
    WHEN building_value > 12000000 THEN 22
    ELSE 0
  END AS sort_order
FROM nashville_housing_data
WHERE building_value IS NOT NULL
GROUP BY building_value_range, sort_order
ORDER BY sort_order;
```

- Aggregated and grouped home counts by bedrooms, bathrooms, year built, and price tiers
  
```sql
-- Pie charts
-- Looking at the percentage of houses built in each year in Nashville
-- COUNT(*) counts all rows that match either the WHERE condition or, if GROUP BY is used, all rows within each group.
-- The subquery COUNT(*) is not affected by the group by statement
SELECT year_built, COUNT(*) as count, 
COUNT(*)/(SELECT COUNT(*) FROM nashville_housing_data WHERE year_built IS NOT NULL) *100 AS percentage
FROM nashville_housing_data
WHERE year_built IS NOT NULL
GROUP BY year_built
ORDER BY percentage DESC;

-- Bar charts

-- Looking at the amount of homes with X amount of bedrooms
SELECT DISTINCT(bedrooms), COUNT(bedrooms) AS count
FROM nashville_housing_data 
WHERE bedrooms IS NOT NULL
GROUP BY bedrooms
ORDER BY bedrooms;

-- Looking at the amount of homes with X amount of full bathrooms
SELECT DISTINCT(full_baths), COUNT(full_baths) AS count
FROM nashville_housing_data
WHERE full_baths IS NOT NULL
GROUP BY full_baths
ORDER BY full_baths;
```

- Deleted irrelevant columns after transformation to finalize the dataset

- Built a Tableau dashboard to visualize metrics such as construction year distribution, bedroom counts, building value tiers, land usage, and notable high/low-value properties

- Key Concepts Demonstrated Data wrangling with SQL (CTEs, joins, aggregates, string manipulation)

- Data pipeline setup from raw CSV to structured, queryable tables

- Value binning using CASE for price brackets

- Excel techniques for NULL handling and formatting (Ctrl+H, filters, keyboard shortcuts)

### Outcome: 

This project simulates a real-world data cleaning and reporting workflow that a business analyst or data analyst would perform to prepare property data for stakeholder analysis, reporting, or integration into BI tools.


## Personal Project, COVID-19 Data Analysis and Dashboard Development

#### Covid data between 2020-02-24 to 2021-04-30: 
<https://ourworldindata.org/covid-deaths>

#### Tableau Dashboard Link: 
<https://shorturl.at/YBznK> 

![Covid Data Dashboard](/assets/image/Covid_Data_Tableau_Dashboard.png)

This project demonstrates full-cycle data exploration using SQL to analyze and uncover insights from global COVID-19 datasets. It highlights core analyst skills including data cleaning, joins, CTEs, window functions, aggregate analysis, and performance optimization using temporary tables and views. 🦠

### 🔧 Tools Used: 

- MySQL – For data import, cleaning, and in-depth querying

- CSV Files – Raw COVID-19 death and vaccination datasets

- Windows File System – Used LOAD DATA INFILE to bring data into MySQL from local CSVs

- Git/GitHub – For source control and project sharing

- Tableau

### 📁 Project Overview:

- Imported and normalized large-scale COVID-19 datasets into SQL using LOAD DATA INFILE with NULLIF() handling

```sql
-- Loading data from provided CSV files and accounting for Null spaces with NULLIF()

LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Data/coviddata/BASE1(Deaths)tocsv.csv' INTO TABLE coviddeathstable
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\r\n'
IGNORE 1 LINES
(
  @iso_code, @continent, @location, @curr_date, @population, @total_cases, @new_cases,
  @new_cases_smoothed, @total_deaths, @new_deaths, @new_deaths_smoothed,
  @total_cases_per_million, @new_cases_per_million, @new_cases_smoothed_per_million,
  @total_deaths_per_million, @new_deaths_per_million, @new_deaths_smoothed_per_million,
  @reproduction_rate, @icu_patients, @icu_patients_per_million, @hosp_patients,
  @hosp_patients_per_million, @weekly_icu_admissions, @weekly_icu_admissions_per_million,
  @weekly_hosp_admissions, @weekly_hosp_admissions_per_million
)
SET
  iso_code = NULLIF(@iso_code, ''),
  continent = NULLIF(@continent, ''),
  location = NULLIF(@location, ''),
  curr_date = NULLIF(@curr_date, ''),
  population = NULLIF(@population, ''),
  total_cases = NULLIF(@total_cases, ''),
  new_cases = NULLIF(@new_cases, ''),
  new_cases_smoothed = NULLIF(@new_cases_smoothed, ''),
  total_deaths = NULLIF(@total_deaths, ''),
  new_deaths = NULLIF(@new_deaths, ''),
  new_deaths_smoothed = NULLIF(@new_deaths_smoothed, ''),
  total_cases_per_million = NULLIF(@total_cases_per_million, ''),
  new_cases_per_million = NULLIF(@new_cases_per_million, ''),
  new_cases_smoothed_per_million = NULLIF(@new_cases_smoothed_per_million, ''),
  total_deaths_per_million = NULLIF(@total_deaths_per_million, ''),
  new_deaths_per_million = NULLIF(@new_deaths_per_million, ''),
  new_deaths_smoothed_per_million = NULLIF(@new_deaths_smoothed_per_million, ''),
  reproduction_rate = NULLIF(@reproduction_rate, ''),
  icu_patients = NULLIF(@icu_patients, ''),
  icu_patients_per_million = NULLIF(@icu_patients_per_million, ''),
  hosp_patients = NULLIF(@hosp_patients, ''),
  hosp_patients_per_million = NULLIF(@hosp_patients_per_million, ''),
  weekly_icu_admissions = NULLIF(@weekly_icu_admissions, ''),
  weekly_icu_admissions_per_million = NULLIF(@weekly_icu_admissions_per_million, ''),
  weekly_hosp_admissions = NULLIF(@weekly_hosp_admissions, ''),
  weekly_hosp_admissions_per_million = NULLIF(@weekly_hosp_admissions_per_million, '');
```

- Performed advanced SQL analysis to compute infection and death percentages, population-level vaccination rates, and temporal insights

```sql
-- Shows likelihood of death if you contract coronavirus in selected country
SELECT location, curr_date, total_cases, total_deaths, (total_deaths/total_cases)*100 AS DeathPercentage
FROM coviddeathstable WHERE location LIKE '%States%'
AND location IS NOT NULL
ORDER BY 1,2;

-- Shows percent of population infected with coronavirus in selected country
SELECT location, curr_date, total_cases, population, (total_cases/population)*100 AS PercentInfected
FROM coviddeathstable WHERE location LIKE '%States%'
AND location IS NOT NULL
ORDER BY 1,2;

-- Looking at countries with highest infection rate compared to population
SELECT location, population, MAX(total_cases) AS HighestInfectionCount, MAX((total_cases/population))*100 AS HighestPercentInfected
FROM coviddeathstable
WHERE location IS NOT NULL
GROUP BY location, population
ORDER BY HighestPercentInfected DESC;

-- Rolling highest infection rate
SELECT location, population, curr_date, MAX(total_cases) AS HighestInfectionCount, MAX((total_cases/population))*100 AS HighestPercentInfected
FROM coviddeathstable
WHERE location IS NOT NULL
GROUP BY location, population, curr_date
ORDER BY HighestPercentInfected DESC;

-- Showing countries with the highest death count per population
SELECT location, MAX(total_deaths) AS HighestDeathCount
FROM coviddeathstable
WHERE location IS NOT NULL
GROUP BY location
ORDER BY HighestDeathCount DESC;

-- Breaking things down by continent

-- Showing the continents with the highest death counts per population
SELECT continent, MAX(total_deaths) AS HighestDeathCount
FROM coviddeathstable
WHERE location IS NULL
GROUP BY continent
ORDER BY HighestDeathCount DESC;

-- Global numbers

-- World death percentage
SELECT SUM(new_cases) AS TotalCases, SUM(new_deaths) AS TotalDeaths, SUM(new_deaths)/SUM(new_cases)*100 AS WorldDeathPercentage
FROM coviddeathstable 
WHERE location IS NOT NULL
ORDER BY 1,2;
```


- Applied joins, CTEs, temporary tables, and views to structure reusable and modular analytics queries

```sql
-- Looking at total population vs vaccinations
-- Shows percentage of population that has received at least one covid vaccine

SELECT dea.continent, dea.location, dea.curr_date, dea.population, vac.new_vaccinations,
SUM(vac.new_vaccinations) OVER (PARTITION BY dea.location ORDER BY dea.location, 
dea.curr_date) AS RollingCountofVaccinations -- , RollingCountofVaccinations/population*100 AS PercentVaxxed
FROM coviddeathstable dea
JOIN covidvaccinationstable vac
	ON dea.location = vac.location
    AND dea.curr_date = vac.curr_date
WHERE dea.location IS NOT NULL AND dea.location <> 'International' AND dea.location <> 'World'
ORDER BY 2,3;

-- Using a CTE to perform calculation on Partion By in previous query

WITH PopvsVac (continent, location, curr_date, population, new_vaccinations, RollingCountofVaccinations)
AS (
SELECT dea.continent, dea.location, dea.curr_date, dea.population, vac.new_vaccinations,
SUM(vac.new_vaccinations) OVER (PARTITION BY dea.location ORDER BY dea.location, 
dea.curr_date) AS RollingCountofVaccinations -- , RollingCountofVaccinations/population*100 AS PercentVaxxed
FROM coviddeathstable dea
JOIN covidvaccinationstable vac
	ON dea.location = vac.location
    AND dea.curr_date = vac.curr_date
WHERE dea.location IS NOT NULL AND dea.location <> 'International' AND dea.location <> 'World'
-- ORDER BY 2,3
)
SELECT *, (RollingCountofVaccinations/population)*100 AS RollingPercentofVaccinations
FROM PopvsVac;

-- TEMP TABLE
-- Using a Temp Table to perform calculation on Partition By in previous query

DROP TABLE IF EXISTS percentpopulationvaccinated;
CREATE TEMPORARY TABLE percentpopulationvaccinated
(
continent varchar(255),
location varchar(255),
curr_date datetime,
population numeric,
new_vaccinations numeric,
RollingCountofVaccinations numeric
);

INSERT INTO percentpopulationvaccinated
SELECT dea.continent, dea.location, dea.curr_date, dea.population, vac.new_vaccinations,
SUM(vac.new_vaccinations) OVER (PARTITION BY dea.location ORDER BY dea.location, 
dea.curr_date) AS RollingCountofVaccinations -- , RollingCountofVaccinations/population*100 AS PercentVaxxed
FROM coviddeathstable dea
JOIN covidvaccinationstable vac
	ON dea.location = vac.location
    AND dea.curr_date = vac.curr_date;
-- WHERE dea.location IS NOT NULL AND dea.location <> 'International' AND dea.location <> 'World';
-- ORDER BY 2,3;

SELECT *, (RollingCountofVaccinations/population)*100 AS RollingPercentofVaccinations
FROM percentpopulationvaccinated;
```

- Segmented analysis by country, continent, and date for trend breakdowns

- Created views to simplify repeated access to key KPIs, including highest infection rate, rolling vaccination percentage, and global death rates

```sql
CREATE VIEW percentpopulationvaccinated AS
SELECT dea.continent, dea.location, dea.curr_date, dea.population, vac.new_vaccinations,
SUM(vac.new_vaccinations) OVER (PARTITION BY dea.location ORDER BY dea.location, 
dea.curr_date) AS RollingCountofVaccinations -- , RollingCountofVaccinations/population*100 AS PercentVaxxed
FROM coviddeathstable dea
JOIN covidvaccinationstable vac
	ON dea.location = vac.location
    AND dea.curr_date = vac.curr_date
WHERE dea.location IS NOT NULL AND dea.location <> 'International' AND dea.location <> 'World';
-- ORDER BY 2,3;

CREATE VIEW highestinfectionrate AS
-- Looking at countries with highest infection rate compared to population
SELECT location, population, MAX(total_cases) AS HighestInfectionCount, MAX((total_cases/population))*100 AS HighestPercentInfected
FROM coviddeathstable
WHERE location IS NOT NULL
GROUP BY location, population
ORDER BY HighestPercentInfected DESC;

CREATE VIEW highestdeathcount AS
-- Showing countries with the highest death count per population
SELECT location, MAX(total_deaths) AS HighestDeathCount
FROM coviddeathstable
WHERE location IS NOT NULL
GROUP BY location
ORDER BY HighestDeathCount DESC;
```

### 🧠 Key SQL Concepts Demonstrated:

- Data Cleaning: Using NULLIF() to handle missing entries on load

- Data Joins: Merged vaccination and death datasets on location and date

- Window Functions: Used SUM() OVER (PARTITION BY ...) for cumulative vaccination counts

- CTEs: Structured complex logic like percent vaccinated by population

- Temporary Tables: Stored intermediate rolling results for analysis and visualization

- Views: Persisted KPIs like highest death rates, infection rates, and vaccination coverage

- Aggregate Functions: Calculated total cases, deaths, infection % by population, and death rates

- CASE Statements: Used for readable output formatting and filtering logic

### 🧮 Views Created:

- percentpopulationvaccinated – Cumulative vaccinations by country

- highestinfectionrate – Countries with highest % of population infected

- highestdeathcount – Countries with highest death counts

- highestdeathcountbycontinent – Continent-level death tallies

- worlddeathpercentage – Global fatality ratio based on reported cases and deaths

### 🧾 Outcome: 

This project showcases practical data exploration methods used in health and crisis analytics. By using SQL to analyze real-world COVID-19 data, it simulates the responsibilities of a data analyst investigating trends in large, multi-dimensional public health datasets.

## Personal Project, Seattle Airbnb Data Cleaning & Visualization Project

### 🔗 Tableau Dashboard:
[Seattle Airbnb Tableau Dashboard](https://shorturl.at/SS0bl)

![Seattle Airbnb Tableau Dashboard](/assets/image/Seattle_Airbnb_Data_Dashboard.png)

### 📁 Dataset Source: 
[Inside Airbnb - Seattle Listings](https://insideairbnb.com/get-the-data/)

---

### 🧹 Project Summary:

This project demonstrates a full data analytics pipeline—from raw data ingestion to dashboard visualization—using Airbnb property data from Seattle. It simulates the tasks a data or business analyst might perform to deliver actionable insights for stakeholders exploring short-term rental investments or business strategies.

---

### 🛠 Tools & Technologies Used:

- **Excel** – Deduplication, formatting, NULL handling  
- **Tableau** – Building interactive dashboards and visualizing key metrics  
- **Git/GitHub** – Version control, documentation, and project hosting  

---

### 📌 Project Workflow:

- Cleaned raw data using `IFNULL`, `NULLIF`, and Excel preprocessing  
- Removed irrelevant and unused columns post-transformation  
- Built a KPI-focused dashboard with filters and interactive visuals in Tableau  

---

### 📈 Key Findings:

- **Average Guests (Accommodates):** 1.68  
- **Average Security Deposit:** $287.09  
- **Average Cleaning Fee:** $62.37  
- **Most Expensive ZIP Codes:** 98134, 98119, 98101  
- **Peak Rental Season:** Late May through winter holidays  
- **Most Common Listings:** Private rooms dominate due to affordability and demand

---

### 💡 Use Case: 

This project can inform decisions on:

- Where and when to list for maximum profitability  
- Optimal property type to rent (private rooms vs. full homes)  
- Pricing strategies based on ZIP code, accommodation size, and seasonality  
- Budgeting for cleaning fees and security deposits  
- Understanding seasonal rental behavior (e.g., renting out during peak season, living in during off-season)

---

### 📤 Outcome:

This end-to-end project demonstrates real-world analytics skills in:

- **ETL (Extract, Transform, Load)**  
- **Exploratory Data Analysis (EDA)**  
- **Data Storytelling & Dashboard Design**  

It prepares Airbnb market data for use in stakeholder presentations, BI dashboards, predictive modeling, or profitability tools.

---

## 📫 Contact

**Mikhail Ivanov**  
[LinkedIn](https://www.linkedin.com/in/mvivanov494) · [GitHub](https://github.com/badburrito24/PortfolioProjects)



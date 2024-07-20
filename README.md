# Carbon Emission Analysis
___
## What is the project about?
![cover](https://github.com/user-attachments/assets/2fc9d703-c868-443d-888e-ea9a427f18fe)

Photo by Chris LeBoutillier (unsplash.com)
This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.
___
## Data Source: Where Our Data Comes From
Our dataset is compiled from publicly available data from [nature.com](https://www.nature.com) and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).
___
## Data Structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.
![Database diagram](https://github.com/user-attachments/assets/c2e5b247-f29a-4922-9352-ecf548d11bab)

___
## Tables' column description
### Table `product_emissions`

- **`id`**: Identifier for each product emission record.
- **`company_id`**: Identifier for the company associated with the product.
- **`country_id`**: Identifier for the country where the product is being produced.
- **`industry_group_id`**: Identifier for the industry group to which the product belongs.
- **`year`**: The year in which the emissions data was recorded.
- **`product_name`**: The name of the product associated with the emissions data.
- **`weight_kg`**: The weight of the product in kilograms.
- **`carbon_footprint_pcf`**: The carbon footprint of the product, measured in CO2 equivalent.
- **`upstream_percent_total_pcf`**: The percentage of the total carbon footprint attributed to upstream activities.
- **`operations_percent_total_pcf`**: The percentage of the total carbon footprint attributed to operations.
- **`downstream_percent_total_pcf`**: The percentage of the total carbon footprint attributed to downstream activities.

First 10 records of table `product_emissions`

|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|
|10261-3-2017|14|16|25|2017|Multifunction Printers|110.0|2274|20.05|3.61|76.34|
|10324-1-2016|15|16|19|2016|KURALON  fiber|1500.0|10000|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10418-1-2013|84|9|19|2013|Portland Cement|1000.0|1102|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10661-10-2014|85|28|11|2014|Regular Straight 505® Jeans – Steel (Water<Less™)|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10661-10-2015|85|28|6|2015|Regular Straight 505® Jeans – Steel (Water<Less™)|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|


### Table `industry_groups`

- **`id`**: Unique identifier for each industry group.
- **`industry_group`**: The name of the industry group, categorizing businesses within similar sectors based on their products or services offered.

First 10 rows of table `industry_groups`

|id|industry_group|
|--|--------------|
|1|"Consumer Durables, Household and Personal Products"|
|2|"Food, Beverage & Tobacco"|
|3|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|
|4|"Mining - Iron, Aluminum, Other Metals"|
|5|"Pharmaceuticals, Biotechnology & Life Sciences"|
|6|"Textiles, Apparel, Footwear and Luxury Goods"|
|7|Automobiles & Components|
|8|Capital Goods|
|9|Chemicals|
|10|Commercial & Professional Services|

### Table `companies`

- **`id`**: Unique identifier for each company.
- **`company_name`**: The name of the company, identifying the specific organization within the dataset.

First 10 rows of table `companies`

|id|company_name|
|--|------------|
|1|"Autodesk, Inc."|
|2|"Casio Computer Co., Ltd."|
|3|"Cisco Systems, Inc."|
|4|"CNX Coal Resources, LP"|
|5|"Coca-Cola Enterprises, Inc."|
|6|"Compañía Española de Petróleos, S.A.U. CEPSA"|
|7|"Daikin Industries, Ltd."|
|8|"Elitegroup computer systems co., Ltd."|
|9|"Fuji Xerox Co., Ltd."|
|10|"Gamesa Corporación Tecnológica, S.A."|

### Table `countries`

- **`id`**: Unique identifier for each country.
- **`country_name`**: The name of the country.

First 10 rows of table `countries`

|id|country_name|
|--|------------|
|1|Australia|
|2|Belgium|
|3|Brazil|
|4|Canada|
|5|Chile|
|6|China|
|7|Colombia|
|8|Finland|
|9|France|
|10|Germany|
___
## Question to research

### Which products contribute the most to carbon emissions? What are the industry groups of these products?

SQL query
```sql
SELECT
    product_name
    , REPLACE(company_name,"\"","") as company_name
    , country_name
    , REPLACE (industry_group,"\"","") as industry_group
    , year
    , weight_kg
    , carbon_footprint_pcf
FROM product_emissions pe
JOIN companies c ON c.id = pe.company_id
JOIN industry_groups ig ON ig.id = pe.industry_group_id
JOIN countries c2 ON c2.id = pe.country_id
ORDER BY carbon_footprint_pcf DESC
LIMIT 3;
```
Result

|product_name|company_name|country_name|industry_group|year|weight_kg|carbon_footprint_pcf|
|------------|------------|------------|--------------|----|---------|--------------------|
|Wind Turbine G128 5 Megawats|"Gamesa Corporación Tecnológica, S.A."|Spain|Electrical Equipment and Machinery|2015|600,000|3,718,044|
|Wind Turbine G132 5 Megawats|"Gamesa Corporación Tecnológica, S.A."|Spain|Electrical Equipment and Machinery|2015|600,000|3,276,187|
|Wind Turbine G114 2 Megawats|"Gamesa Corporación Tecnológica, S.A."|Spain|Electrical Equipment and Machinery|2015|400,000|1,532,608|

___
### What are the industries with the highest contribution to carbon emissions?

SQL query
```sql
SELECT 
    REPLACE (industry_group,"\"","") as industry_group, 
    SUM(carbon_footprint_pcf) AS total_pcf
FROM (
    SELECT DISTINCT 
        industry_group_id, 
        product_name, 
        company_id, 
        country_id, 
        year, 
        weight_kg, 
        carbon_footprint_pcf
    FROM product_emissions
) pe
join industry_groups ig on ig.id = pe.industry_group_id
GROUP BY industry_group_id
order by total DESC
limit 3;
```
Result

|industry_group|total_pcf|
|--------------|-----|
|Electrical Equipment and Machinery|9,801,558|
|Automobiles & Components|2,582,264|
|Materials|430,199|

___
### What are the companies with the highest contribution to carbon emissions?

SQL query
```sql
SELECT 
    REPLACE(company_name,"\"","") as company_name, 
    SUM(carbon_footprint_pcf) AS total_pcf
FROM (
    SELECT DISTINCT 
        industry_group_id, 
        product_name, 
        company_id, 
        country_id, 
        year, 
        weight_kg, 
        carbon_footprint_pcf
    FROM product_emissions
) pe
join companies c  on c.id = pe.company_id
GROUP BY company_id
order by total DESC
limit 3;
```
Result

|company_name|total_pcf|
|------------|-----|
|Gamesa Corporación Tecnológica, S.A.|9,778,464|
|Daimler AG|1,594,300|
|Volkswagen AG|655,960|
___
### What are the countries with the highest contribution to carbon emissions?
SQL query

```sql
SELECT 
    country_name, 
    SUM(carbon_footprint_pcf) AS total_pcf
FROM (
    SELECT DISTINCT 
        industry_group_id, 
        product_name, 
        company_id, 
        country_id, 
        year, 
        weight_kg, 
        carbon_footprint_pcf
    FROM product_emissions
) pe
join countries c  on c.id = pe.country_id
GROUP BY country_id
order by total_pcf DESC
limit 3;
```

Result

|country_name|total_pcf|
|------------|---------|
|Spain|9,786,127|
|Germany|2,251,225|
|Japan|519,339|

___
What is the trend of carbon footprints (PCFs) over the years?
SQL query
```sql
SELECT 
    year, 
    SUM(carbon_footprint_pcf) AS total_pcf
FROM (
    SELECT DISTINCT 
        industry_group_id, 
        product_name, 
        company_id, 
        country_id, 
        year, 
        weight_kg, 
        carbon_footprint_pcf
    FROM product_emissions
) pe
GROUP BY year;
```
Result

|year|total_pcf|
|----|---------|
|2013|496,076|
|2014|548,229|
|2015|10,810,407|
|2016|1,612,755|
|2017|228,522|

The data shows a significant spike in carbon footprint in 2015, likely due to reporting changes or anomalies, followed by a sharp decline in subsequent years. This suggests that the 2015 value may be an outlier or anomaly, and the overall trend indicates a reduction in carbon footprint in the following years.
___
### Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
SQL query
```sql
SELECT 
    year,
    REPLACE(industry_group,'\"','') as industry_group,
    SUM(carbon_footprint_pcf) AS total_pcf
FROM (
    SELECT DISTINCT 
        industry_group_id, 
        product_name, 
        company_id, 
        country_id, 
        year, 
        weight_kg, 
        carbon_footprint_pcf
    FROM product_emissions
) pe
join industry_groups ig  on ig.id = pe.industry_group_id
GROUP BY year, industry_group 
order by industry_group, year

```
Result


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
    , REPLACE(company_name,'\"','') as company_name
    , REPLACE (industry_group,'\"','') as industry_group
    , round(carbon_footprint_pcf,2) as carbon_footprint_pcf
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
JOIN companies c ON c.id = pe.company_id
JOIN industry_groups ig ON ig.id = pe.industry_group_id
JOIN countries c2 ON c2.id = pe.country_id
ORDER BY carbon_footprint_pcf DESC
LIMIT 10;
```
Result

|product_name|company_name|industry_group|carbon_footprint_pcf|
|------------|------------|--------------|--------------------|
|Wind Turbine G128 5 Megawats|Gamesa Corporación Tecnológica, S.A.|Electrical Equipment and Machinery|3718044|
|Wind Turbine G132 5 Megawats|Gamesa Corporación Tecnológica, S.A.|Electrical Equipment and Machinery|3276187|
|Wind Turbine G114 2 Megawats|Gamesa Corporación Tecnológica, S.A.|Electrical Equipment and Machinery|1532608|
|Wind Turbine G90 2 Megawats|Gamesa Corporación Tecnológica, S.A.|Electrical Equipment and Machinery|1251625|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|Hino Motors, Ltd.|Automobiles & Components|191687|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|Arcelor Mittal|Materials|167000|
|TCDE|Mitsubishi Gas Chemical Company, Inc.|Materials|99075|
|Mercedes-Benz GLE (GLE 500 4MATIC)|Daimler AG|Automobiles & Components|91000|
|Electric Motor|Weg S/A|Capital Goods|87589|
|Mercedes-Benz S-Class (S 500)|Daimler AG|Automobiles & Components|85000|

We can see here that the production of wind turbines, despite their role in renewable energy, has a significant carbon footprint.
The majority of the top carbon-emitting products are from the Electrical Equipment and Machinery and Automobiles & Components industries. This indicates that these sectors have high emissions associated with their products.
This suggests that while materials and capital goods do contribute to carbon emissions, their impact is relatively less significant compared to high-tech machinery.
___
### What are the industries with the highest contribution to carbon emissions?

SQL query
```sql
SELECT 
    REPLACE (industry_group,'\"','') as industry_group, 
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

The Electrical Equipment and Machinery industry is the largest contributor to carbon emissions, accounting for *9,801,558 pcf*, significantly surpassing Automobiles & Components at *2,582,264 pcf*, and Materials at *430,199 pcf*. This indicates that the Electrical Equipment and Machinery sector substantially impacts overall carbon emissions among the industries analyzed.
___
### What are the companies with the highest contribution to carbon emissions?

SQL query
```sql
SELECT 
    REPLACE(company_name,'\"','') as company_name, 
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

The company with the highest contribution to carbon emissions is Gamesa Corporación Tecnológica, S.A., with a total of *9,778,464 pcf*, followed by Daimler AG with *1,594,300 pcf*, and Volkswagen AG with *655,960 pcf*. This highlights that Gamesa Corporación Tecnológica, S.A. has the largest impact on carbon emissions compared to the other companies listed.
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
To demonstrate, we only count industry_group that has been record at least 3 years in the dataset
SQL query
```sql
WITH data AS (
    SELECT 
        year,
        REPLACE(industry_group, '\"', '') AS industry_group,
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
    JOIN industry_groups ig ON ig.id = pe.industry_group_id
    GROUP BY year, industry_group
)
SELECT 
    year, 
    industry_group, 
    total_pcf AS total_emission_pcf
FROM data
WHERE industry_group IN (
    SELECT
        industry_group
    FROM (
        SELECT
            industry_group,
            COUNT(DISTINCT year) AS year_count
        FROM data
        GROUP BY industry_group
    ) subquery
    WHERE year_count >= 3
);
```
Result

|year|industry_group|total_emission_pcf|
|----|--------------|------------------|
|2013|Automobiles & Components|130189|
|2014|Automobiles & Components|230015|
|2015|Automobiles & Components|817227|
|2016|Automobiles & Components|1404833|
|2013|Capital Goods|60117|
|2014|Capital Goods|93699|
|2015|Capital Goods|3505|
|2016|Capital Goods|6369|
|2017|Capital Goods|94943|
|2013|Commercial & Professional Services|817|
|2014|Commercial & Professional Services|477|
|2016|Commercial & Professional Services|2890|
|2017|Commercial & Professional Services|741|
|2013|Consumer Durables & Apparel|2860|
|2014|Consumer Durables & Apparel|3123|
|2016|Consumer Durables & Apparel|1114|
|2014|Food & Staples Retailing|773|
|2015|Food & Staples Retailing|706|
|2016|Food & Staples Retailing|2|
|2013|Food, Beverage & Tobacco|4308|
|2014|Food, Beverage & Tobacco|2023|
|2015|Food, Beverage & Tobacco|0|
|2016|Food, Beverage & Tobacco|99639|
|2017|Food, Beverage & Tobacco|3162|
|2013|Materials|194464|
|2014|Materials|66719|
|2016|Materials|61887|
|2017|Materials|107129|
|2013|Media|9645|
|2014|Media|9645|
|2015|Media|1919|
|2016|Media|1808|
|2013|Software & Services|3|
|2014|Software & Services|143|
|2015|Software & Services|22851|
|2016|Software & Services|22846|
|2017|Software & Services|690|
|2013|Technology Hardware & Equipment|60539|
|2014|Technology Hardware & Equipment|101153|
|2015|Technology Hardware & Equipment|93807|
|2016|Technology Hardware & Equipment|1280|
|2017|Technology Hardware & Equipment|21857|
|2013|Telecommunication Services|52|
|2014|Telecommunication Services|183|
|2015|Telecommunication Services|183|

We draws some conclusion about this question

1. **Capital Goods**
   - **Peak**: 93,699 pcf in 2014
   - **Drop**: To 6,369 pcf in 2016, then increased to 94,943 pcf in 2017
   - **Trend**: Significant decrease followed by a notable increase

2. **Food & Staples Retailing**
   - **Peak**: 773 pcf in 2014
   - **Drop**: To 2 pcf in 2016
   - **Trend**: Sharp decline to a minimal value before rising again

3. **Food, Beverage & Tobacco**
   - **Peak**: 99,639 pcf in 2016
   - **Drop**: To 3,162 pcf in 2017
   - **Trend**: Significant drop after a peak

4. **Technology Hardware & Equipment**
   - **Peak**: 101,153 pcf in 2014
   - **Drop**: To 1,280 pcf in 2016, then increased to 21,857 pcf in 2017
   - **Trend**: Major reduction followed by a partial rebound

___
## Insight and patterns
During the exploration, the following facts and patterns were identified:
- The product produces the highest level of carbon emissions typically in heavy industry and comes from Machinery, Automobiles & Components, and Materials.
- Car models that are leading in carbon emissions during production: **Land Cruiser Prado, Mercedes-Benz GLA, Mercedes-Benz S-Class**
- Most of the countries leading the carbon emissions are from Europe, indicating that, on average, air equality in euro is lower than in other continents.
- According to the data, there was a significant spike in carbon emissions levels in 2015.
- There was a downtrend in CO2 emissions after 2015.

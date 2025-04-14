# Carbon-Emission-Analysis
## 1. Introduction
This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

![image](https://github.com/user-attachments/assets/34fca1ff-74ef-4c81-a360-6b31f1d5ee0f)
### 1.1 Data Model
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.

![image](https://github.com/user-attachments/assets/e29cfa48-8d15-44a2-9841-71d94256ef16)
### 1.2 Data Structure
Table 'product_emissions'
```sql
SELECT * FROM product_emissions LIMIT 10;
```
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

Table 'industry_groups'
```sql
SELECT * FROM industry_groups LIMIT 10;
```
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

Table 'companies'
```sql
SELECT * FROM companies LIMIT 10;
```
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

Table 'countries'
```sql
SELECT * FROM countries  LIMIT 10;
```
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

## 2. Data Explore
Data duplicate
```sql
SELECT *,
	COUNT(*) AS duplicate_count
FROM product_emissions  
GROUP BY  
id,
company_id,
industry_group_id,
year,
product_name,
weight_kg,
carbon_footprint_pcf,
upstream_percent_total_pcf,
operations_percent_total_pcf,
downstream_percent_total_pcf
HAVING COUNT(*) >1
LIMIT 10;
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|duplicate_count|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|---------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|2|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|2|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|2|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|2|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|2|
|10261-3-2017|14|16|25|2017|Multifunction Printers|110.0|2274|20.05|3.61|76.34|2|
|10324-1-2016|15|16|19|2016|KURALON  fiber|1500.0|10000|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10418-1-2013|84|9|19|2013|Portland Cement|1000.0|1102|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-1-2014|85|28|11|2014|501® Original Jeans – Dark Stonewash|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-1-2015|85|28|6|2015|501® Original Jeans – Dark Stonewash|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|

Duplicate result
```sql
SELECT COUNT(product_name) as 'Total number of products',
			COUNT(DISTINCT product_name) as 'Number of unique products'
FROM product_emissions pe ;
```
|Total number of products|Number of unique products|
|------------------------|-------------------------|
|1037|661|

## 3. Data Analysis 
3.1 Which products contribute the most to carbon emissions?
```sql
SELECT product_name,
	ROUND(AVG(carbon_footprint_pcf),2) AS 'Average PCF'
FROM product_emissions
GROUP BY product_name 
ORDER BY carbon_footprint_pcf DESC
LIMIT 10;
```
|product_name|Average PCF|
|------------|-----------|
|Wind Turbine G128 5 Megawats|3718044.00|
|Wind Turbine G132 5 Megawats|3276187.00|
|Wind Turbine G114 2 Megawats|1532608.00|
|Wind Turbine G90 2 Megawats|1251625.00|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.00|
|TCDE|99075.00|
|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.00|
|Mercedes-Benz S-Class (S 500)|85000.00|
|Mercedes-Benz SL (SL 350)|72000.00|

3.1.1 Conclusion:

In the list of products, Wind Turbines have the highest PCF (Product Carbon Footprint), indicating that the production and operation of these wind turbines generate a large amount of carbon emissions, even though they are renewable energy products.
On the other hand, products such as Mercedes-Benz cars and Land Cruiser Prado have lower PCF values, ranging from 72,000 to 191,687, showing that while these products have considerable carbon emissions, they are still lower than the large wind turbines.

3.2 What are the industry groups of these products?
```sql
SELECT ig.industry_group,
	pe.product_name,

	ROUND(AVG(pe.carbon_footprint_pcf),2) AS 'Average PCF'
FROM product_emissions pe
JOIN industry_groups ig ON ig.id = pe.industry_group_id
GROUP BY product_name
ORDER BY carbon_footprint_pcf DESC
LIMIT 10;
```
|industry_group|product_name|Average PCF|
|--------------|------------|-----------|
|Electrical Equipment and Machinery|Wind Turbine G128 5 Megawats|3718044.00|
|Electrical Equipment and Machinery|Wind Turbine G132 5 Megawats|3276187.00|
|Electrical Equipment and Machinery|Wind Turbine G114 2 Megawats|1532608.00|
|Electrical Equipment and Machinery|Wind Turbine G90 2 Megawats|1251625.00|
|Automobiles & Components|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|
|Materials|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.00|
|Materials|TCDE|99075.00|
|Automobiles & Components|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.00|
|Automobiles & Components|Mercedes-Benz S-Class (S 500)|85000.00|
|Automobiles & Components|Mercedes-Benz SL (SL 350)|72000.00|

3.2.1 Conclusion:

The table above shows the top 10 industry groups with the highest average PCF.
Among them, the Electrical Equipment and Machinery industry group has the highest average PCF compared to other industry groups.
Products in the Automobiles & Components industry group have a much lower average PCF than those in other industry groups.

3.3 What are the industries with the highest contribution to carbon emissions?
```sql
SELECT ig.industry_group,
	ROUND(SUM(pe.carbon_footprint_pcf),2) AS 'Total PCF'
FROM product_emissions pe
JOIN industry_groups ig ON ig.id = pe.industry_group_id
GROUP BY ig.industry_group
ORDER BY ROUND(SUM(pe.carbon_footprint_pcf),2) DESC
LIMIT 10;
```
|industry_group|Average PCF|
|--------------|-----------|
|Electrical Equipment and Machinery|9801558.00|
|Automobiles & Components|2582264.00|
|Materials|577595.00|
|Technology Hardware & Equipment|363776.00|
|Capital Goods|258712.00|
|"Food, Beverage & Tobacco"|111131.00|
|"Pharmaceuticals, Biotechnology & Life Sciences"|72486.00|
|Chemicals|62369.00|
|Software & Services|46544.00|
|Media|23017.00|

3.3.1 Conclusion:

The table above shows the top 10 industries with the highest total carbon emissions.
Among them, the Electrical Equipment and Machinery industry has the highest total PCF with a value of 9,801,558.00.
The Media industry has the lowest total PCF among all industries, with a value of 23,017.00.

3.4 What are the companies with the highest contribution to carbon emissions?
```sql
SELECT c.company_name, 
       ROUND(SUM(pe.carbon_footprint_pcf), 2) AS 'Total PCF'
FROM product_emissions pe
JOIN companies c ON c.id = pe.company_id
GROUP BY c.company_name
ORDER BY `Total PCF` DESC
LIMIT 10;
```
|company_name|Total PCF|
|------------|---------|
|"Gamesa Corporación Tecnológica, S.A."|9778464.00|
|Daimler AG|1594300.00|
|Volkswagen AG|655960.00|
|"Mitsubishi Gas Chemical Company, Inc."|212016.00|
|"Hino Motors, Ltd."|191687.00|
|Arcelor Mittal|167007.00|
|Weg S/A|160655.00|
|General Motors Company|137007.00|
|"Lexmark International, Inc."|132012.00|
|"Daikin Industries, Ltd."|105600.00|

3.4.1 Conclusion:

The table above shows the top 10 companies with the highest carbon emissions.
Among them, the company with the highest total PCF is Gamesa Corporación Tecnológica, S.A., with carbon emissions reaching 9,778,464.00.
The company with the lowest total PCF is Daikin Industries, Ltd., with carbon emissions of 105,600.00.

3.5 What are the countries with the highest contribution to carbon emissions?
```sql
SELECT co.country_name, 
       ROUND(SUM(pe.carbon_footprint_pcf), 2) AS 'Total PCF'
FROM product_emissions pe
JOIN countries co ON co.id = pe.country_id
GROUP BY co.country_name
ORDER BY `Total PCF` DESC
LIMIT 10;
```
|country_name|Total PCF|
|------------|---------|
|Spain|9786130.00|
|Germany|2251225.00|
|Japan|653237.00|
|USA|518381.00|
|South Korea|186965.00|
|Brazil|169337.00|
|Luxembourg|167007.00|
|Netherlands|70417.00|
|Taiwan|62875.00|
|India|24574.00|

3.5.1 Conclusion:

The table above shows the top 10 countries with the highest total carbon emissions.
Among them, Spain has the highest total carbon emissions, with a value of 9,786,130.00.
Meanwhile, India has the lowest total carbon emissions within the top 10, with a value of 24,574.00.

3.6 What is the trend of carbon footprints (PCFs) over the years?
```sql
SELECT year, 
       ROUND(SUM(carbon_footprint_pcf), 2) AS 'Total PCF'
FROM product_emissions
GROUP BY year
ORDER BY year ASC;
```
|year|Total PCF|
|----|---------|
|2013|503857.00|
|2014|624226.00|
|2015|10840415.00|
|2016|1640182.00|
|2017|340271.00|

3.6.1 Conclusion:
The total carbon emissions change unevenly from year to year. The data table above shows the total carbon emissions from 2013 to 2017. In 2013, the total PCF was 503,857.00, and by 2017, it had decreased to 340,271.00, indicating a gradual reduction in carbon emissions over the four years. However, in 2015 and 2016, there were sudden spikes in total emissions, with values of 10,840,415.00 for 2015 and 1,640,182.00 for 2016. This suggests that the control of carbon emissions was very effective, leading to a rapid decrease in emissions.

3.7 Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
```sql
SELECT 
    ig.industry_group AS 'Industry Group',
    ROUND(SUM(CASE WHEN pe.year = 2013 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2013 Emission',
    ROUND(SUM(CASE WHEN pe.year = 2014 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2014 Emission',
    ROUND(SUM(CASE WHEN pe.year = 2015 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2015 Emission',
    ROUND(SUM(CASE WHEN pe.year = 2016 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2016 Emission',
    ROUND(SUM(CASE WHEN pe.year = 2017 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2017 Emission'
FROM product_emissions AS pe
LEFT JOIN industry_groups AS ig ON ig.id = pe.industry_group_id
GROUP BY ig.industry_group
ORDER BY 
    `2017 Emission`, 
    `2016 Emission`,
    `2015 Emission`,
    `2014 Emission`,
    `2013 Emission`;
```
|Industry Group|2013 Emission|2014 Emission|2015 Emission|2016 Emission|2017 Emission|
|--------------|-------------|-------------|-------------|-------------|-------------|
|Household & Personal Products|0.00|0.00|0.00|0.00|0.00|
|"Pharmaceuticals, Biotechnology & Life Sciences"|32271.00|40215.00|0.00|0.00|0.00|
|Tobacco|0.00|0.00|1.00|0.00|0.00|
|Semiconductors & Semiconductors Equipment|0.00|0.00|3.00|0.00|0.00|
|Retailing|0.00|19.00|11.00|0.00|0.00|
|Gas Utilities|0.00|0.00|122.00|0.00|0.00|
|Food & Beverage Processing|0.00|0.00|141.00|0.00|0.00|
|Telecommunication Services|52.00|183.00|183.00|0.00|0.00|
|Trading Companies & Distributors and Commercial Services & Supplies|0.00|0.00|239.00|0.00|0.00|
|"Textiles, Apparel, Footwear and Luxury Goods"|0.00|0.00|387.00|0.00|0.00|
|"Consumer Durables, Household and Personal Products"|0.00|0.00|931.00|0.00|0.00|
|Tires|0.00|0.00|2022.00|0.00|0.00|
|Containers & Packaging|0.00|0.00|2988.00|0.00|0.00|
|"Mining - Iron, Aluminum, Other Metals"|0.00|0.00|8181.00|0.00|0.00|
|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|0.00|0.00|8909.00|0.00|0.00|
|Chemicals|0.00|0.00|62369.00|0.00|0.00|
|Electrical Equipment and Machinery|0.00|0.00|9801558.00|0.00|0.00|
|Food & Staples Retailing|0.00|773.00|706.00|2.00|0.00|
|Semiconductors & Semiconductor Equipment|0.00|50.00|0.00|4.00|0.00|
|Utilities|122.00|0.00|0.00|122.00|0.00|
|Consumer Durables & Apparel|2867.00|3280.00|0.00|1162.00|0.00|
|Media|9645.00|9645.00|1919.00|1808.00|0.00|
|Energy|750.00|0.00|0.00|10024.00|0.00|
|Automobiles & Components|130189.00|230015.00|817227.00|1404833.00|0.00|
|Software & Services|6.00|146.00|22856.00|22846.00|690.00|
|Commercial & Professional Services|1157.00|477.00|0.00|2890.00|741.00|
|"Food, Beverage & Tobacco"|4995.00|2685.00|0.00|100289.00|3162.00|
|Technology Hardware & Equipment|61100.00|167361.00|106157.00|1566.00|27592.00|
|Capital Goods|60190.00|93699.00|3505.00|6369.00|94949.00|
|Materials|200513.00|75678.00|0.00|88267.00|213137.00|

3.7.1 Conclusion:
The data table above shows that certain industry groups have demonstrated a significant reduction in carbon emissions from 2013 to 2017. Among them, the Automobiles & Components industry group had carbon emissions of 130,189.00 in 2013. However, during 2015 and 2016, emissions spiked to 817,227.00 in 2015 and 1,404,833.00 in 2016. Despite this, by 2017, the group had reduced its carbon emissions to 0, proving that it achieved the most significant reduction in carbon emissions.

The Electrical Equipment and Machinery industry group had 0 emissions in 2013, but experienced a sharp increase in 2015, reaching 9,801,558.00. Nonetheless, by 2017, emissions had been reduced back to 0, showing that this group also performed very well in significantly lowering carbon emissions.

On the other hand, the Capital Goods, Materials, and Software & Services industry groups did not demonstrate a significant reduction in carbon emissions. Instead, their emissions tended to increase year by year, indicating a lack of effective emission control.
















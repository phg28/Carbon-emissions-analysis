# Carbon-emissions-analysis

![Photo by Chris LeBoutillier (unsplash.com)](https://github.com/RusAbk/Carbon-Emission-Analysis/blob/main/cover.jpg?raw=true)

Photo by Chris LeBoutillier (unsplash.com)

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends. Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters. Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

## Data Source: Where Our Data Comes From
Our dataset is compiled from publicly available data from [nature.com](https://nature.com) and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

## Data Structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.

![Database diagram](https://github.com/RusAbk/Carbon-Emission-Analysis/blob/main/Database%20diagram.png?raw=true)

## Join tables and Clean data
Create a temporary table with joining data

```sql
with t1 as
(SELECT p.id, p.company_id, p.country_id,p.industry_group_id, p.year, p.product_name, p.weight_kg, p.carbon_footprint_pcf, 
 p.upstream_percent_total_pcf, p.operations_percent_total_pcf, p.downstream_percent_total_pcf,
 cp.company_name, ct.country_name, i.industry_group FROM product_emissions p
left join companies cp ON p.company_id=cp.id
left join countries ct ON p.country_id=ct.id
left join industry_groups i ON p.industry_group_id=i.id)

select * from t1 limit 10
```

| id            | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf                       | operations_percent_total_pcf                     | downstream_percent_total_pcf                     | company_name           | country_name | industry_group                                 | 
| ------------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -----------------------------------------------: | -----------------------------------------------: | -----------------------------------------------: | ---------------------: | -----------: | ---------------------------------------------: | 
| 10056-1-2014  | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | Kellogg Company        | USA          | "Food, Beverage & Tobacco"                     | 
| 10056-1-2015  | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | Kellogg Company        | USA          | Food & Beverage Processing                     | 
| 10222-1-2013  | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                                            | 17.36                                            | 2.01                                             | KNOLL INC              | USA          | Capital Goods                                  | 
| 10261-1-2017  | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                                            | 5.51                                             | 63.84                                            | "Konica Minolta, Inc." | Japan        | Technology Hardware & Equipment                | 
| 10261-2-2017  | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                                            | 4.51                                             | 70.41                                            | "Konica Minolta, Inc." | Japan        | Technology Hardware & Equipment                | 
| 10261-3-2017  | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 2274                 | 20.05                                            | 3.61                                             | 76.34                                            | "Konica Minolta, Inc." | Japan        | Technology Hardware & Equipment                | 
| 10324-1-2016  | 15         | 16         | 19                | 2016 | KURALON  fiber                                                  | 1500      | 10000                | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | "Kuraray Co., Ltd."    | Japan        | Materials                                      | 
| 10418-1-2013  | 84         | 9          | 19                | 2013 | Portland Cement                                                 | 1000      | 1102                 | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | Lafarge S.A.           | France       | Materials                                      | 
| 10661-10-2014 | 85         | 28         | 11                | 2014 | Regular Straight 505® Jeans – Steel (Water                      | 0.7665    | 15                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | Levi Strauss & Co.     | USA          | Consumer Durables & Apparel                    | 
| 10661-10-2015 | 85         | 28         | 6                 | 2015 | Regular Straight 505® Jeans – Steel (Water                      | 0.7665    | 15                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | Levi Strauss & Co.     | USA          | "Textiles, Apparel, Footwear and Luxury Goods" | 



## Analysis


### Top 10 Products with highest Carbon emissions

```select product_name, year, weight_kg, carbon_footprint_pcf, industry_group from t1 
group by id
order by carbon_footprint_pcf DESC
limit 10
```
| product_name                                                                                                                       | year | weight_kg | carbon_footprint_pcf | industry_group                     | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---: | --------: | -------------------: | ---------------------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 2015 | 600000    | 3718044              | Electrical Equipment and Machinery | 
| Wind Turbine G132 5 Megawats                                                                                                       | 2015 | 600000    | 3276187              | Electrical Equipment and Machinery | 
| Wind Turbine G114 2 Megawats                                                                                                       | 2015 | 400000    | 1532608              | Electrical Equipment and Machinery | 
| Wind Turbine G90 2 Megawats                                                                                                        | 2015 | 361000    | 1251625              | Electrical Equipment and Machinery | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 2016 | 2272.33   | 191687               | Automobiles & Components           | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 2013 | 140000    | 167000               | Materials                          | 
| TCDE                                                                                                                               | 2017 | 12000     | 99075                | Materials                          | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 2016 | 3500      | 91000                | Automobiles & Components           | 
| Electric Motor                                                                                                                     | 2014 | 90        | 87589                | Capital Goods                      | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 2016 | 2050      | 85000                | Automobiles & Components           | 

Data showed that products contribute the most carbon emissions: Wind turbine, Land Cruiser, Retaining wall, TCDE, Electric Motor and Mercedes-Benz cars. It aslo can be seen that some of products coming from same industry. Now let's see which industries emitted most carbons. 



### Top 10 industries with highest carbon emissions
```sql
select industry_group as Industry, sum(pcf) as Total_pcf 
from t1
group by industry_group
order by Total_pcf DESC limit 10
```

| Industry                                         | Total_pcf | 
| -----------------------------------------------: | --------: | 
| Electrical Equipment and Machinery               | 9801558   | 
| Automobiles & Components                         | 2582264   | 
| Materials                                        | 430199    | 
| Technology Hardware & Equipment                  | 278650    | 
| Capital Goods                                    | 258633    | 
| "Food, Beverage & Tobacco"                       | 109132    | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486     | 
| Software & Services                              | 46533     | 
| Chemicals                                        | 44939     | 
| Media                                            | 23017     | 

Data showed that the industry with highest carbon emission is Electrical Equipment and Machinery, followed by Automobiles. It is noticeable that the Electrical industry has a much higher carbon footprint than other industries, more than 4 times higher than the second highest industry.



### Top 10 countries with highest carbon emissions

```sql
select country_name as Country, sum(pcf) as Total_pcf from t1 
group by country_name
order by Total_pcf DESC
limit 10
```

| Country     | Total_pcf | 
| ----------: | --------: | 
| Spain       | 9786127   | 
| Germany     | 2251225   | 
| Japan       | 519348    | 
| USA         | 451867    | 
| Brazil      | 167587    | 
| Luxembourg  | 167007    | 
| South Korea | 140995    | 
| Netherlands | 70417     | 
| Taiwan      | 61511     | 
| India       | 24574     | 


It can be seen that the 2 countries with the highest PCF index are both from Europe, followed by Japan of Asia. America has 2 countries in the top 5: USA and Brazil. 
With the PCF amount of Spain and Germany far surpassing that of the remaining countries, it seems that Europe is the continent with a much higher PCF level than others.


### Top 10 companies with highest carbon emissions

```sql
select company_name, industry_group, round(sum(pcf),2) as Total_pcf from t1
group by company_name
order by Total_pcf DESC
limit 10
```

| company_name                            | industry_group                     | Total_pcf | 
| --------------------------------------: | ---------------------------------: | --------: | 
| "Gamesa Corporación Tecnológica, S.A."  | Electrical Equipment and Machinery | 9778464   | 
| Daimler AG                              | Automobiles & Components           | 1594300   | 
| Volkswagen AG                           | Automobiles & Components           | 655960    | 
| "Hino Motors, Ltd."                     | Automobiles & Components           | 191687    | 
| Arcelor Mittal                          | Materials                          | 167007    | 
| Weg S/A                                 | Capital Goods                      | 160655    | 
| General Motors Company                  | Automobiles & Components           | 137007    | 
| "Mitsubishi Gas Chemical Company, Inc." | Materials                          | 106008    | 
| "Daikin Industries, Ltd."               | Capital Goods                      | 105600    | 
| CJ Cheiljedang                          | "Food, Beverage & Tobacco"         | 94817     | 

Only Gamesa Corporación Tecnológica, S.A. comes from Electrical industry, and is also a company with a huge carbon footprint compared to companies from other industries.
While the carbon footprint of the automobiles industry is contributed by many companies.

### PCFs of 3 producing stages

```sql
select round(sum(upstream_percent_total_pcf),2) as upstream_PCFs
     , round(sum(operations_percent_total_pcf),2) as operation_PCFs 
	 , round(sum(downstream_percent_total_pcf),2) as downstream_PCFs 
from t1

```

| upstream_PCFs | operation_PCFs | downstream_PCFs | 
| ------------: | -------------: | --------------: | 
| 18744.81      | 9739.07        | 13616.19        | 

The largest amount of carbon emissions comes from upstream activities, followed by downstream and then operations. This shows that while operating, the least amount of carbon is emitted.

### PCFs trend over the years

```sql
select year, sum(pcf) as Total_pcf from t1
group by year
order by year
```

| year | Total_pcf | 
| ---: | --------: | 
| 2013 | 496076    | 
| 2014 | 548229    | 
| 2015 | 10810407  | 
| 2016 | 1612760   | 
| 2017 | 228531    | 

![image](https://github.com/user-attachments/assets/d154ea1e-c1db-4a0a-9e37-4fab9702c740)

During the period from 2013 to 2017, the PCFs trendline could be divided into 3 parts:

.) In 2013 and 2014, The amount of PCF did not differ significantly, at low levels. 

.) The PCF rocketed in 2015.

.) PCFs fell sharply in 2016 and continued to decrease in 2017.


### Carbon emission trend of industry groups
```sql
select year, industry_group as Industry, sum(pcf) as Total_pcf 
from t1
group by year, industry_group
order by industry_group, year
```

| year | Industry                                                               | Total_pcf | 
| ---: | ---------------------------------------------------------------------: | --------: | 
| 2015 | "Consumer Durables, Household and Personal Products"                   | 931       | 
| 2013 | "Food, Beverage & Tobacco"                                             | 4308      | 
| 2014 | "Food, Beverage & Tobacco"                                             | 2023      | 
| 2015 | "Food, Beverage & Tobacco"                                             | 0         | 
| 2016 | "Food, Beverage & Tobacco"                                             | 99639     | 
| 2017 | "Food, Beverage & Tobacco"                                             | 3162      | 
| 2015 | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 8909      | 
| 2015 | "Mining - Iron, Aluminum, Other Metals"                                | 8181      | 
| 2013 | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 32271     | 
| 2014 | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 40215     | 
| 2015 | "Textiles, Apparel, Footwear and Luxury Goods"                         | 228       | 
| 2013 | Automobiles & Components                                               | 130189    | 
| 2014 | Automobiles & Components                                               | 230015    | 
| 2015 | Automobiles & Components                                               | 817227    | 
| 2016 | Automobiles & Components                                               | 1404833   | 
| 2013 | Capital Goods                                                          | 60117     | 
| 2014 | Capital Goods                                                          | 93699     | 
| 2015 | Capital Goods                                                          | 3505      | 
| 2016 | Capital Goods                                                          | 6369      | 
| 2017 | Capital Goods                                                          | 94943     | 
| 2015 | Chemicals                                                              | 44939     | 
| 2013 | Commercial & Professional Services                                     | 817       | 
| 2014 | Commercial & Professional Services                                     | 477       | 
| 2016 | Commercial & Professional Services                                     | 2890      | 
| 2017 | Commercial & Professional Services                                     | 741       | 
| 2013 | Consumer Durables & Apparel                                            | 2860      | 
| 2014 | Consumer Durables & Apparel                                            | 3123      | 
| 2016 | Consumer Durables & Apparel                                            | 1114      | 
| 2015 | Containers & Packaging                                                 | 2988      | 
| 2015 | Electrical Equipment and Machinery                                     | 9801558   | 
| 2013 | Energy                                                                 | 750       | 
| 2016 | Energy                                                                 | 10024     | 
| 2015 | Food & Beverage Processing                                             | 138       | 
| 2014 | Food & Staples Retailing                                               | 773       | 
| 2015 | Food & Staples Retailing                                               | 706       | 
| 2016 | Food & Staples Retailing                                               | 2         | 
| 2015 | Gas Utilities                                                          | 61        | 
| 2013 | Household & Personal Products                                          | 0         | 
| 2013 | Materials                                                              | 194464    | 
| 2014 | Materials                                                              | 66719     | 
| 2016 | Materials                                                              | 61887     | 
| 2017 | Materials                                                              | 107129    | 
| 2013 | Media                                                                  | 9645      | 
| 2014 | Media                                                                  | 9645      | 
| 2015 | Media                                                                  | 1919      | 
| 2016 | Media                                                                  | 1808      | 
| 2014 | Retailing                                                              | 11        | 
| 2015 | Retailing                                                              | 11        | 
| 2014 | Semiconductors & Semiconductor Equipment                               | 50        | 
| 2016 | Semiconductors & Semiconductor Equipment                               | 2         | 
| 2015 | Semiconductors & Semiconductors Equipment                              | 3         | 
| 2013 | Software & Services                                                    | 3         | 
| 2014 | Software & Services                                                    | 143       | 
| 2015 | Software & Services                                                    | 22851     | 
| 2016 | Software & Services                                                    | 22846     | 
| 2017 | Software & Services                                                    | 690       | 
| 2013 | Technology Hardware & Equipment                                        | 60539     | 
| 2014 | Technology Hardware & Equipment                                        | 101153    | 
| 2015 | Technology Hardware & Equipment                                        | 93807     | 
| 2016 | Technology Hardware & Equipment                                        | 1285      | 
| 2017 | Technology Hardware & Equipment                                        | 21866     | 
| 2013 | Telecommunication Services                                             | 52        | 
| 2014 | Telecommunication Services                                             | 183       | 
| 2015 | Telecommunication Services                                             | 183       | 
| 2015 | Tires                                                                  | 2022      | 
| 2015 | Tobacco                                                                | 1         | 
| 2015 | Trading Companies & Distributors and Commercial Services & Supplies    | 239       | 
| 2013 | Utilities                                                              | 61        | 
| 2016 | Utilities                                                              | 61        | 

We can see the change in carbon emissions of industry groups:

+) Decrease: Food, Beverage & Tobacco ; Commercial & Professional Services ; Consumer Durables & Apparel ; Food & Staples Retailing ;  Media ; Semiconductors & Semiconductors Equipment ; Software & Services ; Technology Hardware & Equipment 

+) Increase: Pharmaceuticals, Biotechnology & Life Sciences ; Automobiles & Components ; Capital Goods ; Energy ; Materials ; Telecommunication Services   

In my opinion, There are some groups that cannot be classified due to lack of data. 
Overall, the number of groups show decreasing trend more than the increasing one. This is a good signal since some of industry groups have made efforts to reduce carbon emissions.

## Conclusions and Insights

1. Industry groups that emitted large amount of emissions belongs to the heavy industry such as: Electrical Equipment and Machinery, Automobiles & Components. More specifically, they produce wind turbines and cars.

2. Countries from Europe have alarming carbon emissions because their countries have emissions many times higher than countries on other continents.

3. Gamesa Corporación Tecnológica, S.A. is the only company belonging to Electrical industry group. While the carbon footprint of the automobiles industry is contributed by many companies.

4. The largest amount of carbon emissions came from upstream activities.

5. The number of groups show decreasing trend more than the increasing one.





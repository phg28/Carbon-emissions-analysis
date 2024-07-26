# Carbon-emissions-analysis

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


### Top 10 countries with highest carbon emissions

```sql
select company_name, industry_group, sum(pcf) as Total_pcf from t1
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

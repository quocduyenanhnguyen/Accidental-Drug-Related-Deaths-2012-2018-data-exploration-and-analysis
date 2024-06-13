# 1. Death count by manner of death
```
select MannerofDeath, count(distinct ID) as mannerofdeath_count from accidental_drug_related_deaths_2012_to_2018
group by MannerofDeath
order by mannerofdeath_count desc
;
```
#### Output
![Screen Shot 2024-06-13 at 12 30 39 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/1bb6b533-f06b-48ba-9d3a-d4582dcfec4a)

-> Most people died from accident while on drug.

# 2. Death count by date type
```
select DateType, count(distinct ID) as mannerofdeath_count from accidental_drug_related_deaths_2012_to_2018
group by DateType
order by mannerofdeath_count desc
;
```
#### Output
![Screen Shot 2024-06-13 at 12 32 23 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/f3c40119-8d16-4569-8c73-ea9f19fbd19e)

-> Most people died unreported or have not been reported yet.

# 3. Death count by age group
```
select distinct 
case 
when age < 20 then 'Under 20'
when age BETWEEN 20 and 29 then '20-29'
when age BETWEEN 30 and 39 then '30-39'
when age BETWEEN 40 and 49 then '40-49'
when age BETWEEN 50 and 59 then '50-59'
when age >= 60 then '60 and above'
end Age_Group, count(distinct ID) as death_count
from accidental_drug_related_deaths_2012_to_2018
group by Age_Group
order by death_count desc
;
```
#### Output
![Screen Shot 2024-06-13 at 12 38 54 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/cc30dc83-3df4-402e-b693-28bd46e50f11)

-> More people aged 30 to 39 dead compared to other age groups.

### 3.1 Death count by Age Group and location
```
select distinct 
case 
when age < 20 then 'Under 20'
when age BETWEEN 20 and 29 then '20-29'
when age BETWEEN 30 and 39 then '30-39'
when age BETWEEN 40 and 49 then '40-49'
when age BETWEEN 50 and 59 then '50-59'
when age >= 60 then '60 and above'
end Age_Group, Location, count(distinct ID) as death_count
from accidental_drug_related_deaths_2012_to_2018
group by Age_Group, Location
order by death_count desc
;
```
#### Output
![Screen Shot 2024-06-13 at 12 41 04 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/a3e1d363-10a5-4734-92ee-3a83a8d7b7b0)

-> Most people in all age groups were dead in a residence.

### 3.2. Death count by City and State
```
select SUBSTRING_INDEX(DeathCityGeo, '(', 1) AS Death_State,
count(distinct ID) as death_count
from accidental_drug_related_deaths_2012_to_2018
group by Death_State
order by death_count desc
limit 3
;
```
#### Output
![Screen Shot 2024-06-13 at 12 44 24 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/4287b38c-6ee1-45fc-9529-f572eb5c1f67)

-> Most people were found dead in Hartford, New Haven, and Waterbury.

### 3.3 Residence of victims
```
select SUBSTRING_INDEX(ResidenceCityGeo, '(', 1) AS Residence_State,
count(distinct ID) as death_count
from accidental_drug_related_deaths_2012_to_2018
group by ResidenceCityGeo
order by death_count desc
limit 3
;
```
#### Output
![Screen Shot 2024-06-13 at 12 46 16 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/6f3b0db9-2c5f-483a-b438-278789f7593c)

-> Most deceased people have residence in HARTFORD, WATERBURY, and BRIDGEPORT.

# 4. Death count by gender and race
### 4.1. race
```
select Race, 
count(distinct ID) as death_count
from accidental_drug_related_deaths_2012_to_2018
group by Race
order by death_count desc
;
```
#### Output
![Screen Shot 2024-06-13 at 12 48 34 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/9f994846-bb10-495a-b5ea-55389dc42ba1)

-> White race has more deaths than other races.

### 4.2. race and gender
```
select Race, Sex,
count(distinct ID) as death_count
from accidental_drug_related_deaths_2012_to_2018
group by Race, Sex
order by death_count desc
;
```
#### Output
![Screen Shot 2024-06-13 at 12 50 32 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/d146e204-8777-4b70-95e1-035fd7be4342)

-> White male, white female, and hispanic white male seen more deaths than other groups. 

# 5. Death count by cause of death
```
select COD,
count(distinct ID) as death_count
from accidental_drug_related_deaths_2012_to_2018
group by COD
order by death_count desc
limit 3
;
```
#### Output
![Screen Shot 2024-06-13 at 12 52 52 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/3457a163-090c-4d60-b55d-39b4b0cf8c5b)

-> Most people died from fentanyl, heroin intoxication, and multiple drug toxicity. 

# 6. Death count by using opioid or not
### 6.1. Death count by how many opioids used
```
with cte1 as (with cte as (select ID, Heroin value, 'Heroin' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Cocaine value, 'Cocaine' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Fentanyl value, 'Fentanyl' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, FentanylAnalogue value, 'FentanylAnalogue' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Oxycodone value, 'Oxycodone' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Oxymorphone value, 'Oxymorphone' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Ethanol value, 'Ethanol' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Hydrocodone value, 'Hydrocodone' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Benzodiazepine value, 'Benzodiazepine' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Methadone value, 'Methadone' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Amphet value, 'Amphet' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Tramad value, 'Tramad' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Morphine_NotHeroin value, 'Morphine_NotHeroin' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Hydromorphone value, 'Hydromorphone' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, OpiateNOS value, 'OpiateNOS' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, AnyOpioid value, 'AnyOpioid' opioid_type
from accidental_drug_related_deaths_2012_to_2018)
select ID, count(distinct opioid_type) as number_of_opioids_used
from cte
where value = 'Y' -- and ID = '16-0165'
group by ID)
select distinct number_of_opioids_used, count(distinct ID) as death_count from cte1
group by number_of_opioids_used
order by death_count desc
;
```
#### Output
![Screen Shot 2024-06-13 at 12 54 48 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/bd46b95f-4ddf-4d2b-881c-3940ecf562fd)

-> Most people died from using 2 or 3 opioids.

### 6.2. Death count by opioid type
```
with cte as (select ID, Heroin value, 'Heroin' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Cocaine value, 'Cocaine' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Fentanyl value, 'Fentanyl' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, FentanylAnalogue value, 'FentanylAnalogue' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Oxycodone value, 'Oxycodone' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Oxymorphone value, 'Oxymorphone' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Ethanol value, 'Ethanol' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Hydrocodone value, 'Hydrocodone' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Benzodiazepine value, 'Benzodiazepine' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Methadone value, 'Methadone' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Amphet value, 'Amphet' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Tramad value, 'Tramad' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Morphine_NotHeroin value, 'Morphine_NotHeroin' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, Hydromorphone value, 'Hydromorphone' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, OpiateNOS value, 'OpiateNOS' opioid_type
from accidental_drug_related_deaths_2012_to_2018
union all
select ID, AnyOpioid value, 'AnyOpioid' opioid_type
from accidental_drug_related_deaths_2012_to_2018)
select opioid_type, count(distinct ID) as death_count
from cte
where value = 'Y' 
group by opioid_type
order by death_count desc
;
```
#### Output
![Screen Shot 2024-06-13 at 12 56 39 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/b8097aec-0313-4b97-9ac5-1cdb1a2dd0ff)

-> A lot of people died from using heroin, any other opioids, and fentanyl.

# 7. Death count by month year
### by month year
```
select date_format(STR_TO_DATE(Date, '%m/%d/%Y'), '%Y-%m') as converted_date, count(distinct ID) as death_count
from accidental_drug_related_deaths_2012_to_2018
-- where ID = '18-1002'
group by converted_date
order by death_count desc
limit 3
;
```
#### Output
![Screen Shot 2024-06-13 at 12 58 58 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/415fa214-b3eb-4578-bdb3-1b34d4fb060a)

-> Look at the top 3, most people died on Jun 2018, November 2016, and May 2017.

### where did they die?
```
select date_format(STR_TO_DATE(Date, '%m/%d/%Y'), '%Y-%m') as converted_date, location, count(distinct ID) as death_count
from accidental_drug_related_deaths_2012_to_2018
-- where ID = '18-1002'
group by converted_date, location
order by death_count desc
limit 10
;
```
#### Output
![Screen Shot 2024-06-13 at 1 00 48 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/13ec405e-686d-49f7-84ba-66849d3349b6)

-> Most of them died in a residence. 

### by month
```
select date_format(STR_TO_DATE(Date, '%m/%d/%Y'), '%m') as converted_date, count(distinct ID) as death_count
from accidental_drug_related_deaths_2012_to_2018
group by converted_date
order by death_count desc
;
```
#### Output
![Screen Shot 2024-06-13 at 1 01 43 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/aaf28eed-b25a-42cf-8c5f-347b0fc22ced)

-> Most people died in November, June, and October.

### by year
```
select date_format(STR_TO_DATE(Date, '%m/%d/%Y'), '%Y') as converted_date, count(distinct ID) as death_count
from accidental_drug_related_deaths_2012_to_2018
group by converted_date
order by death_count desc
;
```
#### Output
![Screen Shot 2024-06-13 at 1 02 52 PM](https://github.com/quocduyenanhnguyen/Accidental-Drug-Related-Deaths-2012-2018-data-exploration-and-analysis/assets/92205707/f355dca5-1b63-42e5-884a-f4671ac8c85a)

-> We saw more deaths in 2017, 2018, and 2016 compared to other years.


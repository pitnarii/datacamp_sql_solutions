
![](image.jpg)

# Introduction

Humans not only take debts to manage necessities. A country may also take debt to manage its economy. For example, infrastructure spending is one costly ingredient required for a country's citizens to lead comfortable lives. The World Bank is the organization that provides debt to countries.

## `international_debt` table

| Column | Definition | Data Type |
|-|-|-|
|country_name|Name of the country|`varchar`|
|country_code|Code representing the country|`varchar`|
|indicator_name|Description of the debt indicator|`varchar`|
|indicator_code|Code representing the debt indicator|`varchar`|
|debt|Value of the debt indicator for the given country (in current US dollars)|`float`|


## Total number of distinct countries

```sql
SELECT COUNT(DISTINCT country_name)  AS total_distinct_countries
FROM international_debt;
```
Output:

| total_distinct_countries |
|--------------------------|
|         124              |
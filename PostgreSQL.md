# PostgreSQL

[PostgreSQL Playground](https://www.pgtutorial.com/playground/)

#### Create a table

```sql
CREATE TABLE cities (
  name VARCHAR(50),
  country VARCHAR(50),
  population INTEGER,
  area INTEGER
);
```

#### Inserting data into a table

```sql
INSERT INTO cities (name, country, population, area)
VALUES 
  ('Delhi', 'India', 28125000, 2240),
  ('Shanghai', 'China', 22125000, 4015),
  ('Sao Paulo', 'Brazil', 20935000, 3043);
```

#### Retrieving data from a table

```sql
SELECT * FROM cities;
```

```sql
SELECT name, country FROM cities;
```

#### Calculated Column's

Listed a few math operators. There are more available.

```
+ Add
- Subtract
* Multiply
/ Divide
^ Exponent
|/ Square Root
@ Absolute Value
% Remainder
```

```sql
SELECT name, population / area AS population_density
FROM cities;
```

#### String operators and functions

```
|| Join two strings
concat() Join two strings
lower() Gives a lower case string
upper() Gives an upper case string
length() Gives number of characters in a string
```

```sql
SELECT name || ', ' || country AS location FROM cities;
```

```sql
SELECT CONCAT(name, ', ', UPPER(country)) AS location FROM cities;
```

```sql
SELECT UPPER(CONCAT(name, ', ', country)) AS location FROM cities;
```

---

#### Filtering rows with "WHERE"

```sql
SELECT name, area FROM cities WHERE area > 4000
```

Comparison Math Operators

```
=
>
<
>=
<=
<> - Are the values not equal
!= - Are the values not equal
BETWEEN - Is the value between two other values?
IN - Is the value present in a list?
NOT IN - Is the value not present in a list?
```

Compound WHERE clauses

```sql
SELECT name, area FROM cities WHERE area BETWEEN 2000 AND 4000;
```

```sql
SELECT name, area FROM cities WHERE name IN ('Delhi', 'Shanghai');
```

```sql
SELECT name, area FROM cities WHERE name NOT IN ('Delhi', 'Shanghai');
```

```sql
SELECT 
  name, area 
FROM 
  cities 
WHERE 
  name NOT IN ('Delhi', 'Shanghai') AND area != 8223;
```

```sql
SELECT 
  name, area 
FROM 
  cities 
WHERE 
  name NOT IN ('Delhi', 'Shanghai') OR area = 3043;
```

Calculations in WHERE clauses

```sql
SELECT 
  name, population / area AS population_density
FROM 
  cities 
WHERE 
  population / area > 6000;
```

---

#### Updating Rows

```sql
UPDATE 
  cities
SET 
  population = 39505000
WHERE 
  name = 'Tokyo';
```

---

#### Deleting Rows

```sql
DELETE FROM cities WHERE name = 'Tokyo';
```

---


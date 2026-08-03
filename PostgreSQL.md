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

#### Primary and Foreign Key

Primary Key - Uniquely identifies a record in a particular table

Foreign Key - Identifies a record(usually in another table) that this row is associated with

#### Auto Generated ID's

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(50)
);
```

#### How to create a foreign key column

```sql
CREATE TABLE photos (
  id SERIAL PRIMARY KEY,
  url VARCHAR(200),
  user_id INTEGER REFERENCES users(id)
);
```

#### Quick look into Join statements

```sql
SELECT * FROM photos 
JOIN users ON users.id = photos.user_id;
```

#### Foreign Key Constraints Around Insertion

- We insert a photo that is tied to a user that exits - Everything works Ok
- We insert a photo that refers to a user that doesn't exist - An error, you will not be able to run the INSERT query cause of foreign key constraint
- We insert a photo that isn't tied to any user - We will use NULL for the user_id if we have a scenario where user_id is not required.

```sql
INSERT INTO photos (url, user_id)
VALUES ('http://daily.jpg', NULL);
```

#### Foreign Key Constraints Around Deletions

Imagine we have a few records in the photos table referencing user with id 1. **What happens if we delete user with id 1 in the users table?**

- On Delete Restrict - You won't be able to delete user with id 1 cause its referenced in the photos table. You will get a foreign key constraint error.
- On Delete No Action -
- On Delete Cascade - Delete the user and the photos associated to the user
- On Delete set Null - Delete the user and set user_id as NULL in the photo's table
- On Delete set Default - Set the user_id of the photo to a default value, if one is provided

**On Delete Cascade**

```sql
CREATE TABLE photos (
  id SERIAL PRIMARY KEY,
  url VARCHAR(200),
  user_id INTEGER REFERENCES new_users(id) ON DELETE CASCADE
);
```

**On Delete Set Null**

```sql
CREATE TABLE photos (
  id SERIAL PRIMARY KEY,
  url VARCHAR(200),
  user_id INTEGER REFERENCES new_users(id) ON DELETE SET NULL
);
```

---

## Joins and Aggregation

For practice use these queries to create tables and insert data

```sql
CREATE TABLE users(
  id SERIAL PRIMARY KEY,
  username VARCHAR(50)
);
 
CREATE TABLE photos (
  id SERIAL PRIMARY KEY,
  url VARCHAR(200),
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE
);
 
CREATE TABLE comments (
  id SERIAL PRIMARY KEY,
  contents VARCHAR(240),
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
  photo_id INTEGER REFERENCES photos(id) ON DELETE CASCADE
);
 
INSERT INTO users (username) 
VALUES 
  ('Reyna.Marvin'),
        ('Micah.Cremin'),
        ('Alfredo66'),
        ('Gerard_Mitchell42'),
        ('Frederique_Donnelly');
 
INSERT INTO photos (url, user_id)
VALUES
  ('https://santina.net', 3),
        ('https://alayna.net', 5),
        ('https://kailyn.name', 3),
        ('http://marjolaine.name', 1),
        ('http://chet.net', 5),
        ('http://jerrold.org', 2),
        ('https://meredith.net', 4),
        ('http://isaias.net', 4),
        ('http://dayne.com', 4),
        ('http://colten.net', 2),
        ('https://adelbert.biz', 5),
        ('http://kolby.org', 1),
        ('https://deon.biz', 2),
        ('https://marina.com', 5),
        ('http://johnson.info', 1),
        ('https://linda.info', 2),
        ('https://tyrique.info', 4),
        ('http://buddy.info', 5),
        ('https://elinore.name', 2),
        ('http://sasha.com', 3);
 
INSERT INTO comments (contents, user_id, photo_id)
VALUES
  ('Quo velit iusto ducimus quos a incidunt nesciunt facilis.', 2, 4),
        ('Non est totam.', 5, 5),
        ('Fuga et iste beatae.', 3, 3),
        ('Molestias tempore est.', 1, 5),
        ('Est voluptatum voluptatem voluptatem est ullam quod quia in.', 1, 5),
        ('Aut et similique porro ullam.', 1, 3),
        ('Fugiat cupiditate consequatur sit magni at non ad omnis.', 1, 2),
        ('Accusantium illo maiores et sed maiores quod natus.', 2, 5),
        ('Perferendis cumque eligendi.', 1, 2),
        ('Nihil quo voluptatem placeat.', 5, 5),
        ('Rerum dolor sunt sint.', 5, 2),
        ('Id corrupti tenetur similique reprehenderit qui sint qui nulla tenetur.', 2, 1),
        ('Maiores quo quia.', 1, 5),
        ('Culpa perferendis qui perferendis eligendi officia neque ex.', 1, 4),
        ('Reprehenderit voluptates rerum qui veritatis ut.', 1, 1),
        ('Aut ipsum porro deserunt maiores sit.', 5, 3),
        ('Aut qui eum eos soluta pariatur.', 1, 1),
        ('Praesentium tempora rerum necessitatibus aut.', 4, 3),
        ('Magni error voluptas veniam ipsum enim.', 4, 2),
        ('Et maiores libero quod aliquam sit voluptas.', 2, 3),
        ('Eius ab occaecati quae eos aut enim rem.', 5, 4),
        ('Et sit occaecati.', 4, 3),
        ('Illum omnis et excepturi totam eum omnis.', 1, 5),
        ('Nemo nihil rerum alias vel.', 5, 1),
        ('Voluptas ab eius.', 5, 1),
        ('Dolor soluta quisquam voluptatibus delectus.', 3, 5),
        ('Consequatur neque beatae.', 4, 5),
        ('Aliquid vel voluptatem.', 4, 5),
        ('Maiores nulla ea non autem.', 4, 5),
        ('Enim doloremque delectus.', 1, 4),
        ('Facere vel assumenda.', 2, 5),
        ('Fugiat dignissimos dolorum iusto fugit voluptas et.', 2, 1),
        ('Sed cumque in et.', 1, 3),
        ('Doloribus temporibus hic eveniet temporibus corrupti et voluptatem et sint.', 5, 4),
        ('Quia dolorem officia explicabo quae.', 3, 1),
        ('Ullam ad laborum totam veniam.', 1, 2),
        ('Et rerum voluptas et corporis rem in hic.', 2, 3),
        ('Tempora quas facere.', 3, 1),
        ('Rem autem corporis earum necessitatibus dolores explicabo iste quo.', 5, 5),
        ('Animi aperiam repellendus in aut eum consequatur quos.', 1, 2),
        ('Enim esse magni.', 4, 3),
        ('Saepe cumque qui pariatur.', 4, 4),
        ('Sit dolorem ipsam nisi.', 4, 1),
        ('Dolorem veniam nisi quidem.', 2, 5),
        ('Porro illum perferendis nemo libero voluptatibus vel.', 3, 3),
        ('Dicta enim rerum culpa a quo molestiae nam repudiandae at.', 2, 4),
        ('Consequatur magnam autem voluptas deserunt.', 5, 1),
        ('Incidunt cum delectus sunt tenetur et.', 4, 3),
        ('Non vel eveniet sed molestiae tempora.', 2, 1),
        ('Ad placeat repellat et veniam ea asperiores.', 5, 1),
        ('Eum aut magni sint.', 3, 1),
        ('Aperiam voluptates quis velit explicabo ipsam vero eum.', 1, 3),
        ('Error nesciunt blanditiis quae quis et tempora velit repellat sint.', 2, 4),
        ('Blanditiis saepe dolorem enim eos sed ea.', 1, 2),
        ('Ab veritatis est.', 2, 2),
        ('Vitae voluptatem voluptates vel nam.', 3, 1),
        ('Neque aspernatur est non ad vitae nisi ut nobis enim.', 4, 3),
        ('Debitis ut amet.', 4, 2),
        ('Pariatur beatae nihil cum molestiae provident vel.', 4, 4),
        ('Aperiam sunt aliquam illum impedit.', 1, 4),
        ('Aut laudantium necessitatibus harum eaque.', 5, 3),
        ('Debitis voluptatum nesciunt quisquam voluptatibus fugiat nostrum sed dolore quasi.', 3, 2),
        ('Praesentium velit voluptatem distinctio ut voluptatum at aut.', 2, 2),
        ('Voluptates nihil voluptatum quia maiores dolorum molestias occaecati.', 1, 4),
        ('Quisquam modi labore.', 3, 2),
        ('Fugit quia perferendis magni doloremque dicta officia dignissimos ut necessitatibus.', 1, 4),
        ('Tempora ipsam aut placeat ducimus ut exercitationem quis provident.', 5, 3),
        ('Expedita ducimus cum quibusdam.', 5, 1),
        ('In voluptates doloribus aut ut libero possimus adipisci iste.', 3, 2),
        ('Sit qui est sed accusantium quidem id voluptatum id.', 1, 5),
        ('Libero eius quo consequatur laudantium reiciendis reiciendis aliquid nemo.', 1, 2),
        ('Officia qui reprehenderit ut accusamus qui voluptatum at.', 2, 2),
        ('Ad similique quo.', 4, 1),
        ('Commodi culpa aut nobis qui illum deserunt reiciendis.', 2, 3),
        ('Tenetur quam aut rerum doloribus est ipsa autem.', 4, 2),
        ('Est accusamus aut nisi sit aut id non natus assumenda.', 2, 4),
        ('Et sit et vel quos recusandae quo qui.', 1, 3),
        ('Velit nihil voluptatem et sed.', 4, 4),
        ('Sunt vitae expedita fugiat occaecati.', 1, 3),
        ('Consequatur quod et ipsam in dolorem.', 4, 2),
        ('Magnam voluptatum molestias vitae voluptatibus beatae nostrum sunt.', 3, 5),
        ('Alias praesentium ut voluptatem alias praesentium tempora voluptas debitis.', 2, 5),
        ('Ipsam cumque aut consectetur mollitia vel quod voluptates provident suscipit.', 3, 5),
        ('Ad dignissimos quia aut commodi vel ut nisi.', 3, 3),
        ('Fugit ut architecto doloremque neque quis.', 4, 5),
        ('Repudiandae et voluptas aut in excepturi.', 5, 3),
        ('Aperiam voluptatem animi.', 5, 1),
        ('Et mollitia vel soluta fugiat.', 4, 1),
        ('Ut nemo voluptas voluptatem voluptas.', 5, 2),
        ('At aut quidem voluptatibus rem.', 5, 1),
        ('Temporibus voluptates iure fuga alias minus eius.', 2, 3),
        ('Non autem laboriosam consectetur officiis aut excepturi nobis commodi.', 4, 3),
        ('Esse voluptatem sed deserunt ipsum eaque maxime rerum qui.', 5, 5),
        ('Debitis ipsam ut pariatur molestiae ut qui aut reiciendis.', 4, 4),
        ('Illo atque nihil et quod consequatur neque pariatur delectus.', 3, 3),
        ('Qui et hic accusantium odio quis necessitatibus et magni.', 4, 2),
        ('Debitis repellendus inventore omnis est facere aliquam.', 3, 3),
        ('Occaecati eos possimus deleniti itaque aliquam accusamus.', 3, 4),
        ('Molestiae officia architecto eius nesciunt.', 5, 4),
        ('Minima dolorem reiciendis excepturi culpa sapiente eos deserunt ut.', 3, 3);

```

**Joins**

Produces values by merging together rows from different related tables.

Use a join most times that you're asked to find data that involves multiple resources.

**Aggregation**

Look at many rows and calculates a single value.

Words like 'most', 'average', 'least' are a sign that you need to use an aggregation.

```sql
SELECT contents, username
FROM comments
JOIN users ON users.id = comments.user_id;
```

- Table order between FROM and JOIN frequently makes a difference
- We must give context if column name collide - like the example below

  During joins the database makes a temporary table matching the rows from different tables and then runs the SELECT on that temporary table.

  Now that we have id column in both the comments and users table, if we do a SELECT id, postgres will throw an error, cause it doesn't know which id you asking for.

  To fix this we need to SELECT id a bit different

  ```sql
  SELECT comments.id AS comment_id, photos.id AS photo_id, contents, url
  FROM comments
  JOIN photos ON photos.id = comments.photo_id;
  ```
- Tables can be renamed us AS keyword

  ```sql
  SELECT c.id AS comment_id, photos.id AS photo_id, contents, url
  FROM comments AS c
  JOIN photos ON photos.id = c.photo_id;
  ```

Just JOIN

There is a problem with using just JOIN in our queries, if there is row from the source table that doesn't match with a related table, then that row gets dropped from the overall result set.

If we have a photo in the photos table where user_id is NULL, then there won't be a relation in the users table for that row, so in the final temporary table created with JOIN, that 
particular photo will be removed.

#### Four Kinds of Joins

- **Inner Join**

  ```sql
  SELECT url, username
  FROM photos AS p
  JOIN users AS u ON u.id = p.user_id;
  ```

  Whenever there are rows that don't matchup to a row in the other table, that row is going to be dropped from the result set.

- **Left Outer Join**

  ```sql
  SELECT url, username
  FROM photos AS p
  LEFT JOIN new_users AS u ON u.id = p.user_id;
  ```

  A SQL operation that retrieves all records from the left table and the matching records from the right table. If a row in the left table does not have a match in the
  right table, the resulting columns from the right table will display NULL values.
  
- **Right Outer Join**

  ```sql
  SELECT url, username
  FROM photos AS p
  RIGHT JOIN new_users AS u ON u.id = p.user_id;
  ```

  A SQL RIGHT JOIN returns all records from the right table and only the matching records from the left table. If there is no match in the left table,
  the result shows NULL for the left table's columns.
  All the rows on the right hand side table are included backfilled with NULL's and all the non-matching records from the left hand side table are dropped.
  
- **Full Join**

  ```sql
  SELECT url, username
  FROM photos AS p
  FULL JOIN new_users AS u ON u.id = p.user_id;
  ```

  A database command that returns all records from both tables, combining matched rows and placing NULL values where there is no match.

---

We can run filters on the imaginary table's created using JOIN's using WHERE

```sql
SELECT url, contents
FROM comments
JOIN photos ON photos.id = comments.photo_id
WHERE photos.user_id = comments.user_id;
```

#### Three Way Joins

```sql
SELECT url, contents, username
FROM comments
JOIN photos ON photos.id = comments.photo_id
JOIN users ON users.id = comments.user_id AND users.id = photos.user_id;
```

---


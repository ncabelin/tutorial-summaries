# S.Q.L.
notes regarding SQL in general. For more specific tech and how to connect to each. i.e. MySQL, PostgreSQL, SQLite, refer to other documents.

### Count the number of Rows
```
SELECT COUNT(*) FROM table_name;
SELECT COUNT(column_name) FROM table_name;
```
### Aggregate Funtions = Sum/Avg/Max/Min the number of Rows
```
SELECT sum/avg/max/min(column_name) FROM table_name;
// NOTE: you can combine to form a table in one query
```
### Aggregates within Clauses
```
// PROBLEM : e.g. Group together the cost for all movies with similar genres
title - cost - genre
---------------------
Gone with the wind - 390,000 - Romance
Casablanca - 1,039,000 - Romance
Frankenstein - 3,000,000 - Horror
----------------------
SELECT genre, sum(cost)
FROM Movies
GROUP BY genre;
```
### Using the HAVING Clause
```
// e.g. Only include genres that have more than 1 movie
SELECT genre, sum(cost)
FROM Movies
GROUP BY genre
HAVING COUNT(*) > 1;
```

### Constraints to Tables
```
// NOT NULL = cannot be empty
// UNIQUE = must be unique
CREATE TABLE Promotions
(
  id int,
  name varchar(50) NOT NULL,
  category varchar(15),
  CONSTRAINT unique_name UNIQUE (name)
)
```

### Primary Keys
```
// e.g. insert this into CREATE TABLE
id int PRIMARY KEY
```
### Foreign Keys
```
// e.g. insert this into CREATE TABLE
movie_id int REFERENCES movies(id)
movie_id int REFERENCES movies // automatically uses id
FOREIGN KEY (movie_id) REFERENCES movies
```
### Preventing Orphan Records
```
// e.g. delete would-be orphan first
DELETE FROM Promotions WHERE movie_id = 6;
DELETE FROM Movies WHERE id = 6;
```
### Add CHECK constraint
```
CREATE TABLE Movies
(
  ...
  duration int CHECK (duration > 0) // checks for positive
)
// this would throw an error
INSERT INTO Movies(id, title, genre, duration)
VALUES(4, 'title', 'horror', -10)
```

### Normalization
```
// Build a JOIN table with e.g. naming convention Movies_Genres
// build separate tables, then join them, with each column representing a foreign key
```

### Relationships
1. one to many
2. many to many

### Inner Joins
![inner join](http://res.cloudinary.com/dd6kwd0zn/image/upload/q_auto/v1503516307/Screenshot_2017-08-23_12.24.08_cpevsi.png)
```
SELECT * FROM Movies
INNER JOIN Reviews
ON Movies.id=Reviews.movie_id

// on multiple tables
SELECT Movies.title, Genres.name
FROM Movies
INNER JOIN Movies_Genres
ON Movies.id = Movies_Genres.movie_id
INNER JOIN Genres
ON Movies_Genres.genre_id = Genres.id
WHERE Movies.title = "Peter Pan";
```

### Using Column alias
* Give columns new temporary names
```
// a) with one word
SELECT Movies.title AS films, Reviews.review AS reviews
FROM Movies
INNER JOIN Reviews
ON Movies.id=Reviews.movie_id

// b) with more than 2 words
SELECT Movies.title "Weekly Movies",
Reviews.review "Weekly Revies"
INNER JOIN Reviews
ON Movies.id=Reviews.movie_id
```

### Using Table aliases
* reduce verbosity of query statements by putting a letter beside table name i.e. `Movies m`
```
SELECT m.title, r.review
FROM Movies m
INNER JOIN Reviews r
ON m.id = m.movie_id
ORDER BY m.title;
```

### Outer Joins
* LEFT outer Join

![left outer join](http://res.cloudinary.com/dd6kwd0zn/image/upload/q_auto/v1503516307/Screenshot_2017-08-23_12.22.56_ficv89.png)
```
// e.g. All Movies and only the Reviews associated with them
SELECT *
FROM Movies
LEFT OUTER JOIN Reviews
ON Movies.id = Reviews.movie_id;
```
* RIGHT outer Join

![right outer join](http://res.cloudinary.com/dd6kwd0zn/image/upload/q_auto/v1503518115/Screenshot_2017-08-23_12.36.29_zck6zb.png)
```
// e.g. All Reviews and matching Movies if they exist
SELECT *
FROM Movies
RIGHT OUTER JOIN Reviews
ON Movies.id = Reviews.movie_id;
```

### Subqueries
```
// is easier to read
SELECT SUM(sales)
FROM Movies
WHERE id IN
(SELECT movie_id
FROM Promotions
WHERE category = 'Non-cash');
```
* Same as :
```
// in the long run more performant
SELECT SUM(m.sales)
FROM Movies m
INNER JOIN Promotions p
ON m.id = p.movie_id
WHERE p.category = 'Non-cash';
```

### CORRELATED Subqueries
* used whenever you want to use 'Aggregate Functions'
```
SELECT * FROM Movies WHERE duration > (SELECT AVG(duration) FROM Movies);
```
* because the following is not valid :
```
SELECT * FROM Movies WHERE duration > AVG(duration);
```

Query Mechanics



## The Select Clause



```sql
SELECT * FROM category;
```

| Clause name | Purpose                                                      |
| ----------- | ------------------------------------------------------------ |
| select      | Determines which columns to include in the query's result set |
| from        | identifies the tables from which to retrieve data and how the tables should be joined |
| where       | Filters out unwanted data                                    |
| group by    | Used to group rows together by common column values          |
| having      | Filters out unwanted gropus                                  |
| order by    | Sorts the rows of the final result set by one or more columns |





```sql
SELECT * FROM language;
```



```sql
SELECT language_id, name, last_update FROM language;
```

### Column Aliases

```sql
SELECT language_id,
	'COMMON' language_usage,
	language_id * 3.1415927 lang_pi_value,
	upper(name) language_name
FROM language;
```



```sql
SELECT version(),
	user(),
	database();
```

Many people feel that including the optional as keyword improves readability,

```sql
SELECT language_id,
	'COMMON' AS language_usage,
	language_id * 3.1415927 AS lang_pi_value,
	upper(name) AS language_name
FROM language;
```

### Removing Duplicates

```sql
SELECT actor_id FROM film_actor ORDER BY actor_id;
```

加上DISTINCT 去重复

```sql
SELECT DISTINCT actor_id FROM film_actor ORDER BY actor_id;
```

:exclamation: 请记住，生成一个独特的结果集需要对数据进行排序，这对大型结果集来说可能很耗时。不要为了确保没有重复而陷入使用 distinct 的陷阱；相反，要花时间了解你正在处理的数据，这样你就会知道是否有重复的可能。



## The From Clause

The from clause defines the tables used by a query, along with the means of linking the tables together.



### Tables

just the set of related rows

Four different types of tables meet this relaxed definition:

1. Permanent tables (i.e. created using the `create table` statement)
2. Derived tables (i.e., rows returned by a subquery and held in memory)
3. Temporary tables (i.e., volatile data held in memory)
4. Virtual Tables (i.e. created using the `create view` statement)



A **subquery** is a query contained within another query.

```sql
SELECT concat(cust.last_name, ',', cust.first_name) full_name
FROM
	(SELECT first_name, last_name, email
   FROM customer
   WHERE first_name = 'JESSIE'
  ) cust;
```

#### Temporary table

```sql
CREATE TEMPORARY TABLE actors_j
(actor_id smallint(5),
 first_name varchar(45),
 last_name varchar(45)
);
```



```sql
INSERT INTO actors_j
SELECT actor_id, first_name, last_name
FROM actor 
WHERE last_name LIKE 'J%';
```

#### Views

视图是存储在数据字典中的查询。它看起来就像个表，但没有与视图相关联的数据。所以叫它虚拟表virtual table。





```sql
CREATE VIEW cust_vm AS 
SELECT customer_id, first_name, last_name, active
FROM customer;
```



```sql
SELECT first_name, last_name
FROM cust_vm
WHERE active=0;
```

### Table Links

```sql
SELECT customer.first_name, customer.last_name, time(rental.rental_date) rental_time
FROM customer
	INNER JOIN rental
	ON customer.customer_id = rental.customer_id
WHERE date(rental.rental_date) = '2005-06-14';
```

[Chapter 5](Ch5-QueryingMultipleTables.md) for a thorough discussion of joining multiple tables.

## The where clause

The where clause is the mechanism for filtering out unwanted rows from your result set.

For example, perhaps you are interested in renting a film but you are only interested in movies rated G that can be kept for at least a week.

```sql
SELECT title FROM film
WHERE rating='R' AND rental_duration >= 7;
```



```sql
SELECT title, rating, rental_duration
FROM file
WHERE (rating='G' AND rental_duration >= 7)
OR (rating='PG-13' AND rental_duration < 4)
```



## The group by and having Clauses

```sql
SELECT c.first_name, c.last_name, count(*)
FROM customer c
	INNER JOIN rental r
	ON c.customer_id = r.customer_id
GROUP BY c.first_name, c.last_name
HAVING count(*) >= 40;
```

## The order by Clause

The `order` by clause is the mechanism for sorting your result set using either raw column data or expressions based on column data.

```sql
SELECT c.first_name, c.last_name, time(r.rental_date) rental_time
FROM customer c
	INNER JOIN rental r
	ON c.customer_id = r.customer_id
WHERE date(r.rental_date)='2005-06-14'
ORDER BY c.last_name;
```

### Ascending Versus Descending Sort Order



```sql
SELECT c.first_name, c.last_name, time(r.rental_date) rental_time
FROM customer c
	INNER JOIN rental r
	ON c.customer_id = r.customer_id
WHERE date(r.rental_date) = '2005-06-14'
ORDER BY time(r.rental_date) desc;
```

### Sorting via Numeric Placeholders

```sql
SELECT c.first_name, c.last_name, time(r.rental_date) rental_time
FROM customer c
	INNER JOIN rental r
	ON c.customer_id = r.customer_id
WHERE date(r.rental_date) = '2005-06-14'
ORDER BY 3 desc;
```

## Exercise 3-1

Retrieve the actor ID, first name, and last name for all actors. Sort by last name and then by first name.

```sql
SELECT actor_id, first_name, last_name from actor ORDER BY last_name, first_name;
```

## Exercise 3-2

Retrieve the actor ID, first name, and last name for all actors whose last name equals 'WILLIAMS' or 'DAVIS'.

```sql
SELECT actor_id, first_name, last_name 
from actor 
where last_name='WILLIAMS' 
OR last_name='DAVIS';
```

```sql
SELECT actor_id, first_name, last_name 
FROM actor 
WHERE last_name IN ('WILLIAMS', 'DAVIS')
```



## Exercise 3-3

Write a query against the `rental` table that returns the IDs of the customers who rented a film on July 5, 2005 (use the `rental.rental_date` column, and you can use the `date()` function to ignore the time component). Include a single row for each distinct customer ID.

```sql
SELECT customer_id FROM rental r WHERE date(r.rental_date)='2005-07-05';
```

## Exercise 3-4



```sql

```



```sql

```



```sql

```



```sql

```


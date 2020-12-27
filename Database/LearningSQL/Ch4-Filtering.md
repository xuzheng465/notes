An expression can be any of the following:

* A number
* A column in a table or view
* A string literal, such as 'Maple Street'
* A built-in function, such as concat('Learning', '  ', 'SQL')
* A subquery
* A list of expressions, such as ('Boston', 'New York', 'Chicago')

The operators within conditions include:

* Comparison operators, such as `=, !=, <, >, <>, like, in, between`
* Arithmetic operators, such as `+, -, * /`

## Condition Types



### Equality Conditions

column = expression

```sql
title = "Cyberpunk 2077"
fed_id = '111-1-111'
amount = 375.25
film_id = (SELECT film_id FROM film WHERE title = 'RIVER OUTLAW')
```



```sql
SELECT c.email
FROM customer c
	INNER JOIN rental r
	ON c.customer_id = r.customer_id
WHERE date(r.rental_date) = '2005-06-14';
```

### Inequality Conditions



```sql
SELECT c.email
FROM customer c
	INNER JOIN rental r
	ON c.customer_id = r.customer_id
WHERE date(r.rental_date) <> '2005-06-14'
```



### Range Conditions

```sql
SELECT customer_id, rental_date
FROM rental
WHERE rental_date < '2005-05-25';
```



### The between operator

When you have both an upper and lower limit for your range, you may choose to use a single condition that utilizes the `between` operator

```sql
SELECT customer_id, rental_date
FROM rental
WHERE rental_date BETWEEN '2005-06-14' AND '2005-06-16';
```



### String ranges

```sql
SELECT last_name, first_name
FROM customer
WHERE last_name BETWEEN 'FA' AND 'FR';
```



### Membership Conditions

`in`

```sql
SELECT title, rating
FROM film
WHERE rating = 'G' OR rating = 'PG';
```

```sql
SELECT title, rating
FROM film
WHERE rating IN ('G', 'PG');
```



### Using subqueries

```sql
SELECT title, rating
FROM film
WHERE rating IN (SELECT rating FROM film WHERE title LIKE '%PET%');
```

### not in

```sql
SELECT title, rating
FROM film
WHERE rating NOT IN ('PG-13', 'R', 'NC-17');
```



## Matching Conditions

```sql
SELECT last_name, first_name
FROM customer
WHERE left(last_name, 1) = 'Q';
```

### Using wildcards

When searching for partial string matches, you might be interested in:

* Strings beginning/ending with a certain character
* Strings beginning/ending with a substring
* Strings containing a certain character anywhere within the string
* Strings containing a substring anywhere within the string
* Strings with a specific format, regardless of individual characters.

| Wildcard character | Matches                                |
| ------------------ | -------------------------------------- |
| -                  | Exactly one character                  |
| %                  | Any number of characters (including 0) |

```sql
SELECT last_name, first_name
FROM customer
WHERE last_name LIKE '_A_T%S';
```

```sql
+-----------+------------+
| last_name | first_name |
+-----------+------------+
| MATTHEWS  | ERICA      |
| WALTERS   | CASSANDRA  |
| WATTS     | SHELLY     |
+-----------+------------+
```

| Search expression | Interpretation                                          |
| ----------------- | ------------------------------------------------------- |
| F%                | Strings beginning with F                                |
| %t                | Strings ending with t                                   |
| %bas%             | Strings containing the substring 'bas'                  |
| `_ _ t_`          | Four-character strings with a `t` in the third position |

### Null: That four-letter word

* Not applicable
  * 如在ATM机上发生的一笔交易的员工ID 列
* Value not yet known
  * Su ch as when the federal ID is not known at the time a customer row is created
* Value undefined
  * Such as when an account is created for a product that has not yet been added to the database

```sql
An expression can be null, but it can never equal null.
Two nulls are never equal to each other.
```

is null operator

```sql
SELECT rental_id, customer_id
FROM rental
WHERE return_date IS NULL;
```

Suppose that you have been asked to find all rentals that were not returned during May through August of  2005

First version:

```sql
SELECT rental_id, customer_id, return_date
FROM rental
WHERE return_date NOT BETWEEN '2005-05-01' AND '2005-09-01';
```



```sql
SELECT rental_id, customer_id, return_date
FROM rental
WHERE return_date IS NULL 
OR return_date NOT BETWEEN '2005-05-01' AND '2005-09-01';
```



## Test Your Knowledge

### ex 4-1



```sql
SELECT payment_id 
FROM payment
WHERE customer_id <> 5
AND (amount > 8 OR date(payment_date) = '2005-08-23');
```

101

### ex 4-2

```sql
SELECT payment_id 
FROM payment
WHERE customer_id = 5
AND NOT (amount>6 OR date(payment_date)='2005-06-19')
```



### ex 4-3

Construct a query that retrieves all rows from the payments table where the amount is either 1.98, 7.98, or 9.98.

```sql
SELECT *
FROM payment
WHERE amount in (1.98, 7.98,9.98);
```

### ex 4-4 :cactus:

Construct a query that finds all customers whose last name contains an A in the second position and a W anywhere after the A.

```sql
SELECT *
FROM customer
WHERE last_name LIKE "_A%W%"
```



```sql

```



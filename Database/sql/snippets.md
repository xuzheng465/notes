Q: Get the number of unique students that have an "@cs" login

A: SELECT COUNT(DISTINCT login) FROM students WHERE login LIKE ‘%@cs'



#### 如何查看当前隔离级别

#### 

```sql
show variables like 'transaction_isolation';
```


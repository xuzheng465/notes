Q: Get the number of unique students that have an "@cs" login

A: SELECT COUNT(DISTINCT login) FROM students WHERE login LIKE ‘%@cs'
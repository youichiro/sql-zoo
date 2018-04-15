# Tutorials: Learn SQL in stages
## 7 More JOIN operations

1. List the films where the yr is 1962 (Show id, title)
```sql
SELECT id, title 
FROM movie
WHERE yr = 1962
```

2. Give year of 'Citizen Kane'.
```sql
SELECT yr 
FROM movie
WHERE title = 'Citizen Kane'
```

3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.
```sql
SELECT id, title, yr 
FROM movie
WHERE title LIKE '%Star Trek%'
ORDER BY yr
```

4. What id number does the actor 'Glenn Close' have?
```sql
SELECT id 
FROM actor
WHERE name = 'Glenn Close'
```

5. What is the id of the film 'Casablanca'
```sql
SELECT id 
FROM movie
WHERE title = 'Casablanca'
```

6. Obtain the cast list for 'Casablanca'. Use movieid=11768, (or whatever value you got from the previous question)
```sql
SELECT name 
FROM actor 
JOIN casting ON actor.id=casting.actorid
WHERE movieid = 11768
```

7. Obtain the cast list for the film 'Alien'
```sql
SELECT name 
FROM movie 
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE title = 'Alien'
```

8. List the films in which 'Harrison Ford' has appeared
```sql
SELECT title 
FROM movie 
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE actor.name = 'Harrison Ford'
```

9. List the films where 'Harrison Ford' has appeared - but not in the starring role. (Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role)
```sql
SELECT title 
FROM movie 
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE actor.name='Harrison Ford'
AND NOT casting.ord = 1
```

10. List the films together with the leading star for all 1962 films.
```sql
SELECT movie.title, actor.name 
FROM movie 
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE casting.ord = 1
AND movie.yr = 1962
```

11. Which were the busiest years for 'John Travolta', show the year and the number of movies he made each year for any year in which he made more than 2 movies.
```sql
SELECT movie.yr, COUNT(yr) 
FROM movie 
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE actor.name='John Travolta'
GROUP BY movie.yr
HAVING COUNT(yr) > 2
)
```

12. List the film title and the leading actor for all of the films 'Julie Andrews' played in.
```sql
SELECT movie.title, actor.name 
FROM movie 
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE movie.id IN (
  SELECT movieid 
  FROM casting
  WHERE actorid IN (
    SELECT id 
    FROM actor
    WHERE name='Julie Andrews'
  )
)
AND casting.ord = 1
```

13. Obtain a list, in alphabetical order, of actors who've had at least 30 starring roles.
```sql
SELECT actor.name 
FROM movie 
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE casting.ord = 1
GROUP BY actor.name
HAVING COUNT(actor.name) >= 30
```

14. List the films released in the year 1978 ordered by the number of actors in the cast, then by title.
```sql
SELECT title, COUNT(actorid)
FROM movie 
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE movie.yr = 1978
GROUP BY title
ORDER BY COUNT(actorid) DESC, title ASC
```

15. List all the people who have worked with 'Art Garfunkel'.
```sql
SELECT actor.name 
FROM movie 
JOIN casting ON movie.id=casting.movieid
JOIN actor ON casting.actorid=actor.id
WHERE movie.id IN (
  SELECT movie.id 
  FROM movie 
  JOIN casting ON movie.id=casting.movieid
  JOIN actor ON casting.actorid=actor.id
  WHERE actor.name = 'Art Garfunkel'
)
AND actor.name != 'Art Garfunkel'
```

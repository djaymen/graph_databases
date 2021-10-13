1. List all Meg Ryan movies.

```sql 
MATCH (meg:Person {name: "Meg Ryan"}) -[:ACTED_IN]-> (movie:Movie) 	
RETURN DISTINCT movie.title
```

2. Find the top 10 youngest actors.
```sql
MATCH (actor:Person) -[:ACTED_IN]-> (movie:Movie)
RETURN DISTINCT actor.born AS birth_year , actor.name AS actor
ORDER BY actor.born
LIMIT 10
```
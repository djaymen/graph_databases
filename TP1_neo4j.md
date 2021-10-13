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
3. What was the role of Meg Ryan in « When Harry Met Sally » ?
```sql 
MATCH (actor:Person {name: "Meg Ryan"}) -[role:ACTED_IN]-> (movie:Movie {title: "When Harry Met Sally"})  
RETURN DISTINCT role.roles
```

4. Find all actors that played in movies released after 2000s...
```sql
MATCH (actor:Person) -[:ACTED_IN]-> (movie:Movie)  
WHERE movie.released >= 2000  
RETURN DISTINCT actor.name , movie.title , movie.released  
ORDER BY movie.released
```
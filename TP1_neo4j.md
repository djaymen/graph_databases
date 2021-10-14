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

5. Find all co-actors who played together in two movies.
```sql 
MATCH (p1)-[:ACTED_IN]->(mv:Movie)<-[:ACTED_IN]-(p2)
WHERE p1 <> p2
WITH p1,p2,collect(mv) AS movies
WHERE size(movies) = 2
RETURN p1.name AS actor_1 ,p2.name AS actor_2, REDUCE(acc="", m in movies |  acc + m.title + " | "  )
ORDER BY p1.name ,p2.name 
```


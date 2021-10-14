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

6. Find someone to introduce Tom Hanks to the director Frank Darabont.
```sql
MATCH (tom:Person{name:"Tom Hanks"})-[:ACTED_IN]->(m:Movie)<-[:ACTED_IN]-(other:Person)
WHERE tom<>other
WITH other AS introducer
MATCH (introducer)-[:ACTED_IN]->(m2)<-[:DIRECTED]-(frank:Person{name:"Frank Darabont"}) 
RETURN DISTINCT introducer.name
ORDER BY introducer.name
```

7. Find the shortest path of any relationships between Tom Hanks and Meg Ryan
```sql
MATCH (tom:Person {name:"Tom Hanks"}) , (meg:Person {name:"Meg Ryan"}),
path = shortestpath((tom)-[*..]-(meg))
RETURN path
```

8. Find all movies about love
```sql
MATCH (m:Movie)
WHERE (m.tagline =~ '.*love.*')
RETURN m.title AS movie_title , m.tagline as tagline
ORDER BY movie_title
```

9. Find actors who had multiple roles in a movie (return the actor, the movie and the
   roles)
```sql
MATCH (a:Person) -[acted_in:ACTED_IN]->(m:Movie)
WITH a,m,collect(acted_in.roles) AS roles
WHERE size(roles[0]) > 1
RETURN a.name AS Actor , m.title AS Movie ,  REDUCE(acc="", x in roles[0] | acc + " | " + x) AS Roles
```

10. Add to all actor nodes a label ACTOR and to all director nodes a label DIRECTOR.
```sql
MATCH (a) -[:ACTED_IN]->(m1) , (d)-[:DIRECTED]->(m2)
SET a.label = "ACTOR", d.label = "DIRECTOR"
RETURN a,d
```
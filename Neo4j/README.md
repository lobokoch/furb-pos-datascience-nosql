# Atividade Prática – Neo4j

### Exercício 1- Retrieving Nodes

Coloque os comandos utilizado em cada item a seguir:
- Exercise 1.1: Retrieve all nodes from the database.
R: MATCH (n) RETURN n

- Exercise 1.2: Examine the data model for the graph.
R: Modelo examinado, bem legal o resultado visual exibido pela ferramenta.

- Exercise 1.3: Retrieve all Person nodes.
R: MATCH (p:Person) RETURN p


- Exercise 1.4: Retrieve all Movie nodes.
R: MATCH (p:Movie) RETURN p


### Exercício 2 – Filtering queries using property values

Coloque os comandos utilizado em cada item a seguir:
- Exercise 2.1: Retrieve all movies that were released in a specific year.
R: match(m:Movie {released: 2000}) return m

- Exercise 2.2: View the retrieved results as a table.
R: Exibe o resultado no formato JSON:

- Exercise 2.3: Query the database for all property keys.
R: call db.propertyKeys

- Exercise 2.4: Retrieve all Movies released in a specific year, returning
their titles.
R: match(m:Movie {released: 2000}) return m.title

- Exercise 2.5: Display title, released, and tagline values for every Movie
node in the graph.
R: match(m:Movie) return m.title, m.released, m.tagline

- Exercise 2.6: Display more user-friendly headers in the table
R: match(m:Movie) return m.title as `Título`, m.released as `Lançamento` , m.tagline as `Tópico`

### Exercício 3 - Filtering queries using relationships
Coloque os comandos utilizado em cada item a seguir:
- Exercise 3.1: Display the schema of the database.
R: db.schema 
Nota: ocorre o erro: Neo.ClientError.Procedure.ProcedureNotFound

- Exercise 3.2: Retrieve all people who wrote the movie Speed Racer.
R: match (p:Person)-[:WROTE]->(:Movie {title: 'Speed Racer'}) RETURN p.name

- Exercise 3.3: Retrieve all movies that are connected to the person,
Tom Hanks.
R: match (m:Movie)<--(:Person {name: 'Tom Hanks'}) RETURN m.title

- Exercise 3.4: Retrieve information about the relationships Tom Hanks
had with the set of movies retrieved earlier.
R: match (m:Movie)-[rel]-(:Person {name: 'Tom Hanks'}) RETURN m.title, type(rel)

- Exercise 3.5: Retrieve information about the roles that Tom Hanks
acted in.
R: match (m:Movie)-[rel:ACTED_IN]-(:Person {name: 'Tom Hanks'}) RETURN m.title, rel.roles

### Exercício 4 – Filtering queries using WHERE clause

Coloque os comandos utilizado em cada item a seguir:
- Exercise 4.1: Retrieve all movies that Tom Cruise acted in.
R: match (p:Person)-[:ACTED_IN]->(m:Movie) where p.name = 'Tom Cruise' RETURN m.title as Movie

- Exercise 4.2: Retrieve all people that were born in the 70’s.
R: match (p:Person) where p.born >= 1970 and p.born < 1980 RETURN p.name as `Pessoa`, p.born as `Ano nascimento` order by p.born

- Exercise 4.3: Retrieve the actors who acted in the movie The Matrix
who were born after 1960.
R: match (p:Person)-[:ACTED_IN]->(m:Movie) where m.title='The Matrix' and p.born > 1960 RETURN p.name as `Pessoa`, p.born as `Ano nascimento` order by p.born

- Exercise 4.4: Retrieve all movies by testing the node label and a
property.
R: match (m) where m:Movie and m.released >= 2000 return m.title as Title

- Exercise 4.5: Retrieve all people that wrote movies by testing the
relationship between two nodes.
R: MATCH (p)-[rel]->(m) WHERE p:Person AND m:Movie AND type(rel) = 'WROTE' RETURN p.name as Nome, m.title as Filme

- Exercise 4.6: Retrieve all people in the graph that do not have a
property.
R: match (a) where a:Person and not exists(a.born) RETURN a.name as `Pessoa sem data de nascimento`

- Exercise 4.7: Retrieve all people related to movies where the
relationship has a property.
R: MATCH (a:Person)-[rel]->(m:Movie) WHERE exists(rel.rating) RETURN a.name as Name, m.title as Movie, rel.rating as Rating

- Exercise 4.8: Retrieve all actors whose name begins with James.
R: MATCH (a:Person)-[:ACTED_IN]->(:Movie) WHERE a.name STARTS WITH 'James' RETURN a.name

- Exercise 4.9: Retrieve all REVIEW relationships from the graph with filtered results.
R: MATCH (:Person)-[r:REVIEWED]->(m:Movie) WHERE toLower(r.summary) CONTAINS 'fun' RETURN  m.title as Movie, r.summary as Review, r.rating as Rating

- Exercise 4.10: Retrieve all people who have produced a movie, but have not directed a movie.
R: MATCH (a:Person)-[:PRODUCED]->(m:Movie) WHERE NOT ((a)-[:DIRECTED]->(:Movie)) RETURN a.name, m.title

- Exercise 4.11: Retrieve the movies and their actors where one of the actors also directed the movie.
R: MATCH (a1:Person)-[:ACTED_IN]->(m:Movie)<-[:ACTED_IN]-(a2:Person) WHERE exists( (a2)-[:DIRECTED]->(m) ) RETURN  a1.name as Actor, a2.name as `Actor/Director`, m.title as Movie

- Exercise 4.12: Retrieve all movies that were released in a set of years.
R: MATCH (m:Movie) WHERE m.released in [2000, 2004, 2008] RETURN m.title, m.released 

- Exercise 4.13: Retrieve the movies that have an actor’s role that is the name of the movie.
R: MATCH (a:Person)-[r:ACTED_IN]->(m:Movie) WHERE m.title in r.roles RETURN  m.title as Movie, a.name as Actor, r.roles as Roles

### Exercício 5 – Controlling query processing

Coloque os comandos utilizado em cada item a seguir:
- Exercise 5.1: Retrieve data using multiple MATCH patterns.
R: MATCH (a:Person)-[:ACTED_IN]->(m:Movie)<-[:DIRECTED]-(d:Person),
      (a2:Person)-[:ACTED_IN]->(m)
WHERE a.name = 'Gene Hackman'
RETURN m.title as movie, d.name AS director , a2.name AS `co-actors`, a.name

- Exercise 5.2: Retrieve particular nodes that have a relationship.
R: MATCH (p1:Person)-[:FOLLOWS]-(p2:Person) WHERE p1.name = 'James Thompson' RETURN p1, p2

- Exercise 5.3: Modify the query to retrieve nodes that are exactly three hops away.
R: MATCH (p1:Person)-[:FOLLOWS*3]-(p2:Person) WHERE p1.name = 'James Thompson' RETURN p1, p2

- Exercise 5.4: Modify the query to retrieve nodes that are one and two hops away.
R: MATCH (p1:Person)-[:FOLLOWS*1..2]-(p2:Person) WHERE p1.name = 'James Thompson' RETURN p1, p2

- Exercise 5.5: Modify the query to retrieve particular nodes that are connected no matter how many hops are required.
R: MATCH (p1:Person)-[:FOLLOWS*]-(p2:Person) WHERE p1.name = 'James Thompson' RETURN p1, p2

- Exercise 5.6: Specify optional data to be retrieved during the query.
R: MATCH (p:Person) WHERE p.name STARTS WITH 'Tom' OPTIONAL MATCH (p)-[:DIRECTED]->(m:Movie) RETURN p.name, m.title

- Exercise 5.7: Retrieve nodes by collecting a list.
R: MATCH (p:Person)-[:ACTED_IN]->(m:Movie) RETURN p.name as actor, collect(m.title) AS `movie list`

- Exercise 5.9: Retrieve nodes as lists and return data associated with the corresponding lists.
R: MATCH (p:Person)-[:REVIEWED]->(m:Movie) RETURN m.title as movie, count(p) as numReviews, collect(p.name) as reviewers

- Exercise 5.10: Retrieve nodes and their relationships as lists.
R: MATCH (d:Person)-[:DIRECTED]->(m:Movie)<-[:ACTED_IN]-(a:Person) RETURN d.name AS director, count(a) AS `number actors` , collect(a.name) AS `actors worked with`

- Exercise 5.11: Retrieve the actors who have acted in exactly five movies.
R: MATCH (a:Person)-[:ACTED_IN]->(m:Movie) WITH  a, count(a) AS numMovies, collect(m.title) AS movies WHERE numMovies = 5 RETURN a.name, movies

- Exercise 5.12: Retrieve the movies that have at least 2 directors with other optional data.
R: MATCH (m:Movie) WITH m, size((:Person)-[:DIRECTED]->(m)) AS directors WHERE directors >= 2 OPTIONAL MATCH (p:Person)-[:REVIEWED]->(m) RETURN  m.title, p.name


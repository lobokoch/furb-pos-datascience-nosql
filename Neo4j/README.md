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

- Exercise 2.4: Retrieve all Movies released in a specific year, returning their titles.

R: match(m:Movie {released: 2000}) return m.title

- Exercise 2.5: Display title, released, and tagline values for every Movie node in the graph.

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

- Exercise 3.3: Retrieve all movies that are connected to the person, Tom Hanks.

R: match (m:Movie)<--(:Person {name: 'Tom Hanks'}) RETURN m.title

- Exercise 3.4: Retrieve information about the relationships Tom Hanks had with the set of movies retrieved earlier.

R: match (m:Movie)-[rel]-(:Person {name: 'Tom Hanks'}) RETURN m.title, type(rel)

- Exercise 3.5: Retrieve information about the roles that Tom Hanks acted in.

R: match (m:Movie)-[rel:ACTED_IN]-(:Person {name: 'Tom Hanks'}) RETURN m.title, rel.roles

### Exercício 4 – Filtering queries using WHERE clause

Coloque os comandos utilizado em cada item a seguir:

- Exercise 4.1: Retrieve all movies that Tom Cruise acted in.

R: match (p:Person)-[:ACTED_IN]->(m:Movie) where p.name = 'Tom Cruise' RETURN m.title as Movie

- Exercise 4.2: Retrieve all people that were born in the 70’s.

R: match (p:Person) where p.born >= 1970 and p.born < 1980 RETURN p.name as `Pessoa`, p.born as `Ano nascimento` order by p.born

- Exercise 4.3: Retrieve the actors who acted in the movie The Matrix who were born after 1960.

R: match (p:Person)-[:ACTED_IN]->(m:Movie) where m.title='The Matrix' and p.born > 1960 RETURN p.name as `Pessoa`, p.born as `Ano nascimento` order by p.born

- Exercise 4.4: Retrieve all movies by testing the node label and a property.

R: match (m) where m:Movie and m.released >= 2000 return m.title as Title

- Exercise 4.5: Retrieve all people that wrote movies by testing the relationship between two nodes.

R: MATCH (p)-[rel]->(m) WHERE p:Person AND m:Movie AND type(rel) = 'WROTE' RETURN p.name as Nome, m.title as Filme

- Exercise 4.6: Retrieve all people in the graph that do not have a property.

R: match (a) where a:Person and not exists(a.born) RETURN a.name as `Pessoa sem data de nascimento`

- Exercise 4.7: Retrieve all people related to movies where the relationship has a property.

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

R: MATCH (a:Person)-[:ACTED_IN]->(m:Movie)<-[:DIRECTED]-(d:Person), (a2:Person)-[:ACTED_IN]->(m) WHERE a.name = 'Gene Hackman' RETURN m.title as movie, d.name AS director , a2.name AS `co-actors`, a.name

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

### Exercício 6 – Controlling results returned

Coloque os comandos utilizado em cada item a seguir:

- Exercise 6.1: Execute a query that returns duplicate records.

R: MATCH (a:Person)-[:ACTED_IN]->(m:Movie) WHERE m.released >= 1990 AND m.released < 2000 RETURN DISTINCT m.released, m.title, collect(a.name)

- Exercise 6.2: Modify the query to eliminate duplication.

R: MATCH (a:Person)-[:ACTED_IN]->(m:Movie) WHERE m.released >= 1990 AND m.released < 2000 RETURN  m.released, collect(m.title), collect(a.name)

- Exercise 6.3: Modify the query to eliminate more duplication.

R: MATCH (a:Person)-[:ACTED_IN]->(m:Movie) WHERE m.released >= 1990 AND m.released < 2000 RETURN  m.released, collect(DISTINCT m.title), collect(a.name)

- Exercise 6.4: Sort results returned.

R: MATCH (a:Person)-[:ACTED_IN]->(m:Movie) WHERE m.released >= 1990 AND m.released < 2000 RETURN  m.released, collect(DISTINCT m.title), collect(a.name) ORDER BY m.released DESC

- Exercise 6.5: Retrieve the top 5 ratings and their associated movies.

R: MATCH (:Person)-[r:REVIEWED]->(m:Movie) RETURN  m.title AS movie, r.rating AS rating ORDER BY r.rating DESC LIMIT 5

- Exercise 6.6: Retrieve all actors that have not appeared in more than 3 movies.

R: MATCH (a:Person)-[:ACTED_IN]->(m:Movie) WITH  a,  count(a) AS numMovies, collect(m.title) AS movies WHERE numMovies <= 3 RETURN a.name, movies

### Exercício 7 – Working with cypher data

Coloque os comandos utilizado em cada item a seguir:

- Exercise 7.1: Collect and use lists.

R: MATCH (a:Person)-[:ACTED_IN]->(m:Movie), (m)<-[:PRODUCED]-(p:Person) WITH  m, collect(DISTINCT a.name) AS cast, collect(DISTINCT p.name) AS producers RETURN DISTINCT m.title, cast, producers ORDER BY size(cast)

- Exercise 7.2: Collect a list.

R: MATCH (p:Person)-[:ACTED_IN]->(m:Movie) WITH p, collect(m) AS movies WHERE size(movies)  > 5 RETURN p.name, movies

- Exercise 7.3: Unwind a list.

R: MATCH (p:Person)-[:ACTED_IN]->(m:Movie) WITH p, collect(m) AS movies WHERE size(movies)  > 5 WITH p, movies UNWIND movies AS movie RETURN p.name, movie.title

- Exercise 7.4: Perform a calculation with the date type.

R: MATCH (a:Person)-[:ACTED_IN]->(m:Movie) WHERE a.name = 'Tom Hanks' RETURN  m.title, m.released, date().year  - m.released as yearsAgoReleased, m.released  - a.born AS `age of Tom` ORDER BY yearsAgoReleased


### Exercício 8 – Creating nodes

Coloque os comandos utilizado em cada item a seguir:

- Exercise 8.1: Create a Movie node.

R: CREATE (:Movie {title: 'Avatar'})

- Exercise 8.2: Retrieve the newly-created node.

R: MATCH (m:Movie) WHERE m.title = 'Avatar' RETURN m

- Exercise 8.3: Create a Person node.

R: CREATE (:Person {name: 'Zoe Saldana-Perego'})

- Exercise 8.4: Retrieve the newly-created node.

R: MATCH (p:Person) WHERE p.name = 'Zoe Saldana-Perego' RETURN p

- Exercise 8.5: Add a label to a node.

R: MATCH (m:Movie) WHERE m.released < 2009 SET m:OlderMovie RETURN DISTINCT labels(m)

- Exercise 8.6: Retrieve the node using the new label.

R: MATCH (m:OlderMovie) RETURN m.title, m.released 

- Exercise 8.7: Add the Female label to selected nodes.

R: MATCH (p:Person) WHERE p.name STARTS WITH 'Zoe' SET p:Female

- Exercise 8.8: Retrieve all Female nodes.

R: MATCH (p:Female) RETURN p.name

- Exercise 8.9: Remove the Female label from the nodes that have this label.

R: MATCH (p:Female) REMOVE p:Female

- Exercise 8.10: View the current schema of the graph.

R: CALL db.schema

- Exercise 8.11: Add properties to a movie.

R: MATCH (m:Movie)
WHERE m.title = 'Avatar'
SET m:OlderMovie,
    m.released = 2009,
    m.tagline = "We can hear Navi",
    m.lengthInMinutes = 162

- Exercise 8.12: Retrieve an OlderMovie node to confirm the label and properties.

R: MATCH (m:OlderMovie) WHERE m.title = 'Avatar' RETURN m

- Exercise 8.13: Add properties to the person, Zoe Saldana-Perego.

R: MATCH (p:Person) WHERE p.name = 'Zoe Saldana-Perego' SET p.born = 1978, p.birthPlace = ' Nova Jersey'

- Exercise 8.14: Retrieve an updated Person node.

R: MATCH (p:Person) WHERE p.name = 'Zoe Saldana-Perego' RETURN p

- Exercise 8.15: Remove a property from a Movie node.

R: MATCH (m:Movie) WHERE m.title = 'Avatar' SET m.lengthInMinutes = null

- Exercise 8.16: Retrieve the node to confirm that the property has been removed.

R: MATCH (m:Movie) WHERE m.title = 'Avatar' RETURN m

- Exercise 8.17: Remove a property from a Person node.

R: MATCH (p:Person) WHERE p.name = 'Zoe Saldana-Perego' REMOVE p.birthPlace

- Exercise 8.18: Retrieve the node to confirm that the property has been removed.

R: MATCH (p:Person) WHERE p.name = 'Zoe Saldana-Perego' RETURN p

### Exercício 9 – Creating relationships

Coloque os comandos utilizado em cada item a seguir:

- Exercise 9.1: Create ACTED_IN relationships.

R: MATCH (m:Movie) WHERE m.title = 'Forrest Gump' MATCH (p:Person) WHERE p.name = 'Tom Hanks' OR p.name = 'Robin Wright' OR p.name = 'Gary Sinise' CREATE (p)-[:ACTED_IN]->(m)

- Exercise 9.2: Create DIRECTED relationships.

R: MATCH (m:Movie) WHERE m.title = 'Forrest Gump' MATCH (p:Person) WHERE p.name = 'Robert Zemeckis' CREATE (p)-[:DIRECTED]->(m)

- Exercise 9.3: Create a HELPED relationship.

R: MATCH (p1:Person) WHERE p1.name = 'Tom Hanks' MATCH (p2:Person) WHERE p2.name = 'Gary Sinise' CREATE (p1)-[:HELPED]->(p2)

- Exercise 9.4: Query nodes and new relationships.

R: MATCH (p:Person)-[rel]-(m:Movie) WHERE m.title = 'Forrest Gump' RETURN p, rel, m

- Exercise 9.5: Add properties to relationships.

R: MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie)
WHERE m.title = 'Forrest Gump'
SET rel.roles =
CASE p.name
  WHEN 'Tom Hanks' THEN ['Forrest Gump']
  WHEN 'Robin Wright' THEN ['Jenny Curran']
  WHEN 'Gary Sinise' THEN ['Lieutenant Dan Taylor']
END

- Exercise 9.6: Add a property to the HELPED relationship.

R: MATCH (p1:Person)-[rel:HELPED]->(p2:Person)
WHERE p1.name = 'Tom Hanks' AND p2.name = 'Gary Sinise'
SET rel.research = 'war history'

- Exercise 9.7: View the current list of property keys in the graph.

R: call db.propertyKeys

- Exercise 9.8: View the current schema of the graph.

R: call db.schema

- Exercise 9.9: Retrieve the names and roles for actors.

R: MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie) WHERE m.title = 'Forrest Gump' RETURN p.name, rel.roles

- Exercise 9.10: Retrieve information about any specific relationships.

R: MATCH (p1:Person)-[rel:HELPED]-(p2:Person) RETURN p1.name, rel, p2.name

- Exercise 9.11: Modify a property of a relationship.

R: MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie) WHERE m.title = 'Forrest Gump' AND p.name = 'Gary Sinise' SET rel.roles =['Lt. Dan Taylor'] 

- Exercise 9.12: Remove a property from a relationship.

R: MATCH (p1:Person)-[rel:HELPED]->(p2:Person) WHERE p1.name = 'Tom Hanks' AND p2.name = 'Gary Sinise' REMOVE rel.research

- Exercise 9.13: Confirm that your modifications were made to the graph.

R: MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie) WHERE m.title = 'Forrest Gump' return p, rel, m

### Exercício 10 – Deleting nodes and relationships

Coloque os comandos utilizado em cada item a seguir:

- Exercise 10.1: Delete a relationship.

R: MATCH (:Person)-[rel:HELPED]-(:Person) DELETE rel

- Exercise 10.2: Confirm that the relationship has been deleted.

R: MATCH (:Person)-[rel:HELPED]-(:Person) RETURN rel

- Exercise 10.3: Retrieve a movie and all of its relationships.

R: MATCH (p:Person)-[rel]-(m:Movie) WHERE m.title = 'Forrest Gump' RETURN p, rel, m

- Exercise 10.4: Try deleting a node without detaching its relationships.

R: MATCH (m:Movie) WHERE m.title = 'Avatar' DELETE m

- Exercise 10.5: Delete a Movie node, along with its relationships.

R: MATCH (m:Movie) WHERE m.title = 'Forrest Gump' DETACH DELETE m

- Exercise 10.6: Confirm that the Movie node has been deleted.

R: MATCH (p:Person)-[rel]-(m:Movie) WHERE m.title = 'Forrest Gump' RETURN p, rel, m

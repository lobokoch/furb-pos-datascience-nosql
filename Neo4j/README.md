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

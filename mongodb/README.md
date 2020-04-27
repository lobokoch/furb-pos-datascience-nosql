# Atividade Prática – MongoDB

### Exercício 1- Aquecendo com os pets

1. Adicione outro Peixe e um Hamster com nome Frodo

R:
db.pets.insert({name: "Frodo", species: "Hamster"})
db.pets.insert({name: "Frodo", species: "Peixe"})

2. Faça uma contagem dos pets na coleção

R: db.pets.count()

3. Retorne apenas um elemento o método prático possível

R: db.pets.find({}, {"name": 1})

4. Identifique o ID para o Gato Kilha.

R: db.pets.find({"name": "Kilha"}, {"_id":1})

5. Faça uma busca pelo ID e traga o Hamster Mike

R: db.pets.find({"_id" : ObjectId("5ea420eade107b00061856fa")})

6. Use o find para trazer todos os Hamsters

R: db.pets.find({"species" : "Hamster"})

7. Use o find para listar todos os pets com nome Mike

R: db.pets.find({"name" : "Mike"})

8. Liste apenas o documento que é um Cachorro chamado Mike

R: db.pets.find({"name" : "Mike", "species" : "Cachorro"})


### Exercício 2 – Mama mia!

1. Liste/Conte todas as pessoas que tem exatamente 99 anos. 
Você pode usar um count para indicar a quantidade.

R: db.italians.find({"age": 99}).count() 
0


2. Identifique quantas pessoas são elegíveis atendimento 
prioritário (pessoas com mais de 65 anos)

R: db.italians.find({"age": { "$te": 65 }}).count()
1809



3. Identifique todos os jovens (pessoas entre 12 a 18 anos).

R: db.italians.find({"age": { "$gte": 12, "$lte": 18 }}).count()
902


4. Identifique quantas pessoas tem gatos, quantas tem cachorro e 
quantas não tem nenhum dos dois

R: 
- Gatos:
db.italians.find({cat: { $exists: true }}).count()
5982

- Cachorros:
db.italians.find({dog: { $exists: true }}).count()
3969

- Quantas não tem nenhum dos dois:
db.italians.find({cat: { $exists: false }, dog: {$exists: false}}).count()
2445


5. Liste/Conte todas as pessoas acima de 60 anos que tenham gato

R: db.italians.find({"age": { "$gt": 60 }}, {cat: { $exists: true }}).count()


6. Liste/Conte todos os jovens com cachorro

R: db.italians.find({"age": { "$gte": 12, "$lte": 18 }}, dog: {$exists: true}).count()


7. Utilizando o $where, liste todas as pessoas que tem gato e cachorro

R: 
db.italians.find({ $where: "this.cat !== null && this.dog !== null" })


8. Liste todas as pessoas mais novas que seus respectivos gatos.

R:  db.italians.find({ $and: [ { cat: { $exists: true }}, {$where: "this.age < this.cat.age"}]});

db.italians.find({ $and: [ { cat: { $exists: true }}, {$where: "this.age < this.cat.age"}]}).count()
595

9. Liste as pessoas que tem o mesmo nome que seu bichano (gatou ou cachorro)

R: 
db.italians.find({ $where: "this.cat && (this.firstname === this.cat.name)"}).count()

db.italians.find( { $or: [ { $where: "this.cat && (this.firstname === this.cat.name)"}, { $where: "this.dog && (this.firstname === this.dog.name)"}  ]})


10. Projete apenas o nome e sobrenome das pessoas com tipo de sangue de fator RH negativo
R: db.italians.find({ bloodType: /.*-$/  }, {firstname: 1, surname: 1})

11. Projete apenas os animais dos italianos. Devem ser listados os animais com nome e idade. Não mostre o identificado do mongo (ObjectId)

R: db.italians.find({ $where: "this.cat || this.dog" }, {"cat.name": 1, "dog.name": 1, _id: 0})


12. Quais são as 5 pessoas mais velhas com sobrenome Rossi?

R: db.italians.find({ surname: "Rossi" }).limit(5).sort({ age: -1 })


13. Crie um italiano que tenha um leão como animal de estimação.  um nome e idade ao bichano

R: db.italians.insert( { "firstname": "Joe", "surname": "Doe", "leao": {name: "Mike", age: 18}})


14. Infelizmente o Leão comeu o italiano. Remova essa pessoa usando o Id.

R: db.italians.deleteOne({"_id" : ObjectId("5ea58951ace7c81c550162c6")})

15. Passou um ano. Atualize a idade de todos os italianos e dos bichanos em (1)

R: db.italians.update({}, {$inc: { age: 1, "cat.age": 1, "doc.age": 1 }}, { multi: true })

16. O Corona Vírus chegou na Itália e misteriosamente atingiu pessoas somente com gatos e de 66 anos. Remova esses italianos.

R: db.italians.remove( { age: 66, cat: { $exists: true } })


17. Utilizando o framework agregate, liste apenas as pessoas com nomes iguais 
a sua respectiva mãe e que tenha gato ou cachorro.

R: 
db.italians.aggregate([
{'$match': { mother: { $exists: 1} }},
{'$match': { $or: [ {cat: {$exists: 1} }, {dog: {$exists: 1} } ] }},
{'$project': {
"firstname": 1,
"mother": 1,
"isEqual": { "$cmp": ["$firstname","$mother.firstname"]}
}},
{'$match': {"isEqual": 0}}
])


18. Utilizando aggregate framework, faça uma lista de nomes única de nomes. 
Faça isso usando apenas o primeiro nome

R: db.italians.aggregate([{ $group: { _id: "$firstname" } }])

19. Agora faça a mesma lista do item acima, considerando nome completo.

R: db.italians.aggregate([{ $group: { _id: {firstname: "$firstname", surname: "$surname" } }} ])

20. Procure pessoas que gosta de Banana ou Maçã, tenham cachorro ou gato,mais de 20 e menos de 60 anos.

R: db.italians.find( { $and: [ {"age": { "$gt": 20, "$lt": 60 }}, {$or: [{ dog: {$exists: true}}, {cat: {$exists: true}}]}, { $or: [ {favFruits: ["Banana"]}, {favFruits: ["Maçã"]}]} ]})


### Exercício 3 - Stockbrokers

Analise um pouco a estrutura dos dados novamente e em seguida, responda as seguintes perguntas:
1. Liste as ações com profit acima de 0.5 (limite a 10 o resultado)

R: db.stocks.find( { "Profit Margin": { "$gt": 0.5 } } ).limit(10)


2. Liste as ações com perdas (limite a 10 novamente)

R: db.stocks.find( { "Profit Margin": { "$lt": 0 } } ).limit(10)

3. Liste as 10 ações mais rentáveis

R: db.stocks.find().limit(10).sort({ "Profit Margin": -1 })

4. Qual foi o setor mais rentável?

R: db.stocks.find({}, {"Sector": 1}).limit(1).sort({ "Profit Margin": -1 })

5. Ordene as ações pelo profit e usando um cursor, liste as ações.

R: 
var cursor = db.stocks.find().sort({ "Profit Margin": -1 });
cursor.forEach(function(x){printjson(x);});

6. Renomeie o campo “Profit Margin” para apenas “profit”.

R: db.stocks.updateMany( {}, { $rename: { "Profit Margin": "profit" } } )

7. Agora liste apenas a empresa e seu respectivo resultado

R: db.stocks.find({}, {"Company": 1, "profit": 1})

8. Analise as ações. É uma bola de cristal na sua mão... Quais as três ações você investiria?

R:
{ "_id" : ObjectId("52853800bb1177ca391c1800"), "Sector" : "Basic Materials" }
{ "_id" : ObjectId("52853800bb1177ca391c1801"), "Sector" : "Financial" }
{ "_id" : ObjectId("52853800bb1177ca391c1806"), "Sector" : "Technology" }


9. Liste as ações agrupadas por setor

R: db.stocks.aggregate([{ $group: { _id: "$Sector" } }])

# Exercício 4 – Fraude na Enron!

1. Liste as pessoas que enviaram e-mails (de forma distinta, ou seja, sem repetir). Quantas pessoas são?

R: db.stocks.aggregate([{ $group: { _id: "$sender" } }])
db.stocks.aggregate([{ $group: { _id: "$sender" } }, { $count: "quantidade_pessoas" }])
{ "quantidade_pessoas" : 2201 }


2. Contabilize quantos e-mails tem a palavra “fraud”

R: db.stocks.find( { "text": /(^|\s)fraud($|\s)/ } ).count()
16 e-mails.


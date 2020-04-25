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

R:


6. Liste/Conte todos os jovens com cachorro

R:


7. Utilizando o $where, liste todas as pessoas que tem gato e cachorro

R:


8. Liste todas as pessoas mais novas que seus respectivos gatos.

R:


9. Liste as pessoas que tem o mesmo nome que seu bichano (gatou ou cachorro)

R:


10. Projete apenas o nome e sobrenome das pessoas com tipo de sangue de fator RH negativo

R:


11. Projete apenas os animais dos italianos. Devem ser listados os animais com nome e idade. Não mostre o identificado do mongo (ObjectId)

R:


12. Quais são as 5 pessoas mais velhas com sobrenome Rossi?

R:


13. Crie um italiano que tenha um leão como animal de estimação.  um nome e idade ao bichano

R:


14. Infelizmente o Leão comeu o italiano. Remova essa pessoa usando o Id.

R:

15. Passou um ano. Atualize a idade de todos os italianos e dos bichanos em (1)

R:

16. O Corona Vírus chegou na Itália e misteriosamente atingiu pessoas somente com gatos e de 66 anos. Remova esses italianos.

R:


17. Utilizando o framework agregate, liste apenas as pessoas com nomes iguais a sua respectiva mãe e que tenha gato ou cachorro.

R:

18. Utilizando aggregate framework, faça uma lista de nomes única de nomes. Faça isso usando apenas o primeiro nome

R:

19. Agora faça a mesma lista do item acima, considerando nome completo.

R:

20. Procure pessoas que gosta de Banana ou Maçã, tenham cachorro ou gato,mais de 20 e menos de 60 anos.

R:


### Exercício 3 - Stockbrokers

Analise um pouco a estrutura dos dados novamente e em seguida, responda as seguintes perguntas:
1. Liste as ações com profit acima de 0.5 (limite a 10 o resultado)

R:


2. Liste as ações com perdas (limite a 10 novamente)

R:

3. Liste as 10 ações mais rentáveis

R:

4. Qual foi o setor mais rentável?

R:

5. Ordene as ações pelo profit e usando um cursor, liste as ações.

R:

6. Renomeie o campo “Profit Margin” para apenas “profit”.

R:

7. Agora liste apenas a empresa e seu respectivo resultado

R:

8. Analise as ações. É uma bola de cristal na sua mão... Quais as três ações você investiria?

R:

9. Liste as ações agrupadas por setor

R:

# Exercício 4 – Fraude na Enron!

1. Liste as pessoas que enviaram e-mails (de forma distinta, ou seja, sem repetir). Quantas pessoas são?

R:


2. Contabilize quantos e-mails tem a palavra “fraud”

R:


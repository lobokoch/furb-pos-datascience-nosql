# Atividade Prática – HBase

### Exercício 1

Agora, de dentro da imagem faça os sefuintes procedimentos:

1. Crie a tabela com 2 famílias de colunas:
	a. personal-data
	b. professional-data
	
R: create 'italians', 'personal-data', 'professional-data'	

2. Importe o arquivo via linha de comando

R: hbase shell /tmp/italians.txt

Operações:
1. Adicione mais 2 italianos mantendo adicionando informações como data
de nascimento nas informações pessoais e um atributo de anos de
experiência nas informações profissionais;

R:
- put 'italians', '11', 'personal-data:name',  'Carlos Caramujo'
- put 'italians', '11', 'personal-data:city',  'Verona'
- put 'italians', '11', 'personal-data:data_nascimento',  '25/04/1970'
- put 'italians', '11', 'professional-data:role',  'Gestao Comercial'
- put 'italians', '11', 'professional-data:salary',  '10000'
- put 'italians', '11', 'professional-data:anos_experiencia',  '20'

- put 'italians', '12', 'personal-data:name',  'Carlota Caramujo'
- put 'italians', '12', 'personal-data:city',  'Verona'
- put 'italians', '12', 'personal-data:data_nascimento',  '25/08/1978'
- put 'italians', '12', 'professional-data:role',  'Gestao Comercial'
- put 'italians', '12', 'professional-data:salary',  '15000'
- put 'italians', '12', 'professional-data:anos_experiencia',  '15'

2. Adicione o controle de 5 versões na tabela de dados pessoais.

R: alter 'italians', NAME => 'personal-data', VERSIONS => 5

3. Faça 5 alterações em um dos italianos;

R: 
- put 'italians', '11', 'personal-data:city',  'Blumenau'
- put 'italians', '11', 'personal-data:city',  'Pomerode'
- put 'italians', '11', 'personal-data:city',  'Taio'
- put 'italians', '11', 'personal-data:city',  'Gaspar'
- put 'italians', '11', 'personal-data:city',  'Floripa'

4. Com o operador get, verifique como o HBase armazenou o histórico.

R:
get 'italians', '11', {COLUMN => 'personal-data:city', VERSIONS => 5}
- COLUMN                                               CELL                                                                                                                                                   
- personal-data:city                                  timestamp=1588066819259, value=Floripa                                                                                                                 
- personal-data:city                                  timestamp=1588066817336, value=Gaspar                                                                                                                  
- personal-data:city                                  timestamp=1588066817284, value=Taio                                                                                                                    
- personal-data:city                                  timestamp=1588066817216, value=Pomerode                                                                                                                
- personal-data:city                                  timestamp=1588066752493, value=Blumenau 


5. Utilize o scan para mostrar apenas o nome e profissão dos italianos.

R:
scan 'italians', { COLUMNS => ['personal-data:name', 'professional-data:role'] }

6. Apague os italianos com row id ímpar:

R:
- deleteall 'italians', '1'
- deleteall 'italians', '3'
- deleteall 'italians', '5'
- deleteall 'italians', '7'
- deleteall 'italians', '9'
- deleteall 'italians', '11'

7. Crie um contador de idade 55 para o italiano de row id 5

R:
- put 'italians', '5', 'personal-data:name',  'Rosa Donati'
- put 'italians', '5', 'personal-data:city',  'Palermo'
- incr 'italians', '5', 'personal-data:idade', 55
- COUNTER VALUE = 55

8. Incremente a idade do italiano em 1

R: 
- incr 'italians', '5', 'personal-data:idade'
- COUNTER VALUE = 56

# aula05
Nesta aula continuamos a trabalhar a linguagem SQL. Vemos como o operador seleção permite selecionar e filtrar tuplos aplicando condições diferentes condições desde comparações simples e progressivamente mais complexas. Olhamos para vários exemplo de funções de linha, funções que podemos utilizar diretamente na query e que são aplicadas sobre os tuplos de acordo com os resultados pretendidos.
Bom trabalho!

Select: WHERE: comparação, BETWEEN, AND, OR, IN, LIKE, REGEXP, IS NULL
Funções de linha: NUMBER, CHAR/STRING, DATE, conversão, CASE (DECODE)

[0. Requisitos](#0-requisitos)

[1. Operadores de Comparação](#1-operadores-de-comparação)

[2. AND, OR e NOT](#2-and-or-e-not)

[3. IN, LIKE e REGEXP](#3-in-like-e-regexp)

[4. Funções Numéricas](#4-funções-numéricas)

[5. Funções de String](#5-funções-de-string)

[6. Funções de Data](#6-funções-de-data)

[7. Trabalho de Casa](#7-trabalho-de-casa)

[Bibliografia e Referências](#bibliografia-e-referências)

[Outros](#outros)

## 0. Requisitos
Requisitos: Para esta aula, precisa de ter o ambiente de trabalho configurado ([Docker](https://www.docker.com/products/docker-desktop/) com [base de dados HR](https://github.com/ULHT-BD/aula03/blob/main/docker_db_aula03.zip) e [DBeaver](https://dbeaver.io/download/)). Caso ainda não o tenha feito, veja como fazer seguindo o passo 1 da [aula anterior 3](https://github.com/ULHT-BD/aula03/edit/main/README.md#1-prepare-o-seu-ambiente-de-trabalho).

Caso já tenha o docker pode iniciá-lo usando o comando ```docker start mysgbd``` no terminal, ou através do interface gráfico do docker-desktop:
<img width="1305" alt="image" src="https://user-images.githubusercontent.com/32137262/194916340-13af4c85-c282-4d98-a571-9c4f7b468bbb.png">

Deve também ter o cliente DBeaver.

## 1. Operadores de Comparação
Como vimos nas aulas anteriores, em SQL podemos usar a cláusula WHERE para especificar condições que condicionam o conjunto de tuplos retornados como resultado de pesquisa ou sobre o qual é aplicada alguma operação de atualização ou remoção.

Podemos usar vários operadores:
|Operador|Descrição|Exemplo|
|--------|---------|-------|
|=|Igual|Quais os alunos que têm 20 anos: ```SELECT * FROM alunos WHERE idade = 20;```|
|>|Maior|Quais os alunos que têm mais de 20 anos: ```SELECT * FROM alunos WHERE idade > 20;```|
|<|Menor|Quais os alunos que têm menos de 20 anos: ```SELECT * FROM alunos WHERE idade < 20;```|
|>=|Maior ou igual|Quais os alunos que têm 20 ou mais anos: ```SELECT * FROM alunos WHERE idade >= 20;```|
|<=|Menor ou igual|Quais os alunos que têm até 20 anos (inclusivé): ```SELECT * FROM alunos WHERE idade <= 20;```|
|<>|Diferente|Quais os alunos que não têm 20 anos: ```SELECT * FROM alunos WHERE idade <> 20;```|

### Exercícios
Para cada uma das alíneas seguintes, escreva a query que permite obter:
1. A lista de empregados cujo primeiro nome é John.

2. A lista de empregados cujo salário é superior a 5300.

3. Quais os departamentos cujo id é inferior ou igual a 200.
 
4. Que outras regiões existem para além de Europe.

 
NOTA: 
Podemos também usar operadores de comparação para obter o resultado de comparações diretamente das queries, por exemplo, para cada aluno indique se o aluno tem idade superior a 20 (True=1 ou False=0)?
``` sql
SELECT nome, idade, idade > 20 FROM alunos;
```

## 2. AND, OR e NOT
Em SQL, o valor ```NULL``` é utilizado para representar um campo vazio, sem qualquer valor. Numa dada relação, se um atributo for opcional e se adicionamos um tuplo sem especificar um valor é atribuido o valor ```NULL```. ```NULL``` é diferente do valor ```0``` ou de uma string vazia ```''```

Em SQL existem funções próprias para testar e manipular o valor ```NULL```
* ```IS NULL```, 1 se o valor do atributo for ```NULL``` ou 0 caso contrário
``` sql
SELECT nome FROM estudante WHERE idade IS NULL;
```
* ```IS NOT NULL```, 1 se o valor do atributo não for ```NULL``` ou 0 caso contrário
``` sql
SELECT nome FROM estudante WHERE idade IS NOT NULL;
```
* ```IFNULL()``` retorna valor alternativo caso seja ```NULL```
``` sql
SELECT IFNULL(idade, 20) FROM aluno;
```
* ```COALESCE()``` retorna o primeiro valor não ```NULL``` de uma lista
``` sql
SELECT COALESCE(NULL, NULL, 'Base de Dados', NULL, 'Universidade Lusófona');
```

### Exercícios
Para cada uma das alíneas seguintes, escreva a query que permite obter:
1. Quais os departamentos que não possuem um Manager?

2. Quais os empregados que recebem uma percentagem de comissão (COMISSION_PCT)

3. Lista de todos os empregados e respetivas comissões, mostrando ```0``` sempre que esta é ```NULL```

4. Qual o primeiro valor não ```NULL``` da lista (NULL, 'Lisboa', 'Porto', NULL, 'Braga')

## 3. IN, LIKE e REGEXP
|```BETWEEN```|Entre dois valores|Quais os alunos que têm entre 20 e 25 anos (inclusivé): ```SELECT * FROM alunos WHERE idade BETWEEN 20 AND 25;```|
Em SQL existem vários tipos de literais: string, numérico (exato e aproximado), data e hora, hexadecimal, binário ou boleano
| Literal | Exemplo | Descrição |
| ------- | ------- | --------- |
| String | "Teresa", 'Lisboa'  | Sequências de caractéres delimitadas por aspas (```"```) ou plicas (```'```) |
| Numerico (inteiro) | 1, 43, -210  | Um valor numérico inteiro exato |
| Numerico (decimal) | 0.5, 8.04, 1000.1  | Um valor numérico exato contendo parte inteira e parte decimal |
| Numerico (vírgula flutuante) | 2e-8, 1.34524e-24, 342.453 | Um valor numérico de virgula flutuante, i.e. sem número fixo de digitos à esquerda ou direita da vírgula |
| Data e Hora | '1998-03-15', '19980315', '1998-03-15 18:54:21.5' | Várias formatos possíveis para representar data e hora |
| Hexadecimal | x'123', X'123', 0x123 | Valores hexadecimais podem ser representados utilizando a notação ```x'valor'```, ```X'valor'``` ou ```0xvalor``` |
| Binário | b'0011', B'0011', 0b0011 | Valores binários podem ser representados utilizando a notação ```b'valor'```, ```B'valor'``` ou ```bvalor``` |
| Booleano | TRUE, FALSE | Os valores booleanos ```TRUE``` e ```FALSE``` são avaliados como ```1``` e ```0``` respetivamente |

### Exercícios
Para cada uma das alíneas seguintes, escreva a query que permite obter:
1. A constante 'Base de Dados'
2. O valor 10 dividido por 3
3. O hexadecimal de 'bd2022'
4. O valor 0100 em binário

## 4. Funções Numéricas
O operador DISTINCT permite obter apenas valores distintos/diferentes, i.e. permite remover duplicados.
NOTA: relembre que SQL duplicados podem existir uma vez que é baseado na teoria de sacos de tuplos (duplicados não existiriam na teoria dos conjuntos)

A sintaxe é:
``` sql
SELECT DISTINCT atributo, ...
FROM relacao;
```
Exemplo:
* Quais as diferentes idades dos estudantes
``` sql
SELECT DISTINCT idade
FROM estudante;
```

### Exercícios
Para cada uma das alíneas seguintes, escreva a query que permite obter:
1. A lista de todos os nomes próprios diferentes de empregados
2. A lista de todos os valores salariais diferentes
3. A lista de tuplos de combinação de valores nome, salário diferentes


## 5. Funções de String
O operador ORDER BY permite ordenar os resultados segundo um ou mais atributos.
Por defeito os resultados são ordenados por ordem ascendente, mas podemos definir ordem descendente utilizando ```DESC```

A sintaxe é:
``` sql
SELECT atributo, ...
FROM relacao
ORDER BY atributo;
```

Exemplo:
* Obter nome de alunos ordenados por ordem decrescente de idade
``` sql
SELECT nome, idade
FROM estudante
ORDER BY idade DESC;
```

### Exercícios
Para cada uma das alíneas seguintes, escreva a query que permite obter:
1. Obter a lista de todos os nomes próprios ordenados por ordem alfabética
2. Obter a lista de todos os nomes próprios diferentes ordenados por ordem alfabética
3. Obter a lista de empregados por ordem decrescente de salário
4. Obter a lista de todos os empregados (nome próprio, apelido e salário) ordenado por salário ordem decrescente e nome próprio ordem ascendente

## 6. Funções de Data

## 7. Trabalho de Casa
(será publicado após a aula)

## Bibliografia e Referências
* [w3schools - MySQL WHERE Clause]((https://www.w3schools.com/mysql/mysql_where.asp)
* [w3schools - NULL values](https://www.w3schools.com/sql/sql_null_values.asp)
* [geeksforgeeks - SQL Literals](https://www.geeksforgeeks.org/sql-literals/)
* [w3schools - DISTINCT](https://www.w3schools.com/sql/sql_distinct.asp)
* [w3schools - ORDER BY](https://www.w3schools.com/sql/sql_orderby.asp)

## Outros
Para dúvidas e discussões pode juntar-se ao grupo slack da turma através do [link](https://join.slack.com/t/ulht-bd/shared_invite/zt-1h783xcc2-eRQlIYSqFkDeOAqGC045Rw)

# aula05
Nesta aula continuamos a trabalhar a linguagem SQL. Vemos vários operadores de seleção que podem ser usados para impôr condições e filtrar tuplos através da cláusula WHERE. Olhamos para vários exemplo de funções de linha, funções disponibilizadas pelo MySQL que operam sobre dados numéricos, strings ou datas e que podemos utilizar diretamente na query sendo aplicadas sobre os tuplos de forma a obter diretamente os resultados pretendidos.
Bom trabalho!

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
Na cláusula ```WHERE``` podem ser utilizados os operadores ```AND```, ```OR``` e ```NOT``` para filtrar tuplos segundo várias condições.


Podemos usar os operadores:
|Operador|Descrição|Exemplo|
|--------|---------|-------|
|AND|e - Todas as condições têm de ser verdadeiras|Quais os alunos que têm mas de 20 anos e menos de 25 anos: ```SELECT * FROM alunos WHERE idade > 20 AND idade < 25;```|
|OR|ou - Apenas uma das condições tem de ser verdadeira|Quais os alunos que têm menos de 20 anos ou mais de 25 anos: ```SELECT * FROM alunos WHERE idade < 20 OR idade > 25;```|
|NOT|não - Filtrar quando condição é falsa|Quais os alunos que não têm mais de 20 anos: ```SELECT * FROM alunos WHERE NOT idade > 20;```|

### Exercícios
Para cada uma das alíneas seguintes, escreva a query que permite obter:
1. Quais os empregados que cujo salário é superior a 5300 mas inferior a 5600?

2. Quais os empregados que recebem menos de 5000 ou mais de 10000

3. Lista de empregados cujo departamento não é o 30 nem o 50 

## 3. IN, LIKE e REGEXP
Os operadores ```IN```, ```LIKE``` e ```REGEXP``` permitem fazer comparações mais complexas, veja a seguinte tabela:
|Operador|Descrição|Exemplo|
|--------|---------|-------|
|IN|Comparar com uma lista de valores (equivalente a comparações separadas por OR)|Quais os alunos cujo nome é Maria, Teresa ou Pedro: ```SELECT * FROM alunos WHERE nome IN ('Maria', 'Teresa', 'Pedro');```|
|LIKE|Pesquisar um padrão numa coluna. Wilcards comuns ```%``` (representa zero ou múltiplos caracteres) e ```_``` (um único caracter)|Quais os alunos cujo nome começa por M e acaba em a (e.g. Maria, Marta, Mónica, etc): ```SELECT * FROM alunos WHERE nome LIKE 'M%a';```|
|REGEXP|Pesquisar utilizando expressões regulares|Quais os alunos cujo nome começa por M e acaba em a (e.g. Maria, Marta, Mónica, etc): ```SELECT * FROM alunos WHERE nome REGEXP '^M.*a$';```|

### Exercícios
Para cada uma das alíneas seguintes, escreva a query que permite obter:
1. A lista de empregados cujo primeiro nome é David, Peter ou John
2. A lista de empregados cujo primeiro nome começa e acaba com a letra d
3. A lista de regiões cujo nome contém três ocorrêncas da letra 'a' (possivel resolver com LIKE ou REGEXP)
4. A lista de países cujo nome contém três ocorrêncas da letra 'a' mas nenhum 'l' (possivel resolver com LIKE ou REGEXP)

## 4. Funções Numéricas
O MySQL disponibiliza um grande conjunto de funções que permitem operar sobre dados numéricos. Alguns exemplos são:

|Operador|Descrição|Exemplo|
|--------|---------|-------|
|ABS|Obter o valor absoluto|Qual o valor absoluto de -231.5: ```SELECT ABS(-231.5);```|
|SQRT|Obter o valor da raíz quadrada|Qual a raiz quadrada de 9: ```SELECT SQRT(9);```|
|POW|Calcular o valor da potência|Qual o valor de potência 2 de base 4: ```SELECT POW(4, 2);```|
|RAND|Obter um valor aleatório (entre 0 e 1)|Obtenha um valor aleatório entr 0 e 10: ```SELECT RAND()*10;```|
|ROUND|Obter um valor arredondado|Obtenha o valor de pi arredondado para 2 casas decimais: ```SELECT ROUND(PI(), 2);```|

### Exercícios
Para cada uma das alíneas seguintes, escreva a query que permite obter:
1. A lista dos nomes de empregados e um número id aleatório
2. A lista dos nomes de empregados e respetivo salário com bonificação de 6% arredondado com duas casas décimais
3. Os empregados cujo nome é David, com um número id aleatório e salário com bonificação de 6% arredondado com duas casas décimais


## 5. Funções de String
O MySQL disponibiliza um grande conjunto de funções que permitem operar sobre strings. Alguns exemplos são:

|Operador|Descrição|Exemplo|
|--------|---------|-------|
|LOWER ou LCASE|Converter todos os caracteres para minúsculas|Obter o nome de aluno em minúsculas: ```SELECT LOWER(nome) FROM alunos;```|
|UPPER ou UCASE|Converter todos os caracteres para maiúsculas|Obter o nome de aluno em maiúsculas: ```SELECT UPPER(nome) FROM alunos;```|
|CONCAT|Concatenar várias strings numa só|Construir nome completo a partir de nome e apelido: ```SELECT CONCAT(nome, ' ', apelido) FROM alunos;```|
|CHAR_LENGTH|Obter o número de caracteres de uma string|Contar o número de letras de cada nome: ```SELECT CHAR_LENGTH(nome) FROM alunos;```|
|STRCMP|Comparar duas strings (resultado -1, 0 ou 1) |Verificar se nome de aluno é 'Pedro': ```SELECT STRCMP(nome, 'Pedro') FROM alunos;```|
|SUBSTR|Extrair uma substring de uma string|Obter as duas primeiras letras de cada nome: ```SELECT SUBSTR(nome, 1, 2) FROM alunos;```|

### Exercícios
Para cada uma das alíneas seguintes, escreva a query que permite obter:
1. Obter a lista de todos os nomes próprios em minúsculas e apelidos em maiúsculas
2. Obter a lista de todos os nomes completos concatenando primeiro nome e último nome, bem como um email no formato primeiro_nome.ultimo_nome@ulusofona.pt
3. Obter a lista de nomes, apelidos e uma terceira coluna com as iniciais (e.g. Teresa, Carvalho, TC)

## 6. Funções de Data
O MySQL disponibiliza um grande conjunto de funções que permitem operar sobre datas. Alguns exemplos são:
|Operador|Descrição|Exemplo|
|--------|---------|-------|
|CURDATE e CURTIME|Obter a data ou hora atuais (do sistema)|Obter a data atual: ```SELECT CURDATE();```|
|ADDDATE ou DATE_ADD|Adicionar um intervalo de tempo a uma data (DATE_SUB para subtrair)|Calcular a data que será após 45 dias: ```SELECT ADDDATE("2022-10-18", INTERVAL 45 DAY);```|
|DATEDIFF|Calcular a diferença em dias entre duas datas|Calcular quantos dias passaram entre 24 e 1 de outubro: ```SELECT DATEDIFF("2022-10-24", "2022-10-01");```|
|DATE_FORMAT|Permite formatar datas de acordo com pretendido|Obter data no formato dia, mês por extenso ano 4 digitos: ```SELECT DATE_FORMAT("2022-10-18", "%d %M %Y");```|
|DAY|Extrair dia de uma data (outras possibilidades MONTH, YEAR, HOUR, WEEK, etc)|Obter o dia da data: ```SELECT DAY("2022-10-18");```|

### Exercícios
Para cada uma das alíneas seguintes, escreva a query que permite obter:
1. Duas colunas com qual a data atual e qual a hora atual. (experimente tambéma função NOW)
2. A lista de todos os nomes de empregados e ano de data de contratação
3. O número de dias passados desde que o empregado foi contratado até hoje

## 7. Trabalho de Casa
(será publicado após a aula)

## Bibliografia e Referências
* [w3schools - MySQL WHERE Clause](https://www.w3schools.com/mysql/mysql_where.asp)
* [w3schools - MySQL IN Operator](https://www.w3schools.com/mysql/mysql_in.asp)
* [w3schools - MySQL LIKE Operator](https://www.w3schools.com/mysql/mysql_like.asp)
* [geeksforgeeks - Regular Expressions REGEXP](https://www.geeksforgeeks.org/mysql-regular-expressions-regexp/)
* [w3schools - MySQL Functions](https://www.w3schools.com/mysql/mysql_ref_functions.asp)

## Outros
Para dúvidas e discussões pode juntar-se ao grupo slack da turma através do [link](https://join.slack.com/t/ulht-bd/shared_invite/zt-1h783xcc2-eRQlIYSqFkDeOAqGC045Rw)

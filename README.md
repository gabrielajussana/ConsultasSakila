# Base de Dados Sakila

## Consultas e Consultas Avançadas

1. Listar todos os filmes
~~~
SELECT * FROM FILM;
~~~~
2. Listar filmes e atores que atuaram nestes.
~~~
SELECT F.TITLE, A.FIRST_NAME, A.LAST_NAME
FROM FILM F, FILM_ACTOR FA, ACTOR A
WHERE F.FILM_ID = FA.FILM_ID AND A.ACTOR_ID = FA.ACTOR_ID;
~~~~
3. Qual foi o cliente (costumer) que alugou mais filmes (rental)?
~~~
SELECT (C.FIRST_NAME + ' ' + C.LAST_NAME) AS CLIENTE, COUNT(F.FILM_ID) AS
FILMES
FROM CUSTOMER C, RENTAL R, INVENTORY I, FILM F
WHERE R.CUSTOMER_ID = C.CUSTOMER_ID
AND R.INVENTORY_ID = I.INVENTORY_ID
AND I.FILM_ID = F.FILM_ID
AND FILMES = (
SELECT MAX(FILM_ID)
FROM FILM
);

~~~~
4. Qual foi o cliente que alugou mais filmes diferentes?
~~~

~~~~
5. Qual foi o cliente que mais gastou em aluguéis (payment)?
~~~
SELECT C.FIRST_NAME AS CLIENTE,  MAX(AMOUNT)
FROM CUSTOMER AS C, PAYMENT AS P
WHERE C.CUSTOMER_ID = P.CUSTOMER_ID;
~~~~
6. Qual é o funcionário (staff) que mais realizou aluguéis?
7. Listar o primeiro e o último nome dos atores (actor) ordenados por último nome
e primeiro nome.
~~~
SELECT LAST_NAME, FIRST_NAME
FROM ACTOR
ORDER BY LAST_NAME ASC, FIRST_NAME ASC;
~~~~
8. Listar apenas o primeiro nome e último dos clientes inativos.
~~~
SELECT FIRST_NAME, LAST_NAME
FROM CUSTOMER
WHERE ACTIVE NOT LIKE 1;
~~~~
9. Listar o título dos filmes (film) com duração entre 60 e 80 minutos.
~~~
SELECT TITLE FROM FILM
WHERE LENGTH BETWEEN 60 AND 80;
~~~~
10. Listar o título dos filmes com preço de locação abaixo de 1 dólar ordenados
pelo título de forma decrescente.
~~~
SELECT TITLE FROM FILM
WHERE RENTAL_RATE < 1
ORDER BY TITLE DESC;
~~~~
11. Listar título, preço de locação e custo de substituição dos filmes com custo de
substituição acima de 20 dólares, mas que tenham preço de locação abaixo de 3
dólares. Ordenar pelo custo de substituição seguido do preço de locação.
~~~
SELECT TITLE, RENTAL_RATE, REPLACEMENT_COST
FROM FILM
WHERE REPLACEMENT_COST > 20
AND RENTAL_RATE < 3
ORDER BY REPLACEMENT_COST ASC, RENTAL_RATE ASC;
~~~~
12. Qual a média do preço de locação?
~~~
SELECT AVG(RENTAL_RATE) AS MEDIA_PRECO_LOCACAO
FROM FILM;
~~~~
13. Quantos filmes tem duração de 120 minutos ou mais?
~~~
SELECT TITLE FROM FILM
WHERE LENGTH >= 120;
~~~~
14. Qual o filme com custo de substituição mais caro?
~~~
SELECT TITLE, MAX(REPLACEMENT_COST) AS MAIS_CARO
FROM FILM;
~~~~
15. Qual a menor duração de locação?
~~~
SELECT MIN(F.RENTAL_DURATION) AS MENOR_DURACAO
FROM FILM F;
~~~~
16. Listar o título dos filmes com preço de locação acima da média.
~~~
SELECT TITLE, RENTAL_RATE
FROM FILM
WHERE RENTAL_RATE > (SELECT AVG(RENTAL_RATE) FROM FILM);
~~~~
17. Listar o título dos filmes com maior duração de locação.
~~~
SELECT TITLE FROM FILM
WHERE LENGTH = (
SELECT MAX(LENGTH) FROM FILM
);
~~~~
18. Listar o título dos filmes com o menor custo de substituição.
~~~
SELECT TITLE FROM FILM
WHERE REPLACEMENT_COST = (
SELECT MIN(REPLACEMENT_COST) FROM FILM
);
~~~~
19. Quais cidades do ‘Brasil’ estão cadastradas no sistema?
~~~
SELECT CY.CITY FROM CITY CY, COUNTRY CR
WHERE CY.COUNTRY_ID = CR.COUNTRY_ID
AND CR.COUNTRY = 'BRAZIL';
~~~~
20. Listar o nome dos clientes, a cidade e o país onde moram.
~~~
SELECT CS.FIRST_NAME, CS.LAST_NAME, CI.CITY, CN.COUNTRY
FROM CUSTOMER CS, COUNTRY CN, CITY CI, ADDRESS A
WHERE CN.COUNTRY_ID = CI.COUNTRY_ID
AND CI.CITY_ID = A.CITY_ID
AND A.ADDRESS_ID = CS.ADDRESS_ID;
~~~~
21. Listar o título do filme e sua(s) respectiva(s) categoria(s).
~~~
SELECT F.TITLE, C.NAME
FROM FILM F, CATEGORY C, FILM_CATEGORY FC
WHERE F.FILM_ID = FC.FILM_ID
AND FC.CATEGORY_ID = C.CATEGORY_ID;
~~~~
22. Listar a quantidade de filmes por categoria.
~~~
SELECT C.NAME, COUNT(*)
FROM FILM F, CATEGORY C, FILM_CATEGORY FC
WHERE F.FILM_ID = FC.FILM_ID
AND FC.CATEGORY_ID = C.CATEGORY_ID
GROUP BY C.NAME;
~~~~
23. Listar a quantidade de filmes por categoria apenas das categorias com menos
de 57 filmes.
~~~
SELECT C.NAME, COUNT(*) QTD
FROM FILM F, CATEGORY C, FILM_CATEGORY FC
WHERE F.FILM_ID = FC.FILM_ID AND FC.CATEGORY_ID = C.CATEGORY_ID
GROUP BY C.NAME
HAVING QTD < 57;
~~~~
24. Listar a duração média dos filmes por categoria apenas das categorias com
menos de 57 filmes.
~~~
SELECT C.NAME, COUNT(*) QTD, AVG(F.LENGTH)
FROM FILM F, CATEGORY C, FILM_CATEGORY FC
WHERE F.FILM_ID = FC.FILM_ID AND FC.CATEGORY_ID = C.CATEGORY_ID
GROUP BY C.NAME
HAVING QTD < 57;
~~~~
25. Quais atores trabalharam no filme ‘GOLD RIVER’?
~~~
SELECT ACT.FIRST_NAME, ACT.LAST_NAME
FROM FILM F, FILM_ACTOR FA, ACTOR ACT
WHERE F.FILM_ID = FA.FILM_ID
AND ACT.ACTOR_ID = FA.ACTOR_ID
AND F.TITLE = 'GOLD RIVER';
~~~~
26. Quais filmes a atriz ‘MILLA KEITEL’ trabalhou?
~~~
SELECT F.TITLE, A.FIRST_NAME, A.LAST_NAME
FROM FILM F, FILM_ACTOR FA, ACTOR A
WHERE F.FILM_ID = FA.FILM_ID AND A.ACTOR_ID = FA.ACTOR_ID
AND A.FIRST_NAME = 'MILLA' AND A.LAST_NAME = 'KEITEL';
~~~~
27. Listar os 5 países com o maior número de clientes.
~~~
SELECT CN.COUNTRY, COUNT(CS.CUSTOMER_ID) AS QTD
FROM CUSTOMER CS, COUNTRY CN, CITY CY, ADDRESS A
WHERE CS.ADDRESS_ID = A.ADDRESS_ID
AND A.CITY_ID = CY.CITY_ID
AND CY.COUNTRY_ID = CN.COUNTRY_ID
GROUP BY CN.COUNTRY
ORDER BY QTD DESC LIMIT 5;
~~~~
28. Listar todos os clientes que alugaram filmes de terror.
~~~
SELECT CS.FIRST_NAME, CS.LAST_NAME, COUNT(CS.CUSTOMER_ID) AS QTD,
CT.NAME
FROM CUSTOMER CS
INNER JOIN RENTAL R ON R.CUSTOMER_ID = CS.CUSTOMER_ID
INNER JOIN INVENTORY I ON R.INVENTORY_ID = I.INVENTORY_ID
INNER JOIN FILM F ON I.FILM_ID = F.FILM_ID
INNER JOIN FILM_CATEGORY FG ON F.FILM_ID = FG.FILM_ID
INNER JOIN CATEGORY CT ON FG.CATEGORY_ID = CT.CATEGORY_ID
WHERE CT.NAME = 'HORROR'
GROUP BY CS.CUSTOMER_ID;
~~~~
29. Listar todos os clientes que alugaram mais de 3 filmes de terror.
~~~
SELECT CS.FIRST_NAME, CS.LAST_NAME, COUNT(CS.CUSTOMER_ID) AS QTD,
CT.NAME
FROM CUSTOMER CS
INNER JOIN RENTAL R ON R.CUSTOMER_ID = CS.CUSTOMER_ID
INNER JOIN INVENTORY I ON R.INVENTORY_ID = I.INVENTORY_ID
INNER JOIN FILM F ON I.FILM_ID = F.FILM_ID
INNER JOIN FILM_CATEGORY FG ON F.FILM_ID = FG.FILM_ID
INNER JOIN CATEGORY CT ON FG.CATEGORY_ID = CT.CATEGORY_ID
WHERE CT.NAME = 'HORROR'
GROUP BY CS.CUSTOMER_ID
HAVING QTD > 3;
~~~~
30. Listar o primeiro e o último nome dos atores e atrizes com ‘SON’ no último
nome. Ordenar pelo primeiro nome.
~~~
SELECT FIRST_NAME, LAST_NAME
FROM ACTOR
WHERE LAST_NAME LIKE '%SON%'
ORDER BY FIRST_NAME ASC;
~~~~
31. Listar os nomes completos dos clientes residentes no Brasil. Devem ser
concatenados o primeiro e último nome.
~~~
SELECT CS.FIRST_NAME, CS.LAST_NAME
FROM CUSTOMER CS, COUNTRY CN, CITY CI, ADDRESS A
WHERE CN.COUNTRY_ID = CI.COUNTRY_ID
AND CI.CITY_ID = A.CITY_ID
AND A.ADDRESS_ID = CS.ADDRESS_ID
AND CN.COUNTRY = 'BRAZIL';
~~~~
32. Listar clientes ativos e seus respectivos endereços.
~~~
SELECT FIRST_NAME, LAST_NAME, ADDRESS
FROM CUSTOMER C, ADDRESS A
WHERE ACTIVE = 1 AND C.ADDRESS_ID = A.ADDRESS_ID;
~~~~
33. Listar a quantidade de filmes alugados por cliente em ordem decrescente de
quantidade de filmes alugados.
~~~
SELECT C.FIRST_NAME, C.LAST_NAME, COUNT(*) QTD
FROM CUSTOMER C, RENTAL R
WHERE C.CUSTOMER_ID = R.CUSTOMER_ID
GROUP BY (C.CUSTOMER_ID)
ORDER BY QTD DESC;
~~~~
34. Listar nomes dos clientes que possuem um filme alugado no momento.
~~~
SELECT C.FIRST_NAME, C.LAST_NAME
FROM CUSTOMER C
WHERE EXISTS
(SELECT 1
FROM RENTAL R
WHERE C.CUSTOMER_ID = R.CUSTOMER_ID AND R.RETURN_DATE IS NOT
NULL);
~~~~
36. Listar nomes dos clientes que não possuem um filme alugado no momento.
~~~
SELECT C.FIRST_NAME, C.LAST_NAME
FROM CUSTOMER C
WHERE NOT EXISTS
(SELECT 1
FROM RENTAL R
WHERE C.CUSTOMER_ID = R.CUSTOMER_ID AND R.RETURN_DATE IS NOT
NULL);
~~~~

## DCL e Segurança de Banco de Dados

1. Criar uma VIEW de lista de filmes infanto-juvenis de nome children-
teen_film_list. Esta VIEW deve conter as colunas de ID do filme, título, categoria,
preço do aluguel e lista dos atores que trabalham no filme, além é claro, da
classificação indicativa. A VIEW também deve incluir apenas os filmes das
classificações indicativas G, PG e PG-13, que são as categorias infanto-juvenis.
Devem ficar de fora os filmes das categorias voltadas a adultos (NC-17 e R). Os
registros na VIEW devem ser ordenados por categoria e título em ordem
alfabética.
~~~
CREATE VIEW CHILDREN_TEEN_FILM_LIST AS
(SELECT F.FILM_ID, F.TITLE, C.NAME, F.RENTAL_RATE, A.FIRST_NAME,
A.LAST_NAME, F.RATING
FROM FILM F, CATEGORY C, ACTOR A
WHERE F.RATING = 'G'
AND F.RATING = 'PG'
AND F.RATING = 'PG-13'
ORDER BY C.NAME DESC, F.TITLE DESC);
~~~~

2. Criar um ROLE NonTitular_client.
~~~
CREATE ROLE 'NonTitular_Client';
~~~~
3. Garantir os privilégios de consulta à VIEW children-teen_film_list ao ROLE
NonTitular_client criado.
~~~
GRANT SELECT ON SAKILA . CHILDREN_TEEN_FILM_LIST TO 'NonTitular_Client';
FLUSH PRIVILEGES;
~~~~
4. Criar no ambiente local (localhost) um usuário chamado Pedro e definir uma
senha para ele.
~~~
CREATE USER 'PEDRO'@'localhost' IDENTIFIED BY '123';
~~~~
5. Atribuir ao usuário Pedro o ROLE NonTitular_client.
~~~
GRANT NonTitular_Client TO 'PEDRO'@'localhost';
~~~~

## Transações

1. Desligar o autocommit do MySQL.
2. Criar um bloco de transação que execute as seguintes instruções:
a. Fazer um INSERT de dois registros quaisquer na tabela de filmes, com
classificações indicativas iguais a PG.
b. Criar um SAVEPOINT SP1.
c. Fazer um UPDATE em um dos registros recém-inseridos e mudar a
classificação indicativa para PG-13.
d. Fazer um SAVEPOINT SP2.
e. Fazer um SELECT para mostrar todos os filmes de classificação PG-13.
f. Fazer um DELETE de todos os filmes com a classificação PG-13.
g. Fazer um UPDATE do outro registro recém-inserido para PG-13.
h. Fazer um SELECT para mostrar todos os filmes de classificação PG-13.
i. Fazer um SAVEPOINT SP3.
j. Fazer um ROLLBACK para o SAVEPOINT SP1.
k. Fazer um COMMIT.
l. Fazer um RELEASE de todos os SAVEPOINT criados.
m. Fazer um DELETE de todos os filmes com a classificação PG.
n. Fazer um ROLLBACK.
o. Fazer um SELECT para mostrar todos os filmes de classificação PG.
3. Ligar o autocommit do MySQL.
~~~
SET AUTOCOMMIT = 0;
START TRANSACTION;
INSERT INTO FILM (FILM_ID, TITLE, DESCRIPTION, RELEASE_YEAR,
LENGUAGE_ID, ORIGINAL_LENGUAGE_ID,
RENTAL_DURATION, RENTAL_RATE, LENGTH, REPLACEMENT_COST, RATING,
SPECIAL_FEATURES, LAST_UPDATE)
VALUES (1001, 'THE FILM', 'THIS IS A FILM', 2021, 1, NULL, 5, 0.98, 210, 20.98,
'PG', 'TRAILERS', NULL);
INSERT INTO FILM (FILM_ID, TITLE, DESCRIPTION, RELEASE_YEAR,
LENGUAGE_ID, ORIGINAL_LENGUAGE_ID,
RENTAL_DURATION, RENTAL_RATE, LENGTH, REPLACEMENT_COST, RATING,
SPECIAL_FEATURES, LAST_UPDATE)
VALUES (1002, 'THE FILM 2', 'THIS IS A FILM 2', 2021, 1, NULL, 5, 0.98, 210,
20.98, 'PG', 'TRAILERS', NULL);
SAVEPOINT SP1;
UPDATE FILM SET RATING = 'PG-13' WHERE FILM_ID = 1001;
SAVEPOINT SP2;
SELECT * FROM FILM WHERE RATING = 'PG-13';
DELETE FROM FILM WHERE RATING = 'PG-13';
UPDATE FILM SET RATING = 'PG-13' WHERE FILM_ID = 1002;
SELECT * FROM FILM WHERE RATING = 'PG-13';
SAVEPOINT SP3;
ROLLBACK TO SP1;
COMMIT;
RELEASE SAVEPOINT SP1;
RELEASE SAVEPOINT SP2;
RELEASE SAVEPOINT SP3;
DELETE FROM FILM WHERE RATING = 'PG-13';
ROLLBACK;
SELECT * FROM FILM WHERE RATING LIKE '%PG%';
SET AUTOCOMMIT = 1;
~~~~

## Índices

1. Faça uma consulta que exiba a quantidade de cidades cadastradas agrupadas por
país (nome).
~~~
SELECT COUNT(CY.CITY), CN.COUNTRY
FROM COUNTRY CN, CITY CY
WHERE CN.COUNTRY_ID = CY.COUNTRY_ID
GROUP BY CN.COUNTRY;
~~~~
2. Faça um EXPLAIN SELECT da sua consulta.
~~~
EXPLAIN SELECT COUNT(CY.CITY), CN.COUNTRY
FROM COUNTRY CN, CITY CY
WHERE CN.COUNTRY_ID = CY.COUNTRY_ID
GROUP BY CN.COUNTRY;
~~~~
3. Crie um índice para o nome do país.
~~~
CREATE INDEX IDX_COUNTRY_NAME ON COUNTRY(COUNTRY);
~~~~
4. Faça um EXPLAIN SELECT para a consulta novamente.
~~~
EXPLAIN SELECT COUNT(CY.CITY), CN.COUNTRY
FROM COUNTRY CN, CITY CY
WHERE CN.COUNTRY_ID = CY.COUNTRY_ID
GROUP BY CN.COUNTRY;
~~~~

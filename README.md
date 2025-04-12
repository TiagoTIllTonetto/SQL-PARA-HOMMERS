## SQL PARA HOMMERS 
#Por Tiago Till Tonetto

## HAVING 
 
Having é basicamente usado em juncao com o group by para filtrar resultados de um agrupamento.

```
SELECT coluna1, funcaoAgregacao(coluna2)
FROM nomeTabela
GROUP BY coluna1
HAVING condicao;

```

## DIFERENCA ENTRE HAVING E WHERE

É QUE O GROUP BY  É APLICADO DEPOIS QUE OS DADOS JA FORAM AGRUPADOS, ENQUANTO O WHERE É APLICADO ANTES DOS DADOS SEREM AGRUPADOS

## EXEMPLO HAVING DEPOIS DOS DADOS AGRUPADOS
```
SELECT FirstName, COUNT(FirstName) AS "QUANTIDADE"
FROM Person.Person
GROUP BY FirstName
HAVING COUNT(Firstname) > 10

```

## outro exemplo:

```SQL

SELECT Productid, SUM(linetotal) as "TOTAL"
FROM Sales.SalesOrderDetail
GROUP BY ProductID
HAVING SUM(linetotal) BETWEEN 16200 AND 50000

```

## WHERE E HAVING NA PESQUISA

````

SELECT FirstName, COUNT(FirstName) AS "Quantidade"
FROM Person.Person
WHERE Title = 'MR.'
GROUP BY FirstName
HAVING COUNT(FirstName) > 10
GO

````

## HAVING

````
SELECT StateProvinceID, COUNT(StateProvinceid) AS "QUANTIDADE"
FROM Person.Address
GROUP BY StateProvinceID
HAVING COUNT(StateProvinceID) > 1000;

````
````
SELECT ProductID, AVG(linetotal)
FROM Sales.SalesOrderDetail
GROUP BY ProductID
HAVING AVG(linetotal) < 100000

````

## AS  RENOMEAR AS COLUNAS 

````
SELECT TOP 10 AVG(listPrice) AS "PREÇO MEDIO"
FROM Production.Product

````


````
SELECT TOP 10 Firstname AS "NOME", LastName AS "SOBRENOME" 
FROM person.person

````
````
SELECT TOP 10 ProductNumber AS "NUMERO do Produto"
FROM Production.Product

````
````
SELECT unitPrice AS "PRECO UNITARIO"
FROM Sales.SalesOrderDetail
````

## SQL INNER JOIN  
INNER JOIN JUNTA VALORES CORRESPONDENTES DE CAMPOS COMUNS
````
SELECT C.ClienteID, C.Nome, E.Rua, E.Cidade
FROM Cliente C
INNER JOIN Endereco E on E.EnderecoID = C.EnderecoID;
````
````
SELECT P.BusinessEntityID, P.Firstname, P.LastName, pe.EmailAddress
FROM Person.Person as P
INNER JOIN Person.EmailAddress PE on P.BusinessEntityID = pe.BusinessEntityID
````

## Tipos de join / alguns mais usados 
INNER JOIN retorna apenas os resultados
que correspondem (EXISTEM) tanto na tabelaA 
como na tabelaB

FULL OUTER JOIN retorna um conjunto de todos
os registros correspondentes da tabelaA e tabelaB
quando sao iguai. e alem disso se não 
houver valores correspondetes, ele simplismente ira preencher
esse lado com NULL

LEFT OUTER JOIN / ou resumindo LEFT JOIN retorna um conjunto de todos os registro
da tabelaA e alem disso os registros
correspondentes quando disponiveis na tabelaB. Se
não houver registros correspondetes ele simplesmente 
vai preencher com "NULL"

RIGHT OUTER JOIN ou resumindo / RIGHT JOIN A LÓGICA É A MESMA DO LEFT MAS OLHANDO PRA tabelaB no caso 

````
SELECT P.BusinessEntityID, P.Firstname, P.LastName, pe.EmailAddress
FROM Person.Person as P
LEFT JOIN Person.EmailAddress PE on P.BusinessEntityID = pe.BusinessEntityID
````

## UNION combina doius ou mais resultados de um select em um resultado apenas LEMBRANDO ter a mesma quantidade de colunas e  o mesmo tipo de DADOS.
````
SELECT coluna1, coluna2
FROM tabela1
UNION
SELECT coluna1, coluna2
FROM tabela2
````
## UNION remove os valores duplicados,  UNION ALL tras ate mesmo os dados duplicados
DATEPART
````
SELECT SalesOrderID, DATEPART(MONTH, orderdate) AS "MES"
FROM Sales.SalesOrderHeader
````
````
SELECT AVG(TotalDUE) AS MEDIA, DATEPART(MONTH, OrderDate) AS MES
FROM Sales.SalesOrderHeader
GROUP BY DATEPART(MONTH, Orderdate)
ORDER BY MES;
````
````
SELECT AVG(TotalDUE) AS MEDIA, DATEPART(YEAR,  OrderDate) AS MES
FROM Sales.SalesOrderHeader
GROUP BY DATEPART(YEAR, Orderdate)
ORDER BY MES;
````
````
SELECT AVG(TotalDUE) AS MEDIA, DATEPART(DAY,  OrderDate) AS MES
FROM Sales.SalesOrderHeader
GROUP BY DATEPART(DAY, Orderdate)
ORDER BY MES;
````
````
SELECT AVG(TotalDUE) AS MEDIA, DATEPART(MONTH,  OrderDate) AS MES
FROM Sales.SalesOrderHeader
GROUP BY DATEPART(MONTH, Orderdate)
ORDER BY MES;
````

## OPERACOES DE STRING
````
SELECT CONCAT(FirstName, Lastname)
FROM Person.Person;
````
OU
````
SELECT CONCAT(FirstName, ' ', Lastname)
FROM Person.Person;
````

## TRAZER EM MAIUSCULO UPPER, TRAZER em minusculo LOWER
````
SELECT UPPER(FirstName), LOWER(LastName)
FROM Person.Person;
````
## Ler a quantidade de caracteres
````
SELECT Firstname, LEN(FirstName)
FROM Person.Person;
````
## SUBSTRING EXTRAIR DADOS DE STRING
````
SELECT Firstname, SUBSTRING(FirstName, 1,3)
FROM Person.Person
````

## REPLACE substituir 

````
SELECT REPLACE(ProductNumber, '-', 'teste')
FROM Production.Product
````

## FUNÇOES MATEMATICA ROUND para arredondar,  SQRT raiz quadrada
````
SELECT ROUND(LineTotal, 3), LineTotal
FROM Sales.SalesOrderDetail;
````
````
SELECT SQRT(LineTotal), LineTotal
FROM Sales.SalesOrderDetail;
````
## SUBQUERY (SUBSELECT)  SELECT DENTRO DE OUTRO SELECT 
````
SELECT  * FROM Production.Product
WHERE ListPrice > (SELECT AVG(listPrice) FROM Production.Product)
````
## Outro exemplo
````
SELECT * FROM HumanResources.Employee
WHERE JobTitle = 'DESIGN ENGINEER';
````
````
SELECT FirstName 
FROM Person.Person
WHERE BusinessEntityID IN(SELECT BusinessEntityID FROM HumanResources.Employee
WHERE JobTitle = 'Design Engineer');
````
## mesmo SELECT com JOIN
````
SELECT Firstname
FROM Person.Person P
INNER JOIN HumanResources.Employee E ON P.BusinessEntityID = E.BusinessEntityID
AND E.JobTitle = 'Design Engineer';
````
## DICA VER qual das duas formas trás mais rapido as informação no exemplo acima  as duas condições usaram o mesmo tempo
````
SELECT * FROM Person.Address
WHERE StateProvinceID IN (
SELECT StateProvinceID FROM Person.StateProvince
WHERE NAME = 'ALBERTA')
````

## SELF JOIN AGRUPAR/ORDENAR OS DADOS DENTRO DE UMA MESMA TABELA mas pra isso é necessario usar ALIAS = AS  EXEMPLO tabela1 AS "NOME" */
````
SELECT A.ContactName, A.region, B.ContactName, B.Region
FROM Customers A, Customers B
WHERE A.REGION = B.REGION
````
## outro exemplo
````
SELECT A.firstname, A.hiredate, B.Firstname, B.hiredate
FROM Employees A, Employess B
WHERE DATEPART(YEAR, A.HIREDATE) = DATEPART(YEAR, B.HIREDATE);
````

##TIPOS DE DADOS
1 BOLEANOS
2 CARACTERES
3 NUMEROS
4 TEMPORARIOS
## Boleano pode padrao ele inicialzado como NULL, e pode receber 1 ou 0
Caracteres tamanho fixo - char // permite inserir ate uma quantidade fixa de caracteres  sempre ocupa doto o espaço reservado.
Taamanhos Variaveis - Varchar ou nvarchar // permite inserir até uima qunadidade que for definido porem
só usa o espaço que for preenchido utilizando por exemlo 10 de 50 

## Numeros
VALORES EXATOS:
TYNYINT - Não tem valor fracionado (exe: 1.43, 24.23) somente 1,1232123, 324234, 6545848 etc...
SMALLINT - Mesma coisa porem limite maior
INT - Mesma coisa porem limite maior
BIGINT - Mesma coisa porem limite maior
NUMERI ou DECIMAL valores exatos, porem permite ter a parte fracionados, que tambem pode ser espe  cificado   escala (escala é o numero de digitos na parte pracional)
exemplo NUMERIC (5,2) dois valores apos a virgula exemplo 113,44
## APROXIMADOS:
REAL - Tem precisão aproximada de ate 15 digitos
FLOAT - mesmo conceito do real
## TEMPORAIS:
DATE - Armazena data no formado aaaa/mm/dd
DATETIME - Armazena data e horas no formato aaaa/mm/dd:hh:mm:ss
DATETIME2 - Data e horas com adição de milissegundos no formato aaaa/mm/dd:hh:mm:sssssss
SMALLDATETIME - Data e hora respeitando um range (pesquisa sobre o range)
TIME - Horas minutos, segundos e milisegundos (pesquisar sobre o range)
DATETIMEOFFSET Permite armazernar informações data e hora incluindo fuso horario

## IDENTITY - AUTO_INCREMENT para autoincrementar valores recomendado usar em PRIMARY KEY
--EXEMPLO
````
CREATE TABLE FRUTAS(
	IDFRUTA INT PRIMARY KEY IDENTITY,
	NOME VARCHAR(30) NOT NULL,
	TIPO VARCHAR(30) CHECK (TIPO IN ('VERMELHAS','CITRICAS')) NOT NULL,
	CADASTRO DATE    
);
````
## REMOVENDO COLUNA PRECO
````
ALTER TABLE FRUTAS 
DROP COLUMN PRECO
GO
CREATE TABLE PRECOS(
	IDPRECO INT PRIMARY KEY IDENTITY,
	PRECO MONEY,
	ID_FRUTA INT
)
````
## DICA Criar a FOREIGN KEY por fora da tabela ou seja ALTER TABLE  Para dar o nome a CONSTRAINT
````
ALTER TABLE PRECOS ADD CONSTRAINT PRECO_FRUTA
FOREIGN KEY (ID_FRUTA) REFERENCES FRUTAS (IDFRUTA)
````
DEVE SER FEITA DESSA FORMA A CRIAÇÃO DA CONSTRAINT e não por dentro da tabelapois dessa forma conseguimos dar o nome no caso PRECO_FRUTA sabemos que tem uma restrição na tabela preco_fruta .ao verificar internamente do banco ao inves de um código alátorio 177363287623KHHAG

## Alterando colunas
````
ALTER TABLE FRUTAS
ALTER COLUMN NOME VARCHAR(40) NOT NULL;
````

## SQL SERVER RENOMEANDO TABELA COM PROCEDURES

FRUTAS =  TABELA
NOME  = COLUNA A SER ALTERADA
DESCRICAO = NOVO NOME DA COLUNA 
COLUMN - PARA INDICAR QUE É UMA ALTERAÇÃO DE COLUNA
````
EXEC sp_rename 'FRUTAS.NOME', 'DESCRICAO', 'COLUMN'
````
## INSERT como usamos IDENTITY para auto incrementar  não precisamos informar nada no ID
````
INSERT INTO FRUTAS VALUES ( 'MAÇA', 'VERMELHAS', '2025-04-02')
INSERT INTO PRECOS VALUES (12.00, 2);
````
## INSERINDO TABELA COPIA
````
SELECT * INTO TESTE FROM FRUTAS;
````
## UPDATE sempre cuidar para utilizar o filtro WHERE 
````
UPDATE TESTE SET NOME = 'BANANA',tipo = 'citricas'
WHERE IDFRUTA = 3;
````
## DELETE sempre cuidar para realizar o filtro com WHERE
````
DELETE FROM TESTE WHERE IDFRUTA = 5;
````
## EXCLUINDO TABELAS
````
DROP TABLE PRECOS
````
Lembrando que não conseguimos excluir tabelas que são referenciados por outras
Por exemplo não conseguimos usar o DROP TABLE FRUTAS, pois a tabela FRUTAS possui uma chave estrangeira em PRECOS 
para isso precisa DROP PRECOS para DEPOIS usar o DROP em FRUTAS.

Caso tentassemos DROP TABLE FRUTAS iriamos nos deparar com a seguinte mensagem:
Não foi possível descartar o objeto 'FRUTAS' porque há referência a ele em uma restrição FOREIGN KEY.

TRUNCATE TABLE 
REMOVE TODAS AS LINHAS DE UMA TABELA
TRUNCATE TABLE PRECOS

## VIEWS São tabelas criadas para consulta onde personalizamos os dados por exemplo na tabela
frutas temos os seguintes dados IDFRUTA, DESCRICAO, TIPO E CADASTRO na VIEW vamos personalizar a nossa busca exemplo :
````
CREATE VIEW [FRUTAS VERMELHAS] AS 
SELECT DESCRICAO, TIPO FROM FRUTAS
WHERE TIPO = 'VERMELHAS';
````
## A VIEW é baseada na tabela FRUTAS porem vai trazer apenas a DESCRICAO E O TIPO otimizando assim a consulta .
````
SELECT * FROM [FRUTAS VERMELHAS];
````
````
DROP TRIGGER TRG_ATUALIZA_PRECO
````
## CRIANDO TRGGERS Para atualizar o preço apos o UPDATE
Ao realizarmos o UPDATE de algum valor no banco esse valor ser MENOR que 9 a TRIGGER ira dispara e fará a correção do PREÇO no caso para 10.00
o exemplo da TRIGGER É em SQL SERVER
````
CREATE TRIGGER TRG_ATUALIZA_PRECO
ON PRECOS
FOR UPDATE
AS
BEGIN
	
	DECLARE @PRECO MONEY
	
	SELECT @PRECO = PRECO FROM inserted

	UPDATE PRECOS SET PRECO = 10.00 WHERE PRECO < 9

END
GO
````
## REMOVENDO A TRIGGER DO BANCO
````
DROP TRIGGER TRG_ATUALIZA_PRECO
````

## Criando Trigger para atualizar o preço ao inserir o valor no banco
Ao inserir um valor no banco que seja menor que 8.00 a trigger irá disparar fazendo a correção do valor para 10.00 ou seja todo o valor MENOR que 8.00
sera corrido pela trigger ao ser inserido. 
A trigger foi feita em SQL SERVER.

````
CREATE TRIGGER TRG_ATUALIZA_PRECO_AO_CADASTRAR
ON PRECOS
FOR INSERT
AS
	BEGIN
	
	DECLARE
	@PRECO MONEY

	SELECT @PRECO = PRECO FROM inserted

	UPDATE PRECOS SET PRECO = 10.00 WHERE PRECO < 8


	END
GO
````

## Removendo os dados ao realizar o INSERT 
Ao inserir valores que sejam MENORES que 8.00 a trigger era remover o insert, Trigger feita em SQL SERVER.
````
CREATE TRIGGER TRG_DELETA_PRECO_AO_CADAtradar
ON PRECOS
FOR INSERT
AS
	BEGIN
	
	DECLARE
	@PRECO MONEY

	SELECT @PRECO = PRECO FROM deleted

	delete PRECOS  WHERE PRECO < 8


	END
GO
````


CREATE TABLE USUARIOS (
	IDUSUARIO INT PRIMARY KEY IDENTITY,
	NOME VARCHAR (30)
)
GO

## Criando tabelas 
````
CREATE TABLE SALARIOS (
	IDSALARIO INT PRIMARY KEY IDENTITY,
	SALARIO MONEY,
	ID_USUARIO INT
);
````
## Alterando tabela e inserindo CONSTRAINT
````
ALTER TABLE SALARIOS ADD CONSTRAINT FK_SALARIOS_USUARIOS 
FOREIGN KEY (ID_USUARIO) REFERENCES USUARIOS (IDUSUARIO);
````
## Realizando Insert
````
INSERT INTO SALARIOS VALUES (1500.00,1)
INSERT INTO SALARIOS VALUES (2500.00,2)
INSERT INTO SALARIOS VALUES (3500.00,3)
INSERT INTO SALARIOS VALUES (4500.00,4)
INSERT INTO SALARIOS VALUES (5500.00,5)
````



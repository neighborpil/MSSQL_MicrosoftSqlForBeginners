# MSSQL_MicrosoftSqlForBeginners
Examples Codes for Training
==========================
## Infos
 - 연결 문자열 : Server=localhost\SQLEXPRESS01;Database=master;Trusted_Connection=True;
 - SqlServer 설치 로그 폴더 : C:\Program Files\Microsoft SQL Server\150\Setup Bootstrap\Log\20210803_085457
 - 설치 미디어 폴더 : C:\SQL2019\Express_ENU
 - 설치 리소스 폴더 : C:\Program Files\Microsoft SQL Server\150\SSEI\Resources

## 설치
 1. https://www.microsoft.com/en-us/sql-server/sql-server-downloads 페이지에서 Express 설치
 2. 예제 파일을 C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\DATA 폴더에 넣기
 3. SQL Management Studio(SSMS) 설치 (https://docs.microsoft.com/ko-kr/sql/ssms/download-sql-server-management-studio-ssms?redirectedfrom=MSDN&view=sql-server-ver15)
 4. 예제파일을 C:\Program Files\Microsoft SQL Server\MSSQL15.SQLEXPRESS01\MSSQL\DATA 폴더에 복사
 5. SSMS 실행 후 DB에 접속
 6. 화면 좌측 탐색기에서 Database 우클릭 하여 Attach 선택
 7. 화면에서 로그는 삭제
 8. 완료

## Schema
 - Logical and Physical group of database
 - HumanResources is Schema
 - Department, Employee, EmployeeDepartmentHistory and so on are tables

![image](https://user-images.githubusercontent.com/22423285/128100338-68ae23e3-f5e9-44e0-85b5-28dc5261ec25.png)
 - Table is a collection of data in rows and cols

#### ※ 단축키
 - 쿼리실행 : F5


#### ※ 한글을 사용하려면?
 - SSMS의 설정에서 Language를 한국어로 바꾸면 한글 사용 할 수 있다.

### Select
 - select (4*4)-(3/2);
 - select 'test';
 - select 'Test', 3*3;
 - SELECT [Column 1], [Column 2], ..., [Column n] FROM [Database Name].[Schema Name].[Table Name]
 - SELECT FirstName FROM Person.Person

#### ※ 테이블, 컬럼에 대소문자 구분하지 않을 수 있다
 - select firstname, lastname from person.person;
 - select Firstname, LastName from Person.person;

### Select시 가져오는 rows 리미트 걸기
 - TOP operator를 쓰면 된다
 - TOP 숫자
 - TOP 숫자 PERCENT : 퍼센트만큼 가져온다
   + SELECT TOP 5 FirstName, MiddleName, LastName FROM Person.Person;
   + select top 100 * from Production.Product;
 - Alias 사용법 : AS [별칭] 또는 AS "별칭"
   + select top 100 FirstName AS [Customer First Name], LastName AS "Customer Last Name"
   from person.person;
 - View를 쿼리할때도 동일하다
   + select FirstName, LastName, EmailAddress, PhoneNumber from sales.vIndividualCustomer;
### Where clause
 - <> : not operatior
 - 대부분의 DBMS는 incase sensitive하다
   + select * from HumanResources.vEmployee where FirstName = 'Chris'
   + select * from HumanResources.vEmployee where FirstName = 'CHRIS'

#### Where clause LIKE
 - '_'를 통하여 한글자 모를 경우 사용 가능
   SELECT *
   FROM HumanResources.vEmployee
   WHERE FirstName LIKE 'MI_';
   
#### Where clause Wirdcard Expression
 - D로 시작하고 n으로 끝나는데 중간 글자가 a 또는 o
   + SELECT * FROM HumanResources.vEmployee WHERE FirstName LIKE 'D[a,o]n';
 - D로 시작하고 n으로 끝나는데 중간 글자가 a-f, r-z범위
   + SELECT * FROM HumanResources.vEmployee WHERE FirstName LIKE 'D[a-f, r-z]n';
 - D로 시작하고 n으로 끝나는데 중간 글자가 a가 아님
   + SELECT * FROM HumanResources.vEmployee WHERE FirstName LIKE 'D[^a]n';

#### Where clause filtering null value
 - IS NULL keyword 사용
   + SELECT * FROM Person.Person WHERE MiddleName IS NULL;
 - IS NOT NULL : null이 아닌 것
   + SELECT * FROM Person.Person WHERE MiddleName IS NOT NULL;

#### Order by clause
 - by column name, column ordinal(starts from 1), column alias

#### Order of sql excuetes
 1. FROM
 2. WHERE
 3. GROUP BY
 4. HAVING
 5. SELECT
 6. ORDER BY

 - 이 순서로 인하여 WHERE, GROUP BY, HAVING에서는 Column alias를 못 쓰지만, ORDER BY에서는 사용하는 것이 가능하다

```
-- order by lastname and firstname
select firstname as [customer name], lastname
from Sales.vIndividualCustomer
order by 2, [customer name];

select LastName, FirstName, SalesQuota [Sales Quota]
from Sales.vSalesPerson
where SalesQuota >= 250000
order by [Sales Quota] desc, LastName asc
```

### Join
#### Inner Join
 - only returns when joining columns exists
 - join or inner join
```
select p.name, p.ProductNumber, p.ProductSubcategoryID, ps.name AS "ProductSubCategory Name"
from Production.Product p
inner join Production.ProductSubcategory ps
on p.ProductSubcategoryID = ps.ProductSubcategoryID

select 
	ps.Name as ProductSubCatetoryName,
	pc.Name as ProductCategoryName
from Production.ProductSubcategory ps
inner join Production.ProductCategory pc
on ps.ProductCategoryID = pc.ProductCategoryID

select p.FirstName, p.LastName, e.EmailAddress, pp.PhoneNumber
from Person.Person p
inner join Person.EmailAddress e
on p.BusinessEntityID = e.BusinessEntityID
inner join Person.PersonPhone pp
on pp.BusinessEntityID = pp.BusinessEntityID
```

#### Left Outer Join, Right Outer Join
```
select p.Name, p.ProductNumber, ps.Name as ProductSubCategoryName
from Production.Product p
left outer join Production.ProductSubcategory ps
on ps.ProductSubcategoryID = p.ProductSubcategoryID

select p.Name, p.ProductNumber, ps.Name as ProductSubCategoryName
from Production.Product p
right outer join Production.ProductSubcategory ps
on ps.ProductSubcategoryID = p.ProductSubcategoryID

select 
	p.FirstName, p.LastName,
	soh.SalesOrderNumber,
	soh.TotalDue as SalesAmount,
	t.Name as TerrioryName
from Sales.SalesOrderHeader soh
left outer join Sales.SalesPerson sp
on sp.BusinessEntityID = soh.SalesPersonID
left outer join HumanResources.Employee e
on e.BusinessEntityID = sp.BusinessEntityID
left outer join Person.Person p
on p.BusinessEntityID = e.BusinessEntityID
left outer join Sales.SalesTerritory t
on t.TerritoryID = soh.TerritoryID
where t.Name = 'Northwest'
order by 4 desc
```

### Aggrigate functions
 - MAX
```
select MAX(TotalDue)
from Sales.SalesOrderHeader
```
 - MIN

```
select MIN(TotalDue)
from Sales.SalesOrderHeader
```
 - COUNT
```
select COUNT(TotalDue)
from Sales.SalesOrderHeader

```
   + 기본적으로 NOT NULL조건인것과 같음
```
SELECT COUNT(SalesPersonID)
FROM Sales.SalesOrderHeader

SELECT COUNT(*)
FROM Sales.SalesOrderHeader
WHERE SalesPersonID IS NOT NULL
```
 - count distinct
```
SELECT COUNT(DISTINCT FirstName)
FROM Person.Person
```
 - AVG
```
select avg(TotalDue)
from Sales.SalesOrderHeader
```
 - SUM
```
select SUM(TotalDue)
from Sales.SalesOrderHeader

SELECT SUM(TotalDue)
FROM Sales.SalesOrderHeader
WHERE OrderDate between '2006-1-1' AND '2006-12-31'

-- Error: nvarchar cannot be added
select SUM(FirstName)
from Person.Person
```

### Group By
```
select SalesPersonId, SUM(TotalDue) as [Total Sales]
from Sales.SalesOrderHeader
group by SalesPersonID

select 
	ProductID, 
	sum(Quantity) as [Total in Stock],
	count(*) as [Total Locations]
from Production.ProductInventory
group by ProductID

select 
	st.Name AS [Territory Name], 
	p.FirstName + ' ' + p.LastName AS [Sales Person Name],
	SUM(TotalDue) as [Total Sales]
from Sales.SalesOrderHeader soh
inner join Sales.SalesPerson sp
on sp.BusinessEntityID = soh.SalesPersonID
inner join Person.Person p
on p.BusinessEntityID = sp.BusinessEntityID
inner join Sales.SalesTerritory st
on st.TerritoryID = soh.TerritoryID
where OrderDate Between '1/1/2006' AND '12/31/2006'
group by st.Name, p.FirstName + ' ' + p.LastName
order by 1, 2
```

### Having Clause

```
select 
	st.Name as [Territory Name],
	sum(TotalDue) as [Total Sales - 2006]
from Sales.SalesOrderHeader soh
inner join sales.SalesTerritory st
on st.TerritoryID = soh.TerritoryID
where OrderDate between '1/1/2006' and '12/31/2006'
group by st.Name
having sum(TotalDue) > 4000000
order by 1

select 
	ps.Name as [SubCategory Name],
	count(*) as [Product Count]
from Production.Product p
inner join Production.ProductSubcategory ps
on ps.ProductSubcategoryID = p.ProductSubcategoryID
group by ps.Name
having count(*) >= 15
order by 1

select 
	Department as [Department Name],
	Count(*) as [Employee Count]
from HumanResources.vEmployeeDepartment
group by Department
having count(*) in (6, 10)

select 
	SalesPersonID,
	SUM(TotalDue) as [Total Sales Amount],
	Count(*) as [Total Sales Count]
from Sales.SalesOrderHeader
where OrderDate between '1/1/2006' and '12/31/2006'
	and SalesPersonID is not null
group by SalesPersonID
having SUM(TotalDue) > 2000000
	AND COUNT(*) > 75
order by [Total Sales Amount] desc
```

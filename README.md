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
 - SELECT [Column 1], [Column 2], ..., [Column n]
   FROM [Database Name].[Schema Name].[Table Name]
 - SELECT FirstName
   FROM Person.Person

#### ※ 테이블, 컬럼에 대소문자 구분하지 않을 수 있다
 - select firstname, lastname from person.person;
 - select Firstname, LastName from Person.person;

### Select시 가져오는 rows 리미트 걸기
 - TOP operator를 쓰면 된다
 - TOP 숫자
 - TOP 숫자 PERCENT : 퍼센트만큼 가져온다
 - SELECT TOP 5 FirstName, MiddleName, LastName
   FROM Person.Person;

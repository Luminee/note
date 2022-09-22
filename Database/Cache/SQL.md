### Query

##### 1. Select

```sql
SELECT Name, DOB FROM Sample.Person WHERE Name %STARTSWITH 'A' ORDER BY DOB
```

```sql
SELECT Interns FROM 
	(SELECT Name As Interns FROM Sample.Employee WHERE Age < 21)
WHERE Interns %STARTSWITH 'A'
```

```sql
# list to string with `^`
SELECT TOP 4 Name,$LISTTOSTRING(Home,'^') FROM Sample.Person
```

```sql
# where list element contain `Red`
SELECT Name,FavoriteColors FROM Sample.Person
    WHERE FOR SOME %ELEMENT(FavoriteColors) (%Value = 'Red')
```

```sql
# where list element contain `Red` or `Blue` or `Green`
SELECT Name,FavoriteColors FROM Sample.Person
    WHERE FOR SOME %ELEMENT(Sample.Person.FavoriteColors)
        (%Value IN ('Red', 'Blue', 'Green'))
```

```sql
# match case-sensitive
SELECT Home_City FROM Sample.Person WHERE %EXACT(Home_City) = 'albany'
```

##### 2. Join

```sql
SELECT P.Home_City, P.Home_Zip AS FakeZip, Z.ZipCode, Z.City AS ZipCity, Z.State
FROM Sample.Person AS P 
LEFT OUTER JOIN Sample.USZipCode AS Z ON P.Home_Zip=Z.ZipCode
WHERE Z.ZipCode IS NOT NULL
ORDER BY P.Home_City
```

### DDL

##### 1. Create Table

```sql
CREATE TABLE my_test (
	MyId INT PRIMARY KEY,
    LastName VARCHAR(20) CONSTRAINT unq_lname UNIQUE CONSTRAINT nonull_lname NOT NULL,
    FirstName VARCHAR(20) DEFAULT '***TBD***',
    DateStamp DATETIME DEFAULT CURRENT_TIMESTAMP
)
```

##### 2. Alter Table

```sql
ALTER TABLE tablename ADD fieldname datatype
ALTER TABLE tablename ALTER COLUMN fieldname newdatatype
ALTER TABLE tablename DROP COLUMN fieldname
ALTER TABLE tablename ADD tableconstraint FOR fieldname
ALTER TABLE tablename DROP tableconstraint
ALTER TABLE tablename ADD CONSTRAINT constraint FOREIGN KEY
ALTER TABLE tablename DROP FOREIGN KEY role
ALTER TABLE tablename ADD CONSTRAINT constraint DEFAULT defaultvalue FOR fieldname
ALTER TABLE tablename DROP CONSTRAINT constraint
```

##### 3. Index

```sql
CREATE [UNIQUE] INDEX indexname ON tablename (fieldname [,fieldname2])
DROP INDEX tablename.indexname [,tablename.indexname]
```

##### 4. Trigger

```sql
CREATE TRIGGER triggername ON tablename
[WITH ENCRYPTION]
{FOR | AFTER | INSTEAD OF} {INSERT | DELETE | UPDATE}
[WITH APPEND]
[NOT FOR REPLICATION] 
AS tsql_trigger_code

DROP TRIGGER [owner.]triggername
```

##### 5. View

```sql
CREATE VIEW [owner.]viewname 
[WITH {ENCRYPTION | SCHEMABINDING | VIEW_METADATA}]
AS select_statement
[WITH CHECK OPTION]

DROP VIEW viewname [,viewname2 [,...] ]
```

##### 6. Database

```sql
CREATE DATABASE dbname
DROP DATABASE dbname
```

### DML

##### 1. Delete

```sql
DELETE FROM tablename  WHERE condition
DELETE FROM t1 FROM t2 WHERE t1.fieldname = t2.fieldname
```

##### 2. Insert

```sql
INSERT [INTO] tablename (fieldname[,fieldname2[,...]]) VALUES (list_of_values)
```

##### 3. Update

```sql
UPDATE tablename SET fieldname=value[,fieldname2=value2[,...]]
   WHERE [tablename.]fieldname=value
```

##### 4. Trancate

```sql
TRUNCATE TABLE tablename
```

##### 5. Text (stream field)

```sql
DECLARE @ptrval binary(16);
SELECT @ptrval = TEXTPTR(Notes) FROM Sample.Person
READTEXT Sample.Person.Notes @ptrval 0 0

DECLARE @ptrval binary(16);
SELECT @ptrval = TEXTPTR(Notes) FROM Sample.Person
WRITETEXT Sample.Person.Notes @ptrval 'This is the new text value'

DECLARE @ptrval binary(16);
SELECT @ptrval = TEXTPTR(Notes) FROM Sample.Person
WRITETEXT Sample.Person.Notes @ptrval 0 0 'New'
```

### Grant

##### 1. User

```sql
CREATE USER username 
```



###  DDL Object

##### 1. Create Query and Call

```sql
CREATE QUERY Sample.AgeQuery(IN topnum INT DEFAULT 10,IN minage INT 20)
   PROCEDURE
   BEGIN
   SELECT TOP :topnum Name,Age FROM Sample.Person
   WHERE Age > :minage 
   ORDER BY Age ;
   END
```

```sql
DROP QUERY Sample.AgeQuery
```

```sql
CREATE PROCEDURE Sample.AgeQuerySP(IN topnum INT DEFAULT 10,IN minage INT 20)
   BEGIN
   SELECT TOP :topnum Name,Age FROM Sample.Person
   WHERE Age > :minage ;
   END
```

##### 2. View

```sql
CREATE VIEW [owner.]viewname 
[WITH {ENCRYPTION | SCHEMABINDING | VIEW_METADATA}]
AS select_statement
[WITH CHECK OPTION]
```

##### 3. Procedure

```sql
CREATE PROCEDURE procname [[@var [AS] datatype [= | DEFAULT value] [,...]] [RETURNS datatype] [AS] code
```




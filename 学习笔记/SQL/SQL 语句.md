### SQL 

*SQL 大小写不敏感

#### SQL SELECT 语句

SELECT 语句用于从表中选取数据。

结果被存储在一个结果表中（称为结果集）。

语法

```sql
SELECT 列名称 FROM 表名称
```

以及：

```sql
SELECT * FROM 表名称
```



#### **SQL SELECT DISTINCT 语句**

在表中，可能会包含重复值。这并不成问题，不过，有时您也许希望仅仅列出不同（distinct）的值。

关键词 DISTINCT 用于返回唯一不同的值。

##### **语法：**

```sql
SELECT DISTINCT 列名称 FROM 表名称
```



#### WHERE 子句

如需有条件地从表中选取数据，可将 WHERE 子句添加到 SELECT 语句。

##### 语法

```sql
SELECT 列名称 FROM 表名称 WHERE 列 运算符 值
```

下面的运算符可在 WHERE 子句中使用：

| 操作符  | 描述         |
| :------ | :----------- |
| =       | 等于         |
| <>      | 不等于       |
| >       | 大于         |
| <       | 小于         |
| >=      | 大于等于     |
| <=      | 小于等于     |
| BETWEEN | 在某个范围内 |
| LIKE    | 搜索某种模式 |



#### AND 和 OR 运算符

AND 和 OR 可在 WHERE 子语句中把两个或多个条件结合起来。

如果第一个条件和第二个条件都成立，则 AND 运算符显示一条记录。

如果第一个条件和第二个条件中只要有一个成立，则 OR 运算符显示一条记录。



#### ORDER BY 语句

**ORDER BY 语句用于对结果集进行排序。**

ORDER BY 语句用于根据指定的列对结果集进行排序。

ORDER BY 语句默认按照升序对记录进行排序。

如果您希望按照降序对记录进行排序，可以使用 DESC 关键字。

```sql
SELECT Company, OrderNumber FROM Orders ORDER BY Company
```

```sql
SELECT Company, OrderNumber FROM Orders ORDER BY Company, OrderNumber
```

```sql
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC
```



#### INSERT INTO 语句

INSERT INTO 语句用于向表格中插入新的行。

##### 语法

```sql
INSERT INTO 表名称 VALUES (值1, 值2,....)
```

我们也可以指定所要插入数据的列：

```sql
INSERT INTO 表名称 (列1, 列2,...) VALUES (值1, 值2,....)
```



#### Update 语句

Update 语句用于修改表中的数据。

##### 语法：

```sql
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```

| LastName | FirstName | Address        | City    |
| :------- | :-------- | :------------- | :------ |
| Gates    | Bill      | Xuanwumen 10   | Beijing |
| Wilson   |           | Champs-Elysees |         |

##### 更新某一行中的一个列

我们为 lastname 是 "Wilson" 的人添加 firstname：

```sql
UPDATE Person SET FirstName = 'Fred' WHERE LastName = 'Wilson' 
```



#### DELETE 语句

DELETE 语句用于删除表中的行。

##### 语法

```sql
DELETE FROM 表名称 WHERE 列名称 = 值
```



#### TOP 子句

TOP 子句用于规定要返回的记录的数目。

对于拥有数千条记录的大型表来说，TOP 子句是非常有用的。

**注释：**并非所有的数据库系统都支持 TOP 子句。

##### SQL Server 的语法：

```sql
SELECT TOP number|percent 列名称
FROM 表名称
```

##### MySQL 和 Oracle 中的 SQL SELECT TOP 是等价的

##### MySQL 语法

```sql
SELECT 列名称
FROM 表名称
LIMIT 数字
```

##### Oracle 语法

```sql
SELECT 列名称
FROM 表名称
WHERE ROWNUM <= number
```



#### LIKE 操作符

**LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。**

LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。

##### SQL LIKE 操作符语法

```sql
SELECT 列名称
FROM 表名称
WHERE 列名称 LIKE pattern
```

##### 例子 1

现在，我们希望从上面的 "Persons" 表中选取居住在以 "N" 开始的城市里的人：

我们可以使用下面的 SELECT 语句：

```sql
SELECT * FROM Persons
WHERE City LIKE 'N%'
```

**提示：**"%" 可用于定义通配符（模式中缺少的字母）。

##### 例子 2

接下来，我们希望从 "Persons" 表中选取居住在以 "g" 结尾的城市里的人：

我们可以使用下面的 SELECT 语句：

```sql
SELECT * FROM Persons
WHERE City LIKE '%g'
```

接下来，我们希望从 "Persons" 表中选取居住在包含 "lon" 的城市里的人：

我们可以使用下面的 SELECT 语句：

```sql
SELECT * FROM Persons
WHERE City LIKE '%lon%'
```

##### 例子 4

通过使用 NOT 关键字，我们可以从 "Persons" 表中选取居住在*不包含* "lon" 的城市里的人：

我们可以使用下面的 SELECT 语句：

```sql
SELECT * FROM Persons
WHERE City NOT LIKE '%lon%'
```



#### SQL 通配符

##### **在搜索数据库中的数据时，您可以使用 SQL 通配符。**

在搜索数据库中的数据时，SQL 通配符可以替代一个或多字符。

SQL 通配符必须与 LIKE 运算符一起使用。

在 SQL 中，可使用以下通配符：

| 通配符                       | 描述                   |
| :--------------------------- | :--------------------- |
| %                            | 代表零个或多个字符     |
| _                            | 仅替代一个字符         |
| [charlist]                   | 字符列中的任何单一字符 |
| [^charlist] 或者 [!charlist] | 不在字符列中的任何     |



#### IN 操作符

IN 操作符允许我们在 WHERE 子句中规定多个值。

##### SQL IN 语法

```sql
SELECT 列名称
FROM 表名称
WHERE 列名称 IN (value1,value2,...)
```

现在，我们希望从上表中选取姓氏为 Adams 和 Carter 的人：

我们可以使用下面的 SELECT 语句：

```
SELECT * FROM Persons
WHERE LastName IN ('Adams','Carter')
```



#### BETWEEN 操作符

##### **BETWEEN 操作符在 WHERE 子句中使用，作用是选取介于两个值之间的数据范围。**

操作符 BETWEEN ... AND 会选取介于两个值之间的数据范围。这些值可以是数值、文本或者日期。

##### SQL BETWEEN 语法

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name
BETWEEN value1 AND value2
```



#### SQL Alias

**通过使用 SQL，可以为*列名称和表名称指定别名*（Alias）。**

##### **表的 SQL** 语法

```sql
SELECT column_name(s)
FROM table_name
AS alias_name
```

##### 列的 SQL Alias 语法

```sql
SELECT column_name AS alias_name
FROM table_name
```



#### **SQL JOIN**

##### Join 和 Key

有时为了得到完整的结果，我们需要从两个或更多的表中获取结果。我们就需要执行 join。

数据库中的表可通过键将彼此联系起来。主键（Primary Key）是一个列，在这个列中的每一行的值都是唯一的。在表中，每个主键的值都是唯一的。这样做的目的是在不重复每个表中的所有数据的情况下，把表间的数据交叉捆绑在一起。

##### 引用两个表

我们可以通过引用两个表的方式，从两个表中获取数据：

谁订购了产品，并且他们订购了什么产品？

```sql
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons, Orders
WHERE Persons.Id_P = Orders.Id_P 
```



#### SQL INNER JOIN 关键字

在表中存在至少一个匹配时，INNER JOIN 关键字返回行。

**inner join 内部每行具有相同字段的不同表的数据放在同一行

**有匹配值的行才返回

##### INNER JOIN 关键字语法

```sql
SELECT column_name(s)
FROM table_name1
INNER JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
```

**注释：**INNER JOIN 与 JOIN 是相同的。



#### SQL LEFT JOIN 关键字

LEFT JOIN 关键字会从左表 (table_name1) 那里返回所有的行，即使在右表 (table_name2) 中没有匹配的行。



##### LEFT JOIN 关键字语法

```sql
SELECT column_name(s)
FROM table_name1
LEFT JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
```

**注释：**在某些数据库中， LEFT JOIN 称为 LEFT OUTER JOIN。



#### SQL RIGHT JOIN 关键字

RIGHT JOIN 关键字会右表 (table_name2) 那里返回所有的行，即使在左表 (table_name1) 中没有匹配的行。

##### RIGHT JOIN 关键字语法

```sql
SELECT column_name(s)
FROM table_name1
RIGHT JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
```

**注释：**在某些数据库中， RIGHT JOIN 称为 RIGHT OUTER JOIN。



#### SQL FULL JOIN 关键字

只要其中某个表存在匹配，FULL JOIN 关键字就会返回行。

##### FULL JOIN 关键字语法

```sql
SELECT column_name(s)
FROM table_name1
FULL JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
```

**注释：**在某些数据库中， FULL JOIN 称为 FULL OUTER JOIN。



#### SQL UNION 操作符

##### UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

​	请注意，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。

##### SQL UNION 语法

```sql
SELECT column_name(s) FROM table_name1
UNION
SELECT column_name(s) FROM table_name2
```

**注释：**默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。

##### SQL UNION ALL 语法

```sql
SELECT column_name(s) FROM table_name1
UNION ALL
SELECT column_name(s) FROM table_name2
```

另外，UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。



#### SELECT INTO 语句

##### **SQL SELECT INTO 语句可用于创建表的备份复件。**

SELECT INTO 语句从一个表中选取数据，然后把数据插入另一个表中。

SELECT INTO 语句常用于创建表的备份复件或者用于对记录进行存档。

##### SQL SELECT INTO 语法

您可以把所有的列插入新表：

```sql
SELECT *
INTO new_table_name [IN externaldatabase] 
FROM old_tablename
```

或者只把希望的列插入新表：

```sql
SELECT column_name(s)
INTO new_table_name [IN externaldatabase] 
FROM old_tablename
```



#### CREATE DATABASE 语句

CREATE DATABASE 用于创建数据库。

##### SQL CREATE DATABASE 语法

```sql
CREATE DATABASE database_name
```



#### CREATE TABLE 语句

CREATE TABLE 语句用于创建数据库中的表。

##### SQL CREATE TABLE 语法

```sql
CREATE TABLE 表名称
(
列名称1 数据类型,
列名称2 数据类型,
列名称3 数据类型,
....
)
```

数据类型（data_type）规定了列可容纳何种数据类型。下面的表格包含了SQL中最常用的数据类型：

| 数据类型                                             | 描述                                                         |
| :--------------------------------------------------- | :----------------------------------------------------------- |
| integer(size) int(size) smallint(size) tinyint(size) | 仅容纳整数。在括号内规定数字的最大位数。                     |
| decimal(size,d) numeric(size,d)                      | 容纳带有小数的数字。 "size" 规定数字的最大位数。"d" 规定小数点右侧的最大位数。 |
| char(size)                                           | 容纳固定长度的字符串（可容纳字母、数字以及特殊字符）。 在括号中规定字符串的长度。 |
| varchar(size)                                        | 容纳可变长度的字符串（可容纳字母、数字以及特殊的字符）。 在括号中规定字符串的最大长度。 |
| date(yyyymmdd)                                       | 容纳日期。                                                   |



#### SQL 约束

约束用于限制加入表的数据的类型。

可以在创建表时规定约束（通过 CREATE TABLE 语句），或者在表创建之后也可以（通过 ALTER TABLE 语句）。

我们将主要探讨以下几种约束：

- **NOT** **NULL**

  不接受NULL值

  ```sql
  CREATE TABLE 表名称
  (
  列名称1 数据类型, NOT NULL 
  列名称2 数据类型,
  列名称3 数据类型,
  ....
  )
  ```

- **UNIQUE**

  UNIQUE 约束唯一标识数据库表中的每条记录。

  UNIQUE 和 PRIMARY KEY 约束均为列或列集合提供了唯一性的保证。

  PRIMARY KEY 拥有自动定义的 UNIQUE 约束。

  请注意，每个表可以有多个 UNIQUE 约束，但是每个表只能有一个 PRIMARY KEY 约束。

  ###### MySQL:

  ```sql
  CREATE TABLE Persons
  (
  Id_P int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  UNIQUE (Id_P)
  )
  ```

  ###### SQL Server / Oracle / MS Access:

  ```sql
  CREATE TABLE Persons
  (
  Id_P int NOT NULL UNIQUE,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255)
  )
  ```

  如果需要命名 UNIQUE 约束，以及为多个列定义 UNIQUE 约束，请使用下面的 SQL 语法：

  ###### MySQL / SQL Server / Oracle / MS Access:

  ```sql
  CREATE TABLE Persons
  (
  Id_P int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  CONSTRAINT uc_PersonID UNIQUE (Id_P,LastName)
  )
  ```

  ###### SQL UNIQUE Constraint on ALTER TABLE

  当表已被创建时，如需在 "Id_P" 列创建 UNIQUE 约束，请使用下列 SQL：

  ###### MySQL / SQL Server / Oracle / MS Access:

  ```sql
  ALTER TABLE Persons
  ADD UNIQUE (Id_P)
  ```

  如需命名 UNIQUE 约束，并定义多个列的 UNIQUE 约束，请使用下面的 SQL 语法：

  ###### MySQL / SQL Server / Oracle / MS Access:

  ```sql
  ALTER TABLE Persons
  ADD CONSTRAINT uc_PersonID UNIQUE (Id_P,LastName)
  ```

  ###### 撤销 UNIQUE 约束

  如需撤销 UNIQUE 约束，请使用下面的 SQL：

  ###### MySQL:

  ```sql
  ALTER TABLE Persons
  DROP INDEX uc_PersonID
  ```

  ###### SQL Server / Oracle / MS Access:

  ```sql
  ALTER TABLE Persons
  DROP CONSTRAINT uc_PersonI
  ```

- **PRIMARY KEY**

  PRIMARY KEY 约束唯一标识数据库表中的每条记录。

  主键必须包含唯一的值。

  主键列不能包含 NULL 值。

  每个表都应该有一个主键，并且每个表只能有一个主键。

  ###### MySQL:

  ```sql
  CREATE TABLE Persons
  (
  Id_P int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  PRIMARY KEY (Id_P)
  )
  ```

  ###### SQL Server / Oracle / MS Access:

  ```sql
  CREATE TABLE Persons
  (
  Id_P int NOT NULL PRIMARY KEY,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255)
  )
  ```

  如果需要命名 PRIMARY KEY 约束，以及为多个列定义 PRIMARY KEY 约束，请使用下面的 SQL 语法：

  ###### MySQL / SQL Server / Oracle / MS Access:

  ```sql
  CREATE TABLE Persons
  (
  Id_P int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  CONSTRAINT pk_PersonID PRIMARY KEY (Id_P,LastName)
  )
  ```

  ###### SQL PRIMARY KEY Constraint on ALTER TABLE

  如果在表已存在的情况下为 "Id_P" 列创建 PRIMARY KEY 约束，请使用下面的 SQL：

  ###### MySQL / SQL Server / Oracle / MS Access:

  ```sql
  ALTER TABLE Persons
  ADD PRIMARY KEY (Id_P)
  ```

  如果需要命名 PRIMARY KEY 约束，以及为多个列定义 PRIMARY KEY 约束，请使用下面的 SQL 语法：

  ###### MySQL / SQL Server / Oracle / MS Access:

  ```sql
  ALTER TABLE Persons
  ADD CONSTRAINT pk_PersonID PRIMARY KEY (Id_P,LastName)
  ```

  **注释：**如果您使用 ALTER TABLE 语句添加主键，必须把主键列声明为不包含 NULL 值（在表首次创建时）。

  ###### 撤销 PRIMARY KEY 约束

  如需撤销 PRIMARY KEY 约束，请使用下面的 SQL：

  ###### MySQL:

  ```sql
  ALTER TABLE Persons
  DROP PRIMARY KEY
  ```

  ###### SQL Server / Oracle / MS Access:

  ```sql
  ALTER TABLE Persons
  DROP CONSTRAINT pk_PersonID
  ```

- **FOREIGN KEY**

  一个表中的 FOREIGN KEY 指向另一个表中的 PRIMARY KEY。

  FOREIGN KEY 约束用于预防破坏表之间连接的动作。

  FOREIGN KEY 约束也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一。

  ###### MySQL:

  ```sql
  CREATE TABLE Orders
  (
  Id_O int NOT NULL,
  OrderNo int NOT NULL,
  Id_P int,
  PRIMARY KEY (Id_O),
  FOREIGN KEY (Id_P) REFERENCES Persons(Id_P)
  )
  ```

  ###### SQL Server / Oracle / MS Access:

  ```sql
  CREATE TABLE Orders
  (
  Id_O int NOT NULL PRIMARY KEY,
  OrderNo int NOT NULL,
  Id_P int FOREIGN KEY REFERENCES Persons(Id_P)
  )
  ```

  如果需要命名 FOREIGN KEY 约束，以及为多个列定义 FOREIGN KEY 约束，请使用下面的 SQL 语法：

  ###### MySQL / SQL Server / Oracle / MS Access:

  ```sql
  CREATE TABLE Orders
  (
  Id_O int NOT NULL,
  OrderNo int NOT NULL,
  Id_P int,
  PRIMARY KEY (Id_O),
  CONSTRAINT fk_PerOrders FOREIGN KEY (Id_P)
  REFERENCES Persons(Id_P)
  )
  ```

  ###### SQL FOREIGN KEY Constraint on ALTER TABLE

  如果在 "Orders" 表已存在的情况下为 "Id_P" 列创建 FOREIGN KEY 约束，请使用下面的 SQL：

  ###### MySQL / SQL Server / Oracle / MS Access:

  ```sql
  ALTER TABLE Orders
  ADD FOREIGN KEY (Id_P)
  REFERENCES Persons(Id_P)
  ```

  如果需要命名 FOREIGN KEY 约束，以及为多个列定义 FOREIGN KEY 约束，请使用下面的 SQL 语法：

  ###### MySQL / SQL Server / Oracle / MS Access:

  ```sql
  ALTER TABLE Orders
  ADD CONSTRAINT fk_PerOrders
  FOREIGN KEY (Id_P)
  REFERENCES Persons(Id_P)
  ```

  ###### 撤销 FOREIGN KEY 约束

  如需撤销 FOREIGN KEY 约束，请使用下面的 SQL：

  ###### MySQL:

  ```sql
  ALTER TABLE Orders
  DROP FOREIGN KEY fk_PerOrders
  ```

  ###### SQL Server / Oracle / MS Access:

  ```sql
  ALTER TABLE Orders
  DROP CONSTRAINT fk_PerOrders
  ```

- **CHECK**

  CHECK 约束用于限制列中的值的范围。

  如果对单个列定义 CHECK 约束，那么该列只允许特定的值。

  如果对一个表定义 CHECK 约束，那么此约束会在特定的列中对值进行限制。

  ###### My SQL:

  ```
  CREATE TABLE Persons
  (
  Id_P int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  CHECK (Id_P>0)
  )
  ```

  ###### SQL Server / Oracle / MS Access:

  ```
  CREATE TABLE Persons
  (
  Id_P int NOT NULL CHECK (Id_P>0),
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255)
  )
  ```

  如果需要命名 CHECK 约束，以及为多个列定义 CHECK 约束，请使用下面的 SQL 语法：

  ###### MySQL / SQL Server / Oracle / MS Access:

  ```
  CREATE TABLE Persons
  (
  Id_P int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  CONSTRAINT chk_Person CHECK (Id_P>0 AND City='Sandnes')
  )
  ```

  ###### SQL CHECK Constraint on ALTER TABLE

  如果在表已存在的情况下为 "Id_P" 列创建 CHECK 约束，请使用下面的 SQL：

  ###### MySQL / SQL Server / Oracle / MS Access:

  ```
  ALTER TABLE Persons
  ADD CHECK (Id_P>0)
  ```

  如果需要命名 CHECK 约束，以及为多个列定义 CHECK 约束，请使用下面的 SQL 语法：

  ###### MySQL / SQL Server / Oracle / MS Access:

  ```
  ALTER TABLE Persons
  ADD CONSTRAINT chk_Person CHECK (Id_P>0 AND City='Sandnes')
  ```

  ###### 撤销 CHECK 约束

  如需撤销 CHECK 约束，请使用下面的 SQL：

  ###### SQL Server / Oracle / MS Access:

  ```
  ALTER TABLE Persons
  DROP CONSTRAINT chk_Person
  ```

  ###### MySQL:

  ```
  ALTER TABLE Persons
  DROP CHECK chk_Person
  ```

- **DEFAULT**

  DEFAULT 约束用于向列中插入默认值。

  如果没有规定其他的值，那么会将默认值添加到所有的新记录。

  ###### SQL DEFAULT Constraint on CREATE TABLE

  下面的 SQL 在 "Persons" 表创建时为 "City" 列创建 DEFAULT 约束：

  ###### My SQL / SQL Server / Oracle / MS Access:

  ```
  CREATE TABLE Persons
  (
  Id_P int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255) DEFAULT 'Sandnes'
  )
  ```

  通过使用类似 GETDATE() 这样的函数，DEFAULT 约束也可以用于插入系统值：

  ```
  CREATE TABLE Orders
  (
  Id_O int NOT NULL,
  OrderNo int NOT NULL,
  Id_P int,
  OrderDate date DEFAULT GETDATE()
  )
  ```

  ###### SQL DEFAULT Constraint on ALTER TABLE

  如果在表已存在的情况下为 "City" 列创建 DEFAULT 约束，请使用下面的 SQL：

  ###### MySQL:

  ```
  ALTER TABLE Persons
  ALTER City SET DEFAULT 'SANDNES'
  ```

  ###### SQL Server / Oracle / MS Access:

  ```
  ALTER TABLE Persons
  ALTER COLUMN City SET DEFAULT 'SANDNES'
  ```

  ###### 撤销 DEFAULT 约束

  如需撤销 DEFAULT 约束，请使用下面的 SQL：

  ###### MySQL:

  ```
  ALTER TABLE Persons
  ALTER City DROP DEFAULT
  ```

  ###### SQL Server / Oracle / MS Access:

  ```
  ALTER TABLE Persons
  ALTER COLUMN City DROP DEFAULT
  ```



#### ALTER TABLE 语句

##### ALTER TABLE 语句用于在已有的表中添加、修改或删除列。

##### SQL ALTER TABLE 语法

如需在表中添加列，请使用下列语法:

```
ALTER TABLE table_name
ADD column_name datatype
```

要删除表中的列，请使用下列语法：

```
ALTER TABLE table_name 
DROP COLUMN column_name
```

**注释：**某些数据库系统不允许这种在数据库表中删除列的方式 (DROP COLUMN column_name)。

要改变表中列的数据类型，请使用下列语法：

```
ALTER TABLE table_name
ALTER COLUMN column_name datatype
```



#### SQL 日期

当我们处理日期时，最难的任务恐怕是确保所插入的日期的格式，与数据库中日期列的格式相匹配。

只要数据包含的只是日期部分，运行查询就不会出问题。但是，如果涉及时间，情况就有点复杂了。

在讨论日期查询的复杂性之前，我们先来看看最重要的内建日期处理函数。

##### MySQL Date 函数

下面的表格列出了 MySQL 中最重要的内建日期函数：

| 函数                                                         | 描述                                |
| :----------------------------------------------------------- | :---------------------------------- |
| [NOW()](https://www.w3school.com.cn/sql/func_now.asp)        | 返回当前的日期和时间                |
| [CURDATE()](https://www.w3school.com.cn/sql/func_curdate.asp) | 返回当前的日期                      |
| [CURTIME()](https://www.w3school.com.cn/sql/func_curtime.asp) | 返回当前的时间                      |
| [DATE()](https://www.w3school.com.cn/sql/func_date.asp)      | 提取日期或日期/时间表达式的日期部分 |
| [EXTRACT()](https://www.w3school.com.cn/sql/func_extract.asp) | 返回日期/时间按的单独部分           |
| [DATE_ADD()](https://www.w3school.com.cn/sql/func_date_add.asp) | 给日期添加指定的时间间隔            |
| [DATE_SUB()](https://www.w3school.com.cn/sql/func_date_sub.asp) | 从日期减去指定的时间间隔            |
| [DATEDIFF()](https://www.w3school.com.cn/sql/func_datediff_mysql.asp) | 返回两个日期之间的天数              |
| [DATE_FORMAT()](https://www.w3school.com.cn/sql/func_date_format.asp) | 用不同的格式显示日期/时间           |

##### SQL Server Date 函数

下面的表格列出了 SQL Server 中最重要的内建日期函数：

| 函数                                                         | 描述                             |
| :----------------------------------------------------------- | :------------------------------- |
| [GETDATE()](https://www.w3school.com.cn/sql/func_getdate.asp) | 返回当前日期和时间               |
| [DATEPART()](https://www.w3school.com.cn/sql/func_datepart.asp) | 返回日期/时间的单独部分          |
| [DATEADD()](https://www.w3school.com.cn/sql/func_dateadd.asp) | 在日期中添加或减去指定的时间间隔 |
| [DATEDIFF()](https://www.w3school.com.cn/sql/func_datediff.asp) | 返回两个日期之间的时间           |
| [CONVERT()](https://www.w3school.com.cn/sql/func_convert.asp) | 用不同的格式显示日期/时间        |

##### SQL Date 数据类型

MySQL 使用下列数据类型在数据库中存储日期或日期/时间值：

- DATE - 格式 YYYY-MM-DD
- DATETIME - 格式: YYYY-MM-DD HH:MM:SS
- TIMESTAMP - 格式: YYYY-MM-DD HH:MM:SS
- YEAR - 格式 YYYY 或 YY

SQL Server 使用下列数据类型在数据库中存储日期或日期/时间值：

- DATE - 格式 YYYY-MM-DD
- DATETIME - 格式: YYYY-MM-DD HH:MM:SS
- SMALLDATETIME - 格式: YYYY-MM-DD HH:MM:SS
- TIMESTAMP - 格式: 唯一的数字



#### MySQL 数据类型

在 MySQL 中，有三种主要的类型：文本、数字和日期/时间类型。

##### Text 类型：

| 数据类型         | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| CHAR(size)       | 保存固定长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的长度。最多 255 个字符。 |
| VARCHAR(size)    | 保存可变长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的最大长度。最多 255 个字符。 注释：如果值的长度大于 255，则被转换为 TEXT 类型。 |
| TINYTEXT         | 存放最大长度为 255 个字符的字符串。                          |
| TEXT             | 存放最大长度为 65,535 个字符的字符串。                       |
| BLOB             | 用于 BLOBs (Binary Large OBjects)。存放最多 65,535 字节的数据。 |
| MEDIUMTEXT       | 存放最大长度为 16,777,215 个字符的字符串。                   |
| MEDIUMBLOB       | 用于 BLOBs (Binary Large OBjects)。存放最多 16,777,215 字节的数据。 |
| LONGTEXT         | 存放最大长度为 4,294,967,295 个字符的字符串。                |
| LONGBLOB         | 用于 BLOBs (Binary Large OBjects)。存放最多 4,294,967,295 字节的数据。 |
| ENUM(x,y,z,etc.) | 允许你输入可能值的列表。可以在 ENUM 列表中列出最大 65535 个值。如果列表中不存在插入的值，则插入空值。 注释：这些值是按照你输入的顺序存储的。 可以按照此格式输入可能的值：ENUM('X','Y','Z') |
| SET              | 与 ENUM 类似，SET 最多只能包含 64 个列表项，不过 SET 可存储一个以上的值。 |

##### Number 类型：

| 数据类型        | 描述                                                         |
| :-------------- | :----------------------------------------------------------- |
| TINYINT(size)   | -128 到 127 常规。0 到 255 无符号*。在括号中规定最大位数。   |
| SMALLINT(size)  | -32768 到 32767 常规。0 到 65535 无符号*。在括号中规定最大位数。 |
| MEDIUMINT(size) | -8388608 到 8388607 普通。0 to 16777215 无符号*。在括号中规定最大位数。 |
| INT(size)       | -2147483648 到 2147483647 常规。0 到 4294967295 无符号*。在括号中规定最大位数。 |
| BIGINT(size)    | -9223372036854775808 到 9223372036854775807 常规。0 到 18446744073709551615 无符号*。在括号中规定最大位数。 |
| FLOAT(size,d)   | 带有浮动小数点的小数字。在括号中规定最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| DOUBLE(size,d)  | 带有浮动小数点的大数字。在括号中规定最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| DECIMAL(size,d) | 作为字符串存储的 DOUBLE 类型，允许固定的小数点。             |

\* 这些整数类型拥有额外的选项 UNSIGNED。通常，整数可以是负数或正数。如果添加 UNSIGNED 属性，那么范围将从 0 开始，而不是某个负数。

##### Date 类型：

| 数据类型    | 描述                                                         |
| :---------- | :----------------------------------------------------------- |
| DATE()      | 日期。格式：YYYY-MM-DD 注释：支持的范围是从 '1000-01-01' 到 '9999-12-31' |
| DATETIME()  | *日期和时间的组合。格式：YYYY-MM-DD HH:MM:SS 注释：支持的范围是从 '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59' |
| TIMESTAMP() | *时间戳。TIMESTAMP 值使用 Unix 纪元('1970-01-01 00:00:00' UTC) 至今的描述来存储。格式：YYYY-MM-DD HH:MM:SS 注释：支持的范围是从 '1970-01-01 00:00:01' UTC 到 '2038-01-09 03:14:07' UTC |
| TIME()      | 时间。格式：HH:MM:SS 注释：支持的范围是从 '-838:59:59' 到 '838:59:59' |
| YEAR()      | 2 位或 4 位格式的年。 注释：4 位格式所允许的值：1901 到 2155。2 位格式所允许的值：70 到 69，表示从 1970 到 2069。 |

\* 即便 DATETIME 和 TIMESTAMP 返回相同的格式，它们的工作方式很不同。在 INSERT 或 UPDATE 查询中，TIMESTAMP 自动把自身设置为当前的日期和时间。TIMESTAMP 也接受不同的格式，比如 YYYYMMDDHHMMSS、YYMMDDHHMMSS、YYYYMMDD 或 YYMMDD。
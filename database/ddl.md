# SQL语句
DDL(data definition language)

Forewords:

- SQL语句大小写均可识别，约定大写
- SQL语句中`schema<==>database`
- SQL语句中尖括号必填，方括号选填
- `$`代表需要修改的变量

## 集合关系
table&sube;database&sube;databases  

## 语句操作

**下面操作均围绕增删改查**

### DATABASE

#### 创建(CREATE)

```sql
CREATE DATABASE <$DATABASE_NAME> [default charset = <$encoding>];
```

#### 删除(DROP)

```sql
DROP DATABASE <$DATABASE_NAME>;
```

#### 修改(ALTER)

```sql
-- 更改字符集
alter database <$database_name> character set <$character_set> [collate <$collation_name>]
-- 数据库无法修改名称, 需要重新创建表
```

#### 展示(SHOW)

```sql
SHOW DATABASES;
```

### TABLE
表操作需要指定数据库:  
```sql
USE <$DATABASE_NAME>;
```

#### 创建(CREATE)
```sql
CREATE TABLE <$TABLE_NAME> (
<$ATTRIBUTE> <$DATA_TYPE> [$COL_INTEGRITY_CONSTRAINTS],
<$ATTRIBUTE> <$DATA_TYPE> [$COL_INTEGRITY_CONSTRAINTS],
...,
[<$TAB_INTEGRITY_CONSTRAINTS> (`<$ATTRIBUTE>`) ] 
) [ENGINE=InnoDB DEFAULT CHARSET=utf8];
```

括号内的数据是属性、数据类型、列级完整性约束条件，以及表级完整性约束条件按照指定规律和顺序组合而成  
数据类型查询：[SQL DB数据类型-菜鸟教程](https://www.runoob.com/sql/sql-datatypes.html)

完整性约束条件类型：
```
NOT NULL
UNIQUE
PRIMARY KEY
FOREIGN KEY
CHECK
DEFAULT
```

示例：
```sql
CREATE TABLE User(
	id INT(10) PRIMARY KEY,
	name VARCHAR(32),
	email VARCHAR(128),
	UNIQUE(email)
);
```
#### 删除(DROP)
```sql
DROP TABLE <$TABLE_NAME>;
```

删除主键完整性约束为  
*完整性约束为 null 表示删除当前除主键外的完整性约束*
```sql
drop constraints primary key
```
#### 修改(ALTER)
```sql
ALTER TABLE <$TABLE_NAME>
[ADD [COLUMN] <$ATTRIBUTE> <$DATA_TYPE> [$INTEGRITY_CONSTRAINTS]]
[DROP <$ATTRIBUTE> <$DATA_TYPE> [$INTEGRITY_CONSTRAINTS]]
[MODIFY [COLUMN] <$ATTRIBUTE> <$NEW_DATA_TYPE>]
[CHANGE [COLUMN] <$ATTRIBUTE> <$NEW_ATTRIBUTE> <$NEW_DATA_TYPE>]
[RENAME TO <$NEW_TAB_NAME>]
[convert to character set <$character_set> collate <$collation_name>]
```


How do I show unique constraints of a table in MySQL?
```sql
-- show unique constraints of a table
SELECT DISTINCT CONSTRAINT_NAME FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS WHERE TABLE_NAME = ’yourTableName’ AND CONSTRAINT_TYPE = ’UNIQUE’;
```

#### 展示(SHOW)
```sql
SHOW TABLES;
```



### CONTENT
#### 向表内添加数据记录
```sql
INSERT INTO <$TABLE_NAME> [(<$ATTRIBUTE>,<$ATTRIBUTE>,...)] VALUES ('$VALUE','$VALUE',...);
```

示例:  
```sql
--向User表中添加属性
INSERT INTO User VALUES ('001','Bob','Bob@163.com');
```

#### 删除表内数据记录
`DELETE FROM <$TABLE_NAME> WHERE <$ATTRIBUTE>='<$ATTRIBUTE_ELEMENT>';`  
示例：  
```
--删除User表中name属性是Bob的整条记录
DELETE FROM User WHERE name='Bob';
```

#### 修改表内数据

```sql
UPDATE <$TAB_NAME> SET <$ATTRIBUTE>='<$ATTRIBUTE_ELEMENT>' WHERE <$ATTRIBUTE>='<$ATTRIBUTE_ELEMENT>';
```

示例：
```sql
--修改User表中数据，将name属性为Bob的记录中的name属性修改为Alice
UPDATE User SET name='Alice' WHERE name='Bob';
```

#### 查看表结构

```sql
DESCRIBE <$TABLE_NAME>;
```
简写：  
```sql
DESC <$TABLE_NAME>;
```
示例：  
```
DESC User;
```
输出结果：  
```
+-------+--------------+------+-----+---------+-------+
| Field | Type         | Null | Key | Default | Extra |
+-------+--------------+------+-----+---------+-------+
| id    | int(10)      | NO   | PRI | NULL    |       |
| name  | varchar(32)  | YES  |     | NULL    |       |
| email | varchar(128) | YES  | UNI | NULL    |       |
+-------+--------------+------+-----+---------+-------+
```

#### 查看表中数据
```sql
SELECT <$ATTRIBUTE> FROM <$TABLE_NAME>;
```
示例：

```sql
--从User表中读取所有属性
SELECT * FROM User;
```
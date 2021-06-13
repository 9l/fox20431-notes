
## 导出

### 导出库

```sh
mysqldump -u 用户名 -p 数据库名 > 导出的文件名
```
e.g.:
```sh
mysqldump -u root -p db_name > test_db.sql
```

### 导出表

```sh
mysqldump -u 用户名 -p 数据库名 表名> 导出的文件名
```
e.g.:
```sh
mysqldump -u root -p test_db users> test_users.sql （结尾没有分号）
```

## 导入

### SQL命令导入

```sql
source 路径
```

e.g.
```sql
source C:\Users\Administrator\Desktop\users2.sql
```

### 命令行导入

```
mysql -u用户名 -p 数据库名 < sql文件
```

note that: 要求存在数据库才可以导入
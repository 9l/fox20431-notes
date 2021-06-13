# 编码格式

## 查看编码
## 查看数据库编码

`show variables like 'character_set_database';`
`show create database <$database_name>`
## 查看编码
`show create table <$table_name>`

## 修改编码

### 修改全局编码

```sql
SET GLOBAL character_set_client     = utf8;
SET GLOBAL character_set_connection = utf8;
SET GLOBAL character_set_database   = utf8;
SET GLOBAL character_set_results    = utf8;
SET GLOBAL character_set_server     = utf8;
```

### 修改数据库的编码格式

`alter database <$database_name> CHARACTER SET utf8;`
### 修改表的编码格式
`alter table <$table_name> CONVERT TO CHARACTER SET utf8;`

## 修改默认编码格式
需要从配置文件修改  
找到对应配置文件添加：`default-character-set=utf8`;


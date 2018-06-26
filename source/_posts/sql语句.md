---
title: sql语句
date: 2018-06-26 20:45:41
tags: sql 数据库 database
categories: 服务器
---
## sql语句

### 1 查询
#### 1.1 SELECT
查询语句
```sql
SELECT * FROM users WHERE name='changzhn'
```
-> 查询　列名(*代表所有列)　从　表名　条件　列名　等于　值

显示结果

pk | name
---|---
123 | changzhn

如果改成
```sql
SELECT pk FROM users WHERE name='changzhn'
```
显示　列名为pk的列　从　表名　条件　等于　值
显示结果
pk |
---|
123 |

如果只想显示某几列
```sql
SELECT pk, firstName FROM users WHERE name='changzhn'
```

- 语句对大小写敏感，使用`select`和`SELECT`效果一样
- 对查询内容大小写也不敏感，查询`changzhn`和`Changzhn`效果一样
- 使用单引号(也有数据库支持双引号)

#### 1.2 SELECT DISTINCT
```sql
SELECT DISTINCT name FROM users
```
查询　列　不重复值

#### 1.3 SELECT AND/OR
```sql
SELECT * FROM Persons WHERE FirstName='Thomas' AND LastName='Carter'
SELECT * FROM Persons WHERE firstname='Thomas' OR lastname='Carter'
```
AND和OR也可以结合使用

#### 1.4 SELECT ORDER
```sql
SELECT Company, OrderNumber FROM Orders ORDER BY Company
SELECT Company, OrderNumber FROM Orders ORDER BY Company, OrderNumber
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC, OrderNumber ASC
```
- 查询以公司名称排序
- 查询以公司名称排序，公司名称一样再以orderNumber排序
- 查询以公司名称倒序排列
- 查询以公司名称倒序排列，再以orderNumber正序排序

### 2 插入
#### 2.1 INSERT
```sql
INSERT INTO users VALUES (125, 'joo')
```
插入　在　表名　值　注意顺序

```sql
INSERT INTO users (pk, name) VALUES (126, 'han')
```
在特定的列添加值

### 3 更新
#### 3.1 UPDATE
```sql
UPDATE 表名称 SET 列名称1 = 新值1, 列名称2 = 新值2 WHERE 列名称 = 某值
```
实例
```sql
UPDATE users SET sex = 0 WHERE name = 'joo'
```

### 4 删除
#### 4.1 DELETE
```sql
DELETE FROM 表名称 WHERE 列名称 = 值
```

实例
```sql
DELETE FROM users WHERE	pk = 126
```
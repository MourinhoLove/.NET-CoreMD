# SQL 语句

## 查询

### 1.Like

当你想要查询某人名字是B开头的时候

```sql
SELECT *
FROM 表名
WHERE Name LIKE 'b%'  
```

`%b%` 字符串包含B的

`%b` 字符串以b结尾的

### 2.REGEXP

正则

```sql
SELECT *
FROM customers
WHERE name REGEXP 'Davies'
```

找出所有名字中包含Davies的数据

### 3.IS NULL

```sql
SELECT *
FROM customers
WHERE phone IS NULL 

SELECT *
FROM customers
WHERE phone IS NOT NULL 
```

查询电话未null的顾客

查询电话不是空的顾客

### 4. LIMIT

```sql
SELECT *
FROM customers
LIMIT 3
```

获取前3条记录

### 5.AS

```sql
SELECT name AS new name
FROM customers
```

为name列名 取一个新的名称

### 6.DISTINCT 

```sql
SELECT DISTINCT 列名称 FROM 表名称
```

去除重复项

### 7.In

IN 操作符允许我们在 WHERE 子句中规定多个值。

```sql
SELECT * FROM Persons
WHERE LastName IN ('Adams','Carter')
```

查询LastName 是 *Adams* 或者 *Carter*

### 8.BETWEEN 

**BETWEEN 操作符在 WHERE 子句中使用，作用是选取介于两个值之间的数据范围。**

```sql
SELECT * FROM Persons
WHERE LastName
BETWEEN 'Adams' AND 'Carter'
```

## 排序

### 1. ORDER BY

```sql
SELECT *
FROM customers
ORDER BY name
```

根据名称排序

- DESC

  `ORDER BY name DESC` 这会降序

- ORDER BY name, phone

  `ORDER BY name, phone` 先name排序后phone排序

## 多表查询

#### 1.内连接

```sql
SELECT p.machine_id, 
m.machine_name, 
p.time, 
r.raw_name
FROM production p
JOIN machine m 
ON p.machine_id = m.machine_id
JOIN raw r
ON p.raw_id = r.raw_id
```

多表主要使用的是`Join`关键字

`Join 表名 ON 表的连接条件`

一张表就连接一个`Join`

两张表就连接二个`Join`
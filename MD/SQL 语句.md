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
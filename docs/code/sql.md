# 数据库知识点  <!-- {docsify-ignore} -->

## 主键
唯一区别表中的每一行记录
## 外键
两个表需要互相关联时使用外键约束
## 索引
对某一列或多个列的值进行预排序的数据结构。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录。      
索引的优点是提高了查询效率，缺点是在插入、更新和删除记录时，需要同时修改索引，因此，索引越多，插入、更新和删除记录的速度就越慢。

## 事务
A：Atomic 原子性：将所有SQL作为原子工作单元执行，要么全部执行，要么全部不执行；

C：Consistent 一致性：事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户则必定加上了100；

I：Isolation 隔离性，如果有多个事务并发执行，每个事务作出的修改必须与其他事务隔离；

D：Duration 持久性，即事务完成后，对数据库数据的修改被持久化存储。

## 表之间的连接方式
inner join(内连接，等值连接）

left join(左连接）：结果集即包括连接表的匹配行，也包括左连接表的所有行。

right join(右连接）：结果集即包括连接表的匹配行，也包括右连接表的所有行。
```
SELECT a.runoob_id, a.runoob_author, b.runoob_count
FROM runoob_tbl a INNER JOIN tcount_tbl b
ON a.runoob_author = b.runoob_author;

SELECT a.runoob_id, a.runoob_author, b.runoob_count
FROM runoob_tbl a left JOIN tcount_tbl b
ON a.runoob_author = b.runoob_author;

SELECT a.runoob_id, a.runoob_author, b.runoob_count
FROM runoob_tbl a RIGHT JOIN tcount_tbl b
ON a.runoob_author = b.runoob_author;

```

## 查询
```
select [distinct] {where}
from table;
{where}
group by -- 将查询出的记录分组
having --对分组结果进行再次限定
order by --将查询出对记录进行排序
```
```
SELECT productvendor, count(*) FROM products 
GROUP BY productvendor
HAVING count(*) >= 9;
```
- 过滤数据
in 运算符来确定值是否匹配列表或子查询中的任何值。
```
SELECT 
    officeCode, city, phone
FROM
    offices
WHERE
    country NOT IN ('USA' , 'France');
```
```
SELECT 
    orderNumber, customerNumber, status, shippedDate
FROM
    orders
WHERE
    orderNumber IN (SELECT 
            orderNumber
        FROM
            orderDetails
        GROUP BY orderNumber
        HAVING SUM(quantityOrdered * priceEach) > 60000);
```
- 返回行数，用于分页

```
-- 返回第 3 ~ 5 行
SELECT * FROM mytable LIMIT 2, 3;
-- 返回4行，9表示从表的第十行开始
slect * from student limit 4 offset 9
```

limit 2,2  //第 3 -4 行。limit m,n ,游标从 m+1 到 m+n  ,n 是数目

- 条件查询 where

```
//找到不是在2000-2010年间year上映的电影
SELECT * FROM movies
WHERE Year 
NOT BETWEEN 2000 AND 2010;
```
- 查询结果 Filtering 过滤 和 sorting 排序（去重、最新、升序、降序）

```
//按导演名排重列出所有电影(只显示导演)，并按导演名正序排列
SELECT DISTINCT Director FROM Movies 
ORDER BY Director ASC;
```
```
//列出美国United States人口3-4位的两个城市和他们的人口(包括所有字段)
SELECT * FROM North_american_cities 
WHERE Country = 'United States'
ORDER BY Population DESC
LIMIT 2 OFFSET 2;
```
- 按 xx 则需分组,在查询中进行统计,常用函数有count,MAX,MIN,AVG,SUM

```
-- 按角色(Role)统计一下每个角色的平均就职年份,需要分组
SELECT Role,AVG(Years_employed) 
FROM Employees
GROUP BY Role;
```
根据两个条件查找两张表的三个字段，先看两张表能不能关联，否则通过第三张表外键关联
```
select b.vc,b.vframe,a.iname from tb_1 a.tb_2 b,tb_3 c

where a.po=b.po and b.vo=c.vo and b.brand='' and a.pc=''
```
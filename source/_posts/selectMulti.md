---
title: 数据查询之 多表查询
date: 2017-11-02 23:07:19 
tags: 
    - SQL
categories: [Back-End]
---
数据库系统概论课课后复习整理的笔记（二）。

## 自连接

- 方法：
  - 1.寻找包含所有所需信息的最小表集；
  - 2.（FROM）给各表记别名；
  - 3.（WHERE）
    1）连接相同字段； 2）选择的条件；
  - 4.（SELECT）选择要留下的信息。

e.g.

```SQL
​​​SELECT a.sno,sname--a,b为别名--
FROM SC a,Student as b--as可写可省略--
WHERE cno='2' and grade>60--选择的条件--
AND a.Sno=b.Sno--链接两个表相同字段​--
```
<!-- more -->

## 外连接

### 多标连接

- `inner join`:只保留匹配的行
- `left join`：连接，并保留左表全部行,空余的用NULL代替
- `right join`：连接，并保留右表全部行,空余的用NULL代替
- `full join`：连接，并保留所有行,空余的用NULL代替

e.g.

```SQL
SELECT sname,cno,grade
FROM student a INNER JOIN sc b 
ON a.sno = b.sno
WHERE sage<23​​
```

### 集合查询

多用于字段间的交并差关系。

- `union`-并:注意并相容

e.g.

```SQL
select * from Student where Sdept = 'CS'
union
​​select * from Student where Sage<20
```

- `union all`

e.g.

```SQL
select 1
union
select 2
union all
select 2--会选出122,即不去重
```

- `intersect`-交

e.g.

```SQL
select sno from Student where Sdept = 'CS'
intersect
​​select sno from Sc where cno=‘1’
```

- `except`-差

### 基于派生表查询

多用于连接经过聚集查询而产生的临时表。

e.g.

```SQL
select a.sno,cno from sc a ,
(select sno,avg(grade) avg_g from sc group by sno ) b--b为派生表，是临时产生的
where a.sno = b.sno
and a.grade>b.avg_g
```

### 跨库查询

`..`用于跳出当前库，进行跨库查询。

e.g.

```SQL
select *
from testdb..sc a,course b
where a.cno = b.cno
```
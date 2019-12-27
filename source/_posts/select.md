---
title: 数据查询之 单表查询
date: 2017-10-26 22:19:26 #文章生成时间，一般不改，当然也可以任意修改
tags: 
    - SQL
categories: [Back-End]
---
数据库系统概论课课后复习整理的笔记。

## 简单查询

```sql
select * from [table]
```

## 条件查询

```sql
select * from [table] where 条件
```

- 条件：
  - `=，>，<=`
  - `between ... and ...`
    e.g. `Sage (not) between 20 and 23;`（不）在20-23岁范围的Sage
  - `in('','')`
    e.g. `Sdept (not) in ('CS','MA');`（不）在cs或ma范围的Sdept
  - `like ''`
    e.g. `Sname like '刘%';`姓为刘、名任意的Sname

<!-- more -->
- 通配符：
  - `%`通配任意长度的字符;
  - `_`通配单个字符
    e.g. `where rtrim (Sname) not LIKE'刘__';` Sname不为刘某某
  - `[]`通配包含其中一个条件
    e.g. `where Sno like '%[0-3]'` Sno列的值含有0、1、2、3
e.g. `where rtrim(pwd) like '%[^0-9a-z]%'`pwd列的值包含了0-9或a-z之外的内容的，即包含了特殊字符的
e.g. `where rtrim(pwd) not like '%[0-9]%'`pwd列的值不包含0-9的内容的
e.g. `where rtrim(pwd) like '%[^0-9]%'`pwd列的值包含了非数字字符，即可以有数字，但是一定要有字母或符号
e.g. `where rtrim(pwd) not like '%[^0-9]%'`pwd列的值不包含了0-9之外的内容的，即没有一位非数字
- 转义字符
`where cname like '%/%%' escape '/'`以/为转义字符，其后的字符恢复原意
- 空值
`where cname is null`不可以用`=` 否则将匹配null这个字符串
- `order by`
e.g. `order by Ssex desc, Sage`Ssex降序,第二顺序Sage;等价于 `order by Ssex, -Sage`
`order by newid()`对每次查询都打乱
- `group by … having …`
e.g. `SELECT Ssex, avg(Sage*1.0) avg_age FROM Student group by Ssex`按性别统计平均年龄
e.g. `SELECT Sno, count(Grade) FROM Sc where Grade >=60 group by Sno having count(Grade)>=2​`获得至少2门课大于60分的学号

### 统计查询

- 聚集函数：
  - 不能用在`where`后面，只能在`select`和`having`后；
语句中如果既有聚合字段又有非聚合字段，必须使用group by，所有非聚合字段必须都出现在group by中。​group by 后面如有多个条件，则这些条件全部满足才是一组。
  - `sum()`
  - `avg()`
e.g. `select avg(grade) from sc` 等同于 `select sum(grade) count(grade) from sc`,用于算平均分
e.g. `select sum(grade)*1.0 count(grade) from sc`整型与浮点型
  - `max()`
  - `min()`
- MSSQL实用函数
  - `ltrim()`，`rtrim()`去除左右空格
  - `len()`
  - `substring()`,`left()`,`right()`
e.g. `left('12345678',3)`  结果为123,
`right('12345678',3)`  结果为789,
`substring('12345678',3,4)`  结果为3456
  - `charindex()`,`patindex()`
  - `getdate()`,`datepart()`,`convert()`
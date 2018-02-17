---
title: SQL练习总结
date: 2018-02-17 18:46:32
tags:
- SQL
categories:
- SQL
---

> 对于数据库，去年的时候对`mysql`过了一遍，但是感觉忘得差不多了。然后最近练习了一些比较简单的题目，写一篇博客来记录一下。我用的是`CodeWars`这个网站，这是我的[CodeWars地址](https://www.codewars.com/users/Scarborough_Coral)。练习的都是一些`5kyu`以下的，重点是想再复习一下语法和操作（4kyu的可能不会，没敢尝试！）。

### 0x01.8kyu-7kyu

> 8kyu-7kyu等级的题目是比较傻的，纯属是为了练习语法，但是还是要总结一下，以免再忘。

#### 0x01.1 函数

- SQRT(N) 对n求平方根
- CBRT(N) 对n求立方根
- LOG(N) 对n求以10为底的对数
- LN(N) 对n求自然对数
- TO_HEX(N) 将n转化为16进制
- SUM(N) 求列N的和
- ASCII(N) 对n求ASCII码
- POWER(N,M) n的m次方
- LPAD/RPAD(col,length,char) 将col列左/右侧添加char字符到length长度
- MOD(N,M) n对m求余
- MAX/MIN(COL) 求col列的最大/最小值

#### 0x01.2语法

- AS 
    - 给数据列起一个新别名
     ```sql
        SELECT column1 AS newcolumnname FROM table1;
     ```
    - 给表起一个新别名
     ```sql
        SELECT column1 FROM table1 AS newtablename; 
     ```
    - 计算结果起名
     ```sql
        SELECT max(column1) AS max_col1 FROM table1;
     ```
- WHERE 条件判断
 ```sql
    SELECT col1 FROM table1 WHERE id = 1;
 ```

- ORDER BY 将结果排序,默认正向
    - ASC 正向排序
    - DESC 反向排序
 ```sql
    SELECT * FROM table1 ORDER BY ID DESC;
 ```

### 0x02 6kyu-5kyu

> 5kyu、6kyu等级的也是偏基础的。就拿一个经典来说一下。可能我的solution比较外行，毕竟没熟悉多少。。但能ac

- SQL Basics - Monsters using CASE

`Description:`
`You have access to two tables named top_half and bottom_half, as follows:`

`top_half schema`

`id`
`heads`
`arms`
`bottom_half schema`

`id`
`legs`
`tails`
`You must return a table with the format as follows:`

`output schema`

`id`
`heads`
`legs`
`arms`
`tails`
`species`
`The IDs on the tables match to make a full monster. For heads, arms, legs and tails you need to draw in the data from each table.`

`For the species, if the monster has more heads than arms AND/OR more tails than legs, it is a 'BEAST' else it is a 'WEIRDO'. This needs to be captured in the species column.`

`All rows should be returned (10).`

`Tests require the use of CASE. Order by species.`

> 题目是将一个怪物的属性查询出来。给了两张表，上半身，下半身，然后将这两张表合并起来，并添加新的一列：怪物是什么物种，这个列的值是由两张表的属性决定的，如果头的数目多于胳膊或者腿的数目则物种值为**BEAST**，否则为**WEIRDO**，最终结果以物种列排序。

我的结果是这样的：

```sql
    select t.id,t.heads,b.legs,t.arms,b.tails,
    case 
        when t.heads>t.arms or t.heads>b.tails 
            then 'BEAST' 
        else 'WEIRDO' 
        end as species 
    from top_half as t,
    bottom_half as b 
    where t.id=b.id 
    order by species asc;
```

> 然后看了看别人的代码，卧槽，懵逼，看来我很外行，别人的代码是这样的：

```sql
SELECT
  th.id,
  th.heads,
  bh.legs,
  th.arms,
  bh.tails,
  CASE WHEN th.heads > th.arms or bh.tails > bh.legs
    THEN 'BEAST'
    ELSE 'WEIRDO'
  END as species
  
FROM top_half th

INNER JOIN bottom_half bh
  on bh.id = th.id

ORDER BY species
```

> 恩，果然外行。。。


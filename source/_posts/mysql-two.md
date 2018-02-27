---
title: SQL练习总结
date: 2018-02-27 14:27:53
tags:
- SQL
categories:
- SQL
---

<blockquote class="blockquote-center">对于计算机技术学习，先感性认知，再理性认识。

———— **Coral（\滑稽脸）**
</blockquote>

![](mysql-two/top.jpeg)

### 为什么先感性，再理性

> 今天又开了一门新课程，名曰`系统分析与设计`。当我看到这门课程名字的时候，大概就猜测到这门课要学什么了。。。这节课第一课时听着还算认真，记了一点笔记。虽然我做过一些项目，我的实力却没有像做过项目的大佬那样一样可靠。勉强能够联想到这门课的实际用途。就是系统的分析和设计，实现某一个系统前所做的设计和分析，怎么样才能最优化的完成这个系统，就比如说万金油`图书馆管理系统`。首先你需要搞清楚需求，然后你才能对这个系统进行设计，又是什么UML，什么功能设计，UI设计，数据库设计，架构设计，规范设计等等，还有人员管理，正如老师说的`系统分析与设计不只是讲技术，还有项目管理`。然后听到第二课时，困意无法阻挡，emmmm。嗯，感觉听这门课老师讲的都是对的，无法反驳，但不知道为什么。老师也说项目管理是练出来，当你摸爬滚打了一段时间后，你再来听这门课你就会豁然开朗，没错，这就是我认为的先感性在理性。先要感性的知道这玩意是干啥的，然后再来理性的系统的学习这门课。如何感性的认知呢？那就是实际操作呗。

### 写入正题

> 最近还在复习数据库，也不叫复习，自学的都忘的一干二净了。下面就简单的总结一下语句。(关键字最好大写,我偷个懒...)

#### 数据库操作
``` SQL
-- 创建数据库
create database hello;
create database hello character set utf8;
-- 修改数据库编码
alter database hello character set utf8;
-- 删除数据库
drop table hello;
```

#### 表的操作

```SQL
use hello;
-- 创建数据表
create table users(
    -- 非负主键自增
    id smallint unsigned primary key auto_increment,
    -- 唯一约束,几表中用户名不能有相同的,非空
    username varchar(20) unique key not null,
    password varchar(32) not null,
    -- 年龄默认10岁
    age tinyint unsigned default 10,
    sex boolean,
    pid bigint,
    -- pid设置外键,本表为子表,provinces为父表
    foreign key (pid) references provinces (id)
);
-- 修改数据表
-- 增加列
alter table users add truename varchar(20);
alter table users add truename varchar(20) first;
alter table users add truename varchar(20) after username;
-- 增加多列
alter table users add firstname varchar(15),lastname varchar(15)
-- 删除列
alter table users drop truename;
alter table users drop firstname,drop last name;
-- 添加约束
alter table users add constraint PK_users2_id primary key (id);
alter table users add unique key (username);
alter table users add foreign key (pid) references provinces (id)
alter table users alter age set default 16;
-- 删除约束
alter table users alter age drop default;
alter table users drop primary key;
需要删除key值,需要使用`show indexs from users;`来查看.
alter table users drop index username;
-- 删除外键约束的需要使用其名字
alter table drop foreign key users_ibfk_1;
-- 删除外键后索引存在继续删除index索引
alter table users drop index pid;
```

#### 记录操作

```SQL
-- 增
insert users values ('Tom',20);
insert users values ('Tom',20),('Jhon',10);
insert users(username) values('Tom');
insert users set username='Tom';
insert users(username) select name from customers;
-- 单表改
-- 所有记录都改为用户名tom
update users set username = 'Tom';
update users set username = 'Tom' where age>10;
-- 单表删
-- 所有记录都被删除
delete from users;
delete from users where age>10;
-- 查
select users.id,username from users where age > 10 order by age desc;
-- 将结果分组,类似集合无重复元素
select sex from users group by sex;
-- 设置分组条件,将性别人数大于2的性别显示出来
select sex from users group by sex having count(id)>2;
select * from users limit 10;
-- 从第三个开始,显示出两个,索引从0开始
select * from users limit 2,2;
```

### Over

暂时看了这些,阶段性总结回顾.
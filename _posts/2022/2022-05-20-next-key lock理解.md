---
layout:   post        # 使用的布局（不需要改）
title:    'next-key lock理解'    # 标题
date:    '2022-05-20'    # 时间
author:   meguriri       # 作者
categories: 数据库
tags:
- Mysql
- 锁
excerpt: 'Mysql的一般事务隔离级别为可重复读，因为可串行化级别并发性过低。但是可重复读级别不能很好的解决幻读这一问题，因此在RR（可重复读）隔离级别中，Mysql默认使用一种叫做next-key lock的锁来解决幻读。对于不同的查询和索引类型，next-key lock会有不同的退化，达到锁的优化。'
---

## next-key lock理解

### 几种锁

#### Record Locks

记录锁，锁定一个记录（行），也就是行锁，锁住的只是索引记录，而不是真正的数据。

#### Gap Locks

间隙锁，锁定索引记录之间的间隙，不包括索引本身，左开右开

#### Next-Key Locks

临键锁，他是Record Locks和Gap Locks的结合，左开右闭，

### 对于不同的查询next-key lock的情况

    首先，我们先构造一个简单的表，其中id为主键索引（唯一索引），b是普通索引（非唯一索引），a为普通数据。

t_test:

| id | a   | b  |
| -- | --- | -- |
| 0  | 0   | 0  |
| 4  | 4   | 4  |
| 8  | 8   | 8  |
| 16 | 16  | 16 |
| 32 | 32  | 32 |

### 唯一索引等值查询

当我们用唯一索引进行等值查询的时候，查询的记录存不存在，加锁的规则也会不同：

- **当查询的记录是存在的，在用「唯一索引进行等值查询」时，next-key lock 会退化成「记录锁」**。
- **当查询的记录是不存在的，在用「唯一索引进行等值查询」时，next-key lock 会退化成「间隙锁」**。

#### 记录存在：

```mysql
session 1:
select * from t_test where id=16 for update;/*加 x锁*/

session 2：
update t_test set a=100 where id=16;

session 3:
insert into t_test value(9,9,9);

```

分析：

对于第一个会话，next-key lock加锁范围为**(8,16]**。然而因为id为唯一索引且查询记录存在，因此next-key lock退化为记录锁，因此最终加锁范围仅为**id=16**这一行记录。

因此会话2是更新id=16会阻塞，而会话3插入id=9的记录可以正常执行

#### 记录不存在:

```mysql
session 1：
select * from t_test where id=10 for update;

session 2:
insert into t_test value(9,9,9);

session 3:
update t_test set a=100 where id=16;
```

分析：

对于第一个会话，next-key lock加锁范围为**(8,16]**。然而因为id为唯一索引且查询记录不存在，因此next-key lock退化为间隙锁，因此最终加锁范围仅为**(8,16)**这一行记录。

因此会话2插入id=9的记录会阻塞，而会话3修改id=16这条记录可以正常更新。

### 唯一索引范围查询

等值与范围需要分看判断，比如id>=8,需要分为id=8和id>8分别判断。如果范围的右边界小于next-key lock的右边界，next-key lock将会退化成间隙锁。

```mysql
session 1:
select * from t_test where id>=8 && id<9 for update;

session 2:
insert into t_test value(9,9,9);

session 3:
update t_test set a=100 where id=8;

session 4:
update t_test set a=100 where id=16;
```

分析：

对于会话1，首先id=8 ，因此next-key lock 为(4,8]，又因为id为唯一索引且id=8存在，因此next-key lock退化为记录锁，也就只对**id=8**这一行记录加锁。

接下来，由于范围查找，需要找id>8,且id<9的范围，因此到id=16就会结束,因此next-key lock 为(8,16]。因为id=16不满足id<9,next-key lock退化成间隙锁，加锁范围为**(8,16)**。最终会话1的加锁范围为：**[8,16)**。

因此，会话2会阻塞，会话3会阻塞，而会话4可以正常执行。

### 非唯一索引等值查询

当我们用非唯一索引进行等值查询的时候，查询的记录存不存在，加锁的规则也会不同：

- **当查询的记录存在时，除了会加 next-key lock 外，还额外加下一个范围的间隙锁，也就是会加两把锁，也就是查询记录左右两边的next-key lock**。
- **当查询的记录不存在时，只会加 next-key lock，然后会退化为间隙锁，也就是只会加一把锁。**

#### 记录存在

```mysql
session 1：
select id from t_test where b=8 for update;

session 2：
insert into t_test value(9,9,9);

session 3:
insert into t_test value(5,5,5);

session 4:
update t_test set a=100 where b=8;

session 5:
update t_test set a=100 where b=16;

```

分析：

对于会话1，先在b上加next-key lock ，范围**(4,8]**。因为是非唯一索引，且查询记录存在，所以还会加上间隙锁，规则为向下遍历第一个符合的间隙才停止，因此间隙锁范围**(8,16)**。最终加锁范围为**(4,16)**。

因此会话2阻塞，会话3阻塞，会话4阻塞，而会话不在加锁范围可以正常执行。

#### 记录不存在

```mysql
session 1:
select * from t_test where b=10 for update;

session 2:
insert into t_test value(9,9,9);

session 3:
update t_test set a=100 where b=16;
```

分析：

对于会话1，先在b上加next-key lock ，范围**(8,16]**。由于记录不存在，因此next-key lock 退化为间隙锁，最终加锁范围为**(8,16)**。

因此会话2阻塞，而会话3正常执行。

### 非唯一索引范围查询

非唯一索引和主键索引的范围查询的加锁也有所不同，不同之处在于**普通索引范围查询，next-key lock 不会退化为间隙锁和记录锁**。

```mysql
session 1:
select * from t_test where b>=8 and b<9 for update;

session 2:
update t_test set a=100 where b=8;

session 3:
insert into t_test value(10,10,10);

session 4:
update t_test set a=100 where b=16;

```

分析：

对于会话1，最开始要找b=8，因此next-key lock 为**(4,8]**。但是由于 b 不是唯一索引，并不会退化成记录锁。但是由于是范围查找，就会继续往后找存在的记录，也就是会找到 b = 16 这一行停下来，然后加 next-key lock (8, 16]，因为是普通索引查询，所以并不会退化成间隙锁。最终加锁范围为**(4,16]**。

因此会话2，3，4全部阻塞。

### 总结

唯一索引等值查询：

- 当查询的记录是存在的，next-key lock 会退化成「记录锁」。
- 当查询的记录是不存在的，next-key lock 会退化成「间隙锁」。

非唯一索引等值查询：

- 当查询的记录存在时，除了会加 next-key lock 外，还额外加间隙锁。
- 当查询的记录是不存在的，next-key lock 会退化成「间隙锁」。

非唯一索引和主键索引的范围查询的加锁规则不同之处在于：

- 唯一索引在满足一些条件的时候，next-key lock 退化为间隙锁和记录锁。
- 非唯一索引范围查询，next-key lock 不会退化为间隙锁和记录锁。

也就是说：

1. 唯一索引等值查询命中，next-key lock退化为记录锁
2. 非唯一索引等值命中，除了会加 next-key lock 外，还额外向后加间隙锁，也就是会加两把锁。
3. 无论是唯一还是非唯一索引等值查询未命中，均退化为间隙锁
4. 唯一索引的范围查询，需要分别看待等值和范围而且next-key lock可以退化为记录锁或者间隙锁
5. 非唯一索引的范围查询，需要分别看待等值和范围，但是next-key lock不会退化

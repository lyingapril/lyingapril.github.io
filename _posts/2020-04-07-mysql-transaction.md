---
layout: post
title:  "MySQL 事务"
date:   2020-04-04 20:49:38 +0800
categories: post
---

　　MySQL 事务主要用于处理操作量大，复杂度高的数据。一般来说，事务是必须满足4个条件（**ACID**）：：原子性（**Atomicity**，或称不可分割性）、一致性（**Consistency**）、隔离性（**Isolation**，又称独立性）、持久性（**Durability**）。 
　　事务隔离分为不同级别，包括读未提交（**Read** **uncommitted**）、读提交（**read** **committed**）、可重复读（**repeatable** **read**）和串行化（**Serializable**）。

#### 一、MySQL 事务隔离级别

| 事务隔离级别 | 脏读 | 不可重复读 | 幻读 |
| ------------ | ---- | ---- | ---- |
|读未提交（read-uncommitted）|是|是|是|
|不可重复读（read-committed）|否|是|是|
|可重复读（repeatable-read）|否|否|是|
|串行化（serializable）|否|否|否|

#### 二、读未提交（read-uncommitted）示例

未完待续……20200409

#### 参考链接

[MySQL存储引擎－－MyISAM与InnoDB区别](https://blog.csdn.net/xifeijian/article/details/20316775)

[MySQL的四种事务隔离级别](https://www.cnblogs.com/huanongying/p/7021555.html)


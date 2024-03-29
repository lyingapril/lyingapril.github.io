---
layout: post
title:  "MySQL优化(一) -- 基本知识以及增删改查"
date:   2020-04-08 20:49:38 +0800
categories: Post
tags: mysql
---
　　MySQL的优化主要集中体现在锁的优化上。
　　说到锁，就回忆起大学自学《计算机操作系统》课程里面讲到的死锁必要条件的第一个条件，摘录如下：

> 互斥条件：指进程对所分配到的资源进行排它性使用，即在一段时间内某资源只由一个进程占用。如果此时还有其它进程请求该资源，则请求者只能等待，直至占有该资源的进程用毕释放。

　　MySQL默认的调度策略是写入者优先于读取者。　　

　　写入操作到达时，如果有读取操作还未执行完，那么写入操作会被阻塞。

　　** 要提高MySQL的更新/插入效率，应首先考虑降低锁的竞争，减少写操作的等待时间。 **

　　未完待续…… 20200409

#### 参考链接

[MySQL优化系列（一）--库与表基本操作以及数据增删改](https://blog.csdn.net/jack__frost/article/details/71194208)


---
layout: post
title:  "MySQL 编码"
date:   2020-04-25 14:07:38 +0800
categories: Post
tags: mysql

---
　　因为历史遗留问题，MySQL 中的 utf8 编码并不是真正的 UTF-8，而是阉割版的，最长只有3个字节。

　　最近在搭建商城项目时，手册中推荐的排序规则是 **utf8mb4_general_ci**，特意搜寻一番，关于 MySQL 的排序规则。
#### 参考链接
[MySQL 编码：utf8 与 utf8mb4，utf8mb4_unicode_ci 与 utf8mb4_general_ci_数据库_kikajack的博客-CSDN博客](https://blog.csdn.net/kikajack/article/details/84668924)
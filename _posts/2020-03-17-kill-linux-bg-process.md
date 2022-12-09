---
layout: post
title:  "终止 Linux 后台运行程序的方法和命令"
date:   2020-03-17 10:38:38 +0800
categories: post
---

　　后台运行了得停止呀，怎么找呢？

## 查看后台进程
```sh
ps -ef |grep test.sh
```

|USER|PID|%CPU|%MEM|VSZ|RSS|TTY|STAT|START|TIME|COMMAND|
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
|root|3386|0.0|0.0|113696|1996|pts/0|S|Mar11|8:34|/bin/sh ./jobs.sh|
|root|23197|0.0|0.0|112712|976|pts/1|S+|10:21|0:00|grep --color=auto jobs.sh|

## 结束后台进程

```sh
kill -9 3386
```

## 参考文献

[每天一个linux命令（41）：ps命令](https://www.cnblogs.com/peida/archive/2012/12/19/2824418.html)

[正则表达式 - 语法](https://www.runoob.com/regexp/regexp-syntax.html)

[正则表达式匹配多个空格](https://blog.csdn.net/wuxing164/article/details/52387529)

[每天一个linux命令（42）：kill命令](https://www.cnblogs.com/peida/archive/2012/12/20/2825837.html)
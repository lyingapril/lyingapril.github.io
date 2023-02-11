---
layout: post
title:  "在 Linux 后台运行脚本的方法和命令"
date:   2020-03-02 19:03:38 +0800
categories: Post
tags: [shell, linux]
---

　　有个 sh 脚本没法用宝塔的定时任务来运行，特意学习 Linux 原生的办法后台运行脚本。

## 脚本执行
```sh
  ./test.sh
```
　　中断脚本 test.sh `ctrl+c`

　　切换到后台并暂停 `ctrl+z`

## 直接后台运行
```sh
  /test.sh &
```

## 查看已启动任务
```sh
  jobs
```
　　使用 `jobs` 可看到 `test.sh` 处于 `running` 状态

## 后台运行
```sh
  bg number
```
　　`number` 是使用 `jobs` 命令查到的 `[]` 中的数字，不是pid。

　　执行 `ctrl+z` 后，`test.sh` 在后台是暂停状态（stopped），使用命令 `bg number` 让其在后台开始运行。

## 切换前台运行
```sh
  fg %number
```
　　中断后台运行的 `test.sh` 脚本：先 `fg %number` 切换到前台，再 `ctrl+c` ；或是直接 `kill %number`

## 忽略hangup信号
　　退出当前 shell 终端时会向所有子进程发送hangup信号，后台运行 `test.sh` 随之终止。
```sh
  nohup ./test.sh &
```
　　可以不中断地后台运行 `test.sh` ，且打印信息会输出到当前目录下的 `nohup.out` 中。

　　使用 `ps -ef |grep test.sh` 可查看到正在运行的 `test.sh` 脚本进程。<br>
　　退出当前 shell 终端，再重新打开，使用 `jobs` 看不到正在运行的 `test.sh` ，但使用 `ps -ef` 可以看到。<br>
　　在后台不中断的运行 `test.sh` ，可以使用 `nohup` 忽略 `hangup` 信号，或者使用 `setsid` 将其父进程改为 init 进程（进程号为1）。

　　不中断的在后台运行 `test.sh` 另一个命令
```sh
`setsid ./test.sh &`
```
　　使用
```sh
`ps -ef |grep test.sh`
```
　　可看到 `test.sh` 进程的父进程 id 为1

## 参考链接
[https://www.cnblogs.com/wutao1935/p/10306017.html](https://www.cnblogs.com/wutao1935/p/10306017.html)
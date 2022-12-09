---
layout: post
title:  "Windows 和 Linux 换行符的区别"
date:   2020-03-26 09:00:35 +0800
categories: post
---
　　之前提到<[在 Linux 后台运行脚本的方法和命令](https://aunhappy.github.io/post/2020/03/02/methods-and-commands-for-running-scripts-in-the-Linux-background.html)>，今天运行一个脚本的时候发现了不同寻常的地方。

　　以脚本名 `jobs.sh` 为例，执行如下命令

```sh
[root@iZ8vbf6lodiycg01c3qm05Z /]# nohup ./jobs.sh &
[1] 26568
[root@iZ8vbf6lodiycg01c3qm05Z /]# nohup: ignoring input and appending output to ‘nohup.out’
nohup: failed to run command ‘./jobs.sh’: No such file or directory
```

　　不知为何？出现了报错！

　　直接运行也不可以！

```sh
[root@iZ8vbf6lodiycg01c3qm05Z /]# ./jobs.sh 
-bash: ./jobs.sh: /bin/sh^M: bad interpreter: No such file or directory
```

　　这到底是怎么一回事？莫非是玄学问题？

```sh]
cat jobs.sh
```

　　也没看出来，什么问题．试了下，

```sh
[root@iZ8vbf6lodiycg01c3qm05Z /]# bash jobs.sh 
jobs.sh: line 2: $'\r': command not found
jobs.sh: line 49: syntax error: unexpected end of file
```

　　有了报错行就好办了，但是先前 `cat jobs.sh` ，第二行是肉眼可见的空行，既然肉眼无法可见，那么试试别的？

```js
cat -A jobs.sh
```

　　换行符不对，改好之后就没问题了。

### 参考链接

[Linux、Windows 和 Mac 中的换行符对比](https://www.cnblogs.com/cnjavahome/p/8893813.html)
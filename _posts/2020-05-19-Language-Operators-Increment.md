---
layout: post
title:  "php 递增/递减运算符 "
date:   2020-05-19 11:25:38 +0800
categories: Post
tags: php
---

　　这事得从一道面试题说起：

```php
<?php
$a = "aabbzz";
$a++;
echo $a; //aabcaa
```

　　一开始觉得结果应该是 `z` 的 ASCII 下一位 `{`

　　后来在 php 手册上找到了答案，引用如下：

> PHP follows Perl's convention when dealing with arithmetic operations    on character variables and not C's.  For example, in PHP and Perl    *$a = 'Z'; $a++;* turns *$a* into *'AA'*, while in C    *a = 'Z'; a++;* turns *a* into *'['*    (ASCII value of *'Z'* is 90, ASCII value of *'['* is 91).    Note that character variables can be incremented but not decremented and    even so only plain ASCII alphabets and digits (a-z, A-Z and 0-9) are supported.    Incrementing/decrementing other character variables has no effect, the    original string is unchanged.    

　　php 在处理字符串的递增时，沿袭了 Perl 的风格，`z` 的下一位是 `a` 

#### 参考链接

[PHP: Incrementing/Decrementing Operators - Manual ](https://www.php.net/manual/en/language.operators.increment.php) 
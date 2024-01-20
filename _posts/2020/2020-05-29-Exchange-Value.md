---
layout: post
title:  "交换两个变量的值"
date:   2020-05-29 14:51:38 +0800
categories: Post
tags: php
---

　　这时候，第一时间想到的是，借助第三个变量来实现变量交换。

```php
function swap(&$a,&$b)
{
    $tmp = $a;
    $a = $b;
    $b = $tmp;
} 
```

　　那如果不允许使用第三方变量呢？可以使用加法算法和减法算法来实现！

```php
function addition_swap(&$a,&$b)
{
    $a = $a + $b;
    $b = $a - $b;
    $a = $a - $b;
}

function subtraction_swap(&$a,&$b)
{
    $a = $a - $b;
    $b = $a + $b;
    $a = $b - $a;
}
```

　　以上方法仅适用于变量的数据类型是整型和浮点型，其中浮点型还特别需要注意精度（另一篇文章再探讨精度问题）。

　　可以用异或逻辑运算来交换两个 **等长** 的字符串：

```php
function XOR_swap(&$a,&$b)
{
    $a = $a ^ $b;
    $b = $a ^ $b;
    $a = $b ^ $a;
}
```

#### 参考文献

[交换两个变量的值 - 简书](https://www.jianshu.com/p/74eccdf32de9) 

[PHP: Floating point numbers - Manual](https://www.php.net/manual/en/language.types.float.php) 
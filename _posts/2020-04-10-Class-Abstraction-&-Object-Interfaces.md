---
layout: post
title:  "抽象类和对象接口"
date:   2020-04-10 10:23:38 +0800
categories: Post
tags: laravel
---
　　写业务基本上是离不开抽象类和对象接口的，真到面试的时候，又哑口无言。

> 感觉能写业务和精通是两回事（@初见）

　　那么，究竟什么是 [抽象类](https://www.php.net/manual/en/language.oop5.abstract.php) 和 [对象接口](https://www.php.net/manual/en/language.oop5.interfaces.php) 呢？引用 php 手册的说法就是：

> PHP 5 introduces abstract classes and methods. Classes defined as    abstract cannot be instantiated, and any class that   contains at least one abstract method must also be abstract. Methods   defined as abstract simply declare the method's signature - they cannot   define the implementation.  

>    Object interfaces allow you to create code which specifies which methods a   class must implement, without having to define how these methods are   implemented.  
>
>    Interfaces are defined in the same way as a class, but with the *interface*   keyword replacing the *class* keyword and without any of the methods having   their contents defined.  
>
>    All methods declared in an interface must be public; this is the nature of an   interface.  
>
>    Note that it is possible to declare a [constructor](https://www.php.net/manual/en/language.oop5.decon.php#language.oop5.decon.constructor) in an interface,   which can be useful in some contexts, e.g. for use by factories.  

#### 抽象类

1. 特点

   用 **abstract** 关键字声明，不能实例化，至少有一个抽象方法，子类定义父类所有抽象方法
   
2. Demo-Self
   ```php
<?php
   
   abstract class car
   {
       public $color;
   
       abstract public function gas();
   
       public function getColor()
       {
           return $this->color;
       }
   
       public function setColor($color)
       {
           $this->color = $color;
       }
   }
   
   class specialCar extends car
   {
   
       public function gas()
       {
           echo '#95';
       }
   }
   ```




3. Demo-Laravel
   ```php
   <?php
   
   namespace Illuminate\Cache\Events;
   
   abstract class CacheEvent
   {
       /**
        * The key of the event.
        *
        * @var string
        */
       public $key;
   
       /**
        * The tags that were assigned to the key.
        *
        * @var array
        */
       public $tags;
   
       /**
        * Create a new event instance.
        *
        * @param  string  $key
        * @param  array  $tags
        * @return void
        */
       public function __construct($key, array $tags = [])
       {
           $this->key = $key;
           $this->tags = $tags;
       }
   
       /**
        * Set the tags for the cache event.
        *
        * @param  array  $tags
        * @return $this
        */
       public function setTags($tags)
       {
           $this->tags = $tags;
   
           return $this;
       }
   }
   ```

#### 对象接口

1. 特点

   用 **implements** 声明，不能实例化，所有方法都是 **public** ，类实现 （**implement**）接口（**interface**）中定义的所有方法

2. Demo-Self

   ```php
   <?php
   
   interface car
   {
       public function driving();
   }
   
   class horseCar implements car
   {
       public function driving()
       {
           echo 'slow';
       }
   }
   
   class saloon implements car
   {
       public function driving()
       {
           echo 'fast';
       }
   }
   ```
3. Demo-Laravel
   ```php
   <?php
   
   namespace Illuminate\Contracts\Config;
   
   interface Repository
   {
       /**
        * Determine if the given configuration value exists.
        *
        * @param  string  $key
        * @return bool
        */
       public function has($key);
   
       /**
        * Get the specified configuration value.
        *
        * @param  array|string  $key
        * @param  mixed  $default
        * @return mixed
        */
       public function get($key, $default = null);
   
       /**
        * Get all of the configuration items for the application.
        *
        * @return array
        */
       public function all();
   
       /**
        * Set a given configuration value.
        *
        * @param  array|string  $key
        * @param  mixed  $value
        * @return void
        */
       public function set($key, $value = null);
   
       /**
        * Prepend a value onto an array configuration value.
        *
        * @param  string  $key
        * @param  mixed  $value
        * @return void
        */
       public function prepend($key, $value);
   
       /**
        * Push a value onto an array configuration value.
        *
        * @param  string  $key
        * @param  mixed  $value
        * @return void
        */
       public function push($key, $value);
   }
   ```

#### 抽象类和对象接口的异同

##### 相同之处

1. 不能被实例化，或继承（**extends**）或实现（**implements**）

2. 是抽象层

##### 不同之处

1. 抽象类可以包含具体实现的方法，对象接口不能
2. 子类只能继承一个父类，或抽象类或非抽象类，但可以实现多个对象接口
3. 抽象类是对具体事物的抽象，封装的是具体事物的本质属性；对象接口是对具体事务动作的抽象，封装的是具体事物的非特有属性。
4. 对象接口的每个方法子类都要实现，规范约束了子类，抽象类的抽象方法子类必须定义，非抽象方法可以直接继承使用。

#### 参考链接

[抽象类和接口比较总结 - Laravel China 社区 ](https://learnku.com/laravel/t/9095/a-comparison-of-abstract-classes-and-interfaces)


---
layout: post
title:  "PHP中spl_autoload_register函数的用法"
date:   2011-03-15 12:22:00 +0800
excerpt: 注册__autoload()函数说明
categories: jekyll update
---   
<!--markdown-->spl_autoload_register
(PHP 5 >= 5.1.2)spl_autoload_register — 注册__autoload()函数说明
bool spl_autoload_register ([ callback $autoload_function ] )
将函数注册到SPL __autoload函数栈中。如果该栈中的函数尚未激活，则激活它们。如果在你的程序中已经实现了__autoload函数，它必须显式注册到__autoload栈中。因为spl_autoload_register()函数会将Zend Engine中的__autoload函数取代为spl_autoload()或spl_autoload_call()。


<!--more-->


参数autoload_function 
欲注册的自动装载函数。如果没有提供任何参数，则自动注册autoload的默认实现函数spl_autoload()。返回值
如果成功则返回 TRUE，失败则返回 FALSE。注：SPL是Standard PHP Library(标准PHP库)的缩写。它是PHP5引入的一个扩展库，其主要功能包括autoload机制的实现及包括各种Iterator接口或类。SPL autoload机制的实现是通过将函数指针autoload_func指向自己实现的具有自动装载功能的函数来实现的。SPL有两个不同的函数spl_autoload, spl_autoload_call，通过将autoload_func指向这两个不同的函数地址来实现不同的自动加载机制。范例
设我们有一个类文件A.php，里面定义了一个名字为A的类：

    <?php   
    class A   
    {   
    public function __construct()   
    {   
    echo 'Got it.';   
    }   
    }
    <?php
    class A
    {
    public function __construct()
    {
    echo 'Got it.';
    }
    }

然后我们有一个index.php需要用到这个类A，常规的写法就是


    <?php   
    require('A.php');   
    $a = new A();
    <?php
    require('A.php');
    $a = new A();

但是有一个问题就是，假如我们的index.php需要包含的不只是类A，而是需要很多类，这样子就必须写很多行require语句，有时候也会让人觉得不爽。

不过在php5之后的版本，我们就不再需要这样做了。在php5中，试图使用尚未定义的类时会自动调用__autoload函数，所以我们可以通过编写__autoload函数来让php自动加载类，而不必写一个长长的包含文件列表。
例如在上面那个例子中，index.php可以这样写：


    <?php   
    function __autoload($class)   
    {   
    $file = $class . '.php';   
    if (is_file($file)) {   
    require_once($file);   
    }   
    }   
    
    $a = new A();
    <?php
    function __autoload($class)
    {
    $file = $class . '.php';
    if (is_file($file)) {
    require_once($file);
    }
    }
    $a = new A();

当然上面只是最简单的示范，__autoload只是去include_path寻找类文件并加载，我们可以根据自己的需要定义__autoload加载类的规则。
此外，假如我们不想自动加载的时候调用__autoload，而是调用我们自己的函数（或者类方法），我们可以使用spl_autoload_register来注册我们自己的autoload函数。它的函数原型如下：
bool spl_autoload_register ( [callback $autoload_function] )
我们继续改写上面那个例子：

    <?php   
    function loader($class)   
    {   
    $file = $class . '.php';   
    if (is_file($file)) {   
    require_once($file);   
    }   
    }   
    
    spl_autoload_register('loader');   
    
    $a = new A();
    <?php
    function loader($class)
    {
    $file = $class . '.php';
    if (is_file($file)) {
    require_once($file);
    }
    }
    spl_autoload_register('loader');
    $a = new A();

这样子也是可以正常运行的，这时候php在寻找类的时候就没有调用__autoload而是调用我们自己定义的函数loader了。同样的道理，下面这种写法也是可以的：

    <?php   
    class Loader   
    {   
    public static function loadClass($class)   
    {   
    $file = $class . '.php';   
    if (is_file($file)) {   
    require_once($file);   
    }   
    }   
    }   
    
    spl_autoload_register(array('Loader', 'loadClass'));   
    
    $a = new A();

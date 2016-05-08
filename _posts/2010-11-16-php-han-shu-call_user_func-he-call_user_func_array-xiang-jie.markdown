---
layout: post
title:  "php函数call_user_func和call_user_func_array详解"
date:   2010-11-16 13:17:00 +0800
excerpt: 记录下学习的例子
categories: jekyll update
---   
<!--markdown-->call_user_func函数类似于一种特别的调用函数的方法，使用方法如下： 

    <?php
    function a($b,$c) 
    {
    echo $b;
    echo $c;
    }
    call_user_func('a', "111","222");
    call_user_func('a', "333","444");
    //显示 111 222 333 444
    ?>

调用类内部的方法比较奇怪，居然用的是array，不知道开发者是如何考虑的，当然省去了new，也是满有新意的:


<!--more-->


    <?php
    
    class a {
    function b($c) 
    {
    echo $c;
    }
    }
    call_user_func(array("a", "b"),"111");
    //显示 111
    ?>

call_user_func_array函数和call_user_func很相似，只不过是换了一种方式传递了参数，让参数的结构更清晰:

    function a($b, $c) 
    {
    echo $b;
    echo $c;
    }
    call_user_func_array('a', array("111", "222"));
    //显示 111 222

call_user_func_array函数也可以调用类内部的方法的

    Class ClassA
    {
    function bc($b, $c) {
         $bc = $b + $c;
    echo $bc;
    }
    }
    call_user_func_array(array('ClassA','bc'), array("111", "222"));
    //显示 333

call_user_func函数和call_user_func_array函数都支持引用，这让他们和普通的函数调用更趋于功能一致:

    function a(&$b)
    {
    $b++;
    }
    $c = 0;
    call_user_func('a', &$c);
    echo $c;//显示 1
    call_user_func_array('a', array(&$c));
    echo $c;//显示 2

原文地址：http://www.5iphp.com/zh-hans/content/348.html


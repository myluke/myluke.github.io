---
layout: post
title:  "php的静态变量解释，实例代码"
date:   2011-03-28 22:52:00 +0800
excerpt:  php的静态变量解释，实例代码 自己学习下
categories: jekyll update
---   
<!--markdown-->    class A{
      var $b = 0;
      static private $c = 0;
      public function test(){
         $this->b ++;
         self::$c ++;
      }
      public function out(){
        echo $this->b, "\n", self::$c, "\n";
      }
    }
    $b = new A;
    $c = new A;
    $b->test();  //b=1,c=1
    $b->test();  //b=2,c=2
    $c->test();  //b=1,c=3
    $b->out();
    $c->out();

静态变量不管你实例化几次这个类，他都会往上递加。留个记号！


---
layout: post
title:  "php中$this-&gt;是什么意思？？"
excerpt: $this 的含义是表示实例化后的具体对象！
date:   2008-09-18 22:22:00 +0800
categories: jekyll update
---   
<!--markdown-->> $this 的含义是表示实例化后的具体对象！
> 
> 我们一般是先声明一个类，然后用这个类去实例化对象！
> 
> 但是，当我们在声明这个类的时候，想在类本身内部使用本类的属性或者方法。应该怎么表示呢？

例如：

我声明一个User类！它只含有一个属性 $name;


<!--more-->


    class User
    {
       public $name;
    }

现在，我给User类加个方法。就用getName()方法，输出$name属性的值吧！复制PHP内容到剪贴板PHP代码:

    class User
    {
          public $name;
    
          function getName()
          {
                 echo $this->name;
          }
    }
    
    //如何使用呢？
    
    $user1 = new User();
    
    $user1->name = '张三';
    
    $user1->getName();        //这里就会输出张三！
    
    $user2 = new User();
    
    $user2->name = '李四';    
    
    $user2->getName();       //这里会输出李四！

怎么理解呢？

我上面创建了两个User对象。分别是 $user1 和   $user2 。

当我调用 $user1->getName()的时候。   上面User类中的代码 echo $this->name ; 就是相当于是   echo $user1->name;

大概就是这么个意思！

其实，你也不要去钻牛角尖。你只要知道那是一个用来表示类内部的属性和方法的代号就好了！越想越糊涂的！
    在别的地方找到的！！留下了！！




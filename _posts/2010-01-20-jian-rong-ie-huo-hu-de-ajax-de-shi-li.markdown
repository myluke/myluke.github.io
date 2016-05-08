---
layout: post
title:  "兼容ie、火狐的ajax的实例"
date:   2010-01-20 12:59:00 +0800
excerpt: 需要的就看看,兼容ie、火狐的ajax的实例
categories: jekyll update
---   
<!--markdown-->    <script type="text/javascript">
    /**
    * 初始化一个xmlhttp对象
    */
    function InitAjax()
    {
    　var ajax=false; 
    　try { 
    　　ajax = new ActiveXObject("Msxml2.XMLHTTP"); 
    　} catch (e) { 
    　　try { 
    　　　ajax = new ActiveXObject("Microsoft.XMLHTTP"); 
    　　} catch (E) { 
    　　　ajax = false; 
    　　} 
    　}
    　if (!ajax && typeof XMLHttpRequest!='undefined') { 
    　　ajax = new XMLHttpRequest(); 
    　} 
    　return ajax;
    }
    
    //同时构造相应的JavaScript函数：
    function getNews(newsID)
    {
    　//如果没有把参数newsID传进来
    　if (typeof(newsID) == 'undefined')
    　{
    　　return false;
    　}
    　//需要进行Ajax的URL地址
    　var url = "/test.php?id="+ newsID;
    　//获取新闻显示层的位置
    　var show = document.getElementById("show_news");
    　//实例化Ajax对象
    　var ajax = InitAjax();
    　//使用Get方式进行请求
    　ajax.open("GET", url, true);
    　//获取执行状态
    　ajax.onreadystatechange = function() { 
    　　//如果执行是状态正常，那么就把返回的内容赋值给上面指定的层
    　　if (ajax.readyState == 4 && ajax.status == 200) { 
    　　　show.innerHTML = ajax.responseText; 
    　　} 
    　}
    　//发送空
    　ajax.send(null); 
    }
    </script>
    </head>
    <body>
    //将链接改为：<br/>
    <a href="#" onClick="getNews(1)">新闻1</a><br/>
    //并且设置一个接收新闻的层，并且设置为不显示：<br/>
    <div id="show_news"></div>
    
    <hr>

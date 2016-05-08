---
layout: post
title:  "文本框提示样式，鼠标点击获得焦点时提示内容消失"
date:   2008-12-24 22:46:00 +0800
excerpt: 代码记录下
categories: jekyll update
---   
<!--markdown-->    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=gb2312" />
    <title>文本框提示样式</title>
    <style type="text/css">
    <!--
    .a1 {
    color: #CCCCCC;
    }
    .a2 {
    color: #000;
    }
    -->
    </style>


<!--more-->


    </head>
    <body>
    <input name="n" type="text"/>
    <input name="n1" type="text"/>
    <script language="javascript">
    window.onload=function(){
    s('n','Your message1');s('n1','Your message2')
    }
    function g(a){return document.getElementById(a)}
    function s(n,v){
    with(g(n)){
    className='a1';value=v;
    onfocus=function(){if(value==v)value='';className='a2'}
    onblur=function(){if(value==''){value=v;className='a1'}}
    }
    }
    </script>
    </body>
    </html>

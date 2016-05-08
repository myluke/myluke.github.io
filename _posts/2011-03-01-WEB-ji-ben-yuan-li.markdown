---
layout: post
title:  "WEB基本原理"
date:   2011-03-01 22:14:00 +0800
excerpt: 当从浏览器输入一个网址 如：example.com/news/hello-word  浏览器会向找到这个叫”example.com”的服务器  并向它请求”/news/hello-word”这资源服务会响应这个请求并返回一个源代码(就是你在浏览器上查看到的那个源代码)
categories: jekyll update
---   
<!--markdown-->当从浏览器输入一个网址
如：example.com/news/hello-word
浏览器会向找到这个叫”example.com”的服务器
并向它请求”/news/hello-word”这资源
服务会响应这个请求并返回一个源代码(就是你在浏览器上查看到的那个源代码)


<!--more-->


如:

    <html>
    <head>
    <title>hello word</title>
    …...
    <link rel=”stylesheet” href=”main.css”>
    <script type="text/javascript" src="main.js"></script>
    …...
    </head>
    <body>
    …...
    <img src=”logo.png” />
    <em>Hell Word</em>
    …...
    </body>
    </html>

接下来浏览器会解析这个源代码使之变成我们平时看到的页面.
在这里面
浏览器解析到这个时:`<link rel=”stylesheet” href=”main.css”>`
将会产生一个新的请求： example.com/main.css
并把它的返回作为css样式用于控制页面显示。
当遇到：`<script type="text/javascript" src="main.js"></script>`
时也会产生别一个新请求：example.com/main.js
并将返回的内容作为javascript执行
同样这个：`<img src=”logo.png” />`
也会产生一个新的请求：example.com/logo.png
并把返回结果作为内容里指定的一个图片
最后就是我们最终看到的页面。
浏览器对于页面的显示是解析到哪儿显示到哪儿，这个可以注意观查一些网站很容易就可以看出来。
对于js也是解析到哪就执到哪。（<script>...</script>里面的内容作为一个整体，而且你不能访
问还没有解析到的html文档）
只要是还是web它的工作方面就都是这样子的。所以如果出了问题不要只在最终显示的界面上或只在
php程序中找问题。你应该关注一下浏览器中最终的源代码是不是和你预期的一至。
不管页面多么怎么样，js程序交互多么怎么样，css多么怎么样，所有一切的一切都只是纯文本格的源
代码而己。
浏览器只是解析这些源代码并按原先设计好的方式处理。所谓的服务器端程序（如：php）也只是拼出
一个格式适当的源代码而己。只是纯文本！


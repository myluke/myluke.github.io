---
layout: post
title:  "phpstorm 内存调整"
excerpt: phpstorm 内存调整,16G内存为例
author: Luke
date:   2016-07-25 13:18:00 +0800
categories: jekyll update
---   

以前其实已经调整过，但每次大版本的phpstorm的更新都会覆盖掉这个配置，所以我先记一下。。不然下次又要忘了

* 文件路径:
 
 `sudo vim /Applications/PhpStorm.app/Contents/bin/phpstorm.vmoptions`

* 对着vmoptions的值进行调整，比如象我16G内存，就可以将它设置的大一点，这样可以开起来更流畅

 `# 修改参数，根据具体需要修改即可，一般修改  -Xmx(即最大可占内存)即可  
  -Xms512m  
  -Xmx2048m  
  -XX:MaxPermSize=350m  
  -XX:ReservedCodeCacheSize=240m  
  -XX:+UseCompressedOops`
  
* 上面只是个例子，请根据实际 情况调整

* 转载地址: http://neatstudio.com/show-2671-1.shtml
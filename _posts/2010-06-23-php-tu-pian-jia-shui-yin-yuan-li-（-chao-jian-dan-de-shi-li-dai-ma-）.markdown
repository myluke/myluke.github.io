---
layout: post
title:  "php图片加水印原理（超简单的实例代码）"
date:   2010-06-23 13:31:00 +0800
excerpt: 用过的例子记录下
categories: jekyll update
---   
<!--markdown-->我看到网上有好多关于图片加水印的类,写的很好 ,我这里只是把相应的原理写下,具体需求,根据自己的情况来修改,很简单的,写的不好,高手见谅

    //文字水印:
    $w = 80;
    $h = 20;
    
    $im = imagecreatetruecolor($w,$h);
    
    $textcolor = imagecolorallocate($im, 123, 12, 255);
    
    $white = imagecolorallocate($im, 255, 255, 255);
    $grey = imagecolorallocate($im, 128, 128, 128);
    $black = imagecolorallocate($im, 0, 0, 0);
    imagefilledrectangle($im, 0, 0, 399, 29, $grey);      //画一矩形并填充
    
    // 把字符串写在图像左上角
    imagestring($im, 3, 2, 3, "Hello world!", $textcolor);
    
    // 输出图像
    header("Content-type: image/jpeg");
    imagejpeg($im);
    imagedestroy($im);
    
    //图片水印
    
    $groundImg = "DSC05940.jpeg";
    $groundInfo = getimagesize($groundImg);
    $ground_w = $groundInfo[0];
    //print_r($groundInfo);
    $ground_h = $groundInfo[1];
    switch($groundInfo[2]){
    case 1:
    $ground_im = imagecreatefromgif($groundImg);
    break;
    case 2:
    $ground_im = imagecreatefromjpeg($groundImg);
    break;
    case 3:
    $ground_im = imagecreatefrompng($groundImg);
    break;
    }
    
    $waterImg = "DSC05949.jpeg";
    $imgInfo =getimagesize($waterImg);
    $water_w = $imgInfo[0];
    $water_w = $imgInfo[1];
    
    switch($imgInfo[2]){
    case 1:
    $water_im = imagecreatefromgif($waterImg);
    break;
    case 2:
    $water_im = imagecreatefromjpeg($waterImg);
    break;
    case 3:
    $water_im = imagecreatefrompng($waterImg);
    break;
    }
    imagecopy($ground_im,$water_im,100,100,0,0,500,500); 
    header("Content-type: image/jpeg");
    
    imagejpeg($ground_im);

合并图片php提供了很多函数：例如：imagecopymerge，imagecopyresized
原文地址：http://hi.baidu.com/%B1%D5%C9%CF%D1%DB%BF%B4%BC%FB%CC%EC/blog/item/50c2946196bf9349eaf8f8e6.html

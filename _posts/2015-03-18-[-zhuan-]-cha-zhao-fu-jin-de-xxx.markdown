---
layout: post
title:  "[转]查找附近的xxx 球面距离以及Geohash方案探讨（下）"
date:   2015-03-18 13:01:00 +0800
categories: jekyll update
excerpt: "Geohash算法；geohash是一种地址编码，它能把二维的经纬度编码成一维的字符串。"
---
<!--markdown-->二、方案B
===============================================================

Geohash算法；geohash是一种地址编码，它能把二维的经纬度编码成一维的字符串。
比如，成都永丰立交的编码是wm3yr31d2524

优点:

1、利用一个字段，即可存储经纬度；搜索时，只需一条索引，效率较高
2、编码的前缀可以表示更大的区域，查找附近的，非常方便。 SQL中，LIKE ‘wm3yr3%’，即可查询附近的所有地点。
3、通过编码精度可模糊坐标、隐私保护等。

缺点: 距离和排序需二次运算（筛选结果中运行，其实挺快）


<!--more-->


1、geohash的编码算法

成都永丰立交经纬度(30.63578,104.031601)

1.1、纬度范围(-90, 90)平分成两个区间(-90, 0)、(0, 90)， 如果目标纬度位于前一个区间，则编码为0，否则编码为1。
由于30.625265属于(0, 90)，所以取编码为1。
然后再将(0, 90)分成 (0, 45), (45, 90)两个区间，而39.92324位于(0, 45)，所以编码为0，
然后再将(0, 45)分成 (0, 22.5), (22.5, 45)两个区间，而39.92324位于(22.5, 45)，所以编码为1，
依次类推可得永丰立交纬度编码为101010111001001000100101101010。

1.2、经度也用同样的算法，对(-180, 180)依次细分，(-180，0)、(0,180) 得出编码110010011111101001100000000000

1.3、合并经纬度编码，从高到低，先取一位经度，再取一位纬度；得出结果 111001001100011111101011100011000010110000010001010001000100

1.4、用0-9、b-z（去掉a, i, l, o）这32个字母进行base32编码，得到(30.63578,104.031601)的编码为wm3yr31d2524。

    11100 10011 00011 11110 10111 00011 00001 01100 00010 00101 00010 00100 => wm3yr31d2524 
    十进制  0   1   2   3   4   5   6   7   8   9   10  11  12  13  14  15
    base32   0   1   2   3   4   5   6   7   8   9   b   c   d   e   f   g
    十进制  16  17  18  19  20  21  22  23  24  25  26  27  28  29  30  31
    base32   h   j   k   m   n   p   q   r   s   t   u   v   w   x   y   z

2、策略

1、在纬度和经度入库时，数据库新加一字段geohash，记录此点的geohash值

2、查找附近，利用 在SQL中 LIKE ‘wm3yr3%’；且此结果可缓存；在小区域内，不会因为改变经纬度，而重新数据库查询

3、查找出的有限结果，如需要求距离或者排序，可利用距离公式和二维数据排序；此时也是少量数据，会很快的。

三、测试
竟然粘贴不上来，我叉。大家去原作者那站上看去吧
四、总结

方案B的亮点在于：
1、搜索结果可缓存，重复使用，不会因为用户有小范围的移动，直接穿透数据库查询。
2、先缩小结果范围，再运算、排序，可提升性能。

254条记录，性能对比，

在实际应用场景中，方案B数据库搜索可内存缓存；且如数据量更大，方案B结果会更优。

方案A:
0.016560077667236
0.032402992248535
0.040318012237549

方案B
0.0079810619354248
0.0079669952392578
0.0064868927001953

五、其他

两种方案，根据应用场景以及负载情况合理选择，当然推荐方案B;
不管哪种方案，都记得，给列加上索引，利于数据库检索。


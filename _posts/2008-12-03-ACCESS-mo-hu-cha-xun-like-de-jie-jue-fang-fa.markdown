---
layout: post
title:  "ACCESS模糊查询like的解决方法&amp;&amp;SQL查询语句通配符问题"
date:   2008-12-03 22:41:00 +0800
excerpt: ACCESS的通配符和SQL SERVER的通配符比较
categories: jekyll update
---   
<!--markdown-->ACCESS的通配符和SQL SERVER的通配符比较
===================================================
ACCESS库的通配符为： 
*   与任何个数的字符匹配 
?   与任何单个字母的字符匹配 

SQL Server中的通配符为：
% 与任何个数的字符匹配
_ 与单个字符匹配
正文
我今天在写个页面的时候，也很郁闷，表中明明有记录，但在ASP里就是搜索不到，理论的sql语句如下：
SELECT * FROM t_food WHERE t_food.name like '*苹果*'
去GOOGLE搜搜发现，ASP中模糊查询要这样写：
SELECT * FROM t_food WHERE t_food.name like '%%苹果%%'


<!--more-->


必须是“％”，而且要两个。大家多注意。
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
SQL查询语句通配符问题
在Access中用SQL语句进行数据查询时，用了通配符*进行查询。语句如下：
SELECT * from normal where bookname like '*h*'
在Access的SQL视图中试验没有任何问题，工作一切正常。于是将SQL语句写入到C#程序中，结果一到查询语句时就出错跳出，百思不得其解。于是查找Access帮助文件，找到如下帮助：
////////////////////////////////////////////////////////////
将字符串表达式与 SQL 表达式中的模式进行比较。
语法
expression Like "pattern"
Like 运算符语法包含以下部分：
部分说明 
expression 在 WHERE 子句中使用的 SQL 表达式。 
pattern 与 expression 进行比较的字符串文字。

说明
可以通过 Like 运算符来查找与所指定的模式相匹配的字段值。对于 pattern，可以指定完整的值（例如 Like "Smith"），也可以使用通配符来查找某个范围内的值（例如 Like "Sm*"）。
在表达式中，可以使用 Like 运算符来比较字段值与字符串。例如，如果在 SQL 查询中输入 Like "C*"，那么该查询将返回所有以字母 C 开头的字段值。在参数查询中，可以提示用户键入要搜索的模式。
下面的示例返回以字母 P 开头并且后面为 A 到 F 之间任何字母以及三个数字的数据：
Like "P[A-F]###"
下表展示了如何通过 Like 来测试不同模式的表达式。

匹配类型
模式匹配
（返回 True）不匹配
（返回 False） 
多个字符 a*a aa, aBa, aBBBa aBC 
   *ab* abc, AABB, Xab aZb, bac 
特殊字符 a
a a*a aaa 
多个字符 ab* abcdefg, abc cab, aab 
单个字符 a?a aaa, a3a, aBa aBBBa 
单个数字 a#a a0a, a1a, a2a aaa, a10a 
字符范围 [a-z] f, p, j 2, & 
范围之外 [!a-z] 9, &, % b, a 
非数字值 [!0-9] A, a, &, ~ 0, 1, 9 
复合值 a[!b-m]# An9, az0, a99 abc, aj0

参考地址：http://office.microsoft.com/zh-cn/assistance/HP010322532052.aspx
///////////////////////////////////////////////////////////
帮助都这么写了，没有任何问题啊，到底问题是出在哪里呢？更加让本人迷惑。后来问了一下同事说：你的SQL语句错了，通配符应该用％，而不是*。可是帮助里面说的是*，而且我在Access中试验一切正常，同事也说不上个所以然来，于是继续查找帮助需求答案。在另一个帮助文件中找到了如下信息：
///////////////////////////////////////////////////////////
内置的模式匹配方法提供了一个用于字符串比较的通用工具。下表中展示了可以用于 Like 运算符的通配符，以及与它们匹配的数字和字符串。
pattern 中的字符expression 中的匹配项 
? 或 _（下划线） 任何单个字符 
* 或 % 零个或多个字符 
# 任何单个数字 (0— 9) 
[charlist] 在 charlist 中的任何单个字符。 
[!charlist] 不在 charlist 中的任何单个字符。

可以使用一组由中括号 ([]) 括住的一个或多个字符（charlist）来匹配在 expression 中的任何单个字符，并且 charlist 可以包含大部分 ANSI 字符集中的字符，包括数字在内。可以通过将特定字符如左方括号 ([)、问号 (?)、数字号 (#) 和星号 (*) 包含于方括号内来直接与这些符号自身进行匹配。不能将右方括号用在一个组中以匹配它自身，但可以将它作为单个字符用于组外。
除了括在方括号中的简单字符列表外，charlist 可以通过使用连字符号 (-) 来分隔范围的上界和下界。例如，在 pattern 中使用 [A-Z] 时，如果 expression 中相应的字符包含了任何在 A 到 Z 范围之间的大写字符，就能实现匹配。可以在方括号中包含多个范围而不必为范围划界。例如，[a-zA-Z0-9] 可以匹配任何字母数字字符。
请注意，ANSI SQL 通配符 (%) 和 (_) 仅在 Microsoft? Jet 4.X 版本和 Microsoft OLE DB Provider for Jet 中才是有效的。如果用在 Microsoft Access 或 DAO 中，那么它们被视为文本。
其他重要的用于模式匹配的规则如下所示：
在 charlist 的开头使用感叹号 (!) 将表示如果在 charlist 以外的任何字符出现在 expression 中，则发生匹配。当它用在方括号的外面时，感叹号匹配它自身。 
可以将连字符号 (-) 用于 charlist 的开头（感叹号之后）或末尾以匹配它自身。在其他任何位置中，连字符号标识一个 ANSI 字符范围。 
指定了一个字符范围时，字符必须以升序排列出现（A-Z 或 0-100）。[A-Z] 是有效的模式，[Z-A] 是无效模式。 
忽略字符顺序 [ ]；它被视为一个零长度字符 ("")。 
参考地址：http://office.microsoft.com/zh-cn/assistance/HP010322842052.aspx
///////////////////////////////////////////////////////////////
至此，原因总算是找到了，由于本人在Access中使用通配符*一切正常，换成％则不能成功。而C#中则只是支持％通配符，而换成*则会出错！这个问题算不算是一个兼容性问题呢？

通配符:
通配符 描述 示例 
% 包含零个或更多字符的任意字符串。 WHERE title LIKE '%computer%' 将查找处于书名任意位置的包含单词 computer 的所有书名。 
_（下划线） 任何单个字符。 WHERE au_fname LIKE '_ean' 将查找以 ean 结尾的所有 4 个字母的名字（Dean、Sean 等）。 
[ ] 指定范围 ([a-f]) 或集合 ([abcdef]) 中的任何单个字符。 WHERE au_lname LIKE '[C-P]arsen' 将查找以arsen 结尾且以介于 C 与 P 之间的任何单个字符开始的作者姓氏，例如，Carsen、Larsen、Karsen 等。 
[^] 不属于指定范围 ([a-f]) 或集合 ([abcdef]) 的任何单个字符。 WHERE au_lname LIKE 'de[^l]%' 将查找以 de 开始且其后的字母不为 l 的所有作者的姓氏。
将通配符作为文字使用
可以将通配符模式匹配字符串用作文字字符串，方法是将通配符放在括号中。下表显示了使用 LIKE 关键字和 [ ] 通配符的示例。
符号 含义 
LIKE '5[%]' 5% 
LIKE '[_]n' _n 
LIKE '[a-cdf]' a、b、c、d 或 f 
LIKE '[-acdf]' -、a、c、d 或 f 
LIKE '[ [ ]' [ 
LIKE ']' ] 
LIKE 'abc[_]d%' abc_d 和 abc_de 
LIKE 'abc[def]' abcd、abce 和 abcf
使用 ESCAPE 子句的模式匹配
可搜索包含一个或多个特殊通配符的字符串。例如，customers 数据库中的 discounts 表可能存储含百分号 (%) 的折扣值。若要搜索作为字符而不是通配符的百分号，必须提供 ESCAPE 关键字和转义符。例如，一个样本数据库包含名为 comment 的列，该列含文本 30%。若要搜索在 comment 列中的任何位置包含字符串 30% 的任何行，请指定由 WHERE comment LIKE '%30!%%' ESCAPE '!' 组成的 WHERE 子句。如果不指定 ESCAPE 和转义符，SQL Server 将返回所有含字符串 30 的行。
下例说明如何在 pubs 数据库 titles 表的 notes 列中搜索字符串"50% off when 100 or more copies are purchased"：
SELECT notes FROM titles WHERE notes LIKE '50%% off when 100 or more copies are purchased' ESCAPE '%'

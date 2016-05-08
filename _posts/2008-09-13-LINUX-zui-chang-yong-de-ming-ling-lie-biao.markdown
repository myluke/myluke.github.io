---
layout: post
title:  "LINUX最常用的命令列表"
excerpt: 下面列出十个在使用linux过程中使用频率最高的命令。这里只作简单介绍，具体用法请参考后面内容
date:   2008-09-13 22:15:00 +0800
categories: jekyll update
---   
<!--markdown-->下面列出十个在使用linux过程中使用频率最高的命令。这里只作简单介绍，具体用法请参考后面内容。
cat，显示文件内容。
cd，改变目录路径。
cp，复制文件。
find，查找文件。
grep，搜索、过滤信息。
ls，列出目录信息。
more，分页显示。
rm，删除文件或目录。
vi，调用vi文本编辑器。
who，显示登录用户信息。
-------------------


<!--more-->


chmod----改变一个或多个文件的存取模式(mode)
chmod [options] mode files
$ chmod u+x file                 给file的属主增加执行权限
$ chmod 751 file                 给file的属主分配读、写、执行(7)的权限，给file的所在组分配读、执行(5)的权限，给其他用户分配执行(1)的权限
$ chmod u=rwx,g=rx,o=x file     上例的另一种形式
$ chmod =r file                 为所有用户分配读权限
$ chmod 444 file                同上例
$ chmod a-wx,a+r                同上例
$ chmod -R u+r directory        递归地给directory目录下所有文件和子目录的属主分配读的权限
$ chmod 4755                    设置用ID，给属主分配读、写和执行权限，给组和其他用户
chgrp----修改文件或目录的所属组
chgrp [options] newgroup files/directorys
组名可以用组的ID号，也可用/etc/group中的组名。只有文件的属主或特权用户(root)才可改变它的组。
$ chgrp root test            把test的所属组更改root组$ chgrp -R mysql test        递归地把test目录及该目录下所有文件和子目录的组属性设置成mysql$ chgrp root *               把当前目录中所有文件的组属性设置成root
chown----设置一个或多个文件或目录的属主身份
chown [options] newowner files/directorys
新的属主可以是用户的ID号，也可以是/etc/passwd里的登录名。chown也可接受这样的形式：newowner:newgroup或newowner.newgroup。同时改变所属组的属性。如果句点和冒号后没有组名，则组改变为新属主的组。只有文件或目录的当前属主才有权改变它的属性。$ chown   root test                        把test文件的属主改进root$ chown -R root test_directory            递归地把test_directory目录下的所有文件属主改成root$ chown --dereference root test_link      把test_link链接的原文件属主改成root，链接文件属主不变$ chown --no-dereference root test_link   把test_link的链接文件属主改成root，
df-----显示已安装文件系统的磁盘容量状态
df [options][name]
$ df -h 以友好的格式输出所有已安装文件系统的磁盘容量状态$ df -m /home 以M为单位输出home目录的磁盘容量状态$ df -k 以K为单位输出所有已安装文件系统的磁盘容量状态$ df -i 报告空闲的、用过的或部份用过的（百份比）索引节点$ df -t ext3 仅显示文件类型为ext3的文件系统的磁盘状态$ df -x ext3 仅显示文件类型不为ext3的文件系统的磁盘状态$ df -T 除显示文件系统磁盘容量大小外还显示文件系统类型$ df -l 仅显示本地文件系统。
fdisk----分区表查询工具
fdisk [options][driver]
$ fdisk -l           列出所有分区信息
hdparm----硬盘管理
hdparm [options][driver]
$ hdparm -d   /dev/hda            显示硬盘的DMA模式是不打开，1代表on$ hdparm -tT /dev/hda            测试硬盘的写性能$ hdparm -d1 /dev/hda 开启dma功能$ hdparm -d1 -X68 -c3 -m16 /dev/hda   选项说明：-c3：把硬盘的IO模式从16位转成32位。-m16：改变硬盘的多路扇区的读功能，-m16使硬盘在一次I/O中断中读入16个扇区的数据。-d1：打开DMA模式。-X68：支持ATA66的数据传输模式。下面是其它模式的设置对照ATA33.......参数是-X66 ATA66.......参数是-X68 ATA100......参数是-X69$ hdparm -k1 /dev/hda             保存设置
ln-----为文件建立别名
ln [options] sourcename [destname]
ln [options] sourcenames destdirectory
$ ln -s file1 file2        建立一个到file1的符号链接file2，删除file2不会影响file1$ ln -s -f file1 file2     建立一个到file1的符号链接file2，并不提示是否重写
shutdown-----终止所有进程序，关闭计算机。
shutdown [options] when [message]
用when可以是指定的关机时间(以hh:mm格式)、关机前要等待的时间(以+m格式)、或者now。message指定一条广播消息通知所有用户退出系统。showdown给所有进程发送SIGTERM信号，并调用init 1执行实际的关机动作。
$ shutdown -c           取消正在进行的关闭操作$ shutdown -f           快速重新启动，在重新启动时禁止对fsck的常规调用$ shutdown -h           当关闭完成时停止系统$ shutdown -k           输出警告信息，但禁止实际的关闭$ shutdown -n           不调用init就执行关闭$ shutdown -r           当关闭完成时重新启动系统$ shutdown -t 5         在杀死进程和改变运行级别之间确保延时5秒
sleep-----执行另一个命令之前等待的时间
sleep amount [units]
units默认为秒(s)，m表示分钟，h表示小时，d表示天
swapon/swapoff-----启动和关闭交换分区
swapon/swapoff [options] device
$ swapon -s             显示交换分区信息$ swapon -a             激活所有在/etc/fstab中有sw标记的分区$ swapon -p 1           设置交换分区优先级为1 tune2fs-----调整Linux第二扩展文件系统的参数
tune2fs [options] device
$ tune2fs -l /dev/hda1        显示hda1分区的超级块内容$ tune2fs -c 100 /dev/hda1    设置hda1分区每mount100次就进行磁盘检查

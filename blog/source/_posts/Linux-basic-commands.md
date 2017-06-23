---
title: Linux basic commands
subtitle: Linux 's basic usage
date: 2017-06-14 15:40:47
author: Ricann
head-img:
categories:  tools
tags:
 - Linux
 - tools
 - command
keywords:  Linux, tools, command
comments: true
---

## netstat用法

用于显示各种网络相关信息，如网络连接、路由表、接口状态等。

直接执行netstat命令显示的内容分为两部分：

* Active Internet connections，因特网socket连接信息
* Active UNIX domain sockets，域套接字信息

<!--more-->

常用的选项：

| 参数 | 作用 |
| --- | :---: |
| -a | 显示所有选项 |
| -t | 仅显示tcp相关选项 |
| -u | 仅显示udp相关选项 |
| -l | 仅列出有在listen的服务状态 |
| -p | 显示建立相关链接的程序名 |
| -r | 显示路由信息 |
| -s | 按协议显示统计信息 |

## ifconfig
配置IP地址：ifconfig eth0 192.168.1.99 netmask 255.255.255.0 up

配置物理地址：ifconfig eth0 hw ether 00:11:00:00:11:22

禁止激活网络接口：ifconfig eth0 {down|up}

## dmesg
dmesg用来显示内核环缓冲区（kernel-ring buffer）内容，内核将各种消息存放在这里。在系统引导时，内核将与硬件和模块初始化相关的信息填到这个缓冲区中。

打印显示后清除环缓冲内的内容：dmesg -c

检查当前系统是否为虚拟机可参照下面命令：
 + lspci
 + dmesg | grep vm

## lspci
lspci是一个用来显示系统中所有PCI总线设备或连接到该总线上的所有设备的工具。

查看一般详细信息：lspci -v

查看特定总线详细信息：lspci -s 03:02.0 -v

PCI是Peripheral Component Interconnect（外围设备互联）的简称，PCI规范允许单个系统拥有高达256个总线，所以总线编号是8位。但对于大型系统而言，这是不够的，所以引入了域的概念，每个PCI域可以拥有最多256个总线，每个总线上可支持32个设备，所以设备号是5位，而每个设备上最多可有8种功能，所以功能号是3位。

## lsusb
lsusb -v：显示USB设备的详细信息

lsusb -s <总线:设备号> -v：仅显示指定的总线和设备号的设备

## lsmod
显示已经加载到内核中的模块的状态信息，通常在使用lsmod命令时，都会采用类似**lsmod | grep -i ext3**这样的命令来查询当前系统是否加载了某些模块。

## iptables
五个控制位置点：
+ 内核空间中：从一个网络接口进来，到另一个网络接口去的
+ 数据包从内核流入用户空间的
+ 数据包从用户空间流出的
+ 进入/离开本机的外网接口
+ 进入/离开本机的内网接口

这五个位置也被称为五个钩子函数，也叫五个规则链：
+ PREROUTING (路由前)
+ INPUT (数据包流入口)
+ FORWARD (转发管卡)
+ OUTPUT(数据包出口)
+ POSTROUTING（路由后）

防火墙策略一般分为两种，一种叫通策略，一种叫堵策略。我们现在用的比较多个功能有3个：
+ filter：定义允许或者不允许的，一般只能做在INPUT、FORWARD、OUTPUT三个链上
+ nat：定义地址转换的，一般只能做在PREROUTING、OUTPUT、POSTROUTING三个链上
+ mangle：修改报文原数据，主要是修改TTL，上面五个链都可以做

iptables定义规则的方式比较复杂:
```
格式：
iptables [-t table] COMMAND chain CRETIRIA -j ACTION
字段意义：
-t table ：filter、nat、mangle三个策略
COMMAND：定义如何对规则进行管理
chain：指定你接下来的规则到底是在哪个链上操作的，当定义策略的时候，是可以省略的
CRETIRIA：指定匹配标准
-j ACTION：指定如何进行处理
例子：不允许172.16.0.0/24的进行访问
iptables -t filter -A INPUT -s 172.16.0.0/16 -p udp --dport 53 -j DROP
查看定义规则的详细信息：
iptables -L -n -v
```

更多可参考：
http://blog.chinaunix.net/uid-22780578-id-3346350.html
## strace
strace命令是一个集诊断、调试、统计与一体的工具，我们可以使用strace对应用的系统调用和信号传递的跟踪结果来对应用进行分析，以达到解决问题或者是了解应用工作过程的目的。

strace -c：系统调用统计

strace -p pid：可以追踪一个指定进程

## find
| 常用参数 | 作用 |
| --- | :---: |
| -name filename | 查找名为filename的文件 |
| -perm | 按执行权限来查找 |
| -user username | 按文件属主来查找 |
| -group groupname | 按组来查找 |
| -mtime -n +n | 按文件更改时间来查找文件，-n指n天以内，+n指n天以前 |
| -atime -n +n | 按文件访问时间来查找文件，-n指n天以内，+n指n天以前 |
| -newer filename | 查更改时间比filename新的文件 |
| -type b/d/c/p/l/f | 查是块设备、目录、字符设备、管道、符号链接、普通文件 |
| -size n[bkMG] | 查长度为n[bkMG]字节的文件 |
| -depth | 使查找在进入子目录前先行查找完本目录 |
| -prune | 忽略某个目录 |

locate命令其实是*find -name*的另一种写法，但是要比后者快得多，原因在于它不搜索具体目录，而是搜索一个数据库*/var/lib/locatedb*，这个数据库中含有本地所有文件信息。


## grep

| 常用参数 | 作用 |
| --- | :---: |
| -i | 忽略大小写 |
| -l | 仅列出文件名 |
| -n | 列出搜索结果所在行号 |
| -r | 在指定文件夹中递归搜索 |
| -c | 计算搜索字符串统计次数 |
| -v | 反向选择，即显示出没有搜索字符串内容的哪一行 |

| 通配符 | 作用 |
| --- | :---: |
| . | 代表一个任意字符 |
| * | 重复前面一个字符0-n次 |
| ^ | 行首 |
| $ | 行尾 |
|\[\] | 里面不管有多少字符，只代表一个字节 |
|\[^\]| 反向选择，如\[^a-z\]表示非a-z的字符 |

/< 和 /> 分别标注单词的开始与结尾
例如：
grep man * 会匹配 ‘Batman’、‘manic’、‘man’等
grep '/<man' * 匹配‘manic’和‘man’，但不是‘Batman’
grep '/<man/>' 只匹配‘man’，而不是‘Batman’或‘manic’等其他的字符串
'^'：指匹配的字符串在行首
'$'：指匹配的字符串在行尾

示例：
```
1. 开头不是英文字母
grep -n '^[^a-zA-Z]' regular_express.txt

2. 找出行尾结束为小数点 (.) 的那一行
grep -n '\.$' regular_express.txt

3. 找出空白行
grep -n '^$' regular_express.txt

4. 在当前目录及其子目录下搜索test行的文件，但是不显示匹配的行，只显示匹配的文件
grep -l -r test *

5. 结合find使用，搜索子目录下所有cpp文件，并查找其中的grpc字符串
find . -name "*.cpp" | xargs grep -ni grpc
```

## ps
查看CPU占用的前五名：
>ps aux --sort=-pcpu | head -5

显示一个进程的线程:
>ps -p 3150 -L

按层次关系显示所有进程：
>ps -ef --forest
>ps auxf


进程状态信息：

| 状态字符 | 作用 |
| --- | :---: |
| D | 无法中断的休眠状态（通常 IO 的进程） |
| R | 该进程目前正在运作，或者是可被运作 |
| S | 处于休眠状态 |
| s | 进程的领导者（在它之下有子进程） |
| T | 停止或被追踪 |
| X | 死掉的进程，很少见 |
| Z | 僵尸进程 |
| < | 优先级高的进程 |
| N | 优先级较低的进程 |
| L | 有些页被锁进内存 |
| l | 多进程的（使用 CLONE_THREAD, 类似 NPTL pthreads） |
| + | 位于后台的进程组 |


## route
打印路由表
>route -n

设置默认路由
>route add default gw {IP-ADDRESS} {INTERFACE-NAME}

添加或删除到指定网络的路由规则
>route {add|del} -net {NETWORK-ADDRESS} netmask {NETMASK} dev {INTERFACE-NAME}

设置或取消到指定网络为不可达
>route {add|del} -net {NETWORK-ADDRESS} netmask {NETMASK} reject

## ip
命令格式：
```
ip [ OPTIONS ] OBJECT { COMMAND | help }
常用的OPTIONS：
-s：打印更多信息（如统计信息RX/RX errors），该选项可多次使用
-f：指定协议族（inet/inet6/bridge/ipx/dnet/link），link表示不涉及任何网络协议
-r：使用系统的名字解析功能打印出DNS名字，而不是主机地址
常用的OBJECT：
address：设备上的协议（IP/IPv6）地址
link：网络设备
maddress：多播地址
route：路由表项
rule：路由规则
COMMAND：指定在OBJECT上执行的动作（如add/delete/show/list/help）
```
示例：
```
ip addr help
ip addr show
ip addr add 192.168.19.130/24 dev ens33
ip addr del 192.168.19.130/24 dev ens33

ip route help
ip route show
ip route add default via 192.168.0.196

设置mac地址：
ip link set dev eth0 address 00:0c:29:d1:2b:34
```

[Linux下ip命令手册](http://www.cnblogs.com/gx-303841541/archive/2012/10/13/2722441.html)

## fdisk
sudo fdisk /dev/sda1

## dig
dig gitlab.i.deephi.tech @127.0.0.1

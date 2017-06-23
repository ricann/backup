---
title: GDB basic usage
subtitle:
date: 2017-06-23 10:02:04
author: Ricann
head-img:
categories: tools
tags:
 - GDB
 - tools
keywords:  GDB, tools
comments: true
---
# 断点
## 设置断点
代码的指定位置中断，这个是我们用得最多的一种。设置断点的命令是break，它通常有如下方式：
 + break <function>    在进入指定函数时停住
 + break <linenum>    在指定行号停住。
 + break +/-offset    在当前行号的前面或后面的offset行停住。offiset为自然数。
 + break filename:linenum    在源文件filename的linenum行处停住。
 + break ... if <condition>    ...可以是上述的参数，condition表示条件，在条件成立时停住。比如在循环境体中，可以设置break if i=100，表示当i为100时停住程序

## 禁用断点
 + delete    删除所有断点
 + delete breakpoint [n]    删除某个断点
 + disable breakpoint [n]    禁用某个断点
 + enable breakpoint [n]    使能某个断点

## 设置观察点
在变量读、写或变化时中断，这类方式常用来定位bug。
 + watch <expr>    变量发生变化时中断
 + rwatch <expr>    变量被读时中断
 + awatch <expr>     变量值被读或被写时中断

## 设置捕捉点
捕捉点用来补捉程序运行时的一些事件。如：载入共享库（动态链接库）、C++的异常等。通常也是用来定位bug。

捕捉点的命令格式是：catch <event>，event可以是下面的内容
 + throw     C++抛出的异常时中断
 + catch     C++捕捉到的异常时中断
 + exec    调用系统调用exec时（只在某些操作系统下有用）
 + fork    调用系统调用fork时（只在某些操作系统下有用）
 + vfork    调用系统调用vfork时（只在某些操作系统下有用）
 + load 或 load <libname>     载入共享库时（只在某些操作系统下有用）
 + unload 或 unload <libname>    卸载共享库时（只在某些操作系统下有用）

## 在特定线程中中断
linespec指定了断点设置在的源程序的行号。threadno指定了线程的ID，注意，这个ID是GDB分配的，你可以通过"info threads"命令来查看正在运行程序中的线程信息：
 + break <linespec> thread <threadno>
 + break <linespec> thread <threadno> if ...

如果你不指定thread <threadno>则表示你的断点设在所有线程上面。你还可以为某线程指定断点条件。如：
 + break frik.c:13 thread 28 if bartab > lim

当你的程序被GDB停住时，所有的运行线程都会被停住。这方便你你查看运行程序的总体情况。而在你恢复程序运行时，所有的线程也会被恢复运行。

## 查看
 + 可以通过info breakpoints [n]命令查看当前断点信息
 + 可以通过info watchpoints [n]命令查看当前观察点信息

## 调试步进
在gdb中，和调试步进相关的命令主要有如下几条：
 + continue    继续运行程序直到下一个断点
 + next        逐过程步进，不会进入子函数
 + step        逐语句步进，会进入子函数
 + until        运行至当前语句块结束
 + finish    运行至函数结束并跳出，并打印函数的返回值

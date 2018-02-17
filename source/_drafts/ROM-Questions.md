---
title: ROM Questions
date: 2018-02-03 17:22:31
categories: [记录]
tags: [android, ROM]
---

整理记录一些关于ROM的问答。

<!-- more -->

# 待整理

## JNI方面

问：JNI是什么，有什么作用。
- JNI是Java Native Interface的缩写，JNI允许Java代码和其他语言的代码进行交互，作为一种双向接口，即允许Java应用程序调用原生代码，也允许原生代码调用Java代码。JNI主要应用在兼容旧的库，以及提高程序性能等方面。

问：JNI的局部引用对象表的容量上限是多少？
- 512

问：Android系统启动时，init进程的主要完成的功能？
- 解析init.rc及init.{hardware}.rc初始化脚本文件
- 监听keychord组合按键事件
- 监听属性服务
- 监听并处理子进程死亡事件


## 源码管理方面

问：接触过的rom有哪些？
- （官方，CyanogenMod源码，高通拓展方案，联发科方案，MIUI等）

问：Android系统源码中包括了上百个git仓库，如何进行代码的管理和版本控制？
- Android系统由许许多多子项目组成，不能简单地用Git进行管理，AOSP在Git的基础上建立了一套自己的代码仓库，使用工具Repo进行管理。

## Dalvlik虚拟机

问：Dalvilk虚拟机和Java虚拟机的主要区别。
- Dalvik虚拟机使用的指令是基于寄存器的，而Java虚拟机使用的指令集是基于堆栈的。它们具有不同的类文件格式以及指令集。

Java虚拟机使用class格式的类文件，一个class文件只包括一个类。

Dalvik虚拟机使用的是dex（Dalvik Executable）格式的类文件，一个dex文件可以包含若干个类，可以将各个类中重复的字符串和其它常数只保存一次，从而节省了空间，这样就适合在内存和处理器速度有限的手机系统中使用。


## 应用层方面

问：对核心应用有什么了解（Phone/Contacts/MMS/Camera/Gallery/Music/Video/Settings）

答：

问：（情景）当需要为现在研发的app，添加一个快捷启动的图标到通知栏的开关（wifi、蓝牙等），需要修改哪部分的源码，

## 系统框架方面

问题：系统框架

- Telephony
- MultiMedia
- Connectivity
- Window/View/ActivityManager
- Surface/Graphics

整体

问：为Android系统增加一个物理按键，需要有哪些过程。

答：

OTA升级

全量升级和增量升级

MakeFile编译体系概述

ps要求：

- 具备修改窗口管理服务实现自定义UI界面及自定义UI交互的能力，熟悉system UI的相关源码，如锁屏、通话、状态栏等应用的源码；
- 熟悉Git项目合作开发模式；
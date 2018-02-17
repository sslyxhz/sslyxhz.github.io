---
title: Android System Tips
date: 2018-02-03 13:26:27
categories: [tips]
tags: [android]
---

整理记录在Android系统的一些操作。

<!-- more -->

# 通用行为

## Wifi连接识别
### root设备设置
``` shell
adb root
adb remount
adb shell busybox vi system/build.prop
# 添加命令
service.adb.tcp.port=5555
# 设备重启
reboot
```
### 系统启动脚本
在init.rc中添加设置。


# 硬件相关

## USB接线识别
``` shell
sudo apt-get install android-tools-adb      # 安装 adb 工具

mkdir -p ~/.android
vi ~/.android/adb_usb.ini                   # 添加一行设备标识 0x2207

sudo vi /etc/udev/rules.d/51-android.rules
# 加入udev规则，添加以下一行：
# SUBSYSTEM=="usb", ATTR{idVendor}=="2207", MODE="0666"

# 刷新规则，重新拔插数据线，或者运行以下命令
sudo udevadm control --reload-rules
sudo udevadm trigger

# 重启
sudo adb kill-server
adb start-server
```
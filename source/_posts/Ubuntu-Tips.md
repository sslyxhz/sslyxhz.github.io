---
title: Ubuntu Tips
date: 2018-01-28 00:59:17
categories: [tips]
tags: [ubuntu]
---

记录在Ubuntu上用到的一些操作。

<!-- more -->

# tips-应用篇

## 下载百度云网盘文件
实测wget可用，速度维持在70~100kb；aria2失败；
```
wget -c --referer="百度云分享链接" -O 保存的文件名 "百度云实际下载地址"
```
参考：http://blog.csdn.net/cuihaolong/article/details/78625507

## ZSSH文件传输
### 上传本地文件到服务器
在服务器上先cd至相应要放上传文件的目录之后
``` shell
rz -bye                 # 在远程服务器的相应目录上运行此命令,表示做好接收文件的准备
ctrl+@                  # 运行上面命令后,会出现一些乱码字符,不要怕,按此组合键,进入zssh
zssh >                  # 这里切换到了本地机器
zssh > pwd              # 看一下本地机器的目录在那
zssh > ls               # 看一下有那些文件
zssh > sz 123.txt       # 上传本地机器的当前目录的123.txt到远程机器的当前目录
```
### 下载服务器文件到本地
``` shell
sz filename             # 在远程机器上,启动sz, 准备发送文件
ctrl+@                  # 看到一堆乱码,不要怕,这会按下组合键
zssh > pwd              # 看看在那个目录,cd 切换到合适的目录
zssh > rz -bye          # 接住对应的文件
```
参考：https://www.bbsmax.com/A/o75N40WzW3/

## 桌面分享
1. Dash, Desktop Sharing，allow desktop sharing.
2. sudo apt-get install dconf-editor
3. dconf-editor
4. org->gnome->desktop->remote-access, set option requre-encryption false
5. run vnc viewer in windows

# tips-系统篇

## 用户管理
``` shell
# 1. 添加用户
sudo adduser xxx

# 2. 管理员权限
sudo vim /etc/sudoers
root ALL=(ALL) ALL
xxx ALL=(ALL) ALL
```

## 配置DNS服务地址
安装ubuntu16.04，网卡驱动加载但无法联网，配置dns服务器地址后正常。
``` shell
vi /etc/network/interfaces
# 1.添加dns-nameserver 114.114.114.114

vi /etc/resolvconf/resolv.conf.d/base
# 2. 追加nameserver 114.114.114.114

vi /etc/resolv.conf
# 3. 添加 nameserver 114.114.114.114

# 4. 重启网络
```

## 主目录中文转英文
有几次装了中文版Ubuntu系统，主目录下默认是中文名称目录，于是有了转回英文目录名称的想法。
``` shell
export LANG=en_US
xdg-user-dirs-gtk-update  # 弹窗询问是否将目录转化为英文路径,同意并关闭.

export LANG=zh_CN
# 关闭终端,并注销或重启.
# 下次进入系统,系统会提示是否把转化好的目录改回中文.选择不许要并且勾上不再提示,并取消修改.主目录的中文转英文就完成了
```

## 切换命令行模式
### 动态切换命令行模式
- 图形桌面--->命令行模式：Ctrl+Alt+F1/F2/F3/F4/F5/F6
- 命令行模式--->图形桌面：Ctrl+Alt+F7
- 解除命令行模式锁定光标快捷键：Ctrl+Alt

### 默认命令行模式
``` shell
sudo vi /etc/default/grub
# 注释掉 GRUB_CMDLINE_LINUX_DEFAULT=”quiet” 这行
# 把GRUB_CMDLINE_LINUX=”" 改为 GRUB_CMDLINE_LINUX=”text”
# 去掉 #GRUB_TERMINAL=console 的注释，即 GRUB_TERMINAL=console

sudo update-grub

sudo systemctl set-default multi-user.target # 开机后进入命令行界面
#sudo systemctl set-default graphical.target # 开机后进入图形界面

sudo reboot

sudo systemctl start lightdm # 切换回来  startx
```

## 安装桌面环境
``` shell
# 法一
sudo apt-get install xfce4
sudo apt-get install xubuntu-desktop # 较多其他安装包

# 法二
sudo apt-get install gnome-desktop-environment 
# or: sudo apt-get install ubuntu-desktop**
# or: sudo apt-get install —no-install-recommends ubuntu-desktop
# then, reboot

# 法三
sudo apt-get install xinit
sudo apt-get install gdm
sudo apt-get install kubuntu-desktop
sudo apt-get install —no-install-recommends kubuntu-desktop
```

# tips-巨坑篇

## 在某HP台式机上安装Ubuntu14.04的坑
### desc
Ubuntu14.04使用USB安装时，出现"install ubuntu/ try ubuntu without installation"选择，但是Enter安装时，显示器没有内容显示，片刻之后直接休眠。
### reason
由于ubuntu对于显卡支持有问题，需要手动添加显卡驱动选项，启动的时候告诉内核不要加载显卡。
- 无效一，有的说是UEFI的问题，关了UEFI不行；
- 无效二，有的说是N卡不兼容，禁用了显卡还不行。
### solution
#### step 1
安装时选择"install ubuntu"后，按"e"进入编辑模式，进入命令行模式, 然后去掉"—"后，依照不同显卡进行不同显卡驱动选项的添加
1.Intel 82852/82855 或8系列显示晶片：i915.modeset=1或i915.modeset=0
2.Nvidia：**nomodeset**
3.其它厂牌(如ATI，技嘉)：xforcevesa或radeon.modeset=0 xforcevesa
[DELL T3400显卡为Nvidia FX580，选择nomodeset]
F10安装
#### step 2
当安装结束后，启动系统出现黑画面
1.开机，进入grub画面(如果硬碟没有别的OS,请开机时按住shift不放才会有grub画面)
** (然而就是卡在这一步，没有grub画面！折腾了两次修复grub的设置都是失败，不然后面的都是一样了。)
2.按'''e''' 进入编辑开机指令的模式, 同样找到'''quite splash''' 并在后面加上对应的字。
3.按 ''F10''启动系统.
4.进去系统之后编辑'''/etc/default/grub''' 这个档案(要管理者权限sudo)。
Ubuntu>打开终端机，输入sudo vi /etc/default/grub
5.找到这一行:
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
修改为：
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash **nomodeset**"
6. 更新GRUB： sudo update-grub
7.存档，并重新开机。
### 参考
- [Ubuntu安装时出现黑屏问题的解决](http://www.linuxidc.com/Linux/2017-01/139318.htm)
- [Windows7硬盘安装Ubuntu14.04引导后黑屏解决方案](http://blog.csdn.net/ubunfans/article/details/46544175/)
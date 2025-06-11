---
title: Firefly-RK3399编译记录
pubDate: 2017-11-17
categories: ['Articles']
tags: [android, firefly, rk3399]
description: '一块板子一份源码二话不说就是怼，南墙不挡头铁。'
---


# 环境搭建

## 操作系统
- 推荐64位Ubuntu，[[官网](http://www.ubuntu.org.cn/download/desktop)](http://www.ubuntu.org.cn/download/desktop)
- 以UltraISO写入U盘，bios设置优先启动进行安装，略。

## 换源
提高国内下载速度（惯用我科 [https://lug.ustc.edu.cn/wiki/mirrors/help/ubuntu）](https://lug.ustc.edu.cn/wiki/mirrors/help/ubuntu）)
``` shell
# 法一：
sudo sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list

# 法二：
# 直接编辑 /etc/apt/sources.list 文件，在开头添加：

# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial main main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
# 预发布软件源，不建议启用
# deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
```

## 设置JDK
Openjdk，需要另外添加该源。
``` shell
sudo add-apt-repository ppa:openjdk-r/ppa
# 当add-apt-repository不可用，执行sudo apt-get install software-properties-common
sudo apt-get update
sudo apt-get install openjdk-7-jdk      # 6.0用openjdk-7
# sudo apt-get install openjdk-8-jdk    # 7.1.1用openjdk-8
```

安装结束后输入java、javac、java -version验证是否设置完毕，如果出现问题需要追加配置信息：
``` shell
# sudo gedit /etc/profile
# 以下内容追加在文件末尾
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
export JRE_HOME=${JAVA_HOME}/jre 
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib 
export PATH=${JAVA_HOME}/bin:$PATH

# source /etc/profile      # 刷新
```
      
## 环境依赖

编译Android系统，需要依赖以下项目，
``` shell
# Ubuntu14.04，Android6.0
sudo apt-get install git-core gnupg flex bison gperf libsdl1.2-dev \
libesd0-dev libwxgtk2.8-dev squashfs-tools build-essential zip curl \
libncurses5-dev zlib1g-dev pngcrush schedtool libxml2 libxml2-utils \
xsltproc lzop libc6-dev schedtool g++-multilib lib32z1-dev lib32ncurses5-dev \
lib32readline-gplv2-dev gcc-multilib libswitch-perl

# Ubuntu14.04，Android7.1
sudo apt-get install git-core gnupg flex bison gperf libsdl1.2-dev \
libesd0-dev libwxgtk2.8-dev squashfs-tools build-essential zip curl \
libncurses5-dev zlib1g-dev pngcrush schedtool libxml2 libxml2-utils \
xsltproc lzop libc6-dev schedtool g++-multilib lib32z1-dev lib32ncurses5-dev \
lib32readline-gplv2-dev gcc-multilib libswitch-perl

# Ubuntu16.04, 与14.04有所不同，此处只验证对Android6.0编译可用
sudo apt-get install -y git flex bison gperf build-essential libncurses5-dev:i386 
sudo apt-get install libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-dev g++-multilib 
sudo apt-get install tofrodos python-markdown libxml2-utils xsltproc zlib1g-dev:i386 
sudo apt-get install dpkg-dev libsdl1.2-dev libesd0-dev
sudo apt-get install git-core gnupg flex bison gperf build-essential  
sudo apt-get install zip curl zlib1g-dev gcc-multilib g++-multilib 
sudo apt-get install libc6-dev-i386 
sudo apt-get install lib32ncurses5-dev x11proto-core-dev libx11-dev 
sudo apt-get install lib32z-dev ccache
sudo apt-get install libgl1-mesa-dev libxml2-utils xsltproc unzip m4

# 通用项，交叉编译工具链
sudo apt-get install gcc-arm-linux-gnueabihf lzop libncurses5-dev libssl1.0.0 libssl-dev
```

另，可以在 .bashrc文件末尾追加，提高编译效率
``` shell
echo export USE_CCACHE=1 >> ~/.bashrc
```

# Source Code

## 源码压缩包下载
云盘地址：
http://pan.baidu.com/s/1o7Cdilk#list/path=%2FPublic%2FDevBoard%2FFirefly-RK3399%2FSource%2FAndroid6.0&parentPath=%2FPublic%2FDevBoard


``` shell
# 验证MD5
md5sum /project/Firefly-RK3399_Android6.0_git_20170310.tar.gz

# 解压缩
cd ~/project/firefly-rk3399
tar xzvf Firefly-RK3399_Android6.0_git_20170218.tar.gz
# 7z x /path/to/Firefly-RK3399_Android7.1.1_git_20170518.7z

# 还原代码
git reset --hard
```

## 线上源码
源码：
https://gitlab.com/TeeFirefly/FireNow-Marshmallow/tree/Firefly_RK3399

``` shell
# 都开始编译了才发现有线上源码是不是傻...
git remote rm origin
git remote add gitlab [https://gitlab.com/TeeFirefly/FireNow-Marshmallow.git](https://gitlab.com/TeeFirefly/FireNow-Marshmallow.git)
# git remote add gitlab [https://gitlab.com/TeeFirefly/FireNow-Nougat.git](https://gitlab.com/TeeFirefly/FireNow-Nougat.git)

# 更新远程仓库
git pull gitlab Firefly_RK3399:Firefly_RK3399
```

## 高速缓存
``` shell
# 设置编译器高速缓存,提高编译效率
cd ~/workspace/RK3399
prebuilts/misc/linux-x86/ccache/ccache -M 50G
```

# 编译前填坑

## error: unsupported reloc 43
碰到报出一串error: unsupported reloc 43，尝试第一次修改。
``` 
# mydroid/art/build/Android.common_build.mk，定位到75行
ifneq ($(WITHOUT_HOST_CLANG),true)
# 改为
ifeq ($(WITHOUT_HOST_CLANG),false)
```
经过第一次修改之后发现编译还是报同样的错误，执行下面：
``` shell
cp /usr/bin/ld.gold <source_android>/prebuilts/gcc/linux-x86/host/x86_64-linux-glibc2.11-4.6/x86_64-linux/bin/ld
make update-api
make...
```

## others
其他坑参考的官方文档后基本不会碰上了，目前编译中断是因为cpu烧的停了。


# 编译方式
- 官方编译脚本(发现的太晚...)
``` shell
cd ~/project/firefly-rk3399/
./FFTools/make.sh -k -j8    # 单独编译kernel
./FFTools/make.sh -u -j8    # 单独编译uboot
./FFTools/make.sh -a -j8    # 单独编译android上层
./FFTools/make.sh -j8       # 同时编译ubooot、kernel、android
```

- 手动编译
``` shell
cd ~/project/firefly-rk3399/kernel/
      
# 编译kernel
make ARCH=arm64 firefly_defconfig
make -j8 ARCH=arm64 rk3399-firefly.img
      
# 编译uboot：
make rk3399_box_defconfig
make ARCHV=aarch64 -j8
      
# 编译android：
source build/envsetup.sh
lunch rk3399_firefly_box-userdebug
make -j8
./mkimage.sh
```

备注：8为同时编译的线程数，一般google推荐这个数字为2倍的cpu个数再加上2，比如4核，就是10。
``` shell
# 查看CPU个数
cat /proc/cpuinfo
```

# 编译错误记录

记录下编译的注意点和错误。

## 中断编译
如果中断后重新编译，最好make clean后再编。

## bc not found
编译kernel出现bc not found，[include/generated/timeconst.h] Error 127。
``` shell
sudo apt-get install bc
```

## Communication error with Jack server (52)
``` shell
# Error
Building with Jack: out/target/common/obj/JAVA_LIBRARIES/android-support-transition-res_intermediates/classes.jack
Communication error with Jack server (52). Try 'jack-diagnose'

# Solution
find . -name jack-admin
export PATH=$PATH:~/proj/firefly-rk3399/prebuilts/sdk/tools
jack-admin start-server
export JACK_SERVER_VM_ARGUMENTS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx4096m"
```

## directory xxx is not writable
``` shell
# Error
Property 'jack.dex.output.dir' (in Options): directory 'out/target/common/obj/JAVA_LIBRARIES/conscrypt_intermediates/jack-rsc' is not writable (required because 'jack.dex.output.container' (defined in Options) is set to 'dir')

# Solution
edit $HOME/.jack-server/config.properties
and set jack.server.max-service=1
```
      
## file xxx can not be created
``` shell
# Error
[ 30% 15013/48677] build out/target/common/obj/JAVA_LIBRARIES/sdk_v9_intermediates/classes.jack

FAILED: /bin/bash -c "(mkdir -p out/target/common/obj/JAVA_LIBRARIES/sdk_v9_intermediates/classes.jack.tmpjill.res ) && (unzip -qo prebuilts/sdk/9/android.jar -d out/target/common/obj/JAVA_LIBRARIES/sdk_v9_intermediates/classes.jack.tmpjill.res ) && (find out/target/common/obj/JAVA_LIBRARIES/sdk_v9_intermediates/classes.jack.tmpjill.res -iname \"*.class\" -delete ) && (JACK_VERSION=3.36.CANDIDATE out/host/linux-x86/bin/jack @build/core/jack-default.args --verbose error -D jack.import.resource.policy=keep-first -D jack.import.type.policy=keep-first -D jack.android.min-api-level=1 --import prebuilts/sdk/9/android.jar --import-resource out/target/common/obj/JAVA_LIBRARIES/sdk_v9_intermediates/classes.jack.tmpjill.res --output-jack out/target/common/obj/JAVA_LIBRARIES/sdk_v9_intermediates/classes.jack ) && (rm -rf out/target/common/obj/JAVA_LIBRARIES/sdk_v9_intermediates/classes.jack.tmpjill.res )"

1 error during configuration. Try --help-properties for help.

Property 'jack.library.output.zip' (in Options): file 'out/target/common/obj/JAVA_LIBRARIES/sdk_v9_intermediates/classes.jack' can not be created (required because 'jack.library' (defined in Options) is set to true and 'jack.library.output.container' (defined in Options) is set to 'zip')

# Solution
export ANDROID_JACK_VM_ARGS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx4096m"
```

## 'atomic' file not found
``` shell
# Error
frameworks/native/include/binder/Binder.h:20:10: fatal error: 'atomic' file not found
#include <atomic>
^
1 error generated.

# Solution
make clean and rebuild.
```

## Cannot allocate memory
``` shell
# Error
[  9% 4800/48682] host C++: libart_32 <= art/runtime/verifier/method_verifier.ccninja: fatal: fork: Cannot allocate memory

# Solution
解决，调低参数

export JACK_SERVER_VM_ARGUMENTS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx4096m"
export ANDROID_JACK_VM_ARGS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx4096m"
```

## Waiting for unfinished jobs
``` shell
# Error
make: *** [out/target/product/rk3399_firefly_box/obj/STATIC_LIBRARIES/libLLVMScalarOpts_intermediates/ADCE.o] Error 1
make: *** Waiting for unfinished jobs....

make: *** [out/target/product/rk3399_firefly_box/obj/STATIC_LIBRARIES/libLLVMObject_intermediates/IRObjectFile.o] Error 1

make: *** [out/target/product/rk3399_firefly_box/obj/STATIC_LIBRARIES/libLLVMARMDisassembler_intermediates/ARMDisassembler.o] Error 1

# Solution
结束编译，clean后重编。

```

## OTA编译失败
``` shell
# Error

No RK Loader for TARGET_DEVICE rk3288 to otapackage
package add resource.img to BOOT and RECOVERY
No uboot for uboot/uboot.img to otapackage
No trust for uboot/trust.img to otapackage
No charge for uboot/charge.img to otapackage
No parameter for TARGET_DEVICE rk3288 to otapackage
Package target files: out/target/product/rk3288/obj/PACKAGING/target_files_intermediates/rk3288-target_files-eng.guochongxin.zip
building image from target_files RECOVERY...
Traceback (most recent call last):
File "./build/tools/releasetools/make_recovery_patch", line 68, in
main(sys.argv[1:])
File "./build/tools/releasetools/make_recovery_patch", line 39, in main
input_dir, "RECOVERY")
File "/home/guochongxin/rk/rk3288_5.1/build/tools/releasetools/common.py", line 411,in GetBootableImage
info_dict)
File "/home/guochongxin/rk/rk3288_5.1/build/tools/releasetools/common.py", line 365, in BuildBootableImage
p4 = Run(sign_cmd)
File "/home/guochongxin/rk/rk3288_5.1/build/tools/releasetools/common.py", line 86, in Run
return subprocess.Popen(args, kwargs)
File "/usr/lib/python2.7/subprocess.py", line 679, in init
errread, errwrite)
File "/usr/lib/python2.7/subprocess.py", line 1249, in _execute_child
raise child_exception
OSError: [Errno 2] No such file or directory
make: * [out/target/product/rk3288/obj/PACKAGING/target_files_intermediates/rk3288-target_files-eng.guochongxin.zip] Error 

# Solution
drmsigntool没有编译进去，

cd build/tools/drmsigntool/
mm -B
cd ~/workspace/RK3399
make otapackage

```

# 打包固件
- 编译到打包
``` shell
source build/envsetup.sh
lunch rk3399_**
./FFTools/make.sh -j12
    
./mkimge.sh ota
make otapackage
#[100% 234/234] 在out/target/product/xxxx/生成xxxx.zip本地OTA升级包
    
# 打包统一固件，rockdev/Image-rk3399_firefly_box/update.img
./FFTools/mkupdate/mkupdate.sh update
```
- Windows下打包，不常用
    - 将编译生成的文件拷贝到 AndroidTool 的 rockdev\Image 目录中
    - 然后运行 rockdev 目录下的 mkupdate.bat 批处理文件即可创建 update.img 并存放到 rockdev\Image 目录里。

# 主要参考目录：

[官方文档 Android 6.0]([http://wiki.t-firefly.com/index.php/Firefly-RK3399/Build_android](http://wiki.t-firefly.com/index.php/Firefly-RK3399/Build_android))

[官方文档 Android 7.1](http://wiki.t-firefly.com/index.php/Firefly-RK3399/Build_android_7.1)

[Ubuntu14.04编译Android6.0]([http://www.linuxidc.com/Linux/2016-01/127292p2.htm](http://www.linuxidc.com/Linux/2016-01/127292p2.htm))

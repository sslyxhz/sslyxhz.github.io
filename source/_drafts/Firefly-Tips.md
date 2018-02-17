---
title: Firefly Tips
date: 2018-02-03 16:41:34
categories: [tips]
tags: [firefly, android]
---

firefly-rk3399上折腾的琐碎记录。
<!-- more -->

# 待整理

## Android Audio System

[第7章  深入理解Audio系统](http://wiki.jikexueyuan.com/project/deep-android-v1/audio.html)
cat /proc/asound/cards

# OTA升级
java -Xmx2048m -jar out/host/linux-x86/framework/signapk.jar -w build/target/product/security/testkey.x509.pem build/target/product/security/testkey.pk8

[http://www.cnblogs.com/yin52133/p/ota_package.html](http://www.cnblogs.com/yin52133/p/ota_package.html)

完全升级包和差分包
[https://moshuqi.github.io/2017/04/23/Android-OTA-%E7%B3%BB%E7%BB%9F%E7%9A%84%E6%9B%B4%E6%96%B0%E5%8D%87%E7%BA%A7/](https://moshuqi.github.io/2017/04/23/Android-OTA-%E7%B3%BB%E7%BB%9F%E7%9A%84%E6%9B%B4%E6%96%B0%E5%8D%87%E7%BA%A7/)

过程：
- 打补丁
``` shell
git apply xxx.patch  （上面附件的补丁）
```
- 在源码根目录
``` shell
make installclean
source build/envsetup.sh
make -j4
source buildenvsetup.sh
cd build/tools/drmsigntool/
mm -B
```
- 返回源码根目录：
``` shell
./mkimage.sh ota
make otapackage
```
这样后，会在 out/target/product/xxxx/ 生成 xxxx.zip 文件，即本地OTA 升级包；


# 开关机动画

[http://dev.t-firefly.com/forum.php?mod=viewthread&tid=185&highlight=%BF%AA%BB%FAlogo](http://dev.t-firefly.com/forum.php?mod=viewthread&tid=185&highlight=%BF%AA%BB%FAlogo)

[http://www.cnblogs.com/jasonleeee/p/3967768.html](http://www.cnblogs.com/jasonleeee/p/3967768.html)

开机LOGO

[http://bbs.gfan.com/android-7919037-1-1.html](http://bbs.gfan.com/android-7919037-1-1.html)

[http://cubie.cc/forum.php?mod=viewthread&tid=4816](http://cubie.cc/forum.php?mod=viewthread&tid=4816)

替换Launcher

[http://blog.csdn.net/mirkerson/article/details/18840383](http://blog.csdn.net/mirkerson/article/details/18840383)

# 
---
author: sslyxhz
pubDatetime: 2017-12-10T12:13:24Z
modDatetime: 2017-12-11T09:09:06Z
title: Leanote源码部署记录
slug: leanote-deploy-note
featured: false
draft: false
tags:
  - software
  - selfhost
description:
  Leanote，开源笔记，在苛求数据安全的时候就得自建笔记服务了。
---


# 准备工作
- go1.9.2.linux-amd64.tar.gz
	地址：https://pan.baidu.com/s/1hs1qX5i
- leanote-all-master.zip
	地址：https://pan.baidu.com/s/1eRJvFkY
- mongodb-linux-x86_64-3.0.1.gz
	地址：https://pan.baidu.com/s/1i4MD2M5
	或：https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.1.tgz

```
# 下载百度云分享文件
$> wget -c  —referer=http://pan.baidu.com/s/xxxx -O  filename "url"
```

# 安装golang
```
$> cd /home/xhz/leanote
$> tar -xzvf go1.9.2.linux-amd64.tar.gz

$> mkdir /home/xhz/leanote/gopackage
$> sudo vim /etc/profile
export GOROOT=/home/xhz/leanote/go
export GOPATH=/home/xhz/leanote/gopackage
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
$> source /etc/profile

# test
$> go version
```

# 获取Revel和Leanote的源码
```
$> cd /home/xhz/leanote
$> unzip leanote-all-master.zip
$> cp -R leanote-all-master/src/ gopackage/

$> go install github.com/revel/cmd/revel
```

# 安装Mongodb
```
# 解压缩
$> mv mongodb-linux-x86_64-3.0.1.gz mongodb-linux-x86_64-3.0.1.tgz
$> tar -xzvf mongodb-linux-x86_64-3.0.1.tgz

# 设置环境变量
$> sudo vim /etc/profile
export PATH=$PATH:/home/xhz/leanote/mongodb-linux-x86_64-3.0.1/bin
$> source /etc/profile

# 启动mongodb, 加载初始数据
$> mkdir /home/xhz/leanote/data
$> mongod --dbpath /home/xhz/leanote/data
$> mongorestore -h localhost -d leanote --dir /home/xhz/leanote/gopackage/src/github.com/leanote/leanote/mongodb_backup/leanote_install_data

$> mongo
> show dbs #　查看数据库
leanote	0.203125GB
local	0.078125GB
> use leanote # 切换到leanote
switched to db leanote
> show collections # 查看表
files
has_share_notes
note_content_histories
note_contents
....

```

# 运行
```
$> revel run github.com/leanote/leanote

```
访问：http://localhost:9000
http://10.4.230.97:9000/

user1 username: admin, password: abc123
user2 username: demo@leanote.com, password: demo@leanote.com (仅供体验使用)

#  参考
https://github.com/leanote/leanote/wiki/Leanote-%E6%BA%90%E7%A0%81%E7%89%88%E8%AF%A6%E7%BB%86%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B----Mac-and-Linux
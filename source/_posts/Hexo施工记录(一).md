---
title: Hexo施工记录（一）
date: 2016-07-01 20:58:56
category: [记录]
tags: [software, hexo, 挖坑]
---

其一 安装篇
hexo，博客模板框架，基于nodejs。

<!-- more -->

# 前置工作
## 下载安装
- Git（http://git-scm.com/download/）
- nodejs（https://nodejs.org/en/）

## 配置Git
``` shell
# 配置用户名和邮件
$ git config --global user.name "sslyxhz"
$ git config --global user.email "sslyxhz@hotmail.com"

# 创建SSH Key
$ ssh-keygen -t rsa -C "sslyxhz@hotmail.com"
```
登陆GitHub->Settings->“SSH Keys”->“Add SSH Key”


# 安装设置
## 安装Hexo
``` shell
# Hexo主体
mkdir sslyxhz.github.io
cd sslyxhz.github.io
npm install -g hexo-cli
npm install hexo --save
```

## Hexo插件 
``` shell
npm install hexo-generator-index --save
npm install hexo-generator-archive --save
npm install hexo-generator-category --save
npm install hexo-generator-tag --save
npm install hexo-generator-feed --save
npm install hexo-server --save
npm install hexo-deployer-git --save
npm install hexo-deployer-heroku --save
npm install hexo-deployer-rsync --save
npm install hexo-deployer-openshift --save
npm install hexo-renderer-marked --save
npm install hexo-renderer-stylus --save

# 部署插件（推荐，git形式备份blog源码）
npm install git+git@github.com:hexojs/hexo-deployer-git.git --save

```
## 参数说明
- --save，将依赖保存到根目录package.json，以后执行npm install即可自动下载package.json中的所有依赖

## 错误情况
- npm WARN notsup Not compatible with your operating system or architecture: fsevents@1.1.2
  - 尝试加上参数--no-optional解决，形如：npm install xxx --no-optional


# 运行部署
## 基础运行
``` shell
# 生成静态站点文件
hexo generate

# 运行服务器 默认访问站点: http://localhost:4000
hexo server
```

## 部署
部署前需要设置根目录的配置文件
```
# _config.yml
deploy:
  type: git
  repo: https://github.com/sslyxhz/sslyxhz.github.io.git
  branch: master
```
``` shell
# 部署命令
hexo deploy
```

# 基本操作
## 新建博文
```
hexo new [layout] "postName"
```
layout：查看scaffolds目录

| Layout |   说明   |
|--------|---------|
|  post  |   默认   |
|  page  |   页面   |
|  draft |   草稿   |

## 命令缩写
- hexo n == hexo new
- hexo g == hexo generate
- hexo s == hexo server
- hexo d == hexo deploy

## 复合命令
- hexo deploy -g
- hexo server -g

# 插件拾遗
## git+git部署（推荐）
通过git发布页面内容和备份blog源码，经过一步hexo d发布到位。

``` shell
# 安装插件
npm install git+git@github.com:hexojs/hexo-deployer-git.git --save
```
调整根目录配置文件，将源码备份到src分支：
```
# _config.yaml
deploy:
  - type: git
    repo: git@github.com:<username>/<username>.github.io.git
    branch: master
  - type: git
    repo: git@github.com:<username>/<username>.github.io.git
    branch: src
    extend_dirs: /
    ignore_hidden: false
    ignore_pattern:
        public: .
```
参考：https://github.com/hexojs/hexo-deployer-git
备注：当无法推送成功时，尝试删除.deploy_git目录，重新部署。

## 后台管理（可选）
配置后台管理插件，直接通过web页面管理编辑博客信息。
``` shell
npm install --save hexo-admin
hexo server -d
open http://localhost:4000/admin/
```
```
# _config.yml
admin:
  username: myfavoritename
  password_hash: be121740bf988b2225a313fa1f107ca1
  secret: a secret something
```
参考：https://github.com/jaredly/hexo-admin

## 资源贴图
1. 调整配置
```
# _config.yml
  post_asset_folder: true
```
2. 安装插件
``` shell
npm install hexo-asset-image --save
```
3. 引用图片
```
![new_year](new_year.jpg)
```

## 图床贴图
插件：七牛云
``` shell
npm install --save hexo-admin-qiniu 
```
https://github.com/xbotao/hexo-admin-qiniu
https://xbotao.github.io/hexo-admin-qiniu/

## 轮播图
```
# Fancybox
---
title: Hexo施工记录（一）
date: 2016-07-01 20:58:56
photos:
- https://78.media.tumblr.com/be85ef76868a71471b946247f025d835/tumblr_osv3pcQfz81slhhf0o1_1280.jpg
- https://78.media.tumblr.com/910a65ded4a7e13c807e43b1bc3edbae/tumblr_oqnsfaw6MN1slhhf0o1_1280.jpg
---
```

## RSS
``` shell
npm install hexo-generator-feed
```
```
# themes\next_config.yml
rss：
```
启动服务器，查看 http://localhost:4000/atom.xml

## Sitemap
``` shell
npm install hexo-generator-sitemap
```
启动服务器，查看 http://localhost:4000/sitemap.xml

## 顺序图和流程图
### 安装插件：
``` shell
npm install --save hexo-filter-sequence
npm install --save hexo-filter-flowchart
```
### 配置选项：
``` 
# 配置hexo目录下，而非主题下的_config.yml

sequence:
  # webfont:     # optional, the source url of webfontloader.js
  # snap:        # optional, the source url of snap.svg.js
  # underscore:  # optional, the source url of underscore.js
  # sequence:    # optional, the source url of sequence-diagram.js
  # css: # optional, the url for css, such as hand drawn theme 
  options: 
    theme: 
    css_class: 

flowchart:
  # raphael:   # optional, the source url of raphael.js
  # flowchart: # optional, the source url of flowchart.js
  options: # options used for `drawSVG` 
```
### 简单示例
参考源码说明：
- [sequence](https://github.com/bubkoo/hexo-filter-sequence)
- [flowchart](https://github.com/bubkoo/hexo-filter-flowchart)




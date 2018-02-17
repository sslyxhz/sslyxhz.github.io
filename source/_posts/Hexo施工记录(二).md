---
title: Hexo施工记录（二）
date: 2016-07-12 10:48:19
category: [记录]
tags: [software, hexo, 挖坑]
---

其二 主题篇
hexo，博客模板框架，基于nodejs。

<!-- more -->

# 主题 - Next
采用了iissnan的Next主题，http://theme-next.iissnan.com/

## 下载
```
$ cd sslyxhz.github.io
# 法一
$ git submodule add https://github.com/iissnan/hexo-theme-next themes/next
# 法二
git clone https://github.com/iissnan/hexo-theme-next themes/next

cd themes/next
# 备份主题配置文件_config.yml
git pull  
```

## 应用主题
```
# themes/next/_config.yml
theme: next
```

# 页面设置
## 标签页面
``` shell
cd sslyxhz.github.io
hexo new page tags
```
内容如下：
```
---
title: 标签
date: 2016-07-01 21:13:23
type: "tags"
comments: false
---
```

## 分类页面
``` shell
$ cd myblog
$ hexo new page categories
```
内容如下：
```
---
title: 分类
date: 2016-07-01 21:17:56
type: "categories"
comments: false
---
```

## 404页面
- 新建页面：404.html
- 可用404公益：http://www.qq.com/404/
- 404页面部署
  - 注，关于默认页面跳转404，在github上不生效，通过nginx来访问没有问题。
  - 部署在github上则参考：https://help.github.com/articles/creating-a-custom-404-page-for-your-github-pages-site/

# 评论模块
Disqus要翻墙，畅言要备案，尝试Gitment。
Gitment使用Github Issues作为评论系统。
## OAuth application注册接入
地址：https://github.com/settings/applications/new
- Application name：随意
- Homepage URL：https://sslyxhz.github.io
- description: 随意
- Authorization callback URL: https://sslyxhz.github.io
> callback URL必须正确。
## 主题配置
主题Next已经支持Gitment，简单配置即可。
``` 
# themes/next/_config.yml
gitment:
  enable: true
  lazy: false # Comments lazy loading with a button
  github_user: sslyxhz # MUST HAVE, Your Github ID
  github_repo: sslyxhz.github.io # MUST HAVE, The repo you use to store Gitment comments
  client_id: xxxxxxxxxxxxxx # MUST HAVE, Github client id for the Gitment
  client_secret: xxxxxxxxx # EITHER this or proxy_gateway, Github access secret token for the Gitment
  ...
```

# 统计模块
## 谷歌统计
- 注册Google Analytics， http://www.google.cn/intl/zh-CN_ALL/analytics/learn/index.html
```
# _config.yml
google_analytics: UA-[TrackingID]-1
```

# 搜索模块
## 站内搜索
Swiftype等搜索存在试用期，使用Hexo集成的搜索引擎。
```
npm install hexo-generator-search --save
```
```
# /themes/next/_config.yml
local_search:
  enable: true
  trigger: auto
  # show top n results per article, show all results by setting to -1
  top_n_per_article: 1
```

## 阅读次数、访问量等
```
# /themes/next/_config.yml
busuanzi_count:
  enable: true
  site_uv: true
  site_uv_header: <i class="fa fa-user"></i> 访问人数
  site_uv_footer:
  site_pv: true
  site_pv_header: <i class="fa fa-eye"></i> 总访问量
  site_pv_footer: 次
  page_pv: true
  page_pv_header: <i class="fa fa-file-o"></i> 浏览
  page_pv_footer: 次
```

## 友情链接
TODO

## 域名指向
TODO

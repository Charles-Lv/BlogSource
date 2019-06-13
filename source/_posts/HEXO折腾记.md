---
title: HEXO折腾记
date: 2017-06-22 23:14:45
tags:
---


>前言：最近瞎逛的时候看到HEXO，之前的博客用的GHOST。不过GHOST需要服务器，感觉还是HEXO比较简便，所以就瞎折腾了下。虽然同为NODE平台下的博客框架，不过确实HEXO比较简单，想当初GHOST折腾了我好几天。不过主要也是因为HEXO搭了GitHub Pages这趟便车，不需要去折腾服务器。下面附上搭建过程，以下过程基于windows平台，站点发布于GitHub Pages。有相关问题可以访问[HEXO官网](https://hexo.io/zh-cn/),里面又中文版教程，并且过程特别清晰
# 博客搭建过程
## Hexo
1. Hexo是一款基于Node.js的静态博客框架,所以NODE.js当然是必装的，到Node.js官网下载相应平台的最新版本，一路安装即可。
2. Git也是必须，要将相关代码上传到Github。

接下去新建站点

```
$ hexo init <folder>
$ cd <folder>
$ npm install
```
新建完成后，指定文件夹的目录如下：

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
站点_config.yml配置文件

```
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: 吕焕的博客
subtitle:
description: 诸法所生 唯心所现
author: 吕焕
language: zh-Hans
timezone:
#NEXT主题的头像地址
avatar: images/avatar.png
#NEXT主题，多说评论系统配置项
duoshuo_shortname: lvhuan

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://www.hexo.lvhuan.me
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:

# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: 
  repo: 

```
主题替换为NEXT，[NEXT官网](http://theme-next.iissnan.com/)中有详细的教程。

其实就是把NEXT的代码下过来，放在站点目录中的themes文件夹下的next文件夹中，然后将站点_config.yml中的theme改为next。

NEXT可以集成很多第三方的插件，比如多说评论、百度统计以及阅读量统计等，具体设置请参照官方教程。

然后执行下面的命令后成功后，再浏览器里输入`http://localhost:4000/`应该就可以看到博客的内容了。

```
$ hexo server
```

##上传博客到GitHub Pages服务

*前提条件：在GitHub服务器中已经建立GitHub Pages所需代码库（GitHub中建立一个仓库，名称为`GitHub用户名.github.io`）。博客的静态文件上传到这个代码库后，会自动解析，然后在`GitHub用户名.github.io`这个域名下就能看到你的博客了*


### 方案一：本地上传静态文件

安装 hexo-deployer-git。

```
$ npm install hexo-deployer-git --save

```

修改站点_config.yml配置


```
deploy:
  type: git
  repo: <repository url>
  branch: [branch]
  message: [message]
```


参数| 描述
---|---
`repo` | 库（Repository）地址
`branch`| 分支名称。如果您使用的是 GitHub 或 GitCafe 的话，程序会尝试自动检测。
`message` | 自定义提交信息 (默认为 Site updated: 'YYYY-MM-DD HH:mm:ss')

然后执行HEXO的发布命令，就能将编译好的静态文件发布到GitHub Pages代码库

```
$ hexo deploy 
```

可以简写成 

```
$ hexo d
```

参数| 描述
---|---
`-g, --generate` | 部署之前预先生成静态文件

### 方案二：将所有的博客源文件上传到GitHub，然后通过[travis-ci](https://travis-ci.org/)执行编译静态文件，然后发布到GitHub Pages代码库


---
title: "Hugo博客搭建配置"
date: 2019-10-14T21:30:06+08:00
categories:
- hugo
tags:
- hugo
- 博客搭建
- 博客配置
keywords:
- hugo
- 博客
- 搭建
- 配置
autoThumbnailImage: false
thumbnailImagePosition: "top"
thumbnailImage: /img/hugo/rabstractoilpainting0.jpg
coverImage: /img/hugo/abstractoilpainting0.jpg
metaAlignment: center
---
很早之前就想自己搭建自己的个人博客，手上也有服务器、域名，一直拖到现在才开始做，记录下流程，以免以后忘了
# 下载安装hugo

## Mac
```shell
brew install hugo
```
安装完查看hugo版本
```shell
hugo version
```
顺带安装上golang
```shell
brew install go
```
安装完查看版本确认安装上
```shell
go version
```

## Linux
我用的是ubuntu18.04
```shell
sudo apt-get install hugo
```
使用命令安装可能不是最新版，可以去hugo github网站下载最新版本
```shell
#使用wget下载
wget https://github.com/gohugoio/hugo/releases/download/v0.58/hugo_0.58.3_Linux-64bit.deb
#安装
 sudo dpkg -i hugo_0.58.3_Linux-64bit.deb
```
顺带安装上golang
```
sudo apt-get install golang
```
# 创建博客
使用命令创建博客
```shell
hugo new site myBlog
```
在myBlog文件夹下会生成以下目录
```shell
.
├── archetypes # 存放生成博客的模版
├── assets # 存放被 Hugo Pipes 处理的文件
├── config # 存放 hugo 配置文件 支持 JSON YAML TOML 三种格式配置文件
├── content # 存放 markdown 文件
├── data # 存放 Hugo 处理的数据
├── layouts # 存放布局文件
├── static # 存放静态文件 图片 CSS JS文件
└── themes # 存放主题
```

# 下载主题
前往[hugo官网](https://themes.gohugo.io/)下载主题
选择一个主题进入，会有教如何安装下载主题
这是我选的主题[Tranquilpeak](https://themes.gohugo.io/hugo-tranquilpeak-theme/)
```shell
#在新建的博客文件夹根目录下
cd themes
git clone https://github.com/kakawait/hugo-tranquilpeak-theme.git
```
我之前mac更新后使用git出错，查询需要安装xcode插件
```shell
xcode-select --install
```
使用git下载比较慢的情况，建议修改镜像源或者挂VPN，我是使用海外的VPS下载的

博客主题的具体使用修改可以查看[用户手册](https://github.com/kakawait/hugo-tranquilpeak-theme/blob/master/docs/user.md)
# 启动博客
将/themes/hugo-tranquilpeak-theme/exampleSite文件里的config.toml、content、static拷贝到博客根目录下
```shell
#博客根目录下
#-i：覆盖既有文件之前先询问用户
#-R/r：递归处理，将指定目录下的所有文件与子目录一并处理
cp -ri ./themes/hugo-tranquilpeak-theme/exampleSite/* ./
#启动服务
hugo server
```
在游览器中输入http://localhost:1313 就可以查看博客界面
# 修改配置主题
在博客根目录下找到config.toml文件

博客本地的图片应放在static文件夹下，博客内引用填写方式为/img/hugo/whale.jpg
# 创建编写博客
```shell
#创建博客
hugo new post/hugo博客.md
.
└── content
    └── post
        └── hugo博客.md
```
在根目录content文件夹下就会生成post/hugo博客.md
我试了下，md文件只有在post文件夹下才会在网页端显示
# 服务器端安装Apache服务器

# 博客网站上传服务器

# 添加评论功能

<!--more-->

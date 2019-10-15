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
autoThumbnailImage: true
thumbnailImagePosition: "top"
thumbnailImage: /img/hugo/abstractoilpainting0S.jpg
coverImage: /img/hugo/abstractoilpainting0.jpg
metaAlignment: center
---
很早之前就想自己搭建自己的个人博客，手上也有服务器、域名，一直拖到现在才开始做，记录下流程，以免以后忘了
<!--more-->[注释]:(主页显示阅读全文位置)
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
在博客根目录下找到从主题下拷贝出的config.toml文件对其进行修改，这是我的配置文件[config.toml](https://github.com/bmi269/hugoPeakTheme/blob/master/config.toml)
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
在linux服务器上安装Apache服务器
```shell
sudo apt-get install apache2
#安装apache文档与实用程序
sudo apt-get install apache2-doc apache2-utils
```

检测apache运行状态与apache版本
```shell
systemctl systemctl status
apache2 -V
```
更新防火墙，Ubuntu上最常用的防火墙是UFW，要允许通过80（http）和443（https）端口的流量
```shell
ufw allow 'Apache Full'
```
Apache2Buddy是一个可以自动调整 Apache 配置的脚本
```shell
curl -sL https://raw.githubusercontent.com/richardforth/apache2buddy/master/apache2buddy.pl | perl
```

为网站创建一个新的文件夹
```shell
#example.com是你的域名
mkdir -p /var/www/example.com/public_html
```
设置目录权限
```shell
chown -R www-data:www-data /var/www/example.com
chmod -R og-r /var/www/example.com
```
给网站创建一个新的虚拟主机
```shell
vim /etc/apache2/sites-available/example.com.conf
```
黏贴以下内容
```shell
#www.example.com是你的域名
<VirtualHost *:80>
     ServerAdmin admin@example.com
     ServerName example.com
     ServerAlias www.example.com
   
     DocumentRoot /var/www/example.com/public_html
    
     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
启用虚拟主机
```shell
a2ensite example.com.conf
```
重启apache服务器，使更改生效
```shell
systemctl restart apache2
```
*这节内容来自[如何在 Ubuntu 上安装和优化 Apache](https://linux.cn/article-9679-1.html),这片文章中有更详细的apache服务器设置*
# 博客网站上传服务器
启动hugo后会在根目录下生成一个public文件夹，将public文件夹中的所有内容上传到服务器/var/www/example.com/public_html目录下

# 添加评论功能
hugo支持多个评论插件，Disqus需要注册账号，比较懒，就选择使用gitment。Disqus请参考[Comments](https://gohugo.io/content-management/comments/)
由于我的域名还在备案所以gitment还没做,gitment请参考这个文章[Hugo 集成 Gitment 评论插件](https://www.qikqiak.com/post/hugo-integrated-gitment-plugin/)
# 参考文章链接
- [hugo官网](https://gohugo.io)
- [手把手教你从0开始搭建自己的个人博客 |第二种姿势 | hugo](https://www.bilibili.com/video/av51574688)
- [Hugo + Github Pages 搭建个人博客](https://juejin.im/post/5cc41bfef265da036505031c)
- [在Ubuntu 18.04系统中安装和使用Hugo的方法](https://ywnz.com/linuxyffq/4334.html)
- [Hugo 集成 Gitment 评论插件](https://www.qikqiak.com/post/hugo-integrated-gitment-plugin/)
- [如何在 Ubuntu 上安装和优化 Apache](https://linux.cn/article-9679-1.html)


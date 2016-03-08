---
layout: post
title: 本地运行调试jekyll网站
date: 2016-03-01 11:00:13
category: "other"
---
  在本地调试GitHub pages博客，调试完毕后再push，避免频繁push。

---------------------------------


## 1. 安装Ruby和DevKit ##

1. 下载安装Ruby；（检测是否成功安装：cmd~ruby -version）
2. 下载安装DevKit（由于国内gem安装被墙，最好下载到本地安装）
3. 安装Jekyll（检测是否成功安装：cmd~Jekyll）

## 2. 部署jekyll网站 ##

1. 从GitHub将GitHub pages项目Clone到本地；
2. 在cmd中cd到项目路径，如：C:\Users\acer\git\yxzhangbupt.github.io
3. 在cmd中输入jekyll serve，出现下图所示情况说明成功部署：

![jekyll-running](/images/jekyll-running.png)

4. 打开浏览器，输入“http://localhost:4000/”，打开网站；
5. 对文章进行修改并保存，服务器会自动更新你的改动，并实时显示；

## 注意： ##

一般我们在配置文件"_config.yml"中设置的URL是你的博客地址，因此在本地点击网站中的链接，其URL还是你的博客链接而不是本地连接，有两种解决方法：

1. 将配置文件中的URL改成“http://localhost:4000/”，完成调试后改回你的博客地址并push到GitHub；
2. 手动将链接前缀，如“http://yxzhangbupt.github.io//algorithm/2016/03/01/Jekyll-Local.html”中的“http://yxzhangbupt.github.io”改成“http://localhost:4000/”，就可以正常访问了；




原创文章转载请注明出处: [本地运行调试jekyll网站]( http://yxzhangbupt.github.io/java/2016/03/01/Jekyll-Local.html)

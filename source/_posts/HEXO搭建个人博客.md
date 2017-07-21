---
title: HEXO搭建个人博客
date: 2017-07-20 10:00:00
categories: blog 
tags: [随笔]
---

配置环境

>安装Node
>
	前往Node.js[官网]下载最新版本(https://nodejs.org/en/)安装

>安装Git
>
	前往[git Download](https://git-for-windows.github.io/)下载(安装教程配置等请自行百度)
 
<!--more-->
>申请GitHub账号

>准备工作完成后,安装HEXO　
>
    $ sudo npm install -g hexo (ubuntu系统)
    $ npm install -g hexo (windows系统)

>hexo常用的命令,#后面为注释
>
    $ hexo g #完整命令为hexo generate，用于生成静态文件
	$ hexo s #完整命令为hexo server，用于启动服务器，主要用来本地预览
	$ hexo d #完整命令为hexo deploy，用于将本地文件发布到github上
	$ hexo n #完整命令为hexo new，用于新建一篇文章
	$ hexo new "postName" #新建文章
    $ hexo new page "pageName" #新建页面
	$ hexo generate #生成静态页面至public目录
	$ hexo help  #查看帮助
	$ hexo version  #查看Hexo的版本

>初始化(创建一个文件夹，如：Blog,cd到Blog里执行hexo init命令)
>
    $ hexo init

    
>文章创建步骤
>>1.生成静态页面(继续再Blog目录下执行如下命令，生成静态页面)
>
    hexo generate
>>2.本地启动(启动本地服务，进行文章预览调试，命令)
>
    hexo server 
>>3.手动配置
>
    npm install hexo-renderer-ejs --save
	npm install hexo-renderer-stylus --save
	npm install hexo-renderer-marked --save

>>4.github需要修改配置文件_config.yml
>
	deploy:
	  type: git #类型
	  repository: git@github.com:######/######.git #ssh路径
	  branch: gh-pages #分支

>>5.使用git部署命令
>
	npm install hexo-deployer-git --save

>>6.部署步骤(更新文章等操作每次以以下步骤进行)
>
    hexo clean
	hexo generate
	hexo deploy
<!--more-->
---

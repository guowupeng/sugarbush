---
title: 更改Ubuntu默认python版本的两种方法
date: 2017-07-25 10:53:18
categories: blog 
tags: [随笔]
---

>使用 ls 命令来查看系统中都有那些 Python 的二进制文件可供使用
>
	ls /usr/bin/python*

>查看默认的 Python 版本信息
>
	python --version
 
<!--more-->
>在系统级修改 Python 版本

>使用 update-alternatives 来为整个系统更改 Python 版本。以 root 身份登录，首先罗列出所有可用的 python 替代版本信息
>
    update-alternatives --list python

>将 python2.7 和 python3.5 放入其中
>
    update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
	update-alternatives --install /usr/bin/python python /usr/bin/python3.4 2
	【--install 选项使用了多个参数用于创建符号链接。最后一个参数指定了此选项的优先级，如果我们没有手动来设置替代选项，那么具有最高优先级的选项就会被选中】

>列出可用的 Python 替代版本
>
    update-alternatives --list python

>使用下方的命令随时在列出的 Python 替代版本中任意切换
>
    update-alternatives --config python
    
>移除替代版本
>
    update-alternatives --remove python /usr/bin/python2.7

<!--more-->
---

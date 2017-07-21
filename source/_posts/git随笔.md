---
title: Git学习笔记
date: 2017-07-21 10:43:53
categories: blog 
tags: [随笔]
---

在Windows上安装Git

>前往[git Download](https://git-for-windows.github.io/)下载
 
>安装完成后，在开始菜单里找到“Git”->“Git Bash”，在命令行输入:
>
    $ git config --global user.name "your name"
    $ git config --global user.email "email@example.com"

<!--more-->
>创建版本库
>
    $ mkdir sugar
    $ cd sugar
    $ pwd(用于显示当前目录)
>通过git init 命令把目录变成Git可以管理的仓库
>
    $ git init

>把文件添加到版本库
>>1.编写文件(vi  readme.txt)
>
    Git is a version control system.
    Git is free software.
>>2.用命令git add <file>将文件添加到仓库
>
    git add  readme.txt
    注：git add <file> <file>可反复多次使用，添加多个文件
>>3.用命令git commit 将文件提交到仓库
>
    git commit -m "wrote a  readme file"
    注：-m后面输入的是本次提交的说明

>掌握工作区的状态
>
    1.使用git status命令查看仓库当前的状态
    2.使用git diff(或git diff HEAD -- file)可以查看工作区和版本库里最新版本的区别

>版本回退
>
    1.使用git log查看提交历史
    【注：git log --pretty=oneline(或git log --oneline)可减少信息显示】
>
    2.git reset --hard HEAD(当前版本),git reset --hard HEAD^(回退到上一个版本)
    【注：HEAD指向的版本就是当前版本,因此,可以使用命令git reset --hard commit_id在版本的历史之间穿梭;且往上100个版本可写成HEAD~100】
    3.使用git reflog查看命令历史

>撤销修改
>
    使用git checkout -- file丢弃工作区的修改
    使用git reset HEAD file可以把暂存区的修改撤销掉(unstage)，重新放回工作区
    【注：撤销修改前提是没有推送到远程库】

>删除文件
>
    使用rm file命令删除文件
    注意：1.确实要从版本库中删除该文件,用命令git rm file删掉,并且git commit
          2.误删文件恢复到最新版本git checkout -- file


>远程仓库
>>1.创建SSH Key
>
    $ ssh-keygen -t rsa -C "youremail@example.com（自己的邮箱地址）",接下来点击enter键即可（也可以输入密码）
    在Key文本框里粘贴id_rsa.pub文件的内容
    (注：在用户主目录下,存在.ssh目录,目录下存在id_rsa(私钥)和id_rsa.pub(公钥)这两个文件，跳过此步骤。如果不存在，打开Shell（Windows下打开Git Bash）,创建SSH Key)
>>2.添加远程仓库
>
    使用命令git remote add origin git@server-name:path/repo-name.git关联一个远程库
    (注：origin远是程库的名字,git的默认叫法,可改)
>
    关联后,使用命令git push -u origin master第一次推送master分支的所有内容
    (注：由于远程库是空的,第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令,之后可直接使用git push origin master推送)
>>3.SSH警告
>
    第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告
    The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
    RSA key fingerprint is xx.xx.xx.xx.xx.
    Are you sure you want to continue connecting (yes/no)?
    这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。
>
    Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了
    Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
    (注：这个警告只会出现一次，后面的操作就不会有任何警告了.)
>>4.从远程库克隆
>
    使用git clone git@server-name:path/repo-name.git命令克隆
    (注：Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议)



>分支管理
>>1.创建与合并分支
>
    查看分支：git branch
    创建分支：git branch <name>
    切换分支：git checkout <name>
    创建+切换分支：git checkout -b <name>
    合并某分支到当前分支：git merge <name>
    删除分支：git branch -d <name>
    
>>2.解决冲突
>
    当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
    用git log --graph(git log --graph --pretty=oneline --abbrev-commit)命令可以看到分支合并图

>>3.分支管理策略(合并分支时Git会用Fast forward模式,这种模式下,删除分支后,会丢掉分支信息)
>
    切换回master分支,并使用命令git merge --no-ff -m "merge with no-ff" <name>合并分支
    (准备合并分支时,强制禁用Fast forward模式,请使用--no-ff参数;因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去)

>>4.Bug分支
>
    使用命令git stash,可以把当前工作现场“储藏”起来,修复完bug后,再git stash pop,回到工作现场
    使用命令git stash list查看stash内容,恢复指定的stash,用命令git stash apply stash@{0}
    (注:用git stash apply恢复,stash内容并不删除,还需用git stash drop来删除;用git stash pop,恢复的同时把储藏的stash内容也删除)
>
    例:1.储藏当前工作现场(dev分支):git stash
       2.切换到需要修复的bug(master)分支:git checkout master
       3.从bug(master)分支创建临时分支(issue-101):git checkout -b issue-101
       4.修复bug后,对修改内容进行commit提交后,切换回master分支:git checkout master
       5.合并修复后的分支:git merge --no-ff -m "merged bug fix 101" issue-101
       6.删除issue-101分支:git branch -d issue-101
       7.切换回工作现场:git checkout dev
       8.查看stash内容:git stash list
       9.恢复stash内容:git stash pop(或者用命令git stash apply stash@{0}恢复指定stash,但要用git stash drop删除stash内容)

>>5.Feature分支
>   
    丢弃一个没有被合并过的分支,通过git branch -D <name>强行删除
    (注:git branch -d <name>只能删除合并后的分支)

>>6.多人协作
>
    查看远程库的信息用git remote -v(注:会显示可以抓取和推送的远程库地址,如果没有推送权限,就看不到push的地址)
>
    从本地推送分支,使用git push origin branch-name,如果推送失败,先用git pull抓取远程的新提交
>
    在本地创建和远程分支对应的分支,使用git checkout -b branch-name origin/branch-name,本地和远程分支的名称最好一致
>
    建立本地分支和远程分支的关联,使用git branch --set-upstream branch-name origin/branch-name
>
    从远程抓取分支,使用git pull,如果有冲突,要先处理冲突
    

>标签管理
>>1.创建标签
>
	命令git tag <name>用于新建一个标签,默认为HEAD,也可以指定一个commit id(git tag <name> commitId)
>
	创建带有说明的标签,用-a指定标签名,-m指定说明文字,-s用私钥签名一个标签
	指定标签信息:git tag -a <tagname> -m "blablabla..." commitId
	用私钥签名一个标签:git tag -s <tagname> -m "blablabla..." commitId
>
	命令git tag可以查看所有标签
>
	git show <tagname>查看标签信息

>>2.操作标签
>
	推送一个本地标签:git push origin <tagname>
>
	推送全部未推送过的本地标签:git push origin --tags
>
	删除本地标签:git tag -d <tagname>
>
	删除一个远程标签:git push origin :refs/tags/<tagname>
	(注:删除远程标签要先删除本地标签)

>配置别名
>
	git config --global alias.别名 checkout
>
	例:
		配置git co,切换分支:git config --global alias.co checkout
		配置git ci,提交说明:git config --global alias.ci commit
		配置git br,显示分支信息:git config --global alias.br branch
		配置git last,让其显示最后一次提交信息:git config --global alias.last 'log -1'
	    配置git lg:git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"


网站根目录权限遵循：
>
    文件 644， 文件夹 755 ，权限用户和用户组www 
    如出现文件权限问题时，请执行下面 3  条命令：
    chown -R www.www /data/wwwroot/
    find /data/wwwroot/ -type d -exec chmod 755 {} \;
    find /data/wwwroot/ -type f -exec chmod 644 {} \;
<!--more-->
---

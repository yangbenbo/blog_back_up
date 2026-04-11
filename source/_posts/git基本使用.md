---
title: git基本使用
date: 2019-07-16 17:36:13
categories:
- Linux
tags:
- git

---

<!-- @import "[TOC]" {cmd="toc" depthFrom=2 depthTo=3 orderedList=false} -->

<!-- code_chunk_output -->

- [基本操作](#基本操作)
  - [常用](#常用)
  - [远程仓库](#远程仓库)
  - [分支](#分支)
  - [冲突](#冲突)
  - [BUG分支](#bug分支)
  - [时光穿梭](#时光穿梭)
  - [feature](#feature)
  - [合并commit](#合并commit)
  - [.gitignore](#gitignore)
  - [submodule](#submodule)
- [git 遇到的问题](#git-遇到的问题)
  - [git add file](#git-add-file)
  - [将一个项目和多个远程仓库关联](#将一个项目和多个远程仓库关联)
  - [不要使用Windows自带的记事本编辑任何文本文件](#不要使用windows自带的记事本编辑任何文本文件)
  - [删除本地上已经不存在的远程分支](#删除本地上已经不存在的远程分支)
- [引用](#引用)

<!-- /code_chunk_output -->

git管理版本主要分三个地址进行层次提交
- 工作区(Working Directory):当前目录,即电脑能看到的目录
- 版本库(Repository):
    - 暂存区(stage或index):git add提交目的区域;
    - 分支(如master):git commit则会提交目的区域

##　基本操作
记住常用的几个命令就可以,其他的在需要的时候返回来查询就可以

### 常用
		git add readme.md
		git commit -m "add readme.md"
		# Auto stage all modified files and commit with a message:
		# 将暂存区中所有修改的文件commit(针对已近跟踪文件,省略了git add,新建文件没有使用 git add之前属于未跟踪文件)
		git commit -am "add readme.md"
		git diff readme.md          
		git status     
		git log         
		git log --pretty=oneline
		git diff HEAD -- readme.md    # 可以不用HEAD
		git rm test.txt     # 从版本库中删除该文件
		
		# 从暂存区恢复,可以丢弃工作区的修改 回到最近一次git commit或git add时的状态
		git checkout -- readme.md   
		# 把暂存区的修改撤销掉（unstage）,重新放回工作区,HEAD表示当前分支
		git reset HEAD readme.md    
		# 分支的回退针对已经进行了git commit操作
		git reset --hard commitid

### 远程仓库

		git remote add origin git@server-name:path/repo-name.git
		git push -u origin master   # 第一次推送master分支的所有内容(如果分支不是master则需要指明)
		git push origin master   # 可以直接使用git push
		
		# 也可以使用下面命令
		#git push -u origin --all
		#git push -u origin --tags
		# 如果本体仓库已经存在 需要先rename
		git remote rename origin old-origin


建立本地和远程分支**映射关系**后,使用`git push`或者`git pull` 时就不必每次都要指定从远程的哪个分支拉取合并和推送到远程的哪个分支了。
		
		# 从远程仓库拉取所有分支
		git branch		# 查看所有本地分支
		git branch -r	# 查看所有远程分支
		git branch -a	# 查看所有分支
		# 本地新建分支x,并自动切换到x,建立了本地和远程分支的映射关系
		git checkout -b 本地分支名x origin/远程分支名x
		# 第二种方式 不会和远程分支建立映射关系,也需要手动checkout到x
		git fetch origin 远程分支名x:本地分支名x
		# 查看映射关系
		git branch -vv



### 分支

		git branch   # 查看分支
		git branch <name>  # 创建分支
		git checkout <name>   # 切换分支
		git checkout -b <name>   # 创建+切换分支
		git merge <name>   # 合并某分支到当前分支
		git branch -d <name>   # 删除分支

### 冲突

	合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

		git log --graph --pretty=oneline --abbrev-commit  # 查看分支合并图
		git merge --no-ff -m "merge with no-ff" dev   # 普通模式合并

### BUG分支

		git stash  # 把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
		git stash list 
	你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：

		$ git stash apply stash@{0}
	修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
	当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再
		
		git stash pop  # 回到工作现场。

### 时光穿梭
		
	在Git中，用HEAD表示当前版本，也就是最新的提交版本(针对git log),上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

		git reset --hard commit_id  
		git reflog   # 查看命令历史，以便确定要回到未来的哪个版本
		# 回到之前版本发现未追踪文件,这是因为还没add 或commit就切换版本
		# 确认这些文件没用的话就可以直接clean,一般情况最好保留一个版本
		git clean -df  

### feature

		git branch -D <name>   # 丢弃一个没有被合并过的分支
	- master分支是主分支，因此要时刻与远程同步；

	- dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

	- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

	- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

### 合并commit
    - git log 提交历史,显示提交过版本信息,不包括删除的commit和reset操作
    - git reflog 命令历史,显示所有操作记录,包括提交,回退,合并等,一般用来回退
    
    合并commit针对git log
        
            git log     #查看commit记录
            git rebase -i commitid  # 这里的id是不需要合并的commit对应id
            # 进入vim模式,根据提示进行pick(执行commit)或者squash(合并commit)

### .gitignore
详细规则参考[官方英文文档](https://git-scm.com/docs/gitignore)

    - 忽略操作系统自动生成的文件，比如缩略图等；
    - 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
    - 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。
        
            /build/  # 匹配根目录下build文件夹下所有文件, 特别注意/的使用
            build/  # 匹配目录下build文件夹下所有文件,包括/child/build/
            !lib.a  # 除开lib.a, 即需要跟踪
            *.pyc   # 所有以.pyc结尾的文件 
            *.ipynb_checkpoints # JupyterNotebook创建的自动保存的文件,可以通过 “File > Revert to Checkpoint“恢复
    
    git只会跟踪文件,不会跟踪空文件夹        
    
    pyc文件是由py文件经过编译后，生成的二进制文件，是一种byte code，py文件变成pyc文件后，加载的速度有所提高，而且pyc是一种跨平台的字节码，是由Python的虚拟机来执行的
    python执行文件流程
        - 完成模块的加载和链接；
        - 将源代码翻译为PyCodeObject对象（字节码），并将其写入内存当中（方便CPU读取，起到加速程序运行的作用）；
        - 从上述内存空间中读取指令并执行；
        - 程序结束后，根据命令行调用情况（即运行程序的方式）决定是否将PyCodeObject写回硬盘当中（也就是直接复制到.pyc或.pyo文件中）；
        - 之后若再次执行该脚本，则先检查本地是否有上述字节码文件。有则执行，否则重复上述步骤。    
    
    检查.gitignore哪条规则匹配了App.class可用如下命令(感觉不是很好用)
    
        git check-ignore -v App.class

.gitignore规则不生效,只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。
解决方法就是先把本地缓存删除（改变成未track状态），然后再提交:

	git rm -r --cached .
	git add .
	git commit -m 'update .gitignore'


### submodule
有种情况我们经常会遇到：某个工作中的项目需要包含并使用另一个项目。 也许是第三方库，或者你独立开发的，用于多个父项目的库。 现在问题来了：你想要把它们当做两个独立的项目，同时又想在一个项目中使用另一个。更详细的使用请看官方文档[Git 工具 - 子模块](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)
增加子模块之后会生辰.gitmodules文件,里面是相关的配置

	git submodule add https://github.com/chaconinc/DbConnector

clone带有子模块的项目

	git clone --recurse-submodules https://github.com/chaconinc/MainProject

## git 遇到的问题

### git add file

提示modified content, untracked content是因为在add的时候这个目录下面本来就有一个.git文件，自然就会add失败，先删除这个.git文件再add

还有问题可先删除缓存再来

	git rm --cached filename
	git add filename
	git commit -m "remark"
	git push origin master

修改或生成.gitignore在`git add .`之后会导致已经被跟踪的文件无法去除跟踪状态

    git rm -r --cached .
    git add .

### 将一个项目和多个远程仓库关联
增加`remote`(远程仓库),并在`push`的时候加上相应的参数,配置文件可查看`.git/config`

	git remote add gitlab git@server-Power:common_driver/Universal_Robots_ROS_Driver.git
	# 指定远程仓库和`upstream`即上传的分支
	git push gitlab -u master
这种方式在`push`需要指定仓库并且需要多次操作,可以使用`set-url`来一次完成
	git remote set-uri --add origin git@server-Power:common_driver/Universal_Robots_ROS_Driver.git
可以通过`git remote -v`查看当前的远程仓库	

如果想覆盖`remote`,需要`rename`

	cd existing_repo
	git remote rename origin old-origin
	git remote add origin git@server-Power:benbo/test.git
	git push -u origin --all
	git push -u origin --tags

### 不要使用Windows自带的记事本编辑任何文本文件
Microsoft开发记事本的团队保存UTF-8编码的文件:在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。建议你下载Notepad++代替记事本，不但功能强大，而且免费！记得把Notepad++的默认编码设置为UTF-8 without BOM即可

### 删除本地上已经不存在的远程分支
可使用如下指令
`git remote prune origin`

删除本地已存在的一个远程分支名为`branch_name`
`git branch -d -r origin/branch_name`


## 引用

1. 廖雪峰 [Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
2. [A collection of .gitignore templates](https://github.com/github/gitignore)
3. [Python之pyc文件作用及生成方法](https://blog.csdn.net/zong596568821xp/article/details/87341933)
4. [git commit -m与-am的区别](https://www.cnblogs.com/smile-fanyin/p/10827438.html)
5. [Git使用小技巧之多个远程仓库](https://www.cnblogs.com/endless-code/p/11173886.html)
6. [git拉取远程分支并创建本地分支](https://blog.csdn.net/tterminator/article/details/78108550)
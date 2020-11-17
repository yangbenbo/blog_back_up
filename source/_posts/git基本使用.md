---
title: git基本使用
date: 2019-07-16 17:36:13
categories:
- Linux
tags:
- git

---

git管理版本主要分三个地址进行层次提交
- 工作区(Working Directory):当前目录,即电脑能看到的目录
- 版本库(Repository):
    - 暂存区(stage或index):git add提交目的区域;
    - 分支(如master):git commit则会提交目的区域

基本操作
===
1. 常用

		git add readme.md
		git commit -m "add readme.md"
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

2. 远程仓库

		git remote add origin git@server-name:path/repo-name.git
		git push -u origin master   # 第一次推送master分支的所有内容
		git push origin master   # 可以直接使用git push
3. 分支

		git branch   # 查看分支
		git branch <name>  # 创建分支
		git checkout <name>   # 切换分支
		git checkout -b <name>   # 创建+切换分支
		git merge <name>   # 合并某分支到当前分支
		git branch -d <name>   # 删除分支
4. 冲突

	合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

		git log --graph --pretty=oneline --abbrev-commit  # 查看分支合并图
		git merge --no-ff -m "merge with no-ff" dev   # 普通模式合并
5. BUG分支

		git stash  # 把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
		git stash list 
	你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：

		$ git stash apply stash@{0}
	修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
	当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再
		
		git stash pop  # 回到工作现场。
6. 时光穿梭
		
	在Git中，用HEAD表示当前版本，也就是最新的提交版本(针对git log),上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

		git reset --hard commit_id  
		git reflog   # 查看命令历史，以便确定要回到未来的哪个版本
		# 回到之前版本发现未追踪文件,这是因为还没add 或commit就切换版本
		# 确认这些文件没用的话就可以直接clean,一般情况最好保留一个版本
		git clean -df  
7. feature

		git branch -D <name>   # 丢弃一个没有被合并过的分支
	- master分支是主分支，因此要时刻与远程同步；

	- dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

	- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

	- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

8. 合并commit
    - git log 提交历史,显示提交过版本信息,不包括删除的commit和reset操作
    - git reflog 命令历史,显示所有操作记录,包括提交,回退,合并等,一般用来回退
    
    合并commit针对git log
        
        git log     #查看commit记录
        git rebase -i commitid  # 这里的id是不需要合并的commit对应id
        # 进入vim模式,根据提示进行pick(执行commit)或者squash(合并commit)

git 遇到的问题
===

1. git add 文件添加失败

提示modified content, untracked content是因为在add的时候这个目录下面本来就有一个.git文件，自然就会add失败，先删除这个.git文件再add

还有问题可先删除缓存再来
	git rm --cached filename
	git add filename
	git commit -m "remark"
	git push origin master

引用
---
1.廖雪峰 [Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
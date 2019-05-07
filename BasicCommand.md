## 1.git config信息配置
```
--global、--local
	git config --global	#表示配置对当前用户所有仓库生效
	git config --local	#表示配置只对某个仓库生效
针对某个仓库 --local优先级大于--global
	git config --global user.name "your name"
	git config --global user.email "your email"
	git config --global user.name  #查看配置好的用户名信息
--list 显示config的配置
	git config --list --global
	git config --local --list
--unset 清除配置信息
	git config --unset --global user.name
	git config --unset --local user.email
```
## 2.创建版本库 git init

```
1.把已有的项目代码纳入git管理
	cd 项目代码所在文件夹
	git init	# 项目目录下的隐藏目录.git问git的repository
2.新建项目使用git管理
	cd 某个文件夹
	git init your_project  #会在当前路径下创建和项目同名的文件夹
	cd your_project
```
## 3.工作区、暂存区、版本库
```
用git status查看当前repository的状态
git diff 查看的修改内容
	git diff HEAD -- <file>  #可以查看工作区和版本库里最新版本的区别
1.工作区(Working Directory)
	工作区即是电脑中可以看到的项目根目录
2.版本库(Repository)
	工作区中的隐藏目录.git就是git的版本库
	工作区、版本库和暂存区的关系如下图
```
![relation](/imgs/01.jpg "工作区、版本库及暂存区关系")

```
git add # 将文件的修改添加到暂存区
	git add -u 	# 提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
	git add . 	# 提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件
	git add -A 	# 提交所有变化
git commit -m "message"	 # 将暂存区的所有内容提交到当前分支
git mv 原文件名 目标文件名  # git中快速给文件重命名
```
## 4.git log
```
用来查看当前提交(commit)历史
git log <branch>
--all 查看所有分支的提交历史
	git log --all
-- oneline 以简洁的方式展示提交历史
	git log --oneline
-n4	 查看最近的4次commit历史
	git log --oneline -n4
--graph 查看各分支合并图(使用gitk图形工具可以查看)
	git log --graph --oneline
```
## 5.版本回退和管理修改
```
git reset来实现版本回退
	git reset --hard HEAD^  #回退到上一个版本,HEAD指向的是当前版本
	git reset --hard HEAD~100  #回退到当前版本的第前100个版本
	git reset --hard commit_id  #回退到指定commit_id的版本
	git reflog  #可以查看命令历史,可以用来回到未来某个版本
git管理的是修改,git add将文件的修改提交到暂存区,git commit将暂存区的所有修改提交当前分支
撤销修改
	1.文件修改之后还未添加到暂存区
	git checkout -- <file>	#将file文件在工作区的修改全部撤销
	2.文件修改之后已添加到暂存区
	git reset HEAD <file>	#将暂存区的修改撤销(unstage),重新放回工作区
	git checkout -- <file>
	3.文件修改之后已经提交
	git reset --hard HEAD^	#回退到提交前的版本
git rm <file>	#删除一个文件(若先已经删除一个文件, git add和git rm效果相同)
```
## 6.分支管理
```
在Git中HEAD指向当前分支,而分支则指向提交(可以在.git目录中查看HEAD文件)
```
![branch1](/imgs/02.png "HEAD、分支关系")
```
查看分支：
	git branch
	git branch -av
创建分支：
	git branc <name>
切换分支：
	git checkout <name>
创建并切换分支：
	git checkout -b <name>
合并某分支到当前分支(Fast-forward)：
	git merge <name>
删除分支：
	git branch -d <name>
	git branch -D <name>	#删除一个没有被合并过的分支,-D表示强行删除
```
![branch2](/imgs/03.png "创建并切换到新分支")
![branch3](/imgs/04.png "合并分支")
```
当Git无法自动合并分支是,必须先解决冲突。解决冲突后再提交,完成合并
	解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交
	git log --graph或者gitk图形工具可以查看分支合并图

合并分支时禁用Fast-forward
	git merge --no-ff -m <message> <branch_name>

修复bug时,会通过创建临时的bug分支修复,修复完成后合并删除
如果接到紧急的修复bug任务,但当前手头分支的开发工作尚未完成无法提交时
	git stash	#把现场储藏起来,之后恢复
	git stash list  #查看存储起来的工作现场
	git stash pop  #恢复的同时把stash内容也删除
```
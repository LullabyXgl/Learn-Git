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
	git branch <name>
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

## 7.远程仓库
```
本地仓库和远程GitHub仓库实现ssh传输需要先设置Key
	ssh -t rsa -C "youremail@example.com"  #生成ssh公钥和私钥,存在与用户主目录下.ssh目录中
将本地仓库和远程仓库关联
	git remote add git@github.com:LullabyXgl/Learn-Git.git
关联后,便可以将本地仓库的内容推送的远程仓库
	git push -u origin master	#第一次推送时加选项-u
	git push origin master
也可以从远程仓库克隆一个仓库到本地
	git clone git@github.com:LullabyXgl/Learn-Git.git  #克隆后的本地仓库会自动和远程仓库关联
克隆之后,本地默认只能看到master分支,在本地创建和远程分支对应的分支
	git checkout -b branch-name origin/branch-name  #本地和远程分支的名称最好一致
抓取远程分支的新提交
	git pull
git pull 拉取失败时,需要建立本地分支和远程分支的关联
	git branch --set-upstream branch-name origin/branch-name
查看远程仓库的信息
	git remote
	git remote -v  #使用-v选项查看更详细的信息
删除关联的远端仓库
	git rm origin
```

## 8.标签tag
```
发布一个版本时,通常需要再版本库打一个标签(tag),便于根据版本标签找到对应的commit
创建标签
	git tag <tagname>  #默认新建的标签打在HEAD指向的commit上
	git tag <tagname> commit_id  #在指定的commit上打标签
	git tag -a <tagname> -m <message> commit_id  #-a指定标签名,-m指定说明文字
查看标签
	git tag  #查看标签,标签是按字母排序的
	git show <tagname>  #查看标签的详细信息
创建的标签都只存储在本地,不会自动推送到远程,推送标签到到远程
	git push origin <tagname>  #推送一个本地标签
	git push origin --tags  #推送全部未推送过的本地标签
删除标签
	git tag -d <tagname>  #删除一个本地标签
	git push origin :refs/tags/<tagname>  #先删除本地标签后,使用该命令删除远端标签
```
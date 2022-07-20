## git常用命令

#### 初始化仓库
	git init

#### 配置全局
	git config --global user.name "name"
	git config --global user.email "email"
	
#### 提交修改
	git add <file> #添加更新的文件
	git commit -m #"descriptions" #提交更新文件描述
	git commit --amend #修改最后一次提交
	git commit --amend --author "user_name <user_email>" #修改最近提交用户名和邮箱

#### 查看状态、比对
	git status #查看状态
	git status -s #文件状态缩略信息
	git diff #查看变更内容
	git diff HEAD -- <file>	 #查看工作区和版本库里面最新版本的区别
	git diff --check <file>  #检查是否有空白错误
	git diff --cached <file>  #查看已add的内容

#### 查看历史版本、历史操作
	git log #查看提交日志
	git reflog #查看当前仓库的操作日志
	git log -n #最近n条的提交历史
	git log <branch_name> -n #分支branch_name最近n条的提交历史
	git log --stat #历次commit的文件变化
	git log --name-status  #显示新增、修改、删除的文件清单
	git log -p  #历次commit的内容增删

	git log --since --after     #显示时间之后的提交
	git log --until --before    #显示时间之前的提交
	git --author                #显示指定作者的提交
	git log --since=1.weeks     #过去一周的提交
	git log --since=1.days      #过去一天的提交

#### 版本回退、前进
	git reset --hard HEAD^		#回退到上1版本
	git reset --hard HEAD~5		#回退到上5个版本
	git reset --hard id		#回退到指定版本

#### 撤销修改
	git checkout -- <file>		#撤销修改：误修改工作区文件，未git add/commit
	git restore <file>		#撤销修改：误修改工作区文件，未git add/commit
	git reset HEAD <file>		#撤销git add：误将文件加入暂存区（git add），未git commit
	git reset --hard HEAD^		#撤销git commit：误将文件提交（一旦提交，只能通过版本回退进行撤销）

#### 删除与恢复
	git rm/add <file>
	git commit -m "remove <file>"	#删除版本库中的<file>：删除工作区文件后，继续删除版本库中相应的文件
	git checkout -- <file>		#根据版本库中的<file>恢复工作区<file>

#### 清理工作区未track也未ignore的文件或文件夹(如各种临时.swp, .patch文件等)
	git clean -i    #交互式清理, 不常用
	git clean -n    #查看清理文件列表(不包括文件夹), 不执行实际清理动作
	git clean -n -d #查看清理文件列表(包括文件夹), 不执行实际清理动作
	git clean -f    #清理所有未track文件
	git clean -df   #清理所有未track文件和文件夹, 常用, 但使用前确保新增加的文件或文件夹已add, 否则新创建的文件或者文件夹也会被强制删除

#### 关联远程仓库（本地到远程）
	git remote add origin <remote address>	#在本地工作区目录下按照 Git 提示进行关联
	git remote rm origin			#解除错误关联
	git push -u origin master		#第一次将本地仓库推送至远程仓库（每次在本地提交后进行操作）
	git push origin master			#以后每次将本地仓库推送至远程仓库（每次在本地提交后进行操作）
	git remote update origin --prune        #更新远程分支，清除已经不存在的远程分支
	git remote set-url origin <url>         #更新远程仓库地址
	<remote address>:
	git@github.com:<username>/<repository>.git
	https://github.com/<username>/<repository>.git
	
	git config --global credential.helper cache #记住密码（默认15分钟)
	git config credential.helper 'cache --timeout=3600' #自己定义时间（一小时后失效）
	git config --global credential.helper store #永久存储密码

#### 克隆GitHub远程仓库（远程到本地）
	git clone <remote address>	#git协议速度更快但通常公司内网不允许，https协议速度慢

#### 分支管理：创建、切换、查看、合并、删除
	git branch <branch name>	#创建<branch name>分支
	git checkout <branch name>	#切换至<branch name>分支
	git switch <branch name>	#切换至<branch name>分支
	git checkout -b <branch name>	#创建并切换至<branch name>分支
	git switch -c <branch name>	#创建并切换至<branch name>分支
	git branch			#查看已有分支（* 表示当前分支）
	git merge <branch name>		#合并<branch name>到当前分支（通常在master分支下操作）
	git branch -d <branch name>	#删除分支

#### 解决合并冲突
	合并时报错“分支发生冲突”，首先vim相应文件，修改冲突位置，然后按照git add/commit重新提交，最后删除多余分支即可。
	git log --graph --pretty=oneline --abbrev-commit
	git log --graph

#### 分支管理：合并后删除分支也在 log 中保留分支记录
	git merge --no-ff -m "descriptions" <branch name>

#### 开发流程：
	master分支		#发布稳定版本
	dev分支			#发布开发版本
   	feature-*分支             #基于master分支创建，一般一个新的功能模块对应一个新的分支，开发完成后合并到 develop 分支并发布测试环境，测试结束再合并到master

#### Bug分支管理（建立单独分支进行bug修复）
	bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。
	git stash			#保存当前工作现场（在dev未完成开发，但master有bug需要修复）
	git stash pop			#回到dev分支后恢复工作现场（list中的现场会同时被删除）
	git stash list			#查看当前存储的工作现场
	git stash apply stash@{#}	#回到指定工作现场（list中的现场不会被删除，需要用git stash drop）
	git stash drop stash@{#}	#删除指定工作现场
	git cherry-pick <id>		#在master修复好bug后，在dev复制一遍bug修复流程

#### Feature分支管理（建立单独分支添加新功能）
	软件开发中，总有无穷无尽的新的功能要不断添加进来。添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。
	git branch -D <branch name>	#强制删除分支（丢弃未合并分支）

#### 协作与分支推送
	User 1:
	git remote [-v]						#查看远程库信息（-v 查看详细信息）
	git push origin [master/dev/...]			#推送指定分支到远程
	User 2:
	git clone <remote address>				#克隆到本地（只能克隆master）
	git checkout -b dev origin/dev				#本地新建分支并关联远程
	git add/commit/push					#添加、提交、推送更新
	User 1:
	git add/commit/push					#推送时报错（与user 2推送的更新冲突）
	git pull <remote> <branch>
	git branch --set-upstream-to=origin/<branch> <branch>	#本地与远程关联
	git pull						#拉取远程文件（并解决冲突）
	git commit/push						#重新提交并推送

##### 标签管理（常用于版本管理）：查看、创建、操作
	git tag								#查看标签
	git show <tag name>						#查看指定标签
	git log --pretty=oneline --abbrev-commit --decorate=full	#在log中显示标签
	git tag <tag name>						#为上次commit位置打标签
	git tag <tag name> <commit id>					#为指定commit位置打标签
	git tag -a <tag name> -m "descriptions" <commit id>		#为指定commit打标并添加描述
	git tag -d <tag name>						#删除本地标签
	git push origin <tag name>					#推送指定标签到远程
	git push origin --tags						#推送所有本地标签到远程
	git push origin :refs/tags/<tag name>				#删除远程标签（先删除本地标签）

#### rebase(换基)
- rebase 在日常中常用功能主要是两个, 多人协同开发定期rebase master以及压缩某分支多个commit
```
git rebase master 常见于多人开发, 每个开发人员从master checkout出自己的分支, 开发一段时间后提交至master之前最好rebase一下, 防止冲突,
	      就算真有冲突在本地解决好过强制提交, 开发流程中尽量保证master的干净整洁

举个例子:
master分支上有三个提交C1, C2, C3
某一时刻usr1在C3的master分支上checkout出新的分支, 用于开发服务端支持ipv6新特性, 并提交了C4, C5
git checkout -b ipv6_support
......
git commit -m C4
......
git commit -m C5
此时提交状态如下所示
      (origin/master branch)
	     |
C1 <- C2 <- C3
	     \
	      \
	       \
		C4 <- C5
		       |
		(ipv6_support branch)

某同事usr2修改了master上的内存泄漏错误, 并提交了C6, C7, C8三个commit, 然后直接推送origin/master(假设这个期间无其他人推新内容到master)
此时提交状态如下所示
		    (origin/usr2/fix_mem_leak branch)
			       |
C1 <- C2 <- C3 <- C6 <- C7 <- C8
	     \                 |
	      \         (origin/master branch)
	       \
		C4 <- C5
		       |
		(ipv6_support branch)

如果此时usr1希望将ipv6的新特性提交至master, 那么在其直接push origin master时会提示master需要合并分支ipv6_support
虽然C4, C5的改动内容完全独立于C6, C7, C8的改动
但git仍会抓取C5和C8的提交并产生一个新的C9 commit(因两者分支的base不同), 如下图所示
C1 <- C2 <- C3 <- C6 <- C7 <- C8
	     \                 \
	      \                 \
	       \                 \
		C4 <- C5 <------ C9

如果是为了保证master提交记录的"干净完整"
或者是某分支不着急提交, 仍需要更多的测试与开发, 但又不想分支开发周期结束后"偏离"当初checkout的master分支太久远(容易造成更多的冲突)
可以考虑(定期)利用rebase来进行变基
即上面提到过的多人协同开发, 定期rebase master是个好习惯
git checkout ipv6_support
git rebase master
结果提交状态如下所示
	    (origin/master origin/usr2/fix_mem_leak branch)
			       |
C1 <- C2 <- C3 <- C6 <- C7 <- C8
				\
				 \
				  \
				   C4' <- C5'
					   |
				    (ipv6_support branch)
这种rebase在功能上类似将某分支所有的改动做成多个patch并依次打在指定的新base上
此时再提交master就不会产生抓取效果, 会将C4'和C5'直接提交至master, 即can be fast-forwarded, 同时也保证了master提交记录的整洁性
(注: 虽然C4'和C5'的内容和C4, C5完全一致, 但两者base不同, commit hash code也完全不同)

git rebase --noto <branch_lhs> <branch_rhs> #重放, 用于变基在分支branch_lhs中而不在branch_rhs中的commit
```

- 某项目状态分支如下所示, 其中Cn的数字代表提交时间顺
- T1 某员工urs1从C2(master分支)checkout出一个新的分支用于开发某基础公共组件功能
- T2 员工usr1开发完毕提交C3, 然后继续在该分支上(或checkout -b server)开发服务端相关功能, 并提交C4
- T3 master分支有更新, 其他同事usr2提交了C5, C6并推送到了origin master
- T4 员工usr1从server分支切回到C3公共基础的提交, 并创建新分支client, 用于开发客户端功能, 并提交C8, C9
- T5 员工usr1从client分支切回到server分支继续开发服务端功能, 并提交C10
```
	    (master branch)
		   |
C1 <- C2 <- C5 <- C6
	 \
	  \
	   \
	    C3 <- C4 <- C10
	       \         |
		\ (server branch)
		 \
		  C8 <- C9
			 |
		  (client branch)
```

- 此时该员工希望将客户端相关的功能合并到主分支并发布，但暂时并不想合并 server 中的修改，因为它们还需要经
- 过更全面的测试。 这时可以使用 git rebase 命令的 --onto 选项，选中在 client 分支里但不在
- server 分支里的修改（即 C8 和 C9），将它们在 master 分支上重放：
```
git rebase --noto client server
```	

- 得到如下图所示的提交状态
- 注:其中C3', C8', C9'与C3, C8, C9的提交内容完全一样, 但是hash id是完全不同的
```
		    (master branch)(client branch)
			   |            |
	C1 <- C2 <- C5 <- C6 <- C8' <- C9'
		 \
		  \
		   \
		    C3 <- C4 <- C10
		       \         |
			\ (server branch)
			 \
		       [#####disable######]
		       [  C8 <- C9        ]
		       [         |        ]
		       [  (client branch) ]

	#can be fast-forwarded
	git checkout master
	git merge client
```

- 提交后分支状态如下
```
					(client branch)
					       |
	C1 <- C2 <- C5 <- C6 <- C3' <- C8' <- C9'
		 \                             |
		  \                     (master branch)
		   \
		    C3 <- C4 <- C10
				 |
			  (server branch)

	git rebase -i HEAD~n 压缩当前分支的n个commit并合并为1个commit, 常见第一行为pick, 剩下的n-1行为squash

	git rebase --abort # rebase过程中发生错误, 可以利用该命令终止整个rebase过程
	git rebase --continue # rebase过程中发生冲突, 在解决冲突后可以利用该命令进行后续过程
```

#### 打patch(补丁)
- 生成diff patch文件(git可以识别diff文件)
```
git <branch> log -n -p > diff.patch # 生成某分支过去n个commit的文件diff信息至单个diff文件
git diff <--cached> diff.patch # 针对当前缓存区的内容生成diff文件
```

- 利用apply打patch
```
git apply --check diff.patch    #检查是否可以正常应用, 无回显证明无冲突
git apply --stat diff.patch     #查看应用diff文件后的文件变化
git apply diff.patch            #打patch, 仅仅改变文件信息, 无commit信息, 仍然需要add, commit
```

- 利用--format-patch生成patch, 带commit信息
```
git format-patch <branch> -n 　 #生成分支<branch>最近的n次commit的patch
git format-patch <r1>..<r2>     #生成两个commit间的修改的patch（包含两个commit. <r1>和<r2>都是具体的commit号)
git format-patch -1 <r1>        #生成单个commit的patch
git format-patch <r1>           #生成某commit以来的修改patch（不包含该commit）
git format-patch --root <r1>　　#生成从根到r1提交的所有patch
```

- 利用am打patch
```
git apply --check 0001-update-bash.sh.patch #检查patch是否冲突可用
git apply --stat 0001-update-bash.sh.patch  #检查patch文件变更情况, 无回显证明无冲突
git am 0001-update-bash.sh.patch            #将该patch打上到当前分支, 带commit信息
git am ./*.patch                            #将当前路径下的所有patch按照先后顺序打上
git am --abort                              #终止整个打patch的过程, 类似rebase --abort
git am --resolved                           #解决冲突后, 可以执行该命令进行后续的patch, 类似rebase --continue
```

#### bundle(打包)
- 该命令会将git工程打包, 默认情况下会打包所有commit记录和track的文件
- 不同于简单粗暴tar.gz打包整个文件夹, bundle只打包那些push过的记录
- 如某git工程下存在.build构建后的目录, 而.gitignore又忽略了该文件夹
- 如果利用tar.gz打包则会将那些忽略的文件文件夹一并打包, 可能会造成压缩包极大的臃肿
- 而又不想仅仅为了打个包就删除整个build目录(如重新build时间成本太大)
- 那么就可以使用bundle进行打包, 该命令只打包track过的文件
- 并且像url那样直接调用git clone来重建
```
git bundle create awesome-cheatsheets.bundle HEAD master #打包重建master分支的所有数据
git clone awesome-cheatsheets.bundle #重建工程
```

#### bundle也可以打包指定的区间, 至于提交区间有多种表示方式
	git bundle create awesome-cheatsheets.bundle HEAD~10
	git bundle create awesome-cheatsheets.bundle HEAD~10..HEAD
	git bundle create awesome-cheatsheets.bundle lhs_commit_md5..rhs_commit_md5
	git bundle create awesome-cheatsheets.bundle origin/master..master
	git bundle create awesome-cheatsheets.bundle master ^origin/master

#### 使用GitHub
	fork --> clone --> add/commit/push --> pull request

#### 配置.gitignore文件
	/<dir name>/			#忽略文件夹
	*.zip				#忽略.zip文件
	/<dir name>/<file name>		#忽略指定文件

#### 文件.gitignore生效后
	git add -f <file>		#强制添加
	git check-ignore -v <file>	#查看生效规则

#### 配置别名
	git config [--global] alias.<alias> '<original command>'	#为所有工作区/当前工作区配置别名
	.git/config			#当前工作区的配置文件
	~/.gitconfig		#当前用户的配置文件

#### 不常用命令

- 更新远程分支，清除已经不存在的分支

	git remote update origin --prune
	git 设置只输入一次用户名和密码
	https方式

- 按照如下设置可只输入一次

	记住密码（默认15分钟）：
	git config --global credential.helper cache

	自己定义时间（一小时后失效）：
	git config credential.helper 'cache --timeout=3600'

	永久存储密码：
	git config --global credential.helper store

	取消记住的密码
	git config --global --unset credential.helper

- git修改ssh和https上传模式
	git remote set-url origin https://github.com/aimi-cn/AILearners.git
	git remote set-url origin git@github.com:aimi-cn/AILearners.git

- 远程存在新的提交，本地无法提交
	git pull mld master --allow-unrelated-histories

#### 附录
- [廖雪峰Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
- [git](https://training.github.com/downloads/zh_CN/github-git-cheat-sheet/)

#### .gitignore

- 优先级由高到低

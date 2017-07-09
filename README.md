# git-note
git 笔记

----------
GitHub 教程
===
GitHub信息
---
* GitHub是为开发者提供Git仓库的代码托管服务。
* GitHub上公开的代码都是由Git管理。
* Pull request功能是指开发者在本地对源代码进行更改后，向GitHub中托管的Git仓库请求合并的功能。
* Issue（问题）可以进行任务管理和bug报告的交互。是将一个任务或问题分配给Issue进行追踪和管理的功能，每一个功能更改或修正都对应一个Issue。只要查看issue就知道和这个修改相关的一切信息，并加以管理。
* Watch将感兴趣的库添加到watch中，可以第一时间掌握最新版的新功能或bug修正的信息。
* 查看别人的News Feed可以知道对方对哪些库感兴趣。
* Wiki功能，任何人都能随时对一篇文章进行修改并保存。

Git与GitHub联动
---
* 创建SSH Key
	* 第一步
		
			$ ssh-keygen -t rsa -C "youremail@example.com"   //这个命令添加自己的GitHub邮箱，之后一路回车，使用默认值就可以
	用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人
	* 第二步，登陆GitHub，打开“Account settings”，“SSH Keys”页面：然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容
	![添加ＳＳＨ　Ｋｅｙ到ＧｉｔＨｕｂ](http://www.liaoxuefeng.com/files/attachments/001384908342205cc1234dfe1b541ff88b90b44b30360da000/0)
	* GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了
* 将git仓库与GitHub上的项目连接
		
		$ git remote add origin git@github.com:huanghaibin91/git-note.git   //这条命令将本地与远程库关联一起
		
		错误提示：fatal: remote origin already exists.
		解决办法：
		$ git remote rm origin
		然后在执行：$ git remote add origin git@github.com:huanghaibin91/git-note.git 就不会报错误了
		
		$ git push -u origin master   //把本地库的所有内容推送到远程库上,如果远程库是空的第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来

		$ git push origin master   //第一次提交之后，就可以去掉-u参数了

		错误提示：error: failed to push some refs to 
		解决办法：
		$ git pull --rebase origin master   //把GitHub上的最新文件下载下来，接着再使用 $ git push origin master
* 从远程库克隆
		
		$ git clone git@github.com:michaelliao/gitskills.git
		Cloning into 'gitskills'...
		remote: Counting objects: 3, done.
		remote: Total 3 (delta 0), reused 0 (delta 0)
		Receiving objects: 100% (3/3), done.

		$ cd gitskills
		$ ls
		README.md
	GitHub给出的地址不止一个，还可以用https://github.com/michaelliao/gitskills.git这样的地址。实际上，Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议

Git 教程
===
* 创建版本库

		$ mkdir learn-git   //在合适的位置，用mkdir命令创建一个空文件目录，文件夹名称不要用中文
		$ cd learn-git   //指到这个目录
		$ pwd   //pwd命令显示当前目录
		/Users/michael/learn-git //当前目录的名字

		$ git init   //git init命令可以将当前目录变成一个Git仓库
		Initialized empty Git repository in /Users/michael/learn-git/.git/
		//此时Git仓库下会有一.git文件夹，是Git用来跟踪管理版本库的，不要乱改，这个不算工作区，而是Git的版本库
* 把文件添加到版本库
		
		$ git add readme.text   //用git add命令把文件添加到仓库，把文件添加到暂存区
		$ git commit -m "wrote a readme file"   //用git commit命令把文件提交到仓库，把暂存区的所有内容提交到当前分支，-m后面输入的是本次提交的说明，可以是任何内容，最好是有意义的
		
		//Git添加文件需要add，commit一共两步，因为commit可以一次提交很多文件，所以你可以多次add不同的文件，先add几个文件，然后一次全部提交
		$ git add file1.txt
		$ git add file2.txt file3.txt
		$ git commit -m "add 3 files."
* 时光机穿梭
		
		$ git status   //git status命令可以让我们时刻掌握仓库当前的状态，上面的命令告诉我们，readme.txt被修改过了，但还没有准备提交的修改，随时掌握工作区的状态
		# On branch master
		# Changes not staged for commit:
		#   (use "git add <file>..." to update what will be committed)
		#   (use "git checkout -- <file>..." to discard changes in working directory)
		#
		#    modified:   readme.txt
		#
		no changes added to commit (use "git add" and/or "git commit -a")

		$ git diff readme.txt   //git diff顾名思义就是查看difference，可以看到对文件做了何种修改
		diff --git a/readme.txt b/readme.txt
		index 46d49bf..9247db6 100644
		--- a/readme.txt
		+++ b/readme.txt
		@@ -1,2 +1,2 @@
		-Git is a version control system.
		+Git is a distributed version control system.
 		Git is free software.
* 版本回退
		
		$ git log   //用git log命令可以查看历史记录
		$ git log --pretty=oneline   //可以输出简洁的历史记录
		$ git reset --hard HEAD^   //用git reset可以回退前面的版本，HEAD表示当前版本，HEAD^表示上一个版本，HEAD^^表示上上个版本，当版本过前是^符号太麻烦，可以使用HEAD~25（数字），返回到数字表示的版本
		$ git reset --hard (commit id 版本号)   //就能回到版本号的那个版本
		$ git reflog   //可以记录你的命令记录，利用历史命令可以获得commit id等信息
* 工作区和暂存区
	* Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD 
	![版本库](http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)
	* git add命令实际上就是把要提交的所有修改放到暂存区（Stage） 
	![添加文件之后](http://www.liaoxuefeng.com/files/attachments/001384907720458e56751df1c474485b697575073c40ae9000/0)
	* 执行git commit就可以一次性把暂存区的所有修改提交到分支
	![提交文件到分支](http://www.liaoxuefeng.com/files/attachments/0013849077337835a877df2d26742b88dd7f56a6ace3ecf000/0)
* 撤销修改
		
		$ git checkout -- readme.txt   //命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况:
        一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
        一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。总之，就是让这个文件回到最近一次git commit或git add时的状态。

		git reset HEAD file   //用命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区
	* 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
	* 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。
	* 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。
* 删除文件
		
		$ rm test.txt   //用rm命令删除文件管理器中的文件
		$ git commit -m "delete test.text"  //再用git commit命令确认此次操作
		$ git checkout -- test.txt   //删错了，因为版本库里还有，所以可以很轻松地把误删的文件恢复到最新版本。git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”
* 创建分支
		
		$ git checkout -b dev   //创建dev分支，然后切换到dev分支，checkout加个-b相当于下面两条命令
		Switched to a new branch 'dev'

		$ git branch dev   //创建dev分支
		$ git checkout dev   //切换到dev分支
		Switched to branch 'dev'
		
		$ git branch   //查看当前分支

		$ git merge dev   //把dev分支的工作成果合并到master分支

		$ git branch -d dev   //删除dev分支
	查看分支：git branch
	创建分支：git branch <name>
	切换分支：git checkout <name>
	创建+切换分支：git checkout -b <name>
	合并某分支到当前分支：git merge <name>
	删除分支：git branch -d <name>
* 创建标签
    命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；
    git tag -a <tagname> -m "blablabla..."可以指定标签信息；
    git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；
    命令git tag可以查看所有标签
	
	命令git push origin <tagname>可以推送一个本地标签；
	命令git push origin --tags可以推送全部未推送过的本地标签；
	命令git tag -d <tagname>可以删除一个本地标签；
	命令git push origin :refs/tags/<tagname>可以删除一个远程标签
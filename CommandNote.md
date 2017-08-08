# 命令笔记
## SSH 密钥配置
1. 生成SSH密钥

`ssh-keygen -b 4096 -t rsa`

对于提示可以按照默认设置即可，直接按回车

2. 向远端服务器上传公钥

`ssh-copy-id user@ip`

按照提示输入user用户在服务器ip上的密码

## GitHub密钥配置

1. GitHub上创建Git公钥

找到.ssh文件夹，用文本编辑器打开“id_rsa.pub”文件，复制内容到剪贴板。
打开 https://github.com/settings/ssh ，点击 Add SSH Key 按钮，粘贴进去保存即可。

2. 测试连接

把公钥添加到GitHub上后，在终端输入命令`$ ssh -T git@github.com` 如果返回的结果中包含如下内容，则连接配置成功：
` You've successfully authenticated, but GitHub does not provide shell access.`

3. 克隆Git仓库的时候记得需要选择SSH地址，这样在提交代码的时候就可以不用再次输入用户名和密码了。

## node.js安装  
* 在CentOS下执行下面的命令，安装node.js 8  
```
curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
sudo yum -y install nodejs
```
* node.js配置国内源  
```
npm config set registry https://registry.npm.taobao.org

// 配置后可通过下面方式来验证是否成功
npm config get registry
// 或
npm info express
```

## Git命令

1. 设置用户名和邮箱

```git config --global user.name "Your Name"
git config --global user.email "Your email"```

git config --list

列出git管理的文件
git ls-files
git blame <filename>
git init
git status
git diff filename.txt
git diff --staged
git add
git commit -m "commit log"
添加到暂存区的同时进行提交
git commit -a -m "commit log" 
版本回退
git reset --hard HEAD^
git reset --hard 3628164
git log --oneline
穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
git reflog

Git管理的是修改，而不是文件
git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别

命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。

命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区

git checkout -- readme.txt

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了

场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

删除文件git rm test.txt


要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；

关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；


Git鼓励大量使用分支：

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>

$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.

解决冲突

$ git add readme.txt 
$ git commit -m "conflict fixed"
git log --graph --pretty=oneline --abbrev-commit

git log --graph命令可以看到分支合并图。
git merge --abort
强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
git merge --no-ff -m "merge with no-ff" dev

Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作
$ git stash
$ git stash list
$ git stash pop

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。
查看分支合并情况
git log --graph --decorate --pretty=oneline --all --abbrev-commit

命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；

git tag -a <tagname> -m "blablabla..."可以指定标签信息；

git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；

命令git tag可以查看所有标签。

命令git push origin <tagname>可以推送一个本地标签；

命令git push origin --tags可以推送全部未推送过的本地标签；

命令git tag -d <tagname>可以删除一个本地标签；

命令git push origin :refs/tags/<tagname>可以删除一个远程标签。



查看远端分支
git branch -av
git ls-remote
git ls-remote origin
git remote add origin F:/tmp/gittest.git
git pull origin master --allow-unrelated-histories
git branch --set-upstream-to=origin/master    或者git branch -u origin/serverfix

git clone http://git.yonyou.com/ICC-Custom-group/GuJing.git

切换远端分支
$ git checkout -b serverfix origin/serverfix
git checkout --track origin/serverfix

查看设置的所有跟踪分支，可以使用 git branch 的 -vv 选项
git branch -vv
删除远程分支
git push origin --delete serverfix

Git cherry-pick可以选择某一个分支中的一个或几个commit(s)来进行操作。http://blog.csdn.net/wh_19910525/article/details/7554430
git cherry-pick 38361a68

git push -u origin master


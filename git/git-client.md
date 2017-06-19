## git客户端常用命令

+ 创建本地版本库
```
git init
```
+ 添加文件到暂存区
```
git add .
或者
git add -A
```
+ 把暂存区的内容提交到当前分支并注释
```
git commit -m'注释文字'
```
+ 提交本地到远程仓库
```
git push origin master
```
第一次提交时可加上-u参数，简历本地与远程的连接
```
git push -u origin master
```
+ 查看提交历史
```
git log
git log --graph命令可以看到分支合并图
git log --pretty=oneline --abbrev-commit   缩略显示
```
+ git 版本回退
```
回退到上一个版本
git reset --hard HEAD^
```
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。

穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

+ git 查看状态
```
git status
```

+ git 撤销修改
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令
```
git checkout -- file
```

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退，一般使用git reset --hard commit_id  ,不过前提是没有推送到远程库。

+ git 删除文件
```
git rm 
```
命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容

+ git 关联远程仓库

要关联一个远程库，使用命令
```
git remote add origin git@server-name:path/repo-name.git；
```
关联后，使用命令
```
git push -u origin master
```
第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令
```
git push origin master
```
推送最新修改；

+ git 远程仓库
```js
//克隆远程仓库
 git clone git@server-name:path/repo-name.git

 //查看远程仓库的信息
 git remote 或者git remote -v 

 //推送分支
 //推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：
git push origin master

//如果要推送其他分支，比如dev，就改成：
git push origin dev

 ```

 + git 分支管理
```
查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

删除分支：git branch -d <name>
丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除

合并某分支到当前分支：git merge <name> (默认是fast forward模式)

合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并

git merge --no-ff -m "merge with no-ff" dev
```

+ git 储存工作区
```js
//储存工作区
git stash
//查看stash列表
git stash list
//恢复工作区
git stash pop
```

+ git 多人协作
```
查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交(git pull 相当于执行了git fetch   git merge两条命令)；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。
```
+ git 标签管理

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

tag是一个让人容易记住的有意义的名字，它跟某个commit绑在一起

添加tag
```js
//默认标签是打在最新提交的commit上的
git tag v1.0

//为之前commit添加标签，后面加上commit id 即可
git tag v0.9 commit_id

//查看commit 提交历史
git log --pretty=oneline --abbrev-commit

git tag -a <tagname> -m "blablabla..."可以指定标签信息；

git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错

git tag  可以查看所有标签。
git show <tagname>查看标签信息

//删除标签
git tag -d v1.0
//推送标签到远程
git push origin <tagname>
//推送所有标签到远程
git push origin --tags

//删除远程标签
1.先删除本地标签 git tag -d v1.0
2.从远程删除  git push origin :refs/tags/v1.0
```

+ git 忽略特殊文件
```
创建 .gitignore 文件，写入需要git 忽略的文件

```

+ git 命令配置别名
```js
git config --global alias.st status
//撤销修改
git config --global alias.unstage 'reset HEAD'
//git log别名
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

//git 配置文件
配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用；每个仓库的Git配置文件都放在.git/config文件中；
```

+ 服务器端创建git仓库
```bash
1.先选定一个目录作为Git仓库,假定是/srv/sample.git，在/srv目录下输入命令

sudo git init --bare sample.git

Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。
2.然后，把owner改为git

sudo chown -R git:git sample.git

3.禁用shell登录：

出于安全考虑，创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成
把
git:x:1001:1001:,,,:/home/git:/bin/bash
修改为
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell

这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出

4.克隆远程仓库

git clone git@server:/srv/sample.git

```

+ 参考
廖雪峰的官方网站
http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000
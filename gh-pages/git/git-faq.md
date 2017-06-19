Git使用中的问题及解决


+ 1.1. push：unpack failed: unpack-objects abnormal exit

错误信息：
```bash
error: insufficient permission for adding an object to repository database ./objectserror: unpack failed: unpack-objects abnormal exit

fatal: failed to write object

To ssh://sunwr@192.168.30.249/home/gitdata/code/ESB

! [remote rejected] master -> master (n/a (unpacker error))

error: failed to push some refs to 'ssh://sunwr@192.168.30.249/home/gitdata/code/ESB'

clean install --X
```

解决：

把git库中code目录重新赋权chmod -R 777 *(对所有文件都赋予最高权限)


+ 1.2. push文档：branch is currently checked out


错误信息：

在使用Git Push代码到数据仓库时，提示如下错误:
```bash
[remote rejected] master -> master (branch is currently checked out)

remote: error: refusing to update checked out branch: refs/heads/master

remote: error: By default, updating the current branch in a non-bare repository

remote: error: is denied, because it will make the index and work tree inconsistent

remote: error: with what you pushed, and will require 'git reset --hard' to match

remote: error: the work tree to HEAD.

remote: error:

remote: error: You can set 'receive.denyCurrentBranch' configuration variable to

remote: error: 'ignore' or 'warn' in the remote repository to allow pushing into

remote: error: its current branch; however, this is not recommended unless you

remote: error: arranged to update its work tree to match what you pushed in some

remote: error: other way.

remote: error:

remote: error: To squelch this message and still keep the default behaviour, set

remote: error: 'receive.denyCurrentBranch' configuration variable to 'refuse'.

To git@192.168.1.X:/var/git.server/.../web

! [remote rejected] master -> master (branch is currently checked out)

error: failed to push some refs to 'git@192.168.1.X:/var/git.server/.../web'
```

原因及解决：

这是由于git默认拒绝了push操作，需要进行设置，修改.git/config添加如下代码：
```
[receive]

   denyCurrentBranch = ignore

```

在初始化远程仓库时最好使用 git --bare init   而不要使用：git init

如果使用了git init初始化，则远程仓库的目录下，也包含work tree，当本地仓库向远程仓库push时,   如果远程仓库正在push的分支上（如果当时不在push的分支，就没有问题）, 那么push后的结果不会反应在work tree上,  也即在远程仓库的目录下对应的文件还是之前的内容，必须得使用git reset --hard才能看到push后的内容.


+ 1.3. push错误non-fast-forward后的冲突解决


错误信息
```bash
Non-fast-forward
```


原因及解决：

问题（Non-fast-forward）的出现原因在于：git仓库中已经有一部分代码，所以它不允许你直接把你的代码覆盖上去。于是你有2个选择方式：

```bash
1，强推，即利用强覆盖方式用你本地的代码替代git仓库内的内容

git push -f

2，先把git的东西fetch到你本地然后merge后再push

$ git fetch

$ git merge

这2句命令等价于

$ git pull

```
+ 1.4. Git错误：管道已关闭---fatal: Not a git repository (or any of the parent directories): .git


错误信息

Eclipse中执行Push提示：管道已关闭，或管道正在被关闭

使用git bash执行提示fatal: Not a git repository (or any of the parent directories): .git


原因及解决

提示说没有.git这样一个目录

解决：在git bash中相应的git目录下执行： $git init


+ 1.5. If no other git process is currently running


错误信息

执行git add somefile的时候，出现如下错误：
``` bash 
If no other git process is currently running, this probably means a git process crashed in this repository earlier. Make sure no other git process is running and remove the file manually to continue.
```

原因及解决

解决：rm -f ./.git/index.lock





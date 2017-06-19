##centos7下git服务器端搭建

git的安装：

yum 源仓库里的 Git 版本更新不及时，最新版本的 Git 是 1.8.3.1，但是官方最新版本已经到了 2.9.2。想要安装最新版本的的 Git，只能下载源码进行安装。

1. 查看 yum 源仓库的 Git 信息：

1
```bash
# yum info git
```

可以看出，截至目前，yum 源仓库中最新的 Git 版本才 1.8.3.1，而查看最新的 Git 发布版本，已经 2.9.2 了。

2. 依赖库安装
```bash
# yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
# yum install gcc perl-ExtUtils-MakeMaker
```
3. 卸载低版本的 Git

通过命令：git –-version 查看系统带的版本，Git 版本是： 1.8.3.1，所以先要卸载低版本的 Git，命令：

```bash
# yum remove git
```
4. 下载新版的 Git 源码包（我放的了  /usr/local/git 的目录下了，git是我自己mkdir的目录）
```bash
　　进入：/usr/local下，新建git目录:  # mkdir git 

　　# cd git 
```
在线下载最新的源码包
```bash
# wget https://github.com/git/git/archive/v2.9.2.tar.gz
```
也可以离线下载，然后传到 CentOS 系统中指定的目录下。

5. 解压当前目录
```bash
# tar -xzvf v2.9.2.tar.gz
```
6. 安装 Git

分别执行以下命令进行编译安装，编译过程可能比较漫长，请耐心等待完成。
```bash
# cd git-2.9.2
# make prefix=/usr/local/git all
# make prefix=/usr/local/git install
```
7. 添加到环境变量
```bash
vim /etc/profile
```  
```bash
#如果没有vim，则安装vim工具   yum install vim

添加这一条：   export PATH="/usr/local/git/bin:$PATH" 

source /etc/profile   #是配置立即生效
```
 

8. 查看版本号
```bash
# git --version
```
git version 2.9.2
8. 将git设置为默认路径，不然后面克隆时会报错
```bash
[root@localhost code]$ ln -s /usr/local/git/bin/git-upload-pack /usr/bin/git-upload-pack 

[root@localhost code]$ ln -s /usr/local/git/bin/git-receive-pack /usr/bin/git-receive-pack 
```
至此，CentOS 就安装上了最新版本的 Git。

第二步，创建一个git用户组和用户，用来运行git服务：
```bash
$ groupadd git
$ useradd git -g git
$ passwd git  #参数是用户名
```
最好切换到git用户 不然后面新建的git仓库都要改权限 烦烦烦！！
```bash
$ su - git 
```
 
第三步，创建证书登录：
如何生成密钥：http://www.cnblogs.com/xuange306/p/6403907.html

备注：下边虚线内容为多余内容，只是留着存档而已。于本教程没有关系

*添加证书之前，还要做这么一步：*

*Git服务器打开RSA认证 。在Git服务器上首先需要将/etc/ssh/sshd_config中将RSA认证打开，*

*即：*

*1.RSAAuthentication yes*

*2.PubkeyAuthentication yes*

*3.AuthorizedKeysFile .ssh/authorized_keys*

*这里我们可以看到公钥存放在.ssh/authorized_keys文件中。*

*所以我们在/home/git下创建.ssh目录，然后创建authorized_keys文件，并将刚生成的公钥导入进去。*

*然后再次clone的时候，或者是之后push的时候，就不需要再输入密码了：*

*Zhu@XXX/E/testgit/8.34 $ git clone git@192.168.8.34:/data/git/learngit.git Cloning into 'learngit'... warning: You appear to have cloned an empty repository. Checking connectivity... done.*

===============================

收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。
```bash
$ cd /home/git/
$ mkdir .ssh #新建文件夹
$ chmod 700 .ssh 
$ touch .ssh/authorized_keys  #新建文件
$ chmod 600 .ssh/authorized_keys
```
第四步，初始化Git仓库
```bash
$ cd /home/git
$ git init --bare test.git
Initialized empty Git repository in /home/git/test.git/
```
以上命令会创建一个空仓库，服务器上的Git仓库通常都以.git结尾。



第五步、本地克隆仓库
```bash
$ git clone git@your-ip:test.git
Cloning into 'test'...
warning: You appear to have cloned an empty repository.
Checking connectivity... done.

```
your-ip 为您 Git 所在服务器 ip 

用git clone 获取服务器上的代码
```bash
[root@localhost code]$ git clone root@192.168.57.61:/root/code.git 
```
报错如下：
```
bash: git-upload-pack: command not found
fatal: The remote end hung up unexpectedly
```

什么原因呢？原来代码服务器【192.168.57.61】上的git安装路径是/usr/local/git，不是默认路径，根据提示，在git服务器192.168.57.61上， 建立链接文件：
```
[root@localhost code]# ln -s /usr/local/git/bin/git-upload-pack /usr/bin/git-upload-pack 
```
再次，执行git clone ，果真可以了。

当然，如果无法修改git代码服务器上配置，可以在clone时，添加--upload-pack选项来指定git服务器上的git-upload-pack 路径，达到上面相同的目的，如下所示：
```
[root@localhost code]$ git clone --upload-pack "/usr/local/git/bin/git-upload-pack" root@192.168.57.61:/root/code.git 
```
当然，也许你会遇到git-receive-pack 之类的错误，很有可能和这个原理是一样的，请采用类似的操作即可

5.禁止Shell登录
出于安全考虑，git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。 
找到类似下面的一行：
```
git:x:502:502::/home/git:/bin/bash
改为

git:x:502:502::/home/git:/usr/local/git/bin/git-shell
这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出
```


参考文档：
git安装部署  http://www.cnblogs.com/imayanlong/p/5617626.html 
git/ssh捋不清的几个问题  http://www.cnblogs.com/hustskyking/p/problems-in-git-when-ssh.html 
centos7下git服务器端搭建和验证  https://my.oschina.net/u/2343829/blog/644663  
Git 服务器搭建  http://www.runoob.com/git/git-server.html   
Windows下SSH连接git服务器  https://www.zybuluo.com/zhutoulwz/note/30495                        
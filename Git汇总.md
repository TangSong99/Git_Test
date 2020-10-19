## Git 分布式版本控制器

相较于`cvs`、`svn`这类集中式版本控制器，git的优势在于：
本地版本控制、重写提交说明、可以退回“后悔”、分支系统等。

SVN：增量的分支系统
Git：全量的分支系统（每一个版本都包含全部的文件,时刻保持数据的完整性）

![](https://s1.ax1x.com/2020/10/11/0cEkFg.png)

***

### git安装

1. 网址： `msysgit.github.io`

2. 安装：`Use git from git bash only..`,其他默认下一步

3. 配置path：`F\SystemApp\Git\bin`

4. 配置git（用户名+邮箱）：右键选中`Git Bash Here`

   `git config --global user.name "CTX"`

   `git config --global user.email "348958520@qq.com"`
   
   （注：`--system`为整个计算机、`--global`为当前用户、`--local`为当前项目）

### 建立git连接

1. 建立ssh免秘钥登陆

   本地生成：`ssh-keygen -t rsa -C 348958520@qq.com` + 一直回车
   （默认存放地址：C:\Users\CTX\.ssh）

   发往远端：（`个人GitHub→Settings→SSH and...→New SSH：title[协作用户名]+Key[C:\Users\CTX\.ssh\id_rsa.pub的内容]`

   测试连通：`ssh -T git@github.com`  
   （若`C:\Users\CTX\.ssh` 中出现`known_hosts`文件则表示成功连通）

2. 把本地文件初始化为git项目：
文件夹里右键选中 `Git Bash Here` + `git init`

3. 在GitHub上新建一个仓库生成（**区分大小写**）:
`git@github.com:TangSong99/Git_Test.git`

4. 本地项目和远程仓库关联
`git remote add origin git@github.com:TangSong99/Git_Test.git`
用`git remote -v`查看是否关联，若本机之前已存在可以先执行`git remote rm origin`

### 第一次发布

1. `git add .`
   将当前文件写入暂存区
   
2. `git commit -m "注释内容"`（-m "注释内容"可以替换为回车进行多注释声明）
   暂存区提交到本地分支，默认master（重复提交会显示`On branch master nothing to commit, working tree clean`，因为commit将文件从暂存区提交到对象区了，暂存区没有东西）
   
3. `git push -u origin master`
   作为主分支提交到仓库中，若不是新仓库即使是清空仓库也会失败（因为先前仓库的库版本会有一个`commit readme.md`而本地没有导致两个版本不一致，可以用pull更新本地再上传或者`git push -f origin master`强制覆盖。如果为空仓库建议重新建立仓库。）

### 第一次下载

1. `git clone git@github.com:TangSong99/Git_Test.git`
   下载到本地，**再次强调仓库地址区分带小写！**

### 提交本地更新（本地→远程）

1. `git add .`
2. `git commit -m "提交到分支"`
3. `git push origin master`

### 拉取仓库更新（远程→本地）

1. `git pull`

***

### 团队协作

发起者：
GitHub仓库的Settings→Manage acess→invite a collaborator→合作者GitHub全名/邮箱→发送邀请链接

协作者：
点击链接参与合作：clone、编写、提交更新（add/commit/push）

***

### 实际开发中的项目拉取

实际开发中公司一般用的是自己的GitLab，用申请到的帐号进行登陆，进去后先配一下本地的公钥，git中的name和email也要改成要求的格式。然后进去到对应的项目直接拉取master，开发时再在本地切换需要的分支就行了。

```
//本地git账户改名（修改之前的提交仍然使用的是之前的邮箱和名字），项目提交前也可以再检查一遍
git config --global "caitianxin"
git config --global "caitianxin@taqu.cn"

//下载项目,SSH下载不了就尝试用HTTP，HTTP需要帐号密码验证
git clone [global's SSH/HTTP]

//打开项目切换分支，VSCode左下角直接切换分支进行开发，可以查看当前分支：
git branch

//要提交前一定要记得先拉取项目到最新！
git pull

//（将AppVersion目录下的文件）提交到暂存区
git add src/views/appConfig/appVersion
//多个文件写法(逗号+空格)
git add src/views/a.vue, src/views/b.vue

//中途可以查看一下是否成功
git status

//提交到本地分支
git commit -m "注释尽量要详细、具体"

//提交到远程仓库
git push
```



***

### Git执行流程
#### Git 3+1种状态：

（已管理）：项目中被git管理的部分才有以下三种状态
已修改（modified）：修改后的代码、暂存区回到工作区（`git rm --cached`）
已暂存（staged）：执行add后
已提交（commited）：执行commit后

![](https://s1.ax1x.com/2020/10/11/0cEHcn.png)

(注：新版git用restore替换checkout)

通过`git init`将目录纳入git管理，默认为master分支。执行后会生成`.git`文件，它是git版本控制的目录。



* 从暂存区回到工作区：
  `git reset head [filename]`

* 删除已提交到对象区的文件

  `git rm a.txt`//删除本地文件和对象区中的a.txt，再把删除文件的操作指令退回到暂存区

  `git commit -m "彻底删除a.txt"//相当于重新提交了上一条语句，讲a.txt彻底删除`

***

#### Git 日志

`git log` / `git log -number`：查看（最近次数）提交的日志

```
$ git log
commit d433136c1d275fa26fdbf835cc4b2ecd4aeae4ec (HEAD -> master)
Author: CTX <348958520@qq.com>
Date:   Sun Oct 11 11:00:03 2020 +0800

    更新时间2020年10月11日 11:00:22

commit 8bca948ba633f2d93231560573c97da88df74a5b (origin/master, origin/HEAD)
Author: CTX <348958520@qq.com>
Date:   Sun Oct 11 09:49:54 2020 +0800

    更新git进阶

commit 480e0a9184d0f1691de70afe4bcad5943b41b2b8
Author: 唐宋丶 <52407046+TangSong99@users.noreply.github.com>
Date:   Sun Oct 11 09:08:36 2020 +0800

    Create 仓库说明.txt

```

`git log --pretty=online`：单行显示简要信息

```
$ git log --pretty=oneline
d433136c1d275fa26fdbf835cc4b2ecd4aeae4ec (HEAD -> master) 更新时间2020年10月11日 11:00:22
8bca948ba633f2d93231560573c97da88df74a5b (origin/master, origin/HEAD) 更新git进阶
480e0a9184d0f1691de70afe4bcad5943b41b2b8 Create 仓库说明.txt

```

`git log --pretty=format:"%h - %an ,%ar : %s"`：自定义格式化输出

```
$ git log --pretty=format:"%h - %an ,%ar : %s"
d433136 - CTX ,7 minutes ago : 更新时间2020年10月11日 11:00:22
8bca948 - CTX ,77 minutes ago : 更新git进阶
480e0a9 - 唐宋丶 ,2 hours ago : Create 仓库说明.txt
```

其中commit后面跟着的是用sha1计算出的随机数，用于区分是哪一次提交







***

### 记一次Git旧版本覆盖本地代码后的找回

**场景**：修改完代码后忘记先pull拉取到最新版本了，直接commit后无法提交报错

```
error: Your local changes to the following files would be overwritten by merge:
.env.development
vue.config.js
Please commit your changes or stash them before you merge.
```
卡在暂存区，这时候应该只需要用下面方法撤销commit即可，

```
git stash
git pull
git stash pop
```

但是我看了第一篇公众号的方法直接执行了`git rebase -i HEAD^`导致本地代码直接被昨天提交的覆盖了，真刺激。VSCode没有自带Local History插件后面用一下方法解决：

```
 git reflog  //查看最近提交状态及其更新的时间
 24d45df5 HEAD@{6}: commit: Android/IOS版本更新
 
 git reset --hard 24d45df5  //回退版本号
```

【提交前一定要pull同步更新代码！！！】




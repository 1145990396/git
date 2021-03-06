## 目录

- [安装Git](#安装Git)
- [创建版本库](#创建版本库)
- [时光机穿梭](#时光机穿梭)
	- [版本回退](#版本回退)
	- [工作区和暂存区](#工作区和暂存区)
	- [管理修改](#管理修改)
	- [撤销修改](#撤销修改)
	- [删除文件](#删除文件)
- [远程仓库](#远程仓库)
	- [添加远程库](#添加远程库)
	- [从远程库克隆](#从远程库克隆)
- [分支管理](#分支管理)
	- [创建与合并分支](#创建与合并分支)
	- [解决冲突](#解决冲突)
	- [分支管理策略](#分支管理策略)
	- [Bug分支](#Bug分支)
	- [Feature分支](#Feature分支)
	- [多人协作](#多人协作)
	- [Rebase](#Rebase)
- [标签管理](#标签管理)
	- [创建标签](#创建标签)
	- [操作标签](#操作标签)
- [使用GitHub](#使用GitHub)
- [自定义Git](#自定义Git)
	- [忽略特殊文件](#忽略特殊文件)
	- [配置别名](#配置别名)
	- [搭建Git服务器](#搭建Git服务器)
- [使用SourceTree](#使用SourceTree)

## 安装Git
- **在Linux上安装Git**

首先，你可以试着输入git，看看系统有没有安装Git：
> $ git
>
> The program 'git' is currently not installed. You can install it by typing:sudo apt-get install git

像上面的命令，有很多Linux会友好地告诉你Git没有安装，还会告诉你如何安装Git。

如果你碰巧用Debian或Ubuntu Linux，通过一条`sudo apt-get install` git就可以直接完成Git的安装，非常简单。

老一点的Debian或Ubuntu Linux，要把命令改为`sudo apt-get install git-core`，因为以前有个软件也叫GIT（GNU Interactive Tools），结果Git就只能叫`git-core`了。由于Git名气实在太大，后来就把GNU Interactive Tools改成gnuit，git-core正式改为git。

如果是其他Linux版本，可以直接通过源码安装。先从Git官网下载源码，然后解压，依次输入：`./config`，`make`，`sudo make install`这几个命令安装就好了。

- **在Mac OS X上安装Git**

如果你正在使用Mac做开发，有两种安装Git的方法。

一是安装homebrew，然后通过homebrew安装Git，具体方法请参考homebrew的文档：http://brew.sh/ 

第二种方法更简单，也是推荐的方法，就是直接从AppStore安装Xcode，Xcode集成了Git，不过默认没有安装，你需要运行Xcode，选择菜单“Xcode”->“Preferences”，在弹出窗口中找到“Downloads”，选择“Command Line Tools”，点“Install”就可以完成安装了。

![在Mac OS X上安装Git](https://www.liaoxuefeng.com/files/attachments/919018691743136/0)

Xcode是Apple官方IDE，功能非常强大，是开发Mac和iOS App的必选装备，而且是免费的！

- **在Windows上安装Git**

在Windows上使用Git，可以从[Git官网](https://git-scm.com/downloads)直接下载安装程序，然后按默认选项安装即可。

国内官网下载较慢，建议从[国内镜像下载](https://github.com/waylau/git-for-win)

安装完成后，在开始菜单里找到`Git`->`Git Bash`，蹦出一个类似命令行窗口的东西，就说明Git安装成功！
![在Windows上安装Git](https://github.com/1145990396/git/blob/master/Photo/1.png?raw=true)

安装完成后，还需要最后一步设置，在命令行输入：

> $ git config --global user.name "Your Name"
>
> $ git config --global user.email "email@email.com"

因为Git是分布式版本控制系统，所以每个机器都要有你的名字和email地址。

注意`git config`命令和`--global`参数，用了这个参数，表示你的机器上所以的Git库都会使用这个配置

## 创建版本库
什么是版本库呢，版本库又叫仓库，英文名repository，可以简单理解为一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改，删除，Git都能跟踪，任何时刻都可以追踪历史，或者在将来某个时刻可以还原

所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

> $ mkdir learngit
>
> $ cd learngit
>
> pwd
>
> /Users/wuxianxin/learngit 

`pwd`命令用于显示当前目录 

第二步，通过`git init`命令把这个目录变成Git的仓库

> $ git init
>
> Initialized empty Git repository in /Users/wuxianxin/learngit/.git/ 

仓库建好了，并且是一个空的仓库，文件夹下面会有一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到`.git`目录，那么它是一个隐藏的目录，用`ls -ah`命令就可以看见。也可以更改电脑属性看到

现在我们编写一个`readme.txt`文件，内容如下：
> Git is a version control system.
>
> Git is free software.

一定要放到learngit目录下（子目录也行），因为这是一个Git仓库，放到其他地方Git再厉害也找不到这个文件。

第一步，用命令`git add`告诉Git，把文件添加到仓库：
> $ git add readme.txt

执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

第二步，用命令`git commit`告诉Git，把文件提交到仓库：
> $ git commit -m "wrote a readme" 
>
> [master (root-commit) eaadf4e] wrote a readme file
>
> 1 file changed, 2 insertions(+)
>
> create mode 100644 readme.txt

简单解释下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容

`git commit`命令执行成功后会告诉你`1 file changed`1个文件被改动（我们新添加的readme.txt文件）,`2 insertions`：插入了两行内容（readme.txt有两行内容）。

为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：
> $ git add file1.txt
>
> $ git add file2.txt file3.txt
>
> $ git commit -m "add 3 files."
## 时光机穿梭
我们已经成功地添加并提交了一个readme.txt文件，现在，是时候继续工作了，于是，我们继续修改readme.txt文件，改成如下内容：
> Git is a distributed version control system.
>
> Git is free software.

现在，运行`git status`命令看看结果：
> $ git status
>
> On branch master
> Changes not staged for commit:
>  (use "git add <file>..." to update what will be committed)
>  (use "git checkout -- <file>..." to discard changes in working directory)
>	modified:   readme.txt
> no changes added to commit (use "git add" and/or "git commit -a")

`git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，`readme.txt`被修改过了，但还没有准备提交的修改。

虽然Git告诉我们`readme.txt`被修改了，但如果能看看具体修改了什么内容,需要用`git diff`这个命令看看：

> $ git diff readme.txt 
>
> diff --git a/readme.txt b/readme.txt
>index 46d49bf..9247db6 100644
> --- a/readme.txt
> +++ b/readme.txt
> @@ -1,2 +1,2 @@
> -Git is a version control system.
> +Git is a distributed version control system.
> Git is free software.

`git diff`顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，可以从上面的命令输出看到，我们在第一行添加了一个`distributed`单词。

知道了对`readme.txt`作了什么修改后，再把它提交到仓库，提交修改和提交新文件是一样的两步，第一步是git add：

> $ git add readme.txt

同样没有任何输出。在执行第二步`git commit`之前，我们再运行`git status`看看当前仓库的状态：

> $ git status
>
> On branch master
> Changes to be committed:
>  (use "git reset HEAD <file>..." to unstage)
>
>	modified:   readme.txt

`git status`告诉我们，将要被提交的修改包括`readme.txt`，下一步，就可以放心地提交了：

> $ git commit -m "add distributed"
>
> [master e475afc] add distributed
> 1 file changed, 1 insertion(+), 1 deletion(-)

提交后，我们再用`git status`命令看看仓库的当前状态：

> $ git status
>
> On branch master
> nothing to commit, working tree clean
### 版本回退
现在，你已经学会了修改文件，然后把修改提交到Git版本库，现在，再练习一次，修改readme.txt文件如下：

> Git is a distributed version control system.
>
> Git is free software distributed under the GPL.

然后尝试提交：

> $ git add readme.txt
>
> $ git commit -m "append GPL"
>
> [master 1094adb] append GPL
> 1 file changed, 1 insertion(+), 1 deletion(-)

像这样，你不断对文件进行修改，然后不断提交修改到版本库里,每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为`commit`。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

现在，我们回顾一下readme.txt文件一共有几个版本被提交到Git仓库里了：

版本1：wrote a readme file

> Git is a version control system.
>
> Git is free software.

版本2：add distributed

> Git is a distributed version control system.
>
> Git is free software.

版本3：append GPL

> Git is a distributed version control system.
>
> Git is free software distributed under the GPL.

当然了，在实际工作中，我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容，不然要版本控制系统干什么。版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用`git log`命令查看：
> $ git log
>
> commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
> Author: Michael Liao <askxuefeng@gmail.com>
> Date:   Fri May 18 21:06:15 2018 +0800
>    append GPL
> commit e475afc93c209a690c39c13a46716e8fa000c366
> Author: Michael Liao <askxuefeng@gmail.com>
> Date:   Fri May 18 21:03:36 2018 +0800
>    add distributed
> commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
> Author: Michael Liao <askxuefeng@gmail.com>
> Date:   Fri May 18 20:59:18 2018 +0800
>    wrote a readme file

`git log`命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是`append GPL`，上一次是`add distributed`，最早的一次是`wrote a readme file`。

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：
> $ git log --pretty=oneline
>
> 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
> e475afc93c209a690c39c13a46716e8fa000c366 add distributed
> eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file

每提交一个新版本，实际上Git就会把它们自动串成一条时间线

好了，现在我们启动时光穿梭机，准备把`readme.txt`回退到上一个版本，也就是`add distributed`的那个版本，怎么做呢？

首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交1094adb...（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

现在，我们要把当前版本`append GPL`回退到上一个版本`add distributed`，就可以使用`git reset`命令：

> $ git reset --hard HEAD^
>
> HEAD is now at e475afc add distributed

--hard参数有啥意义？这个后面再讲，现在你先放心使用。

看看`readme.txt`的内容是不是版本`add distributed`：

> $ cat readme.txt
>
> Git is a distributed version control system.
> Git is free software.

果然被还原了。

还可以继续回退到上一个版本`wrote a readme file`，不过且慢，然我们用`git log`再看看现在版本库的状态：

> $ git log
>
> commit e475afc93c209a690c39c13a46716e8fa000c366 (HEAD -> master)
> Author: Michael Liao <askxuefeng@gmail.com>
> Date:   Fri May 18 21:03:36 2018 +0800
>    add distributed
> commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
> Author: Michael Liao <askxuefeng@gmail.com>
> Date:   Fri May 18 20:59:18 2018 +0800
>    wrote a readme file

最新的那个版本append GPL已经看不到了！好比你从21世纪坐时光穿梭机来到了19世纪，想再回去已经回不去了，肿么办？

办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个append GPL的commit id是1094adb...，于是就可以指定回到未来的某个版本：

> $ git reset --hard 1094a
>
> HEAD is now at 83b0afe append GPL

版本号没必要写全，前几位就可以了，Git会自动去找。

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把`HEAD`从指向`append GPL`：
> ┌────┐
>
> │HEAD│
>
> └────┘
>   │
>
>   └──> ○ append GPL
>
>        │
>
>        ○ add distributed
>
>        │
>
>        ○ wrote a readme file

改为指向add distributed：

> ┌────┐
>
> │HEAD│
>
> └────┘
>
>   │
>
>   │    ○ append GPL
>
>   │    │
>
>   └──> ○ add distributed
>
>        │
>
>        ○ wrote a readme file

然后顺便把工作区的文件更新了。所以你让HEAD指向哪个版本号，你就把当前版本定位在哪。

现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的`commit id`怎么办？

Git提供了一个命令`git reflog`用来记录你的每一次命令：
> $ git reflog
>
> e475afc HEAD@{1}: reset: moving to HEAD^
> 1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
> e475afc HEAD@{3}: commit: add distributed
> eaadf4e HEAD@{4}: commit (initial): wrote a readme file
### 工作区和暂存区
Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。

- **工作区（Working Directory）**
就是你在电脑里能看到的目录
- **版本库（Repository）**

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

现在，我们再练习一遍，先对readme.txt做个修改，比如加上一行内容：
> Git is a distributed version control system.
>
> Git is free software distributed under the GPL.
>
>Git has a mutable index called stage.

然后，在工作区新增一个`LICENSE`文本文件（内容随便写）。

先用`git status`查看一下状态：

> $ git status

`readme.txt`被修改了 而`LICENSE`还从来没有被添加过，所以它的状态应该是`Untracked`。

现在，使用两次命令`git add`，把`readme.txt`和`LICENSE`都添加后，用`git status`再查看一下

一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：

> $ git status
>
> On branch master
> nothing to commit, working tree clean

### 管理修改
为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。
### 撤销修改

`git checkout -- file`可以丢弃工作区的修改：
> $ git checkout -- readme.txt

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

`git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。

如果已经`add`到暂存区了，在`commit`发现了这个问题用`git status`查看一下，修改只是添加到了暂存区，还没有提交

用命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区：

> $ git reset HEAD readme.txt
>
> Unstaged changes after reset:
> M	readme.txt

git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交,只能版本回退了

### 删除文件
有两个选择 一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：
> $ git rm test.txt
>
>rm 'test.txt'

> $ git commit -m "remove test.txt"
>
> [master d46f35e] remove test.txt
>  1 file changed, 1 deletion(-)
>  delete mode 100644 test.txt

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

> $ git checkout -- test.txt

git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

 `注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！`
## 远程仓库
请自行注册GitHub账号。由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

> $ ssh-keygen -t rsa -C "youremail@example.com"

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：

点“Add Key”，你就应该看到已经添加的Key：
### 添加远程库
首先，登陆GitHub，然后，在右上角找到“New repository”按钮，创建一个新的仓库：

在`Repository name`填入`learngit`，其他保持默认设置，点击`“Create repository”`按钮，就成功地创建了一个新的Git仓库：

现在，我们根据GitHub的提示，在本地的`learngit`仓库下运行命令：

> $ git remote add origin git@github.com:1145990396/learngit.git

添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。

下一步，就可以把本地库的所有内容推送到远程库上：

> $ git push -u origin master

把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。

`要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；`

`关联后，使用命令git push -u origin master第一次推送master分支的所有内容；`

`此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；`
### 从远程库克隆
命令git clone克隆一个本地库：

> $ git clone git@github.com:1145990396/gitskills.git

如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

`还可以用https://github.com/1145990396/git.git这样的地址。实际上，Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议。`

`Git支持多种协议，包括https，但ssh协议速度最快。`
## 分支管理
但Git的分支是与众不同的，无论创建、切换和删除分支，Git在1秒钟之内就能完成！无论你的版本库是1个文件还是1万个文件。
### 创建与合并分支
首先，我们创建`dev`分支，然后切换到`dev`分支：

> $ git checkout -b dev
> Switched to a new branch 'dev'

`git checkout`命令加上-b参数表示创建并切换，相当于以下两条命令：

> $ git branch dev
>
> $ git checkout dev
>
> Switched to branch 'dev'

然后，用`git branch`命令查看当前分支：
> $ git branch
>
> * dev
>
>  master

`git branch`命令会列出所有分支，当前分支前面会标一个*号。

然后，我们就可以在`dev`分支上正常提交，比如对`readme.txt`做个修改，加上一行：

> Creating a new branch is quick.

然后提交：

> $ git add readme.txt 
>
> $ git commit -m "branch test"
>
> [dev b17d20e] branch test
 1 file changed, 1 insertion(+)

现在，`dev`分支的工作完成，我们就可以切换回`master`分支：

>　$ git checkout master
>
> Switched to branch 'master'

切换回`master`分支后，再查看一个`readme.txt`文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：

现在，我们把`dev`分支的工作成果合并到`master`分支上：

> $ git merge dev

`git merge`命令用于合并指定分支到当前分支

合并完成后，就可以放心地删除dev分支了：
> $ git branch -d dev

删除后，查看branch，就只剩下master分支了：

> $ git branch

用switch更科学。因此，最新版本的Git提供了新的git switch命令来切换分支：
创建并切换到新的dev分支，可以使用：

> $ git switch -c dev

直接切换到已有的master分支，可以使用：

> $ git switch master


Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>或者git switch <name>`

创建+切换分支：`git checkout -b <name>或者git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`
### 解决冲突
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。
### 分支管理策略
合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

> $ git merge --no-ff -m "merge with no-ff" dev

### Bug分支
Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

> $ git stash

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

> $ git checkout master

> $ git checkout -b issue-101

现在修复bug，需要把“Git is free software ...”改为“Git is a free software ...”，然后提交：

> $ git add readme.txt 
>
>$ git commit -m "fix bug 101"

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

> $ git switch master
>
> $ git merge --no-ff -m "merged bug fix 101" issue-101

用git stash list命令看看刚才的工作现场存到哪去了

> $ git stash list

> 当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；
> 
> 在`master`分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。
### Feature分支
添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个`feature`分支，在上面开发，完成后，合并，最后，删除该`feature`分支。

> $ git switch -c feature-vulcan

5分钟后，开发完毕：

> $ git add vulcan.py
>
> $ git status
>
> $ git commit -m "add feature vulcan"

切回`dev`，准备合并：

> $ git switch dev

就在此时，接到上级命令，因经费不足，新功能必须取消！

> $ git branch -d feature-vulcan

销毁失败。Git友情提醒，`feature-vulcan`分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的`-D`参数。。

现在我们强行删除：

> $ git branch -D feature-vulcan

开发一个新`feature`，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。
### 多人协作
多人协作的工作模式通常是这样：

首先，可以试图用`git push origin <branch-name>`推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。


查看远程库信息，使用`git remote -v`；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；

在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。
### Rebase

> $ git rebase

rebase操作可以把本地未push的分叉提交历史整理成直线；

rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。
## 标签管理
### 创建标签
在Git中打标签非常简单，首先，切换到需要打标签的分支上：

> $ git switch <name>

然后，敲命令git tag <name>就可以打一个新标签：

> $ git tag v1.0

注意，标签不是按时间顺序列出，而是按字母排序的。可以用git show <tagname>查看标签信息：

> $ git show v1.0

还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：

> $ git tag -a v0.1 -m "version 0.1 released" 1094adb

用命令git show <tagname>可以看到说明文字：

> $ git show v0.1

命令git tag <tagname>用于新建一个标签，默认为HEAD，也可以指定一个commit id；

命令git tag -a <tagname> -m "blablabla..."可以指定标签信息；

命令git tag可以查看所有标签。
### 操作标签
如果标签打错了，也可以删除：

> $ git tag -d v0.1

如果要推送某个标签到远程，使用命令git push origin <tagname>：

> $ git push origin v1.0

或者，一次性推送全部尚未推送到远程的本地标签：

> $ git push origin --tags

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

> $ git tag -d v0.9

然后，从远程删除。删除命令也是push，但是格式如下：

> $ git push origin :refs/tags/v0.9

命令`git push origin <tagname>`可以推送一个本地标签；

命令`git push origin --tags`可以推送全部未推送过的本地标签；

命令`git tag -d <tagname>`可以删除一个本地标签；

命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。
## 使用GitHub
如何参与一个开源项目呢？比如人气极高的bootstrap项目，这是一个非常强大的CSS框架，你可以访问它的项目主页https://github.com/twbs/bootstrap，点“Fork”就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone：

> git clone git@github.com:twbs/bootstrap.git

在GitHub上，可以任意Fork开源仓库；

自己拥有Fork后的仓库的读写权限；

可以推送pull request给官方仓库来贡献代码。
## 自定义Git
让Git显示颜色，会让命令输出看起来更醒目：

> git config --global color.ui true

### 配置别名
有没有经常敲错命令？比如`git status`？`status`这个单词真心不好记。

如果敲`git st`就表示`git status`那就简单多了，当然这种偷懒的办法我们是极力赞成的。

我们只需要敲一行命令，告诉Git，以后`st`就表示`status`：

> $ git config --global alias.st status

好了，现在敲`git st`看看效果。

当然还有别的命令可以简写，很多人都用`co`表示`checkout`，`ci`表示`commit`，`br`表示`branch`：

> $ git config --global alias.co checkout
>
> $ git config --global alias.ci commit
>
> $ git config --global alias.br branch

配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

配置文件放哪了？每个仓库的Git配置文件都放在`.git/config`文件中：

别名就在`[alias]`后面，要删除别名，直接把对应的行删掉即可。

而当前用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中：
### 搭建Git服务器
搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的apt命令就可以完成安装。

假设你已经有sudo权限的用户账号，下面，正式开始安装。

第一步，安装git：

> $ sudo apt-get install git

第二步，创建一个git用户，用来运行git服务：

> $ sudo adduser git

第三步，创建证书登录：

收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。

第四步，初始化Git仓库：

先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：

> $ sudo git init --bare sample.git

Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：

> $ sudo chown -R git:git sample.git

第五步，禁用shell登录：

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

> git:x:1001:1001:,,,:/home/git:/bin/bash

改为：

> git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell

这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。

第六步，克隆远程仓库：

现在，可以通过git clone命令克隆远程仓库了，在各自的电脑上运行：

> $ git clone git@server:/srv/sample.git
>
> Cloning into 'sample'...
> warning: You appear to have cloned an empty repository.

剩下的推送就简单了。

管理公钥
如果团队很小，把每个人的公钥收集起来放到服务器的/home/git/.ssh/authorized_keys文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用Gitosis来管理公钥。

这里我们不介绍怎么玩Gitosis了，几百号人的团队基本都在500强了，相信找个高水平的Linux管理员问题不大。

管理权限
有很多不但视源代码如生命，而且视员工为窃贼的公司，会在版本控制系统里设置一套完善的权限控制，每个人是否有读写权限会精确到每个分支甚至每个目录下。因为Git是为Linux源代码托管而开发的，所以Git也继承了开源社区的精神，不支持权限控制。不过，因为Git支持钩子（hook），所以，可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的。`Gitolite`就是这个工具。

这里我也不介绍`Gitolite`了，不要把有限的生命浪费到权限斗争中。

小结
搭建`Git`服务器非常简单，通常10分钟即可完成；

要方便管理公钥，用`Gitosis`；

要像SVN那样变态地控制权限，用`Gitolite`。
## 使用SourceTree


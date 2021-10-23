# 基于Git的分布式代码管理

**变更履历**

---
本文档主要描述了Git的基本原理和使用方法，并介绍了GitLab的一些基本使用方法。

## <a name='目录'>目录</a>

1. [Git介绍](#Git介绍)
	1. [现在的问题](#现在的问题)
	1. [Git是什么](#Git是什么)
	1. [Git的特性](#Git的特性)
	1. [Git和SVN的区别](#Git和SVN的区别)
	1. [参考资料](#参考资料)
1. [GitLab介绍](#GitLab介绍)	
1. [Git实践](#Git实践)
	1. [Git的安装](#Git的安装)
	1. [Git的配置](#Git的配置)
	1. [Git常用命令](#Git常用命令)
		1. [注册GitLab](#注册GitLab)
		1. [下载项目](#下载项目)
		1. [提交修改](#提交修改)
		1. [分支操作](#分支操作)
		1. [解决冲突](#解决冲突)
		1. [查看状态](#查看状态)
1. [Git流程](#Git流程)
	1. [集中式工作流](#集中式工作流)
	1. [功能分支工作流](#功能分支工作流)
	1. [Gitflow工作流](#Gitflow工作流)
	1. [Forking工作流](#Forking工作流)
	1. [Pull Request工作流](#Pull Request工作流)


## <a name="Git介绍">Git介绍</a>

### <a name="现在的问题">现在的问题</a>
采用任何一个新技术都有风险，那么我们为什么要需要冒险采用新的技术？从根本上来说，使用新的技术是为了解决现有问题。我们需要**面向问题思考而不是面向技术思考**。

那么，我们现有的代码管理流程上有什么样的一些问题呢？

+ **难以从流程上保证代码评审**

常理上来说，我们应该是代码评审之后再提交代码，但是实际上来说，没有提交代码的情况下，我只能让别人在我的电脑本地上看代码的diff，这样让代码评审的推行难度提高很多。反之如果我提交了代码之后，那么这些代码已经进入了测试周期，又会破坏代码评审之后再进行测试的原则。

+ **分支合并痛苦**

分支基本上只能由SCM来建立，而不同版本的代码合并又需要程序员自己来维护，我们很难通过SVN现有的分支合并来进行代码合并。只能重复的向不同版本拷贝并提交各种修改内容，人肉合并修改内容。

+ **一边开发一边提交修改经常会漏文件**

在开发新功能的时候，对应紧急的bug修改，常常让本地的环境很复杂，我们搞不清应该提交哪些文件？要么花很长时间来checkout一个干净环境提交，要么提心吊胆的提交之后让别的同学帮着看看。

+ **提交日志混乱**

多次提交内容，对应同一个问题，后来的人很难确定这次修改是为了解决什么问题。


### <a name="Git是什么">Git是什么</a>
Git是一种分布式版本管理系统，来源于 Linux 的缔造者 Linus Torvalds 。自从2005年Git发明以来，Git迅速成为最流行的分布式版本控制系统，尤其是2008年，GitHub网站上线了，它为开源项目免费提供Git存储，无数开源项目开始迁移至GitHub，包括jQuery，PHP，Ruby等等。

Git的初始设计目标是：

* 速度
* 简单的设计 
* 对非线性开发模式的强力支持（允许上千个并行开发的分支） 
* 完全分布式 
* 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

对于版本管理系统大家都很熟悉了，CVS和SVN，包括之前的VSS都是版本管理系统。那么分布式是一个什么样的概念？在分布式的版本管理系统（Distributed Version Control System，简称 DVCS ）中，客户端并不只提取最新版本的文件快照，而是把原始的代码仓库完整地镜像下来。这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜 像出来的本地仓库恢复。因为每一次的提取操作，实际上都是一次对代码仓库的完整备份。

分布式版本管理系统的结构如下所示：

![Git-arch](https://gitee.com/miawei/pic-go-img/raw/master/imgs/Git-arch.png)

从上图我们可以看出，与我们现行的SVN相比，最大的改变来自于两点。

+ **增加本地仓库**：增加本地仓库是处于分布式的考虑，我们在每台机器上都有所有代码的完全备份和修改履历，即使远端服务器完全宕机，也对本地没有任何影响，只是我们不能再通过远端服务器来交换提交代码。

+ **增加暂存区(Stage)**：在提交本地仓库之前，先需要提交到暂存区，我们可以把暂存区想象成一个购物车，我们在最终付款（提交代码）之前，可以先把所有要提交的代码都放到暂存区，并整理提交记录，最终再向仓库一次性提交。


### <a name="Git的特性">Git的特性</a>

+ **直接记录快照，而非差异比较**

Git 和其他版本控制系统的主要差别在于，Git 只关心文件数据的整体是否发生变化，而大多数其他系统则只关心文件内容的具体差异。

VCS的文件存储方式如下：

![VCS-files](https://gitee.com/miawei/pic-go-img/raw/master/imgs/VCS-files.png)

Git的文件存储方式如下：

![Git-files](https://gitee.com/miawei/pic-go-img/raw/master/imgs/Git-files.png)

+ **近乎所有操作都是本地执行**

在 Git 中的绝大多数操作都只需要访问本地文件和资源，不用连网。但如果用 CVCS 的话，差不多所有操作都需要连接网络。因为 Git 在本地磁盘上就保存着所有当前项目的历史更新，所以处理起来速度飞快。

+ **多数操作仅添加数据**

常用的 Git 操作大多仅仅是把数据添加到数据库。因为任何一种不可逆的操作，比如删除数据，都会使回退或重现历史版本变得困难重重。在别的 VCS 中，若还未提交更新，就有可能丢失或者混淆一些修改的内容，但在 Git 里，一旦提交快照之后就完全不用担心丢失数据，特别是养成定期推送到其他仓库的习惯的话。

+ **时刻保持数据完整性**

在保存到 Git 之前，所有数据都要进行内容的校验和（checksum）计算，并将此结果作为数据的唯一标识和索引。换句话说，不可能在你修改了文件或目录之后，Git 一无所知。这项特性作为 Git 的设计哲学，建在整体架构的最底层。所以如果文件在传输时变得不完整，或者磁盘损坏导致文件数据缺失，Git 都能立即察觉。

+ **文件的三种状态**

对于任何一个文件，在 Git 内都只有三种状态：已提交（committed），已修改（modified）和已暂存（staged）。已提交表示该文件已经被安全地保存在本地数据库 中了；已修改表示修改了某个文件，但还没有提交保存；已暂存表示把已修改的文件放在下次提交时要保存的清单中。

由此我们看到 Git 管理项目时，文件流转的三个工作区域：Git 的工作目录，暂存区域（Stage），以及本地仓库。

![Git-state](https://gitee.com/miawei/pic-go-img/raw/master/imgs/Git-state.png)

### <a name="为什么要使用Git">为什么要使用Git</a>
回到我们最初的问题点，Git可以这样来解决我们的问题。

1. 通过pull request的工作流来保证一个体验很好的代码评审流程
2. 通过强大的分支管理功能，来优化我们分支合并，提高我们的工作效率
3. 通过本地仓库，我们能够整理每次需要提交的内容，而且通过灵活的分支管理，我们可以在不同的分支直接灵活切换
4. 通过本地的暂存区，我们能够让我们每次提交都是有意义的

### <a name="Git和SVN的区别">Git和SVN的区别</a>

1. GIT是分布式的，SVN不是
1. GIT把内容按元数据方式存储，而SVN是按文件
1. GIT分支和SVN的分支不同
1. GIT没有一个全局的版本号，而SVN有
1. GIT的内容完整性要优于SVN
1. Git下载下来后，在本地不必联网就可以看到所有的log，很方便学习，SVN却需要联网

### <a name="参考资料">参考资料</a>

+ [Git官方教程](https://git-scm.com/book/zh/v2)
+ [国内一个比较全面的Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
+ [Pull Request工作流](http://blog.jobbole.com/76854/)
+ [Pull Request介绍](http://www.cnblogs.com/kidsitcn/p/5319282.html)
+ [GIT命令一览](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)
+ [关于GIT的一些八卦](http://blog.dyngr.com/blog/2013/09/26/junio-c-hamano-interview/)

[[↑TOP]](#目录)

## <a name="Gitlab介绍">Gitlab介绍</a>
简单一句话来说，Gitlab是一个可以私有化部署的GitHub。

GitLab是利用 Ruby on Rails 一个开源的版本管理系统，实现一个自托管的Git项目仓库，可通过Web界面进行访问公开的或者私人项目。它拥有与Github类似的功能，能够浏览源代码，管理缺陷和注释。可以管理团队对仓库的访问，它非常易于浏览提交过的版本并提供一个文件历史库。团队成员可以利用内置的简单聊天程序(Wall)进行交流。它还提供一个代码片段收集功能可以轻松实现代码复用，便于日后有需要的时候进行查找。
	
我们已经在内网上部署了Gitlab，大家可以通过下面的网址访问。

`http://192.168.1.188/users/sign_in`	

## <a name="Git实践">Git实践</a>

### <a name="Git的安装">Git的安装</a>
Git有很多GUI的工具，但是为了理解Git的操作原理，我推荐大家先从命令行开始掌握，大家可以从下面的官网上下载:

`https://git-scm.com/downloads`

可以直接使用小米盘中的下载镜像:

`\\xiaomi\04_开发工具\38_Git`


### <a name="Git的配置">Git的配置</a>

**设置用户名**

请使用汉字名称注册用户名和邮箱。

```bash
$ git config --global user.name "章承科"
$ git config --global user.email zhangck@repeatlink.cn
```

**设置gitignore**

工程中总有一些我们不能提交到仓库的文件（比如.class文件/一些本地配置文件），我们可以通过`.gitignore`文件来设定哪些文件是我们需要忽略的。

大家可以从[这个地址]((https://github.com/github/gitignore))下载一些基础文件，然后根据自己需要修改


**设置不进行回车换行转换**

由于Linux默认的回车换行符和Windows的不太一样，所以git默认的行为会对所有文件的回车换行符进行转换，我们可以用下面的设定关掉这个行为。

`git config --global core.autocrlf false`


### <a name="Git常用命令">Git常用命令</a>

下面是Git的一些常用命令。我们将使用这些常用命令来完成一些简单操作。

```bash
git checkout
git status
git checkout -b
git commit
git diff
git add
git clean -fdx
git fetch --prune
git pull --ff-only
git branch -D
git push
git merge
git rm
git rebase
git reset
git revert
git push origin :yourbranch
```

![GitCommand](https://gitee.com/miawei/pic-go-img/raw/master/imgs/GitCommand.png)


#### <a name="注册GitLab">注册GitLab</a>
首先我们需要在内网环境的GitLab中注册一个用户，请注意如下规范。

+ **姓名**使用汉字全名。如章承科
+ **用户名**使用拼音全拼。如zhangchengke
+ **邮箱**使用公司邮箱。如zhangck@repeatlink.cn

注册之后我们需要设定SSH的免密登录。具体的设定方法如下所示：

[SSH设定方法](http://192.168.1.188/help/ssh/README)

需要注意的是：

+ 在GitLab的个人资料设定>>SSH密钥进行设定
+ 生成密钥的时候，请使用`用户ID@192.168.1.188`的格式，比如`zhangchengke@192.168.1.188`
+ 设置成功之后使用`ssh -T git@192.168.1.188`进行验证

#### <a name="下载项目">下载项目</a>
输入命令下载一个项目到本地仓库，请注意项目的URL表示可以从项目的页面直接拷贝。

`git clone git@192.168.1.188:git-demo/demo1.git`

和SVN不同的是，我们下载的是一个包含修改履历的完整项目到本地仓库。


#### <a name="提交修改">提交修改</a>
在本地修改了文件之后，我们可以有两种方式提交我们的修改。

**两段提交**
两段提交的意思是我们先把修改内容提交到Stage（暂存区），再提交到本地代码仓库。具体做法如下：

```
git add test.txt
git add test2.txt
git commit -m "modify test.txt" test.txt test2.txt
```

*Tip*:两段提交是推荐做法，因为暂存区的存在意义就是整理我们所有需要提交的内容。核心目的是为了保证提交的原子性。


**一段提交**
一段提交的意思是跳过Stage，直接提交到本地代码仓库。具体做法如下：

```
git commit -a
```


#### <a name="分支操作">分支操作</a>
让我们来看一个简单的分支新建与分支合并的例子，实际工作中你可能会用到类似的工作流。 你将经历如下步骤：

1. 开发某个网站。
2. 为实现某个新的需求，创建一个分支。
3. 在这个分支上开展工作。

正在此时，你突然接到一个电话说有个很严重的问题需要紧急修补。 你将按照如下方式来处理：

1. 切换到你的线上分支（production branch）。
2. 为这个紧急任务新建一个分支，并在其中修复它。
3. 在测试通过之后，切换回线上分支，然后合并这个修补分支，最后将改动推送到线上分支。
4. 切换回你最初工作的分支上，继续工作。

要实现这样的工作流，我们需要这样来操作：

1. 为实现新需求，切出分支iss53
```bash
$ git checkout -b iss53
Switched to a new branch "iss53"
```
2. 在分支中修改并提交内容。
```bash
$ vim index.html
$ git add index.html
$ git commit -m 'added a new footer [issue 53]'
```
3. 现在你需要紧急修改线上问题。首先需要把所有修改都提交了之后，切换到master。此时你得到了一个干净的可以修改线上问题的环境。
```bash
$ git checkout master
Switched to branch 'master'
```
4. 从master切出为了修改线上问题的分支hotfix，修改并提交直到修改完成。
```bash
$ git checkout -b hotfix
Switched to a new branch 'hotfix'
$ vim index2.html
$ git add index2.html
$ git commit -m 'fixed the broken email address'
[hotfix 1fb7853] fixed the broken email address
 1 file changed, 2 insertions(+)
```
5. 在hotfix中修改完之后，我们需要把分支给合并到主分支中去。合并完分支之后，可以删除掉分支。
```bash
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
$ git branch -d hotfix
Deleted branch hotfix (3a0874c).
```
6. hotfix对应完毕之后，我们可以把主分支推送到远端仓库
```bash
$ git push origin master
```
7. 完成之后，你可以切换回iss53分支继续修改。
```bash
$ git checkout iss53
Switched to branch "iss53"
$ vim index.html
$ git commit -a -m 'finished the new footer [issue 53]'
[iss53 ad82d7a] finished the new footer [issue 53]
1 file changed, 1 insertion(+)
```

#### <a name="解决冲突">解决冲突</a>
在协作开发中，一种通常的模式是，我们从远端仓库clone一个项目，添加内容之后提交。但是如果我们需要和别人协作修改一个文件，这时候我们往往需要把远端服务器的最新内容给同步到本地服务器，在这种情况下，如果远端服务器的修改内容和本地服务器的修改内容有可能发生冲突。

![rebase1](https://gitee.com/miawei/pic-go-img/raw/master/imgs/rebase1.png)

通过git pull命令，可以把远端仓库的修改内容直接同步到本地。

```bash
$ git pull
Auto-merging feature.txt
CONFLICT (content): Merge conflict in feature.txt
Automatic merge failed; fix conflicts and then commit the result.
```

从提示消息可以看出，出现了冲突，我们需要解决冲突。
修改feature.txt文件，重新提交之后即可以解决冲突，我们也可以使用`git mergetool`来解决冲突。
解决冲突之后的再把修改提交到远端仓库。

```
$ git push origin master
Counting objects: 13, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (13/13), 1.39 KiB | 0 bytes/s, done.
Total 13 (delta 6), reused 0 (delta 0)
To 192.168.1.188:git-demo/demo1.git
   dab8288..55d8f72  master -> master
```

解决完冲突之后的示意图如下所示

![rebase2](https://gitee.com/miawei/pic-go-img/raw/master/imgs/rebase2.jpg)


#### <a name="查看状态">查看状态</a>
下面介绍一些常见的查看状态的命令。

+ 通过`$ git status`我们可以查看当前目标的各个文件的状态
+ 通过`$ git diff`我们可以查看文件的差异
+ 通过`$ git log --graph`我们可以查看当前提交的版本历史


## <a name="Git流程">Git流程</a>
Git只是一个工具，Git的工作流才是我们关注的重点。使用Git我们应该遵循怎样的流程规范，才能最大程度发挥Git的强大功能？一般来说，Git有下面几种工作流程。

### <a name="集中式工作流">集中式工作流</a>
集中式工作流基本上来说非常适合使用Subversion的团队，集中式工作流让你无需去适应一个全新流程就可以体验Git带来的收益。这个工作流也可以作为向更Git风格工作流迁移的友好过渡。

![git-workflow-svn](https://gitee.com/miawei/pic-go-img/raw/master/imgs/git-workflow-svn.png)

[详细介绍](http://blog.jobbole.com/76847/)


### <a name="功能分支工作流">功能分支工作流</a>
功能分支工作流以集中式工作流为基础，不同的是为各个新功能分配一个专门的分支来开发。这样可以在把新功能集成到正式项目前，用Pull Requests的方式讨论变更。

![git-workflow-feature_branch](https://gitee.com/miawei/pic-go-img/raw/master/imgs/git-workflow-feature_branch.png)

[详细介绍](http://blog.jobbole.com/76857/)


### <a name="Gitflow工作流">Gitflow工作流</a>
Gitflow工作流通过为功能开发、发布准备和维护分配独立的分支，让发布迭代过程更流畅。严格的分支模型也为大型项目提供了一些非常必要的结构。

![git-workflows-gitflow](https://gitee.com/miawei/pic-go-img/raw/master/imgs/git-workflows-gitflow.png)

[详细介绍](http://blog.jobbole.com/76867/)


### <a name="Forking工作流">Forking工作流</a>
Forking工作流是分布式工作流，充分利用了Git在分支和克隆上的优势。可以安全可靠地管理大团队的开发者（developer），并能接受不信任贡献者（contributor）的提交。

![git-workflow-forking](https://gitee.com/miawei/pic-go-img/raw/master/imgs/git-workflow-forking.png)

[详细介绍](http://blog.jobbole.com/76861/)


### <a name="Pull Request工作流">Pull Request工作流</a>
Pull requests是Bitbucket提供的让开发者更方便地进行协作的功能，提供了友好的Web界面可以在提议的修改合并到正式项目之前对修改进行讨论。

![pull-request](https://gitee.com/miawei/pic-go-img/raw/master/imgs/pull-request.png)

[详细介绍](http://blog.jobbole.com/76854/)

![Git-struct](https://gitee.com/miawei/pic-go-img/raw/master/imgs/Git-struct.png)

## 分支策略

完整的git版本分支管理内容如下：

- master: 主分支

  ```
  主要用来版本发布线上版本,不能在该分支上直接开发和提交代码,只有release 分支和hotfix分支可以合并到该分支上
  ```

- develop:基于master分支克隆日常开发分支

  ```
  该分支正常保存了开发的最新代码
  	不在该分支上开发，在feature分支上开发具体的新功能然后合并到该分支上
  	当某一期的功能全部开发完成就合并到release分支，打包给测试人员测试
  	不能动master那么就克隆一个它的分支,进行代码操作,然后最终合并-项目经理操作
  ```

- feature：具体的功能开发分支

  ```
  基于develop分支克隆
  	新功能开发完成并自测通过后合并到develop分支
  	只与 develop 分支交互
  	具体的功能开发分支,----组长操作的
  ```

- release：release 分支可以认为是 master 分支的未测试版

  ```
  基于develop分支克隆
  	该分支可以认为是 master 分支的未测试版，主要用于打包给测试人员测试
  	比如说某一期的功能全部开发完成，那么就将 develop 分支合并到 release 分支，测试没有问题就合并到 master 分支，只有develop分支可以合并到该分支上
  	在测试过程中发现的BUG就从该分支再切一个分支进行修复，修复完之后再合并到develop分支
  	专门测试主干代码的分支,基于master的
  ```

- hotfix：线上紧急bug修复分支

  ```
  基于master分支克隆
  	用于对线上版本的BUG进行修复
  	BUG修复完之后需要合并到develop和master分支
  	bug修复分支!基于master的
  ```

总体就是这样的:

![image-20211023161148576](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023161148576.png)

## 其他注意点

在idea中使用切分支要注意:

![image-20211023161456039](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023161456039.png)

我们提交代码都commit提交到本地分支上的,然后push的时候才会将本地分支的代码提交到远程分支上,idea创建分支的时候是创建本地分支,然后提交的时候需要远程仓库跟本地也有一个一模一样的分支才可以!

切分支,得先换到master,因为要基于xx分支去切换,但是只是切了一个本地分支,我们在push的时候如果远程没有这个分支就会重新在远程创建一个新的分支!

> 其实在这个过程中,我们需要记住一个点就是我们提交代码commit都是提交到本地分支上的,而push则是将本地分支上的代码提交到对应远程仓库中去!

#### 合并代码

合并分支有两种方式:

1. IDEA

   - 要合并到哪个分支,那么我们首先得切换到那个分支上去操作,比如修改dev2,合并到master,那么我就切换到master

     ![image-20211023171735822](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023171735822.png)

   - 在master分支上点击dev2进行合并,但是此时只是合并到本地分支上,所以我们需要push远程仓库中!

   注意:这种方式需要一定的权限才可以,因为合并是需要上级有权限才可以同意合并

2. web界面

   - 我们需要发起一个合并请求,让上级有权限的人同意才可以合并成功
   - 在pull Requests中就可以点击,我们创建合并请求写明请求内容,然后就直接提交
   - 领导看到就会同意然后合并!

   比如:

   1. 创建Pull Request合并请求

   ![image-20211023164628531](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023164628531.png)

   2. 填写请求信息

      ![image-20211023164731132](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023164731132.png)

   3. 这时有权限的上级才可以同意合并

      ![image-20211023164806142](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023164806142.png)

   4. 这时就可以完成合并

      ![image-20211023164847459](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023164847459.png)

      ![image-20211023164853003](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023164853003.png)


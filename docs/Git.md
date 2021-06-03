# Git介绍

版本控制系统

本地版本控制系统->集中化的版本控制系统->分布式的版本控制系统(Git是分布式的版本控制系统)

# Git仓库

英文名**repository**，您可以简单理解成一个目录，这个目录里面的所有文件都可以被 Git 管理起来，每个文件的修改、删除， Git 都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

## 初始化仓库

首先，选择一个合适的地方，创建一个空目录。

```git
# open shell (PowerShell on Windows) in a proper directory
$ mkdir learngit
$ cd learngit
$ pwd
/users/hope-studio/learngit
```

`pwd` 命令用于显示当前目录。

第二步，通过 `git init` 命令把这个目录变成 Git 可以管理的仓库:

```
$ git init
Initialized empty Git repository in /users/hope-studio/learngit/.git/

```

瞬间 Git 就把仓库建好了，而且告诉您是一个空的仓库(empty Git repository)，可以发现当前目录下多了一个.git 的目录，这个目录是 Git 来跟踪管理仓库的，如果这个目录里面的文件破坏了，Git 仓库也破坏了。

## 把文件添加到仓库

首先这里再明确一下，所有的版本控制系统，其实只能跟踪**文本文件**的改动，比如 TXT 文件，网页，所有的程序代码等等，Git 也不例外。 所以要使用版本控制系统，就要以纯文本方式编写文件。

强烈建议使用标准的 UTF-8 编码，所有语言使用同一种编码，既没有冲突，又被所有平台所支持。

现在编写一个 readme.txt 文件，内容如下:

```
Git is a version control system.
Git is free software.
```

一定要放到 `learngit` 目录下(子目录也行)，因为这是一个 Git 仓库，放到其他地方 Git 再厉害也找不到这个文件。

把一个文件放到 Git 仓库只需要两步:

1.用命令 `git add` 告诉 Git，把文件添加到仓库:

```sh
git add readme.txt
```

执行上面的命令，没有任何显示，这就对了，Unix 的哲学是“没有消息就是好消息”，说明添加成功。

2.用命令 `git commit` 告诉 Git，把文件提交到仓库:

```sh
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

`git commit` 命令，`-m` 后面输入的是本次提交的说明，这样您就能从历史记录里方便地找到改动记录。

## 小结

- 初始化一个 Git 仓库，使用 `git init` 命令。
- 添加文件到 Git 仓库，分两步:
  1. 使用命令 `git add <file>`，可反复多次使用，添加多个文件；
  2. 使用命令 `git commit -m <message>`，完成。

# 纵向查看

我们已经添加并提交了一个readme.txt文件，继续修改此文件，改成如下内容：

```
Git is a distributed version control system.
Git is free software.
```

现在，运行 `git status` 命令看看结果:

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

`git status` 命令可以输出仓库当前的状态，上面的命令输出告诉我们，readme.txt 被修改过了，但还没有准备提交的修改。

简洁的输出：

`git status` 命令的输出十分详细，但其用语有些繁琐。Git 有一个选项可以帮您缩短状态命令的输出，这样可以以简洁的方式查看更改。如果您使用 `git status -s` 命令或 `git status --short` 命令，您将得到一种格式更为紧凑的输出。

输出中有两栏，左栏指明了暂存区的状态，右栏指明了工作区的状态。

Git 现在只告诉我们 readme.txt 被修改了，我们用 `git diff` 这个命令能看看具体修改了什么内容:

知道了对 readme.txt 作了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步是 `git add`:

同样没有任何输出。在执行第二步 `git commit` 之前，我们再运行 `git status` 看看当前仓库的状态:

`git status` 告诉我们，将要被提交的修改包括 readme.txt，下一步，就可以放心地提交了:

提交后，我们再用 `git status` 命令看看仓库的当前状态:

Git 告诉我们当前没有需要提交的修改，而且，工作目录是干净(working tree clean)的。

## 小结

- 要随时掌握工作区的状态，使用 `git status` 命令。
- 如果 `git status` 告诉您有文件被修改过，用 `git diff` 可以查看修改内容。

# 撤销操作

有些撤消操作是不可逆的。这是在使用 Git 的过程中，会因为操作失误而导致之前的工作丢失的少有的几个 地方之一。

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。此时，可以运行带有 `--amend` 选 项的提交命令来重新提交:

```
git commit --amend
```

这个命令会将暂存区中的文件提交。如果自上次提交以来您还未做任何修改(例如，在上次提交后马上执行了 此命令)， 那么快照会保持不变，而您所修改的只是提交信息。

文本编辑器启动后，可以看到之前的提交信息。编辑后保存会覆盖原来的提交信息。

例如，您提交后发现忘记了暂存某些需要的修改，可以像下面这样操作:

```
git commit -m 'initial commit'
git add forgotten_file
git commit --amend
```

最终您只会有一个提交——第二次提交将代替第一次提交的结果。

# 版本回退

修改 readme.txt 文件如下:

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

然后尝试提交:

```
$ git add readme.txt
# nothing displayed
$ git commit -m "append GPL"
[master 1094adb] append GPL
 1 file changed, 1 insertion(+), 1 deletion(-)
```

## 查看版本信息

版本控制系统用 `git log` 命令可以告诉我们历史记录，在 Git 中，我们查看:

`git log` 命令显示从最近到最远的提交日志

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上 `--pretty=oneline` 参数:

## 回退版本

如果要把版本回退到上一个版本，也就是 `add distributed` 的那个版本，怎么做呢?

首先，Git 必须知道当前版本是哪个版本，在 Git 中，用 `HEAD` 表示当前版本，也就是最新的提交 `1094adb...` (注意我的提交 ID 和您的肯定不一样)，上一个版本就是 `HEAD^`，上上一个版本就是 `HEAD^^`，当然往上 100 个版本写 100 个 `^` 比较容易数不过来，所以写成 `HEAD~100`。

现在，要把当前版本 `append GPL` 回退到上一个版本 `add distributed`，就可以使用 `git reset` 命令:

```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

看看 readme.txt 的内容是不是版本 add distributed:

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
```

果然被还原了。

最新的那个版本 append GPL 已经看不到了! 肿么办?

办法其实还是有的，只要上面的命令行窗口还没有被关掉，您就可以顺着往上找啊找啊，找到那个 append GPL 的 commit id 是 1094adb...，于是就可以指定回到未来的某个版本:

```sh
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```

版本号没必要写全，前几位就可以了，Git 会自动去找。当然也不能只写前一两位，因为 Git 可能会找到多个版本号，就无法确定是哪一个了。

## 找回commit id

如果回退到了某个版本找不到新版本的 `commit id`，在 Git 中，就无法用 `$ git reset --hard HEAD^` 回退。

所以 Git 提供了一个命令 `git reflog` 用来记录您的每一次命令:

## 小结

- `HEAD` 指向的版本就是当前版本，因此，Git 允许我们在版本的历史之间穿梭，使用命令 `git reset --hard commit_id`。
- 用 `git log` 可以查看提交历史，以便确定要回退到哪个版本。
- 用 `git reflog` 查看命令历史，以便确定要回到未来的哪个版本。

# 工作区和暂存区

Git 和其他版本控制系统如 SVN 的一个不同之处就是有暂存区的概念。

## 工作区

就是您在电脑里能看到的目录，比如 learngit 文件夹就是一个工作区:

## 版本库

工作区有一个隐藏目录.git，这个不算工作区，而是 Git 的版本库。

Git 的版本库里存了很多东西，其中最重要的就是称为 stage(或者叫 index)的暂存区，还有 Git 为我们自动创建的第一个分支 `master`，以及指向 master 的一个指针叫 `HEAD`。

<!-- ![版本库图例](https://mrhope.site/assets/img/git3.086f5ec6.jpg) -->

前面讲了我们把文件往 Git 版本库里添加的时候，提交更改，实际上就是把暂存区的所有内容提交到当前分支。我们创建 Git 版本库时，Git 自动为我们创建了唯一一个 `master` 分支，所以，现在，`git commit` 就是往 master 分支上提交更改。

您可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

# 修改管理

## 管理修改

Git 比其他版本控制系统设计得优秀，因为 Git 跟踪并管理的是修改，而非文件。

您会问，什么是修改? 比如您新增了一行，这就是一个修改，删除了一行，也是一个修改，更改了某些字符，也是一个修改，删了一些又加了一些，也是一个修改，甚至创建一个新文件，也算一个修改。

用 `git diff HEAD -- readme.txt` 命令可以查看工作区和版本库里面最新版本的区别:

### 管理小结

现在，您又理解了 Git 是如何跟踪修改的，每次修改，如果不用 `git add` 到暂存区，那就不会加入到 `commit` 中。

## 撤销对文件的修改

`git checkout -- <file>` 可以丢弃工作区的修改:

```sh
git checkout -- readme.txt
```

命令 `git checkout -- readme.txt` 意思就是，把 readme.txt 文件在工作区的修改全部撤销，这里有两种情况:

一种是 readme.txt 自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是 readme.txt 已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次 `git commit` 或 `git add` 时的状态。

`git checkout -- file` 命令中的 `--` 很重要，没有 `--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到 `git checkout` 命令。

## 取消暂存的文件

用命令 `git reset HEAD <file>` 可以把暂存区的修改撤销掉(unstage)，重新放回工作区:

`git reset` 命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用 `HEAD` 时，表示当前的指针(最新的版本)。

再用 `git status` 查看一下，现在暂存区是干净的，工作区有修改:

还记得如何丢弃工作区的修改吗?

```sh
git checkout -- readme.txt

$ git status
On branch master
nothing to commit, working tree clean
```

整个世界终于清静了!

那么如果您想要丢弃工作区和暂存区全部更改呢? 不要忘了 `HEAD` 就是当前指针，所以

```sh
git reset --hard HEAD
```

就是丢弃工作区与暂存区的全部文件啦。

### 撤销小结

场景 1: 当您改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令 `git checkout -- file`。

场景 2: 当您不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令 `git reset HEAD <file>`，就回到了场景 1，第二步按场景 1 操作。

场景 3: 已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

## 删除文件

### 如何删除文件

在 Git 中，删除也是一个修改操作。我们实战一下，先添加一个新文件 test.txt 到 Git 并且提交:

一般情况下，您通常直接在文件管理器中把没用的文件删了，或者用 `rm` 命令删了:

```sh
rm test.txt
```

这个时候，Git 知道您删除了文件，因此，工作区和版本库就不一致了，`git status` 命令会立刻告诉您哪些文件被删除了:

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

现在您有两个选择，一是确实要从版本库中删除该文件，那就用命令 `git rm` 删掉，并且 `git commit`:

```sh
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

现在，文件就从版本库中被删除了。

### 撤销删除

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本:

```sh
git checkout -- test.txt
```

`git checkout` 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

### 删除小结

命令 `git rm` 用于删除一个文件。如果一个文件已经被提交到版本库，那么您永远不用担心误删，但是要小心，您只能恢复文件到最新版本，您会丢失最近一次提交后您修改的内容。

# 远程仓库

## 什么是远程仓库

最后友情提示，在 GitHub 上免费托管的 Git 仓库，任何人都可以看到喔(但只有您自己才能改)。所以，不要把敏感信息放进去。

## 添加远程库

配置公钥，让计算机和远程库能够联系。

首先，登陆 GitHub，然后，在右上角找到 “+” 按钮，点击 "New Repository"。

填写项目名称，其他的选项可默认。点击“创建”按钮。

目前，在 GitHub 上的这个 learngit 仓库还是空的，GitHub 告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到 GitHub 仓库。

现在，我们根据 GitHub 的提示，在本地的 learngit 仓库下运行命令:

```sh
git remote add origin git@github.com:Hope-Studio/learngit.git
```

添加后，远程库的名字就是 `origin`，这是 Git 默认的叫法，也可以改成别的。

下一步，就可以把本地库的所有内容推送到远程库上:

```
$ git push -u origin master
Counting objects: 20, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (15/15), done.
Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.
Total 20 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), done.
To github.com:Hope-Studio/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

把本地库的内容推送到远程，用 `git push` 命令，实际上是把当前分支 `master` 推送到远程。

由于远程库是空的，我们第一次推送 `master` 分支时，加上了 `-u` 参数，Git 不但会把本地的 `master` 分支内容推送的远程新的 `master` 分支，还会把本地的 `master` 分支和远程的 `master` 分支关联起来，在以后的推送或者拉取时就可以简化命令。

推送成功后，可以立刻在 GitHub 页面中看到远程库的内容已经和本地一模一样:

从现在起，只要本地作了提交，就可以通过命令:

```sh
git push origin master
```

把本地 `master` 分支的最新修改推送至 GitHub，现在，您就拥有了真正的分布式版本库!

## SHH警告

第一次连接时会有一个警告，之后就没有了。

### 关联小结

要关联一个远程库，使用命令 `git remote add origin git@<server-name>:<path>/<repo-name>.git`；

关联后，使用命令 `git push -u origin master` 第一次推送 master 分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令 `git push origin master` 推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而 SVN 在没有联网的时候是拒绝干活的! 当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了!

## 从远程库克隆

上次我们讲了先有本地库，后有远程库的时候，如何关联远程库。

现在，假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。

首先，登陆 GitHub，创建一个新的仓库，名字叫 `gitskills`:

我们勾选使用 “readme 初始化项目”，这样 GitHub 会自动为我们创建一个 README.md 文件。创建完毕后，可以看到 README.md 文件。

现在，远程库已经准备好了，下一步是用命令 `git clone` 克隆一个本地库:

```
$ git clone git@github.com:Hope-Studio/gitskills.git
Cloning into 'gitskills'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
Receiving objects: 100% (3/3), done.
```

注意把 Git 库的地址换成您自己的，然后进入 `gitskills` 目录看看，已经有 README.md 文件了:

```sh
$ cd gitskills
$ ls
README.md
```

如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

### 克隆小结

要克隆一个仓库，首先必须知道仓库的地址，然后使用 `git clone` 命令克隆。

Git 支持多种协议，包括 https，但通过 ssh 支持的原生 git 协议速度最快。

## 从远程仓库中抓取

从远程仓库中获得数据，可以执行:

```sh
git fetch <remote>
```

这个命令会访问远程仓库，从中拉取所有您还没有的数据。执行完成后，您将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。

`git fetch origin` 会抓取克隆(或上一次抓取)后新推送的所有工作。必须注意 `git fetch` 命令只会将数据下载到您的本地仓库——它并不会自动合并或修改您当前的工作。当准备好时您必须手动将其合并入您的工作。

## 推送到远程仓库

当您想分享您的项目时，必须将其推送到上游。这个命令很简单: `git push <remote> <branch>`。当您 想要将 master 分支推送到 origin 服务器时(再次说明，克隆时通常会自动帮您设置好那两个名字)， 那么 运行这个命令就可以将您所做的备份到服务器:

```sh
git push origin master
```

只有当您有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。当您和其他人在同一时间克隆，他们先推送到上游然后您再推送到上游，您的推送就会毫无疑问地被拒绝。您必须先抓取他们的工作并将其合并进您的工作后才能推送。

## 查看某个远程仓库

如果想要查看某一个远程仓库的更多信息，可以使用 `git remote show <remote>` 命令。如果想以一个特 定的缩写名运行这个命令，例如 origin，会得到像下面类似的信息:

```sh
$ git remote show origin
* remote origin
Fetch URL: https://github.com/schacon/ticgit
Push URL: https://github.com/schacon/ticgit
HEAD branch: master
Remote branches:
master tracked
dev-branch tracked
Local branch configured for 'git pull':
master merges with remote master
Local ref configured for 'git push':
master pushes to master (up to date)
```

它同样会列出远程仓库的 URL 与跟踪分支的信息。这些信息非常有用，它告诉您正处于 master 分支，并且如果运行 `git pull`，就会抓取所有的远程引用，然后将远程 master 分支合并到本地 master 分支。它也会列出拉取到的所有远程引用。

## 远程仓库的重命名与移除

您可以运行 `git remote rename` 来修改一个远程仓库的简写名。例如，想要将 pb 重命名为 paul，可以用 `git remote rename` 这样做:

```sh
$ git remote rename pb paul
$ git remote
origin
paul
```

值得注意的是这同样也会修改您所有远程跟踪的分支名字。那些过去引用 `pb/master` 的现在会引用 `paul/master`。

如果因为一些原因想要移除一个远程仓库——您已经从服务器上搬走了或不再想使用某一个特定的镜像了，又或者某一个贡献者不再贡献了——可以使用 `git remote remove` 或 `git remote rm`:

```sh
$ git remote remove paul
$ git remote
origin
```

一旦您使用这种方式删除了一个远程仓库，那么所有和这个远程仓库相关的远程跟踪分支以及配置信息也会一起被删除。

# 分支管理

## 概述

分支就是科幻电影里面的平行宇宙，当您正在电脑前努力学习 Git 的时候，另一个您正在另一个平行宇宙里努力学习 SVN。

如果两个平行宇宙互不干扰，那对现在的您也没啥影响。不过，在某个时间点，两个平行宇宙合并了，结果，您既学会了 Git 又学会了 SVN!

## 创建与合并分支

在版本回退里，您已经知道，每次提交，Git 都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在 Git 里，这个分支叫主分支，即 `master` 分支。`HEAD` 严格来说不是指向提交，而是指向 `master`，`master` 才是指向提交的，所以，`HEAD` 指向的就是当前分支。

一开始的时候，`master` 分支是一条线，Git 用 `master` 指向最新的提交，再用 `HEAD` 指向 `master`，就能确定当前分支，以及当前分支的提交点:

![branch](./assets/branch.png)

每次提交，`master` 分支都会向前移动一步，这样，随着您不断提交，`master` 分支的线也越来越长:

当我们创建新的分支，例如 dev 时，Git 新建了一个指针叫 `dev`，指向 `master` 相同的提交，再把 `HEAD` 指向 `dev`，就表示当前分支在 dev 上:

![dev](./assets/Dev.png)

您看，Git 创建一个分支很快，因为除了增加一个 `dev` 指针，改改 `HEAD` 的指向，工作区的文件都没有任何变化!

不过，从现在开始，对工作区的修改和提交就是针对 dev 分支了，比如新提交一次后，`dev` 指针往前移动一步，而 `master` 指针不变:

<img src="./assets/newDev.png" alt="newDev"  />

假如我们在 dev 上的工作完成了，就可以把 dev 合并到 master 上。Git 怎么合并呢? 最简单的方法，就是直接把 master 指向 dev 的当前提交，就完成了合并:

![1](./assets/1.png)

所以 Git 合并分支也很快! 就改改指针，工作区内容也不变!

合并完分支后，甚至可以删除 dev 分支。删除 dev 分支就是把 dev 指针给删掉，删掉后，我们就剩下了一条 master 分支:

<!-- ![2](D:\前端学习\2.png) -->

下面开始实战。首先，我们创建 dev 分支，然后切换到 dev 分支:

```
$ git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout` 命令加上 `-b` 参数表示创建并切换，相当于以下两条命令:

```
$ git branch dev
# nothing
$ git checkout dev
Switched to branch 'dev'
```

然后，用 `git branch` 命令查看当前分支:

```
$ git branch
* dev
  master
```

`git branch` 命令会列出所有分支，当前分支前面会标一个 `*` 号。

然后，我们就可以在 dev 分支上正常提交，比如对 `readme.txt` 做个修改，加上一行:

```
Creating a new branch is quick.
```

然后提交:

```
$ git add readme.txt
# nothing
$ git commit -m "branch test"
[dev b17d20e] branch test
 1 file changed, 1 insertion(+)
```

现在，dev 分支的工作完成，我们就可以切换回 master 分支:

```
$ git checkout master
Switched to branch 'master'
```

切换回 master 分支后，再查看一个 `readme.txt` 文件，刚才添加的内容不见了! 因为那个提交是在 dev 分支上，而 master 分支此刻的提交点并没有变:

<!-- ![3](D:\前端学习\3.png) -->

现在，我们把 dev 分支的工作成果合并到 master 分支上:

```
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

`git merge` 命令用于合并指定分支到当前分支。合并后，再查看 readme.txt 的内容，就可以看到，和 dev 分支的最新提交是完全一样的。

查看文件：

```
cat readme.txt
```

注意到上面的 `Fast-forward` 信息，Git 告诉我们，这次合并是 “快进模式”，也就是直接把 master 指向 dev 的当前提交，所以合并速度非常快。当然，也不是每次合并都能 `Fast-forward`。合并完成后，就可以放心地删除 dev 分支了: 

```
$ git branch -d dev
Deleted branch dev (was b17d20e).
```

删除后，查看 `branch`，就只剩下 `master` 分支了:

```
$ git branch
* master
```

因为创建、合并和删除分支非常快，所以 Git 鼓励您使用分支完成某个任务，合并后再删掉分支，这和直接在 `master` 分支上工作效果是一样的，但过程更安全。

### 分支小结

查看分支: `git branch`

创建分支: `git branch <name>`

切换分支: `git checkout <name>`

创建+切换分支: `git checkout -b <name>`

合并某分支到当前分支: `git merge <name>`

删除分支: `git branch -d <name>`

## 冲突

就是创建了一个分支feature1，并做出了修改，然后又在master上进行了修改，将feature1合并到master上时出现了冲突。修改冲突的内容后再提交：

```
$ git add readme.txt
# nothing
$ git commit -m "conflict fixed"
[master cf810e4] conflict fixed
```

现在，`master` 分支和 `feature1` 分支变成了下图所示:

<!-- ![4](D:\前端学习\4.png) -->

最后，删除 feature1 分支:

```
$ git branch -d feature1
Deleted branch feature1 (was 14096d0).
```

### **冲突小结**

1.当 Git 无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。解决冲突就是把 Git 合并失败的文件手动编辑为我们希望的内容，再提交。

2.用 `git log --graph` 命令可以看到分支合并图。

## 分支管理策略

通常，合并分支时，如果可能，Git 会用 `Fast forward` 模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用 `Fast forward` 模式，Git 就会在 merge 时生成一个新的 commit，这样，从分支历史上就可以看出分支信息。

实战一下 `--no-ff` 方式的 `git merge`。首先，仍然创建并切换 `dev` 分支:

```
$ git checkout -b dev
Switched to a new branch 'dev'
```

修改 readme.txt 文件，并提交一个新的 `commit`:

```
$ git add readme.txt
# nothing
$ git commit -m "add merge"
[dev f52c633] add merge
 1 file changed, 1 insertion(+)
```

现在，我们切换回 `master`:

```
$ git checkout master
Switched to branch 'master'
```

准备合并 `dev` 分支，请注意 `--no-ff` 参数，表示禁用 `Fast forward`:

```
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

因为本次合并要创建一个新的 `commit`，所以加上 `-m` 参数，把 `commit` 描述写进去。

合并后，我们用 `git log` 看看分支历史:

```
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\
| * f52c633 (dev) add merge
|/
*   cf810e4 conflict fixed
...
```

可以看到，不使用 `Fast forward` 模式，`merge` 后就像这样:

<!-- ![5](D:\前端学习\5.png) -->

### 分支管理惯例

在实际开发中，我们应该按照几个基本原则进行分支管理:

首先，`master` 分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢? 干活都在 `dev` 分支上，也就是说，`dev` 分支是不稳定的，到某个时候，比如 `1.0` 版本发布时，再把 `dev` 分支合并到 `master` 上，在 `master` 分支发布 `1.0` 版本；

您和您的小伙伴们每个人都在 `dev` 分支上干活，每个人都有自己的分支，时不时地往 `dev` 分支上合并就可以了。

所以，团队合作的分支看起来就像这样:

<!-- ![示例图](data:image/png;base64,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) -->

### 分支管理小结

Git 分支十分强大，在团队开发中应该充分应用。

合并分支时，加上 `--no-ff` 参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而 `fast forward` 合并就看不出来曾经做过合并。

## Bug分支

软件开发中，bug 就像家常便饭一样。有了 bug 就需要修复，在 Git 中，由于分支是如此的强大，所以，每个 bug 都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

Git 还提供了一个 `stash` 功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作:

```
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```

现在，用 `git status` 查看工作区，就是干净的(除非有没有被 Git 管理的文件)，因此可以放心地创建分支来修复 bug。

首先确定要在哪个分支上修复 bug，假定需要在 master 分支上修复，就从 master 创建临时分支:

```
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git checkout -b issue-101
Switched to a new branch 'issue-101'
```

现在修复 bug，需要把“Git is free software ...”改为“Git is a free software ...”，然后提交:

```
git add readme.txt
$ git commit -m "fix bug 101"
[issue-101 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

修复完成后，切换到 `master` 分支，并完成合并，最后删除 `issue-101` 分支:

```
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

太棒了，原计划两个小时的 bug 修复只花了 5 分钟! 现在，是时候接着回到 `dev` 分支干活了!

```
$ git checkout dev
Switched to branch 'dev'

$ git status
On branch dev
nothing to commit, working tree clean
```

工作区是干净的，刚才的工作现场存到哪去了? 用 `git stash list` 命令看看:

```
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

工作现场还在，Git 把 stash 内容存在某个地方了，但是需要恢复一下，有两个办法:

一是用 `git stash apply` 恢复，但是恢复后，`stash` 内容并不删除，您需要用 `git stash drop` 来删除；

另一种方式是用 `git stash pop`，恢复的同时把 `stash` 内容也删了:

```
$ git stash pop
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   readme.txt

Dropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```

再用 `git stash list` 查看，就看不到任何 stash 内容了。您可以多次 stash，恢复的时候，先用 `git stash list` 查看，然后恢复指定的 stash，用命令:

```
git stash apply stash@{0}
```

### 暂存小结

- 修复 bug 时，我们会通过创建新的 bug 分支进行修复，然后合并，最后删除；
- 当手头工作没有完成时，先把工作现场 `git stash` 一下，然后去修复 bug，修复后，再 `git stash pop`，回到工作现场。

## Feature分支

软件开发中，总有无穷无尽的新的功能要不断添加进来。

添加一个新功能时，您肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个 `feature` 分支，在上面开发，完成后，合并，最后，删除该 `feature` 分支。

现在，您终于接到了一个新任务: 开发代号为 `Vulcan` 的新功能，该功能计划用于下一代星际飞船。

于是准备开发:

```
$ git checkout -b feature-vulcan
Switched to a new branch 'feature-vulcan'
```

5 分钟后，开发完毕:

```
git add vulcan.py

$ git status
On branch feature-vulcan
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   vulcan.py

$ git commit -m "add feature vulcan"
[feature-vulcan 287773e] add feature vulcan
 1 file changed, 2 insertions(+)
 create mode 100644 vulcan.py
```

切回 `dev`，准备合并:

```
git checkout dev
```

一切顺利的话，feature 分支和 bug 分支是类似的，合并，然后删除。

但是!

就在此时，接到上级命令，因经费不足，新功能必须取消!

虽然白干了，但是这个包含机密资料的分支还是必须就地销毁:

```
$ git branch -d feature-vulcan
error: The branch 'feature-vulcan' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
```

销毁失败。Git 友情提醒，feature-vulcan 分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的 `-D` 参数。

现在我们强行删除:

```
$ git branch -D feature-vulcan
Deleted branch feature-vulcan (was 287773e).
```

终于删除成功!

### 删除小结

- 开发一个新 feature，最好新建一个分支；
- 如果要丢弃一个没有被合并过的分支，可以通过 `git branch -D <name>` 强行删除。

## 多人协作

当您从远程仓库克隆时，实际上 Git 自动把本地的 `master` 分支和远程的 `master` 分支对应起来了，并且，远程仓库的默认名称是 `origin`。

要查看远程库的信息，用 `git remote`:

```
$ git remote
origin
```

或者，用 `git remote -v` 显示更详细的信息:

```
$ git remote -v
origin  git@github.com:Hope-Studio/learngit.git (fetch)
origin  git@github.com:Hope-Studio/learngit.git (push)
```

上面显示了可以抓取和推送的 `origin` 的地址。如果没有推送权限，就看不到 push 的地址。

### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git 就会把该分支推送到远程库对应的远程分支上:

```
git push origin master
```

如果要推送其他分支，比如 `dev`，就改成:

```
git push origin dev
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢?

`master` 分支是主分支，因此要时刻与远程同步；

`dev` 分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

`bug` 分支只用于在本地修复 bug，就没必要推到远程了，除非老板要看看您每周到底修复了几个 bug；

`feature` 分支是否推到远程，取决于您是否和您的小伙伴合作在上面开发。

总之，就是在 Git 中，分支完全可以在本地自己藏着玩，是否推送，视您的心情而定!

### 抓取分支

多人协作时，大家都会往 `master` 和 `dev` 分支上推送各自的修改。

克隆：

```
$ git clone git@github.com:Hope-Studio/learngit.git
```

当您的小伙伴从远程库 clone 时，默认情况下，您的小伙伴只能看到本地的 `master` 分支。不信可以用 `git branch` 命令看看:

```
$ git branch
* master
```

现在，您的小伙伴要在 `dev` 分支上开发，就必须创建远程 `origin` 的 `dev` 分支到本地，于是他用这个命令创建本地 `dev` 分支:

```
git checkout -b dev origin/dev
```

现在，他就可以在 `dev` 上继续修改，然后，时不时地把 `dev` 分支 push 到远程:

```
git add env.txt

$ git commit -m "add env"
[dev 7a5e5dd] add env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

$ git push origin dev
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 308 bytes | 308.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:Hope-Studio/learngit.git
   f52c633..7a5e5dd  dev -> dev

```

您的小伙伴已经向 `origin/dev` 分支推送了他的提交，想要获取他的工作，您可以执行 `git fetch origin dev`。

如果这时碰巧您也对同样的文件作了修改，并试图推送:

```
$ cat env.txt
env

git add env.txt

$ git commit -m "add new env"
[dev 7bd91f1] add new env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

$ git push origin dev
To github.com:Hope-Studio/learngit.git
 ! [rejected]        dev -> dev (non-fast-forward)
error: failed to push some refs to 'git@github.com:Hope-Studio/learngit.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

推送失败，因为您的小伙伴的最新提交和您试图推送的提交有冲突，解决办法也很简单，Git 已经提示我们，先用 `git pull` 把最新的提交从 `origin/dev` 抓下来，然后，在本地合并，解决冲突，再推送: 

```
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```

`git pull` 也失败了，原因是没有指定本地 `dev` 分支与远程 `origin/dev` 分支的链接，根据提示，设置 `dev` 和 `origin/dev` 的链接:

```
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```

再 pull:

```
$ git pull
Auto-merging env.txt
CONFLICT (add/add): Merge conflict in env.txt
Automatic merge failed; fix conflicts and then commit the result.
```

这回 `git pull` 成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再 push:

```
$ git commit -m "fix env conflict"
[dev 57c53ab] fix env conflict

$ git push origin dev
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To github.com:Hope-Studio/learngit.git
   7a5e5dd..57c53ab  dev -> dev
```

因此，多人协作的工作模式通常是这样:

首先，可以试图用 `git push origin <branch-name>` 推送自己的修改；

如果推送失败，则因为远程分支比您的本地更新，需要先用 `git pull` 试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用 `git push origin <branch-name>` 推送就能成功!

如果 `git pull` 提示 `no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令 `git branch --set-upstream-to <branch-name> origin/<branch-name>`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

### 多人协作小结

- 查看远程库信息，使用 `git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用 `git push origin branch-name`，如果推送失败，先用 `git pull` 抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用 `git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用 `git branch --set-upstream branch-name origin/branch-name`；
- 从远程拉取分支，使用 `git fetch`。
- `git pull`，相当于 `git fetch` + `git merge`，如果您的修改并没有和远程的修改发生冲突，两者会自动合并到您的本地，您可以直接推送上去。如果有冲突，要先处理冲突。

## Rebase

在上一节我们看到了，多人在同一个分支上协作时，很容易出现冲突。即使没有冲突，后 `push` 的童鞋不得不先 `pull`，在本地合并，然后才能 `push` 成功。

Git 有一种称为 `rebase` 的操作，有人把它翻译成“变基”。

`rebase` 操作的特点: 把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。

### Rebase 小结

- `rebase` 操作可以把本地未 `push` 的分叉提交历史整理成直线；
- `rebase` 的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

# 标签管理

发布一个版本时，我们通常先在版本库中打一个标签 (`tag`)，这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git 的标签虽然是版本库的快照，但其实它就是指向某个 `commit` 的指针(跟分支很像对不对? 但是分支可以移动，标签不能移动)，所以，创建和删除标签都是瞬间完成的。

`tag` 就是一个让人容易记住的有意义的名字，它跟某个 `commit` 绑在一起。

提示：推荐的标签规范是以小写字母 `v` 开头，后接 `x.x` 或 `x.x.x` 等若干位版本号。

## 创建轻量标签

在 Git 中打标签非常简单，首先，切换到需要打标签的分支上:

```
$ git branch
* dev
  master
$ git checkout master
Switched to branch 'master'
```

然后，敲命令 `git tag <name>` 就可以打一个新标签:

```
$ git tag v1.0
-- no output --
```

可以用命令 `git tag` 查看所有标签:

```
$ git tag
v1.0
```

对某个特定的commit id打标签：

```
$ git tag v0.9 f52c633
--no output --
```

可以用 `git show <tagname>` 查看标签信息:

```
$ git show v0.9
commit f52c63349bc3c1593499807e5c8e972b82c8f286 (tag: v0.9)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:56:54 2018 +0800

    add merge

diff --git a/readme.txt b/readme.txt
...
```

## 附注标签

Git 还可以创建带有说明的标签，用 `-a` 指定标签名，`-m` 指定说明文字:

```
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
--no output --
```

### 添加小结

命令 `git tag <tagname>` 用于新建一个标签，默认为 `HEAD`，也可以指定一个 `commit id`；

命令 `git tag -a <tagname> -m "blablabla..."` 可以指定标签信息；

命令 `git tag` 可以查看所有标签。

## 操作标签

如果标签打错了，也可以删除:

```
$ git tag -d v0.1
Deleted tag 'v0.1' (was f15b0dd)\
```

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令 `git push origin <tagname>`:

```
$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0)
To github.com:Hope-Studio/learngit.git
 * [new tag]         v1.0 -> v1.0
```

或者，一次性推送全部尚未推送到远程的本地标签:

```
$ git push origin --tags
Total 0 (delta 0), reused 0 (delta 0)
To github.com:Hope-Studio/learngit.git
 * [new tag]         v0.9 -> v0.9
```

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除:

```
$ git tag -d v0.9
Deleted tag 'v0.9' (was f52c633)
```

然后，从远程删除。删除命令也是 push，但是格式如下:

```
$ git push origin :refs/tags/v0.9
To github.com:Hope-Studio/learngit.git
 - [deleted]         v0.9
```

要看看是否真的从远程库删除了标签，可以登陆 GitHub 查看。

### 管理标签小结

- 命令 `git push origin <tagname>` 可以推送一个本地标签；
- 命令 `git push origin --tags` 可以推送全部未推送过的本地标签；
- 命令 `git tag -d <tagname>` 可以删除一个本地标签；
- 命令 `git push origin :refs/tags/<tagname>` 可以删除一个远程标签。

# 忽略特殊文件

有些时候，您必须把某些文件放到 Git 工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件啦，等等，每次 `git status` 都会显示 `Untracked files` ...，有强迫症的童鞋心里肯定不爽。 

好在 Git 考虑到了大家的感受，这个问题解决起来也很简单，在 Git 工作区的根目录下创建一个特殊的 `.gitignore` 文件，然后把要忽略的文件名填进去，Git 就会自动忽略这些文件。

忽略文件的原则是:

- 忽略操作系统自动生成的文件，比如缩略图等；
- 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如 Java 编译产生的 `.class` 文件；
- 忽略您自己的带有敏感信息的配置文件，比如存放口令的配置文件。

最终得到一个完整的 `.gitignore` 文件，内容如下:

```
# Windows:

Thumbs.db
ehthumbs.db
Desktop.ini

# Python:

_.py[cod]
_.so
_.egg
_.egg-info
dist
build

# My configurations:

db.ini
deploy_key_rsa
```

最后一步就是把 `.gitignore` 也提交到 Git，就完成了! 当然检验 `.gitignore` 的标准是 `git status` 命令是不是说 `working directory clean`。

使用 Windows 的童鞋注意了，如果您在资源管理器里新建一个 `.gitignore` 文件，它会非常弱智地提示您必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为 `.gitignore` 了。

有些时候，您想添加一个文件到 Git，但发现添加不了，原因是这个文件被 `.gitignore` 忽略了:

```
$ git add App.class
The following paths are ignored by one of your .gitignore files:
App.class
Use -f if you really want to add them.
```

如果您确实想添加该文件，可以用 `-f` 强制添加到 Git:

```
git add -f App.class
```

或者您发现，可能是 `.gitignore` 写得有问题，需要找出来到底哪个规则写错了，可以用 `git check-ignore` 命令检查:

```
$ git check-ignore -v App.class
.gitignore:3:*.class    App.class
```

Git 会告诉我们，`.gitignore` 的第 3 行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。

## 格式规范

```
# 忽略所有的 .a 文件
*.a

# 但跟踪所有的 lib.a，即便您在前面忽略了 .a 文件
!lib.a

# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO

# 忽略任何目录下名为 build 的文件夹
build/

# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt

# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
```

## 小结

- 忽略某些文件时，需要编写 `.gitignore`；
- `.gitignore` 文件本身要放到版本库里，并且可以对 `.gitignore` 做版本管理!

# Git原理

## 直接记录快照，而非差异比较

## 近乎所有操作都是本地执行

## Git保证完整性

## Git 一般只添加数据

## 三种状态

Git 有三种状态，您的文件可能处于其中之一: 已提交(committed)、已修改(modified) 和 已暂存(staged)。

- 已修改表示修改了文件，但还没保存到数据库中。
- 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。
- 已提交表示数据已经安全地保存在本地数据库中。

这会让我们的 Git 项目拥有三个阶段: 工作区、暂存区以及 Git 目录。

基本的 Git 工作流程如下:

1. 在工作区中修改文件。
2. 将您想要下次提交的更改选择性地暂存，这样只会将更改的部分添加到暂存区。
3. 提交更新，找到暂存区的文件，将快照永久性存储到 Git 目录。

如果 Git 目录中保存着特定版本的文件，就属于 已提交 状态。如果文件已修改并放入暂存区，就属于 已暂存 状态。如果自上次检出后，作了修改但还没有放到暂存区域，就是 已修改 状态。

# 自定义Git

## 配置别名

## 配置文件

配置 Git 的时候，加上 `--global` 是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

配置别名也可以直接修改这个文件（用户主目录下的.gitconfig），如果改错了，可以删掉文件重新通过命令配置。

### 别名小结

- 给 Git 配置好别名，就可以输入命令时偷个懒。我们鼓励偷懒。




































































































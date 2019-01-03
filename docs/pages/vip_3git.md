# Git入门到高级教程

## 为什么要进行项目文件的版本管理

1. 代码备份和恢复
1. 团队开发和协作流程
1. 项目分支管理和备份

## git 是什么？

git是一个分布式的版本控制软件。版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。

最初由林納斯·托瓦茲創作，於2005年以GPL釋出。最初目的是為更好地管理Linux內核開發而設計。

> 2005年，安德鲁·垂鸠写了一个简单程序，可以连接BitKeeper的存储库，BitKeeper著作权拥有者拉里·麦沃伊认为安德鲁·垂鸠对BitKeeper内部使用的协议进行逆向工程，决定收回无偿使用BitKeeper的许可。Linux内核开发团队与BitMover公司进行磋商，但无法解决他们之间的歧见。林纳斯·托瓦兹决定自行开发版本控制系统替代BitKeeper，以十天的时间，编写出第一个git版本.

## 集中式和分布式版本控制软件

### 集中式版本控制软件

集中化的版本控制系统（`Centralized Version Control Systems`，简称 `CVCS`）应运而生。 这类系统，诸如 `CVS`、`Subversion` 以及 `Perforce` 等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。

集中式管理的缺点：

1. 中央服务器的单点故障，导致整个版本管理瘫痪
1. 中央服务器故障导致所有的版本丢失
1. 当客户端和服务器端无法进行网络连接的时，客户端无法进行提交代码和保存版本，也不能恢复某个版本

![集中式](../images/centralized.png)

### 分布式版本控制软件

分布式版本控制系统（`Distributed Version Control System`，简称 `DVCS`）.分布式就是每个人都有一份完整的仓库版本，提交和管理都是在本地进行，当然也可以进行远程的仓库之间进行合并提交等操作。

![分布式](../images/distributed.png)

## git 安装

### Linux 上安装

如果你想在 Linux 上用二进制安装程序来安装 Git，可以使用发行版包含的基础软件包管理工具来安装。 如果 以 Fedora 上为例，你可以使用 yum:

```sh
$ sudo yum install git
```

如果你在基于 Debian 的发行版上，请尝试用 apt-get: 

```sh
$ sudo apt-get install git
```
要了解更多选择，Git 官方网站上有在各种 Unix 风格的系统上安装步骤，网址为 [http://git-scm.com/download/linux](http://git-scm.com/download/linux)。

### 在 Mac 上安装

方法一：

最简单的方法是安装 `Xcode Command Line Tools`,然后在 Terminal 里尝试首次运行 git 命令即可。 如果没有安装过命令行开发者工具，将会提示 你安装。

方法二：（推荐）

使用 [homebrew](https://github.com/mxcl/homebrew)

```sh
$ brew install git
```

### windows上安装

直接下载安装包：[https://gitforwindows.org/](https://gitforwindows.org/)

### 检测是否安装成功

windows请从开始菜单程序中打开`gitbash`,其他系统请打开终端工具或者shell命令行工具。

```sh
$ git --version

# 输出，仅供参考
git version 2.18.0
```

## git 快速上手和配置

### 用户名和邮箱配置

当安装完 Git 应该做的第一件事就是设置你的用户名称与邮件地址。 这样做很重要，因为每一个 Git 的提交都会 使用这些信息，并且它会写入到你的每一次提交中。

windows请从开始菜单程序中打开`gitbash`,其他系统请打开终端工具或者shell命令行工具。

```sh
$ git config --global user.name "aicoder"
$ git config --global user.email laoma@aicoder.com
```

**请把用户名和邮箱换成你自己的**

> 如果使用了 --global 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事 情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运 行没有 --global 选项的命令来配置。

以上配置完成后，检测是否配置成功：

```sh
$ git config --list

# 输出以下内容，仅供参考
credential.helper=osxkeychain
user.name=aicoder
user.email=laoma@aicoder.com
color.status=auto
...
```

### 配置SSH

`Git` 服务器大部分都使用 `SSH` 公钥进行认证,其他认证方式都不方便，创建`SSH`公钥的方法。

**请打开用gitbash或者终端工具运行以下命令**

默认情况下，用户的 `SSH` 密钥存储在其 `~/.ssh` 目录下。 进入该目录并列出其中内容，你便可以快 速确认自己是否已拥有密钥:

```sh
$ cd ~/.ssh
$ ls
authorized_keys2  id_dsa       known_hosts
config            id_dsa.pub
```

> cd命令可以帮助我们进入系统的某个目录。 ls命令可以列出当前目录下面的所有文件和文件夹的信息。

如果已经存在`id_dsa`、`id_dsa.pub`，说明就已经生成果，后面的步骤可以省略。

> id_dsa 或 id_rsa 命名的文件，其中一个带有 .pub 扩展名。 .pub 文件是你的公钥，另 一个则是私钥。

```sh
  $ ssh-keygen

  # 一路回车回车即可，以下为输出内容，仅供参考
  Generating public/private rsa key pair.
  Enter file in which to save the key (/home/schacon/.ssh/id_rsa):
  Created directory '/home/schacon/.ssh'.
  Enter passphrase (empty for no passphrase):
  Enter same passphrase again:
  Your identification has been saved in /home/schacon/.ssh/id_rsa.
  Your public key has been saved in /home/schacon/.ssh/id_rsa.pub.
  The key fingerprint is:
  d0:82:24:8e:d7:f1:bb:9b:33:53:96:93:49:da:9b:e3 schacon@mylaptop.local
```

首先 ssh-keygen 会确认密钥的存储位置(默认是 .ssh/id_rsa)，然后它会要求你输入两次密钥口令。如 果你不想在使用密钥时输入口令，将其留空即可。

## git基本操作

### 在现有目录中初始化仓库

```sh
$ cd /path/to/init
$ git init
```

此时在目录中将创建一个名为 `.git` 的子目录,这里面存放当前仓库的所有的跟踪的信息。

此时当前文件夹下面的所有的文件都没有被跟踪，如果需要跟踪变化，必须添加到跟踪的参考中。

### 添加文件和提交信息

`git add 文件` 命令可以帮祝我们让git帮忙跟踪具体的文件。然后执行`git commit`提交信息，相当于确认跟踪。

```sh
$ git add ./*.js
$ git add a.txt
$ git commit -m 'first commit'
```

### 工作区和暂存区

- 工作区（Working Directory）

工作区，也称为工作目录，也就是要进行版本管理的文件夹。

- 暂存区（Stage或者Index）

暂时存放将要记录修改版本的文件的区域。

![工作目录和暂存区](../images/0.jpeg)

工作目录下的每一个文件都不外乎这两种状态:已跟踪或未跟踪。

`git add`可以把文件加入暂存区。
`git commit`命令可以把暂存区的文件更新变化记录到版本库中永久保存。

不在暂存区的文件，不会被追踪。

![文件修改的流程](../images/zc.png)

> 暂存区和版本库存放在 .git目录中。

### 检查当前文件状态

要查看哪些文件处于什么状态，可以用 git status 命令。 如果在克隆仓库后立即使用此命令，会看到类似这 样的输出:

```sh
$ git status
```

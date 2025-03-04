# Git是什么

# Git 设计剖析

# Git 的命令分区

# Git 生命周期

状态模型

![](https://pic1.zhimg.com/v2-e9edaef2103785c164876ff8b19b45ac_1440w.png)

上图描述了 git 对象的在不同的生命周期中不同的存储位置，通过不同的 git 命令改变 git 对象的存储生命周期。


## 工作区 (workspace)

就是我们当前工作空间，也就是我们当前能在本地文件夹下面看到的文件结构。初始化工作空间或者工作空间 clean 的时候，文件内容和 index 暂存区是一致的，随着修改，工作区文件在没有 add 到暂存区时候，工作区将和暂存区是不一致的。

## 暂存区 (index)

老版本概念也叫 Cache 区，就是文件暂时存放的地方，所有暂时存放在暂存区中的文件将随着一个 commit 一起提交到 local repository 此时 local repository 里面文件将完全被暂存区所取代。暂存区是 git 架构设计中非常重要和难理解的一部分。


## 本地仓库 (local repository)

git 是分布式版本控制系统，和其他版本控制系统不同的是他可以完全去中心化工作，你可以不用和中央服务器 (remote server) 进行通信，在本地即可进行全部离线操作，包括 log，history，commit，diff 等等。完成离线操作最核心是因为 git 有一个几乎和远程一样的本地仓库，所有本地离线操作都可以在本地完成，等需要的时候再和远程服务进行交互。


## 远程仓库 (remote repository)

中心化仓库，所有人共享，本地仓库会需要和远程仓库进行交互，也就能将其他所有人内容更新到本地仓库把自己内容上传分享给其他人。结构大体和本地仓库一样。

# Git 的 对象结构

git 主要有四个对象，分别是 Blob，Tree， Commit， Tag 他们都用 SHA-1 进行命名。

你可以用 git cat-file -t 查看每个 SHA-1 的类型，用 git cat-file -p 查看每个对象的内容和简单的数据结构。git cat-file 是 git 的瑞士军刀，是底层核心命令。

## Blob 对象

只用于存储单个文件内容，一般都是二进制的数据文件，不包含任何其他文件信息，比如不包含文件名和其他元数据。

## Tree 对象

对应文件系统的目录结构，里面主要有：子目录 (tree)，文件列表 (blob)，文件类型以及一些数据文件权限模型等。

## Commit 对象

是一次修改的集合，当前所有修改的文件的一个集合，可以类比一批操作的“事务”。是修改过的文件集的一个快照，随着一次 commit 操作，修改过的文件将会被提交到 local repository 中。通过 commit 对象，在版本化中可以检索出每次修改内容，是版本化的基石。

## Tag 对象

Tag 对象

tag 是一个"固化的分支"，一旦打上 tag 之后，这个 tag 代表的内容将永远不可变，因为 tag 只会关联当时版本库中最后一个 commit 对象。

分支的话，随着不断的提交，内容会不断的改变，因为分支指向的最后一个 commit 不断改变。所以一般应用或者软件版本的发布一般用 tag。

git 的 Tag 类型有两种：

1 lightweight (轻量级)

创建方式：

git tag tagName

这种方式创建的 Tag，git 底层不会创建一个真正意义上的 tag 对象，而是直接指向一个 commit 对象，此时如果使用 git cat-file -t tagName 会返回一个 commit。
```go
→ git cat-file -t v4
commit
→ git cat-file -p v4
tree ceab4f96440655b0ff1a783316c95450fa1fb436
parent 7f23c9ca70ce64fc58e8c7507c990c6c6a201d3d
author 与水  1506224164 +0800
committer 与水  1506224164 +0800
```
rawtest2

2 annotated (含附注)

创建方式：

git tag -a tagName -m''

这种方式创建的标签，git 底层会创建一个 tag 对象，tag 对象会包含相关的 commit 信息和 tagger 等额外信息，此时如果使用 git cat-file -t tagname 会返回一个 tag。

总结：所有对象模型之间的关系大致如下：
![](https://pic1.zhimg.com/v2-d7113216a59f7fe01179b11d20e02814_1440w.png)

title: GIT分支模型的策略与发布
categories: []
tags:
  - GIT
date: 2020-01-13 13:50:00
---
本文用于介绍 GIT分支模型的策略与发布。

![img](https://nvie.com/img/git-model@2x.png)

在git分支模型中我们一般会用到如下四种分支:

- **主分支**
- **特性分支**
- **release分支**
- **hotFix分支**

分别使用4个种类的分支来进行开发的。

## 主分支

![img](https://nvie.com/img/main-branches@2x.png)

主分支有两种：master分支和develop分支

- **master**
  master分支只负责管理发布的状态。在提交时使用标签记录发布版本号。
- **develop**
  develop分支是针对发布的日常开发分支。刚才我们已经讲解过有合并分支的功用。

## 特性分支

特性分支就是我们在前面讲解过的topic分支的功用。

这个分支是针对新功能的开发，在bug修正的时候从develop分支分叉出来的。基本上不需要共享特性分支的操作，所以不需要远端控制。完成开发后，把分支合并回develop分支后发布。

![img](https://nvie.com/img/fb@2x.png)

## release分支

release分支是为release做准备的。通常会在分支名称的最前面加上release-。release前需要在这个分支进行最后的调整，而且为了下一版release开发用develop分支的上游分支。

一般的开发是在develop分支上进行的，到了可以发布的状态时再创建release分支，为release做最后的bug修正。

到了可以release的状态时，把release分支合并到master分支，并且在合并提交里添加release版本号的标签。

要导入在release分支所作的修改，也要合并回develop分支。

## hotFix分支

hotFix分支是在发布的产品需要紧急修正时，从master分支创建的分支。通常会在分支名称的最前面加上 `hotfix-`。

例如，在develop分支上的开发还不完整时，需要紧急修改。这个时候在develop分支创建可以发布的版本要花许多的时间，所以最好选择从master分支直接创建分支进行修改，然后合并分支。

修改时创建的hotFix分支要合并回develop分支。

![img](https://nvie.com/img/hotfix-branches@2x.png)

## 参考

- [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)
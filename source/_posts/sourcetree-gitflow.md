---
title: SourceTree 实现 git flow 流程
date: 2018-11-19 14:51:16
tags: [sourcetree,gitflow,git]
categories: [Laravel 教程]
---

为什么使用 git 和 git flow，这篇文章 [深入理解学习Git工作流][1] 的内容相信能够给你一个完整的答案。

- 我们以使用SVN的工作流来使用git有什么不妥？
- git 方便的branch在哪里，团队多人如何协作？冲突了怎么办？如何进行发布控制？
- 经典的master-发布、develop-主开发、hotfix-不过修复如何避免代码不经过验证上线？
- 如何在github上面与他人一起协作，star-fork-pull request是怎样的流程？

这篇文章分为两部分，一部分引用 [深入理解学习Git工作流][2] 文章中的 git flow 工作流内容，一部分是使用 SourceTree 中的 git flow 工具来实现可视化的 git flow 流程管理，避免我们在使用 git 命令时遇到的坑。而且 SourceTree 的所有可视化操作都会展示相应的执行命令。

> [SourceTree Win10 安装过程及配置][3]


## `Gitflow` 工作流

`Gitflow`工作流通过为功能开发、发布准备和维护分配独立的分支，让发布迭代过程更流畅。严格的分支模型也为大型项目提供了一些非常必要的结构。

![Git Workflows: Gitflow Cycle](https://github.com/xirong/my-git/raw/master/images/git-workflows-gitflow.png)

这节介绍的[`Gitflow`工作流](http://nvie.com/posts/a-successful-git-branching-model/)借鉴自在[nvie](http://nvie.com/)的*Vincent Driessen*。

`Gitflow`工作流定义了一个围绕项目发布的严格分支模型。虽然比[功能分支工作流](workflow-feature-branch.md)复杂几分，但提供了用于一个健壮的用于管理大型项目的框架。

`Gitflow`工作流没有用超出功能分支工作流的概念和命令，而是为不同的分支分配一个明确的角色，并定义分支之间如何和什么时候进行交互。
除了使用功能分支，在做准备、维护和记录发布时，也定义了各自的分支。
当然你可以用上功能分支工作流所有的好处：`Pull Requests`、隔离实验性开发和更高效的协作。

### 2.3.1 工作方式
`Gitflow`工作流仍然用中央仓库作为所有开发者的交互中心。和其它的工作流一样，开发者在本地工作并`push`分支到要中央仓库中。

### 2.3.2 历史分支

相对于使用仅有的一个`master`分支，`Gitflow`工作流使用两个分支来记录项目的历史。`master`分支存储了正式发布的历史，而`develop`分支作为功能的集成分支。
这样也方便`master`分支上的所有提交分配一个版本号。

![](https://github.com/xirong/my-git/raw/master/images/git-workflow-release-cycle-1historical.png)

剩下要说明的问题围绕着这2个分支的区别展开。

### 2.3.3 功能分支

每个新功能位于一个自己的分支，这样可以[`push`到中央仓库以备份和协作](https://www.atlassian.com/git/tutorial/remote-repositories#!push)。
但功能分支不是从`master`分支上拉出新分支，而是使用`develop`分支作为父分支。当新功能完成时，[合并回`develop`分支](https://www.atlassian.com/git/tutorial/git-branches#!merge)。
新功能提交应该从不直接与`master`分支交互。

![](https://github.com/xirong/my-git/raw/master/images/git-workflow-release-cycle-2feature.png)

注意，从各种含义和目的上来看，功能分支加上`develop`分支就是功能分支工作流的用法。但`Gitflow`工作流没有在这里止步。

### 2.3.4 发布分支

![](https://github.com/xirong/my-git/raw/master/images/git-workflow-release-cycle-3release.png)

一旦`develop`分支上有了做一次发布（或者说快到了既定的发布日）的足够功能，就从`develop`分支上`checkout`一个发布分支。
新建的分支用于开始发布循环，所以从这个时间点开始之后新的功能不能再加到这个分支上——
这个分支只应该做`Bug`修复、文档生成和其它面向发布任务。
一旦对外发布的工作都完成了，发布分支合并到`master`分支并分配一个版本号打好`Tag`。
另外，这些从新建发布分支以来的做的修改要合并回`develop`分支。

使用一个用于发布准备的专门分支，使得一个团队可以在完善当前的发布版本的同时，另一个团队可以继续开发下个版本的功能。
这也打造定义良好的开发阶段（比如，可以很轻松地说，『这周我们要做准备发布版本4.0』，并且在仓库的目录结构中可以实际看到）。

常用的分支约定：

```
用于新建发布分支的分支: develop
用于合并的分支: master
分支命名: release-* 或 release/*
```

### 2.3.5 维护分支

![](https://github.com/xirong/my-git/raw/master/images/git-workflow-release-cycle-4maintenance.png)

维护分支或说是热修复（`hotfix`）分支用于给产品发布版本（`production releases`）快速生成补丁，这是唯一可以直接从`master`分支`fork`出来的分支。
修复完成，修改应该马上合并回`master`分支和`develop`分支（当前的发布分支），`master`分支应该用新的版本号打好`Tag`。

为`Bug`修复使用专门分支，让团队可以处理掉问题而不用打断其它工作或是等待下一个发布循环。
你可以把维护分支想成是一个直接在`master`分支上处理的临时发布。

### 2.3.6 示例
下面的示例演示本工作流如何用于管理单个发布循环。假设你已经创建了一个中央仓库。

#### 创建开发分支

![](https://github.com/xirong/my-git/raw/master/images/git-workflow-release-cycle-5createdev.png)

第一步为`master`分支配套一个`develop`分支。简单来做可以[本地创建一个空的`develop`分支](https://www.atlassian.com/git/tutorial/git-branches#!branch)，`push`到服务器上：

```bash
git branch develop
git push -u origin develop
```

以后这个分支将会包含了项目的全部历史，而`master`分支将只包含了部分历史。其它开发者这时应该[克隆中央仓库](https://www.atlassian.com/git/tutorial/git-basics#!clone)，建好`develop`分支的跟踪分支：

```bash
git clone ssh://user@host/path/to/repo.git
git checkout -b develop origin/develop
```

现在每个开发都有了这些历史分支的本地拷贝。

#### 小红和小明开始开发新功能

![](https://github.com/xirong/my-git/raw/master/images/git-workflow-release-cycle-6maryjohnbeginnew.png)

这个示例中，小红和小明开始各自的功能开发。他们需要为各自的功能创建相应的分支。新分支不是基于`master`分支，而是应该[基于`develop`分支](https://www.atlassian.com/git/tutorial/git-branches#!checkout)：

```bash
git checkout -b some-feature develop
```

他们用老套路添加提交到各自功能分支上：编辑、暂存、提交：
```bash
git status
git add <some-file>
git commit
```

#### 小红完成功能开发

![](https://github.com/xirong/my-git/raw/master/images/git-workflow-release-cycle-7maryfinishes.png)

添加了提交后，小红觉得她的功能OK了。如果团队使用`Pull Requests`，这时候可以发起一个用于合并到`develop`分支。
否则她可以直接合并到她本地的`develop`分支后`push`到中央仓库：

```bash
git pull origin develop
git checkout develop
git merge some-feature
git push
git branch -d some-feature
```

第一条命令在合并功能前确保`develop`分支是最新的。注意，功能决不应该直接合并到`master`分支。
冲突解决方法和[集中式工作流](workflow-centralized.md)一样。

#### 小红开始准备发布

![](https://github.com/xirong/my-git/raw/master/images/git-workflow-release-cycle-8maryprepsrelease.png)

这个时候小明正在实现他的功能，小红开始准备她的第一个项目正式发布。
像功能开发一样，她用一个新的分支来做发布准备。这一步也确定了发布的版本号：

```bash
git checkout -b release-0.1 develop
```

这个分支是清理发布、执行所有测试、更新文档和其它为下个发布做准备操作的地方，像是一个专门用于改善发布的功能分支。

只要小红创建这个分支并`push`到中央仓库，这个发布就是功能冻结的。任何不在`develop`分支中的新功能都推到下个发布循环中。

#### 小红完成发布

![](https://github.com/xirong/my-git/raw/master/images/git-workflow-release-cycle-9maryfinishes.png)

一旦准备好了对外发布，小红合并修改到`master`分支和`develop`分支上，删除发布分支。合并回`develop`分支很重要，因为在发布分支中已经提交的更新需要在后面的新功能中也要是可用的。
另外，如果小红的团队要求`Code Review`，这是一个发起`Pull Request`的理想时机。

```bash
git checkout master
git merge release-0.1
git push
git checkout develop
git merge release-0.1
git push
git branch -d release-0.1
```

发布分支是作为功能开发（`develop`分支）和对外发布（`master`分支）间的缓冲。只要有合并到`master`分支，就应该打好`Tag`以方便跟踪。

```bash
git tag -a 0.1 -m "Initial public release" master
git push --tags
```

`Git`有提供各种勾子（`hook`），即仓库有事件发生时触发执行的脚本。
可以配置一个勾子，在你`push`中央仓库的`master`分支时，自动构建好版本，并对外发布。

#### 最终用户发现`Bug`

![](https://github.com/xirong/my-git/raw/master/images/git-workflow-gitflow-enduserbug.png)

对外版本发布后，小红小明一起开发下一版本的新功能，直到有最终用户开了一个`Ticket`抱怨当前版本的一个`Bug`。
为了处理`Bug`，小红（或小明）从`master`分支上拉出了一个维护分支，提交修改以解决问题，然后直接合并回`master`分支：

```bash
git checkout -b issue-#001 master
# Fix the bug
git checkout master
git merge issue-#001
git push
```

就像发布分支，维护分支中新加这些重要修改需要包含到`develop`分支中，所以小红要执行一个合并操作。然后就可以安全地[删除这个分支](https://www.atlassian.com/git/tutorial/git-branches#!branch)了：

```bash
git checkout develop
git merge issue-#001
git push
git branch -d issue-#001
```

到了这里，但愿你对[集中式工作流](workflow-centralized.md)、[功能分支工作流](workflow-feature-branch.md)和`Gitflow`工作流已经感觉很舒适了。
你应该也牢固的掌握了本地仓库的潜能，`push`/`pull`模式和`Git`健壮的分支和合并模型。

记住，这里演示的工作流只是可能用法的例子，而不是在实际工作中使用`Git`不可违逆的条例。
所以不要畏惧按自己需要对工作流的用法做取舍。不变的目标就是让`Git`为你所用。

-----------------

## SourceTree 实现 git flow

通过文章《[SourceTree Win10 安装过程及配置][4]》 正确安装 SourceTree 以及管理示例源码，界面如下。

![图片描述][5]

### 初始化 Git flow

点击右上角的 “Git 工作流” ，初次会提示我们 “使用 Git Flow 来初始化此仓库”，已经帮助我们预定义好了一些配置，我们只需要点击 “确定” 按钮即可。

![图片描述][6]

点击“确定”按钮后，我们会发现 SourceTree 为我们自动创建了 `develop` 分支，并且切换到了 `develop` 分支。

![图片描述][7]

### Git flow: 建立新功能

继续点击右上角的 “Git 工作流” ，这次会提示我们选择具体的下一个流程动作，这里我们演示一个 “建立新的功能” 流程。

![图片描述][8]

点击 “建议新的功能” ，会让我们对即将要开发的功能进行命名（名称请使用英文），这里我们输入 `simple-git-flow`，点击确定按钮，会自动帮助我们创建 `feature/simple-git-flow` 分支，并切换到该分支上。

![图片描述][9]

同时我们也能看到 SourceTree 帮助我们执行了什么命令来达到这样的效果。

![图片描述][10]

已经自动切换到 `feature/simple-git-flow` 分支。

![图片描述][11]

### 提交代码

此时我们可以在分支上开发我们的新功能，可以在分支上管理代码，而不影响到其他同事的开发工作。

为了简单演示，我们修改下 `readme.md` 的内容如下：

![图片描述][12]

在 SourceTree 界面，我们需要在 `未暂存文件` 区域选中 `readme.md` 并点击 `暂存所选` 按钮，此时 `readme.md` 文件会进入到 `已暂存文件` 区域。只有 `已暂存文件` 的文件会进行 `提交操作` 。

在下方的空白区域输入本次提交的说明：`a simple git flow` 后点击提交按钮，就会提交源码。

![图片描述][13]

如下图所示：可以看到刚刚提交的代码记录

![图片描述][14]

### 完成新功能开发

经过不断的代码完善，并且经过单元测试后，代码已经完成后，此时就可以完成 `新功能的开发` ，继续点击 `Git 工作流`

![图片描述][15]

点击 “完成功能”，默认会如下图所示，在正常的开发流程下，我们不需要更改任何设置，直接点击确定即可。

![图片描述][16]

SourceTree 展示所执行的命令及结果。

![图片描述][17]

完成后，我们会发现 `feature/simple-git-flow` 分支已经不见了，同时在 `develop` 分支上多了一个  `a simple git flow` 的提交信息。

![图片描述][18]

至此整个 “建议新功能” 的 git flow 流程就完毕了。

## 总结

实际开发过程中，会遇到各种情况，无法通过简短的文章来说明所有的情况，具体需要在实践过程中不断学习。

再次总结下为什么要使用 SourceTree

1. 降低入门门槛。很多应届生并不知道版本管理工具和 git，通过可视化工具，可以避免初学者在使用过程中因为不熟悉命令而产生的问题。
2. 使用 SourceTree 的所有操作，都会展示响应的执行命令，也能让初学者了解到具体的操作是通过什么命令来实现的。
3. 在完全不熟悉命令的情况下，也可以通过 SourceTree 来参与团队协作开发。

并且在我们的 Centos 服务器部署中，我们使用 docker 来管理版本和发布，也基本用不到复杂的 git 命令，对于初学者的快速入门 SourceTree 足够了。


[1]: https://github.com/xirong/my-git/blob/master/git-workflow-tutorial.md#23-gitflow%E5%B7%A5%E4%BD%9C%E6%B5%81
[2]: https://github.com/xirong/my-git/blob/master/git-workflow-tutorial.md#23-gitflow%E5%B7%A5%E4%BD%9C%E6%B5%81
[3]: https://segmentfault.com/a/1190000013810310
[4]: https://segmentfault.com/a/1190000013810310#articleHeader2
[5]: https://cdn.chenhow.com/sourcetree-gitflow/bV56QN.jpg
[6]: https://cdn.chenhow.com/sourcetree-gitflow/bV56Ut.jpg
[7]: https://cdn.chenhow.com/sourcetree-gitflow/bV56Uz.jpg
[8]: https://cdn.chenhow.com/sourcetree-gitflow/bV56UL.jpg
[9]: https://cdn.chenhow.com/sourcetree-gitflow/bV56U1.jpg
[10]: https://cdn.chenhow.com/sourcetree-gitflow/bV56U9.jpg
[11]: https://cdn.chenhow.com/sourcetree-gitflow/bV56Vj.jpg
[12]: https://cdn.chenhow.com/sourcetree-gitflow/bV56VS.jpg
[13]: https://cdn.chenhow.com/sourcetree-gitflow/bV56V2.jpg
[14]: https://cdn.chenhow.com/sourcetree-gitflow/bV56WO.jpg
[15]: https://cdn.chenhow.com/sourcetree-gitflow/bV56Xv.jpg
[16]: https://cdn.chenhow.com/sourcetree-gitflow/bV56Xx.jpg
[17]: https://cdn.chenhow.com/sourcetree-gitflow/bV56XA.jpg
[18]: https://cdn.chenhow.com/sourcetree-gitflow/bV56XI.jpg
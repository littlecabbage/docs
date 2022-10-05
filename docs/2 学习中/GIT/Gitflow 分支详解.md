> created: 2022-10-05T12:18:00
> tags: [gitflow]
> source: [原文地址](https://blog.csdn.net/fd2025/article/details/124336480)
> author:

---

# Gitflow 分支详解

## 1 gitflow 介绍

![在这里插入图片描述|400](https://img-blog.csdnimg.cn/144bafc5941e4cf1bdb67b9d19e96f51.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

比如我很喜欢的 `git stash` 命令，可以把当前没有完成的事先暂存一下，然后去忙别的事。`git cherry-pick` 命令可以让你有选择地合并提交。`git add -p` 可以让你挑选改动提交，`git grep $regexp $(git rev-list --all)` 可以用来在所有的提交中找代码。因为都是本地操作，所以你会觉得速度飞快。

除此之外，由 Git 衍生出来的 GitHub/GitLab 可以帮你很好地管理编程工作，比如 wiki、fork、pull、request、issue……集成了与编程相关的工作，让人觉得这不是一个冷冰冰的工具，而真正和我们的日常工作发生了很好的交互。

GitHub/GitLab 这样工具的出现，让我们的工作可以呈现在一个工作平台上，并以此来规范整个团队的工作，这才正是 Git 这个版本管理工具成功的原因。

### 1.1 中心式协同工作流

首先，我们先说明一下，Git 是可以像 SVN 这样的中心工作流一样工作的。我相信很多程序员都是在采用这样的工作方式。

这个过程一般是下面这个样子的。
1. 从服务器上做 `git pull origin master` 把代码同步下来。
2. 改完后，`git commit` 到本地仓库中。
3. 然后 `git push origin master` 到远程仓库中，这样其他同学就可以得到你的代码了。

如果在第 3 步发现 push 失败，因为别人已经提交了，那么你需要先把服务器上的代码给 pull 下来，为了避免有 merge 动作，你可以使用 `git pull --rebase`。<mark style="background: #ABF7F7A6;">这样就可以把服务器上的提交直接合并到你的代码中</mark>，对此，Git 的操作是这样的。
1. 先把你本地提交的代码放到一边。
2. 然后把服务器上的改动下载下来。
3. 然后在本地把你之前的改动再重新一个一个地做 commit，直到全部成功。

如下图所示。Git 会把 Origin/Master 的远程分支下载下来（紫色的），然后把本地的 Master 分支上的改动一个一个地提交上去（蓝色的）。
![在这里插入图片描述](https://img-blog.csdnimg.cn/28847496cb6d4c76a127ec18ec0aa414.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

如果有冲突，那么你要先解决冲突，然后做 `git rebase --continue` 。如下图所示，git 在做 pull --rebase 时，会一个一个地应用（apply）本地提交的代码，如果有冲突就会停下来，等你解决冲突。
![在这里插入图片描述](https://img-blog.csdnimg.cn/037c6326070c45fab72a460c317529e8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

### 1.2 功能分支协同工作流

上面的那种方式有一个问题，就是大家都在一个主干上开发程序，对于小团队或是小项目你可以这么干，但是对比较大的项目或是人比较多的团队，这么干就会有很多问题。

**最大的问题就是代码可能干扰太严重。** 尤其是，我们想安安静静地开发一个功能时，我们想把各个功能的代码变动隔离开来，同时各个功能又会有多个开发人员在开发。

这时，我们不想让各个功能的开发人员都在 Master 分支上共享他们的代码。我们想要的协同方式是这样的：<mark style="background: #ABF7F7A6;">同时开发一个功能的开发人员可以分享各自的代码，但是不会把代码分享给开发其他功能的开发人员，直到整个功能开发完毕后，才会分享给其他的开发人员（也就是进入主干分支）。</mark>

因此，我们引入“功能分支”。这个协同工作流的开发过程如下。
1. 首先使用 `git checkout -b new-feature` 创建 “new-feature”分支。
2. 然后共同开发这个功能的程序员就在这个分支上工作，进行 add、commit 等操作。
3. 然后通过 `git push -u origin new-feature` 把分支代码 push 到服务器上。
4. 其他程序员可以通过 `git pull --rebase` 来拿到最新的这个分支的代码。
5. 最后通过 Pull Request 的方式做完 Code Review 后合并到 Master 分支上。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2c2d709240d0424293eb01efa5ab63d6.png)

就像上面这个图显示的一样，紫色的分支就是功能分支，合并后就会像上面这个样子。

### 1.3 GitFlow 协同工作流

在真实的生产过程中，前面的协同工作流还是不能满足工作的要求。这主要因为我们的生产过程是比较复杂的，软件生产中会有各式各样的问题，并要面对不同的环境。我们要在不停地开发新代码的同时，维护线上的代码，于是，就有了下面这些需求。
1. **希望有一个分支是非常干净的，上面是可以发布的代码，上面的改动永远都是可以发布到生产环境中的。** 这个分支上不能有中间开发过程中不可以上生产线的代码提交。
2. 希望当代码达到可以上线的状态时，也就是在 alpha/beta release 时，**在测试和交付的过程中，依然可以开发下一个版本的代码。**
3. 最后，对于已经发布的代码，也会有一些 **Bug-fix** 的改动，不会将正在开发的代码提交到生产线上去。

为了解决这些问题，GitFlow 协同工作流就出来了。
这个协同工作流的核心思想如下图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/a836de2bf65841b78fbfbbd755b0064a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

整个代码库中一共有**五种分支** 。
- <mark style="background: #ABF7F7A6;">Master 分支</mark>。也就是主干分支，用作发布环境，上面的每一次提交都是可以发布的。
- <mark style="background: #ABF7F7A6;">Feature 分支</mark>。也就是功能分支，用于开发功能，其对应的是开发环境。
- <mark style="background: #ABF7F7A6;">Developer 分支</mark>。是开发分支，一旦功能开发完成，就向 Developer 分支合并，合并完成后，删除功能分支。这个分支对应的是集成测试环境。
- Release 分支。当 Developer 分支测试达到可以发布状态时，开出一个 Release 分支来，然后做发布前的准备工作。这个分支对应的是预发环境。
> 之所以需要这个 Release 分支，是我们的开发可以继续向前，不会因为要发布而被 block 住而不能提交。一旦 Release 分支上的代码达到可以上线的状态，那么需要把 Release 分支向 Master 分支和 Developer 分支同时合并，以保证代码的一致性。然后再把 Release 分支删除掉。

- <mark style="background: #ABF7F7A6;">Hotfix 分支。</mark>是用于处理生产线上代码的 Bug-fix，每个线上代码的 Bug-fix 都需要开一个 Hotfix 分支，完成后，向 Developer 分支和 Master 分支上合并。
>合并完成后，删除 Hotfix 分支。

这就是整个 GitFlow 协同工作流的工作过程。我们可以看到：
1. **我们需要长期维护 Master 和 Developer 两个分支。**
2. 这其中的方式还是有一定复杂度的，尤其是 Release 和 Hotfix 分支需要同时向两个分支作合并。所以，如果没有一个好的工具来支撑的话，这会因为我们可能会忘了做一些操作而导致代码不一致。
3. GitFlow 协同虽然工作流比较重。但是它几乎可以应对所有公司的各种开发流程，包括瀑布模型，或是快速迭代模型。

### 1.4 Gitflow 协同工作流的缺点

其中有个问题就是因为分支太多，所以会出现 git log 混乱的局面。具体来说，主要是 `git- flow` 使用 `gitmerge --no-ff` 来合并分支，在 git-flow 这样多个分支的环境下会让你的分支管理的 log 变得很难看。如下所示，左边是使用–no-ff 参数在多个分支下的问题。

![在这里插入图片描述](https://img-blog.csdnimg.cn/19ad31f7ba794d27a93e0cf13be78d58.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

所谓 `--no-ff` 参数的意思是 `——no fast forward` 的意思。也就是说，合并的方法不要把这个分支以 **前置合并** 的方式提交，而是留下一个 merge 的提交。这是把双刃剑，我们希望我们的 `--no-ff` 能像右边那样，而不是像左边那样。

对此的建议是：只有 feature 合并到 developer 分支时，使用–no-ff 参数，其他的合并都不使用 `--no-ff` 参数来做合并。

```text
    --no-ff指的是强行关闭fast-forward方式。
    fast-forward方式就是当条件允许的时候，git直接把HEAD指针指向合并分支的头，完成合并。属于“快进方式”，
    不这种情况如果删除分支，则会丢失分支信息。因为在这个过程中没有创建commit
      
    git merge --squash 是用来把一些不必要commit进行压缩，比如说，你的feature在开发的时候写的commit很乱，
    那么我们合并的时候不希望把这些历史commit带过来，于是使用--squash进行合并，此时文件已经同合并后一样了,
    但不移动HEAD，不提交。需要进行一次额外的commit来“总结”一下，然后完成最终的合并。
    
    总结：
    --no-ff：不使用fast-forward方式合并，保留分支的commit历史
    --squash：使用squash方式合并，把多次分支commit历史压缩为一次

```

另外，还有一个问题就是，在开发得足够快的时候，你会觉得同时维护 Master 和 Developer 两个分支是一件很无聊的事，因为这两个分支在大多数情况下都是一样的。包括 Release 分支，你会觉得创建的这些分支太无聊。

而你的整个开发过程也会因为这么复杂的管理变得非常复杂。尤其当你想回滚某些人的提交时，你就会发现这事似乎有点儿不好干了。而且在工作过程中，你会来来回回地切换工作的分支，有时候一不小心没有切换，就提交到了不正确的分支上，你还要回滚和重新提交，等等。

## 2 分支详细介绍

- 首先有一个 master 版本，如果新开发一个功能，则会创建一个 develop 分支

![](https://img-blog.csdnimg.cn/ff8b012433ea46ff9cf563c766f48557.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

- 我们现在想开发一个`新功能`，glodstyle , 改变界面的皮肤，分别创建对应的分支

![在这里插入图片描述|400](https://img-blog.csdnimg.cn/d91c475947a4473f9e139a9dae2ea9bb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

- 再开发一个`小游戏功能`，分别创建对应的分支
![在这里插入图片描述|400](https://img-blog.csdnimg.cn/a2592ed4453342058e1dee7771e8791b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

- 如果线上跑的程序出现 bug 了，创建一个`热修复分支`。

![在这里插入图片描述|400](https://img-blog.csdnimg.cn/8367d84d2f4b44d0b46563ebecb6bc7e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

- 修复 bug 以后，尽快合并到 master 中。同时修复的版本，也要合并到 develop 分支上

![在这里插入图片描述|400](https://img-blog.csdnimg.cn/7a8dd59c93014334a35d1c809f63d880.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

- 两个功能分支继续往前推进，feature\_goldstyle 先开发完成，合并到 develop 分支上
![在这里插入图片描述|400](https://img-blog.csdnimg.cn/904520e5081e485b80f134ffbfb643c4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

- feature\_goldstyle 完成后，合并到 develop 分支，然后基于 develop 分支 创建预发布分支(release) ,在设个分支上进行上线前的测试。
![在这里插入图片描述|400](https://img-blog.csdnimg.cn/6e3aabfc3881417987407e3a591f8774.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

- 如果 release 分支出现 bug,则直接在 release 分支上进行发布，修复以后，合并到 master 分支上。开发分支也需要这个功能，也要合并到开发分支上。
![在这里插入图片描述|400](https://img-blog.csdnimg.cn/2561eb5135fe414d85000c77005a4720.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

- 然后 小游戏分支也开发完成了，合并到 develop 分支上，然后创建 release 分支，如果测试没问题，则合并到 master 分支上。
![在这里插入图片描述|400](https://img-blog.csdnimg.cn/e9ac0f581e8f4e55855f4c4b90b6193c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

hotfix 分支 修复完 bug 后，直接就删除了。也可以不需要 release 分支，直接在 develop 分支上进行测试。

分支类型
- master 主分之(生产环境分支)，**确保任何时刻该分之上的代码都是可发布的稳定的，不允许直接提交代码到该分支。**<u>为实现更严格的控制可以添加权限，只有主程序员才可操作该分支，普通开发员无权限。</u>
- develop 开发分支，该分支上的代码是开发完成且经过测试(自测)的代码。在多人协作开发的场景下不建议直接在该分支上提交代码应该配合功能分支、预发布分支和补丁分支来进行代码的合并
- feature/FEATURE\_NAME 功能分支
- release/vSEMATIC\_VERSION 预发布分支
- hotfix/HOTFIX\_NAME 补丁分支
- vMAJOR.MINOR.PATCH 版本标签

分支说明

| 分支类型 | 定义 | 作用 | 合并关系 | 建立时机 | 初始代码来源 |
| :---: | :---: | :---: | :---: | :---: | :---: |
| master | 主分支 | 记录每一个正式发布版本，TAG 所在分支 | 允许来自 release 和 hotfix 分支的合并 | 仓库初始化 | 仓库初始化 |
| hotfix | 补丁分支 | 修复已发布版本的 bug | 不允许来自任何分支的合并 | 已发布版本出现 BUG 时 | master(或 master 上的 TAG) |
| release | 预发布分支 | 表示预发布在测试 QA 环境的分支，待测试人员进行测试 | 不允许来自任何分支的合并 | develop 上代码满足发布要求 | 推荐使用 develop 上最新的 commit |
| develop | 开发分支 | 保持最新的经过自测的代码 | 允许来自 feature、release 和 hotfix 分支的合并 | master 创建完成后 | master |
| feature | 功能分支 | 开发独立的功能需求 | 不允许来自任何分支的合并 | 有独立的新功能需求时 | 推荐使用 develop 上最新的 commit |

### 2.1 历史分支

相对使用仅有的一个 master 分支，**Gitflow 工作流使用 2 个分支来记录项目的历史。** <mark style="background: #ABF7F7A6;">master 分支</mark>存储了正式发布的历史，而 <mark style="background: #ABF7F7A6;">develop 分支</mark>作为功能的集成分支。 这样也方便 master 分支上的所有提交分配一个<mark style="background: #ABF7F7A6;">版本号</mark>。

![在这里插入图片描述|400](https://img-blog.csdnimg.cn/7b6713e6950d42d7827503fe1e8eb237.png)

### 2.2 功能分支

**每个新功能位于一个自己的分支，这样可以 push 到中央仓库以备份和协作。** <mark style="background: #ABF7F7A6;">但功能分支不是从 master 分支上拉出新分支，而是使用 develop 分支作为父分支。当新功能完成时，合并回 develop 分支。 </mark>新功能提交应该从不直接与 master 分支交互。
![在这里插入图片描述](https://img-blog.csdnimg.cn/d439258cb248431d9857d21001fa1c3e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

注意，从各种含义和目的上来看，功能分支加上 develop 分支就是功能分支工作流的用法。但 Gitflow 工作流没有在这里止步。

### 2.3 发布分支

![](https://img-blog.csdnimg.cn/c8e27089b097406eb3ca7fb34608bc95.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

一旦 develop 分支上有了做一次发布（或者说快到了既定的发布日）的足够功能，<mark style="background: #ABF7F7A6;">就从 develop 分支上 fork 一个发布分支（Release）。 新建的分支用于开始发布循环，</mark>**所以从这个时间点开始之后新的功能不能再加到这个分支（Release）上—— 这个分支（Release）只应该做 Bug 修复、文档生成和其它面向发布任务。** <mark style="background: #ABF7F7A6;">一旦对外发布的工作都完成了，发布分支（Release）合并到 master 分支并分配一个版本号打好 Tag。 </mark>

**另外，这些从新建发布分支以来的做的修改要合并回 develop 分支。** (最后再删除 Release 分支)使用一个用于发布准备的专门分支，使得一个团队可以在完善当前的发布版本的同时，另一个团队可以继续开发下个版本的功能。 这也打造定义良好的开发阶段（比如，可以很轻松地说，『这周我们要做准备发布版本 4.0』，并且在仓库的目录结构中可以实际看到）。

**常用的分支介绍**

```
用于新建发布分支的分支: develop
用于合并的分支: master
分支命名: release-* 或 release/*
```

### 2.4 维护分支

![在这里插入图片描述](https://img-blog.csdnimg.cn/c880c306128d49e5a067a2fad9d66c0d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

**维护分支或说是热修复（hotfix）分支用于生成快速给产品发布版本（production releases）打补丁，这是唯一可以直接从 master 分支 fork 出来的分支。** 修复完成，修改应该马上合并回 master 分支和 develop 分支（当前的发布分支），master 分支应该用新的版本号打好 Tag。为 Bug 修复使用专门分支，让团队可以处理掉问题而不用打断其它工作或是等待下一个发布循环。 你可以把维护分支想成是一个直接在 master 分支上处理的临时发布。

## 3 GItFlow 的实现

### 3.1 使用命令来实现

1. 创建一个仓库
![在这里插入图片描述|400](https://img-blog.csdnimg.cn/666e49084b024a6d9a04544445dd6c38.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)
2. 然后 clone 项目

3. 为 master 分支配套一个 develop 分支，简单来说，可以在本地创建一个空的 develop 分支，push 到服务器上

```
git checkout -b develop  #基于当前分支
```

- 初始化项目工程，创建 springBoot 项目
- 提交分支到服务器上

```
git push --set-upstream origin develop
```

- 初始化框架，加上 springBoot 框架及样例


4. 以后这个分支将会包含了项目的全部历史，而 master 分支将包含了部分历史。其他开发者这时应该克隆中央仓库，建好 develop 分支的跟踪分支:

- 创建功能分支
 **程序员 A**

```
git clone http://192.168.53.197:3000/root/gitflow.git
git checkout develop # 切换到develop分支
git checkout -b feature/FEATURE-01 # 基于develop 分支创建功能分支
git push -u origin feature/FEATURE-01

```
- 做功能修改，添加功能
- 提交
- 程序员 A 完成了开发的功能。如果团队 使用 Pull requests ,这时候发起一个用于合并到 develop 分支。

否则可以直接合并到他本地的 develop 分支后 push 到中央仓库

```
# 拉取远程的develop分支，并且当前分支（本地分支some-feature）合并上远程分支develop
#第一条命令在合并功能前确保develop分支是最新的。注意，功能决不应该直接合并到master分支。 冲突解决方法和集中式工作流一样。
git pull origin develop
git checkout develop
git merge --no-ff feature/FEATURE-01 #合并分支到develop分支上
git push
# 删除本地分支
git branch -d feature/FEATURE-01
# 如果在服务器上有这个分支，则需要执行以下命令
git push origin --delete feature/FEATURE-01
```

**创建功能分支程序员 B**:

```
git clone http://192.168.50.197:3000/root/gitflow.git
git checkout develop # 切换到develop分支
git checkout -b feature/FEATURE-02 # 
```

功能开发完成后，需要合并分支

```
# 拉取远程的develop分支，并且当前分支（本地分支some-feature）合并上远程分支develop
#第一条命令在合并功能前确保develop分支是最新的。注意，功能决不应该直接合并到master分支。 冲突解决方法和集中式工作流一样。
git pull origin develop
git checkout develop
git merge --no-ff feature/FEATURE-01 #合并分支到develop分支上
git push
# 删除本地分支
git branch -d feature/FEATURE-02
```

6.在 develop 分支上创建 release 分支

这时候开始第一个项目的正式发布。像功能开发，用一个分支来做发布准备。

```
git checkout develop
git checkout -b release/v0.9.0
```

这个分支是清理发布、执行所有测试、更新文档和其它为下个发布做准备操作的底仓

测试人员进行测试

开发人员进行修复测试问题

```
git add .
git commit -m "fix:fix some ... "
```

如果测试没有问题，则合并到 master 分支上

一旦准备好了对外发布，就需要合并修改到 master 分支上和 develop 分支上。删除发布分支。

合并回 develop 分支很重要，因为在发布分支中已经提交的更新需要在后面的新功能中也要是可

用的。另外，如果团队要求 Code Review ，这时一个发起 Pull Request 的李想时机。

```
git checkout master
git merge --no-ff release/v0.9.0
git push 

# 把release 版本也合并到develop分支上
git checkout develop
git merge --no-ff release/v0.9.0
git push

git branch -d release-0.1.0
# If you pushed branch to origin:
git push origin --delete release-0.1.0   
```

发布分支是作为功能开发（develop 分支）和对外发布（master 分支）间的缓冲。只要有合并到 master 分支，就应该打好 Tag 以方便跟踪。

```
git tag -a v0.9.0 -m "Initial public release" master
git push --tags
```

Git 有提供各种钩子(hook) , 即仓库有事件发生时触发执行的脚本。可以配置一个钩子，在你 push 中央仓库的 master 分支上时，自动构建好对外发布。

7. 如果 master 在生产环境上出现 bug

```
git checkout master
git checkout -b hotfix/v0.9.0
```

进行修复 bug

8. 把修复的版本合并到 master 分支上

```
git checkout master
git merge --no-ff hotfix/v0.9.0
git tag v0.9.1
git push
git push origin v0.9.1

# 把补丁分支 合并到develop 分支上
git checkout develop
git merge --no-ff hotfix/v0.9.0
git push
git branch -d hotfix/v0.9.0
```

### 3.2 使用 source tree 来实现

1.首先在 gitlab 上创建一个项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/267ac8c5b2e8463baa54758dd565d94c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

2.在 source tree 创建分支

在本地把服务器上的项目拉下来

```
# 用命令行创建develop分支
git checkout -n develop
git push --set-upstream origin develop
```

然后用 source tree 打开
![在这里插入图片描述](https://img-blog.csdnimg.cn/7064e4e7cdfd425db6b5b350c72809ba.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

创建 feature-01 分支
![在这里插入图片描述](https://img-blog.csdnimg.cn/238d27888d854663800c0c8dfabd4f5c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

对 feature-01 分支 进行 修改上传

可以在此页面进行提交，点击 + 号

功能开发完成后，该怎么办呢？

再次点击 git 工作流：
![在这里插入图片描述](https://img-blog.csdnimg.cn/7b0fa13e5b8a4dc997bab53341bd08c2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

点击完成功能

![在这里插入图片描述](https://img-blog.csdnimg.cn/63e7398ac8b74262844322e5aca74b42.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_18,color_FFFFFF,t_70,g_se,x_16)

一般情况下，不会选择在开发分支上进行变基

点击确定

再次点击 GIt 工作流，创建预发布版本，一般在 develop 分支上。
![在这里插入图片描述](https://img-blog.csdnimg.cn/8995bcdd2caf459b83dfb57c5443ad11.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/e360aecaba8e4b5c859cb0fab4c3ba29.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_19,color_FFFFFF,t_70,g_se,x_16)

点击确定

如果预发布版本测试没有问题，则再次点击 Git 工作流，完成发布版本
![在这里插入图片描述](https://img-blog.csdnimg.cn/387a67e4ae734985bc70ca8ec85a5b45.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/af2f5dd71eb9477ba406df37ff52df33.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

点击确定：

加入我们在生产环境上出现 bug,补丁分支是基于 master 的

![在这里插入图片描述](https://img-blog.csdnimg.cn/0e2a039b862d4fabb01db09f2fb940dc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/02f15c315b4b473d96bda403e247f412.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_18,color_FFFFFF,t_70,g_se,x_16)

修复完成后，再次点击 GIt 工作流

![在这里插入图片描述](https://img-blog.csdnimg.cn/76bf3c4d2ba3457a879bfa7deccc89a7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/080002fe765f473fb87a62ab84fd66fd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_16,color_FFFFFF,t_70,g_se,x_16)

### 3.3 使用 gitlab 实现 git flow

![在这里插入图片描述|400](https://img-blog.csdnimg.cn/1066fd58e9dd4e298f63626e23d95675.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

可以安装 git flow 插件

1. 首先在 gitlab 上创建仓库

    把仓库代码拉到本地

    创建 develop 分支

```
# clone 项目
git clone http://192.168.50.197:82/rorm_group/recas.git
# 创建develop 分支 并切换分支
git checkout -b develop
# 添加和修改文件

# 把develop 分支提交到服务器
git push --set-upstream origin develop

```

设置 master 和 develop 为保护分支，且只有 master 角色才能 merge 和 push

2.成员进行 clone git clone

添加成员

3.项目组长分配里程碑与议题
![在这里插入图片描述](https://img-blog.csdnimg.cn/8e5913b16e19453f84ac6f32a6009af5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/9328d2ffdfdc49869732b62025a66dbb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

创建完成如下:

![在这里插入图片描述](https://img-blog.csdnimg.cn/51858fbc2d294f219dbc417db72d24ca.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

下面是创建 issue
![在这里插入图片描述](https://img-blog.csdnimg.cn/8f7e778bf190443cb5a51c35398458ae.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/8e87d537044f46c0b0d8bba9130d5c54.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

添加完成后，就可以在里程碑中看到添加的问题

![在这里插入图片描述](https://img-blog.csdnimg.cn/85ed19a7d3fd4d81b5cc9bd6b4ac0971.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

可以指派人解决

4.项目成员根据自己被分配的任务建立对应的分支

- 先在项目中建立分支，再拉取开发
- 先在本地建立分支，再提交

```
# 拉取代码
# clone 项目
git clone http://192.168.53.197:82/rorm_group/xx.git
# 创建develop 分支 并切换分支
git checkout -b question01
# 添加和修改文件

# 把develop 分支提交到服务器
git push --set-upstream origin question1

```

然后编辑文件

```
git add .
git commit -m "ddd"
git push origin feature/question01
```

开发人员完成开发后，在 gitlab 上 创建 MergeRequest
![在这里插入图片描述](https://img-blog.csdnimg.cn/84b710587cb14da081d6384b02d03acf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

- `fixes #xxx`

- `fixed #xxx` ： 关闭某一 issue

- `fix #xxx`

- `closes #xxx`

- `close #xxx`

- `closed #xxx` ： 关闭某一 issue

![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-5dPbpWrR-1650586937687)(D: \study\项目\GIt 分支管理\分支策略.assets\image-20211021095022645.png)]](https://img-blog.csdnimg.cn/04eb6930006045238ebd262585093e04.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

点击 修改分支

![](https://img-blog.csdnimg.cn/03e708424f1a471c8ed22f5cbcb0f1d5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

点击提交合并请求。

用管理员用户登录，右上角有合并请求
![在这里插入图片描述](https://img-blog.csdnimg.cn/20c4d3c7dbca4101a5ea100593256910.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

点击进去

![在这里插入图片描述](https://img-blog.csdnimg.cn/1aaf938416724f28859590d9befa7cb4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2K5aSPXzIwMjE=,size_20,color_FFFFFF,t_70,g_se,x_16)

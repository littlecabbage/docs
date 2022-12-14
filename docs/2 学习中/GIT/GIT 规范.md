# GIT 规范

> created: 2022-10-05T13:43:26
> tags: [git 规范]
> source: [原文地址](https://blog.csdn.net/luozochuan/article/details/124883747)
> author:

## 一、分支管理

### 1.1 分支定义

#### a. master 分支

- 主分支，最稳定，功能最完整的分支，可以随时发布的代码
- 以 Tag 标记一个版本，因此每一个 tag 都对应一个线上（生产）版本
- master 分支一般由 dev 以及 fix 分支合并，不允在 master 分支直接提交代码！！

#### b. dev 分支

- dev 分支始终保持功能最全、bug 修复后的代码，未发布正式前，功能最新最全的分支
- 新功能开发必须基于 dev 分支创建新功能的 feat 分支

#### c. feat 分支

- 开发新功能（feature）由 dev 为基础创建的新分支
- <mark style="background: #ABF7F7A6;">分支命名规则：feat-功能特性，如：feat-profile</mark>
- 该分支原则上不要提交到 git 中央库，功能开发测试完后，及时合并到 dev 分支，并删除该分支。但是如果一个新功能是和多人合作开发需将同步到 git 中央库。

#### d. fix 分支

- 线上出现紧急 bug，需要及时修复，以 master 分支为基础，建 fix 分支，修复完成后，需要合并到 master 分支和 dev 分支，并删除该分支。
- 分支命名规则：项目-fix-功能特性，如：fix-profile

#### e. 补充

稳定且长期存在的分支只有 master 和 dev 分支，别的分支则在完成对应开发使命之后都会合并到这两个分支然后被删除。

### 1.2 git 启用分支后的标准流程及命令

1.管理员创建 git 仓库，建立 master 分支，dev 分支, 命令行创建如下： 
```shell

git branch master

git push -u origin master

git branch dev

git push -u origin dev

```

2.项目成员 clone 仓库，并建立自己的 feat 功能分支
```shell

git clone 项目地址

git fetch

git checkout dev
```

创建本地功能分支 `git checkout -b 项目-feat-功能特性`

3.在本地功能分支开发，`git add, git commit &etc`, 注意功能模块为完成测试之前，禁止合并到 dev 或者 master 上

4.功能完成后可以直接合并本地的 dev 分支后 push 到远程 ，合并的时候很大几率 会发生冲突，此时需要合并代码，合并的确候确保不影响项目其他成员，如果多个人都操作了同一段代码，最好当面确认后 在进行修改。等合并完成确认无误后，删除本地 feat 开发分支, 命令行提示: 
```shell
git checkout dev

git pull origin dev

git merge 项目-feat-功能特性

git push origin dev

git branch -d 项目-feat-功能特性
```


5.发布版本之后，为 master 打上 tag

```shell
git checkout master

git tag -a 0.1 -m "Initial public release" master

git push --tags
```

6.bug 修复分支，如果正在开发功能的同时，dev 上线发现 bug，或者未上线发现的 bug，可以开一 个 fix 分支来修复 bug
```shell
git checkout -b fix-(bug-分支、特性)

** 修复完成 **

git checkout bug 分支

git merge fix-(bug-分支、特性)

git push origin bug 分支

git branch -d fix-(bug 分支、特性)
```


## 二、规范说明

### 2.1 用户规范

1、使用真实姓名提交代码，方便查询，通过命令：git config --global user.name 真实姓名 更新用户名

2、不要使用其他人的账号获取或提交代码，没有账号或密码的自行注册，并找管理员授权要访问的项目。

### 2.2 分支规范

1、禁止在 master 分支上开发代码

2、新功能分支以 feat-开头，bug 修复分支以 fix-开头

3、创建分支之前先同步 dev 代码，避免分支从一开始就是比较旧的代码，给将来合并带来不必要的麻烦。

4、需要紧急修复并发布的 bug，从标签版本创建分支，如果第一次没有标签从 master 分支创建，而不是其他

5、自己创建的临时分支比如 fix 分支不要 pull 到 git 服务器上，feature 分支是否推到远程，取决于你是否和你的小伙伴合作在该分支上开发

6、合并分支时，如果代码和另外一人冲突，需和他一起决定代码的去留，而不是自行决定

### 2.3 commit 规范

commit message 格式

示例如下

fix(dashboard): 更新 table 字段

type
- feat : 新功能
- fix : 修复 bug
- docs : 文档改变
- style : 代码格式改变
- refactor : 某个已有功能重构
- perf : 性能优化
- test : 增加测试
- build : 改变了 build 工具 如 grunt 换成了 npm
- revert : 撤销上一次的 commit
- chore : 构建过程或辅助工具的变动
- ci: 对 CI 配置文件和脚本的更改

scope
- 用来说明本次 Commit 影响的范围，即简要说明修改会涉及的部分。

Subject
- 用来简要描述本次改动，概述就好了，因为后面还会在 Body 里列出具体信息。

body
- 本次代码改动的详细信息

为了写出符合规范的提交信息，开发人员需安装插件 Git Commit Template↓：该插件让开发人员在界面上勾勾填填即可拼装出规范的 commit 内容
![](https://img-blog.csdnimg.cn/img_convert/dae091b30d46b787092dba7be97986e5.png)

常见不合规范的操作举例：

1. 创建分支前未，不同步最新的代码，从一开始就导致代码不一致了。
2. 在某个 feat 分支上敲代码，代码敲到一半，需要紧急修复 bug，切换到 master 分支前没有提交 feat 分支的代码，导致部分 feat 分支里面的代码跑到 master 分支上。
3. fix 分支不是以 master 分支或 tag 标签为基础创建的，而是直接在 dev 上创建分支，导致大量新的未经测试的功能发布
4. 通过 master 分支创建的分支，合并的时候却往 dev 上合并，会导致大量冲突，记住，分支就像平行宇宙，你从哪个宇宙穿越过来的，在新的宇宙空间中忙活完了，一定要回到你来的那个宇宙，而不是又穿越到第三个宇宙，否则时间线会乱掉，反映到代码中就是各种冲突。
5. 代码发布后没有打 tag 标签，导致紧急修复 bug 时，找不到上次发布的版本，因为 master 分支上的代码并不一定能保证没有合并过 dev 的代码。
6. 合并代码出现和别的开发人员有冲突，自行决定代码的去留，而不是两人一起合并。
7. 提交代码 commit message 随意写 123、aaa、几个空格等一些无意义的描述信息。

## 三、git 分支操作手册

### 1、开发新功能时 git 操作流程：

新建分支：

```shell

git checkout dev # 进入 dev 分支

git pull（或者直接界面按钮） # 拉取最新代码

git checkout -b feat-*** # 创建并切换至新的功能开发分支。

进行开发工作........

# 中间及时提交代码：
git add . 
git commit -m "***' #（或者界面操作，只 commit,不要 push,除非该 feat 是多人协作开发）
```


合并分支：

合并分支前的工作：

确保 `feat-***` 分支的代码都已 commit,或者 stash，
如果 feat 分支为多人合作开发，还需要 git pull 拉取远程代码。

```shell
git checkout dev # 切换到 dev 分支

git pull # 拉取 dev 新代码

git merge feat-*** # 合并新开发分支
```

当前 dev 分支包含了新功能的代码，打包发布测试环境，交测试人员测试，测试没问题合并到 master 分支，

```shell
git checkout master # 切换到 master 分支

git pull master # 拉取 master 最新代码

git merge dev # 合并 dev 分支代码

git tag -a v2.1.0 -m 'v2.1.0 发布版本' # 为当前 master 节点打一个 v2.1.0 的标签
```


### 2、修复 bug 时 git 操作流程：

每一次版本发布我们都会为新版本打一个 tag,如果上个版本为 v2.1.0，则执行如下命令：

```shell
git checkout -b fix-*** v2.1.0 # 以 v2.1.0 版本为基础创建修复分支 fix-***

***进行 bug 修复工作......***

# 中间及时提交代码 

git add . 
git commit -m '***' # (或者界面操作，只 commit,不要 push,除非该 fix 是多人协作开发）
```


修复工作完成后，打包发布测试环境，交开发人员测试，测试没问题将代码合并至 master


```shell
git checkout master # 切换至 master 分支

git pull # 拉取 master 代码

git merge fix-*** # 把修复的代码合并回 master 分支

git tag -a v2.1.1 -m 'v2.1.1 修复***的发布版本' # 为当前带发布的更新版本打 v2.1.1 的标签

git log # 查看 master 分支代码提交 id，记录下 bug 修复的 id

git checkout dev # 切换至 dev 分支

git pull # 拉代码

git cherry-pick commitId # 把 master 分支上的修复代码的那次提交合并到 dev 分支，这样修复的代码在 dev 和 master 上都有了

git branch -d fix-**** # (删除临时修复分支)
```


## 四、模拟一个完整的版本管理流程（必看）：

`git branch` 查看当前分支状态：

![](https://img-blog.csdnimg.cn/img_convert/63b8d4b5aadc923dca689b694b2a4e42.png)

`git pull` （开始敲代码之前先同步大家的代码）

`git checkout -b feat-yljgdm` （切换到分支 feat-yljgdm，参数-b 的意思是如果没有分支自动创建该分支）

![](https://img-blog.csdnimg.cn/img_convert/fbfaa5a087f976a0e40500920a6bdecf.png)

`git branch` （查看分支状态，当前分支前面有一个星号）

![](https://img-blog.csdnimg.cn/img_convert/399947b4c17d5cee546cc5c77c6dd623.png)

之所以新建一个 feat 分支，而不是直接在 dev 上开发，是为了保证 dev 上面都是开发完成的功能，能做到随时合并到 master 分支并进行发布的状态。

**......创建类，写代码，等等等一系列操作.....然后提交代码，这种临时的小的分支，只需 commit,不要 push**

![](https://img-blog.csdnimg.cn/img_convert/eb7879e076a77e436992b70c3912a127.png)

这个时候突然接收到一个用户反馈的 bug,需要紧急修复，并与今晚发布。

`git checkout master` （切换至主分支，切换分支之前，确保之前 feat 分支的代码已经 commit，否则切换其他分支后，这些没提交的代码也会带过去，因为没有提交到仓库的代码，git 是不会帮你管理的！！如果工作只进行一半代码还有问题确实不能提交。而别人又着急等着你去另一个分支解决问题，使用 git stash 命令把你当前分支未提交的代码隐藏起来。去其他分支干完活后再次来到你的分支，使用 git stash pop 命名恢复你上次的工作现场）

![](https://img-blog.csdnimg.cn/img_convert/950493ad83c95cd6d141304f51f4ede1.png)

`git pull` (更新主分支最新代码)

![](https://img-blog.csdnimg.cn/img_convert/f7ad3949a625032ae567a5278490ce9e.png)

git checkout -b fix-ztbz（从主分支创建修复分支）

（要点：一定要从主分支为基础创建分支，而不是 dev，否则修复 bug 发布，会带上很多新开发的功能，这些新特性可能还没测试，或者功能还不完整！）

![c532f2ec6122d9608bedd3c5bf348a47.png](https://img-blog.csdnimg.cn/img_convert/c532f2ec6122d9608bedd3c5bf348a47.png)

切换分支后发现，之前在 feat 分支上添加的内容已经已经看不到了

*在新建的 fix 分支上愉快的敲代码改 bug*

![b9ac702494b175672f93eac0ee70fb60.png](https://img-blog.csdnimg.cn/img_convert/b9ac702494b175672f93eac0ee70fb60.png)

`git add, git commit` （提交代码）

![](https://img-blog.csdnimg.cn/img_convert/a38de6332d95250095fa45cb3ebf01ba.png)

`mvn clean package` (项目打包，交给测试人员，这样打出来的包，只有，上次发布的代码和 bug 修复的代码)

测试没有问题：

`git tag -a v2.1.2 -m '修复 ztbz 为空的 bug'` (保证每一次发布的版本都打标签)

![](https://img-blog.csdnimg.cn/img_convert/b357dfa2a004ed2b5cdf397e739f9bbc.png)

`git push origin v2.1.2`（推送标签）

![](https://img-blog.csdnimg.cn/img_convert/ef70a98d3d2fc4624739fb08a96bdf4b.png)

`git checkout master`(切换到 master 分支)

![](https://img-blog.csdnimg.cn/img_convert/75de13f17c23c3dacf9c13edc3674fef.png)

`git pull origin master`(拉取 master 最新代码)

`git merge fix-ztbz` (把修复的代码合并到 master 分支)

`git branch -d fix-ztbz` (删除临时分支)

![](https://img-blog.csdnimg.cn/img_convert/ff918c148fde4a2beaed31c78de42fad.png)

修复 bug 的代码 master 里面已经有了，如何把这部分代码同步到 dev 分支呢：

`git log` (master 分支查看变更记录，找到 bug 修复的那次提交，并得到 git 变更 id)

![](https://img-blog.csdnimg.cn/img_convert/74db47a0fd97c0d97107d9e20457f3af.png)

`git checkout dev` （切换至 dev 分支）

`git cherry-pick d42e907ee4fb714e24fc0eb6c48b1265710a2ccb` (通过 cherry-pick 命令将这次提交的代码复制到 dev 分支)

完事！可以通过 `git checkout feat-yljgdm` 命令切换到上次新功能开发的分支，继续搬砖。


*过了两天，测试发现上次修复的 ztbz,把 0 和 1 搞反了，要求把状态标志定义成枚举或者常量，不要使用魔法值，但是上次的 fix-分支已经删掉了，如何找到上次修复的代码呢。上次修复完发布更新程序，我们打了个 tag*



`git checkout -b fix-ztbz2 v2.1.2` (以标签 v2.1.2 为基础创建分支：fix-ztbz2)

![](https://img-blog.csdnimg.cn/img_convert/b2bbe76ae932ceb27419d6d750b32b61.png)

`git branch` (已创建并切换至分支 fix-ztbz2)

![](https://img-blog.csdnimg.cn/img_convert/17bffbed2ddc64a6f1f82636cec44f66.png)

这时的分支状态为上次修复完 bug 后的代码。在此基础上继续修改。

再次修复 bug

![dbc152690e5cb0e97d3152ee37704e53.png](https://img-blog.csdnimg.cn/img_convert/dbc152690e5cb0e97d3152ee37704e53.png)

`git add . git commit -m 'fix:(修复状态标志 bug)'`

![](https://img-blog.csdnimg.cn/img_convert/de00f3e074776bbcab7fe6769b49d99e.png)

`mvn clean package` 打包发布

`git tag -a v2.1.3 -m '添加状态标志'`

![](https://img-blog.csdnimg.cn/img_convert/3bcb0fcb3b2ca370727fc45ddbc3462d.png)

`git checkout master (切换到 master 分支)`

`git merge fix-ztbz2 (合并 fix-ztbz2 至 masterv 分支)`

`git branch -d fix-ztbz2 (删除临时分支)`

**可以看到再次修复 bug 的 git 过程，跟第一次修复 bug 很像，唯一的区别这次我们是从 tag 中创建的分支。**

bug 修复的工作做完了，现在回到新功能的开发：

`git checkout feat-yljgdm`

....开始新的工作

搞完了及时合并到 dev 分支，过程同上，不再赘述。

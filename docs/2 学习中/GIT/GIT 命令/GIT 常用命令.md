---
Author: sync
date: 2022-10-05 13:39 Wednesday
tag: null
---

# GIT 常用命令

## start a working area (see also: git help tutorial)
开始一个工作区
   `clone`     Clone a repository into a new directory, 将存储库克隆到新目录
   `init`      Create an empty Git repository or reinitialize an existing one, 创建空 Git 存储库或重新初始化现有存储库

## work on the current change (see also: git help everyday
处理当前更改
   `add`       Add file contents to the index, 将文件内容添加到 `index`
   `mv`        Move or rename a file, a directory, or a symlink, 移动或重命名文件、目录或符号链接
   `restore`   Restore working tree files, 还原工作树文件
   `rm`        Remove files from the working tree and from the index, 从工作树和 `index` 中删除文件

## examine the history and state (see also: git help revisions)
检查历史和状态
   `bisect`    Use binary search to find the commit that introduced a bug, 使用 `binary search` 查找引起 bug 的提交
   `diff`      Show changes between commits, commit and working tree, etc
   `grep`      Print lines matching a pattern
   `log`       Show commit logs
   `show`      Show various types of objects
   `status`    Show the working tree status

## grow, mark and tweak your common history
发展、标记和调整你的共同历史
   `branch`    List, create, or delete branches
   `commit`    Record changes to the repository
   `merge`     Join two or more development histories together
   `rebase`    Reapply commits on top of another base tip
   `reset`     Reset current HEAD to the specified state
   `switch`    Switch branches
   `tag`       Create, list, delete or verify a tag object signed with GPG

## collaborate (see also: git help workflows)
   `fetch`     Download objects and refs from another repository, 从其他存储库下载对象和引用
   `pull`      Fetch from and integrate with another repository or a local branch, 从另一个存储库或本地分支获取**并与之集成**
   `push`      Update remote refs along with associated objects

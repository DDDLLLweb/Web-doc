[Git DOCUMENT](https://git-scm.com/book/en/v2)
>  git add 和 git stage 区别
   >> 其实两个是同义的，引入git stage 的原因是因为与svn add 区分，两者的功能是完全不一样的，svn add 是将某个文件加入版本控制，而git add 是将某个文件加入暂存区
---
### git 仓库的组成部分
- 工作区：在 git 管理下的正常目录都算是工作区，我们平时的编辑工作都是在工作区完成。
- 暂存区：临时区域。里面存放将要提交文件的快照。
- 历史记录区：git commit 后的记录区。
---
### git reset、git revert 和 git checkout 有什么区别？
- git reset 可以将一个分支的末端指向之前的一个 commit。然后再下次 git 执行垃圾回收的时候，会把这个 commit 之后的 commit 都扔掉。git reset 还支持三种标记，用来标记 reset 指令影响的范围：--mixed：会影响到暂存区和历史记录区。也是默认选项；--soft：只影响历史记录区；--hard：影响工作区、暂存区和历史记录区。
- git checkout 可以将 HEAD 移到一个新的分支，并更新工作目录。因为可能会覆盖本地的修改，所以执行这个指令之前，你需要 stash 或者 commit 暂存区和工作区的更改。
- git revert 和 git reset 的目的是一样的，但是做法不同，它会以创建新的 commit 的方式来撤销 commit，`这样能保留之前的 commit 历史，比较安全。另外，同样因为可能会覆盖本地的修改，所以执行这个指令之前，你需要 stash 或者 commit 暂存区和工作区的更改

###git 合并指定文件到另一个分支
1. 将分支devyaomy上的LivePayListAction.java文件改动合并到master分支上，而不是将devyaomy上整个分支的改动合并到master上可以使用命令
- 首先要切换到master分支
    
       git checkout master
- 执行合并命令 

       git checkout --patch devyaomy file
2. 强制覆盖

       git reset --hard origin/master

### 合并某个分支上的单个commit
1. 比如feature 分支上的commit 82ecb31 非常重要，它含有一个bug的修改，现在需要将82ecb31 合并到master，而不合并feature上的其他commits
```js
    git checkout master  
    git cherry-pick 82ecb31
```
### 合并某个分支上的一系列commits
1. 在一些特性情况下，合并单个commit并不够，你需要合并一系列相连的commits。这种情况下就不要选择cherry-pick了，rebase 更适合。还以上例为例，假设你需要合并feature分支的commit76cada ~62ecb3 到master分支
- 首先要基于feature创建一个新的分支，并指明分支的最后一个commit
```js
git checkout featuregit
git checkout -b newbranch 62ecb3
```
- 然后rebase这个新分支的commit到master。76cada^指明你想从哪个特定的commit开始

       git rebase --ontomaster 76cada^

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

....


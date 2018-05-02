# git是什么？
  git是开源的分布式版本控制系统，用于敏捷高效的处理任何大小项目
  ## git与svn的区别
  Git 与 SVN 区别点：    
  1.GIT是分布式的，SVN不是：这是GIT和其它非分布式的版本控制系统，例如SVN，CVS等，最核心的区别。   
  2.GIT把内容按元数据方式存储，而SVN是按文件：所有的资源控制系统都是把文件的元信息隐藏在一个类似.svn,.cvs等的文件夹里。   
  3.GIT分支和SVN的分支不同：分支在SVN中一点不特别，就是版本库中的另外的一个目录。   
  4.GIT没有一个全局的版本号，而SVN有：目前为止这是跟SVN相比GIT缺少的最大的一个特征。   
  5.GIT的内容完整性要优于SVN：GIT的内容存储使用的是SHA-1哈希算法。这能确保代码内容的完整性，确保在遇到磁盘故障和网络问题时降低对版本库的破坏。
# git工作区、暂存区和版本库
  ## 了解基本概念
  - 工作区:在电脑里能看到的目录    
  - 暂存区:英文叫stage, 或index。一般存放在 ".git目录下" 下的index文件     （.git/index）中，所以我们把暂存区有时也叫作索引（index）。      
  - 版本库:工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

  工作区、版本库中的暂存区和版本库之间的关系

  ![工作区、版本库中的暂存区和版本库之间的关系](http://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg)
# 创建版本库（repository）
简单说明：版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
简单总结：    
初始化一个Git仓库，使用git init命令。
添加文件到Git仓库，分两步：
- 第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；
+ 第二步，使用命令git commit，完成。    
* 简单解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录
我们使用 git clone 从现有 Git 仓库中拷贝项目（类似 svn checkout）   

克隆仓库的命令格式为：  
    
     git clone <repo>
克隆到指定的目录

    git clone <repo> <directory>

参数说明：  
- repo：repo:Git 仓库。
- directory:本地目录。
# 基本操作
### git init
    $ mkdir runoob
    $ cd runoob/
    $ git init
    Initialized empty Git repository in /Users/tianqixin/www/runoob/.git/
    #在 /www/runoob/.git/ 目录初始化空 Git 仓库完毕。

    ls -a #使用此命令查看项目中生成的隐藏的.git子目录
    .    ..    .git
### git clone
使用git clone 拷贝一个 Git 仓库到本地，让自己能够查看该项目，或者进行修改。

    git clone <url>
例如：

    git clone https://github.com/libgit2/libgit2
### git add 
git add 命令可将该文件添加到缓存
### git status
git status 以查看在你上次提交之后是否有修改。
### git diff
执行 git diff 来查看执行 git status 的结果的详细信息。
git diff 命令显示已写入缓存与已修改但尚未写入缓存的改动的区别。git diff 有-两个主要的应用场景。
- 尚未缓存的改动：git diff
- 查看已缓存的改动： git diff --cached
- 查看已缓存的与未缓存的所有改动：git diff HEAD
- 显示摘要而非整个 diff：git diff --stat
### git commit
使用 git add 命令将想要快照的内容写入缓存区， 而执行 git commit 将缓存区内容添加到仓库中。
### git reset HEAD
git reset HEAD 命令用于取消已缓存的内容
### git rm
如果只是简单地从工作目录中手工删除文件，运行 git status 时就会在 Changes not staged for commit 的提示。   
要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除，然后提交。可以用以下命令完成此项工作 

    git rm <file>
如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f

    git rm -f <file>
如果把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除，使用 --cached 选项即可    

    git rm --cached <file>
### git mv
git mv 命令用于移动或重命名一个文件、目录、软连接

    $ git mv README  README.md   #重命名
# git分支
创建分支命令：

    git branch (branchname)
切换分支命令:

    git checkout (branchname)
当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。    
删除分支命令：

    git branch -d (branchname)
合并分支命令:

    git merge 
你可以多次合并到统一分支， 也可以选择在合并之后直接删除被并入的分支。

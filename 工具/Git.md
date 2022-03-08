# Git设置



```plain
- 添加指定目录到 暂存区
git add file1 file2 ...

git add [dir]
- 添加当前目录所有文件到 暂存区

git add .
- 添加每个变化前，都会要求确认

- 对于同一个文件的多处变化，可以实现分次提交
git add -p

- 把文件同时从删除 工作区 和 暂存区删除
git rm file1 file2

- 停止追踪指定文件，但该文件会保留在 工作区
git rm --cached file

- 改名文件，并且将这个文件放入暂存区
git mv oldFile newFile
```

# 代码提交

```plain
- 提交暂存区到仓库区并添加注释
git commit -m "提交 readme.txt"

- 提交暂存区指定文件到仓库区
git commit file1 file2 ... -m "提交file1和file2"

- 提交工作区自上次commit之后的变化，直接到仓库区
git commit -a

- 提交时显示所有diff信息
git commit -v

- 使用一次新的commit，替代上一次提交   ？ what ？
- 如果代码没有任何新的变化，则用来改写上一次commit的提交信息
git commit --amend -m "提交啦"

- 重做上一次commit，并包括指定文件的变化
git commit --amend file1 file2
```

# 分支

```plain
- 创建并切换分支
git checkout -b dev

- 列出所有本地分支
git  branch

- 列出所有远程分支
git  branch -r

- 列出所有本地分支 + 远程分支
git branch -a

- 切换分支，并更新工作区
git checkout dev

- 新建分支，但依然停留在当前分支
git branch feature/001

- 新建分支，并切换到该分支
git checkout -b feature/001

- 新建一个分支，指定提交：
git branch [branch] [commit]

- 新建一个分支，与指定的远程分支建立追踪关系  ※ 
git branch --track [branch] [remote-branch]

- 建立追踪关系，在现有分支与指定的远程分支之间  ※ 
git branch --set-upstream dev origin/dev

- 移除追踪关系
git remote rm upstream
git remote add upstream https://github.com/xxx/xxx.git

- 切换到上一分支
git checkout -

- 选择一个commit，合并进当前分支
git cherry-pick [commit]

- 删除分支
git branch -d feature/001

- 删除远程分支 
git push origin --delete feature/001 

git branch -dr feature/001
- 在master上合并dev分支的内容
git merge dev
git branch -d dev

- 终止 merge
git merge --abort
- 分支管理策略
通常合并分支时，git一般使用”Fast forward”模式，在这种模式下，删除分支后，会丢掉分支信息，
现在我们来使用带参数 –no-ff 来禁用”Fast forward”模式。

git merge --no-ff -m "merge with no-ff" dev 
git branch -d dev  最后在查看历史记录，发现被删除的分支信息还在

- 查看历史记录
git log --graph
git log --graph --pretty=oneline --abbrev-commit
- bug分支、stash功能
git stash
- 查看stash记录
git stash list
- 恢复/删除
git stash apply 恢复，
git stash drop 删除 ，
git stash pop 恢复+删除
```



# 标签Tag

```plain
- 添加tag 
git tag v1.0

- 查看tag提交信息
git show v1.0

- 本地删除tag
git tag -d v1.0

- 远程删除tag
git push origin --delete v1.0

- 检出tag
git checkout v1.0.0

- 查看tag列表
git tag -l

- 后期打标签
先查看提交历史 
git log --pretty=oneline
	6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'master'
    9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
	0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc added a commit
再在相应的记录后面打tag
git tag -a v1.2 9fceb02
```

# 查看记录

```plain
- 显示有变更的文件，commit之前之后都调一次
git status

- 显示当前分支的版本历史
git log

- 显示commit历史，以及每次commit发生变更的文件
git log --stat

- 根据关键字搜索提交历史
git log -S [keyword]

- 显示某个文件的版本历史，包括文件改名
git log --follow file1
git whatchanged file1
- 查看对 readme.txt 文件修改的历史记录，从近到远显示日志
git log
git log --pretty=oneline
`显示最近五次提交` 
git log -5 --pretty=oneline   等价于 git log -5 --pretty --oneline
- 显示指定文件是什么人在什么时间修改过
git blame file1
- 显示暂存区和工作区的差异
git diff
- 查看 readme.txt 改变了哪些内容
git diff readme.txt
- 显示工作区与当前分支最新commit之间的差异
git diff HEAD
- 显示今天你写了多少行代码
git diff --shortstat "@{0 day ago}"
- 显示当前分支的最近几次提交
git reflog
```

# 远程同步

```plain
- 下载远程仓库的所有变动
git fetch [remote]
- 显示所有远程仓库
git remote -v
- 显示某个远程仓库的信息  -  ？失败？
git remote show [remote]
- 增加新的远程仓库，并命名
git remote add feature/001 xxx.git
- 取回远程仓库的变化，并与本地分支合并
git pull origin dev
- 上传本地分支到远程
git push origin dev
- 强行推送当前分支到远程仓库，即使有冲突
git push origin --force
- 推送所有分支到远程仓库
git push [remote] --all
git push origin --all
```

# 撤销修改

```plain
- 恢复暂存区的指定文件到工作区
git checkout -- file1
- 恢复暂存区所有文件到工作区
git checkout .
- 重置暂存区与工作区，与上一次commit保持一致
git reset --hard
- 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
git reset --hard [commit]
- 重置当前HEAD为指定commit，但保持暂存区和工作区不变
git reset --keep [commit]
- 新建一个commit，用来撤销指定commit
- 后者的所有变化都将被前者抵消，并且应用到当前分支
git revert [commit]
```

当 readme.txt 文件中有问题未提交之前，git status 命令提示的两种方法：

1. 直接修改文件内容，add/commit；
2. git checkout -- readme.txt   会丢弃掉工作区的修改，优先从 暂存区 里拿
   - readme.txt自动修改后，还没有放到暂存区，使用 撤销修改就回到和版本库一模一样的状态。
   - 另外一种是readme.txt已经放入暂存区了，接着又作了修改，撤销修改就回到添加暂存区后的状态。

# 删除文件

eg: touch b.md -> git add b.md -> git commit -m "Add b.md" -> rm b.md

```plain
- git status 显示 deleted: b.md
此时b.md已从工作区删除但仍存在于stage（暂存区），可通过:
- 恢复文件到工作区
git checkout -- b.md 恢复文件到工作区
如果从暂存区删除，在rm 之后调用 : git commit - a   之后会提示提交 message的vim窗口。或者：git commit -a -m "重命名 Git常用命令.md"
- git清除本地缓存（改变成未track状态），然后再提交:
git rm -r --cached .
- 删除多提交的目录,比如.idea,build文件夹:
$ git pull origin master 将远程仓库里面的项目拉下来
$ dir  查看有哪些文件夹
$ git rm -r --cached .idea  删除.idea文件夹
$ git commit -m '删除了.idea'  提交,添加操作说明
$ git push -u origin master 将本次更改更新到github项目上去
  操作完成.
```

# 多人协作

```plain
- 查看远程仓库信息
git remote  加上 -v 查看详细信息
- 创建本地dev分支 + 从远程检出dev分支 
git checkout -b dev origin/dev
`修改内容后 ` git pull origin dev , git push origin dev
- git pull 失败，可能是没有指定本地dev分支与远程origin/dev分支的链接，根据提示设置dev origin/dev 的链接： 
git branch --set-upstream dev origin/dev ;
git pull
```

**如果GitHub上新建的仓库为空**

```plain
目前，在GitHub上的这个testgit仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，
也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。
现在，我们根据GitHub的提示，在本地的testgit仓库下运行命令：
git remote add origin https://github.com/javakam/gitpractice.git
```

**把本地仓库的 master 分支推到远程仓库 ，第一次加上 -u**

```plain
由于远程库是空的，我们第一次推送master分支时，加上了 –u参数，Git不但会把本地的master分支内容推送的远程新的master分支，
还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
git push -u origin master
```

# 文件状态

**git库所在的文件夹中的文件大致有`四种状态`**

## `Untracked`

未跟踪, 此文件在文件夹中, 但并没有加入到git库, 不参与版本控制. 通过git add 状态变为Staged.

## `Unmodify`

文件已经入库, 未修改, 即版本库中的文件快照内容与文件夹中完全一致. 这种类型的文件有两种去处, 如果它被修改, 而变为Modified. 如果使用git rm移出版本库, 则成为Untracked文件

## `Modified`

文件已修改, 仅仅是修改, 并没有进行其他的操作. 这个文件也有两个去处, 通过git add可进入暂存staged状态, 使用git checkout 则丢弃修改过, 返回到unmodify状态, 这个git checkout即从库中取出文件, 覆盖当前修改

## `Staged`

暂存状态. 执行git commit则将修改同步到库中, 这时库中的文件和本地文件又变为一致, 文件为Unmodify状态. 执行git reset HEAD filename取消暂存, 文件状态为Modified

## Git untracked 和 not staged的区别

1. untrack     表示是新文件，没有被add过，是为跟踪的意思。
2. not staged  表示add过的文件，即跟踪文件，再次修改没有add，就是没有暂存的意思

# 存储模型

git 区别与其他 vcs 系统的一个最主要原因之一是：git 对文件版本管理和其他 vcs 系统对文件版本的实现理念完成不一样。这也就是 git 版本管理为什么如此强大的最核心的地方。

Svn 等其他的 VCS 对文件版本的理念是以文件为水平维度，记录每个文件在每个版本下的 delta 改变。

Git 对文件版本的管理理念却是以每次提交为一次快照，提交时对所有文件做一次全量快照，然后存储快照引用。

Git 在存储层，如果文件数据没有改变的文件，Git 只是存储指向源文件的一个引用，并不会直接多次存储文件，这一点可以在 pack 文件中看见。

如下图所示：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200520124026883.png)

# 向开源项目提交代码

🍎 [gist.github.com/zxhfighter/…](https://gist.github.com/zxhfighter/62847a087a2a8031fbdf)

# 每次开发完新`feature`之后

```plain
git pull origin develop 
git push origin feature/dialog 
After PR merged : 
git branch -dr feature/dialog 
push之后，GitHub上会有黄色提示窗提示Pull Request，提完PR后如果没问题，
直接merge，若需要review时候，可直接在review上修改，也可以在IDE上修改之后
再次执行上面的拉取和推送操作，此时不会创建新的PR。
```

![img](https://cdn.nlark.com/yuque/0/2021/jpeg/1567186/1612932810838-0657fa07-6bb0-4ae7-83e2-937eca386861.jpeg)
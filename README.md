# learnGit

## git add 提交到暂存区，出错怎么办
一般代码提交流程为：工作区 -> git status 查看状态 -> git add . 将所有修改加入暂存区-> git commit -m "提交描述" 将代码提交到 本地仓库 -> git push 将本地仓库代码更新到 远程仓库
场景1：工作区
当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
需要写上文件的后缀名

## 暂存区
当你不但改乱了工作区某个文件的内容，还 git add 添加到了暂存区时，想丢弃修改，分两步，第一步用命令 git reset HEAD <file>，就回到了场景1，第二步按场景1操作。
// 把暂存区的修改撤销掉（unstage），重新放回工作区。
git reset HEAD <文件名> 

## 三、git commit 提交到本地仓库，出错怎么办？
### 1. 提交信息出错
更改 commit 信息
```
git commit --amend -m“新提交消息”
```
### 2. 漏提交
commit 时，遗漏提交部分更新，有两种解决方案：
#### 方案一：再次 commit
```
git add missed-file 
git commit -m "commit message"
```
此时，git 上会出现两次 commit

#### 方案二：遗漏文件提交到之前 commit 上
````
git add missed-file // missed-file 为遗漏提交文件
git commit --amend --no-edit
````
--no-edit 表示提交消息不会更改，在 git 上仅为一次提交

### 3. 提交错误文件，回退到上一个 commit 版本，再 commit
git reset
删除指定的 commit
```
// 修改版本库，保留暂存区，保留工作区
// 将版本库软回退1个版本，软回退表示将本地版本库的头指针全部重置到指定版本，且将这次提交之后的所有变更都移动到暂存区。
git reset --soft HEAD~1

// 修改版本库，修改暂存区，修改工作区
//将版本库回退1个版本，不仅仅是将本地版本库的头指针全部重置到指定版本，也会重置暂存区，并且会将工作区代码也回退到这个版本
git reset --hard HEAD~1
// git版本回退，回退到特定的commit_id版本，可以通过git log查看提交历史，以便确定要回退到哪个版本(commit 之后的即为ID);
git reset --hard commit_id 

```
git revert
撤销某次操作，此次操作之前和之后的commit和history都会保留，并且把这次撤销

作为一次最新的提交
```
// 撤销前一次 commit
git revert HEAD
// 撤销前前一次 commit
git revert HEAD^
// (比如：fa042ce57ebbe5bb9c8db709f719cec2c58ee7ff）撤销指定的版本，撤销也会作为一次提交进行保存。
git revert commit

```
git revert是提交一个新的版本，将需要revert的版本的内容再反向修改回去， 版本会递增，不影响之前提交的内容
git revert 和 git reset 的区别

git revert是用一次新的commit来回滚之前的commit，git reset是直接删除指定的commit。
在回滚这一操作上看，效果差不多。但是在日后继续merge以前的老版本时有区别。因为git revert是用一次逆向的commit“中和”之前的提交，因此日后合并老的branch时，导致这部分改变不会再次出现，但是git reset是之间把某些commit在某个branch上删除，因而和老的branch再次merge时，这些被回滚的commit应该还会被引入。
git reset 是把HEAD向后移动了一下，而git revert是HEAD继续前进，只是新的commit的内容和要revert的内容正好相反，能够抵消要被revert的内容。
## git 常用命令

1. 初始开发 git 操作流程
克隆最新主分支项目代码 git clone 地址
创建本地分支 git branch 分支名
查看本地分支 git branch
查看远程分支 git branch -a
切换分支  git checkout 分支名 (一般修改未提交则无法切换，大小写问题经常会有，可强制切换  git checkout 分支名 -f  非必须慎用)
将本地分支推送到远程分支 git push <远程仓库> <本地分支>:<远程分支>

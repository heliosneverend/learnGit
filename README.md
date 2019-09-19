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
### 1. 初始开发 git 操作流程
克隆最新主分支项目代码 git clone 地址
创建本地分支 git branch 分支名
查看本地分支 git branch
查看远程分支 git branch -a
切换分支  git checkout 分支名 (一般修改未提交则无法切换，大小写问题经常会有，可强制切换  git checkout 分支名 -f  非必须慎用)
将本地分支推送到远程分支 git push <远程仓库> <本地分支>:<远程分支>
git push origin Helios_t 将Helios_t分支推到远端

### 2. git fetch
将某个远程主机的更新，全部/分支 取回本地（此时之更新了Repository）它取回的代码对你本地的开发代码没有影响，如需彻底更新需合并或使用git pull

### git pull
拉取远程主机某分支的更新，再与本地的指定分支合并（相当与fetch加上了合并分支功能的操作）

###  git push
将本地分支的更新，推送到远程主机，其命令格式与git pull相似

### 分支操作
使用 Git 下载指定分支命令为：git clone -b 分支名仓库地址
拉取远程新分支 git checkout -b serverfix origin/serverfix
合并本地分支 git merge hotfix：(将 hotfix 分支合并到当前分支)
合并远程分支 git merge origin/serverfix
删除本地分支 git branch -d hotfix：(删除本地 hotfix 分支)
删除远程分支 git push origin --delete serverfix
上传新命名的本地分支：git push origin newName;
创建新分支：git branch branchName：(创建名为 branchName 的本地分支)
切换到新分支：git checkout branchName：(切换到 branchName 分支)
创建并切换分支：git checkout -b branchName：(相当于以上两条命令的合并)
查看本地分支：git branch
查看远程仓库所有分支：git branch -a
本地分支重命名： git branch -m oldName newName
重命名远程分支对应的本地分支：git branch -m oldName newName
把修改后的本地分支与远程分支关联：git branch --set-upstream-to origin/newName

## 五、优化操作
### 1. 拉取代码 pull --rebase
在团队协作过程中，假设你和你的同伴在本地中分别有各自的新提交，而你的同伴先于你 push 了代码到远程分支上，所以你必须先执行 git pull 来获取同伴的提交，然后才能push 自己的提交到远程分支。
而按照 Git 的默认策略，如果远程分支和本地分支之间的提交线图有分叉的话（即不是 fast-forwarded），Git 会执行一次 merge 操作，因此产生一次没意义的提交记录，从而造成了像上图那样的混乱。
其实在 pull 操作的时候，，使用 git pull --rebase选项即可很好地解决上述问题。 加上 --rebase 参数的作用是，提交线图有分叉的话，Git 会 rebase 策略来代替默认的 merge 策略。
假设提交线图在执行 pull 前是这样的：

````
A---B---C  remotes/origin/master
/
D---E---F---G  master
````
如果是执行 git pull 后，提交线图会变成这样：
```
A---B---C remotes/origin/master
/         \
D---E---F---G---H master
```
结果多出了 H 这个没必要的提交记录。如果是执行 git pull --rebase 的话，提交线图就会变成这样
```
remotes/origin/master
|
D---E---A---B---C---F'---G'  master
```
F G 两个提交通过 rebase 方式重新拼接在 C 之后，多余的分叉去掉了，目的达到。
小结
大多数时候，使用 git pull --rebase是为了使提交线图更好看，从而方便 code review。
不过，如果你对使用 git 还不是十分熟练的话，我的建议是 git pull --rebase多练习几次之后再使用，因为 rebase 在 git 中，算得上是『危险行为』。
另外，还需注意的是，使用 git pull --rebase比直接 pull 容易导致冲突的产生，如果预期冲突比较多的话，建议还是直接 pull。

```
注意：

git pull = git fetch + git merge

git pull --rebase = git fetch + git rebase
```
## 合代码 merge --no-ff
上述的 git pull --rebase 策略目的是修整提交线图，使其形成一条直线，而即将要用到的 git merge --no-ff <branch-name> 策略偏偏是反行其道，刻意地弄出提交线图分叉出来。
假设你在本地准备合并两个分支，而刚好这两个分支是 fast-forwarded 的，那么直接合并后你得到一个直线的提交线图，当然这样没什么坏处，但如果你想更清晰地告诉你同伴：这一系列的提交都是为了实现同一个目的，那么你可以刻意地将这次提交内容弄成一次提交线图分叉。
执行 git merge --no-ff <branch-name> 的结果大概会是这样的：
![1](/assets/1.png)
中间的分叉线路图很清晰的显示这些提交都是为了实现 complete adjusting user domains and tags
更进一步
往往我的习惯是，在合并分支之前（假设要在本地将 feature 分支合并到 dev 分支），会先检查 feature 分支是否『部分落后』于远程 dev 分支：
```
git checkout dev
git pull # 更新 dev 分支
git log feature..dev
```
如果没有输出任何提交信息的话，即表示 feature 对于 dev 分支是 up-to-date 的。如果有输出的话而马上执行了 git merge --no-ff 的话，提交线图会变成这样：
![2](/assets/2.png)
所以这时在合并前，通常我会先执行：
```
git checkout feature
git rebase dev
```
这样就可以将 feature 重新拼接到更新了的 dev 之后，然后就可以合并了，最终得到一个干净舒服的提交线图。

再次提醒：像之前提到的，rebase 是『危险行为』，建议你足够熟悉 git 时才这么做，否则的话是得不偿失啊。
总结
使用 git pull --rebase 和 git merge --no-ff 其实和直接使用 git pull git merge 得到的代码应该是一样。
使用 git pull --rebase 主要是为是将提交约线图平坦化，而 git merge --no-ff 则是刻意制造分叉

七、暂存
git stash 可用来暂存当前正在进行的工作，比如想 pull 最新代码又不想 commit ， 或者另为了修改一个紧急的 bug ，先 stash，使返回到自己上一个 commit,，改完 bug 之后再 stash pop , 继续原来的工作；

添加缓存栈： git stash ;
查看缓存栈： git stash list ;
推出缓存栈： git stash pop ;
取出特定缓存内容： git stash apply stash@{1} ;


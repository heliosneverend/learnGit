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

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

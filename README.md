# learnGit

## git add 提交到暂存区，出错怎么办
一般代码提交流程为：工作区 -> git status 查看状态 -> git add . 将所有修改加入暂存区-> git commit -m "提交描述" 将代码提交到 本地仓库 -> git push 将本地仓库代码更新到 远程仓库
场景1：工作区
当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
需要写上文件的后缀名

## 暂存区

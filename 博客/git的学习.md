# git的学习

命令和Linux很相似，所以需要在前面加一个git来区分 

## 一些基本配置

配置名字和邮箱

`git config --global user.name wl`

`git config --global user.email wllei13@outlook.com`

查看配置的信息

`git config --global --list`

## 创建仓库的两种方式

本地

`git init （+项目名）`

远程

`git clone + github地址`

  

查看仓库状态

`git status`

查看本地分支列表

`git branch (-r“远端”)`

将文件添加到暂存区

```
git add +文件名

保存以txt结尾的文件 	git add *.txt
全部文件				git add .
```



取消暂存

`git rm --cached<file>...`

提交到仓库（只会提交暂存区的文件）

`git commit -m "提交时的备注信息"`

本地仓库上传到github

`git push -u origin main`

本地拉取远程github

git pull

查看提交记录

```
git log
简单提交：	git log --oneline
```



回退版本	git reset

| 命令                       | 工作区 | 暂存区 |
| -------------------------- | ------ | ------ |
| git reset --soft           | √      | √      |
| git reset --hard           | ×      | ×      |
| git reset --mixed（默认 ） | √      | ×      |

回溯操作	git reflog



git diff 		默认是工作区和缓存区之间的差异

git diff HEAD 	比较工作区和版本库之间的差距

git diff  --cached	比较暂存区和版本库之间的差距

git diff ID1 ID2		比较两个不同版本的差异



 删除文件  rm

直接使用rm可以删除文件，但是需要告诉暂存区，所以需要执行git add filename

git rm 直接删除

<u>删除文件以后记得提交</u>，否则删除的文件在版本库中还是存在的

![image-20240804191342806](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20240804191342806.png)



分支	branch

git branch fileName	创建分支

git branch			 查看分支情况

git checkout branchName		切换到不同的分支，但是可能出现错误，因为恢复文件也是用的这个命令

git switch branchNmae			专门用来切换分支

合并分支

git merge 将要被合并的分支名 		（当前所在的分支是合并后的目标分支）

git rebase 合并后的目标分支		

![image-20240804204055465](C:\Users\吴磊\AppData\Roaming\Typora\typora-user-images\image-20240804204055465.png)

## 初次远端提交方法

…or create a new repository on the command line

```
echo "# agricultural-products" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:fengmian13/agricultural-products.git
git push -u origin main
```

…or push an existing repository from the command line

```
git remote add origin SSH路径
git branch -M main
git push -u origin main
```

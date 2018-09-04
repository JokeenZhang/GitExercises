git branch 

git checkout -b second

git log

git status

git diff



## 分支

列出仓库中的所有分支：git branch，这个命令只能查看本地仓库的分支，git branch -a能查看所有分支，包括远程仓库的分支，如origin/HEAD，origin/master

创建分支，不切换：git branch -d 分支名

删除分支 ：git branch -d 分支名

删除指定分支：git branch -D 分支名

查看分支：git checkout 分支名

创建并切换分支：git checkout -b 分支名



## 操作

1. 创建新分支并切换到新分支company

2. 创建新的commit

3. git push origin company

4. 快快推送！！！

5. 再修改

6. 修改后需要commit，再去push，甚至是修改后需要重新add，然后再去commit，通过git status，如果修改后的文件是红色的，那么就需要去add，add后，再敲命令git status ，如果modified变成绿色，就可以了，准备提交吧

7. 切换分支到master，合并，并删除company分支

   ```nginx
   git checkout master 切换分支到master
   git pull 拉取远程的更新
   git merge company 合并master到master
   git push 推送到远程仓库
   git branch -d company 删除本地的master分支
   git push origin -d books 删除仓库的company分支
   ```


## 配置

1. 通过将Git配置变量 core.quotepath 设置为false，就可以解决中文文件名称在这些Git命令输出中的显示问题

   >  $ git config --global core.quotepath false



## 2018-09-04

### 操作



1. 创建分支develop
2. 修改并在分支提交
3. 推送修改到远程分支
4. 切换到本地master分支
5. 合并develop到master
6. 推送修改到远程master
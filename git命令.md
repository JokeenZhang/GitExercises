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

   git checkout -b develop

2. 修改并在分支提交

   git push origin develop

   git status

   git commit -m "温习git流程"

3. 推送修改到远程分支

   git push origin develop

4. 以上步骤多执行几次，再合并到master分支

5. 切换到本地master分支

6. 合并develop到master

7. 推送修改到远程master

总结：总的来说，在分支修改完成后，提交到远程分支，再在本地切换分支到master分支，合并分支，推送到远程master分支

发现：并不能产生多条线，给人的感觉好像是还是在同一个分支里工作一样，这与之前的工作流表现完全不同

![](.\images\工作流不同.png)

### 疑问

1. 合并不能产生多条线

   原因： master 前面没有任何的新 commit，这种合并会自动使用 fast forward，加上--no-ff，git merge develop --no-ff

   ![](.\images\关闭fast fordward.png)

   > git merge –no-ff 可以保存你之前的分支历史。能够更好的查看 merge历史，以及branch 状态。
   >
   > git merge 则不会显示 feature，只保留单条分支记录。

2. 当远程master分支比远程develop分支更超前，如何同步两个分支？

   git checkout develop

   git pull origin master

   git push origin develop
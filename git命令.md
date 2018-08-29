git branch 

git checkout -b second

git log

git status

git diff



## 分支

列出仓库中的所有分支：git branch

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
6. 修改后需要commit，再去push，甚至是修改后需要重新add，然后再去commit

## 配置

1. 通过将Git配置变量 core.quotepath 设置为false，就可以解决中文文件名称在这些Git命令输出中的显示问题

   >  $ git config --global core.quotepath false
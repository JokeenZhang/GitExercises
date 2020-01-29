git branch 

git checkout -b second

git log

git status

git diff

## 将项目添加到版本控制

### 创建 git 仓库:

```
mkdir PythonExercise
cd PythonExercise
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin git仓库地址
git push -u origin master
```

### 已有仓库?

```
cd existing_git_repo
git remote add origin git仓库地址
git push -u origin master
```



## 分支

列出仓库中的所有分支：git branch，这个命令只能查看本地仓库的分支，git branch -a能查看所有分支，包括远程仓库的分支，如origin/HEAD，origin/master

创建分支，不切换

```
git branch -d 分支名
```

删除分支 

```
git branch -d 分支名
```

删除指定分支

```
git branch -D 分支名
```

切换分支

```
git checkout 分支名
```

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

## remote

查看远程仓库地址

```
git remote -v
```

查看仓库名字

```
git remote
//得到origin，这个是默认的，可更改
```

添加远程仓库，下方origin是远程仓库的地址，可更改为其他名字

```
git remote add origin 仓库地址
```

修改仓库地址

```
git remote set-url origin 仓库地址
```

## 2018-09-04

### 操作

1. 创建分支develop

   ```
   git checkout -b develop
   ```

2. 修改并在分支提交

   ```
git push origin develop
   git status
git commit -m "温习git流程"
   ```

3. 推送修改到远程分支

   ```
   git push origin develop
   ```

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

   ```
git checkout develop
   git pull origin master
git push origin develop
   ```

## 2018-09-20

### Stash

> 背景：
>
> 1. 经常有这样的事情发生，当你正在进行项目中某一部分的工作，里面的东西处于一个比较杂乱的状态，而你想转到其他分支上进行一些工作。问题是，你不想提交进行了一半的工作，否则以后你无法回到这个工作点。解决这个问题的办法就是`git stash`命令
>
> 2. 当前分支有修改，不能直接切换到其他分支，需要`commit`或者`stash`，如果是先`commit`再去切换分支，无疑在`log`中增加了很多无意义的commit，这时候`stash`就派上用场了

#### 命令注解

1. `git stash `

   保存当前工作进度，会把暂存区和工作区的改动保存起来，执行完这个命令后，在运行`git status`命令，就会发现当前是一个干净的工作区，没有任何改动

2. `git stash list`

   显示保存进度的列表

3. `git stash pop stash [stash_id]`

   - `git stash pop` 恢复最新的进度到工作区。git默认会把工作区和暂存区的改动都恢复到工作区。
   - `git stash pop --index` 恢复最新的进度到工作区和暂存区。（尝试将原来暂存区的改动还恢复到暂存区）
   - `git stash pop stash@{1}`恢复指定的进度到工作区。stash_id是通过`git stash list`命令得到的 
     通过`git stash pop`命令恢复进度后，会删除当前进度。

4. `git stash apply stash [stash_id]`

   除了不删除恢复的进度之外，其余和`git stash pop` 命令一样。

5. `git stash drop [stash_id]`

   删除一个存储的进度。如果不指定stash_id，则默认删除最新的存储进度。

6. `git stash clear`

   删除所有存储的进度。

#### 场景

##### 冲突1

在使用过程中，有遇到如下场景：

在当前项目中已经有过修改，此时同事推送commit到远程仓库，我需要将那些commit拉取到本地，那么此时我就先将自己的修改通过`git stash`存储到stash里，通过`git pull`获取到同事修改的代码，然后再恢复自己的更新：`git stash pop`（因为删掉了之前的stash，直接拿最新的stash），发现有冲突，日志如下，但是去发现，这指明冲突的几个文件并无修改，但就是在工作区中，且之前保存的几处修改都消失了

```
Administrator@MINEW-66 MINGW64 /f/self (master|REBASE-i 1/1)
$ git stash apply stash@{0}
error: Your local changes to the following files would be overwritten by merge:
        app/src/main/A.java
        app/src/main/B.java
        app/src/main/C.java
Please commit your changes or stash them before you merge.
Aborting
```

这时，`git stash show -p | git apply && git stash drop`解决问题

```
Administrator@MINEW-66 MINGW64 /f/self (master|REBASE-i 1/1)
$ git stash show -p | git apply && git stash drop
Dropped refs/stash@{0} (c4f1cce0e9e43c468d4109f233d4770b0a37d672)
```

##### 冲突2

在两台电脑操作，先后clone responsity到这两台电脑，且分别做修改，A电脑`commit`修改并`push`到远程仓库里。B电脑要想`pull`此时的远程仓库的`commit`的话，需要：

```
#将B电脑的修改存到暂存区里，
git stash
#获取远程仓库的commit
git pull
#恢复进度
git stash pop
```

如上三个步骤会产生冲突。解决步骤是：

```
#查看冲突文件路径
git status
#如果文件标红，那么就将文件添加进git管理
git add .
#这时候就开始真正解决冲突，通过代码对比工具，解决冲突后再次恢复进度
#git stash pop
```

注：在这里我用的代码对比工具是Android Studio自带的git工具，在执行上述步骤的git add .之后，就开始正式按照Android Studio解决冲突的标准方式来解决冲突了，如下：

1. Android Studio工具栏中，`VCS——》Git——》resolve conflicts`，点击，则打开了一个对话框，如下：

   ![image](https://wx2.sinaimg.cn/large/e60a8256gy1fw8nzei6ekj20gq0fcjrg.jpg)

   其实能找到在哪里打开这个对话框才是更重要的一步。解决冲突反而是比较简单的，强调是在`VCS——》Git——》resolve conflicts`中打开

2. 解决完对话框所显示的冲突文件后，直接关闭就可以了

### 当前git配置

1. 获取**当前目录**下git配置信息

   在同一台电脑中既有公司的git账号，也有github账号，虽然已经将公司git账号设置为全局，特定文件夹用github上的账号，但还是需要检验一下，以免出现意外

   `git config user.name` 获取用户名称

   `git config user.email` 获取用户邮箱

2. 在指定目录下配置信息

   直接设定当前目录下的用户名为John，邮箱为johndoe@example.com

   `git config user.name "John"`

   `git config user.email johndoe@example.com`

3. 设置全局配置信息

   通过`--global`设置用户和邮箱，在任意文件夹下如果关联git，每次提交时都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录

   `git config --global user.name "John"`

   `git config --global user.email johndoe@example.com`

4. 检查已有的配置信息

   `git config --list` 不止能获取到当前目录下的name和email，还包括远程仓库地址、分支等等


## clone 

从仓库中clone带有library module的项目时，并不会把library module的内容也一并clone下来，但是会clone出.gitsubmodule这个描述文件。如果还需要检出submodule，那么还需要两条命令：

```
git@gitee.com:know_the_season/AppModule.git
# 进入AppModule
$ cd AppModule

$ git submodule init
Submodule 'BaseModule' (git@gitee.com:know_the_season/BaseModule.git) registered for path 'BaseModule'
Submodule 'BleModule' (git@gitee.com:know_the_season/BleModule.git) registered for path 'BleModule'

$ git submodule update
Cloning into 'D:/UserDocuments/Porject/AndroidMultiModule/BaseModule'...
Cloning into 'D:/UserDocuments/Porject/AndroidMultiModule/BleModule'...
Submodule path 'BaseModule': checked out '30d558935f357f7c6aeb273c8a0b4aca3be06898'
Submodule path 'BleModule': checked out 'cbcbf111f18a92c9eecc6ffc3f9efdac6d182483'
```

如果觉得这样多敲了多条命令的话，就直接用下面这条命令，一步到位。只是需要注意的是，如果有多个submodule或者submodule较大，那么就需要花很多的时间

```
git clone --recursive https://gitee.com/know_the_season/AppModule.git
$ git clone --recursive https://gitee.com/know_the_season/AppModule.git
Cloning into 'AppModule'...
remote: Enumerating objects: 18, done.
remote: Counting objects: 100% (18/18), done.
remote: Compressing objects: 100% (16/16), done.
remote: Total 18 (delta 2), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (18/18), done.
Resolving deltas: 100% (2/2), done.
Submodule 'BaseModule' (git@gitee.com:know_the_season/BaseModule.git) registered for path 'BaseModule'
Submodule 'BleModule' (git@gitee.com:know_the_season/BleModule.git) registered for path 'BleModule'
Cloning into 'F:/AppModule/AppModule/BaseModule'...
remote: Enumerating objects: 12, done.
remote: Counting objects: 100% (12/12), done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 12 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (12/12), done.
Cloning into 'F:/AppModule/AppModule/BleModule'...
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (10/10), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 10 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (10/10), done.
Submodule path 'BaseModule': checked out 'cd7210724eaf14271c3f7b11ebd5f8b6fca4c106'
Submodule path 'BleModule': checked out '44d7cc4fc676c224b5052da88285fb183a9d066a'
```

## add

即新建SubModule

```
//git submodule add <仓库地址> <本地路径>
git submodule add git@gitee.com:know_the_season/BaseModule.git BaseModule
```

如果命令中没有输入`本地路径`，那么就会直接添加到当前路径下

使用submodule add后，如果原项目中没有submodule，那么会直接创建.submodule，否则会修改.submodule，在该文件中，会有对于submodule的一些描述

```
[submodule "BaseModule"]
	path = BaseModule
	url = git@gitee.com:know_the_season/BaseModule.git
[submodule "BleModule"]
	path = BleModule
	url = git@gitee.com:know_the_season/BleModule.git
```

在项目中添加submodule就此完成

### 场景一：引入submodule并替换

在该场景中，是想将BaseModule抽取出来，并给其他项目中引入，称为一个“公用库”。在仓库中创建BaseModule并将对应文件上传到该仓库，就想着将这个仓库引入到AndroidMultiModule中

原项目目录结构如下：

```
AndroidMultiModule
    |-- BaseModule
    |-- BleModule
    |-- .gitignore
    |-- build.gradle
    ……
```

操作步骤：

1. 直接在文件夹目录中删除指定目录：BaseModule

2. 打开命令行工具（git base here）,接下来就全程在根目录下进行

3. 输入

   ```
   $ git submodule add git@gitee.com:know_the_season/BaseModule.git BaseModule
   'BaseModule' already exists in the index
   ```

   在这里直接提示失败

解决方法：

```
$ git ls-files --stage base
```

该输出的第一列将告诉您projectfolder索引中的对象类型。如果是列出的对象类似大概如（以160000开始）：

```
160000 d00cf29f23627fc54eb992dde6a79112677cd86c 0   BaseModule/.gitignore
```

那么就可以使用如下命令来删除

```
git rm --cached BaseModule
```

通过这条命令，可以将BaseModule从暂存区或分支中删除。但是如果对象类型是

还可能会看到列出了许多Blob（文件模式为100644和100755），这向我建议您在将新存储库复制到位之前，没有正确地取消暂存项目文件夹中的文件。这种情况下，可以使用如下命令来删除（递归删除）

```
git rm -r --cached BaseModule
```

删除完成后，就能使用`git submodule add`命令来添加submodule了

而在本次实际使用中，用的正是这一种。全部过程如下：

```
$ git submodule add git@gitee.com:know_the_season/BaseModule.git BaseModule
'BaseModule' already exists in the index

$ git ls-files --stage BaseModule
100644 665c3ff39bfda9079f66e0a7e464658b4ea2bb4d 0       BaseModule/.gitignore
100644 e69de29bb2d1d6434b8b29ae775ad8c2e48c5391 0       BaseModule/README.md
100644 7c3643606304fde2f3c270b8c000215f11c5dd39 0       BaseModule/build.gradle
100644 e69de29bb2d1d6434b8b29ae775ad8c2e48c5391 0       BaseModule/consumer-rules.pro
100644 c45d7a667bd3ee39769ec0d000a3b723c1663e5d 0       BaseModule/proguard-rules.pro

$ git rm --cached BaseModule
#在提示信息中，就已经提示没有-r不会递归删除'BaseModule'
fatal: not removing 'BaseModule' recursively without -r

$ git rm -r --cached BaseModule
rm 'BaseModule/.gitignore'
rm 'BaseModule/README.md'
rm 'BaseModule/build.gradle'
rm 'BaseModule/consumer-rules.pro'
rm 'BaseModule/proguard-rules.pro'

$ git submodule add git@gitee.com:know_the_season/BaseModule.git BaseModule
```

## 切换submodule分支

需要注意的是，项目添加submodule后，在git命令行中，进入项目非submodule目录，显示的是项目所在的分支，进入submodule后，显示的是submodule所在分支。两者各自切换、创建、删除分支，都不会互相影响

添加submodule到项目后，默认添加的是master分支，如果想要切换到其他分支，分为两种方式，分别是submodule对应仓库中是否有想要切换的分支，在这里分别对两种情况予以说明：

场景：切换项目中submodule的分支到android_module分支

1. 假如仓库中已经有android_module分支。首先需要先进入对应文件夹（submodule的根目录，而不是项目的根目录）

   ```
   # 进入BaseModule目录（已经添加到项目中）
   $ cd BaseModule
   
   $ git branch -a
   * master
     remotes/origin/HEAD -> origin/master
     remotes/origin/android_module
     remotes/origin/master
     
   $ git checkout -b android_module origin/android_module
   $ git add .
   $ git commit -m "checkout submodule branch to android_module from master"
   $ git push origin/master
   ```

2. 加入android_module分支是新建，那么新建分支后直接推送到仓库，与原来平常修改后推送到仓库中的操作方式没有区别

   ```
   $ git checkout -b android_module
   $ git push origin/android_module
   ```

注意：在仓库中，是会保存submodule相对于原module所在commit的，所以在submodule中修改分支后，commit然后push，那么仓库中保存的就是所在commit的id。所以在检出项目代码（clone）时，需要分别进入submodule目录中再clone的原因

## 删除submodule

场景：在项目中删除BaseModule

```
#首先是在命令行中直接进入项目根目录
git rm BaseModule/
git add .
git commit -m "remove BaseModule"
git push
```

首先是通过`git rm`命令删除BaseModule，这时会删除BaseModule文件夹和修改`.gitmodules`文件，即会删除对应submodule相关信息，比如

```
[submodule "BaseModule"]
	path = BaseModule
	url = git@gitee.com:know_the_season/BaseModule.git
```

这一段关于BaseModule的信息会被删除，然后保存修改，并push到仓库，就完成了本地仓库到远程仓库的修改

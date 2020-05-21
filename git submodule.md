## clone 

从仓库中clone带有library module的项目时，并不会把library module的内容也一并clone下来，但是会clone出.gitsubmodule这个描述文件。如果还需要检出submodule，那么还需要两条命令：

```
git@gitee.com:know_the_season/AppModule.git
# 进入AppModule
cd AppModule
git submodule init
git submodule update
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
git submodule add git@gitee.com:know_the_season/BaseModule.git src/SubModule
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




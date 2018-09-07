创建标签

git tag v1.0.0 #创建了标签名为v1.0.0的标签

git tag #展示所有的标签

git show v0.1.2  #查看标签名为0.1.2的信息，可获取这个标签所在的commit的信息

git tag -d v0.1.2 # 删除标签



在指定commit添加标签

git tag -a v0.1.1 9fbc3d0  #在commit校验和为9fbc3d0  的commit添加标签

在已提交的最后一个commit添加标签

git tag v222

删除标签

git tag -d v32



推送标签

通常的git push不会将标签对象提交到git服务器，我们需要进行显式的操作：

git push origin v0.1.2 # 将v0.1.2标签提交到git服务器 

git push origin --tags # 将本地所有标签一次性提交到git服务器




# ssh

查看是否有ssh文件，有则获取ssh公钥



进入ssh文件夹，如果没有密钥则不会有此文件夹，有则备份删除`cd ~/.ssh`

- 如果有文件，那么：

  查看ssh文件夹下的文件
  `ls`
  获取公钥，输入完成按回车，获取到公钥，并添加到`github`的`ssh key`中，在github上添加ssh密钥，这要添加的是“id_rsa.pub”里面的公钥
  `cat id_rsa.pub`

- 如果没有，则生成密钥：
  `ssh-keygen -t rsa -C "zzq787an150@gmail.com"`
  按3个回车，密码为空。
  Your identification has been saved in /home/tekkub/.ssh/id_rsa.
  Your public key has been saved in /home/tekkub/.ssh/id_rsa.pub.
  The key fingerprint is:
  ………………
  最后得到了两个文件：id_rsa和id_rsa.pub，再按照有ssh文件的情况下操作，在github中创建新的ssh key



# github


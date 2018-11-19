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

## 多账号使用

参考资料：[多github帐号的SSH key切换](https://www.cnblogs.com/BeginMan/p/3548139.html)

一般来说，我们都会有好几个`Git`账号，比如公司的git账号，个人的`github`账号，我甚至还会去用`码云`的`git`服务。如果都是同一个账号那还好说，如果不是，那就显然麻烦些。

使用git服务，就需要创建相对应的`ssh key`（大多数 Git 服务器都会选择使用`SSH公钥`来进行授权。系统中的每个用户都必须提供一个`公钥`用于`授权`，没有的话就要生成一个）。[git ssh](https://git-scm.com/book/zh/v1/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E7%94%9F%E6%88%90-SSH-%E5%85%AC%E9%92%A5)作用可自行查找资料

在此前已经记录了如何创建`ssh`并添加到`github`中，那么如何继续创建针对于码云的`ssh`而又不会和`github`的`ssh`相混淆甚至于相互影响呢？在这里用的是不同的名字并创建配置文件说明。做法如下：

1. 创建`github`账号的`ssh key`

   ```
   # 切换到C:\Users\Administrator\.ssh
   cd ~/.ssh     
   # 新建工作的SSH key
   ssh-keygen -t rsa -C "mywork@email.com"  
   # 设置名称为id_rsa_work
   Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa): id_rsa_work
   ```

2. 将这个密钥添加到`ssh agent`中，注意：在第一步已经将创建的`ssh key`名称设置为`id_rsa`

   ```
   ssh-add ~/.ssh/id_rsa_work
   ```

   如果出现`Could not open a connection to your authentication agent`的错误，就试着用以下命令：

   ```
   ssh-agent bash
   ssh-add ~/.ssh/id_rsa_work
   ```

3. 创建码云账号的`ssh key`，步骤同上，不过名字不能相同，可设置为`id_rsa_mayun`

4. 将创建的码云的密钥也添加到`ssh agent`中，注意命令后面的名字

   ```
   ssh-add ~/.ssh/id_rsa_mayun
   ```

5. 将创建好的两个`ssh`文件分别添加到`github`和`码云`上，这很关键

6. 创建或修改`config`文件

   1. 如果.ssh文件夹没有`config`文件，可通过命令行创建

      ```
      touch config
      ```

      然后在文件夹中选择`config`**右键**点击文件，并选择编辑文件，添加如下内容：

      ```
      # github
      Host github.com
          HostName github.com
          User git
          IdentityFile ~/.ssh/id_rsa
      # gitee
      Host gitee.com
          HostName gitee.com
          User git
          IdentityFile ~/.ssh/id_rsa_mayun
      ```

      注意：`github`的`host`地址是`github.com`，`码云`的`host`的地址是：`gitee.com`。保存完后去测试是否成功

      ```
      ssh -T git@gitee.com
      ssh -T git@github.com
      ```

      如果成功后会有提示`“Hi xxx, You've successfully authenticated”`的提示

7. 失败可能

   - 可能没有**分别**添加`ssh key`到`github`及`git`服务，如`码云`
   - rsa文件名可能错误，如相同

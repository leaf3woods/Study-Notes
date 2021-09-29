# Git和TortoiseGit配置过程

##### SSh keys

ssh keys是远程ssh连接的一种基于秘钥方式安全连接起来的秘钥文件。ssh keys是ssh中基于秘钥的安全验证，你可以通过创建公钥和私钥对的方式来完成ssh keys方式的ssh登录验证。首先需要执行的操作是必须在本地开发机上创建一对秘钥，并把公钥的内容放在需要访问的服务器上。如果你连接到ssh服务器上，客户端软件就会向服务器发出请求，请求用你的秘钥进行安全验证。   服务器收到请求后，先在该服务器上的主目录下寻找你的公用秘钥，然后把它和你发送过来的公用秘钥进行比较。如果两个秘钥一致，服务器就用公用秘钥加密“质询”并把它发送给客户端软件。客户端软件收到质询之后就可以用你的私有秘钥解密再把它发送给服务器。基于ssh keys的登录验证方式可以避免假冒服务器的问题，因为假冒问题获取不到你的秘钥。它比基于用户密码的口令方式更加安全。

<!--git生成的公钥和TortoiseGit生成的公钥是不一致的。-->

##### Git bash公钥

###### 确认是否已经拥有Git秘钥

```
$ cd ~/.ssh
$ ls
Authorized_keys2   id_dsa  known_hosts
Config     id_rsa_pub
```

###### 公钥生成

```
$ ssh-keygen
```

可选

```
-t rsa -C "2695923549@qq.com"
```

##### TortoiseGit公钥

[^PuTTYgen]: TortoiseGit安装后自带，默认目录在`C:\Program Files\TortoiseGit\bin `1

<!--进入网站粘贴公钥标记当前设备-->

##### 拉取远程仓库代码

使用Gitbash 

``` 
$ git clone 网址（ssh）.git 预备拷贝的目录
```


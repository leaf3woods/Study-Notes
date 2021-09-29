# Linux常用指令

​            

### 关机&重启命令

```shell
shutdown    –h    now   ：表示立即关机  
shutdown    –h    1     ：表示一分钟后关机 
shutdown    –r    now   ：立即重启

halt        ：直接使用，等价于关机
reboot      ：重启系统                
sync        ：把内存中的数据同步到磁盘上，关机前执行一下，防止数据丢失
```

### 用户相关

Linux系统是一个多用户多任务的操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。

**添加用户：**

```shell
useradd  [用户名]  当创建用户成功后，会自动的创建和用户同名的家目录
useradd -d [指定目录] [新的用户名]       给新创建的用户指定家目录
```

**指定/修改密码:**

```shell
passwd    [用户名]
```

**删除用户：**(删除用户时建议保留家目录)

```shell
userdel   [用户名]
```

**查询用户信息:** (当用户不存在时，返回无此用户)

```shell
id  [用户名]
```

样例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210213225500488.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**注销用户：**

```shell
logout 注销指令在图形运行级别无效,在运行级别3一下有效
```

**切换用户：**

登录时尽量少用root帐号登录，因为它是系统管理员，最大的权限，避免操作失误。可以利用普通用户登录，登录后再用`su - 用户名`命令来切换成系统管理员身份。 从权限高的用户切换到权限低的用户，不需要输入密码，反之需要。

```shell
su – [切换用户名]
```

**返回到原来用户：**

```shell
exit
```

**查看当前登录用户：**

```shell
whoami
```

样例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210213230724542.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

### 用户组

用户组类似于角色，系统可以对有共性的多个用户进行统一的管理。

**新增组：**

```shell
groupadd [组名]
```

**删除组：**

```shell
groupdel [组名]
```

**增加用户时直接加上组：**

```shell
useradd  –g [用户组] [用户名]
```

**修改用户的组：**

```shell
usermod  –g [用户组] [用户名]
```

案例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021323254041.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

### 组管理和权限管理

在linux中的每个用户必须属于一个组，不能独立于组外。在linux中每个文件有所有者、所在组、其它组的概念。

**文件/目录 所有者：**

一般为文件的创建者，谁创建了该文件，就自然的成为该文件的所有者。

**查看文件的所有者：** 创建一个组police（`groupadd police`），再创建一个用户tom(`useradd -g police tom`)，将tom放入police组，登录用户tom并创建一个文件。

```shell
ls –ahl
```

样例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220210829201.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**修改文件所有者：**

```shell
chown [用户名] [文件名]
```

样例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220211852421.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**文件/目录 所在组：**

当某个用户创建了一个文件后，这个文件的所在组就是该用户所在的组。

**修改文件所在的组：**

```shell
chgrp [组名] [文件名]
chgrp -R [组名] [目录名] 递归修改目录下所有文件组名
```

案例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220212958247.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**其它组：** 除文件的所有者和所在组的用户外，系统的其它用户都是文件的其它组。

**改变用户所在组：**
 在添加用户时，可以指定将该用户添加到哪个组中，同样的用root的管理权限可以改变某个用户所在的组。

```shell
usermod   –g   [组名] [用户名]
usermod   –d   [目录名] [用户名] 改变该用户登陆的初始目录。
```

样例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220213605927.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)



**用户和组的相关文件：**

/etc/passwd 文件：用户（user）的配置文件，记录用户的各种信息

每行的含义：**用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell**

/etc/shadow 文件：口令的配置文件

每行的含义：**登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志**

/etc/group 文件：组(group)的配置文件，记录Linux包含的组的信息

每行含义：**组名:口令:组标识号:组内用户列表**

### 指定运行级别

**六个运行级别：**

- 0 ：关机
- 1 ：单用户【找回丢失密码】
- 2：多用户状态没有网络服务
- **3：多用户状态有网络服务**
- 4：系统未使用保留给用户
- **5：图形界面**
- 6：系统重启

修改默认的运行级别可改文件：`/etc/inittab的id:5:initdefault`这一行中的数字
 命令：`init [012356]`
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210214121455387.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

### 帮助指令

当我们对某个指令不熟悉时，我们可以使用linux提供的帮助指令来了解这个指令的使用方法

**man 获得帮助信息：**

```shell
man [命令或配置文件]   获得帮助信息
```

**help指令：**

```shell
help [命令]  获得shell内置命令的帮助信息
```

### 文件目录类

**pwd 指令:**

```shell
pwd 显示当前工作目录的绝对路径
```

**ls指令:**

```shell
ls [选项] [目录或是文件]
```

-a ：显示当前目录所有的文件和目录，包括隐藏的
 -l ：以列表的方式显示信息

**cd 指令：**

```shell
cd  [参数] 切换到指定目录
```

**绝对路径和相对路径：**

```shell
cd ~ 回到自己的家目录
cd ：回到自己的家目录
cd .. 回到当前目录的上一级目录
```

案例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210214133213754.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**mkdir指令：**

mkdir指令用于创建目录

```shell
mkdir  [选项]  要创建的目录
-p ：创建多级目录
```

**rmdir指令：**
 rmdir指令删除空目录

```shell
rmdir  [选项]  要删除的空目录
```

rmdir 删除的是空目录，如果目录下有内容时无法删除的。如果需要删除非空目录，需要使用 `rm -rf 要删除的目录`

样例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210214135003873.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**touch指令:**
 touch指令创建空文件

```shell
touch [文件名称]
touch [文件名称1] [文件名称2] 
```

**cp指令：**
 cp 指令拷贝文件到指定目录

```shell
cp [选项] source dest
```

`-r ：递归复制整个文件夹`
\cp 强制覆盖不提示的方法：`


 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210214141503208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)



**rm指令：**
 rm 指令移除文件或目录

```shell
rm  [选项]  [要删除的文件或目录]
    [-rf]   递归删除整个文件夹
    [-f]    强制删除不提示
```

**mv指令：**
 mv 移动文件与目录或重命名

```shell
mv  oldNameFile newNameFile     (功能描述：重命名)
mv /temp/movefile /targetFolder (功能描述：移动文件)
12
```

样例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210214233606512.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**cat指令：**
 cat 查看文件内容，cat 只能浏览文件，而不能修改文件，为了浏览方便，一般会带上管道命令 | more

```shell
cat  [参数] 要查看的文件
     [-n ] 显示行号
```

样例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210214234142455.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**more指令：**
 more指令是一个基于VI编辑器的文本过滤器，它以全屏幕的方式按页显示文本文件的内容。

```shell
more [要查看的文件]
```

more指令中内置了若干快捷键：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210214234245946.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**less指令：**
 less指令用来分屏查看文件内容，它的功能与more指令类似，但是比more指令更加强大，支持各种显示终端。less指令在显示文件内容时，并不是一次将整个文件加载之后才显示，而是根据显示需要加载内容，对于显示大型文件具有较高的效率。

```shell
less [要查看的文件]
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210214234947348.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**echo指令：**
 echo输出内容到控制台

```shell
echo  [选项]  [输出内容]
```

**head指令**
 head用于显示文件的开头部分内容，默认情况下head指令显示文件的前10行内容

```shell
head [文件]
head -n 5 [文件]  查看文件头5行内容，5可以是任意行数)
```

**tail指令：**
 tail用于输出文件中尾部的内容，默认情况下tail指令显示文件的前10行内容。

```shell
tail  文件
tail  -n 5 文件 （功能描述：查看文件头5行内容，5可以是任意行数）
tail  -f  文件 （功能描述：实时追踪该文档的所有更新）
```

案例1：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215003501372.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

案例2: 实时监控 mydate.txt , 看看到文件有变化时，是否看到， 实时的追加日期
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215004554820.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**> 指令 和 >> 指令：**

```
> 输出重定向：将原来的文件内容覆盖`
>> 追加：不会覆盖原来的文件内容
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215001519121.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**ln 指令：**
 软链接也成为符号链接，类似于windows里的快捷方式，主要存放了链接其他文件的路径

```shell
ln -s [原文件或目录] [软链接名] （功能描述：给原文件创建一个软链接）
```

样例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215195759539.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

细节说明：当我们使用pwd指令查看目录时，仍然看到的是软链接所在目录。

**history指令：**
 查看已经执行过历史命令,也可以执行历史指令

```shell
history （功能描述：查看已经执行过历史命令）
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215200626974.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

### 时间日期类

**date指令-显示当前日期：**

```shell
date （功能描述：显示当前时间）
date +%Y（功能描述：显示当前年份）
date +%m（功能描述：显示当前月份）
date +%d（功能描述：显示当前是哪一天）
date "+%Y-%m-%d %H:%M:%S"（功能描述：显示年月日时分秒）
```

**date指令-设置日期：**

```shell
date  -s  字符串时间
```

**cal指令：**
 查看日历指令

```shell
cal [选项] （功能描述：不加选项，显示本月日历）
```

案例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2021021520300540.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

### 搜索查找类

**find指令：**
 find指令将从指定目录向下递归地遍历其各个子目录，将满足条件的文件或者目录显示在终
 端。

```shell
find  [搜索范围]  [选项]
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215204740213.png)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215205931496.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**locate指令：**
 locaate指令可以快速定位文件路径。locate指令利用事先建立的系统中所有文件名称及路径的locate数据库实现快速定位给定的文件。Locate指令无需遍历整个文件系统，查询速度较快。为了保证查询结果的准确度，管理员必须定期更新locate时刻。

```shell
locate 搜索文件
```

由于locate指令基于数据库进行查询，所以第一次运行前，必须使用updatedb指令创建locate数据库。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215232006420.png)

**grep指令和 管道符号 | ：**
 grep 过滤查找 ， 管道符，“|”，表示将前一个命令的处理结果输出传递给后面的命令处理。

```shell
grep [选项] 查找内容 源文件
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215232050680.png)
 案例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215232748119.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

### 压缩和解压类

**gzip/gunzip 指令：**
 gzip 用于压缩文件， gunzip 用于解压的

```shell
gzip [文件]       #功能描述：压缩文件，只能将文件压缩为*.gz文件
gunzip [文件.gz]  #解压缩文件命令
```

**zip/unzip 指令：**
 zip 用于压缩文件， unzip 用于解压的，这个在项目打包发布中很有用的

```shell
zip      [选项] XXX.zip  将要压缩的内容  #压缩文件和目录的命令
unzip    [选项] XXX.zip  解压缩文件
-r：#递归压缩，即压缩目录
-d <目录> #指定解压后文件的存放目录
```

案例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215234421774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**tar 指令：**
 tar 指令 是打包指令，最后打包后的文件是 .tar.gz 的文件。

```shell
tar  [选项]  XXX.tar.gz  打包的内容 #功能描述：打包目录，压缩后的文件格式.tar.gz      
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210215234736411.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)
 案例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210216000418647.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

### 权限管理

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305163003772.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**0-9位说明：**

1. 第0位确定文件类型(`d：目录、 - ：普通文件 、l ：链接文件、 c：字符设备 、 b：块文件`)
2. 第1-3位确定所有者（该文件的所有者）拥有该文件的权限。—所有者权限
3. 第4-6位确定所属组（同用户组的）拥有该文件的权限，—所在组的用户权限
4. 第7-9位确定其他用户拥有该文件的权限 —其他组用户权限

**修改权限-chmod：**
 通过chmod指令，可以修改文件或者目录的权限。

第一种方式：+ 、-、= 变更权限，`u:所有者 g:所有组 o:其他人 a:所有人(u、g、o的总和)`
 第二种方式：通过数字变更权限：用数字表示rwx: `r=4,w=2,x=1，rwx=4+2+1=7`

```shell
chmod   u=rwx,g=rx,o=x   文件目录名
chmod   o+w    文件目录名
chmod   a-x    文件目录名


chmod u=rwx,g=rx,o=x    文件目录名  
||
chmod   751  文件目录名
```

样例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022022582077.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**修改文件所有者-chown：**

```shell
chown  newowner  file  #改变文件的所有者
chown  newowner:newgroup  file  #改变用户的所有者和所有组
-R   #如果是目录 则使其下所有子文件或目录递归生效
```

案例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022023152976.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

### 定时任务调度

任务调度crand：系统在某个时间执行的特定的命令或程序

**任务调度分类：**
 1.系统工作：有些重要的工作必须周而复始地执行。如病毒扫描等
 2.个别用户工作：个别用户可能希望执行某些程序，比如对mysql数据库的备份。

**crontab 进行定时任务的设置：**

```shell
crontab [选项]

crontab –e  #编辑定时任务。
crontab –r	#终止任务调度。
crontab –l	#列出当前有那些任务调度
service crond restart   [重启任务调度]
```

样例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301230622935.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)
 定时调度我们的脚本或这代码，如果只是简单的任务可以不用写脚本，直接再crontab中加入任务即可。

样例：每小时的每分钟执行`ls –l /etc/ >> /tmp/to.txt`命令
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301222116262.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**5个占位符的说明：**
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301221515749.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**特殊符号的说明：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301221551707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

案例：每隔1分钟，就将当前的日期信息，追加到 /tmp/mydate 文件中
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301225521687.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)
 实现文件的定时更新：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301225629368.png)

### Linux磁盘分区与挂载

**查看所有设备挂载情况：**

```shell
lsblk
lsblk -f
```

案例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210301234133742.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**磁盘情况查询：**

```shell
df -h  #查询系统整体磁盘使用情况
du -h  #查询指定目录的磁盘占用情况，默认为当前目录
   -s  #指定目录占用大小汇总
   -h  #带计量单位
   -a  #含文件
   --max-depth=1  #子目录深度
   -c	#列出明细的同时，增加汇总值
```

样例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210302164023278.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**磁盘情况-工作实用指令**

**1.** 统计/home文件夹下文件的个数
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210302164943789.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**2.** 统计/home文件夹下目录的个数
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210302165126715.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**3.** 统计/home文件夹下文件的个数，包括子文件夹里的
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210302165310909.png)

**4.** 以树状显示目录结构
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303210003564.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

### 网络配置

Linux网络配置原理图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303214233936.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**查看网络IP和网关：**

查看虚拟网络编辑器
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303212503285.png)
 查看ip地址、网关
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303214614419.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

查看windows环境的中VMnet8网络配置 (ipconfig指令)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303212944631.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

ping 测试主机之间网络连通性

```shell
ping 目的主机	#测试当前服务器是否可以连接目的主机
```

应用实例

测试当前服务器是否可以连接百度
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303213855257.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

### linux网络环境配置

第一种方法(自动获取)：

说明：登陆后，通过界面的来设置自动获取ip
 特点：linux启动后会自动获取IP,缺点是每次自动获取的ip地址可能不一样。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303215054943.png)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303215121511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303215211195.png)

第二种方法(指定固定的ip)

直接修改配置文件来指定IP,并可以连接到外网(程序员推荐)，编辑

```
vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

要求：将ip地址配置的静态的， IP的配置方法[none|static|bootp|dhcp]（引导时不使用协议|静态分配IP|BOOTP协议|DHCP协议），ip地址为192.168.42.131。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303222224237.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)
 重启网络服务或者重启系统生效：service network restart 、reboot

### 进程管理

**显示系统执行的进程：**

ps命令是用来查看目前系统中，有哪些正在执行，以及它们执行的状况。可以不加任何参数.

```shell
ps –aux|grep xxx	#比如我看看有没有sshd服务
```

案例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303225241780.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

```shell
ps -ef	#是以全格式显示当前所有的进程
   -e   #显示所有进程
   -f   #全格式
ps -ef|grep xxx
```

案例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210303230611945.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**终止进程kill和killall：**

若是某个进程执行一半需要停止时，或是已消了很大的系统资源时，此时可以考虑停止该进程。使用kill命令来完成此项任务。

```shell
killall 进程名称   #通过进程名称杀死进程，也支持通配符，这在系统因负载过大而变得很慢时很有用）
kill  [选项] 进程号 #通过进程号杀死进程
        -9	#表示强迫进程立即停止
```

案例1：踢掉某个非法登录用户
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304103754708.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

案例2: 终止远程登录服务sshd, 在适当时候再次重启sshd服务
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210309101938696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

案例3: 终止多个gedit 编辑器
 `killall gedit`

案例4：强制杀掉一个终端
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304105223998.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**查看进程树pstree**

```shell
pstree [选项]	#可以更加直观的来看进程信息
 		-p 	 #显示进程的PID
 		-u   #显示进程的所属用户
```

案例：树状的形式显示进程的pid
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304105557380.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**服务(service)管理**

服务(service) 本质就是进程，但是是运行在后台的，通常都会监听某个端口，等待其它程序的请求，比如(mysql , sshd 防火墙等)，因此我们又称为守护进程，是Linux中非常重要的知识点。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304111612887.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

service管理指令：在CentOS7.0后 不再使用service ,而是 systemctl

```shell
service  服务名 [start | stop | restart | reload | status]
```

使用案例：重启防火墙，查看当前防火墙的状况。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030411230967.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)
 关闭防火墙：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304112501758.png)

关闭或者启用防火墙后，立即生效，`[telnet 测试 某个端口即可]`。 这种方式只是临时生效，当重启系统后，还是回归以前对服务的设置。 如果希望设置某个服务自启动或关闭永久生效，要使用chkconfig指令。

**查看服务名:**

方式1：使用`setup` -> 系统服务 就可以看到。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304113141201.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)
 方式2: /etc/init.d/服务名称
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304113341445.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**服务的运行级别(runlevel):**

查看或者修改默认级别： `vi /etc/inittab`
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304113427519.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

开机的流程说明：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304113545606.png)

**chkconfig指令：**
 通过chkconfig 命令可以给每个服务的各个运行级别设置自启动/关闭

```shell
chkconfig   --list|grep  xxx  #查看服务
chkconfig   服务名 --list 
chkconfig   --level  5   服务名 on/off   
```

案例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304115649840.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

案例：将sshd 服务在运行级别5下设置为不自动启动
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304115917849.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)
 案例： 在所有运行级别下，关闭和开启防火墙，chkconfig重新设置服务后自启动或关闭，需要重启机器reboot才能生效.
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304120652162.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

**动态监控进程：**

top与ps命令很相似。它们都用来显示正在执行的进程。Top与ps最大的不同之处，在于top在执行一段时间可以更新正在运行的的进程。

```shell
top [选项]
```

选项说明：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304120758582.png)

交互操作说明：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2021030412105870.png)

案例1.监视特定用户
 `top：输入此命令，按回车键，查看执行的进程。`
 `u：然后输入“u”回车，再输入用户名，即可`
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304122242952.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

案例2：终止指定的进程。
 `top：输入此命令，按回车键，查看执行的进程。`
 `k：然后输入“k”回车，再输入要结束的进程ID号`
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304122547684.png)

**监控网络状态：**

查看系统网络情况netstat

```shell
netstat [选项]
         -an  #按一定顺序排列输出
          -p  #显示哪个进程在调用
```

案例：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304123125605.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

案例：请查看服务名为 sshd 的服务的信息。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304130538259.png)

**检测主机连接命令ping：** 是一种网络检测检测工具，它主要是用检测远程主机是否正常，或是两部主机间的介质是否为断、网线是否脱落或网卡故障。

```shell
ping 对方ip地址
```

### RPM 与 YUM

**rpm包的管理：**

一种用于互联网下载包的打包及安装工具，它包含在某些Linux分发版中。它生成具有.RPM扩展名的文件。RPM是RedHat Package  Manager（RedHat软件包管理工具）的缩写，类似windows的setup.exe，这一文件格式名称虽然打上了RedHat的标志，但理念是通用的。

Linux的分发版本都有采用（suse,redhat, centos 等等），可以算是公认的行业标准了。

**rpm包的简单查询指令：**

```shell
rpm  –qa|grep xx
```

案例：查询系统中是否安装火狐，X86_64表示64位系统，i686、i386表示32位系统，noarch表示通用。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304164626100.png)
 rpm包的其它查询指令：

```shell
rpm -qa :查询所安装的所有rpm软件包
rpm -qa | more    
rpm -qa | grep X [rpm -qa | grep firefox ]

rpm -q 软件包名  #查询软件包是否安装
				rpm -q firefox

rpm -qi 软件包名 #查询软件包信息
				rpm -qi file

rpm -ql 软件包名 #查询软件包中的文件
                rpm -ql firefox

rpm -qf 文件全路径名 查询文件所属的软件包
rpm -qf /etc/passwd
rpm -qf /root/install.log
```

**rpm包的管理：**

卸载rpm包：

```shell
rpm -e RPM包的名称
rpm -e --nodeps RPM包的名称  #强制删除 
```

如果其它软件包依赖于您要卸载的软件包，卸载时则会产生错误信息。如： `$ rpm -e foo removing these packages would break dependencies:foo is needed by bar-1.0-1`， 如果我们就是要删除 foo这个rpm 包，可以增加参数 --nodeps ,就可以强制删除，但是一般不推荐这样做，因为依赖于该软件包的程序可能无法运行，如：`$ rpm -e --nodeps foo`

安装rpm包：

```shell
rpm -ivh  RPM包全路径名称
     -i   #=install 安装
     -v	  #=verbose 提示
 	 -h   #=hash  进度条
```

案例：安装firefox浏览器
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210304171532766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)

### yum

Yum 是一个Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包。

**yum的基本指令：**
 查询yum服务器是否有需要安装的软件

```shell
yum list|grep xx软件列表
```

安装指定的yum包

```shell
yum install xxx  下载安装
```

案例：请使用yum的方式来安装firefox
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210305113348472.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x6eTQxMDk5Mg==,size_16,color_FFFFFF,t_70)



------



#### ssh 基础（重点）

在 Linux 中 SSH 是 非常常用 的工具，通过 SSH 客户端 我们可以连接到运行了 SSH 服务器 的远程机器上
Alt
ssh的简单使用：

```shell
ssh [-p port] user@remote
user   #在远程机器上的用户名，如果不指定的话默认为当前用户
remote #远程机器的地址，可以是 IP／域名，或者是 后面会提到的别名
port   #SSH Server 监听的端口，如果不指定，就为默认值 22
```

提示：

    使用 exit 退出当前用户的登录
    ssh 这个终端命令只能在 Linux 或者 UNIX 系统下使用
    如果在 Windows 系统中，可以安装 PuTTY 或者 XShell 客户端软件即可
    在工作中，SSH 服务器的端口号很有可能不是 22，如果遇到这种情况就需要使用 -p 选项，指定正确的端口号，否则无法正常连接到服务器

#### scp

scp 就是 secure copy ，是一个在 Linux 下用来进行 远程拷贝文件 的命令
它的地址格式与 ssh 基本相同，需要注意的是，在指定端口时用的是大写的 -P 而不是小写的

把本地当前目录下的 01.py 文件 复制到 远程 家目录下的 Desktop/01.py

注意：`:` 后面的路径如果不是绝对路径，则以用户的家目录作为参照路径

```shell
scp -P port 01.py user@remote:Desktop/01.py
    #把远程 家目录下的 Desktop/01.py 文件 复制到 本地当前目录下的 01.py
scp -P port user@remote:Desktop/01.py 01.py
    #把当前目录下的 demo 文件夹 复制到 远程 家目录下的 Desktop
    #加上 -r 选项可以传送文件夹
scp -r demo user@remote:Desktop
    #把远程 家目录下的 Desktop 复制到 当前目录下的 demo 文件夹
```

```shell
scp  -r user@remote:Desktop demo
     -r	 #若给出的源文件是目录文件，则 scp 将递归复制该目录下的所有子目录和文件，目标文件必须为一个		  目录名
	 -P	 #若远程 SSH 服务器的端口不是 22，需要使用大写字母 -P 选项指定端口
```

注意：
scp 这个终端命令只能在 Linux 或者 UNIX 系统下使用
如果在 Windows 系统中，可以安装 PuTTY ，使用 pscp 命令行工具或者安装 FileZilla使用 FTP 进行文件传输



------

**系统信息** 

```shell
arch #显示机器的处理器架构
uname -m #显示机器的处理器架构
uname -r #显示正在使用的内核版本 
dmidecode -q #显示硬件系统部件 - (SMBIOS / DMI) 
hdparm -i /dev/hda #罗列一个磁盘的架构特性 
hdparm -tT /dev/sda #在磁盘上执行测试性读取操作 
cat /proc/cpuinfo #显示CPU info的信息 
cat /proc/interrupts #显示中断 
cat /proc/meminfo #校验内存使用 
cat /proc/swaps #显示哪些swap被使用 
cat /proc/version #显示内核的版本 
cat /proc/net/dev #显示网络适配器及统计 
cat /proc/mounts #显示已加载的文件系统 
lspci -tv #罗列 PCI 设备 
lsusb -tv #显示 USB 设备 
date #显示系统日期 
cal 2007 #显示2007年的日历表 
date 041217002007.00 #设置日期和时间 - 月日时分年.秒 
clock -w #将时间修改保存到 BIOS 
```



**关机 (系统的关机、重启以及登出 )** 

```
shutdown -h now 关闭系统
init 0 关闭系统
telinit 0 关闭系统
shutdown -h hours:minutes & 按预定时间关闭系统 
shutdown -c 取消按预定时间关闭系统 
shutdown -r now 重启
reboot 重启
logout 注销 
```



**文件和目录** 

```
cd /home 进入 '/ home' 目录' 
cd .. 返回上一级目录 
cd ../.. 返回上两级目录 
cd 进入个人的主目录 
cd ~user1 进入个人的主目录 
cd - 返回上次所在的目录 
pwd 显示工作路径 
ls 查看目录中的文件 
ls -F 查看目录中的文件 
ls -l 显示文件和目录的详细资料 
ls -a 显示隐藏文件 
ls *[0-9]* 显示包含数字的文件名和目录名 
tree 显示文件和目录由根目录开始的树形结构
lstree 显示文件和目录由根目录开始的树形结构
mkdir dir1 创建一个叫做 'dir1' 的目录' 
mkdir dir1 dir2 同时创建两个目录 
mkdir -p /tmp/dir1/dir2 创建一个目录树 
rm -f file1 删除一个叫做 'file1' 的文件' 
rmdir dir1 删除一个叫做 'dir1' 的目录' 
rm -rf dir1 删除一个叫做 'dir1' 的目录并同时删除其内容 
rm -rf dir1 dir2 同时删除两个目录及它们的内容 
mv dir1 new_dir 重命名/移动 一个目录 
cp file1 file2 复制一个文件 
cp dir/* . 复制一个目录下的所有文件到当前工作目录 
cp -a /tmp/dir1 . 复制一个目录到当前工作目录 
cp -a dir1 dir2 复制一个目录 

cp -r dir1 dir2 复制一个目录及子目录
ln -s file1 lnk1 创建一个指向文件或目录的软链接 
ln file1 lnk1 创建一个指向文件或目录的物理链接 
touch -t 0712250000 file1 修改一个文件或目录的时间戳 - (YYMMDDhhmm) 
file file1 outputs the mime type of the file as text 
iconv -l 列出已知的编码 
iconv -f fromEncoding -t toEncoding inputFile > outputFile creates a new from the given input file by assuming it is encoded in fromEncoding and converting it to toEncoding. 
find . -maxdepth 1 -name *.jpg  -print -exec convert "{}" -resize 80x60 "thumbs/{}" \; batch resize  files in the current directory and send them to a thumbnails directory  (requires convert from Imagemagick) 
```



**文件搜索** 

```
find / -name file1 从 '/' 开始进入根文件系统搜索文件和目录 
find / -user user1 搜索属于用户 'user1' 的文件和目录 
find /home/user1 -name \*.bin 在目录 '/ home/user1' 中搜索带有'.bin' 结尾的文件 
find /usr/bin -type f -atime +100 搜索在过去100天内未被使用过的执行文件 
find /usr/bin -type f -mtime -10 搜索在10天内被创建或者修改过的文件 
find / -name \*.rpm -exec chmod 755 '{}' \; 搜索以 '.rpm' 结尾的文件并定义其权限 
find / -xdev -name \*.rpm 搜索以 '.rpm' 结尾的文件，忽略光驱、捷盘等可移动设备 
locate \*.ps 寻找以 '.ps' 结尾的文件 - 先运行 'updatedb' 命令 
whereis halt 显示一个二进制文件、源码或man的位置 
which halt 显示一个二进制文件或可执行文件的完整路径 
```



**挂载一个文件系统** 

```
mount /dev/hda2 /mnt/hda2 挂载一个叫做hda2的盘 - 确定目录 '/ mnt/hda2' 已经存在 
umount /dev/hda2 卸载一个叫做hda2的盘 - 先从挂载点 '/ mnt/hda2' 退出 
fuser -km /mnt/hda2 当设备繁忙时强制卸载 
umount -n /mnt/hda2 运行卸载操作而不写入 /etc/mtab 文件- 当文件为只读或当磁盘写满时非常有用 
mount /dev/fd0 /mnt/floppy 挂载一个软盘 
mount /dev/cdrom /mnt/cdrom 挂载一个cdrom或dvdrom 
mount /dev/hdc /mnt/cdrecorder 挂载一个cdrw或dvdrom 
mount /dev/hdb /mnt/cdrecorder 挂载一个cdrw或dvdrom 
mount -o loop file.iso /mnt/cdrom 挂载一个文件或ISO镜像文件 
mount -t vfat /dev/hda5 /mnt/hda5 挂载一个Windows FAT32文件系统 
mount /dev/sda1 /mnt/usbdisk 挂载一个usb 捷盘或闪存设备 
mount -t smbfs -o username=user,password=pass //WinClient/share /mnt/share 挂载一个			windows网络共享 
```



**磁盘空间** 

```
df -h 显示已经挂载的分区列表 
ls -lSr |more 以尺寸大小排列文件和目录 
du -sh dir1 估算目录 'dir1' 已经使用的磁盘空间' 
du -sk * | sort -rn 以容量大小为依据依次显示文件和目录的大小 
rpm -q -a --qf '%10{SIZE}t%{NAME}n' | sort -k1,1n 以大小为依据依次显示已安装的rpm包所使用的		空间 (fedora, redhat类系统) 
dpkg-query -W -f='${Installed-Size;10}t${Package}n' | sort -k1,1n 以大小为依据显示已安装		的deb包所使用的空间 (ubuntu, debian类系统) 
```



**用户和群组** 

```
groupadd group_name 创建一个新用户组 
groupdel group_name 删除一个用户组 
groupmod -n new_group_name old_group_name 重命名一个用户组 
useradd -c "Name Surname " -g admin -d /home/user1 -s /bin/bash user1 创建一个属于   	"admin" 用户组的用户 
useradd user1 创建一个新用户 
userdel -r user1 删除一个用户 ( '-r' 排除主目录) 
usermod -c "User FTP" -g system -d /ftp/user1 -s /bin/nologin user1 修改用户属性 
passwd 修改口令 
passwd user1 修改一个用户的口令 (只允许root执行) 
chage -E 2005-12-31 user1 设置用户口令的失效期限 
pwck 检查 '/etc/passwd' 的文件格式和语法修正以及存在的用户 
grpck 检查 '/etc/passwd' 的文件格式和语法修正以及存在的群组 
newgrp group_name 登陆进一个新的群组以改变新创建文件的预设群组 
```



**文件的权限 - 使用 "+" 设置权限，使用 "-" 用于取消** 

```
ls -lh 显示权限 
ls /tmp | pr -T5 -W$COLUMNS 将终端划分成5栏显示 
chmod ugo+rwx directory1 设置目录的所有人(u)、群组(g)以及其他人(o)以读（r ）、写(w)和执行(x)	的权限 
chmod go-rwx directory1 删除群组(g)与其他人(o)对目录的读写执行权限 
chown user1 file1 改变一个文件的所有人属性 
chown -R user1 directory1 改变一个目录的所有人属性并同时改变改目录下所有文件的属性 
chgrp group1 file1 改变文件的群组 
chown user1:group1 file1 改变一个文件的所有人和群组属性 
find / -perm -u+s 罗列一个系统中所有使用了SUID控制的文件 
chmod u+s /bin/file1 设置一个二进制文件的 SUID 位 - 运行该文件的用户也被赋予和所有者同样的权限 
chmod u-s /bin/file1 禁用一个二进制文件的 SUID位 
chmod g+s /home/public 设置一个目录的SGID 位 - 类似SUID ，不过这是针对目录的 
chmod g-s /home/public 禁用一个目录的 SGID 位 
chmod o+t /home/public 设置一个文件的 STIKY 位 - 只允许合法所有人删除文件 
chmod o-t /home/public 禁用一个目录的 STIKY 位 
```



**文件的特殊属性 - 使用 "+" 设置权限，使用 "-" 用于取消** 

```
chattr +a file1 只允许以追加方式读写文件 
chattr +c file1 允许这个文件能被内核自动压缩/解压 
chattr +d file1 在进行文件系统备份时，dump程序将忽略这个文件 
chattr +i file1 设置成不可变的文件，不能被删除、修改、重命名或者链接 
chattr +s file1 允许一个文件被安全地删除 
chattr +S file1 一旦应用程序对这个文件执行了写操作，使系统立刻把修改的结果写到磁盘 
chattr +u file1 若文件被删除，系统会允许你在以后恢复这个被删除的文件 
lsattr 显示特殊的属性 
```



**打包和压缩文件** 

```
bunzip2 file1.bz2 解压一个叫做 'file1.bz2'的文件 
bzip2 file1 压缩一个叫做 'file1' 的文件 
gunzip file1.gz 解压一个叫做 'file1.gz'的文件 
gzip file1 压缩一个叫做 'file1'的文件 
gzip -9 file1 最大程度压缩 
rar a file1.rar test_file 创建一个叫做 'file1.rar' 的包 
rar a file1.rar file1 file2 dir1 同时压缩 'file1', 'file2' 以及目录 'dir1' 
rar x file1.rar 解压rar包 
unrar x file1.rar 解压rar包 
tar -cvf archive.tar file1 创建一个非压缩的 tarball 
tar -cvf archive.tar file1 file2 dir1 创建一个包含了 'file1', 'file2' 以及 'dir1'的档案件 
tar -tf archive.tar 显示一个包中的内容 
tar -xvf archive.tar 释放一个包 
tar -xvf archive.tar -C /tmp 将压缩包释放到 /tmp目录下 
tar -cvfj archive.tar.bz2 dir1 创建一个bzip2格式的压缩包 
tar -jxvf archive.tar.bz2 解压一个bzip2格式的压缩包 
tar -cvfz archive.tar.gz dir1 创建一个gzip格式的压缩包 
tar -zxvf archive.tar.gz 解压一个gzip格式的压缩包 
zip file1.zip file1 创建一个zip格式的压缩包 
zip -r file1.zip file1 file2 dir1 将几个文件和目录同时压缩成一个zip格式的压缩包 
unzip file1.zip 解压一个zip格式压缩包 
```



**RPM 包 - （Fedora, Redhat及类似系统）** 

```
rpm -ivh package.rpm 安装一个rpm包 
rpm -ivh --nodeeps package.rpm 安装一个rpm包而忽略依赖关系警告 
rpm -U package.rpm 更新一个rpm包但不改变其配置文件 
rpm -F package.rpm 更新一个确定已经安装的rpm包 
rpm -e package_name.rpm 删除一个rpm包 
rpm -qa 显示系统中所有已经安装的rpm包 
rpm -qa | grep httpd 显示所有名称中包含 "httpd" 字样的rpm包 
rpm -qi package_name 获取一个已安装包的特殊信息 
rpm -qg "System Environment/Daemons" 显示一个组件的rpm包 
rpm -ql package_name 显示一个已经安装的rpm包提供的文件列表 
rpm -qc package_name 显示一个已经安装的rpm包提供的配置文件列表 
rpm -q package_name --whatrequires 显示与一个rpm包存在依赖关系的列表 
rpm -q package_name --whatprovides 显示一个rpm包所占的体积 
rpm -q package_name --scripts 显示在安装/删除期间所执行的脚本l 
rpm -q package_name --changelog 显示一个rpm包的修改历史 
rpm -qf /etc/httpd/conf/httpd.conf 确认所给的文件由哪个rpm包所提供 
rpm -qp package.rpm -l 显示由一个尚未安装的rpm包提供的文件列表 
rpm --import /media/cdrom/RPM-GPG-KEY 导入公钥数字证书 
rpm --checksig package.rpm 确认一个rpm包的完整性 
rpm -qa gpg-pubkey 确认已安装的所有rpm包的完整性 
rpm -V package_name 检查文件尺寸、 许可、类型、所有者、群组、MD5检查以及最后修改时间 
rpm -Va 检查系统中所有已安装的rpm包- 小心使用 
rpm -Vp package.rpm 确认一个rpm包还未安装 
rpm2cpio package.rpm | cpio --extract --make-directories *bin* 从一个rpm包运行可执行文件 
rpm -ivh /usr/src/redhat/RPMS/`arch`/package.rpm 从一个rpm源码安装一个构建好的包 
rpmbuild --rebuild package_name.src.rpm 从一个rpm源码构建一个 rpm 包 
```



**YUM 软件包升级器 - （Fedora, RedHat及类似系统）** 

```
yum install package_name 下载并安装一个rpm包 
yum localinstall package_name.rpm 将安装一个rpm包，使用你自己的软件仓库为你解决所有依赖关系 
yum update package_name.rpm 更新当前系统中所有安装的rpm包 
yum update package_name 更新一个rpm包 
yum remove package_name 删除一个rpm包 
yum list 列出当前系统中安装的所有包 
yum search package_name 在rpm仓库中搜寻软件包 
yum clean packages 清理rpm缓存删除下载的包 
yum clean headers 删除所有头文件 
yum clean all 删除所有缓存的包和头文件 
```



**DEB 包 (Debian, Ubuntu 以及类似系统)** 

```
dpkg -i package.deb 安装/更新一个 deb 包 
dpkg -r package_name 从系统删除一个 deb 包 
dpkg -l 显示系统中所有已经安装的 deb 包 
dpkg -l | grep httpd 显示所有名称中包含 "httpd" 字样的deb包 
dpkg -s package_name 获得已经安装在系统中一个特殊包的信息 
dpkg -L package_name 显示系统中已经安装的一个deb包所提供的文件列表 
dpkg --contents package.deb 显示尚未安装的一个包所提供的文件列表 
dpkg -S /bin/ping 确认所给的文件由哪个deb包提供 
```



**APT 软件工具 (Debian, Ubuntu 以及类似系统)** 

```
apt-get install package_name 安装/更新一个 deb 包 
apt-cdrom install package_name 从光盘安装/更新一个 deb 包 
apt-get update 升级列表中的软件包 
apt-get upgrade 升级所有已安装的软件 
apt-get remove package_name 从系统删除一个deb包 
apt-get check 确认依赖的软件仓库正确 
apt-get clean 从下载的软件包中清理缓存 
apt-cache search searched-package 返回包含所要搜索字符串的软件包名称 
```

**查看文件内容** 

```
cat file1 从第一个字节开始正向查看文件的内容 
tac file1 从最后一行开始反向查看一个文件的内容 
more file1 查看一个长文件的内容 
less file1 类似于 'more' 命令，但是它允许在文件中和正向操作一样的反向操作 
head -2 file1 查看一个文件的前两行 
tail -2 file1 查看一个文件的最后两行 
tail -f /var/log/messages 实时查看被添加到一个文件中的内容 
```



**文本处理** 

```
cat file1 file2 ... | command <> file1_in.txt_or_file1_out.txt  general syntax for  	text manipulation using PIPE, STDIN and STDOUT 
cat file1 | command( sed, grep, awk, grep, etc...) > result.txt 合并一个文件的详细说明文	本，并将简介写入一个新文件中 
cat file1 | command( sed, grep, awk, grep, etc...) >> result.txt 合并一个文件的详细说明	文本，并将简介写入一个已有的文件中 
grep Aug /var/log/messages 在文件 '/var/log/messages'中查找关键词"Aug" 
grep ^Aug /var/log/messages 在文件 '/var/log/messages'中查找以"Aug"开始的词汇 
grep [0-9] /var/log/messages 选择 '/var/log/messages' 文件中所有包含数字的行 
grep Aug -R /var/log/* 在目录 '/var/log' 及随后的目录中搜索字符串"Aug" 
sed 's/stringa1/stringa2/g' example.txt 将example.txt文件中的 "string1" 替换成 		 		"string2" 
sed '/^$/d' example.txt 从example.txt文件中删除所有空白行 
sed '/ *#/d; /^$/d' example.txt 从example.txt文件中删除所有注释和空白行 
echo 'esempio' | tr '[:lower:]' '[:upper:]' 合并上下单元格内容 
sed -e '1d' result.txt 从文件example.txt 中排除第一行 
sed -n '/stringa1/p' 查看只包含词汇 "string1"的行 
sed -e 's/ *$//' example.txt 删除每一行最后的空白字符 
sed -e 's/stringa1//g' example.txt 从文档中只删除词汇 "string1" 并保留剩余全部 
sed -n '1,5p;5q' example.txt 查看从第一行到第5行内容 
sed -n '5p;5q' example.txt 查看第5行 
sed -e 's/00*/0/g' example.txt 用单个零替换多个零 
cat -n file1 标示文件的行数 
cat example.txt | awk 'NR%2==1' 删除example.txt文件中的所有偶数行 
echo a b c | awk '{print $1}' 查看一行第一栏 
echo a b c | awk '{print $1,$3}' 查看一行的第一和第三栏 
paste file1 file2 合并两个文件或两栏的内容 
paste -d '+' file1 file2 合并两个文件或两栏的内容，中间用"+"区分 
sort file1 file2 排序两个文件的内容 
sort file1 file2 | uniq 取出两个文件的并集(重复的行只保留一份) 
sort file1 file2 | uniq -u 删除交集，留下其他的行 
sort file1 file2 | uniq -d 取出两个文件的交集(只留下同时存在于两个文件中的文件) 
comm -1 file1 file2 比较两个文件的内容只删除 'file1' 所包含的内容 
comm -2 file1 file2 比较两个文件的内容只删除 'file2' 所包含的内容 
comm -3 file1 file2 比较两个文件的内容只删除两个文件共有的部分 
```



**字符设置和文件格式转换** 

```
dos2unix filedos.txt fileunix.txt 将一个文本文件的格式从MSDOS转换成UNIX 
unix2dos fileunix.txt filedos.txt 将一个文本文件的格式从UNIX转换成MSDOS 
recode ..HTML < page.txt > page.html 将一个文本文件转换成html 
recode -l | more 显示所有允许的转换格式 
```

**文件系统分析** 

```
badblocks -v /dev/hda1 检查磁盘hda1上的坏磁块 
fsck /dev/hda1 修复/检查hda1磁盘上linux文件系统的完整性 
fsck.ext2 /dev/hda1 修复/检查hda1磁盘上ext2文件系统的完整性 
e2fsck /dev/hda1 修复/检查hda1磁盘上ext2文件系统的完整性 
e2fsck -j /dev/hda1 修复/检查hda1磁盘上ext3文件系统的完整性 
fsck.ext3 /dev/hda1 修复/检查hda1磁盘上ext3文件系统的完整性 
fsck.vfat /dev/hda1 修复/检查hda1磁盘上fat文件系统的完整性 
fsck.msdos /dev/hda1 修复/检查hda1磁盘上dos文件系统的完整性 
dosfsck /dev/hda1 修复/检查hda1磁盘上dos文件系统的完整性 
```



**初始化一个文件系统** 

```
mkfs /dev/hda1 在hda1分区创建一个文件系统 
mke2fs /dev/hda1 在hda1分区创建一个linux ext2的文件系统 
mke2fs -j /dev/hda1 在hda1分区创建一个linux ext3(日志型)的文件系统 
mkfs -t vfat 32 -F /dev/hda1 创建一个 FAT32 文件系统 
fdformat -n /dev/fd0 格式化一个软盘 
mkswap /dev/hda3 创建一个swap文件系统 
```



**SWAP文件系统** 

```
mkswap /dev/hda3 创建一个swap文件系统 
swapon /dev/hda3 启用一个新的swap文件系统 
swapon /dev/hda2 /dev/hdb3 启用两个swap分区 
```



**备份** 

```
dump -0aj -f /tmp/home0.bak /home 制作一个 '/home' 目录的完整备份 
dump -1aj -f /tmp/home0.bak /home 制作一个 '/home' 目录的交互式备份 
restore -if /tmp/home0.bak 还原一个交互式备份 
rsync -rogpav --delete /home /tmp 同步两边的目录 
rsync -rogpav -e ssh --delete /home ip_address:/tmp 通过SSH通道rsync 
rsync -az -e ssh --delete ip_addr:/home/public /home/local 通过ssh和压缩将一个远程目录同	步到本地目录 
rsync -az -e ssh --delete /home/local ip_addr:/home/public 通过ssh和压缩将本地目录同步到	远程目录 
dd bs=1M if=/dev/hda | gzip | ssh user@ip_addr 'dd of=hda.gz' 通过ssh在远程主机上执行一	次备份本地磁盘的操作 
dd if=/dev/sda of=/tmp/file1 备份磁盘内容到一个文件 
tar -Puf backup.tar /home/user 执行一次对 '/home/user' 目录的交互式备份操作 
( cd /tmp/local/ && tar c . ) | ssh -C user@ip_addr 'cd /home/share/ && tar x -p' 		通	过ssh在远程目录中复制一个目录内容 
( tar c /home ) | ssh -C user@ip_addr 'cd /home/backup-home && tar x -p' 通过ssh在		远程		目录中复制一个本地目录 
tar cf - . | (cd /tmp/backup ; tar xf - ) 本地将一个目录复制到另一个地方，保留原有权限及链接 
find /home/user1 -name '*.txt' | xargs cp -av --target-directory=/home/backup/ --		parents 从一个目录查找并复制所有以 '.txt' 结尾的文件到另一个目录 
find /var/log -name '*.log' | tar cv --files-from=- | bzip2 > log.tar.bz2 查找所有以 	'.log' 结尾的文件并做成一个bzip包 
dd if=/dev/hda of=/dev/fd0 bs=512 count=1 做一个将 MBR (Master Boot Record)内容复制到软	盘的动作 
dd if=/dev/fd0 of=/dev/hda bs=512 count=1 从已经保存到软盘的备份中恢复MBR内容 
```



**光盘** 

```
cdrecord -v gracetime=2 dev=/dev/cdrom -eject blank=fast -force 清空一个可复写的光盘内容 
mkisofs /dev/cdrom > cd.iso 在磁盘上创建一个光盘的iso镜像文件 
mkisofs /dev/cdrom | gzip > cd_iso.gz 在磁盘上创建一个压缩了的光盘iso镜像文件 
mkisofs -J -allow-leading-dots -R -V "Label CD" -iso-level 4 -o ./cd.iso data_cd 创建	  	一个目录的iso镜像文件 
cdrecord -v dev=/dev/cdrom cd.iso 刻录一个ISO镜像文件 
gzip -dc cd_iso.gz | cdrecord dev=/dev/cdrom - 刻录一个压缩了的ISO镜像文件 
mount -o loop cd.iso /mnt/iso 挂载一个ISO镜像文件 
cd-paranoia -B 从一个CD光盘转录音轨到 wav 文件中 
cd-paranoia -- "-3" 从一个CD光盘转录音轨到 wav 文件中（参数-3） 
cdrecord --scanbus 扫描总线以识别scsi通道 
dd if=/dev/hdc | md5sum 校验一个设备的md5sum编码，例如一张 CD 
```



**网络 - （以太网和WIFI无线**） 

```
 ifconfig eth0 显示一个以太网卡的配置 
 ifup eth0 启用一个 'eth0' 网络设备 
 ifdown eth0 禁用一个 'eth0' 网络设备 
 ifconfig eth0 192.168.1.1 netmask 255.255.255.0 控制IP地址 
 ifconfig eth0 promisc 设置 'eth0' 成混杂模式以嗅探数据包 (sniffing) 
 	dhclient eth0 以dhcp模式启用 'eth0' 
 route -n show routing table 
 route add -net 0/0 gw IP_Gateway configura default gateway 
 route add -net 192.168.0.0 netmask 255.255.0.0 gw 192.168.1.1 configure static route 		to reach network '192.168.0.0/16' 
 route del 0/0 gw IP_gateway remove static route 
 echo "1" > /proc/sys/net/ipv4/ip_forward activate ip routing 
 hostname show hostname of system 
 host www.example.com lookup hostname to resolve name to ip address and viceversa
 nslookup www.example.com lookup hostname to resolve name to ip address and viceversa
 ip link show show link status of all interfaces 
 mii-tool eth0 show link status of 'eth0' 
 ethtool eth0 show statistics of network card 'eth0' 
 netstat -tup show all active network connections and their PID 
 netstat -tupl show all network services listening on the system and their PID 
 tcpdump tcp port 80 show all HTTP traffic 
 iwlist scan show wireless networks 
 iwconfig eth1 show configuration of a wireless network card 
 hostname show hostname 
 host www.example.com lookup hostname to resolve name to ip address and viceversa 
 nslookup www.example.com lookup hostname to resolve name to ip address and viceversa 
 whois www.example.com lookup on Whois database 
```


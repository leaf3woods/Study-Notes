## 文件传输协议FTP、SFTP和SCP

# 网络通信协议分层



应用层：



- HTTP（Hypertext Transfer Protocol 超文本传输协议，显示网页）
- DNS（Domain Name System）
- FTP（File Transfer Protocol）
- SFTP（SSH File Transfer Protocol，和FTP不一样）
- SCP（Secure copy，based on SSH）
- SSH （Secure Shell）



通信层：



- TCP（Transmission Control Protocol 三次握手传输协议）
- UDP



网络层：



- IP（Internet Protocol）
- ICMP（Internet Control Message Protocol，主要用于路由发送错误报告）



链接层：



- MAC（media access control）

# 文件传输协议：

**FTP（File Transfer Protocol）**:是TCP/IP网络上两台计算机传送文件的协议，FTP是在TCP/IP网络和INTERNET上最早使用的协议之一，它属于网络协议组的应用层。FTP客户机可以给服务器发出命令来下载文件，上载文件，创建或改变服务器上的目录。相比于HTTP，FTP协议要复杂得多。复杂的原因，是因为FTP协议要用到两个TCP连接，一个是命令链路，用来在FTP客户端与服务器之间传递命令；另一个是数据链路，用来上传或下载数据。FTP是基于TCP协议的，因此iptables防火墙设置中只需要放开指定端口（21 + PASV端口范围）的TCP协议即可。 
 
**ftp工作模式：** 

- **PORT（主动）方式**的连接过程是：客户端向服务器的FTP端口（默认是21）发送连接请求，服务器接受连接，建立一条命令链路。当需要传送数据时，客户端在命令链路上用PORT命令告诉服务器：“我打开了一个1024+的随机端口，你过来连接我”。于是服务器从20端口向客户端的1024+随机端口发送连接请求，建立一条数据链路来传送数据。
- **PASV（Passive被动）方式**的连接过程是：客户端向服务器的FTP端口（默认是21）发送连接请求，服务器接受连接，建立一条命令链路。当需要传送数据时，服务器在命令链路上用PASV命令告诉客户端：“我打开了一个1024+的随机端口，你过来连接我”。于是客户端向服务器的指定端口发送连接请求，建立一条数据链路来传送数据。
- PORT方式，服务器会主动连接客户端的指定端口，那么如果客户端通过代理服务器链接到internet上的网络的话，服务器端可能会连接不到客户端本机指定的端口，或者被客户端、代理服务器防火墙阻塞了连接，导致连接失败
- PASV方式，服务器端防火墙除了要放开21端口外，还要放开PASV配置指定的端口范围


**sftp（Secure File Transfer Protocol）：**安全文件传送协议。可以为传输文件提供一种安全的加密方法。sftp 与 ftp 有着几乎一样的语法和功能。SFTP为SSH的一部份，是一种传输文件到服务器的安全方式。在SSH软件包中，已经包含了一个叫作SFTP(Secure File Transfer Protocol)的安全文件传输子系统，SFTP本身没有单独的守护进程，它必须使用sshd守护进程（端口号默认是22）来完成相应的连接操作，所以从某种意义上来说，SFTP并不像一个服务器程序，而更像是一个客户端程序。SFTP同样是使用加密传输认证信息和传输的数据，所以，使用SFTP是非常安全的。但是，由于这种传输方式使用了加密/解密技术，所以传输效率比普通的FTP要低得多，如果您对网络安全性要求更高时，可以使用SFTP代替FTP。 
 
**SCP（Secure Copy）：**scp就是secure copy，是用来进行远程文件复制的，并且整个复制过程是加密的。数据传输使用ssh，并且和使用和ssh相同的认证方式，提供相同的安全保证。  
 
**ftp & scp/sftp比较：** 

- 和ftp不同的是sftp/scp传输协议是采用加密方式来传输数据的。而ftp一般来说允许明文传输，当然现在也有带SSL的加密ftp，有些服务器软件也可以设置成“只允许加密连接”，但是毕竟不是默认设置需要我们手工调整，而且很多用户都会忽略这个设置。
- 普通ftp仅使用端口21作为命令传输。由服务器和客户端协商另外一个随机端口来进行数据传送。在pasv模式下，服务器端需要侦听另一个端口。假如服务器在路由器或者防火墙后面，端口映射会比较麻烦，因为无法提前知道数据端口编号，无法映射。（现在的ftp服务器大都支持限制数据端口随机取值范围，一定程度上解决这个问题，但仍然要映射21号以及一个数据端口范围，还有些服务器通过UPnP协议与路由器协商动态映射，但比较少见）
- 当你的网络中还有一些unix系统的机器时，在它们上面自带了scp/sftp等客户端，不用再安装其它软件来实现传输目的。
- scp/sftp属于开源协议，我们可以免费使用不像FTP那样使用上存在安全或版权问题。所有scp/sftp传输软件（服务器端和客户端）均免费并开源，方便我们开发各种扩展插件和应用组件。
- 小提示：当然在提供安全传输的前提下sftp还是存在一些不足的，例如他的帐号访问权限是严格遵照系统用户实现的，只有将该帐户添加为操作系统某用户才能够保证其可以正常登录sftp服务器


**SSH（Secure Shell）：**，由 IETF 的网络工作小组（Network Working Group）所制定；SSH 为建立在应用层和传输层基础上的安全协议。SSH 是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题。 

- SSH是由客户端和服务端的软件组成的：服务端是一个守护进程(daemon)，他在后台运行并响应来自客户端的连接请求。服务端一般是sshd进程，提供了对远程连接的处理，一般包括公共密钥认证、密钥交换、对称密钥加密和非安全连接； 客户端包含ssh程序以及像scp（远程拷贝）、slogin（远程登陆）、sftp（安全文件传输）等其他的应用程序。 
- 从客户端来看，SSH提供两种级别的安全验证：第一种级别（基于口令的安全验证）； 第二种级别（基于密匙的安全验证）。
- SSH 主要有三部分组成： 传输层协议 [SSH-TRANS] ；用户认证协议 [SSH-USERAUTH] ；连接协议 [SSH-CONNECT]。


**OpenSSH**:是SSH（Secure SHell）协议的免费开源实现。SSH协议族可以用来进行远程控制，或在计算机之间传送文件。而实现此功能的传统方式，如telnet(终端仿真协议)、 rcp ftp、 rlogin、rsh都是极为不安全的，并且会使用明文传送密码。OpenSSH提供了服务端后台程序和客户端工具，用来加密远程控件和文件传输过程的中的数据，并由此来代替原来的类似服务。 OpenSSH是使用SSH透过计算机网络加密通讯的实现。它是取代由SSH Communications Security所提供的商用版本的开放源代码方案。目前OpenSSH是OpenBSD的子计划。OpenSSH常常被误认以为与OpenSSL有关联，但实际上这两个计划的有不同的目的，不同的发展团队，名称相近只是因为两者有同样的软件发展目标──提供开放源代码的加密通讯软件。 

ssh -V:查看当前服务器的ssh版本信息

<iframe width="1" height="1" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="box-sizing: border-box;" leftmargin="0" topmargin="0" data-id="pianshen.com_980x300_responsive_DFP"></iframe>





## WinSCP无法连接windows服务器进行文件传输



![在这里插入图片描述](https://www.pianshen.com/images/170/fed84b9d6ef1ebb929c79dedc106acf2.png)
 发现无法连接远程window服务器。
 **原因** —Windows用户应该都听说过Telnet，这也是一种命令行的远程登录工具，而且是系统自带的。在XP下是默认开启的，到了Win7及以后，系统虽然有这个功能，但需要用户手动安装并开启这个功能。

解决方法：手动安装 OpenSSH

（1）在需要远程访问的windows电脑上安装OpenSSH的server：具体步骤如下：

1. 下载地址：https://download.csdn.net/upload/10922606，并解压到本地；
2. 在C:\Program Files文件夹中新建OpenSSH文件夹，并将（1）中解压的所有文件copy到OpenSSH文件夹中，此处都需要Administrator的权限；
3. 管理员权限运行command，并切换到C:\ProgramFiles\OpenSSH这个文件夹，运行以下命令：powershell.exe -ExecutionPolicy Bypass -File install-sshd.ps1
    ![在这里插入图片描述](https://www.pianshen.com/images/250/8f021a47eeaed76209413de4b8b5b6da.png)
4. 设置防火墙入站规则：
    ![在这里插入图片描述](https://www.pianshen.com/images/184/6dc6bd9d52bc2a631435caee78dfe8c0.png)
    新建入站规则：选择port，tcp，固定端口22，Name写sshd，description写OpenSSH Server (sshd)，完成。

5、开启sshd服务
 ![在这里插入图片描述](https://www.pianshen.com/images/918/5062b8f95f70b81a3dbf983dea68ea76.png)
 找到sshd 服务，设置 自启动 最后点击Start Service。

6、在本地上安装WinSCP

7、开启WinSCP，Hostname写IP，port写22，username和password自己知道的。

<iframe width="1" height="1" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="box-sizing: border-box;" leftmargin="0" topmargin="0" data-id="pianshen.com_728x90_responsive_DFP"></iframe>
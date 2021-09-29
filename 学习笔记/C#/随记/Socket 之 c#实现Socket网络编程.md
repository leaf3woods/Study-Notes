# [Socket 之 c#实现Socket网络编程](https://www.cnblogs.com/xinaixia/p/5460845.html)

**一、命名空间：**

　　在网络环境下，最有用的两个命名空间是System.Net和 System.Net.Sockets。

　　1、System.Net：通常与较高程的操作有关，例如download或upload，试用HTTP和其他协议进行Web请求等等

　　2、System.Net.Sockets：所包含的类通常与较低程的操作有关。如果要直接使用Sockets或者 TCP/IP之类的协议

 

**二、Socket对象创建：**

　　针对Socket编程，.NET 框架的 Socket 类是 Winsock32 API 提供的套接字服务的托管代码版本。Socket可以象流Stream一样被视为一个数据通道，这个通道架设在应用程序端（客户端）和远程服务器端之间，而后，数据的读取（接收）和写入（发送）均针对这个通道来进行。

　　Socket 类支持两种基本模式：同步和异步。其区别在于：在同步模式中，对执行网络操作的函数（如 Send 和 Receive）的调用一直等到操作完成后才将控制返回给调用程序。在异步模式中，这些调用立即返回。 

　　在使用之前，你需要首先创建Socket对象的实例，这可以通过Socket类的构造方法来实现： 

　　public Socket(AddressFamily addressFamily,SocketType socketType,ProtocolType protocolType); 

　　addressFamily 参数指定 Socket 使用的寻址方案；

　　socketType 参数指定 Socket 的类型；

　　protocolType 参数指定 Socket 使用的协议。 

　　下面的示例语句创建一个 Socket，它可用于在基于 TCP/IP 的网络（如 Internet）上通讯。 

　　Socket temp = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp); 

　　若要使用 UDP 而不是 TCP，需要更改协议类型，如下面的示例所示： 

　　Socket temp = new Socket(AddressFamily.InterNetwork, SocketType.Dgram, ProtocolType.Udp); 

　　

**三、Socket操作流程：**

　　1、一旦创建 Socket，在客户端，将可以通过Connect方法连接到指定的服务器。

　　（1）、通过 Send 相关方法向远程服务器发送数据：

　　[1] Send/SendTo 为同步发送；

　　[2] BeginSend/BeginSendTo 为异步发送。

　　（2）、通过 Receive 相关方法从服务端接收数据：

　　[1] Receive/ReceiveFrom 为同步接收；

　　[2] BeginReceive/BeginReceiveFrom 为异步接收。

　　2、在服务器端

　　需要使用 Bind 方法绑定所指定的接口使Socket与一个本地终结点相联，并通过 Listen 方法侦听该接口上的请求，当侦听到用户端的连接时，调用 Accept 完成连接的操作，创建新的 Socket 以处理传入的连接请求。使用完 Socket 后，记住使用 Shutdown 方法禁用 Socket，并使用 Close 方法关闭 Socket。

 

**四、终结点：**

　　在Internet中，TCP/IP 使用套接字(一个网络地址和一个服务端口号)来唯一标识设备。网络地址(IP)标识网络上的特定设备；端口号(Port)标识要连接到的该设备上的特定服务。网络地址和服务端口的组合称为终结点，在 .NET 框架中正是由 EndPoint 类表示这个终结点，它提供表示网络资源或服务的抽象，用以标志网络地址等信息。

　　.Net同时也为每个受支持的地址族定义了 EndPoint 的子代；对于 IP 地址族，该类为 IPEndPoint。IPEndPoint 类包含应用程序连接到主机上的服务所需的主机和端口信息。

 

**五、IP地址获取方式:**

　　用到IPEndPoint类的时候就不可避免地涉及到计算机IP地址，System.Net命名空间中有两种类可以得到IP地址实例： 

　　IPAddress类： 在同一局域网中采用的获取方式。 IPAddress 类包含计算机在 IP 网络上的地址。其Parse方法可将 IP 地址字符串转换为 IPAddress 实例。下面的语句创建一个 IPAddress 实例： 

　　IPAddress myIP = IPAddress.Parse("192.168.0.1"); 

　　Dns 类： 在互联网中采用的获取方式。 向使用 TCP/IP Internet 服务的应用程序提供域名服务。其Resolve 方法查询 DNS 服务器以将用户友好的域名（如"host.mydomain.com"）映射到数字形式的 Internet 地址（如 192.168.0.1）。

　　Resolve方法 返回一个 IPHostEnty 实例，该实例包含所请求名称的地址和别名的列表。大多数情况下，可以使用 AddressList 数组中返回的第一个地址。下面的代码获取一个 IPAddress 实例，该实例包含服务器 host.mydomain.com 的 IP 地址。 

　　IPHostEntry ipHostInfo = Dns.Resolve("host.mydomain.com "); 

　　IPAddress ipAddress = ipHostInfo.AddressList[0]; 

　　你也可以使用GetHostName方法得到IPHostEntry实例： 

　　IPHosntEntry hostInfo=Dns.GetHostByName("host.mydomain.com ") 

　　在使用以上方法时，你将可能需要处理以下几种异常： 

　　SocketException异常：访问Socket时操作系统发生错误引发 

　　ArgumentNullException异常：参数为空引用引发 

　　ObjectDisposedException异常：Socket已经关闭引发

　　

　　Socket套接字简介：套接字最早是Unix的，window是借鉴过来的。TCP/IP协议族提供三种套接字：流式、数据报式、原始套接字。其中原始套接字允许对底层协议直接访问，一般用于检验新协议或者新设备问题，很少使用。

　　套接字编程原理：延续文件作用思想，打开-读写-关闭的模式。

　　C/S编程模式如下：

　　（1）服务器端：

　　打开通信通道，告诉本地机器，愿意在该通道上接受客户请求——监听，等待客户请求——接受请求，创建专用链接进行读写——处理完毕，关闭专用链接——关闭通信通道（当然其中监听到关闭专用链接可以重复循环）

　　（2）客户端：打开通信通道，连接服务器——数据交互——关闭信道。

　　Socket通信方式：

　　（1）同步：客户端在发送请求之后必须等到服务器回应之后才可以发送下一条请求。串行运行。

　　（2）异步：客户端请求之后，不必等到服务器回应之后就可以发送下一条请求。并行运行。

　　套接字模式：

　　（1）阻塞：执行此套接字调用时，所有调用函数只有在得到返回结果之后才会返回。在调用结果返回之前，当前进程会被挂起。即此套接字一直被阻塞在网络调用上。

　　（2）非阻塞：执行此套接字调用时，调用函数即使得不到得到返回结果也会返回。

　　套接字工作步骤：

　　（1）服务器监听：监听时服务器端套接字并不定位具体客户端套接字，而是处于等待链接的状态，实时监控网络状态

　　（2）客户端链接：客户端发出链接请求，要连接的目标是服务器端的套接字。为此客户端套接字必须描述服务器端套接字的服务器地址与端口号。

　　（3）链接确认：是指服务器端套接字监听到客户端套接字的链接请求时，它响应客户端链接请求，建立一个新的线程，把服务器端套接字的描述发送给客户端，一旦客户端确认此描述，则链接建立好。而服务器端的套接字继续处于监听状态，继续接受其他客户端套接字请求。

　　在TCP/IP网络中，IP网络交互分类两大类：面向连接的交互与面向无连接的交互。

 　![img](https://images.cnblogs.com/cnblogs_com/bloodmaster/2222.jpg)　　![img](https://images.cnblogs.com/cnblogs_com/bloodmaster/1111.jpg)

 

　　Socket构造函数：public socket(AddressFamily 寻址类型, SocketType 套接字类型, ProtocolType 协议类型)。但需要注意的是套接字类型与协议类型并不是可以随便组合。

| SocketType                                               | ProtocolType                                                 | 描述         |
| -------------------------------------------------------- | ------------------------------------------------------------ | ------------ |
| Stream                                                   | Tcp                                                          | 面向连接     |
| Dgram                                                    | Udp                                                          | 面向无连接   |
| Raw                                                      | Icmp                                                         | 网际消息控制 |
| Raw                                                      | Raw                                                          | 基础传输协议 |
| Socket类的公共属性：                                     |                                                              |              |
| 属性名                                                   | 描述                                                         |              |
| AddressFamily                                            | 获取Socket的地址族                                           |              |
| Available                                                | 获取已经从网络接收且可供读取的数据量                         |              |
| Blocking                                                 | 获取或设置一个值，只是socket是否处于阻塞模式                 |              |
| Connected                                                | 获取一个值，指示当前连接状态                                 |              |
| Handle                                                   | 获取socket的操作系统句柄                                     |              |
| LocalEndPoint                                            | 获取本地终端EndPoint                                         |              |
| RemoteEndPoint                                           | 获取远程终端EndPoint                                         |              |
| ProtocolType                                             | 获取协议类型                                                 |              |
| SocketType                                               | 获取SocketType类型                                           |              |
| Socket常用方法：                                         |                                                              |              |
| Bind(EndPoint)                                           | 服务器端套接字需要绑定到特定的终端，客户端也可以先绑定再请求连接 |              |
| Listen(int)                                              | 监听端口，其中parameters表示最大监听数                       |              |
| Accept()                                                 | 接受客户端链接，并返回一个新的链接，用于处理同客户端的通信问题 |              |
|                                                          |                                                              |              |
| Send()                                                   | 发送数据                                                     |              |
| Send(byte[])                                             | 简单发送数据                                                 |              |
| Send(byte[],SocketFlag)                                  | 使用指定的SocketFlag发送数据                                 |              |
| Send(byte[], int, SocketFlag)                            | 使用指定的SocketFlag发送指定长度数据                         |              |
| Send(byte[], int, int, SocketFlag)                       | 使用指定的SocketFlag，将指定字节数的数据发送到已连接的socket(从指定偏移量开始) |              |
| Receive()                                                | 接受数据                                                     |              |
| Receive(byte[])                                          | 简单接受数据                                                 |              |
| Receive (byte[],SocketFlag)                              | 使用指定的SocketFlag接受数据                                 |              |
| Receive (byte[], int, SocketFlag)                        | 使用指定的SocketFlag接受指定长度数据                         |              |
| Receive (byte[], int, int, SocketFlag)                   | 使用指定的SocketFlag，从绑定的套接字接收指定字节数的数据，并存到指定偏移量位置的缓冲区 |              |
|                                                          |                                                              |              |
| Connect(EndPoint)                                        | 连接远程服务器                                               |              |
| ShutDown(SocketShutDown)                                 | 禁用套接字，其中SocketShutDown为枚举，Send禁止发送，Receive为禁止接受，Both为两者都禁止 |              |
| Close()                                                  | 关闭套接字，释放资源                                         |              |
| 异步通信方法：                                           |                                                              |              |
| BeginAccept(AsynscCallBack,object)                       | 开始一个一步操作接受一个连接尝试。参数：一个委托。一个对象。对象包含此请求的状态信息。其中回调方法中必须使用EndAccept方法。应用程序调用BegineAccept方法后，系统会使用单独的线程执行指定的回调方法并在EndAccept上一直处于阻塞状态，直至监测到挂起的链接。EndAccept会返回新的socket对象。供你来同远程主机数据交互。不能使用返回的这个socket接受队列中的任何附加连接。调用BeginAccept当希望原始线程阻塞的时候，请调用WaitHandle.WaitOne方法。当需要原始线程继续执行时请在回调方法中使用ManualResetEvent的set方法 |              |
| BeginConnect(EndPoint, AsyncCallBack, Object)            | 回调方法中必须使用EndConnect()方法。Object中存储了连接的详细信息。 |              |
| BeginSend(byte[], SocketFlag, AsyncCallBack, Object)     |                                                              |              |
| BegineReceive(byte[], SocketFlag, AsyncCallBack, Object) |                                                              |              |
| BegineDisconnect(bool, AsyncCallBack, Object)            |                                                              |              |


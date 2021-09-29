# [Socket 之 API函数介绍](https://www.cnblogs.com/xinaixia/p/5460557.html)

**1、创建套接字──socket()**

　　应用程序在使用套接字前，首先必须拥有一个套接字，系统调用socket()向应用程序提供创建套接字的手段，其调用格式如下：

　　SOCKET PASCAL FAR socket(int af, int type, int protocol);

　　该调用要接收三个参数：af、type、protocol。

　　（1）**af：**指定通信发生的区域：AF_UNIX、AF_INET、AF_NS等，而DOS、WINDOWS中仅支持AF_INET，它是网际网区域。因此，地址族与协议族相同。

　　（2）**type：**描述要建立的套接字的类型。这里分三种：

　　[1]TCP流式套接字(SOCK_STREAM)：提供了一个面向连接、可靠的数据传输服务，数据无差错、无重复地发送，且按发送顺序接收。内设流量控制，避免数据流超限；数据被看作是字节流，无长度限制。文件传送协议（FTP）即使用流式套接字。

　　[2]数据报式套接字(SOCK_DGRAM)：提供了一个无连接服务。数据包以独立包形式被发送，不提供无错保证，数据可能丢失或重复，并且接收顺序混乱。网络文件系统（NFS）使用数据报式套接字。

　　[3]原始式套接字(SOCK_RAW)：该接口允许对较低层协议，如IP、ICMP直接访问。常用于检验新的协议实现或访问现有服务中配置的新设备。

　　（3）protocol：说明该套接字使用的特定协议，如果调用者不希望特别指定使用的协议，则置为0，使用默认的连接模式。

　　根据这三个参数建立一个套接字，并将相应的资源分配给它，同时返回一个整型套接字号。因此，socket()系统调用实际上指定了相关五元组中的“协议”这一元。

 

**2、指定本地地址──bind()**

　　当一个套接字用socket()创建后，存在一个名字空间(地址族)，但它没有被命名。bind()将套接字地址（包括本地主机地址和本地端口地址）与所创建的套接字号联系起来，即将名字赋予套接字，以指定本地半相关。其调用格式如下：

　　int PASCAL FAR bind(SOCKET s, const struct sockaddr FAR * name, int namelen);

　　（1）s：是由socket()调用返回的并且未作连接的套接字描述符(套接字号)。

　　（2）name：是赋给套接字s的本地地址（名字），其长度可变，结构随通信域的不同而不同。

　　（3）namelen：表明了name的长度。

　　如果没有错误发生，bind()返回0。否则返回SOCKET_ERROR。

 

**3、建立套接字连接──connect()****与accept()**

　　这两个系统调用用于完成一个完整相关的建立，其中connect()用于建立连接。accept()用于使服务器等待来自某客户进程的实际连接。

　　connect()的调用格式如下：

　　int PASCAL FAR connect(SOCKET s, const struct sockaddr FAR * name, int namelen);

　　参数s是欲建立连接的本地套接字描述符。

　　参数name指出说明对方套接字地址结构的指针。

　　对方套接字地址长度由namelen说明。

　　如果没有错误发生，connect()返回0。否则返回值SOCKET_ERROR。在面向连接的协议中，该调用导致本地系统和外部系统之间连接实际建立。

　　由于地址簇总被包含在套接字地址结构的前两个字节中，并通过socket()调用与某个协议簇相关。因此bind()和connect()无须协议作为参数。

　　accept()的调用格式如下：

　　SOCKET PASCAL FAR accept(SOCKET s, struct sockaddr FAR* addr, int FAR* addrlen);

　　参数s为本地套接字描述符，在用做accept()调用的参数前应该先调用过listen()。

　　addr 指向客户方套接字地址结构的指针，用来接收连接实体的地址。addr的确切格式由套接字创建时建立的地址族决定。

　　addrlen 为客户方套接字地址的长度（字节数）。

　　如果没有错误发生，accept()返回一个SOCKET类型的值，表示接收到的套接字的描述符。否则返回值INVALID_SOCKET。

　　accept()用于面向连接服务器。参数addr和addrlen存放客户方的地址信息。调用前，参数addr 指向一个初始值为空的地址结构，而addrlen 的初始值为0；调用accept()后，服务器等待从编号为s的套接字上接受客户连接请求，而连接请求是由客户方的connect()调用发出的。当有连接请求到达时，accept()调用将请求连接队列上的第一个客户方套接字地址及长度放入addr 和addrlen，并创建一个与s有相同特性的新套接字号。新的套接字可用于处理服务器并发请求。

　　四个套接字系统调用，socket()、bind()、connect()、accept()，可以完成一个完全五元相关的建立。socket()指定五元组中的协议元，它的用法与是否为客户或服务器、是否面向连接无关。bind()指定五元组中的本地二元，即本地主机地址和端口号，其用法与是否面向连接有关：在服务器方，无论是否面向连接，均要调用bind()，若采用面向连接，则可以不调用bind()，而通过connect()自动完成。若采用无连接，客户方必须使用bind()以获得一个唯一的地址。

 

**4、监听连接──listen()**

　　此调用用于面向连接服务器，表明它愿意接收连接。listen()需在accept()之前调用，其调用格式如下：

　　int PASCAL FAR listen(SOCKET s, int backlog);

　　参数s标识一个本地已建立、尚未连接的套接字号，服务器愿意从它上面接收请求。

　　backlog表示请求连接队列的最大长度，用于限制排队请求的个数，目前允许的最大值为5。

　　如果没有错误发生，listen()返回0。否则它返回SOCKET_ERROR。

　　listen()在执行调用过程中可为没有调用过bind()的套接字s完成所必须的连接，并建立长度为backlog的请求连接队列。

　　调用listen()是服务器接收一个连接请求的四个步骤中的第三步。它在调用socket()分配一个流套接字，且调用bind()给s赋于一个名字之后调用，而且一定要在accept()之前调用。

 

**5、数据传输──send()****与recv()**

　　当一个连接建立以后，就可以传输数据了。常用的系统调用有send()和recv()。

　　send()调用用于s指定的已连接的数据报或流套接字上发送输出数据，格式如下：

　　int PASCAL FAR send(SOCKET s, const char FAR *buf, int len, int flags);

　　参数s为已连接的本地套接字描述符。

　　buf 指向存有发送数据的缓冲区的指针，其长度由len 指定。

　　flags 指定传输控制方式，如是否发送带外数据等。

　　如果没有错误发生，send()返回总共发送的字节数。否则它返回SOCKET_ERROR。

　　recv()调用用于s指定的已连接的数据报或流套接字上接收输入数据，格式如下：

　　int PASCAL FAR recv(SOCKET s, char FAR *buf, int len, int flags);

　　参数s 为已连接的套接字描述符。

　　buf指向接收输入数据缓冲区的指针，

　　其长度由len 指定。

　　flags 指定传输控制方式，如是否接收带外数据等。

　　如果没有错误发生，recv()返回总共接收的字节数。如果连接被关闭，返回0。否则它返回SOCKET_ERROR。

 

**6、输入/****输出多路复用──select()**

　　select()调用用来检测一个或多个套接字的状态。对每一个套接字来说，这个调用可以请求读、写或错误状态方面的信息。请求给定状态的套接字集合由一个fd_set结构指示。在返回时，此结构被更新，以反映那些满足特定条件的套接字的子集，同时， select()调用返回满足条件的套接字的数目，其调用格式如下：

　　int PASCAL FAR select(int nfds, fd_set FAR * readfds, fd_set FAR * writefds, fd_set FAR * exceptfds, const struct timeval FAR * timeout);

　　参数nfds指明被检查的套接字描述符的值域，此变量一般被忽略。

　　参数readfds指向要做读检测的套接字描述符集合的指针，调用者希望从中读取数据。

　　参数writefds 指向要做写检测的套接字描述符集合的指针。

　　exceptfds指向要检测是否出错的套接字描述符集合的指针。

　　timeout指向select()函数等待的最大时间，如果设为NULL则为阻塞操作。

　　select()返回包含在fd_set结构中已准备好的套接字描述符的总数目，或者是发生错误则返回SOCKET_ERROR。

 

**7、关闭套接字──closesocket()**

　　closesocket()关闭套接字s，并释放分配给该套接字的资源；如果s涉及一个打开的TCP连接，则该连接被释放。closesocket()的调用格式如下：

　　BOOL PASCAL FAR closesocket(SOCKET s);

　　参数s待关闭的套接字描述符。

　　如果没有错误发生，closesocket()返回0。否则返回值SOCKET_ERROR。

 

以上就是SOCKET API一些常用的API函数，下面是一段代码：

 

//客户端代码：

\#include <WINSOCK2.H>

\#include <stdio.h>

\#pragma comment(lib,"ws2_32.lib")

 

int main()

{

​    int err;

​    WORD versionRequired;

​    WSADATA wsaData;

​    versionRequired=MAKEWORD(1,1);

​    err=WSAStartup(versionRequired,&wsaData);//协议库的版本信息

   

​    if (!err)

​    {

​       printf("客户端嵌套字已经打开!\n");

​    }

​    else

​    {

​       printf("客户端的嵌套字打开失败!\n");

​       return 0;//结束

​    }

​    SOCKET clientSocket=socket(AF_INET,SOCK_STREAM,0);

​    SOCKADDR_IN clientsock_in;

​    clientsock_in.sin_addr.S_un.S_addr=inet_addr("127.0.0.1");

​    clientsock_in.sin_family=AF_INET;

​    clientsock_in.sin_port=htons(6000);

​    //bind(clientSocket,(SOCKADDR*)&clientsock_in,strlen(SOCKADDR));//注意第三个参数

​    //listen(clientSocket,5);

​    connect(clientSocket,(SOCKADDR*)&clientsock_in,sizeof(SOCKADDR));//开始连接

   

​    char receiveBuf[100];

​    recv(clientSocket,receiveBuf,101,0);

​    printf("%s\n",receiveBuf);

   

​    send(clientSocket,"hello,this is client",strlen("hello,this is client")+1,0);

​    closesocket(clientSocket);

​    WSACleanup();

​    return 0;

}

 

///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

//服务器端代码：

\#include <WINSOCK2.H>

\#include <stdio.h>

\#pragma comment(lib,"ws2_32.lib")

int main()

{

​    //创建套接字

​    WORD myVersionRequest;

​    WSADATA wsaData;

​    myVersionRequest=MAKEWORD(1,1);

​    int err;

​    err=WSAStartup(myVersionRequest,&wsaData);

​    if (!err)

​    {

​       printf("已打开套接字\n");

​       

​    }

​    else

​    {

​       //进一步绑定套接字

​       printf("嵌套字未打开!");

​       return 0;

​    }

​    SOCKET serSocket=socket(AF_INET,SOCK_STREAM,0);//创建了可识别套接字

​    //需要绑定的参数

​    SOCKADDR_IN addr;

​    addr.sin_family=AF_INET;

​    addr.sin_addr.S_un.S_addr=htonl(INADDR_ANY);//ip地址

​    addr.sin_port=htons(6000);//绑定端口

   

​    bind(serSocket,(SOCKADDR*)&addr,sizeof(SOCKADDR));//绑定完成

​    listen(serSocket,5);//其中第二个参数代表能够接收的最多的连接数

   

​    //////////////////////////////////////////////////////////////////////////

​    //开始进行监听

​    //////////////////////////////////////////////////////////////////////////

​    SOCKADDR_IN clientsocket;

​    int len=sizeof(SOCKADDR);

​    while (1)

​    {

​       SOCKET serConn=accept(serSocket,(SOCKADDR*)&clientsocket,&len);//如果这里不是accept而是conection的话。。就会不断的监听

​       char sendBuf[100];

​       

​       sprintf(sendBuf,"welcome %s to bejing",inet_ntoa(clientsocket.sin_addr));//找对对应的IP并且将这行字打印到那里

​       send(serConn,sendBuf,strlen(sendBuf)+1,0);

​       char receiveBuf[100];//接收

​       recv(serConn,receiveBuf,strlen(receiveBuf)+1,0);

​       printf("%s\n",receiveBuf);

​       closesocket(serConn);//关闭

​       WSACleanup();//释放资源的操作

​    }

​    return 0;

}
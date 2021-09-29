# [Socket 之 同步以及异步通信](https://www.cnblogs.com/xinaixia/p/5461415.html)

 用netstat侦听下端口状态

 

| 同步通信：                                                   |
| ------------------------------------------------------------ |
| 预定义结构体，同步通信没有多线程异步委托回调，所以无需预定义结构体 |
| 客户端Client：                                               |

```c#
class Program
{
        static void Main()
        {
            try{
                int port = 2000;
                string host = "127.0.0.1";
                IPAddress ip = IPAddress.Parse(host);
                IPEndPoint ipe = new IPEndPoint(ip, port);//把ip和端口转化为IPEndPoint实例
                Socket c = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);//创建一个Socket
                Console.WriteLine("Conneting...");
                c.Connect(ipe);//连接到服务器
                string sendStr = "hello!This is a socket test";
                byte[] bs = Encoding.ASCII.GetBytes(sendStr);
                Console.WriteLine("Send Message");
                c.Send(bs, bs.Length, 0);//发送测试信息
                string recvStr = "";
                byte[] recvBytes = new byte[1024];
                int bytes;
                bytes = c.Receive(recvBytes, recvBytes.Length, 0);//从服务器端接受返回信息
                recvStr += Encoding.ASCII.GetString(recvBytes, 0, bytes);
                Console.WriteLine("Client Get Message:{0}", recvStr);//显示服务器返回信息
                c.Close();
            }
            catch (ArgumentNullException e){
                Console.WriteLine("ArgumentNullException: {0}", e);
            }
            catch (SocketException e){
                Console.WriteLine("SocketException: {0}", e);
            }
            Console.WriteLine("Press Enter to Exit");
            Console.ReadLine();
        }
}
```

 

| 服务器端： |
| ---------- |

```c#
class Program
{
    static void Main()
    {
        try{
            int port = 2000;
            string host = "127.0.0.1";
            IPAddress ip = IPAddress.Parse(host);
            IPEndPoint ipe = new IPEndPoint(ip, port);
            Socket s = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);//创建一个Socket类
            s.Bind(ipe);//绑定2000端口
            s.Listen(0);//开始监听
            Console.WriteLine("Wait for connect");
            Socket temp = s.Accept();//为新建连接创建新的Socket。
            Console.WriteLine("Get a connect");
            string recvStr = "";
            byte[] recvBytes = new byte[1024];
            int bytes;
            bytes = temp.Receive(recvBytes, recvBytes.Length, 0);//从客户端接受信息
            recvStr += Encoding.ASCII.GetString(recvBytes, 0, bytes);
            Console.WriteLine("Server Get Message:{0}", recvStr);//把客户端传来的信息显示出来
            string sendStr = "Ok!Client Send Message Sucessful!";
            byte[] bs = Encoding.ASCII.GetBytes(sendStr);
            temp.Send(bs, bs.Length, 0);//返回客户端成功信息
            temp.Close();
            s.Close();
        }
        catch (ArgumentNullException e){
            Console.WriteLine("ArgumentNullException: {0}", e);}
        catch (SocketException e){
            Console.WriteLine("SocketException: {0}", e);}
        Console.WriteLine("Press Enter to Exit");
        Console.ReadLine();
    }
}
```

 

| 异步通信：                                                   |
| ------------------------------------------------------------ |
| 客户端Client：                                               |
| 预定义结构体，用于异步委托之间的传递。用户根据自己需要定制即可 |

```c#
public class StateObject
{
    // Client socket.
    public Socket workSocket = null;
    // Size of receive buffer.
    public const int BufferSize = 256;
    // Receive buffer.
    public byte[] buffer = new byte[BufferSize];
    // Received data string.
    public StringBuilder sb = new StringBuilder();
}
```

```c#
public class AsynchronousClient
{
    // The port number for the remote device.
    private const int port = 11000;
    // ManualResetEvent instances signal completion.
    private static ManualResetEvent connectDone = new ManualResetEvent(false);
    private static ManualResetEvent sendDone = new ManualResetEvent(false);
    private static ManualResetEvent receiveDone = new ManualResetEvent(false);
    // The response from the remote device.
private static String response = String.Empty;
 
    private static void StartClient(){
        // Connect to a remote device.
        try{
            // Establish the remote endpoint for the socket.
            // The name of the remote device is "host.contoso.com".
            IPHostEntry ipHostInfo = Dns.Resolve("host.contoso.com");
            IPAddress ipAddress = ipHostInfo.AddressList[0];
            IPEndPoint remoteEP = new IPEndPoint(ipAddress, port);
 
            // Create a TCP/IP socket.
            Socket client = new Socket(AddressFamily.InterNetwork,
                SocketType.Stream, ProtocolType.Tcp);
 
            // Connect to the remote endpoint.
            client.BeginConnect(remoteEP,
                new AsyncCallback(ConnectCallback), client);
            connectDone.WaitOne();
 
            // Send test data to the remote device.
            Send(client, "This is a test<EOF>");
            sendDone.WaitOne();
 
            // Receive the response from the remote device.
            Receive(client);
            receiveDone.WaitOne();
 
            // Write the response to the console.
            Console.WriteLine("Response received : {0}", response);
 
            // Release the socket.
            client.Shutdown(SocketShutdown.Both);
            client.Close();
}
        catch (Exception e){
            Console.WriteLine(e.ToString());}
    }
 
    private static void ConnectCallback(IAsyncResult ar)
    {
        try{
            // Retrieve the socket from the state object.
            Socket client = (Socket)ar.AsyncState;
 
            // Complete the connection.
            client.EndConnect(ar);
 
            Console.WriteLine("Socket connected to {0}",
                client.RemoteEndPoint.ToString());
 
            // Signal that the connection has been made.
            connectDone.Set();
        }
        catch (Exception e){
            Console.WriteLine(e.ToString());}
    }
 
    private static void Receive(Socket client)
    {
        try{
            // Create the state object.
            StateObject state = new StateObject();
            state.workSocket = client;
 
            // Begin receiving the data from the remote device.
            client.BeginReceive(state.buffer, 0, StateObject.BufferSize, 0,
                new AsyncCallback(ReceiveCallback), state);
        }
        catch (Exception e){
            Console.WriteLine(e.ToString());}
    }
 
    private static void ReceiveCallback(IAsyncResult ar)
    {
        try{
            // Retrieve the state object and the client socket
            // from the asynchronous state object.
            StateObject state = (StateObject)ar.AsyncState;
            Socket client = state.workSocket;
 
            // Read data from the remote device.
            int bytesRead = client.EndReceive(ar);
 
            if (bytesRead > 0){
                // There might be more data, so store the data received so far.
                state.sb.Append(Encoding.ASCII.GetString(state.buffer, 0, bytesRead));
 
                // Get the rest of the data.
                client.BeginReceive(state.buffer, 0, StateObject.BufferSize, 0,
                    new AsyncCallback(ReceiveCallback), state);
            }
            else{
                // All the data has arrived; put it in response.
                if (state.sb.Length > 1)
                {
                    response = state.sb.ToString();
                }
                // Signal that all bytes have been received.
                receiveDone.Set();
            }
        }
        catch (Exception e){
            Console.WriteLine(e.ToString());}
    }
 
    private static void Send(Socket client, String data)
    {
        // Convert the string data to byte data using ASCII encoding.
        byte[] byteData = Encoding.ASCII.GetBytes(data);
 
        // Begin sending the data to the remote device.
        client.BeginSend(byteData, 0, byteData.Length, 0,
            new AsyncCallback(SendCallback), client);
    }
 
    private static void SendCallback(IAsyncResult ar)
    {
        try{
            // Retrieve the socket from the state object.
            Socket client = (Socket)ar.AsyncState;
 
            // Complete sending the data to the remote device.
            int bytesSent = client.EndSend(ar);
            Console.WriteLine("Sent {0} bytes to server.", bytesSent);
 
            // Signal that all bytes have been sent.
            sendDone.Set();
}
        catch (Exception e){
            Console.WriteLine(e.ToString());}
    }
 
    public static int Main(String[] args)
    {
        StartClient();
        return 0;
    }
}
```

| 服务器端Server：                                             |
| ------------------------------------------------------------ |
| 预定义结构体，用于异步委托之间的传递。同客户端的一致。不再赘述 |
| 正文：                                                       |

```c#
// State object for reading client data asynchronously
public class AsynchronousSocketListener
{
    // Thread signal.
    public static ManualResetEvent allDone = new ManualResetEvent(false);
    public AsynchronousSocketListener(){}
    public static void StartListening()
    {
        // Data buffer for incoming data.
        byte[] bytes = new Byte[1024];
        // Establish the local endpoint for the socket.
        // The DNS name of the computer
        // running the listener is "host.contoso.com".
 
        //IPHostEntry ipHostInfo = Dns.Resolve(Dns.GetHostName());
        IPHostEntry ipHostInfo = Dns.Resolve("127.0.0.1");
        IPAddress ipAddress = ipHostInfo.AddressList[0];
        IPEndPoint localEndPoint = new IPEndPoint(ipAddress, 11000);
        // Create a TCP/IP socket.
        Socket listener = new Socket(AddressFamily.InterNetwork,SocketType.Stream, ProtocolType.Tcp);
        // Bind the socket to the local endpoint and listen for incoming connections.
        try{
            listener.Bind(localEndPoint);
            listener.Listen(100);
            while (true){
                // Set the event to nonsignaled state.
                allDone.Reset();
                // Start an asynchronous socket to listen for connections.
                Console.WriteLine("Waiting for a connection...");
                listener.BeginAccept(new AsyncCallback(AcceptCallback),listener);
                // Wait until a connection is made before continuing.
                allDone.WaitOne();
            }
        }
        catch (Exception e){
            Console.WriteLine(e.ToString());}
        Console.WriteLine("\nPress ENTER to continue...");
        Console.Read();
    }
    public static void AcceptCallback(IAsyncResult ar)
    {
        // Signal the main thread to continue.
        allDone.Set();
        // Get the socket that handles the client request.
        Socket listener = (Socket)ar.AsyncState;
        Socket handler = listener.EndAccept(ar);
        // Create the state object.
        StateObject state = new StateObject();
        state.workSocket = handler;
        handler.BeginReceive(state.buffer, 0, StateObject.BufferSize, 0,new AsyncCallback(ReadCallback), state);
    }
    public static void ReadCallback(IAsyncResult ar)
    {
        String content = String.Empty;
        // Retrieve the state object and the handler socket
        // from the asynchronous state object.
        StateObject state = (StateObject)ar.AsyncState;
        Socket handler = state.workSocket;
        // Read data from the client socket.
        int bytesRead = handler.EndReceive(ar);
        if (bytesRead > 0)
        {
            // There    might be more data, so store the data received so far.
            state.sb.Append(Encoding.ASCII.GetString(state.buffer, 0, bytesRead));
            // Check for end-of-file tag. If it is not there, read
            // more data.
            content = state.sb.ToString();
            if (content.IndexOf("<EOF>") > -1){
                // All the data has been read from the
                // client. Display it on the console.
                Console.WriteLine("Read {0} bytes from socket. \n Data : {1}",
                content.Length, content);
                // Echo the data back to the client.
                Send(handler, "Server return :" + content);
            }
            else{
                // Not all data received. Get more.
                handler.BeginReceive(state.buffer, 0, StateObject.BufferSize, 0,
                new AsyncCallback(ReadCallback), state);
            }
        }
    }
    private static void Send(Socket handler, String data){
        // Convert the string data to byte data using ASCII encoding.
        byte[] byteData = Encoding.ASCII.GetBytes(data);
        // Begin sending the data to the remote device.
        handler.BeginSend(byteData, 0, byteData.Length, 0,
        new AsyncCallback(SendCallback), handler);
    }
    private static void SendCallback(IAsyncResult ar)
    {
        try{
            // Retrieve the socket from the state object.
            Socket handler = (Socket)ar.AsyncState;
            // Complete sending the data to the remote device.
            int bytesSent = handler.EndSend(ar);
            Console.WriteLine("Sent {0} bytes to client.", bytesSent);
            handler.Shutdown(SocketShutdown.Both);
            handler.Close();
        }
        catch (Exception e){
            Console.WriteLine(e.ToString());
        }
    }
    public static int Main(String[] args)
    {
        StartListening();
        return 0;
    }
}
```


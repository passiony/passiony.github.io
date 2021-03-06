---
layout:     post
title:      "socket-tcp和udp"
subtitle:   "socket"
date:       2018-03-10
author:     "Passion"
header-img: "img/home-bg-o.jpg"
tags:
    - C#
    - unity
    - socket
    - tcp
    - udp
---


##传输层协议
**TCP协议** 和 **UDP协议** 属于传输层协议
![protocols.jpg](https://upload-images.jianshu.io/upload_images/1214868-0f6b32f99419ba0a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##TCP（Transmission Control Protocol 传输控制协议）
“面向连接”就是在正式通信前必须要与对方建立起连接。比如你给别人打电话，必须等线路接通了、对方拿起话筒才能相互通话。
TCP是基于连接的协议，也就是说，在正式收发数据前，必须和对方建立可靠的连接。一个TCP连接必须要经过三次“对话”才能建立起来，其中的过程非常复杂，我们这里只做简单、形象的介绍，你只要做到能够理解这个过程即可。我们来看看这三次对话的简单过程：[主机]A向主机B发出连接请求数据包：“我想给你发数据，可以吗？”，这是第一次对话；主机B向主机A发送同意连接和要求同步（同步就是两台主机一个在发送，一个在接收，协调工作）的数据包：“可以，你什么时候发？”，这是第二次对话；主机A再发出一个数据包确认主机B的要求同步：“我现在就发，你接着吧！”，这是第三次对话。三次“对话”的目的是使数据包的发送和接收同步，经过三次“对话”之后，[主机]A才向主机B正式发送数据。

##UDP（User Data Protocol 用户数据报协议 )
UDP是与TCP相对应的协议。它是[面向非连接]的协议，在正式通信前不必与对方先建立连接，不管对方状态就直接发送。与手机短信非常相似：你在发短信的时候，只需要输入对方手机号就OK了。它不与对方建立连接，而是直接就把[数据包]发送过去！
UDP适用于一次只传送少量数据、对可靠性要求不高的应用环境。比如，我们经常使用“ping”命令来测试两台[主机]之间TCP/IP通信是否正常，其实“ping”命令的原理就是向对方主机发送ICMP数据包，然后对方主机确认收到数据包，如果数据包是否到达的消息及时反馈回来，那么网络就是通的。例如，在默认状态下，一次“ping”操作发送4个数据包（如图所示）。大家可以看到，发送的数据包数量是4包，收到的也是4包（因为对方主机收到后会发回一个确认收到的数据包）。这充分说明了UDP协议是[面向非连接]的协议，没有建立连接的过程。正因为UDP协议没有连接的过程，所以它的通信效率高；但也正因为如此，它的可靠性不如TCP协议高。QQ就使用UDP发消息，因此有时会出现收不到消息的情况。

---
##Socket（套接字）
建立网络通信连接至少要一对端口号(socket)。socket本质是编程接口(API)，对TCP/IP的封装，TCP/IP也要提供可供程序员做网络开发所用的接口，这就是Socket编程接口；HTTP是轿车，提供了封装或者显示数据的具体形式；Socket是发动机，提供了网络通信的能力。

应用层通过传输层进行数据通信时，TCP会遇到同时为多个应用程序进程提供并发服务的问题。多个TCP连接或多个应用程序进程可能需要通过同一个 TCP协议端口传输数据。为了区别不同的应用程序进程和连接，许多计算机操作系统为应用程序与TCP／IP协议交互提供了套接字(Socket)接口。应用层可以和传输层通过Socket接口，区分来自不同应用程序进程或网络连接的通信，实现数据传输的并发服务。

套接字（socket）是通信的基石，是支持TCP/IP协议的网络通信的基本操作单元。它是网络通信过程中端点的抽象表示，包含进行网络通信必须的五种信息：连接使用的协议，本地主机的IP地址，本地进程的协议端口，远地主机的IP地址，远地进程的协议端口。

###建立socket连接

建立Socket连接至少需要一对套接字，其中一个运行于客户端，称为ClientSocket ，另一个运行于服务器端，称为ServerSocket 。

套接字之间的连接过程分为三个步骤：服务器监听，客户端请求，连接确认。

服务器监听：服务器端套接字并不定位具体的客户端套接字，而是处于等待连接的状态，实时监控网络状态，等待客户端的连接请求。

客户端请求：指客户端的套接字提出连接请求，要连接的目标是服务器端的套接字。为此，客户端的套接字必须首先描述它要连接的服务器的套接字，指出服务器端套接字的地址和端口号，然后就向服务器端套接字提出连接请求。

连接确认：当服务器端套接字监听到或者说接收到客户端套接字的连接请求时，就响应客户端套接字的请求，建立一个新的线程，把服务器端套接字的描述发给客户端，一旦客户端确认了此描述，双方就正式建立连接。而服务器端套接字继续处于监听状态，继续接收其他客户端套接字的连接请求。

---
##TCP示例：
服务器
```
using System;
using System.Text;
using System.Net.Sockets;
using System.Net;

class Program
{
    static void Main(string[] args)
    {
        IPAddress address = IPAddress.Parse("192.168.11.3");
        int port = 8500;
        TcpListener listener = new TcpListener(address, port);

        listener.Start();
        Console.WriteLine("开始侦听");

        while (true)
        {
            TcpClient client = listener.AcceptTcpClient();
            //接收到客户端后，交给RemoteCient去处理
            RemoteClient remote = new RemoteClient(client);
        }
    }
}
class RemoteClient
{
    public const int BuffseSize = 8192;

    TcpClient tcpClient;
    NetworkStream stream;
    byte[] buffer;

    public RemoteClient(TcpClient client)
    {
        this.tcpClient = client;
        Console.WriteLine(client.Client.RemoteEndPoint + " -> 连接成功");

        stream = client.GetStream();
        buffer = new byte[BuffseSize];

        //开启异步读取消息
        stream.BeginRead(buffer, 0, BuffseSize, ReadAsync, null);
    }

    void ReadAsync(IAsyncResult result)
    {
        try
        {
            int readCount = stream.EndRead(result);
            if (readCount == 0) throw new Exception("读取到0字节");

            string msg = Encoding.UTF8.GetString(buffer, 0, readCount);
            Console.WriteLine("接收到消息 -> " + msg);

            lock (stream) //再次开启读取
            {
                Array.Clear(buffer, 0, buffer.Length);
                stream.BeginRead(buffer, 0, BuffseSize, ReadAsync, null);
            }
        }
        catch (Exception e)
        {
            Console.WriteLine(e.Message);
        }
    }

    //发送给客户端消息
    void SendMessage(string msg)
    {
        byte[] temp = Encoding.UTF8.GetBytes(msg);
        stream.Write(temp, 0, temp.Length);
        Console.WriteLine("回复客户端 -> " + msg);
    }
}
```

客户端
```
using System;
using System.Text;
using System.Net.Sockets;
using System.Net;

class Program
{
    static void Main(string[] args)
    {
        SocketClient client = new SocketClient();

        //发送消息
        while (true)
        {
            string msg = Console.ReadLine();
            client.SendMessage(msg);
        }
    }
}

public class SocketClient
{
    string ip = "192.168.11.3";
    int port = 8500;

    NetworkStream stream;
    byte[] buffer;
    int BuffseSize = 8192;

    public SocketClient()
    {
        TcpClient client = new TcpClient();
        client.Connect(ip, port);

        Console.WriteLine("连接服务器 -> " + client.Client.RemoteEndPoint);
        stream = client.GetStream();

        buffer = new byte[8192];
        stream.BeginRead(buffer,0,buffer.Length, ReadAsync,null);
    }

    void ReadAsync(IAsyncResult result)
    {
        try
        {
            int readCount = stream.EndRead(result);
            if (readCount == 0) throw new Exception("读取到0字节");

            string msg = Encoding.UTF8.GetString(buffer, 0, readCount);
            Console.WriteLine("接收到消息 -> " + msg);

            lock (stream) //再次开启读取
            {
                Array.Clear(buffer, 0, buffer.Length);
                stream.BeginRead(buffer, 0, BuffseSize, ReadAsync, null);
            }
        }
        catch (Exception e)
        {
            Console.WriteLine(e.Message);
        }
    }

    //发送消息到服务器
    public void SendMessage(string msg)
    {
        byte[] temp = Encoding.UTF8.GetBytes(msg);

        stream.Write(temp, 0, temp.Length);
        Console.WriteLine("发送消息 -> " + msg);
    }
}
```
---
---
##UDP示例

服务器
```
using System;
class Program
{
    static void Main(string[] args)
    {
        SocketClient client = new SocketClient();

        while (true)
        {
            string input = Console.ReadLine();
            client.Send(input);
        }
    }
}

using System;
using System.Net;
using System.Net.Sockets;
using System.Text;

class SocketClient
{
    private UdpClient udpClient;
    private const int localPort = 5500;

    private const string remoteIP = "127.0.0.1";
    private const int remotePort = 6600;
    private IPEndPoint remotePoint;

    public SocketClient()
    {
        remotePoint = new IPEndPoint(IPAddress.Parse(remoteIP), remotePort);

        udpClient = new UdpClient(localPort);
        udpClient.Connect(IPAddress.Parse(remoteIP), remotePort);
        udpClient.BeginReceive(OnReceive, null);
    }

    void OnReceive(IAsyncResult result)
    {
        try
        {
            byte[] buffer = udpClient.EndReceive(result, ref remotePoint);
            OnMessage(buffer);

            lock (udpClient)
            {
                udpClient.BeginReceive(OnReceive, null);
            }
        }
        catch (Exception e)
        {
            Console.WriteLine(e.ToString());
        }
    }

    void OnMessage(byte[] buffer)
    {
        string message = Encoding.Default.GetString(buffer);
        Console.WriteLine("Receive Message:{0}", message);
    }

    public void Send(string message)
    {
        byte[] sendData = Encoding.Default.GetBytes(message);
        udpClient.Send(sendData, sendData.Length);
        Console.WriteLine("Send Message:{0}", message);
    }
}
```
客户端
```
using System;
class Program
{
    static void Main(string[] args)
    {
        SocketClient client = new SocketClient();

        while (true)
        {
            string input = Console.ReadLine();
            client.Send(input);
        }
    }
}

using System;
using System.Net;
using System.Net.Sockets;
using System.Text;
class SocketClient
{
    private UdpClient udpClient;
    private const int localPort = 6600;

    private const string remoteIP = "127.0.0.1";
    private const int remotePort = 5500;
    private IPEndPoint remotePoint;

    public SocketClient()
    {
        remotePoint = new IPEndPoint(IPAddress.Parse(remoteIP), remotePort);

        udpClient = new UdpClient(localPort);
        udpClient.Connect(IPAddress.Parse(remoteIP), remotePort);
        udpClient.BeginReceive(OnReceive, null);
    }

    void OnReceive(IAsyncResult result)
    {
        try
        {
            byte[] buffer = udpClient.EndReceive(result, ref remotePoint);
            OnMessage(buffer);

            lock (udpClient)
            {
                udpClient.BeginReceive(OnReceive, null);
            }
        }
        catch (Exception e)
        {
            Console.WriteLine(e.ToString());
        }
    }

    void OnMessage(byte[] buffer)
    {
        string message = Encoding.Default.GetString(buffer);
        Console.WriteLine("Receive Message:{0}", message);
    }

    public void Send(string message)
    {
        byte[] sendData = Encoding.Default.GetBytes(message);
        udpClient.Send(sendData, sendData.Length);
        Console.WriteLine("Send Message:{0}", message);
    }
}
```
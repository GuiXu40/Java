# -----------------Java网络编程-------------------------
<p id="title"></p>

## 目录
<a href="#p1">:diamonds:IP和InetAddress类</a><br>
<a href="#p2">:diamonds:URL与URLConnection类</a><br>
<a href="#p3">:diamonds:URLEncoder类与URLDecoder类</a><br>
<a href="#p4">:diamonds:TCP程序设计</a><br>
<a href="#p5">:diamonds:UDP程序设计</a><br>
<p id="p1"></p>

## :hearts:IP和InetAddress类
<a href="#title">:spades:回到目录</a><br>
InetAddress类是对IP地址的描述
#### :egg:IP地址简介
```Java
IP地址=网络地址+主机地址
(1)网络号: 用于识别主机所在的网络
(2)主机号: 用于识别该网络中的主机
```
#### :egg:InetAddress类
有两个子类: Inet4Address,Inet6Address,常用操作方法如下表:

方法|描述
---|:--:
public static InetAddress getByName(String host)|通过主机名称得到InetAddress对象
public static InetAddredd getLocalHost()|通过本机得到InetAddress对象
public String getHostName()|得到IP地址
public boolean isReachable(int timeout)|判断地址是否可达,同时指定超时时间

```Java
import java.net.InetAddress;
public class Main {
    public static void main(String[] args) throws Exception{  //所有异常抛出
	// write your code here
        InetAddress locAdd=InetAddress.getLocalHost();  //取得本地的InetAddress对象
        InetAddress remAdd=InetAddress.getByName("www.baidu.com");  //取得远程InetAddress
        System.out.println("本机IP地址:"+locAdd);
        System.out.println("远程IP地址:"+remAdd);
        System.out.println("本机是否可达?"+locAdd.isReachable(5000));
    }
}
//本机IP地址:LAPTOP-TVMGVHHG/10.13.12.234
//远程IP地址:www.baidu.com/39.156.66.14
//本机是否可达?true
```
<p id="p2"></p>

## :hearts:URL与URLConnection类
<a href="#title">:spades:回到目录</a><br>
#### :egg:URL
URL(uniform Rescource Locator)统一资源定位符,URL类的常用方法:

方法|描述
---|:--:
public URL(String spec)|根据指定的地址实例化URL对象
public URL(String Protocol,String host,int port,String file)|实例化URL对象,并指定协议,主机,端口号,资源文件
public URLConnection openConnection()|取得一个URLConnecection对象
public final InputStream()|取得输入流

例:使用 URL 读取内容
```Java
import java.io.InputStream;
import java.net.InetAddress;
import java.net.URL;
import java.util.Scanner;
public class Main {
    public static void main(String[] args) throws Exception{  //所有异常抛出
	// write your code here
        URL url=new URL("https","www.github.com",80,"/GuiXu40"); //指定操作的URL
        InputStream input=url.openStream();   //打开输入流,读取URL内容
        Scanner scan=new Scanner(input);    //实例化Scanner对象
        scan.useDelimiter("\n");         //设置读取分隔符
        while (scan.hasNext()){        //输出内容
            System.out.print(scan.next());
        }
    }
}
//内容大小:-1
//内容类型:text/html; charset=utf-8
```
#### :egg:URLConnection类
URLConnection是**封装访问远程网络资源的一般方法的类**-->通过他可以建立与远程服务器的连接
<p id="p3"></p>

## :hearts:URLEncoder类与URLDecoder类
<a href="#title">:spades:回到目录</a><br>
在URL中,中文会被进行**编码操作**,,在Java中如果想要完成编码和解码的操作就必须使用URLEncode(对传递的内容进行编码),URLDecode(对内容进行解码)
<br>
例:编码及解码操作
```Java
import java.net.*;
public class Main {
    public static void main(String[] args) throws Exception{  //所有异常抛出
	// write your code here
        String keyword="guixu 桂旭";
        String encode= URLEncoder.encode(keyword,"UTF-8");
        System.out.println("编码之后的内容:"+encode);
        String decode= URLDecoder.decode(encode,"UTF-8");
        System.out.println("解码之后的内容:"+decode);
    }
}
//编码之后的内容:guixu+%E6%A1%82%E6%97%AD
//解码之后的内容:guixu 桂旭
```
<p id="p4"></p>

## :hearts:TCP程序设计
<a href="#title">:spades:回到目录</a><br>
#### :egg:ServerSocket类与Socket类
**使用socket类可以方便的建立可靠的,双向的,持续的,点对点的通信连接**<br>
ServerSocket类主要用于服务器端程序的开发,用于接收客户端的请求,常用方法如下:

方法|描述
---|:--:
public ServerSocket(int port)|创建ServerSocket实例,并指定监听端口
public Scoket accept()|等待客户端连接,此方法连接前一直阻塞
public InetAddress getInetAddress()|返回服务器的IP地址
public boolean isClose()|放回ServerSocket的关闭状态
public void close()|关闭ServerSocket

在服务器端每次运行时都要使用accept()方法等待客户端连接,每一个socket都表示一个客户端对象,方法 如下:

方法|描述
---|:--:
public Socket(String host,int port)|构造Socket对象,同时指定要连接服务器的主机名称及连接端口
public InputStream getInputStream()|放回此套接字的输入流
public OutputStream getOutputStream()|放回此套接字的输出流
public void close()|关闭此Socket
public boolean isClose()|判断此套接字是否被关闭

#### :egg:第一个TCP程序
建立服务器程序
```Java
package com.company;

import java.io.InputStream;
import java.io.PrintStream;
import java.net.*;
import java.util.Scanner;
public class Main {
    public static void main(String[] args) throws Exception{  //所有异常抛出
	// write your code here
        ServerSocket server=null;  //声明ServerSocket对象
        Socket client=null;  //一个Socket对象表示一个客户端
        PrintStream out=null;  //声明打印输出流,以向客户端输出
        server=new ServerSocket(8888);  //此时服务器再8888端口上等待客户端的访问
	
        client=server.accept();   //程序在此阻塞,等待客户端连接
        String str="hello world";  //要向客户端输出的信息
        out=new PrintStream(client.getOutputStream());  //实例化打印流对象,输出对象
        out.print(str);  //输出信息
        out.close();    //关闭输出流
        client.close();  //关闭客户端连接
        server.close();  //关闭服务器连接
    }
}
```
编写客户端程序
```Java
public class Main {
    public static void main(String[] args) throws Exception{
        ServerSocket server=null;
        Socket client=null;
        PrintStream out=null;
        server=new ServerSocket(8888);
        System.out.println("begin");
        client=server.accept();
        String str="hello world";
        out=new PrintStream(client.getOutputStream());
        out.print(str);
        out.close();
        client.close();
        server.close();
    }
}
```
#### :egg:案例
#### :egg:在服务器上应用多线程
<p id="p5"></p>

## :hearts:UDP程序设计
<a href="#title">:spades:回到目录</a><br>
#### :egg:UDP简介
#### :egg:UDP程序实现

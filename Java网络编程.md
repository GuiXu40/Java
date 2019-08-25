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
#### :egg:案例
#### :egg:在服务器上应用多线程
<p id="p5"></p>

## :hearts:UDP程序设计
<a href="#title">:spades:回到目录</a><br>
#### :egg:UDP简介
#### :egg:UDP程序实现

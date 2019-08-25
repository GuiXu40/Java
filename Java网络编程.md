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
#### :egg:URLConnection类
<p id="p3"></p>

## :hearts:URLEncoder类与URLDecoder类
<a href="#title">:spades:回到目录</a><br>
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

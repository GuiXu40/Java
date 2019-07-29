# --------------------Java基础程序设计-------------------------
<p id="title"></p>

## 目录
<a href="#p1">:diamonds:数据类型划分</a><br>
<a href="#p2">:diamonds:基本数据类型</a><br>
<a href="#p3">:diamonds:数据类型的转换</a><br>
<a href="#p4">:diamonds:运算符,表达式和语句</a><br>
<a href="#p5">:diamonds:选择与循环语句</a><br>
<p id="p1"></p>

## :hearts:数据类型划分
<a href="#title">:spades:回到目录</a><br>
Java的数据类型可分为基本数据类型和引用数据类型
<p id="p2"></p>

## :hearts:基本数据类型
<a href="#title">:spades:回到目录</a><br>
如果想在程序中使用一个变量,就必须先声明

数据类型|大小/位|数据范围
---|:--:|:--:
long(长整数)|64|-9223372036854775808~9223372036854775807
int(整数)|32|-2147483648~2147483647
short(短整数)|16|-32768~32767
byte(位)|8|-128~127
char(字符)|2|0~65536
float(单精度)|32|-3.4E38~3.4E38
double(双精度)|64|-1.7E308~1.7E308
**关于基本类型数据的选择**<br>
+ 整数用int,小数使用double
+ 描述日期时间数字或者表示文件或内存大小使用long
+ 如果实现内容传递(IO操作,网络编程)或者是编码转换时使用byte
+ 如果实现逻辑的控制,可以使用Boolean(true和false)
+ 处理中文,使用char避免乱码
#### :copyright:整数类型
整形包括long,short,int ,byte,声明变量的方式
```java
short a;
int b;
long c;
```
如果数据超出长度,会报错
#### :copyright:数据的溢出
当数据大小超出可以表示的范围,值将会发生紊乱,下面将整形的最大数值+1和+2
```Java
public class Main {
    public static void main(String[] args) {
	// write your code here
        int max=Integer.MAX_VALUE;
        System.out.println("整形最大值:"+max);
        System.out.println("整形最大值+1:"+(max+1));
        System.out.println("整形最大值+2:"+(max+2));
    }
}

//运行结果
整形最大值:2147483647
整形最大值+1:-2147483648   
整形最大值+2:-2147483647
```
这种情况就像计数器的内容达最大值时会自动清零,所以当最大数+1时,就变成最小值了,这就是产生了**溢出**.
#### :copyright:字符类型
#### :copyright:浮点数类型和双精度浮点数类型
#### :copyright:布尔类型
#### :copyright:基本数据类型的默认值
<p id="p3"></p>

## :hearts:数据类型的转换
<a href="#title">:spades:回到目录</a><br>
#### :copyright:数据类型的自动转换
#### :copyright:数据类型的强制转换
<p id="p4"></p>

## :hearts:运算符,表达式和语句
<a href="#title">:spades:回到目录</a><br>
#### :copyright:运算符
#### :copyright:简洁表达式
<p id="p5"></p>

## :hearts:选择与循环语句
<a href="#title">:spades:回到目录</a><br>
#### :copyright:程序的结构
#### :copyright:选择结构
#### :copyright:循环结构 
#### :copyright:循环的中段

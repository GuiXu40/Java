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
<br>
为了解决int类型的溢出,可以再表达式的任一常亮中加上大写的"L",或者在变量前面加上long(强制类型转换)
```java
public class Main {
    public static void main(String[] args) {
	// write your code here
        int max=Integer.MAX_VALUE;
        System.out.println("整形最大值:"+max);
        System.out.println("整形最大值+1:"+(max+1));
        System.out.println("整形最大值+2:"+(max+2L));
        System.out.println("整形最大值+2:"+((long)max+2));
    }
}
//运行结果
整形最大值:2147483647
整形最大值+1:-2147483648
整形最大值+2:2147483649
整形最大值+2:2147483649
```
**说明**: Integer在Java中属于包装类
#### :copyright:字符类型
其实字符类型是特殊的整数.Unicode编码为每个字符制定了一个唯一的数值.<br>
例:测试字符与整数之间的转换
```Java
public class Main {
    public static void main(String[] args) {
	// write your code here
        char ch1='a';   //定义字符
        char ch2=97;    //定义字符,整形转字符
        System.out.println("ch1="+ch1);
        System.out.println("ch2="+ch2);
    }
}
//运行结果
ch1=a
ch2=a
```
字符要用一对单引号('')括起来
<br>
如果想输出特殊的字符,就必须转义,常见的转义字符

转义字符|描述
---|:--:
\f|换页
\\|反斜线
\b|倒退一格
\'|单引号
\r|归位
\"|双引号
\t|制表符Tab
\n|换行

例:转义字符的运用
```Java
public class Main {
    public static void main(String[] args) {
	// write your code here
        char ch1='\"';
        char ch2='\\';
        System.out.println("ch1="+ch1);
        System.out.println("ch2="+ch2);
        System.out.println("\"hello,world\"");
    }
}
//////
ch1="
ch2=\
"hello,world"
```
#### :copyright:浮点数类型和双精度浮点数类型
```Java
double num ;    //声明num为双精度浮点型变量
float num=3.0F;   //声明num为浮点型变量,其初值为3.0
```
需要注意的是,使用浮点数时,默认的类型是double,在数值后面可加上D或是d.在数据后面加上F或f,则作为float类型的标识.<br>
浮点数计算:
```Java
public class Main {
    public static void main(String[] args) {
	// write your code here
        float num=0.3f;
        System.out.println("两个数向乘："+num*num);
    }
}

//0.9
```
#### :copyright:布尔类型
布尔类型只有true和false,没有其他的值可以赋值给这个变量<br>语法:
```Java
boolean flag=true; 
```
通常用于控制程序的流程
#### :copyright:基本数据类型的默认值
若没有给变量赋值,则会给变量赋默认值

数据类型|默认值
---|:--:
byte|0
short|0
int|0
long|0L
float|0.0f
double|0.0d
char|\u0000
boolean|false
<p id="p3"></p>

## :hearts:数据类型的转换
<a href="#title">:spades:回到目录</a><br>
自动类型转换和强制类型转换
#### :copyright:数据类型的自动转换
转换条件:<br>
+ 转换前的数据类型与转换后的数据类型兼容
+ 转换后的数据类型表示范围比转换前的大(扩大转换)<br>
比如说int与float做计算时,int会被转换成float<br>
**注意**: 任何数据类型碰到String类型的数据或常量之后都向String转换
```Java
public class Main {
    public static void main(String[] args) {
	// write your code here
        String str="guixu";
        int x=60;
        str=str+x;
        System.out.println("str+"+str);
    }
}
```
#### :copyright:数据类型的强制转换
强制类型转换的语法:
```Java
(欲转换的数据类型)变量名称-->显示转换
```
例子:数据类型的强制转换
```Java
public class Main {
    public static void main(String[] args) {
	// write your code here
        float a=30.3f;
        int x=(int)a;
        System.out.println("x:"+x);
        System.out.println("10/3="+((float)10/3));
    }
}
//x:30
//10/3=3.3333333
```
<p id="p4"></p>

## :hearts:运算符,表达式和语句
<a href="#title">:spades:回到目录</a><br>
#### :copyright:运算符

运算符|描述
---|:--:
=|赋值
+|正号/加法
-|负号/减法
* |乘法
>|大于
<|小于
++ |自增
-- |自减
&|AND,与
&&|短路与
'|' |OR,或
'||' |短路或
()|提高括号表达式的优先级
<br>

#### :copyright:简洁表达式

运算符|范例用法|说明|描述
---|:--:|:--:|:--:
+=|a+=b|a+b的值存放到a中|a=a+b
-=|a-=b|a0b的值存放到a中|a=a-b
* = |a*=b|a* b的值存放到a中 |a=a* b
/=|a/=b|a/b的值存放到a中|a=a/b
%=|a%=b|a%b的值存放到a中|a=a%b
<p id="p5"></p>

## :hearts:选择与循环语句
<a href="#title">:spades:回到目录</a><br>
#### :copyright:程序的结构
#### :copyright:选择结构
#### :copyright:循环结构 
#### :copyright:循环的中段

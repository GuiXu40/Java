# -----------------Java IO-------------------------
<p id="title"></p>

## 目录
<a href="#p1">:diamonds:操作文件的类--File</a><br>
<a href="#p2">:diamonds:RandomAccessFile类</a><br>
<a href="#p3">:diamonds:字节流与字符流</a><br>
<a href="#p4">:diamonds:转换流</a><br>
<a href="#p5">:diamonds:内存操作流</a><br>
<a href="#p6">:diamonds:管道流</a><br>
<a href="#p7">:diamonds:打印流</a><br>
<a href="#p8">:diamonds:System类对IO的支持</a><br>
<a href="#p9">:diamonds:BufferedReader类</a><br>
<a href="#p10">:diamonds:Scanner类</a><br>
<a href="#p11">:diamonds:数据操作流</a><br>
<a href="#p12">:diamonds:合并流</a><br>
<a href="#p13">:diamonds:压缩流</a><br>
<a href="#p14">:diamonds:回退流</a><br>
<a href="#p15">:diamonds:字符编码</a><br>
<a href="#p16">:diamonds:对象序列化</a><br>
<a href="#p17">:diamonds:实例操作--单人信息</a><br>
<p id="p1"></p>

## :hearts:操作文件的类--File
<a href="#title">:spades:回到目录</a><br>
#### :egg:FILE类的基本介绍
File类的构造方法
```Java
public File(String pathname)-->实例化File类的时候,必须设置好路径
```
File类中主要方法和常量

方法或常量|描述
---|:--:
public static final String pathSeparator|表示路径的分隔符
public static final String separator|表示路径的分隔符
public Flie(String pathname)|创建Flie类对象,传入完整路径
public File(File parent,String child)|根据指定的父路径创建子文件
public boolean createNewFile() throws IOException|创建新文件
public boolean delete()|删除文件
public boolean exists()|判断文件是否存在
public boolean isDirectory()|判断给定的路径是否在一个目录
public long length()|返回文件的大小
public String[] list()| 列出指定目录的全部内容,只列出名称
public File[] listFiles()|列出指定目录的全部内容,会列出路径
public boolean mkdir()|创建一个目录
public boolean mkdirs()|创建多级目录
public boolean renameTo(File dest)|为已有的文件重命名
public long lastModified()|取得文件的最后一次修改日期的时间
public File getParentFile()|取得当前路径的父路径
#### :egg:使用File类操作文件
+ 创建一个新文件
#### :egg:范例--列出指定目录的全部内容
<p id="p2"></p>

## :hearts:RandomAccessFile类
<a href="#title">:spades:回到目录</a><br>
#### :egg:写入数据
#### :egg:读取数据
<p id="p3"></p>

## :hearts:字节流
<a href="#title">:spades:回到目录</a><br>
#### :egg:字节流
#### :egg:字符流
#### :egg:区别
#### :egg:范例--文件复制
<p id="p4"></p>

## :hearts:转换流
<a href="#title">:spades:回到目录</a><br>
<p id="p5"></p>

## :hearts:内存操作流
<a href="#title">:spades:回到目录</a><br>
<p id="p6"></p>

## :hearts:管道流
<a href="#title">:spades:回到目录</a><br>
<p id="p7"></p>

## :hearts:打印流
<a href="#title">:spades:回到目录</a><br>
#### :egg:基本操作
#### :egg:使用打印流进行格式化
<p id="p8"></p>

## :hearts:System类对IO的支持
<a href="#title">:spades:回到目录</a><br>
#### :egg:System.out
#### :egg:System.err
#### :egg:System.in
#### :egg:输入/输出重定向
<p id="p9"></p>

## :hearts:BufferedReader类
<a href="#title">:spades:回到目录</a><br>
#### :egg:键盘输入数据的标准格式
#### :egg:相关操作实例
<p id="p10"></p>

## :hearts:Scanner类
<a href="#title">:spades:回到目录</a><br>
#### :egg:Scanner类简介
#### :egg:使用Scanner类输入数据
<p id="p11"></p>

## :hearts:数据操作流
<a href="#title">:spades:回到目录</a><br>
#### :egg:DataOuputStream类
#### :egg:DataInputStream类
<p id="p12"></p>

## :hearts:合并流
<a href="#title">:spades:回到目录</a><br>
<p id="p13"></p>

## :hearts:压缩流
<a href="#title">:spades:回到目录</a><br>
#### :egg:ZIP压缩输入输出流
#### :egg:ZipOutputStream类
#### :egg:ZipFile类
#### :egg:ZipInputStream类
<p id="p14"></p>

## :hearts:回退流
<a href="#title">:spades:回到目录</a><br>
<p id="p15"></p>

## :hearts:字母编码
<a href="#title">:spades:回到目录</a><br>
#### :egg:Java常见编码
#### :egg:编码显示
#### :egg:乱码产生
<p id="p16"></p>

## :hearts:对象序列化
<a href="#title">:spades:回到目录</a><br>
#### :egg:基本概念与Serializable接口
#### :egg:对象输出流
#### :egg:对象输入流
#### :egg:Externlizeble接口
#### :egg:transient关键字
#### :egg:序列化一组对象
<p id="p17"></p>

## :hearts:实例操作--单人信息管理
<a href="#title">:spades:回到目录</a><br>

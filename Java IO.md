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
+ 创建一个新文件--createFile()必须使用try--catch进行异常处理
```Java
import java.io.File;
import java.io.IOException;
public class Main {
    public static void main(String[] args) {
	// write your code here
        File f=new File("d:\\text.txt");  //必须给出完整路径
        try{
            f.createNewFile();      //根据指定的路径创建文件
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
观察Flie类中提供的两个常量
```Java
import java.io.File;
public class Main {
    public static void main(String[] args) {
        System.out.println("pathSeparator:"+File.pathSeparator);
        System.out.println("separator:"+File.separator);
    }
}
//pathSeparator:;
//separator:\
```
对于创建文件的代码,最好的 做法是将以上使用的常量表示路径分隔符,修改如下
```Java
import java.io.File;
import java.io.IOException;
public class Main {
    public static void main(String[] args) {
	// write your code here
        String path="d"+File.separator+"text.txt";
        File f=new File(path);  //必须给出完整路径
        try{
            f.createNewFile();      //根据指定的路径创建文件
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
+ 删除一个文件--delete()
在删除文件之前应该保证文件存在可以使用File的exists()方法(返回boolean类型)
<br>
将上面的三种方法结合起来:
```Java
import java.io.File;
import java.io.IOException;
public class Main {
    public static void main(String[] args) {
	// write your code here
        String path="d"+File.separator+"text.txt";
        File f=new File(path);  //必须给出完整路径
        if(f.exists()){
            f.delete();
        }else{
            try{
                f.createNewFile();      //根据指定的路径创建文件
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```
**文件拓展名可有可无**
+ 创建一个文件夹--mkdir()
```Java
public class Main {
    public static void main(String[] args) {
	// write your code here
        String path="d"+File.separator+"mldn";
        File f=new File(path);  //必须给出完整路径
        if (f.getParentFile().exists()){   //父路径不存在
            f.mkdir();                 //创建文件夹
        }
    }
}
```
要先判断父目录是否存在
+ 列出指定目录的全部内容
   + public String[] list(): 列出全部名称,返回一个字符串数组
   + public File[] listFiles(): 列出完整的路径,返回一个File对象数组
```Java
public class Main {
    public static void main(String[] args) {
	// write your code here
        File f=new File("d:"+File.separator);
        String str[]=f.list();
        for (int i=0;i<str.length;i++){
            System.out.println(str[i]);
        }
    }
}
Sql 2014
Sql_server_Database
System Volume Information
Temp
text.txt
```
使用listFiles()方法列出目录的全部内容
```Java
public class Main {
    public static void main(String[] args) {
	// write your code here
        File f=new File("d:"+File.separator);
        File str[]=f.listFiles();
        for (int i=0;i<str.length;i++){
            System.out.println(str[i]);
        }
    }
}
d:\VMware
d:\vs
d:\Windows Kits
d:\学习--Tools
```
+ 判断一个给定的路径是否是目录
```Java
public class Main {
    public static void main(String[] args) {
	// write your code here
        File f=new File("d:"+File.separator);
        if (f.isDirectory()){
            System.out.println("是路径");
        }else{
            System.out.println("不是路径");
        }
    }
}
//是路径
```
#### :egg:范例--列出指定目录的全部内容
```Java
import java.io.File;
import java.io.IOException;
public class Main {
    public static void main(String[] args) {
	// write your code here
        File f=new File("d:"+File.separator);
        print(f);
    }
    public static void print(File file){
        if(file !=null){
            if(file.isDirectory()){
                File f[]=file.listFiles();
                if(f!=null){
                    for (int i=0;i<f.length;i++){
                        print(f[i]);
                    }
                }
            }else{
                System.out.println(file);
            }
        }
    }
}
```
上面的程序采用递归的调用形式,不断地判断传进来的路径是否是目录,如果是目录,则继续列出子文件夹,如果不是,则直接打印路径名称
<p id="p2"></p>

## :hearts:RandomAccessFile类
<a href="#title">:spades:回到目录</a><br>
RandomAccessFile类的常用方法

方法|描述
---|:--:
public RandomAccessFile(File file,String mode)throws FileNotFoundException|接收File类的对象,制定个操作路径没需要设置模式"r\w"
public RandomAccessFile(String name,String mode)throws FileNotFoundException|直接输入一个固定对的文件路径
public void close()throws IOException|关闭操作
public int read(byte[] b)|将内容读取到byte数组中
public final byte readByte()|读取一个字节
public final int readInt()|从文件中读取整形数据
public void seek(long pop)|设置读取指针的位置
public final void writeByte()|将一个字符串写入文件中,按字节的方式处理
public final void writeInt()|将一个int型的数据写入文件,长度为4位
public int skipBytes(int n)|指针跳过多少个字节

如果文件不存在,系统将自动进行创建
#### :egg:写入数据
为了保证可以进行随机读取,写入的名字必须是8个字节,写入的数字必须是4个字节
```Java
import java.io.File;
import java.io.RandomAccessFile;
public class Main {
    public static void main(String[] args) throws Exception{  //直接抛出异常,程序中可以不再做出处理
            // write your code here
            File f=new File("d:"+File.separator+"text.txt");
        RandomAccessFile rdf=null;   //声明一个类对象
        rdf=new RandomAccessFile(f,"rw");
        String name=null;
        int age=0;
        name="zhangshan";  ///字符串长度为8
        age=30;    //数字长度为4
        rdf.writeBytes(name);
        rdf.writeInt(age);
        name="guixu   ";
        age=31;
        rdf.writeBytes(name);
        rdf.writeInt(age);
        name="ysl     ";
        age=32;
        rdf.writeBytes(name);
        rdf.writeInt(age);
    }
}
```
#### :egg:读取数据
读取文件时直接使用"r"的模式即可
<p id="p3"></p>

## :hearts:字节流和字符流
<a href="#title">:spades:回到目录</a><br>
在程序中所有的数据都是以流的形式进行传输和保存的,有字节流和字符流,Java 中IO操作的步骤:
+ 使用File类打开一个文件
+ 通过字节流或字符流的子类指定输出的位置
+ 进行读写操作
+ 关闭输入输出
#### :egg:字节流
+ 字节输出流: OutputStream
<br>
此类的定义如下
```Java
public abstract class OutputStream extends Object implements Closeable,Flushable
```
是一个抽象类,如果要使用,必须通过子类实例化对象,OutputStream类中的主要方法

方法或常量|描述
---|:--:
public void close()|关闭输出流
public void flush()|刷新缓冲区
public void write(byte[] b)|将一个byte数组写入数据流
public void write(byte[] b,int off,int len)|将一个指定范围的byte数组写入数据流
public abstract void write(int b)|将一个字节数据写入数据流

可以使用FileOutputStream子类
```Java
public FileOutputStream(File file)
```
例:向文件中写入字符串
```Java
import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStream;
import java.io.RandomAccessFile;
public class Main {
    public static void main(String[] args) throws Exception{
            // write your code here
	    //第一步:使用子类实例化父类对象
            File f=new File("d:"+File.separator+"text.txt");
	    //第二步: 通过子类实例化对象
        OutputStream out=null;
        out=new FileOutputStream(f);
	
        String str="hello,world";
        byte b[]=str.getBytes();
        out.write(b);
        out.close();
    }
}
```
为了方便起见,直接在主方法上使用throws关键字抛出异常,可以减少try-catch语句
文件不存在会自动创建,也可以把每个字节一个个写入文件中
```Java
import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStream;
import java.io.RandomAccessFile;
public class Main {
    public static void main(String[] args) throws Exception{
            // write your code here
            File f=new File("d:"+File.separator+"text.txt");
        OutputStream out=null;
        out=new FileOutputStream(f);
        String str="hello,world";
        byte b[]=str.getBytes();
        for (int i=0;i<b.length;i++){
            out.write(b[i]);
        }
        out.close();
    }
}
``
+ 追加新内容:上面的代码如果重复执行,会覆盖文件中已有内容,如果想在后面增加内容,就可以使用另一个构造方法 
```Java
public FileOutputStream(Flie file,boolean append){}
```
如果append为true,表示在文件的末尾追加内容
修改之前的代码
```Java
public class Main {
    public static void main(String[] args) throws Exception{
            // write your code here
            File f=new File("d:"+File.separator+"text.txt");
        OutputStream out=null;
        out=new FileOutputStream(f,true);  //此处表示在后面添加内容
        String str="hello,world";
        byte b[]=str.getBytes();
        for (int i=0;i<b.length;i++){
            out.write(b[i]);
        }
        out.close();
    }
}
```
**可以通过\r\n增加换行**
+ 字节输入流: InputStream从文件中把内容读取出来定义与OutputStream类似,主要方法:

方法|描述
---|:--:
public int available()|取得输入文件的大小
public void close()|关闭输入流
public abstract int read()|读取内容,以数字的方式读取
public int read(byte[] b)|将内容读到补byte数组中,同时返回读入的个数

例:从文件中读取内容
```Java
public class Main {
    public static void main(String[] args) throws Exception{
            // write your code here
            File f=new File("d:"+File.separator+"text.txt");
        InputStream inptut=new FileInputStream(f);
        byte b[]=new byte[1024];
        inptut.read(b);
        inptut.close();
        System.out.println(new String(b));   //吧byte数组变成为字符串输出
    }
}
```
上面代码的改进:
```Java
public class Main {
    public static void main(String[] args) throws Exception{
            // write your code here
            File f=new File("d:"+File.separator+"text.txt");
        InputStream inptut=new FileInputStream(f);
        byte b[]=new byte[1024];
        int len=inptut.read(b);
        inptut.close();
        System.out.println(new String(b,0,len));   //吧byte数组变成为字符串输出
    }
}
```
上面的代码虽然指定了byte数组的范围,但是程序开辟了很多的无用空间,要根据文件的大小来指定byte数组的大小
```Java
public class Main {
    public static void main(String[] args) throws Exception{
            // write your code here
            File f=new File("d:"+File.separator+"text.txt");
        InputStream inptut=new FileInputStream(f);
        byte b[]=new byte[(int)f.length()];  //数组大小由文件大小决定
        int len=inptut.read(b);
        inptut.close();
        System.out.println(new String(b,0,len));   //吧byte数组变成为字符串输出
    }
}
```
除以上的方法外,也可以通过循环,从文件中把一个个的内容读取出来,直接使用read即可
```Java
public class Main {
    public static void main(String[] args) throws Exception{
            // write your code here
            File f=new File("d:"+File.separator+"text.txt");
        InputStream inptut=new FileInputStream(f);
        byte b[]=new byte[(int)f.length()];
        for (int i=0;i<b.length;i++){
            b[i]=(byte) inptut.read();
        }
        inptut.close();
        System.out.println(new String(b));   //吧byte数组变成为字符串输出
    }
}
```
上面的程序是在明确知道具体数组大小的前提下开展的,如果不知道输入内容的大小,只能通过判断是否读到文件末尾的方式来读取文件
```Java
public class Main {
    public static void main(String[] args) throws Exception{
            // write your code here
            File f=new File("d:"+File.separator+"text.txt");
        InputStream inptut=new FileInputStream(f);
        int len=0;
        byte b[]=new byte[1024];
        int temp=0;
        while ((temp=inptut.read())!=-1){
            b[len]=(byte)temp;
            len++;
        }
        inptut.close();
        System.out.println(new String(b));   //吧byte数组变成为字符串输出
    }
}
```
#### :egg:字符流
一个字符等于两个字节,Java提供了Reader和Writer两个操作字符流的类<br>
+ 字符输出流Writer,如果是向文件中写入内容,应该使用FileWriter的子类,Writer类的常用方法:

方法|描述
---|:--:
public abstract void close()|关闭输出流
public void write(String str)|将字符串输出
public void write(char[] cbuf)|将字符数组输出
public abstract void flush() |强制性清空缓存

FileWrite类的构造方法定义如下
```Java
public FileWrite(File file)
```
例:向文件中写数据
```Java
public class Main {
    public static void main(String[] args) throws Exception{
        // write your code here
        File f=new File("d:"+File.separator+"text.txt");
        Writer out=new FileWriter(f);  //通过对象多态性,进行实例化
        String str="hello,world";
        out.write(str);
        out.close();  //关闭输出流
    }
}
```
+ 使用FileWriter追加文件的内容,直接使用FileWriter类中的构造函数即可实现
```Java
public FileWrite(File file,boolean append)
```
例:追加文件内容
```Java
public class Main {
    public static void main(String[] args) throws Exception{
        // write your code here
        File f=new File("d:"+File.separator+"text.txt");
        Writer out=new FileWriter(f,true);  //通过对象多态性,进行实例化
        String str="hello,world";
        out.write(str);
        out.close();  //关闭输出流
    }
}
```
+ 字符输入流: Reader,从文件中读取文件,可以直接使用FileReader子类,Reader类的常用方法:

方法|描述
---|:--:
public abstract void close()|关闭输出流
public int read()|读取单个字符
public int read(char[] cbuf)|将内容读到字符串数组中,返回读入的长度

例:从文件中读取内容
```Java
public class Main {
    public static void main(String[] args) throws Exception{
        // write your code here
        File f=new File("d:"+File.separator+"text.txt");
        Reader reader=new FileReader(f);
        char c[]=new char[1024];
        int len=reader.read(c);
        reader.close();
        System.out.println("内容为:"+new String(c,0,len));
    }
}
```
如果不知道数据的长度,可以使用循环的方式进行读取
```Java
public class Main {
    public static void main(String[] args) throws Exception{
        // write your code here
        File f=new File("d:"+File.separator+"text.txt");
        Reader reader=new FileReader(f);
        char c[]=new char[1024];
        int len=0;
        int temp=0;
        while((temp=reader.read())!=-1){
            c[len]=(char)temp;
            len++;
        }
        reader.close();
        System.out.println("内容为:"+new String(c,0,len));
    }
}
```
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

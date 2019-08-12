# -----------------Java新IO------------------------
<p id="title"></p>

## 目录
<a href="#p1">:diamonds:缓冲区和Buffer</a><br>
<a href="#p2">:diamonds:通道</a><br>
<a href="#p3">:diamonds:文件锁FileLock类</a><br>
<a href="#p4">:diamonds:字符集Charset类</a><br>
<a href="#p5">:diamonds:Selector类</a><br>
<p id="p1"></p>

## :hearts:缓冲区和Buffer
<a href="#title">:spades:回到目录</a><br>
以前的IO操作时面向流的,现象面向块,新IO并没有在原来的基础上开发,而是采用了全新的类和接口,缓冲区是一个线性的,有序的数据集,只能容纳某种特定数据类型
#### :egg:Buffer的基本操作

缓冲区类型|描述
---|--:
ByteBuffer|字节
CharBuffer|字符
ShortBuffer|短整型
IntBuffer|整形
LongBuffer|长整形
FloatBuffer|单精度浮点型
DoubleBuffer|双精度浮点型

Buffer类的常用方法

方法|描述
public final int capacity)()|返回缓冲区的容量
public final int limit()|返回缓冲区的限制
public final Buffer limit(int limit)|设置缓冲区的限制
public final int position()|返回缓冲区的操作位置
public final Buffer position(int newPosition)|设置缓冲区的操作位置
public final Buffer clear()|清空缓冲区
public final Buffer flip()|重设缓存区,在写入之前调整,改变缓存的指针
public final Buffer reset()|恢复缓存区的标记位置
public final boolean hasRemaining()|判断当前位置和限制之间是否有关系

各数据类型缓冲区提供的常用方法

方法|描述
---|:--:
public static 缓存区类型 allocate(int capacity)|分配缓冲区空间
public 基本数据类型 get()|取得当前位置的内容
public 基本数据类型 get(int index)|取得指定位置的内容
public 缓冲区类型 put(基本数据类型 x)|写入一组指定的基本数据类型
public 缓冲区类型 put(数据类型[] src,int offset,int length)|写入一组指定的基本数据类型
public 缓冲区类型 slice()|创建子缓冲区,里面的一部分与原缓冲区共享数据
public 缓冲区类型 asReadOnlyBuffer()|将缓冲区设置为只读缓冲区

例:演示缓冲区操作流程
```Java
import java.nio.IntBuffer;
public class Main {
    public static void main(String[] args) throws Exception{
        // write your code here
        IntBuffer buf=IntBuffer.allocate(10);   //开辟10个大小的缓冲区
        System.out.println("写入数据之前:");
        System.out.println("position:"+buf.position()+"limit:"+buf.position()+"capacity:"+buf.capacity());
        int temp[]={5,7,9};
        buf.put(3);   //想缓冲区写入数据
        buf.put(temp);  //向缓冲区写入数组
        System.out.println("写入数据之后:");
        System.out.println("position:"+buf.position()+"limit:"+buf.position()+"capacity:"+buf.capacity());
        buf.flip();  //重设缓冲区
        System.out.println("准备输出数据时的时候:");
        System.out.println("position:"+buf.position()+"limit:"+buf.position()+"capacity:"+buf.capacity());
        System.out.println("缓存区的内容:");
        while (buf.hasRemaining()){
            int x=buf.get();
            System.out.print(x+" ");
        }
    }
}
写入数据之前:
position:0limit:0capacity:10
写入数据之后:
position:4limit:4capacity:10
准备输出数据时的时候:
position:0limit:0capacity:10
缓存区的内容:
3 5 7 9 
```
在每次写入数据后position都会变化,而当调用flip()方法时,position和limit()将同时发生变化

#### :egg:深入缓冲区操作
存在一系列状态变量,会随着写入或读取发生改变

变量名|描述
---|--:
position|表示下一个缓冲区读取或写入的操作指针,指针永远放到写入的最后一个元素之后
limit|表示还有多少数据需要存储或读取
capacity|缓冲区的最大容量
#### :egg:创建子缓冲区
子缓冲区的与原缓存区的部分数据可以共享
```Java
import java.nio.IntBuffer;
public class Main {
    public static void main(String[] args) throws Exception{
        // write your code here
        IntBuffer buf=IntBuffer.allocate(10);   //开辟10个大小的缓冲区
        IntBuffer sub=null;  //定义缓冲区对象
        for (int i=0;i<10;i++){
            buf.put(2*i+1);
        }
        buf.position(2);
        buf.limit(6);
        sub=buf.slice();  //开辟子缓冲区
        for (int i=0;i<sub.capacity();i++){
            int temp=sub.get(i);
            sub.put(temp-1);
        }
        buf.flip();
        buf.limit(buf.capacity());
        System.out.println("主缓冲区的内容");
        while (buf.hasRemaining()){
            int x=buf.get();
            System.out.print(x+" ");
        }
    }
}
主缓冲区的内容
1 3 4 6 8 10 13 15 17 19
```
#### :egg:创建只读缓冲区:asReanOnlyBuffer()方法
#### :egg:缓存直接缓冲区
只有ByteBuffer中可以创建直接缓冲区,创建直接缓冲区的ByteBuffer类定义如下方法:
```Java
public static ByteBuffer allocateDirect(int capacity)
```
例:创建直接缓冲区
```Java
import java.nio.ByteBuffer;
public class Main {
    public static void main(String[] args) throws Exception{
        // write your code here
        ByteBuffer buf=null;
        buf=ByteBuffer.allocateDirect(10); //开辟直接缓冲区
        byte temp[]={1,3,5,7,9};
        buf.put(temp);
        buf.flip();
        System.out.println("缓冲区的内容");
        while (buf.hasRemaining()){
            int x=buf.get();
            System.out.print(x+" ");
        }
    }
}
缓冲区的内容
1 3 5 7 9 
```
<p id="p1"></p>

## :hearts:通道
<a href="#title">:spades:回到目录</a><br>
通道(Channel:本身是一个接口)可以用来读取和写入数据,类似于之前的输入输出流,但是程序不会直接操作通道,所有的内容都是先读或写入到缓冲区内,在通过缓冲区取得或写入<br>

方法|描述
---|:--:
void close()|关闭通道
boolean isOpen()|判断此通道是否是打开的
#### :egg:FileChannel类
是Channel的子类,可以进行文件的读/写操作

方法|描述
---|:--:
public int read(ByteBuffer dst)|将内容读入到缓冲区中
public int write(ByteBuffer src)|将内容从缓冲区中写入到通道
public final void close()|关闭通道
public abstract MappedByteBuffer map(FileChannel.MapMode mode ,long position ,long size)|将通道的文件区域映射到内存中,同时指定映射模式,文件中的映射文件以及要映射的区域大小

例:使用输出通道输出内容
```Java
import java.io.*;
import java.nio.ByteBuffer;
import java.nio.IntBuffer;
import java.nio.channels.FileChannel;
public class Main {
    public static void main(String[] args) throws Exception{
        // write your code here
        String info[]={"guixu","ysl","www","nice"};
        File file=new File("d:"+File.separator+"out.txt");
        FileOutputStream output=new FileOutputStream(file);   //实例化输出流
        FileChannel fout=output.getChannel();  //得到输出的文件通道
        ByteBuffer buf=ByteBuffer.allocate(1024);  //开辟缓存
        for (int i=0;i<info.length;i++){
            buf.put(info[i].getBytes());
        }
        buf.flip();  //重设缓冲区,开始输出
        fout.write(buf);
        fout.close();   //关闭输出通道
        output.close();  //关闭输出流
    }
}
```
FileChannel类是双向操作的,可以同时完成输出和输入数据的功能
```Java
import java.io.*;
import java.lang.reflect.Field;
import java.nio.ByteBuffer;
import java.nio.IntBuffer;
import java.nio.channels.FileChannel;
public class Main {
    public static void main(String[] args) throws Exception{
        // write your code here
        File file1=new File("d:"+File.separator+"note.txt");
        File file2=new File("d:"+File.separator+"outnote.txt");
        FileInputStream input=new FileInputStream(file1);
        FileOutputStream output=new FileOutputStream(file2);
        FileChannel fin=input.getChannel();
        FileChannel fout=output.getChannel();
        ByteBuffer buf=ByteBuffer.allocate(1024);
        int temp=0;
        while((temp=fin.read(buf))!=-1){
            buf.flip();
            fout.write(buf);
            buf.clear();
        }
        fin.close();
        fout.close();
        input.close();
        output.close();
    }
}
```
#### :egg:内存映射
内存映射可以把文件映射到内存中,这种方式读取文件最快,要使用FileChannel类提供的map()方法
```Java
public abstract MappedByteBuffer map(FileChannel.MapMode mode ,long position ,long size)
```
```Java
import java.io.*;
import java.lang.reflect.Field;
import java.nio.ByteBuffer;
import java.nio.IntBuffer;
import java.nio.MappedByteBuffer;
import java.nio.channels.FileChannel;
public class Main {
    public static void main(String[] args) throws Exception{
        // write your code here
        File file=new File("d:"+File.separator+"text.txt");
        FileInputStream input=new FileInputStream(file);
        FileChannel fin=input.getChannel();
        MappedByteBuffer mbb=fin.map(FileChannel.MapMode.READ_ONLY,0,file.length());  //声明文件的内存映射
        byte data[]=new byte[(int)file.length()];
        int foot=0;
        while (mbb.hasRemaining()){
            data[foot++]=mbb.get();
        }
        System.out.println(new String(data));
        fin.close();
        input.close();
    }
}
```
不使用此方法写入数据
<p id="p1"></p>

## :hearts:文件锁FileLock类
<a href="#title">:spades:回到目录</a><br>
#### :egg:
<p id="p1"></p>

## :hearts:字符集Charset类
<a href="#title">:spades:回到目录</a><br>
#### :egg:<p id="p1"></p>

## :hearts:Selector类
<a href="#title">:spades:回到目录</a><br>
#### :egg:

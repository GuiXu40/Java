# ---------------------Java数据库编程-------------------------
<p id="title"></p>

## 目录
<a href="#p1">:diamonds:JDBC操作步骤</a><br>
<a href="#p2">:diamonds:连接数据库</a><br>
<a href="#p3">:diamonds:数据库的更新操作</a><br>
<a href="#p4">:diamonds:ResultSet接口</a><br>
<a href="#p5">:diamonds:PrepareStatement接口</a><br>
<a href="#p6">:diamonds:处理大数据对象</a><br>
<a href="#p7">:diamonds:CallableStatement接口</a><br>
<a href="#p8">:diamonds:JDBC2.0操作</a><br>
<a href="#p9">:diamonds:使用元数据分析数据库</a><br>
<a href="#p10">:diamonds:使用JDBC连接Oracle</a><br>
<p id="p1"></p>

## :hearts:JDBC操作步骤
<a href="#title">:spades:回到目录</a><br>
+ 加载数据库驱动程序: 各个数据库都会提供JDBC的驱动程序开发包,配置到CLASSPATH路径即可
+ 连接数据库: 连接不同的数据库所使用连接语言不同
+ 使用语句进行数据库操作: 分为更新和查询两种操作,各个数据库也可以使用自己提供的各种命令
+ 关闭数据库连接: 需要关闭连接以释放各种资源

<p id="p2"></p>

## :hearts:连接数据库
<a href="#title">:spades:回到目录</a><br>
#### :egg:配置mysql数据库的驱动
**注意**:在下载mysql的时候,最好下载那些网络中有教程的,比较好纠错<br>
第一步,需要将mysql的驱动包放在**你的安装目录\jre\lib\ext**文件夹下
#### :egg:连接及关闭数据库
通过DriverManager类连接数据库

方法|描述
---|:--:
public static Connection getConnection(String url)|通过连接地址连接数据库
public static Connection getConnection(String url,String user,String password)|同时输入用户名和密码

连接地址格式
```Java
jdbc:mysql://IP 地址:端口号/数据库名称
```
连接数据库
```Java
package com.company;//导入包
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

/*
 * 数据库连接
 */
public class Main {
    public static void main(String[] args) {
        Connection con;
        //jdbc驱动
        String driver="com.mysql.cj.jdbc.Driver";
        //这里我的数据库是text
        String url="jdbc:mysql://localhost:3306/text?&useSSL=false&serverTimezone=UTC";
        String user="root";   //用户名
        String password="root";   //密码
        try {
            //注册JDBC驱动程序
            Class.forName(driver);
            //建立连接
            con = DriverManager.getConnection(url, user, password);
            if (!con.isClosed()) {
                System.out.println("数据库连接成功");
            }

            con.close();   //必须要
        } catch (ClassNotFoundException e) {
            System.out.println("数据库驱动没有安装");

        } catch (SQLException e) {
            e.printStackTrace();
            System.out.println("数据库连接失败");
        }
    }
}
```
**数据库打开之后必须关闭**
<p id="p3"></p>

## :hearts:数据库的更新操作
<a href="#title">:spades:回到目录</a><br>
Statement接口的常用方法

方法|描述
---|:--:
int executeUpdate(String sql)|执行数据库的增,删,改
ResultSet executeQuery(String sql)|执行数据库查询操作,返回一个结果集对象
void addBatch(String sql)|增加一个待执行的SQL语句f
int[] executeBatch()|批量执行SQL语句
void close()|关闭Statement操作(必须要)
boolean execute(String sql)|执行SQL语句
#### :egg:插入
```Java
package com.company;//导入包
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

/*
 * 数据库连接
 */
public class Main {
    public static void main(String[] args) {
        Connection con;   //数据库连接
        Statement stmt = null;  //数据库操作
        //jdbc驱动
        String driver="com.mysql.cj.jdbc.Driver";
        //这里我的数据库是text
        String url="jdbc:mysql://localhost:3306/text?&useSSL=false&serverTimezone=UTC";
        String user="root";
        String password="root";

        //操作语句SQL
        String name="百度";
        String url1="www.baidu.com";
        int alexa=18;
        String country="成都";
        String sql="INSERT INTO websites(name,url,alexa,country)"+"VALUES('"+name+"','"+url1+"',"+alexa+",'"+country+"')";
        try {
            //注册JDBC驱动程序
            Class.forName(driver);
            //建立连接
            con = DriverManager.getConnection(url, user, password);
            if (!con.isClosed()) {
                System.out.println("数据库连接成功");
            }
            stmt=con.createStatement();  //实例化statement对象
            stmt.executeUpdate(sql);
            stmt.close();
            con.close();
        } catch (ClassNotFoundException e) {
            System.out.println("数据库驱动没有安装");

        } catch (SQLException e) {
            e.printStackTrace();
            System.out.println("数据库连接失败");
        }
    }
}
```
**在jdbc中一般按照顺序关闭**
#### :egg:修改
```Java
package com.company;//导入包
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

/*
 * 数据库连接
 */
public class Main {
    public static void main(String[] args) {
        Connection con;   //数据库连接
        Statement stmt = null;  //数据库操作
        //jdbc驱动
        String driver="com.mysql.cj.jdbc.Driver";
        //这里我的数据库是text
        String url="jdbc:mysql://localhost:3306/text?&useSSL=false&serverTimezone=UTC";
        String user="root";
        String password="root";

        //操作语句SQL
        int id=1;
        String name="百度";
        String url1="www.baidu.com";
        int alexa=18;
        String country="成都";
        String sql="UPDATE websites SET name='"+name+"',url='"+url1+"',alexa="+alexa+",country='"+country+"'Where id="+id;
        try {
            //注册JDBC驱动程序
            Class.forName(driver);
            //建立连接
            con = DriverManager.getConnection(url, user, password);
            if (!con.isClosed()) {
                System.out.println("数据库连接成功");
            }
            stmt=con.createStatement();  //实例化statement对象
            stmt.executeUpdate(sql);
            stmt.close();
            con.close();
        } catch (ClassNotFoundException e) {
            System.out.println("数据库驱动没有安装");

        } catch (SQLException e) {
            e.printStackTrace();
            System.out.println("数据库连接失败");
        }
    }
}
```
#### :egg:删除
```Java
package com.company;//导入包
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

/*
 * 数据库连接
 */
public class Main {
    public static void main(String[] args) {
        Connection con;   //数据库连接
        Statement stmt = null;  //数据库操作
        //jdbc驱动
        String driver="com.mysql.cj.jdbc.Driver";
        //这里我的数据库是text
        String url="jdbc:mysql://localhost:3306/text?&useSSL=false&serverTimezone=UTC";
        String user="root";
        String password="root";

        //操作语句SQL
        int id=1;
//        String name="百度";
//        String url1="www.baidu.com";
//        int alexa=18;
//        String country="成都";
        String sql="DELETE FROM websites WHERE id="+id;
        try {
            //注册JDBC驱动程序
            Class.forName(driver);
            //建立连接
            con = DriverManager.getConnection(url, user, password);
            if (!con.isClosed()) {
                System.out.println("数据库连接成功");
            }
            stmt=con.createStatement();  //实例化statement对象
            stmt.executeUpdate(sql);
            stmt.close();
            con.close();
        } catch (ClassNotFoundException e) {
            System.out.println("数据库驱动没有安装");

        } catch (SQLException e) {
            e.printStackTrace();
            System.out.println("数据库连接失败");
        }
    }
}
```
<p id="p4"></p>

## :hearts:ResultSet接口
<a href="#title">:spades:回到目录</a><br>
查询记录将使用ResultSet进行接收,并使用ResultSet取得内容,实际上是保存在内存中,所以查询出来的数据总量过大,则系统将报错

方法|描述
---|:--:
boolean next() |将指针移到下一行
int getInt(int columnIndex)|取得整形数据
int getString(String columnIndex)|取得字符串数据
int getFloat(int columnIndex)|以float类型储存数据
int getString(int columnIndex)|以String类型储存数据
Date getDate(int ..)|以date类型储存
从表中查询数据
```Java
package com.company;//导入包
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

/*
 * 数据库连接
 */
public class Main {
    public static void main(String[] args) {
        Connection con;   //数据库连接
        Statement stmt = null;  //数据库操作
        ResultSet rs=null;  // 保存查询结果
        //jdbc驱动
        String driver="com.mysql.cj.jdbc.Driver";
        //这里我的数据库是text
        String url="jdbc:mysql://localhost:3306/text?&useSSL=false&serverTimezone=UTC";
        String user="root";
        String password="root";

        //操作语句SQL

        String sql="SELECT id,name,url,alexa,country FROM websites";
        try {
            //注册JDBC驱动程序
            Class.forName(driver);
            //建立连接
            con = DriverManager.getConnection(url, user, password);
            if (!con.isClosed()) {
                System.out.println("数据库连接成功");
            }
            stmt=con.createStatement();  //实例化statement对象
            rs=stmt.executeQuery(sql);
            while (rs.next()){
                int id=rs.getInt("id");  //取得ID内容
                String url1=rs.getString("url");
                int alexa=rs.getInt("alexa");
                String country=rs.getString("country");
                System.out.println("id:"+id+",url:"+url1+",alexa:"+alexa+",country"+country);
            }
            rs.close();
            stmt.close();
            con.close();
        } catch (ClassNotFoundException e) {
            System.out.println("数据库驱动没有安装");

        } catch (SQLException e) {
            e.printStackTrace();
            System.out.println("数据库连接失败");
        }
    }
}
```
也可以按照值的顺序采用编号
```Java
            while (rs.next()){
                int id=rs.getInt(1);  //取得ID内容
                String url1=rs.getString(2);
                int alexa=rs.getInt(3);
                String country=rs.getString(4);
                System.out.println("id:"+id+",url:"+url1+",alexa:"+alexa+",country"+country);
            }
```
<p id="p5"></p>

## :hearts:PrepareStatement接口
<a href="#title">:spades:回到目录</a><br>
#### :egg:简介
是Statement的子接口,属于**预处理操作**,在PrepareStatement中的SQL语句中,具体的内容是采用占位符形式的

方法|描述
---|:--:
int executeUpdate()|执行设置的预处理操作
void setInt(int parameterIndex,int x)|指定要设置的索引编号,并设置整数内容
void setString(int parameterIndex,int x)|指定要设置的索引编号,并设置字符串内容
void setFloat(int parameterIndex,int x)|指定要设置的索引编号,并设置浮点数内容
void setDate(int parameterIndex,int x)|指定要设置的索引编号,并设置Java.sql.Date类型内容

**注意**: setDate()方法使用的是java.sql.Date类型,而不是java.util.Date,所以要将一个java.util.Date类型的内容变为java.sql.Date
```java
String birthday="2019-9-2";
java.util.Date.temp=null;  //声明一个date对象
//通过SimpleDateFormat类将一个字符串变为java.util.Date类型
temp=SimpleDateFormat("yyyy-MM-dd").parse(birthday);
java.sql.Date bir=new java.util.Date(temp.getTime());
```
#### :egg:使用
完成数据插入操作
```Java
package com.company;//导入包
import java.sql.*;

/*
 * 数据库连接
 */
public class Main {
    //jdbc驱动
    public static final String driver="com.mysql.cj.jdbc.Driver";
    //这里我的数据库是text
    public static final String url="jdbc:mysql://localhost:3306/text?&useSSL=false&serverTimezone=UTC";
    public static final String user="root";
    public static final String password="root";
    public static void main(String[] args) {
        Connection con;   //数据库连接
        PreparedStatement pstmt = null;  //数据库操作
        //操作语句SQL
        String name="google";
        String url1="www.google.com";
        int alexa=45;
        String country="American";
        String sql= "INSERT INTO websites(name,url,alexa,country)"+"VALUES(?,?,?,?)";  //编写预处理SQL
        try {
            //注册JDBC驱动程序
            Class.forName(driver);
            //建立连接
            con = DriverManager.getConnection(url, user, password);
            if (!con.isClosed()) {
                System.out.println("数据库连接成功");
            }
            pstmt=con.prepareStatement(sql);  //实例化preparedStatement对象
            pstmt.setString(1,name);
            pstmt.setString(2,url1);
            pstmt.setInt(3,alexa);
            pstmt.setString(4,country);
            pstmt.executeUpdate();
            pstmt.close();
            con.close();
        } catch (ClassNotFoundException e) {
            System.out.println("数据库驱动没有安装");

        } catch (SQLException e) {
            e.printStackTrace();
            System.out.println("数据库连接失败");
        }
    }
}
```

<p id="p6"></p>

## :hearts:处理大数据对象
<a href="#title">:spades:回到目录</a><br>
在CLOB中储存大量文字,在BLOG中可以储存二进制文件,如图片,电影等<br>
PrepareStatement提供了如下表的方法,专门用于写入大数据对象

方法|描述
---|:--:
void setAsciiStream(int parameterIndex,InputStream,int length)|将指定的输入流写入数据库的文本字段
void setBinaryStream(int parameterIndex,InputStream x,int length)|将二进制的输入流数据写入二进制字段中

ResultSet中提供了方法用于读出大对象数据

方法|描
---|:--:
InputStream getAsciiStream(int columnIndex)|根据列的编号返回大对象的文本输入流
InputStream getAsciiStream(String columnName)|根据列的名称返回大对象的文本输入流
Clob getClob(String colName)|根据列名称返回Clob数据
InputStream getBinaryStream(int columnIndex)|根据列的编号,返回二进制数据
InputStream getBinaryStream(String columnName)|根据列的名称,返回二进制数据
Blob getBlob(int i)|根据列的编号,返回Blob数据
Blob getBlob(String colName)|根据列名称,返回blob的数据

#### :egg:处理CLOB数据
#### :egg:处理BLOB数据
<p id="p7"></p>

## :hearts:CallableStatement接口
<a href="#title">:spades:回到目录</a><br>
<p id="p8"></p>

## :hearts:JDBC2.0操作
<a href="#title">:spades:回到目录</a><br>
#### :egg:可滚动的结果集
#### :egg:使用结果插入数据
#### :egg:使用结果更新数据
#### :egg:使用结果删除数据
#### :egg:批处理
<p id="p9"></p>

## :hearts:使用元数据分析数据库
<a href="#title">:spades:回到目录</a><br>
#### :egg:DatabaseMetaData
#### :egg:ResultSetMetaData
<p id="p10"></p>

## :hearts:使用JDBC连接Oracle
<a href="#title">:spades:回到目录</a><br>
#### :egg:

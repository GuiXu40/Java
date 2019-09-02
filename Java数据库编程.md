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
#### :egg:使用
<p id="p6"></p>

## :hearts:处理大数据对象
<a href="#title">:spades:回到目录</a><br>
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

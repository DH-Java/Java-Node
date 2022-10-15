

# **一、JDBC是什么？**

JAVA DataBase Connectivity（JAVA语言连接数据库）

2、JDBC的本质是什么？

JDBC是SUN公司指定的一套接口（interface）

java.sql.*；(这个软件包下有很多接口)

接口都有调用者和实现者

面向接口调用、面向接口写实现类，这都属于面向接口编程。

为什么要面向接口编程？

解耦合：降低程序的耦合度，提高程序的扩展力。

多态机制就是非常典型的：面向抽象编程。（不要面向具体编程）

建议：

Animal a = new Cat();

Animal b = new Dog();

思考：为什么SUN制定一套JDBC接口呢？

因为每一个数据库的底层实现都不一样

Oracle数据库有自己的原理

MySQL数据库也有自己的原理

MS SqlServer数据库也有自己的原理。

...

每一个数据库产品都有自己独特的实现原理。

JDBC的本质是：一套接口；



# **二、JDBC编程六步（需要背会）**

1、注册驱动 （作用：告诉JAVA程序，即将要连接的是那个品牌的数据库）

2、获取连接 （表示JVM的进程和数据库进程之间的通道打开，这属于进程之间的通信，重要级的，使用完之后一定关闭资源 ）

3、获取数据库操作对象（专门执行sql语句的对象）

4、执行SQL语句 (DQL DML...)

5、处理查询结果集（只有当第四步执行的是select语句的时候，才有这第五步处理查询结果集）

6、释放资源（使用完资源之后一定要关闭资源。JAVA和数据库属于进程间的通信，开启之后一定要关闭。）

```java
package com.dx;

import java.sql.*;

public class JDBCTest01{
    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        try {
            //1、注册驱动
            Driver driver = new com.mysql.jdbc.Driver();//多态，父类型引用指向子类型对象。
            //Driver driver= new oracle.jdbc.driver.OracleDriver();//oracle的驱动
            //使用 DriverManager注册给定的驱动程序。static void           

            DriverManager.registerDriver(driver);
            //2、获取连接
            /*
            * URL:统一资源定位符（网络中某个资源的绝对路径）
            *jdbc:mysql://协议
            * 127.0.0.1 IP地址
            * 3306 mysql数据库端口号
            * MySQL 具体的数据库实例名
            *
            * 说明：localhost 和127.0.0.1都是本机ip地址
            *
            * 什么是通信协议，有什么用？
            *   通信协议是通信之前就提前定好的数据传送格式。
            *   数据包具体怎么传数据，格式都是提前定好的。
            * */
            String url = "jdbc:mysql://127.0.0.1:3306/MySQL";
            String user = "root";
            String password = "020903";
            //尝试建立与给定数据库URL的连接 static
            connection = DriverManager.getConnection(url,user,password);
            //com.mysql.cj.jdbc.ConnectionImpl@5123a213
            System.out.println("数据库连接对象="+connection);
            //3、获取数据库操作对象（Statement专门执行sql语句的）
            //创建一个Statement对象，用于将SQL语句发送到数据库。
            statement = connection.createStatement();
            //4、执行sql
            String sql = "insert into xs.students (stu_num,stu_name,stu_sex,stu_age,stu_pho,stu_qq) value('2061','邓杰','男','19','17388225738','1829908112')";
            //执行DML语句（insert delete update）执行给定的SQL语句，这可能是 INSERT ， UPDATE ，或 DELETE声明，或者不返回任何内容，如SQL DDL语句的SQL语句。
            int executeLargeUpdate = statement.executeUpdate(sql);
            System.out.println(executeLargeUpdate==1?"保存成功":"保存失败");
            //5、处理查询结果集
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            //6、释放资源
            //为了保证资源一定释放，在finally语句中关闭资源
            //并且要遵循从小到大依次关闭
            //分别对其try...catch
            if (statement != null){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (connection !=null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## **1.1.1JDBC完成delete，update操作**

```java
package com.dx;
//JDBC完成delete，update
import java.sql.*;
public class JDBCTest02 {
    public static void main(String[] args) {
        Connection connection=null;
        Statement statement = null;
        try {
            //1、注册连接
            DriverManager.registerDriver(new com.mysql.cj.jdbc.Driver());
            //2、获取链接
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/MySQL","root","020903");
            System.out.println("数据库连接成功");
            //3、获取数据库操作对象
            statement = connection.createStatement();
            //4、执行SQL语句
            int i = statement.executeUpdate("delete from xs.students where stu_sex='女'");//删除
            System.out.println(i==1?"保存成功":"保存失败");
            int j = statement.executeUpdate("update xs.students set stu_name='张杰' where stu_id ='20602061'");//修改
            System.out.println(j==1?"保存成功":"保存失败");
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            //6、关闭资源
            if (statement !=null){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (connection!=null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
```

## **1.1.2利用反射机制完成注册驱动的第二种方式**

```java
package com.dx;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class JDBCTest03 {
    public static void main(String[] args) {
        try {
//            try {
//                这是注册驱动的第一种写法
//               DriverManager.registerDriver(new com.mysql.cj.jdbc.Driver());
//            } catch (SQLException e) {
//                e.printStackTrace();
//            }
            //第二种注册驱动的方式：常用的。
            //为什么这种方式常用？因为参数是一个字符串，字符串可以写到xxx.properties文件中。
            //以下方法不需要接收返回值，因为我们只想用他的类加载动作。
            Class.forName("com.mysql.cj.jdbc.Driver");
            try {
                Connection root = DriverManager.getConnection("jdbc:mysql://localhost:3306/MySQL", "root", "020903");
                System.out.println("连接到数据库"+root);
            } catch (SQLException e) {
                e.printStackTrace();
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

## **1.1.3将连接数据库中的所有信息配置到配置文件中**

```java
package com.dx;
//实际开发中不建议把连接数据库的信息写到java程序中。
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ResourceBundle;

//将连接数据库中的所有信息配置到配置文件中
public class JDBCTest04 {
    public static void main(String[] args) {
        //使用资源绑定器绑定属性配置文件
        ResourceBundle bundle = ResourceBundle.getBundle("JDBC");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");



        Statement statement = null;
        Connection root = null;
        try {
            Class.forName(driver);
            try {
                root = DriverManager.getConnection(url, user,password);
                statement = root.createStatement();
                int i = statement.executeUpdate("delete from xs.students where stu_sex='女'");//删除
                System.out.println(i==1?"保存成功":"保存失败");

            } catch (SQLException e) {
                e.printStackTrace();
            }

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }finally {
            if (statement !=null){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (root!=null){
                try {
                    root.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## **1.1.4处理查询结果集**

```java
package com.dx;

import javax.xml.transform.Result;
import java.sql.*;
import java.util.ResourceBundle;

//处理查询结果集
public class JDBCTest05 {
    public static void main(String[] args) {
        ResourceBundle bundle = ResourceBundle.getBundle("JDBC");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");


        Statement statement = null;
        Connection connection = null;
        ResultSet rs = null;
        //1、注册驱动
        try {
            Class.forName(driver);
            try {
                //2、获取连接
                connection = DriverManager.getConnection(url, user, password);
                System.out.println("数据库连接成功"+connection);
                //3、获取数据库操作对象
                statement = connection.createStatement();
                //4、执行SQL对象
                // int  executeUpdate(insert/delete/update)
                // ResultSet executeQuery专门执行DQL语句的方法
                 rs = statement.executeQuery("select stu_name,stu_age from xs.students");

                //5、处理查询结果集
              while (rs.next()){
                  String stu_name = rs.getString("stu_name");
                  int stu_age = rs.getInt("stu_age");
                  System.out.println(stu_name+stu_age);
              }


            } catch (SQLException e) {
                e.printStackTrace();
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }finally {
            //6、释放资源
            if (rs!=null){
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (statement!=null){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (connection!=null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }





    }
}
```

## **1.1.5properties**

```properties
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/MySQL
user=root
password=020903
```

# **三、模拟用户登录功能的实现。**

\* 实现功能：

\*       1、需求：模拟用户登录功能的实现。

\*       2、业务描述：

\*               程序运行的时候，提供一个输入的入口，可以让用户输入用户名和密码

\*               用户输入用户名和密码之后，提交信息，java程序收集到用户信息

\*               java程序连接数据库验证用户名和密码是否合法

\*               合法：显示登陆成功

\*               不合法：显示登陆失败

\*       3、数据的准备：

\*            在实际开发中，表的设计会使用专业的建模工具，我们这里安装一个建模工具：PowerDesigner

\*            使用PD工具来进行数据库表的设计。(参见table_user.sql脚本)

\*       4、当前程序存在的问题：

\*           请输入账户

\*            fdsa

\*           请输入密码

\*           fdsa ' or '1'='1

\*           登陆成功

\*           这种现象被称为SQL注入（安全隐患）。（黑客经常使用）

\*       5、导致SQL注入的根本原因是什么？

\*           用户输出的信息含有sql关键字，并且这些关键字参与sql语句的编译过程，

\*           导致sql语句的原意被扭曲，进而达到sql注入

\* */

```java
package com.dx;

import java.sql.*;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

/*
* 实现功能：
*       1、需求：模拟用户登录功能的实现。
*       2、业务描述：
*               程序运行的时候，提供一个输入的入口，可以让用户输入用户名和密码
*               用户输入用户名和密码之后，提交信息，java程序收集到用户信息
*               java程序连接数据库验证用户名和密码是否合法
*               合法：显示登陆成功
*               不合法：显示登陆失败
*       3、数据的准备：
*            在实际开发中，表的设计会使用专业的建模工具，我们这里安装一个建模工具：PowerDesigner
*            使用PD工具来进行数据库表的设计。(参见table_user.sql脚本)
*       4、当前程序存在的问题：
*           请输入账户
*            fdsa
*           请输入密码
*           fdsa ' or '1'='1
*           登陆成功
*           这种现象被称为SQL注入（安全隐患）。（黑客经常使用）
*       5、导致SQL注入的根本原因是什么？
*           用户输出的信息含有sql关键字，并且这些关键字参与sql语句的编译过程，
*           导致sql语句的原意被扭曲，进而达到sql注入
* */
public class JDBCTest06 {
    public static void main(String[] args) {
        Map<String,String> userLoginInfo = initUI();
        boolean loginSuccess = Login(userLoginInfo);
        System.out.println(loginSuccess ? "登陆成功":"登陆失败");
    }
    /*用户登录
    *@Author:DH
    *@Date:2021/11/16 14:03
    *@Description:TODO
    ** @param 用户登录信息
    *@return:true 登陆成功 false 登陆失败
    */
    private static boolean Login(Map<String, String> userLoginInfo) {
        //打上标记
        boolean loginSuccess = false;
        //单独定义变量
        String loginName = userLoginInfo.get("loginName");
        String loginPassword = userLoginInfo.get("loginPassword");
        //JDBC代码
        ResultSet resultSet = null;
        Statement statement = null;
        Connection connection = null;
        try {
            //1、注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            //2、获取连接
            connection =DriverManager.getConnection("jdbc:mysql://localhost:3306/MySQL","root","020903");
            //3、获取数据库操作对象
            statement = connection.createStatement();
            //4、执行sql语句
            String sql = "select * from xs.table_user where user='"+loginName+"'and password='"+loginPassword+"'";
            resultSet = statement.executeQuery(sql);
            //5、处理查询结果集
            if (resultSet.next()){
              //登陆成功
                loginSuccess = true;
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //6、释放资源
            if (resultSet != null){
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (statement != null){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (connection != null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }



        return loginSuccess;
    }

    /*
    *@Author:DH
    *@Date:2021/11/16 13:53
    *@Description:TODO
    ** @param
    *@return:java.util.Map<java.lang.String,java.lang.String>
    初始化页面方法
    * 用户输入的账户和密码*/
    private static Map<String, String> initUI() {
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入账户");
        String username = scanner.nextLine();
        System.out.println("请输入密码");
        String password = scanner.nextLine();
        Map<String ,String> useLoginInfo = new HashMap<>();
        useLoginInfo.put("loginName",username);
        useLoginInfo.put("loginPassword",password);
        return useLoginInfo;
    }
}

```

# **四、解决sql注入问题**

\*       只要用户提供的信息不参与SQL语句的编译过程，问题就解决了。

\*       即使用户提供的信息中含有SQL语句的关键字，但是没有参与编译，不起作用。

\*       要想用户信息不参与SQL语句的编译，那么必须使用java.sql.PreparedStatement

\*       PreparedStatement接口继承了java.sql.Statement

\*       PreparedStatement是属于预编译的数据库操作对象。

\*       PreparedStatement的原理是：预先对SQL语句的框架进行编译，然后再给SQL语句传“值”

\* 2、测试结果：

\*          请输入账户

​           fdsa

​           请输入密码

​           fdsa ‘ or ’1'='1

​           登陆失败

  3、解决SQL注入的关键是什么?

  \* 用户提供的信息中即使含有sql语句的关键字，但是这些关键字并没有参与编译。不起作用。

*

*/

```java
package com.dx;

import java.sql.*;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;
/*
*@Author:DH
*@Date:2021/11/16 14:54
*@Description:TODO
** @param null
*@return:
* 1、解决sql注入问题
*       只要用户提供的信息不参与SQL语句的编译过程，问题就解决了。
*       即使用户提供的信息中含有SQL语句的关键字，但是没有参与编译，不起作用。
*       要想用户信息不参与SQL语句的编译，那么必须使用java.sql.PreparedStatement
*       PreparedStatement接口继承了java.sql.Statement
*       PreparedStatement是属于预编译的数据库操作对象。
*       PreparedStatement的原理是：预先对SQL语句的框架进行编译，然后再给SQL语句传“值”
* 2、测试结果：
*          请输入账户
           fdsa
           请输入密码
           fdsa ‘ or ’1'='1
           登陆失败
  3、解决SQL注入的关键是什么?
  * 用户提供的信息中即使含有sql语句的关键字，但是这些关键字并没有参与编译。不起作用。
*
*/


    public class JDBCTest07 {
        public static void main(String[] args) {
            Map<String,String> userLoginInfo = initUI();
            boolean loginSuccess = Login(userLoginInfo);
            System.out.println(loginSuccess ? "登陆成功":"登陆失败");
        }
        /*用户登录
         *@Author:DH
         *@Date:2021/11/16 14:03
         *@Description:TODO
         ** @param 用户登录信息
         *@return:true 登陆成功 false 登陆失败
         */
        private static boolean Login(Map<String, String> userLoginInfo) {
            //打上标记
            boolean loginSuccess = false;
            //单独定义变量
            String loginName = userLoginInfo.get("loginName");
            String loginPassword = userLoginInfo.get("loginPassword");
            //JDBC代码
            ResultSet resultSet = null;
            PreparedStatement Ps= null;//这里使用PreparedStatement（预编译的数据库操作对象）
            Connection connection = null;
            try {
                //1、注册驱动
                Class.forName("com.mysql.cj.jdbc.Driver");
                //2、获取连接
                connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/MySQL","root","020903");
                //3、获取预编译的数据库操作对象
                //SQL语句的框子，其中一个问号表示一个占位符，一个?将来接收一个”值“，注意占位符不能使用单引号括起来。
                String sql = "select * from xs.table_user where user= ? and password = ? ";
                //程序执行到此处，会发送sql语句框子给DBMS，然后DBMS进行sql语句的预先编译
                Ps= connection.prepareStatement(sql);
                //给占位符?传值（第一个问号下标是1,第二个问号下标是2，JDBC所有下标都是从1开始）
                Ps.setString(1,loginName);
                Ps.setString(2,loginPassword);
                //4、执行sql语句
                resultSet = Ps.executeQuery();
                //5、处理查询结果集
                if (resultSet.next()){
                    //登陆成功
                    loginSuccess = true;
                }
            } catch (Exception e) {
                e.printStackTrace();
            }finally {
                //6、释放资源
                if (resultSet != null){
                    try {
                        resultSet.close();
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }
                }
                if (Ps != null){
                    try {
                        Ps.close();
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }
                }
                if (connection != null){
                    try {
                        connection.close();
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }
                }
            }



            return loginSuccess;
        }

        /*
        *@Author:DH
        *@Date:2021/11/16 13:53
        *@Description:TODO
        ** @param
        *@return:java.util.Map<java.lang.String,java.lang.String>
        初始化页面方法
        * 用户输入的账户和密码*/
        private static Map<String, String> initUI() {
            Scanner scanner = new Scanner(System.in);
            System.out.println("请输入账户");
            String username = scanner.nextLine();
            System.out.println("请输入密码");
            String password = scanner.nextLine();
            Map<String ,String> useLoginInfo = new HashMap<>();
            useLoginInfo.put("loginName",username);
            useLoginInfo.put("loginPassword",password);
            return useLoginInfo;
        }
    }



```

## **1.1.1Statment案例**

```java
package com.dx;

import java.sql.*;
import java.util.Scanner;
/*
*@Author:DH
*@Date:2021/11/16 20:34
*@Description:TODO
** @param null
*@return:
* 用户在控制台输入desc就是降序，输出asc就是升序
*/        
public class JDBCTest08 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输出asc/desc asc表示升序，desc表示降序");
        System.out.println("请输入");
        String s = scanner.nextLine();


        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/MySQL","root","020903");
            statement = connection.createStatement();
            String sql = "select * from xs.table_user order by id "+s+"";
            resultSet = statement.executeQuery(sql);
            while (resultSet.next()){
                String id = resultSet.getString("id");
                String user = resultSet.getString("user");
                String password = resultSet.getString("password");
                String name = resultSet.getString("name");
                System.out.println(id+user+password+name);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            if (resultSet!=null){
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (statement!=null){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (connection!=null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```

## **1.1.2使用PreparedStatement进行增删改操作**

```java
package com.dx;

import java.sql.*;

public class JDBCTest09 {
    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement PS = null;
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/MySQL","root","020903");
//            添加数据
//            String sql = "insert into xs.table_user (id,user ,password,name ) values (?,?,?,?)";
//            PS = connection.prepareStatement(sql);
//            PS.setInt(1,20902090);
//            PS.setString(2,"做梦做到手起茧");
//            PS.setString(3,"123456");
//            PS.setString(4,"老李");
//            修改数据
//            String sql = "update xs.table_user set user = ? , password = ? where id = ?";
//            PS = connection.prepareStatement(sql);
//            PS.setString(1,"草泥马");
//            PS.setString(2,"987654");
//            PS.setInt(3,30303030);
//            删除数据
            String sql = "delete from xs.table_user where id=?";
            PS = connection.prepareStatement(sql);
            PS.setInt(1,20902090);
            int count = PS.executeUpdate();
            System.out.println(count==1? "保存成功":"保存失败");
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            if (PS!=null){
                try {
                    PS.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (connection!=null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

#  **五、****JDBC事务机制**

```java
package com.dx;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
/*
*@Author:DH
*@Date:2021/11/16 23:01
*@Description:TODO
** @param null
*@return:
* JDBC事务机制：
*       1、JDBC的事务是自动提交的，什么是自动提交？
*           只要执行任意一条DML语句，则自动提交一次。这是JDBC默认的事务行为。
*           但是在实际的业务当中，通常都是n条DML语句共同联合才能完成的，必须
*           保证他们这些DML语句在同一个事务中同时成功或同时失败
*       2、以下程序先来验证一下JDBC的事务是否是自动提交机制！
*/        
public class JDBCTest10 {
    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement PS = null;
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/MySQL","root","020903");
            //删除数据
            String sql = "update xs.students set user = ? where name = ?";
            PS = connection.prepareStatement(sql);
            PS.setString(1,"去你妈的");
            PS.setString(2,"老李");
            int count = PS.executeUpdate();
            System.out.println(count==1? "保存成功":"保存失败");

            PS.setString(1,"操你妈");
            PS.setString(2,"老杜");
            count = PS.executeUpdate();
            System.out.println(count==1? "保存成功":"保存失败");
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            if (PS!=null){
                try {
                    PS.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (connection!=null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## **1.1.1、重点三行代码**

* connection.setAutoCommit(false);停止事务提交

* connection.commit();//提交事务

* connection.rollback();回滚

```java
package com.dx;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
/*
* 重点三行代码
*       connection.setAutoCommit(false);停止事务提交
*       connection.commit();//提交事务
*       connection.rollback();回滚
* */
public class JDBCTest11 {
    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement PS = null;
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/MySQL","root","020903");
            connection.setAutoCommit(false);//开启事务
            String sql = "update xs.t_act set balance = ? where actno = ?";
            PS = connection.prepareStatement(sql);
            PS.setDouble(1,10000);
            PS.setInt(2,111);
            int count = PS.executeUpdate();

//            String s = null;
//            s.toString();


            PS.setDouble(1,10000);
            PS.setInt(2,222);
            count += PS.executeUpdate();

            connection.commit();//程序能走到这，说明以上程序没有异常，事务结束，手动提交数据
        } catch (Exception e) {
            //回滚事务
            e.printStackTrace();
            try {
                connection.rollback();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
        }finally {
            if (PS!=null){
                try {
                    PS.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (connection!=null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```

# **六、工具类和模糊查询**

\* 这个程序两个任务：

\*       第一：测试DBUtils是否好用

\*       第二：模糊查询怎么写

\* */

```java
package com.dx;

import utils.DBUtil;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
/*
* 这个程序两个任务：
*       第一：测试DBUtils是否好用
*       第二：模糊查询怎么写
* */

public class JDBCTest12 {
    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement ps = null;
        ResultSet resultSet = null;

        try {
            //获取连接
            connection = DBUtil.getConnection();
            //获取预编译的数据库操作对象
            String sql = "select actno from xs.t_act where balance like ?";
            ps=connection.prepareStatement(sql);
            ps.setString(1,"%1%");
            resultSet = ps.executeQuery();
            while (resultSet.next()){
                System.out.println(resultSet.getString("actno"));
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //释放资源
            DBUtil.Close(connection,ps,resultSet);
        }
    }
}
```

# **七、悲观锁for update**

```java
package com.dx;

import utils.DBUtil;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

//悲观锁
public class JDBCTest13 {
    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement ps = null;
        ResultSet resultSet = null;
        try {
            connection=DBUtil.getConnection();
            connection.setAutoCommit(false);
            String sql = "select actno , balance from xs.t_act where t_name like ? for update ";
            ps = connection.prepareStatement(sql);
            ps.setString(1,"老%");
            resultSet = ps.executeQuery();
            while (resultSet.next()){
                System.out.println(resultSet.getString("actno")+"  =  "+resultSet.getString("balance"));
            }
            connection.commit();
        } catch (SQLException e) {
            try {
                connection.rollback();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
            e.printStackTrace();
        }finally {
            DBUtil.Close(connection,ps,resultSet);
        }
    }
}
```

```java
package com.dx;

import utils.DBUtil;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class JDBCTet14 {
    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement ps = null;


        try {
            connection = DBUtil.getConnection();
            String sql = "update xs.t_act set actno = actno + 20 where t_name like ?";
            ps=connection.prepareStatement(sql);
            ps.setString(1,"%老%");
            int i = ps.executeUpdate();
            System.out.println(i);
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            DBUtil.Close(connection,ps,null);
        }
    }
}
```


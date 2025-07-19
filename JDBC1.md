# JDBC

### 基础篇

##### 数据的存储

Java SE中的I/O流技术中，存储在本地磁盘中，虽然解决了持久化问题，但是没有结构和逻辑，不方便管理和维护。

所以引入关系型数据库进行对数据的存储。

##### 数据的操作

<img src="C:\Users\79358\AppData\Roaming\Typora\typora-user-images\image-20250516003738516.png" alt="image-20250516003738516"  />

数据库中的数据还是放入到Java程序中，然后用Java来进行返回增删改。故而引用JDBC这门技术。

##### JDBC的概述

**JDBC** 是 **Java Database Connectivity** 的缩写，意为“Java数据库连接”。它是 Java 提供的一套用于访问关系型数据库的 API（应用程序编程接口）。

JDBC：Java Database Connectivity，意为Java数据库连接。

JDBC是Java提供的一组独立于任何数据库系统的API。

Java提供接口规范，由各个数据库厂商提供接口的实现，厂商提供的实现类封装成jar文件，也就是我们俗称的数据库驱动jar包。

##### JDBC的搭建步骤

1.准备好数据库

2.官网下载好数据库连接驱动jar包。[官方下载地址](https://downloads.mysql.com/archives/c-j/)

3.创建好Java项目，在项目下创建好lib文件夹，将下载好的jar包放入lib文件夹中

4.让lib文件夹与项目集成

##### 🧩 JDBC 编程六大步骤（核心流程）

1. **加载数据库驱动**
2. **获取数据库连接**
3. **创建 SQL 执行对象**
4. **执行 SQL 语句**
5. **处理结果集（如果有）**
6. **释放资源**

**注意：新版本的 MySQL JDBC 驱动是 `com.mysql.cj.jdbc.Driver`，旧版是 `com.mysql.jdbc.Driver`，不要弄错。**

你当前的连接字符串是：

```java
String url = "jdbc:mysql://localhost:3306/atguigu";
```

这个写法在 **MySQL 5.5.45+ / 5.6.26+ / 5.7.6+** 以上版本会触发如下警告：

> WARN: Establishing SSL connection without server's identity verification is not recommended...

------

### ✅ 修改建议

你应该在连接字符串中**明确指定 SSL 和时区等参数**，以避免警告并提升兼容性。

#### ✅ 推荐写法（禁用 SSL）：

```java
String url = "jdbc:mysql://localhost:3306/atguigu?useSSL=false&serverTimezone=UTC";
```

从8.0.25之后就不用管了 

```java
package com.atguigu.base;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class JDBCQuick {
    public static void main(String[] args) throws Exception {
        //1.注册驱动（就是将数据库厂商的驱动类通过类加载的方式加载到程序当中来）
        //Class.forName("com.mysql.cj.jdbc.Driver");这是新版本用的
        Class.*forName*("com.mysql.jdbc.Driver");
        //2.获取连接对象
        String url = "jdbc:mysql://localhost:3306/atguigu?useSSL=false&serverTimezone=UTC";
        String username = "root";
        String password = "abc123";
        Connection connection = DriverManager.*getConnection*(url, username, password);

​        //3.获取执行sql语句的对象（把sql语句发送给MySQL的对象，最终执行的还是MySQL）
​        Statement statement = connection.createStatement();

​        //4.编写sql语句并执行.
​        String sql = "SELECT * FROM t_emp";
​        ResultSet resultSet = statement.executeQuery(sql);

​        //5.处理结果：遍历resultSet结果集
​        while (resultSet.next()){
​            int empId = resultSet.getInt("emp_id");
​            String empName = resultSet.getString("emp_name");
​            double empSalary = resultSet.getDouble("emp_salary");
​            int empAge = resultSet.getInt("emp_age");
​            System.*out*.println(empId+"\t"+empName+"\t"+empSalary+"\t"+empAge);
​        }

​        //6.释放资源(先开后关的原则)
​        resultSet.close();
​        statement.close();
​        connection.close();

​    }
}
```




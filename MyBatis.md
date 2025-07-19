# MyBatis

## 一、MyBatis的概述

+ **MyBatis本质上就是对JDBC的封装，通过MyBatis完成CRUD。**
+ MyBatis在三层架构中负责持久层的，属于持久层框架。

+ ORM：对象关系映射
  - O（Object）：Java虚拟机中的Java对象
  - R（Relational）：关系型数据库
  - M（Mapping）：将Java虚拟机中的Java对象映射到数据库表中一行记录，或是将数据库表中一行记录映射成Java虚拟机中的一个Java对象。
  - ORM图示
    * ![](https://cdn.nlark.com/yuque/0/2022/png/21376908/1659580953095-86e388ac-13b5-4448-b90a-2aacd36688ef.png)
    * ![](https://cdn.nlark.com/yuque/0/2022/png/21376908/1659580965550-87c43dee-f1c4-4bdd-b15e-14f831b3d0da.png)
  - MyBatis属于半自动化ORM框架。
  - Hibernate属于全自动化的ORM框架。
+ MyBatis框架特点：
  - 支持定制化 SQL、存储过程、基本映射以及高级映射
  - 避免了几乎所有的 JDBC 代码中手动设置参数以及获取结果集
  - 支持XML开发，也支持注解式开发。【为了保证sql语句的灵活，所以mybatis大部分是采用XML方式开发。】
  - 将接口和 Java 的 POJOs(Plain Ordinary Java Object，简单普通的Java对象)映射成数据库中的记录
  - 体积小好学：两个jar包，两个XML配置文件。
  - 完全做到sql解耦合。
  - 提供了基本映射标签。
  - 提供了高级映射标签。
  - 提供了XML标签，支持动态SQL的编写。
  - ......

MyBatis下载

从github上下载，地址：[https://github.com/mybatis/mybatis-3](https://github.com/mybatis/mybatis-3)

## 二、MyBatis的使用

#### 1、创建数据库

写代码前准备：

- 准备数据库表：汽车表t_car，字段包括：
  * id：主键（自增）【bigint】
  * car_num：汽车编号【varchar】
  * brand：品牌【varchar】
  * guide_price：厂家指导价【decimal类型，专门为财务数据准备的类型】
  * produce_time：生产时间【char，年月日即可，10个长度，'2022-10-11'】
  * car_type：汽车类型（燃油车、电车、氢能源）【varchar】
- 使用navicat for mysql工具建表
  * ![](https://cdn.nlark.com/yuque/0/2022/png/21376908/1659596348287-5876735d-ba3b-43d2-9e73-1c4bdb29e8b5.png)
- 使用navicat for mysql工具向t_car表中插入两条数据，如下：
  * ![](https://cdn.nlark.com/yuque/0/2022/png/21376908/1659596615335-60a93468-5a9e-48b8-acb7-cb4be313b8f1.png)

#### 2、在Maven中引入依赖（pom.xml中添加）

```xml
<dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.10</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.33</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.1</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.5.13</version>
        </dependency>
</dependencies>
```



#### 3、在根目录（resources）下完善mybatis-config.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/powernode"/>
                <property name="username" value="root"/>
                <property name="password" value="abc123"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="CarMapper.xml"/>
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
```

注意1：mybatis核心配置文件的文件名不一定是mybatis-config.xml，可以是其它名字。

注意2：mybatis核心配置文件存放的位置也可以随意。这里选择放在resources根下，相当于放到了类的根路径下。

#### 4、在根目录（resources）下新建**SQL映射文件**（CarMapper.xml）配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace先随意写一个-->
<mapper namespace="car">
    <!--insert sql：保存一个汽车信息-->
    <insert id="insertCar">
        insert into t_car
            (id,car_num,brand,guide_price,produce_time,car_type) 
        values
            (null,'102','丰田mirai',40.30,'2014-10-05','氢能源')
    </insert>
</mapper>
```

注意1：**<font style="color:#E8323C;">sql语句最后结尾可以不写“;”</font>**

注意2：CarMapper.xml文件的名字不是固定的。可以使用其它名字。

注意3：CarMapper.xml文件的位置也是随意的。这里选择放在resources根下，相当于放到了类的根路径下。

注意4：<font style="color:#F5222D;">将CarMapper.xml文件路径配置到mybatis-config.xml：</font>

```xml
<mapper resource="CarMapper.xml"/>
```

#### 5、创建pojo类（根据数据库表来创建）

#### 6、MyBatis工具类SqlSessionUtil的封装

```java
package org.example.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;

public class SqlSessionUtil {
    private SqlSessionUtil() {

    }

    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    private static ThreadLocal<SqlSession> local = new ThreadLocal<>();

    public static SqlSession openSession() {
        SqlSession sqlSession = local.get();
        if (sqlSession == null) {
            sqlSession = sqlSessionFactory.openSession();
            local.set(sqlSession);
        }
        return sqlSession;
    }

    public static void close(SqlSession sqlSession) {
        if (sqlSession != null) {
            sqlSession.close();
            local.remove();
        }
    }
}

```

#### 7、Mapper代理开发

##### 添加接口

定义与SQL映射文件同名的Mapper接口——在java-org-gexample下创建mapper包，定义CarMapper接口：

![image-20250612191502104](C:\Users\79358\AppData\Roaming\Typora\typora-user-images\image-20250612191502104.png)

在接口中写入返回类型和SQL语句的唯一标识（SQL映射文件中对应SQL语句的id）同名的方法：

```java
package org.example.mapper;

import org.example.pojo.Car;

import java.util.List;

public interface CarMapper {
    List<Car> selectByCarType(String carType);

}

```

##### 统一目录

在根目录下创建一个同名的mapper包，将CarMapper.xml放在mapper包下

##### 修改SQL映射文件

设置SQL映射文件的namespace属性为Mapper接口全限定名（完整路径）

![image-20250612192433512](C:\Users\79358\AppData\Roaming\Typora\typora-user-images\image-20250612192433512.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--    这就是为Mapper接口全限定名-->
<mapper namespace="org.example.mapper.CarMapper">
    <select id="selectByCarType" resultType="org.example.pojo.Car">
        select id,
               car_num      as carNum,
               brand,
               guide_price  as guidePrice,
               produce_time as produceTime,
               car_type     as carType
        from t_car
        where car_type = #{carType}
    </select>
</mapper>

```

##### 修改MyBatis核心配置文件

修改MyBatis核心配置文件中SQL映射文件地址

 <mappers>元素是MyBatis配置文件中的一个元素，用于指定Mapper接口的扫描配置。

<package>元素用于指定要扫描的Mapper接口所在的包名。

```xml
<mappers>
    <package name="com.example.mapper.mapper"/>
</mappers>
```

#### 8、引入日志框架logback

引入logback相关配置文件（文件名叫做logback.xml或logback-test.xml，放到类路径当中）

```xml
<?xml version="1.0" encoding="UTF-8"?>

<configuration debug="false">
    <!-- 控制台输出 -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>
    </appender>
    <!-- 按照每天生成日志文件 -->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志文件输出的文件名-->
            <FileNamePattern>${LOG_HOME}/TestWeb.log.%d{yyyy-MM-dd}.log</FileNamePattern>
            <!--日志文件保留天数-->
            <MaxHistory>30</MaxHistory>
        </rollingPolicy>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>
        <!--日志文件最大的大小-->
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <MaxFileSize>100MB</MaxFileSize>
        </triggeringPolicy>
    </appender>

    <!--mybatis log configure-->
    <logger name="com.apache.ibatis" level="TRACE"/>
    <logger name="java.sql.Connection" level="DEBUG"/>
    <logger name="java.sql.Statement" level="DEBUG"/>
    <logger name="java.sql.PreparedStatement" level="DEBUG"/>

    <!-- 日志输出级别,logback日志级别包括五个：TRACE < DEBUG < INFO < WARN < ERROR -->
    <root level="DEBUG">
        <appender-ref ref="STDOUT"/>
        <appender-ref ref="FILE"/>
    </root>

</configuration>
```

## 后续

##### 1、逆向工程

##### 2、动态SQL

##### 3、高级映射

##### 4、缓存

## 面试真题

1、mybatis 中 #{}和 ${}的区别是什么？

#{}带引号，${}不带引号；
#{}可以防止SQL注入；
${}常用于数据库表名、order by子句；
一般能用#{}就不要使用${}；
2、mybatis 是否支持延迟加载？延迟加载的原理是什么？

延迟加载其实就是讲数据加载时机推迟，比如推迟嵌套查询的时机。

延迟加载可以实现先查询主表，按需实时做关联查询，返回关联表结果集，一定程度上提高了效率。

mybatis仅支持关联对象association和关联集合对象collection的延迟加载，association是一对一，collection是一对多查询，在mybatis配置文件中可以配置lazyloadingEnable=true/false。

3、延迟加载的原理是什么？

使用CGLIB为目标对象建立代理对象，当调用目标对象的方法时进入拦截器方法。

比如调用a.getB().getName()，拦截器方法invoke()发现a.getB()为null，会单独发送事先准备好的查询关联B对象的sql语句，把B查询出来然后调用a.setB(b)，也是a的对象的属性b就有值了，然后调用getName()，这就是延迟加载的原理。

4、mybatis一级缓存、二级缓存

1、一级缓存：指的是mybatis中sqlSession对象的缓存，当我们执行查询以后，查询的结果会同时存入sqlSession中，再次查询的时候，先去sqlSession中查询，有的话直接拿出，当sqlSession消失时，mybatis的一级缓存也就消失了，当调用sqlSession的修改、添加、删除、commit()、close()等方法时，会清空一级缓存。

2、二级缓存：指的是mybatis中的sqlSessionFactory对象的缓存，由同一个sqlSessionFactory对象创建的sqlSession共享其缓存，但是其中缓存的是数据而不是对象。当命中二级缓存时，通过存储的数据构造成对象返回。查询数据的时候，查询的流程是二级缓存 > 一级缓存 > 数据库。

3、如果开启了二级缓存，sqlSession进行close()后，才会把sqlSession一级缓存中的数据添加到二级缓存中，为了将缓存数据取出执行反序列化，还需要将要缓存的pojo实现Serializable接口，因为二级缓存数据存储介质多种多样，不一定只存在内存中，也可能存在硬盘中。

4、mybatis框架主要是围绕sqlSessionFactory进行的，具体的步骤：

定义一个configuration对象，其中包含数据源、事务、mapper文件资源以及影响数据库行为属性设置settings。
通过配置对象，则可以创建一个sqlSessionFactoryBuilder对象。
通过sqlSessionFactoryBuilder获得sqlSessionFactory实例。
通过sqlSessionFactory实例创建qlSession实例，通过sqlSession对数据库进行操作。
5、代码实例

```xml
mybatis-config.xml

<?xml version="1.0" encoding="UTF-8"?>  

<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
"http://mybatis.org/dtd/mybatis-3-config.dtd">  


<configuration>   
    <!-- 加载类路径下的属性文件 -->  
    <properties resource="db.properties"/>  

    <!-- 设置类型别名 -->  
    <typeAliases>  
        <typeAlias type="cn.itcast.javaee.mybatis.app04.Student" alias="student"/>  
    </typeAliases>  
     
    <!-- 设置一个默认的连接环境信息 -->  
    <environments default="mysql_developer">  
     
        <!-- 连接环境信息，取一个任意唯一的名字 -->  
        <environment id="mysql_developer">  
            <!-- mybatis使用jdbc事务管理方式 -->  
            <transactionManager type="jdbc"/>  
            <!-- mybatis使用连接池方式来获取连接 -->  
            <dataSource type="pooled">  
                <!-- 配置与数据库交互的4个必要属性 -->  
                <property name="driver" value="${mysql.driver}"/>  
                <property name="url" value="${mysql.url}"/>  
                <property name="username" value="${mysql.username}"/>  
                <property name="password" value="${mysql.password}"/>  
            </dataSource>  
        </environment>  
     
        <!-- 连接环境信息，取一个任意唯一的名字 -->  
        <environment id="oracle_developer">  
            <!-- mybatis使用jdbc事务管理方式 -->  
            <transactionManager type="jdbc"/>  
            <!-- mybatis使用连接池方式来获取连接 -->  
            <dataSource type="pooled">  
                <!-- 配置与数据库交互的4个必要属性 -->  
                <property name="driver" value="${oracle.driver}"/>  
                <property name="url" value="${oracle.url}"/>  
                <property name="username" value="${oracle.username}"/>  
                <property name="password" value="${oracle.password}"/>  
            </dataSource>  
        </environment>  
    </environments>  
     
    <!-- 加载映射文件-->  
    <mappers>  
        <mapper resource="cn/itcast/javaee/mybatis/app14/StudentMapper.xml"/>  
    </mappers>  

</configuration>  
```

```java
public class MyBatisTest {

    public static void main(String[] args) {
        try {
            //读取mybatis-config.xml文件
            InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
            //初始化mybatis,创建SqlSessionFactory类的实例
            SqlSessionFactory sqlSessionFactory =  new SqlSessionFactoryBuilder().build(resourceAsStream);
            //创建session实例
            SqlSession session = sqlSessionFactory.openSession();
            /*
             * 接下来在这里做很多事情,到目前为止,目的已经达到得到了SqlSession对象.通过调用SqlSession里面的方法,
             * 可以测试MyBatis和Dao层接口方法之间的正确性,当然也可以做别的很多事情,在这里就不列举了
             */
            //插入数据
            User user = new User();
            user.setC_password("123");
            user.setC_username("123");
            user.setC_salt("123");
            //第一个参数为方法的完全限定名:位置信息+映射文件当中的id
            session.insert("com.cn.dao.UserMapping.insertUserInformation", user);
            //提交事务
            session.commit();
            //关闭session
            session.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

}
```

5、mybatis如何防止sql注入

注意：但凡是sql注入漏洞的程序，都是因为程序要接受来自客户端用户输入的变量或URL传递的参数，并且这个变量或参数是组成sql语句的一部分，对于用户输入的内容或传递的参数，我们应该要时刻保持警惕，这是安全领域里的【外部数据不可信任】的原则，纵观web安全领域的各种攻击方式，大多数都是因为开发者违反了这个原则而导致的，所以自然能想到，就是变量的检测、过滤、验证下手，确保变量是开发者所预想的。

1、检查变量数据类型和格式

数据类型检查，sql执行前，要进行数据类型检查，如果是邮箱，参数就必须是邮箱的格式，如果是日期，就必须是日期格式；

只要是有固定格式的变量，在SQL语句执行前，应该严格按照固定格式去检查，确保变量是我们预想的格式，这样很大程度上可以避免SQL注入攻击。

如果上述例子中id是int型的，效果会怎样呢？无法注入，因为输入注入参数会失败。比如上述中的name字段，我们应该在用户注册的时候，就确定一个用户名规则，比如5-20个字符，只能由大小写字母、数字以及汉字组成，不包含特殊字符。此时我们应该有一个函数来完成统一的用户名检查。不过，仍然有很多场景并不能用到这个方法，比如写博客，评论系统，弹幕系统，必须允许用户可以提交任意形式的字符才行，否则用户体验感太差了。

2、过滤特殊符号

3、绑定变量，使用预编译语句

6、MyBatis中 #{}和${}的区别是什么

#{}是预编译处理，${}是字符串替换；

Mybatis在处理#{}时，会将sql中的#{}替换为?号，调用PreparedStatement的set方法来赋值；

Mybatis在处理${}时，就是把${}替换成变量的值；

使用#{}可以有效的防止SQL注入，提高系统安全性。

7、Mybatis 中一级缓存与二级缓存

MyBatis的缓存分为一级缓存和 二级缓存。
一级缓存是SqlSession级别的缓存，默认开启。

二级缓存是NameSpace级别(Mapper)的缓存，多个SqlSession可以共享，使用时需要进行配置开启。

缓存的查找顺序：二级缓存 => 一级缓存 => 数据库

8、MyBatis如何获取自动生成的(主)键值

在<insert>标签中使用 useGeneratedKeys和keyProperty 两个属性来获取自动生成的主键值。

示例:

```xml
<insert id="insertname" usegeneratedkeys="true" keyproperty="id">
    insert into names (name) values (#{name}) 
</insert>
```

Java

9、简述Mybatis的动态SQL，列出常用的6个标签及作用

动态SQL是MyBatis的强大特性之一 基于功能强大的OGNL表达式。

动态SQL主要是来解决查询条件不确定的情况，在程序运行期间，根据提交的条件动态的完成查询

常用的标签:

<if> : 进行条件的判断

<where>：在<if>判断后的SQL语句前面添加WHERE关键字，并处理SQL语句开始位置的AND 或者OR的问题

<trim>：可以在SQL语句前后进行添加指定字符 或者去掉指定字符.

<set>:  主要用于修改操作时出现的逗号问题

<choose> <when> <otherwise>：类似于java中的switch语句.在所有的条件中选择其一

<foreach>：迭代操作

10、Mybatis 如何完成MySQL的批量操作

MyBatis完成MySQL的批量操作主要是通过<foreach>标签来拼装相应的SQL语句

例如:

```xml
<insert** id="insertBatch" >
    insert into tbl_employee(last_name,email,gender,d_id) values 
   <foreach** collection="emps" item="curr_emp" separator=","**>
      (#{curr_emp.lastName},#{curr_emp.email},#{curr_emp.gender},#{curr_emp.dept.id}) 
   </foreach>
</insert>
```



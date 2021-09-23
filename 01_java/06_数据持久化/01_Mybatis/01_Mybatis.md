# 1. 框架概述(了解)

## 1.1. 什么是框架

### 1.1.1. 什么是框架

框架（Framework）是整个或部分系统的可重用设计，表现为一组抽象构件及构件实例间交互的方法;另一种 定义认为，框架是可被应用开发者定制的应用骨架。前者是从应用方面而后者是从目的方面给出的定义。 简而言之，框架其实就是某种应用的半成品，就是一组组件，供你选用完成你自己的系统。

简单说就是使用别人搭好的舞台，你来做表演。而且，框架一般是成熟的，不断升级的软件。

### 1.1.2. 框架要解决的问题 

框架要解决的最重要的一个问题是技术整合的问题，在 J2EE 的 框架中，有着各种各样的技术，不同的 软件企业需要从 J2EE 中选择不同的技术，这就使得软件企业最终的应用依赖于这些技术，技术自身的复杂性和技 术的风险性将会直接对应用造成冲击。而应用是软件企业的核心，是竞争力的关键所在，因此应该将应用自身的设 计和具体的实现技术解耦。这样，软件企业的研发将集中在应用的设计上，而不是具体的技术实现，技术实现是应用的底层支撑，它不应该直接对应用产生影响。 

==框架一般处在低层应用平台（如 J2EE）和高层业务逻辑之间的中间层。==

### 1.1.3. 软件开发的分层重要性 

框架的重要性在于它实现了部分功能，并且能够很好的将低层应用平台和高层业务逻辑进行了缓和。为了实现 软件工程中的“高内聚、低耦合”。把问题划分开来各个解决，易于控制，易于延展，易于分配资源。我们常见的 MVC 软件设计思想就是很好的分层思想。

![image-20210604183519675](images\01_Mybatis\image-20210604183519675.png)

通过分层更好的实现了各个部分的职责，在每一层将再细化出不同的框架，分别解决各层关注的问题。



### 1.1.4. 分层开发下的常见框架

常见的 JavaEE 开发框架： 

1. 解决数据的持久化问题的框架：Mybatis，作为持久层的框架，还有一个封装程度更高的框架就是Hibernate，但这个框架因为各种原因目前在国内的 流行程度下降太多，现在公司开发也越来越少使用。目前使用 Spring Data 来实现数据持久化也是一种趋势。
2. 解决 WEB 层问题的 MVC 框架：SpringMVC
3. 解决技术整合问题的框架：Spring

![image-20210604190532868](images\01_Mybatis\image-20210604190532868.png)

### 1.1.5. MyBatis 框架概述

 mybatis 是一个优秀的基于 java 的持久层框架，它内部封装了 jdbc，使开发者只需要关注 sql 语句本身， 而不需要花费精力去处理加载驱动、创建连接、创建 statement 等繁杂的过程。 

mybatis 通过 xml 或注解的方式将要执行的各种 statement 配置起来，并通过 java 对象和 statement 中 sql 的动态参数进行映射生成最终执行的 sql 语句，最后由 mybatis 框架执行 sql 并将结果映射为 java 对象并返回。 

采用 ORM 思想解决了实体和数据库映射的问题，对 jdbc 进行了封装，屏蔽了 jdbc api 底层访问细节，使我 们不用与 jdbc api 打交道，就可以完成对数据库的持久化操作。 

为了我们能够更好掌握框架运行的内部过程，并且有更好的体验，下面我们将从自定义 Mybatis 框架开始来 学习框架。此时我们将会体验框架从无到有的过程体验，也能够很好的综合前面阶段所学的基础。

![image-20210604190706329](images\01_Mybatis\image-20210604190706329.png)

## 1.2. JDBC 编程的分析

jdbc 问题分析 

1. 数据库链接创建、释放频繁造成系统资源浪费从而影响系统性能，如果使用数据库链接池可解决此问题。
2. Sql 语句在代码中硬编码，造成代码不易维护，实际应用 sql 变化的可能较大，sql 变动需要改变 java 代码。
3. 使用 preparedStatement 向占有位符号传参数存在硬编码，因为 sql 语句的 where 条件不一定，可能 多也可能少，修改 sql 还要修改代码，系统不易维护。 
4. 对结果集解析存在硬编码（查询列名），sql 变化导致解析代码变化，系统不易维护，如果能将数据库记 录封装成 pojo 对象解析比较方便。

# 2. 快速入门(重点)

## 2.1. Mybatis 框架开发的准备

从百度中“`mybatis download`”可以下载最新的 Mybatis 开发包。

![image-20210604184210208](images\01_Mybatis\image-20210604184210208.png)

进入选择语言的界面，进入中文版本的开发文档。

![image-20210604184410525](images\01_Mybatis\image-20210604184410525.png)



## 2.2. 搭建开发环境

### 2.2.1. 创建 maven 工程

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itheima</groupId>
    <artifactId>day01_eesy_01mybatis</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
        </dependency>
    </dependencies>
</project>
```

### 2.2.2. 创建实体类

### 2.2.3. 编写持久层接口 IUserDao

IUserDao 接口就是我们的持久层接口（也可以写成 UserDao 或者 UserMapper）,具体代码如下：

```Java
public interface IUserDao {
    // 查询所有操作
    List<User> findAll();
}
```

### 2.2.4. 编写持久层接口的映射文件 IUserDao.xml

> 【危险】要求
>
> 1.  **创建位置**：必须和持久层接口在相同的包中。
>
> 2. **名称**：必须以持久层接口名称命名文件名，扩展名是.xml
>
>    ![image-20210604191920854](images\01_Mybatis\image-20210604191920854.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.dao.IUserDao">
    <!--配置查询所有-->
    <select id="findAll" resultType="com.itheima.domain.User">
        select * from user
    </select>
</mapper>
```

### 2.2.5. 编写 SqlMapConfig.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- mybatis的主配置文件 -->
<configuration>
    <!-- 配置环境 -->
    <environments default="mysql">
        <!-- 配置mysql的环境-->
        <environment id="mysql">
            <!-- 配置事务的类型-->
            <transactionManager type="JDBC"></transactionManager>
            <!-- 配置数据源（连接池） -->
            <dataSource type="POOLED">
                <!-- 配置连接数据库的4个基本信息 -->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/eesy_mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="1234"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 指定映射配置文件的位置，映射配置文件指的是每个dao独立的配置文件 -->
    <mappers>
        <mapper resource="com/itheima/dao/IUserDao.xml"/>
    </mappers>
</configuration>
```

### 2.2.6. 编写测试类

```Java
public static void main(String[] args)throws Exception {
    //1.读取配置文件
    InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
    //2.创建SqlSessionFactory工厂
    SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
    SqlSessionFactory factory = builder.build(in);
    //3.使用工厂生产SqlSession对象
    SqlSession session = factory.openSession();
    //4.使用SqlSession创建Dao接口的代理对象
    IUserDao userDao = session.getMapper(IUserDao.class);
    //5.使用代理对象执行方法
    List<User> users = userDao.findAll();
    for(User user : users){
        System.out.println(user);
    }
    //6.释放资源
    session.close();
    in.close();
}
```

### 2.2.7. mybatis执行

![image-20210604192449453](images\01_Mybatis\image-20210604192449453.png)

# 3. 常见映射SQL(重点)

## 3.1. 根据 ID 查询

```xml
<select id="findById" resultType="com.itheima.domain.User" parameterType="int">
	select * from user where id = #{uid}
</select>
```

**属性说明**：

1. `resultType` 属性：用于指定结果集的类型。
2. `parameterType` 属性：用于指定传入参数的类型。



sql 语句中使用`#{}`字符：它代表占位符，相当于原来 jdbc 部分所学的?，都是用于执行语句时替换实际的数据。

- 具体的数据是由#{}里面的内容决定的。
- `#{}`中内容的写法：由于数据类型是基本类型，所以此处可以随意写。

## 3.2. 保存操作

```xml
<insert id="saveUser" parameterType="com.itheima.domain.User">
	insert into user(username,birthday,sex,address) values(#{username},#{birthday},#{sex},#{address})
</insert>
```

**属性说明**：

1. `parameterType` 属性：代表参数的类型，因为我们要传入的是一个类的对象，所以类型就写类的全名称。



sql 语句中使用#{}字符：它代表占位符，相当于原来 jdbc 部分所学的?，都是用于执行语句时替换实际的数据。

- 具体的数据是由#{}里面的内容决定的。
- #{}中内容的写法：由于我们保存方法的参数是 一个 User 对象，此处要写 User 对象中的属性名称。它用的是 ognl 表达式。

### 3.2.1. ognl 表达式

它是 apache 提供的一种表达式语言，全称是：Object Graphic Navigation Language 对象图导航语言

它是按照一定的语法格式来获取数据的。

语法格式就是使用 `#{对象.对象}`的方式
`#{user.username}`它会先去找 user 对象，然后在 user 对象中找到 username 属性，并调用getUsername()方法把值取出来。但是我们在 parameterType 属性上指定了实体类名称，所以可以省略 user。而直接写 username。

### 3.2.2. 新增用户 id 的返回值

新增用户后，同时还要返回当前新增用户的 id 值，因为 id 是由数据库的自动增长来实现的，所以就相当于我们要在新增后将自动增长 auto_increment 的值返回。

```xml
<insert id="saveUser" parameterType="USER">
	<!-- 配置保存时获取插入的 id -->
	<selectKey keyColumn="id" keyProperty="id" resultType="int">
		select last_insert_id();
	</selectKey>
	insert into user(username,birthday,sex,address) values(#{username},#{birthday},#{sex},#{address})
</insert
```

## 3.3. 用户更新

```xml
<update id="updateUser" parameterType="com.itheima.domain.User">
	update user set username=#{username},birthday=#{birthday},sex=#{sex},address=#{address} where id=#{id}
</update>
```

## 3.4.  用户删除

```xml
<delete id="deleteUser" parameterType="java.lang.Integer">
	delete from user where id = #{uid}
</delete>
```

## 3.5. 模糊查询

### 3.5.1. 模糊查询方式一

```xml
<select id="findByName" resultType="com.itheima.domain.User" parameterType="String">
     select * from user where username like #{username}
</select>
```

在控制台输出的执行 SQL 语句如下：

![image-20210604203435844](images\01_Mybatis\image-20210604203435844.png)

我们在配置文件中没有加入%来作为模糊查询的条件，所以在传入字符串实参时，就需要给定模糊查询的标识%。配置文件中的#{username}也只是一个占位符，所以 SQL 语句显示为“？”。

### 3.5.2. 模糊查询方式二

```xml
<select id="findByName" parameterType="string" resultType="com.itheima.domain.User">
     select * from user where username like '%${value}%'
</select>
```

在控制台输出的执行 SQL 语句如下：

![image-20210604203612374](images\01_Mybatis\image-20210604203612374.png)

## 3.6. #{}与${}的区别

1. **\#{}表示一个占位符号**

   通过#{}可以实现 preparedStatement 向占位符中设置值，自动进行 java 类型和 jdbc 类型转换， #{}可以有效防止 sql 注入。 #{}可以接收简单类型值或 pojo 属性值。 如果 parameterType 传输单个简单类 型值，#{}括号中可以是 value 或其它名称。 

2. **${}表示拼接 sql 串**

   通过\${}可以将 parameterType 传入的内容拼接在 sql 中且不进行 jdbc 类型转换， \${}可以接收简 单类型值或 pojo 属性值，如果 parameterType 传输单个简单类型值，${}括号中只能是 value。

## 3.7. 查询使用聚合函数

```xml
<select id="findTotal" resultType="int">
	select count(*) from user;
</select>
```

## 3.8. Mybatis 与 JDBC 编程的比较

1. 数据库链接创建、释放频繁造成系统资源浪费从而影响系统性能，如果使用数据库链接池可解决此问题。

   解决： 在 SqlMapConfig.xml 中配置数据链接池，使用连接池管理数据库链接。 

2. Sql 语句写在代码中造成代码不易维护，实际应用 sql 变化的可能较大，sql 变动需要改变 java 代码。 

   解决： 将 Sql 语句配置在 XXXXmapper.xml 文件中与 java 代码分离。 

3. 向 sql 语句传参数麻烦，因为 sql 语句的 where 条件不一定，可能多也可能少，占位符需要和参数对应。 

   解决： Mybatis 自动将 java 对象映射至 sql 语句，通过 statement 中的 parameterType 定义输入参数的 类型。 

4. 对结果集解析麻烦，sql 变化导致解析代码变化，且解析前需要遍历，如果能将数据库记录封装成 pojo 对 象解析比较方便。 

   解决： Mybatis 自动将 sql 执行结果映射至 java 对象，通过 statement 中的 resultType 定义输出结果的 类型。

# 4. Mybatis 的参数深入

## 4.1. parameterType 配置参数

作用：SQL 语句传参，使用标签的 parameterType 属性来设定。该属性的取值可以 是基本类型，引用类型（例如:String 类型），还可以是实体类类型（POJO 类）。同时也可以使用实体类的包装 类

> 【危险】
>
> 1. 基 本 类 型 和 String 我 们 可 以 直 接 写 类 型 名 称 ， 也 可 以 使 用 包 名 . 类 名 的 方 式 ， 例 如 ： java.lang.String。 
> 2. 实体类类型，目前我们只能使用全限定类名。 
>
> 究其原因，是 mybaits 在加载时已经把常用的数据类型注册了别名，从而我们在使用时可以不写包名， 而我们的是实体类并没有注册别名，所以必须写全限定类名。

这些都是支持的默认别名。我们也可以从源码角度来看它们分别都是如何定义出来的。

| 别名       | 映射的类型 |
| ---------- | ---------- |
| _byte      | byte       |
| _long      | long       |
| _short     | short      |
| _int       | int        |
| _integer   | int        |
| _double    | double     |
| _float     | float      |
| _boolean   | boolean    |
| string     | String     |
| byte       | Byte       |
| long       | Long       |
| short      | Short      |
| int        | Integer    |
| integer    | Integer    |
| double     | Double     |
| float      | Float      |
| boolean    | Boolean    |
| date       | Date       |
| decimal    | BigDecimal |
| bigdecimal | BigDecimal |
| object     | Object     |
| map        | Map        |
| hashmap    | HashMap    |
| list       | List       |
| arraylist  | ArrayList  |
| collection | Collection |
| iterator   | Iterator   |

传递 pojo 包装对象

开发中通过 pojo 传递查询条件 ，查询条件是综合的查询条件，不仅包括用户查询条件还包括其它的查 询条件（比如将用户购买商品信息也作为查询条件），这时可以使用包装对象传递输入参数。 Pojo 类中包含 pojo。

## 4.2. resultType 配置结果类型 

resultType 属性可以指定结果集的类型，它支持基本类型和实体类类型。 。

需要注意的是，它和 parameterType 一样，如果注册过类型别名的，可以直接使用别名。没有注册过的必须 使用全限定类名。

例如：我们的实体类此时必须是全限定类名（今天最后一个章节会讲解如何配置实体类的别名） 同时，当是实体类名称是，还有一个要求，实体类中的属性名称必须和查询语句中的列名保持一致，否则无法 实现封装。 

## 4.3. resultMap 结果类型

resultMap 标签可以建立查询的列名和实体类的属性名称不一致时建立对应关系。从而实现封装。

在 select 标签中使用 resultMap 属性指定引用即可。同时 resultMap 可以实现将查询结果映射为复杂类 型的 pojo，比如在查询结果映射对象中包括 pojo 和 list 实现一对一查询和一对多查询。

```xml
<!-- 建立 User 实体和数据库表的对应关系-->
<resultMap type="com.itheima.domain.User" id="userMap">
	<id column="id" property="userId"/>
    <result column="username" property="userName"/>
    <result column="sex" property="userSex"/>
    <result column="address" property="userAddress"/>
    <result column="birthday" property="userBirthday"/>
</resultMap>
```

属性说明：

1. `type` 属性：指定实体类的全限定类名
2. `id` 属性：给定一个唯一标识，是给查询 select 标签引用用的。
3. `id` 标签：用于指定主键字段
4. `result` 标签：用于指定非主键字段
   - `column` 属性：用于指定数据库列名
   - `property` 属性：用于指定实体类属性名称

```xml
<!-- 配置查询所有操作 -->
<select id="findAll" resultMap="userMap">
	select * from user
</select>
```

# 5. SqlMapConfig.xml配置文件

定义顺序：

1. properties（属性）
   - property
2. settings（全局配置参数）
   - setting 
3. typeAliases（类型别名） 
   - typeAliase 
   - package 
4. typeHandlers（类型处理器） 
5. objectFactory（对象工厂） 
6. plugins（插件） 
7. environments（环境集合属性对象） 
   - environment（环境子属性对象） 
     - transactionManager（事务管理） 
     - dataSource（数据源） 
8. mappers（映射器） 
   - mapper 
   - packag

## 5.1. properties（属性）

在使用 properties 标签配置时，我们可以采用两种方式指定属性配置。

### 5.1.1. 直接声明

```xml
<properties>
	<property name="jdbc.driver" value="com.mysql.jdbc.Driver"/>
	<property name="jdbc.url" value="jdbc:mysql://localhost:3306/eesy"/>
    <property name="jdbc.username" value="root"/>
	<property name="jdbc.password" value="1234"/>
</properties>
```

### 5.1.2. 间接声明

先在 classpath 下定义 db.properties 文件

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/eesy
jdbc.username=root
jdbc.password=1234
```

再指定文件

```xml
<properties url=file:///D:/IdeaProjects/day02_eesy_01mybatisCRUD/src/main/resources/jdbcConfig.properties">
</properties>
```

**属性说明**：

1. `resource` 属性：用于指定 properties 配置文件的位置，要求配置文件必须在类路径下 `resource="jdbcConfig.properties"` 
2. `url` 属性： URL： Uniform Resource Locator 统一资源定位符 http://localhost:8080/mystroe/CategoryServlet URL 
                                                                                                                 协议      主机     端口    URI 
                            URI：Uniform Resource Identifier 统一资源标识符 /mystroe/CategoryServlet 它是可以在 web 应用中唯一定位一个资源的路径

###  5.1.3. dataSource 标签就变成了引用上面的配置

```xml
<dataSource type="POOLED">
    <property name="driver" value="${jdbc.driver}"/>
    <property name="url" value="${jdbc.url}"/>
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</dataSource>
```

## 5.2. typeAliases（类型别名）

在前面我们讲的 Mybatis 支持的默认别名，我们也可以采用自定义别名方式来开发。

```xml
<typeAliases>
    <!-- 【1】单个别名定义 -->
    <typeAlias alias="user" type="com.itheima.domain.User"/>
    <!-- 【2】批量别名定义，扫描整个包下的类，别名为类名（首字母大写或小写都可以） -->
    <package name="com.itheima.domain"/>
    <package name="其它包"/>
</typeAliases>
```

## 5.3. mappers（映射器）

### 5.3.1. \<mapper resource=" " />

```xml
使用相对于类路径的资源
<mapper resource="com/itheima/dao/IUserDao.xml" />
```

### 5.3.2. \<mapper class=" " />

```xml
使用 mapper 接口类路径
<mapper class="com.itheima.dao.UserDao"/>
```

> 【危险】此种方法要求 mapper 接口名称和 mapper 映射文件名称相同，且放在同一个目录中。

### 5.3.3. \<package name=""/>

```xml
注册指定包下的所有 mapper 接口
<package name="cn.itcast.mybatis.mapper"/>
```

> 【危险】此种方法要求 mapper 接口名称和 mapper 映射文件名称相同，且放在同一个目录中。 

# 6.  Mybatis 连接池与事务深入

## 6.1. Mybatis 的连接池技术

我们在前面的 WEB 课程中也学习过类似的连接池技术，而在 Mybatis 中也有连接池技术，但是它采用的是自 己的连接池技术。在 Mybatis 的 SqlMapConfig.xml 配置文件中，通过来实 现 Mybatis 中连接池的配置。

### 6.1.1. Mybatis 连接池的分类

在 Mybatis 中我们将它的数据源 dataSource 分为以下几类：

![image-20210605120033686](images\01_Mybatis\image-20210605120033686.png)

可以看出 Mybatis 将它自己的数据源分为三类：

1. UNPOOLED 不使用连接池的数据源 
2. POOLED 使用连接池的数据源 
3. JNDI 使用 JNDI 实现的数据源

具体结构如下：

![image-20210605120127678](images\01_Mybatis\image-20210605120127678.png)

相应地，MyBatis 内部分别定义了实现了 `java.sql.DataSource` 接口的 `UnpooledDataSource`， `PooledDataSource` 类来表示 UNPOOLED、POOLED 类型的数据源。

![image-20210605121939696](images\01_Mybatis\image-20210605121939696.png)

在这三种数据源中，我们一般采用的是 POOLED 数据源（很多时候我们所说的数据源就是为了更好的管理数据 库连接，也就是我们所说的连接池技术）。

### 6.1.2. Mybatis 中数据源的配置

我们的数据源配置就是在 SqlMapConfig.xml 文件中，具体配置如下：

```xml
<!-- 配置数据源（连接池）信息 -->
<dataSource type="POOLED">
    <property name="driver" value="${jdbc.driver}"/>
    <property name="url" value="${jdbc.url}"/>
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</dataSource>
```

MyBatis 在初始化时，根据的 type 属性来创建相应类型的的数据源 DataSource，即：

- `type=”POOLED”`：MyBatis 会创建 PooledDataSource 实例 
- `type=”UNPOOLED”` ： MyBatis 会创建 UnpooledDataSource 实例 
- `type=”JNDI”`：MyBatis 会从 JNDI 服务上查找 DataSource 实例，然后返回使用



### 6.1.3. Mybatis 中 DataSource 的存取

MyBatis 是 通 过 工 厂 模 式 来 创 建 数 据 源 DataSource 对 象 的 ， MyBatis 定 义 了 抽 象 的 工 厂 接 口：`org.apache.ibatis.datasource.DataSourceFactory`,通过其 `getDataSource()`方法返回数据源 `DataSource`。

下面是 `DataSourceFactory` 源码，具体如下：

```java
public interface DataSourceFactory {
     void setProperties(Properties props);
     DataSource getDataSource();
}
```

MyBatis 创建了 DataSource 实例后，会将其放到 Configuration 对象内的 Environment 对象中， 供 以后使用。



### 6.1.4. Mybatis 中连接的获取过程分析

当我们需要创建 SqlSession 对象并需要执行 SQL 语句时，这时候 MyBatis 才会去调用 dataSource 对象 来创建java.sql.Connection对象。也就是说，java.sql.Connection对象的创建一直延迟到执行SQL语句 的时候。

```java
public void testSql() throws Exception {
    InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
    SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
    SqlSession sqlSession = factory.openSession();
    // selectList 才会触发 MyBatis 在底层执行下面这个方法来创建 java.sql.Connection 对象。
    List<User> list = sqlSession.selectList("findUserById",41);
    System.out.println(list.size());
}
```

## 6.2. Mybatis 的事务控制

### 6.2.1. JDBC 中事务的回顾

在 JDBC 中我们可以通过手动方式将事务的提交改为手动方式，通过 `setAutoCommit()`方法就可以调整。

那么我们的 Mybatis 框架因为是对 JDBC 的封装，所以 Mybatis 框架的事务控制方式，本身也是用 JDBC 的 setAutoCommit()方法来设置事务提交方式的。

### 6.2.2. Mybatis 中事务提交方式

Mybatis 中事务的提交方式，本质上就是调用 JDBC 的 setAutoCommit()来实现事务控制。

这是我们的 Connection 的整个变化过程，通过分析我们能够发现之前的 CUD 操作过程中，我们都要手动进 行事务的提交，原因是 setAutoCommit()方法，在执行时它的值被设置为 false 了，所以我们在 CUD 操作中， 必须通过 sqlSession.commit()方法来执行提交操作。

### 6.2.3. Mybatis 自动提交事务的设置

通过上面的研究和分析，现在我们一起思考，为什么 CUD 过程中必须使用 sqlSession.commit()提交事 务？主要原因就是在连接池中取出的连接，都会将调用 connection.setAutoCommit(false)方法，这样我们 就必须使用 sqlSession.commit()方法，相当于使用了 JDBC 中的 connection.commit()方法实现事务提 交。 

明白这一点后，我们现在一起尝试不进行手动提交，一样实现 CUD 操作。

```java 
public void init()throws Exception {
    //1.读取配置文件
    in = Resources.getResourceAsStream("SqlMapConfig.xml");
    //2.创建构建者对象
    SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
    //3.创建 SqlSession 工厂对象
    factory = builder.build(in);
    //4.创建 SqlSession 对象. 设置自动提交方式
    session = factory.openSession(true);
    //5.创建 Dao 的代理对象
    userDao = session.getMapper(IUserDao.class);
}
```



# 7. Mybatis 的动态 SQL 语句

Mybatis 的映射文件中，前面我们的 SQL 都是比较简单的，有些时候业务逻辑复杂时，我们的 SQL 是动态变 化的，此时在前面的学习中我们的 SQL 就不能满足要求了。

## 7.1. 动态 SQL`<if>`之标签

 我们根据实体类的不同取值，使用不同的 SQL 语句来进行查询。比如在 id 如果不为空时可以根据 id 查询， 如果 username 不同空时还要加入用户名作为条件。这种情况在我们的多条件组合查询中经常会碰到。

```xml
<select id="findByUser" resultType="user" parameterType="user">
	select * from user where 1=1
    <if test="username!=null and username != '' ">
        and username like #{username}
    </if>
    <if test="address != null">
    	and address like #{address}
    </if>
</select>
```

> 【警告】标签的 test 属性中写的是对象的属性名，如果是包装类的对象要使用 OGNL 表达式的写法。 另外要注意 where 1=1 的作用~！

## 7.2. 动态 SQL`<where>`之标签

为了简化上面 where 1=1 的条件拼装，我们可以采用标签来简化开发。

```xml
<select id="findUserInIds" resultMap="userMap" parameterType="queryvo">
    select * from user
    <where>
        <if test="ids != null and ids.size()>0">
            <foreach collection="ids" open="and id in (" close=")" item="uid" separator=",">
                #{uid}
            </foreach>
        </if>
    </where>
</select>
```

## 7.3. 动态标签`<foreach>`之标签

```xml
<select id="findInIds" resultType="user" parameterType="queryvo">
    <!-- select * from user where id in (1,2,3,4,5); -->
    select * from user
    <where>
        <if test="ids != null and ids.size() > 0">
            <foreach collection="ids" open="id in ( " close=")" item="uid" separator=",">
                #{uid}
            </foreach>
        </if>
    </where>
</select>

```

`<foreach>`标签用于遍历集合

**它的属性**： 

1. collection：代表要遍历的集合元素，注意编写时==不要写#{}== 
2. open：代表语句的开始部分 
3. close：代表结束部分
4. item：代表遍历集合的每个元素，生成的变量名 
5. sperator：代表分隔符

## 7.4. Mybatis 中简化编写的 SQL 片段

Sql 中可将重复的 sql 提取出来，使用时用 include 引用即可，最终达到 sql 重用的目的。

### 7.4.1 定义代码片段

```xml
<sql id="defaultSql">
	select * from user
</sql>
```

### 7.4.2. 引用代码片段

```xml
<select id="findAll" resultType="user">
	<include refid="defaultSql"></include>
</select>
```

# 8. Mybatis 多表查询

## 8.1. 一对一

### 8.1.1. 方式一

定义专门的 po 类作为输出类型，其中定义了 sql 查询结果集所有的字段。此方法较为简单，企业中使用普 遍。

```xml
<select id="findAll" resultType="accountuser">
	select a.*,u.username,u.address from account a,user u where a.uid =u.id;
</select>
```

### 8.1.2. 方式二

使用 resultMap，定义专门的 resultMap 用于映射一对一查询结果。 通过面向对象的(has a)关系可以得知，我们可以在 Account 类中加入一个 User 类的对象来代表这个账户 是哪个用户的。

```xml
<!-- 定义封装account和user的resultMap -->
<resultMap id="accountUserMap" type="account">
    <id property="id" column="aid"></id>
    <result property="uid" column="uid"></result>
    <result property="money" column="money"></result>
    <!-- 一对一的关系映射：配置封装user的内容-->
    <association property="user" column="uid" javaType="user">
        <id property="id" column="id"></id>
        <result column="username" property="username"></result>
        <result column="address" property="address"></result>
        <result column="sex" property="sex"></result>
        <result column="birthday" property="birthday"></result>
    </association>
</resultMap>
<select id="findAll" resultMap="accountMap">
	select u.*,a.id as aid,a.uid,a.money from account a,user u where a.uid =u.id;
</select>
```

```Java
public class Account implements Serializable {

    private Integer id;
    private Integer uid;
    private Double money;

    //从表实体应该包含一个主表实体的对象引用
    private User user;

    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }
    // 其他get/set方法省略
}
```

## 8.2. 一对多

```xml
<!-- 定义User的resultMap-->
<resultMap id="userMap" type="user">
    <id property="id" column="id"></id>
    <result property="username" column="username"></result>
    <result property="address" column="address"></result>
    <result property="sex" column="sex"></result>
    <result property="birthday" column="birthday"></result>
    <!-- 配置角色集合的映射 -->
    <collection property="roles" ofType="role">
        <id property="roleId" column="rid"></id>
        <result property="roleName" column="role_name"></result>
        <result property="roleDesc" column="role_desc"></result>
    </collection>
</resultMap>

<!-- 查询所有 -->
<select id="findAll" resultMap="userMap">
    select u.*,r.id as rid,r.role_name,r.role_desc from user u
    left outer join user_role ur  on u.id = ur.uid
    left outer join role r on r.id = ur.rid
</select>
```

`collection` 部分定义了用户关联的账户信息。表示关联查询结果集 

`property="accList"`： 关联查询的结果集存储在 User 对象的上哪个属性。 

`ofType="account"`： 指定关联查询的结果集中的对象类型即List中的对象类型。此处可以使用别名，也可以使用全限定名。

## 8.3. 多对多

```java
public class User implements Serializable {

    private Integer id;
    private String username;
    private String address;
    private String sex;
    private Date birthday;

    //多对多的关系映射：一个用户可以具备多个角色
    private List<Role> roles;

    public List<Role> getRoles() {
        return roles;
    }

    public void setRoles(List<Role> roles) {
        this.roles = roles;
    }
    // 省略其他get/set
}
```

```Java
public class Role implements Serializable {

    private Integer roleId;
    private String roleName;
    private String roleDesc;

    //多对多的关系映射：一个角色可以赋予多个用户
    private List<User> users;

    public List<User> getUsers() {
        return users;
    }

    public void setUsers(List<User> users) {
        this.users = users;
    }
    // 省略其他get/set
}
```



```xml
    <!--定义role表的ResultMap-->
    <resultMap id="roleMap" type="role">
        <id property="roleId" column="rid"></id>
        <result property="roleName" column="role_name"></result>
        <result property="roleDesc" column="role_desc"></result>
        <collection property="users" ofType="user">
            <id column="id" property="id"></id>
            <result column="username" property="username"></result>
            <result column="address" property="address"></result>
            <result column="sex" property="sex"></result>
            <result column="birthday" property="birthday"></result>
        </collection>
    </resultMap>

    <!--查询所有-->
    <select id="findAll" resultMap="roleMap">
       select u.*,r.id as rid,r.role_name,r.role_desc from role r
        left outer join user_role ur  on r.id = ur.rid
        left outer join user u on u.id = ur.uid
    </select>
```



# 9. Mybatis 延迟加载策略

## 9.1. 延迟加载

**延迟加载**： 就是在需要用到数据时才进行加载，不需要用到数据时就不加载数据。延迟加载也称懒加载。

- 好处：先从单表查询，需要时再从关联表去关联查询，大大提高数据库性能，因为查询单表要比关联查询多张表速 度要快。 
- 坏处： 因为只有当需要用到数据时，才会进行数据库查询，这样在大批量数据查询时，因为查询工作也要消耗 时间，所以可能造成用户等待时间变长，造成用户体验下降。

## 9.2. 使用 assocation 实现延迟加载

在 Mybatis 的配置文件 ==SqlMapConfig.xml== 文件中添加延迟加载的配置

```xml
<!-- 开启延迟加载的支持
可以配置lazyLoadingEnabled 值为true，不设置aggressiveLazyLoading，为全局设置 
-->
<settings>
    <setting name="lazyLoadingEnabled" value="true"/>
    <setting name="aggressiveLazyLoading" value="false"/>
</settings>
```

##  9.3. 使用 Collection 实现延迟加载

同样我们也可以在一对多关系配置的`<collection>`结点中配置延迟加载策略。`<collection>` 结点中也有 select 属性，column 属性。

```xml
<resultMap id="userAccountMap" type="user">
    <id property="id" column="id"></id>
    <result property="username" column="username"></result>
    <result property="address" column="address"></result>
    <result property="sex" column="sex"></result>
    <result property="birthday" column="birthday"></result>
    <!-- 配置user对象中accounts集合的映射 -->
    <collection property="accounts"="account" select="com.itheima.dao.IAccountDao.findAccountByUid" column="id"></collection>
</resultMap>
```

`collection` 是用于建立一对多中集合属性的对应关系 

`ofType` 用于指定集合元素的数据类型 

`select` 是用于指定查询账户的唯一标识（账户的 dao 全限定类名加上方法名称） 

`column` 是用于指定使用哪个字段的值作为条件查询。至关重要，表关联的查询关联字段可能不尽一样，但是以主表字段为纽带，让另一张表的查询字段去用它的值查询，该字段错误，主表可以得到数据，懒加载会报空指针异常。

# 10. Mybatis 缓存

像大多数的持久化框架一样，Mybatis 也提供了缓存策略，通过缓存策略来减少数据库的查询次数，从而提 高性能。 

Mybatis 中缓存分为一级缓存，二级缓存。

![image-20210605220141235](images\01_Mybatis\image-20210605220141235.png)

## 10.1 Mybatis 一级缓存

一级缓存是 SqlSession 级别的缓存，只要 SqlSession 没有 flush 或 close，它就存在。

### 10.1.1. 一级缓存的分析

一级缓存是 SqlSession 范围的缓存，当调用 SqlSession 的修改，添加，删除，commit()，close()等方法时，就会清空一级缓存。

![image-20210605220734429](images\01_Mybatis\image-20210605220734429.png)

第一次发起查询用户 id 为 1 的用户信息，先去找缓存中是否有 id 为 1 的用户信息，如果没有，从数据库查 询用户信息。 

得到用户信息，将用户信息存储到一级缓存中。 

如果 sqlSession 去执行 commit 操作（执行插入、更新、删除），清空 SqlSession 中的一级缓存，这样 做的目的为了让缓存中存储的是最新的信息，避免脏读。 

第二次发起查询用户 id 为 1 的用户信息，先去找缓存中是否有 id 为 1 的用户信息，缓存中有，直接从缓存 中获取用户信息。



## 10.2. Mybatis 二级缓存

二级缓存是 mapper 映射级别的缓存，多个 SqlSession 去操作同一个 Mapper 映射的 sql 语句，多个 SqlSession 可以共用二级缓存，二级缓存是跨 SqlSession 的。

### 10.2.1. 二级缓存结构图

![image-20210605220858636](images\01_Mybatis\image-20210605220858636.png)

首先开启 mybatis 的二级缓存。

 sqlSession1 去查询用户信息，查询到用户信息会将查询数据存储到二级缓存中

如果 SqlSession3 去执行相同 mapper 映射下 sql，执行 commit 提交，将会清空该 mapper 映射下的二 级缓存区域的数据。 

sqlSession2 去查询与 sqlSession1 相同的用户信息，首先会去缓存中找是否存在数据，如果存在直接从 缓存中取出数据。

### 10.2.2. 二级缓存的开启与关闭

#### ①. 第一步：在 SqlMapConfig.xml 文件开启二级缓存

```xml
<settings>
    <!-- 开启二级缓存的支持 -->
    <setting name="cacheEnabled" value="true"/>
</settings>
```

因为 cacheEnabled 的取值默认就为 true，所以这一步可以省略不配置。为 true 代表开启二级缓存；为 false 代表不开启二级缓存。

#### ②. 第二步：配置相关的 Mapper 映射文件

`<cache>` 标签表示当前这个 mapper 映射将使用二级缓存，区分的标准就看 mapper 的 namespace 值。

```xml
<mapper namespace="com.itheima.dao.IUserDao">
    <!--开启user支持二级缓存-->
    <cache/>

    <!-- 查询所有 -->
    <select id="findAll" resultType="user">
        select * from user
    </select>
```

#### ③. 第三步：配置 statement 上面的 useCache 属性

```xml
<!-- 根据id查询用户 -->
<select id="findById" parameterType="INT" resultType="user" useCache="true">
    select * from user where id = #{uid}
</select>
```

将 UserDao.xml 映射文件中的标签中设置 useCache=”true”代表当前这个 statement 要使用 二级缓存，如果不使用二级缓存可以设置为 false。

> 【危险】针对每次查询都需要最新的数据 sql，要设置成 useCache=false，禁用二级缓存。

### 10.2.3. 二级缓存注意事项

当我们在使用二级缓存时，所缓存的类一定要实现 `java.io.Serializable` 接口，这种就可以使用序列化 方式来保存对象。

# 11. Mybatis 注解开发

## 11.1.  mybatis 的常用注解说明

| 注释            | 说明                                   |
| --------------- | -------------------------------------- |
| @Insert         | 实现新增                               |
| @Update         | 实现更新                               |
| @Delete         | 实现删除                               |
| @Select         | 实现查询                               |
| @Result         | 实现结果集封装                         |
| @Results        | 可以与@Result 一起使用，封装多个结果集 |
| @ResultMap      | 实现引用@Results 定义的封装            |
| @One            | 实现一对一结果集封装                   |
| @Many           | 实现一对多结果集封装                   |
| @SelectProvider | 实现动态 SQL 映射                      |
| @CacheNamespace | 实现注解二级缓存的使用                 |

## 11.2. 注解实现基本 CRUD

```Java
//  查询所有用户
@Select("select * from user")
@Results(id="userMap",
    value= {
        @Result(id=true,column="id",property="userId"),
        @Result(column="username",property="userName"),
        @Result(column="sex",property="userSex"),
        @Result(column="address",property="userAddress"),
        @Result(column="birthday",property="userBirthday")
    })
List<User> findAll();

// 根据 id 查询一个用户
@Select("select * from user where id = #{uid} ")
@ResultMap("userMap")
User findById(Integer userId);

// 保存操作
@Insert("insert into user(username,sex,birthday,address)values(#{username},#{sex},#{birthday},#{address})")
@SelectKey(keyColumn="id",keyProperty="id",resultType=Integer.class,before = false, statement = { "select last_insert_id()" })
int saveUser(User user);

// 更新操作
@Update("update user set username=#{username},address=#{address},sex=#{sex},birthday=#{birthday} where id =#{id} ")
int updateUser(User user);

// 删除用户
@Delete("delete from user where id = #{uid} ")
int deleteUser(Integer userId);

// 查询使用聚合函数
@Select("select count(*) from user ")
int findTotal();

// 模糊查询
@Select("select * from user where username like #{username} ")
List<User> findByName(String name);

```

## 11.3. 使用注解实现复杂关系映射开发

实现复杂关系映射之前我们可以在映射文件中通过配置`<resultMap>`来实现，在使用注解开发时我们需要借 助`@Results` 注解，`@Result` 注解，`@One` 注解，`@Many` 注解。

### 11.3.1. 复杂关系映射的注解说明

1. `@Results` 注解 
   - 代替的是`<resultMap>`标签 
   - 该注解中可以使用单个@Result 注解，也可以使用@Result 集合 
   - `@Results（{@Result（），@Result（）}）或@Results（@Result（））`
2. `@Resutl` 注解 
   - 代替了 `<if>`标签和`<result>`标签 
   - @Result 中 属性介绍： 
     - `id` 是否是主键字段 
     - `column` 数据库的列名 
     - `property` 需要装配的属性名 
     - `one` 需要使用的`@One` 注解`（@Result（one=@One）（）））` 
     - `many` 需要使用的`@Many` 注解`（@Result（many=@many）（）））`
3. `@One` 注解（一对一） 
   - 代替了`<assocation>`标签，是多表查询的关键，在注解中用来指定子查询返回单一对象。
   - @One 注解属性介绍： 
     - `select` 指定用来多表查询的 sqlmapper 
     - `fetchType` 会覆盖全局的配置参数 lazyLoadingEnabled。
   - 使用格式： `@Result(column=" ",property="",one=@One(select=""))`
4. `@Many` 注解（多对一） 
   - 代替了标签,是是多表查询的关键，在注解中用来指定子查询返回对象集合。 
   - 注意：聚集元素用来处理“一对多”的关系。需要指定映射的 Java 实体类的属性，属性的 javaType （一般为 ArrayList）但是注解中可以不定义； 
   - 使用格式： `@Result(property="",column="",many=@Many(select=""))`

### 11.3.2. 注解实现一对一复杂关系映射及延迟加载

```java
@Select("select * from account")
@Results(id="accountMap",value = {
    @Result(id=true,column = "id",property = "id"),
    @Result(column = "uid",property = "uid"),
    @Result(column = "money",property = "money"),
    @Result(property = "user",column = "uid",
            /* @One：一对一
            	select：指定用来多表查询的 sqlmapper 
            	fetchType：会覆盖全局的配置参数 lazyLoadingEnabled
            */
            one=@One(select="com.itheima.dao.IUserDao.findById",fetchType= FetchType.EAGER)) 
})
List<Account> findAll();

@Select("select * from user where id = #{uid} ")
@ResultMap("userMap")
User findById(Integer userId);
```

### 11.3.3. 注解实现一对多复杂关系映射

```Java
@Select("select * from user")
@Results(id="userMap",value={
    @Result(id=true,column = "id",property = "userId"),
    @Result(column = "username",property = "userName"),
    @Result(column = "address",property = "userAddress"),
    @Result(column = "sex",property = "userSex"),
    @Result(column = "birthday",property = "userBirthday"),
    @Result(property = "accounts",column = "id",
            /* @Many 一对多
            */
            many = @Many(select = "com.itheima.dao.IAccountDao.findAccountByUid", // 调用下面的
                         fetchType = FetchType.LAZY))
})
List<User> findAll();
```

```Java
@Select("select * from account where uid = #{userId}")
List<Account> findAccountByUid(Integer userId);
```

## 11.4. 基于注解的二级缓存

### 11.4.1. 在 SqlMapConfig 中开启二级缓存支持

```xml
<!-- 配置二级缓存 -->
<settings>
    <!-- 开启二级缓存的支持 -->
    <setting name="cacheEnabled" value="true"/>
</settings>
```

### 11.4.2. 在持久层接口中使用注解配置二级缓存

```Java
@CacheNamespace(blocking=true)//mybatis 基于注解方式实现配置二级缓存
public interface IUserDao {}
```


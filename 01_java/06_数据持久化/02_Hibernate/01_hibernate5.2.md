# 1、Hibernate入门

## 1.1、Hibernate简介

`Hibernate`框架是当今主流的Java持久层框架之一，由于它具有简单易学、灵活性强、扩展性强等特点，能够大大地简化程序的代码量，提高工作效率，因此受到广大开发人员的喜爱。

Hibernate是一个开放源代码的`ORM`框架，它对`JDBC`进行了轻量级的对象封装，使得Java开发人员可以使用面向对象的编程思想来操作数据库。

这里使用版本：hibernate-release-5.0.7.Final

## 1.2、ORM

 `Object Relation Mapping `对象关系映射。

   对象-关系映射（OBJECT/RELATIONALMAPPING，简称ORM），是随着面向对象的软件开发方法发展而产生的。用来把对象模型表示的对象映射到基于SQL的关系模型数据库结构中去。这样，我们在具体的操作实体对象的时候，就不需要再去和复杂的SQL语句打交道，只需简单的操作实体对象的属性和方法。ORM技术是在对象和关系之间提供了一条桥梁，前台的对象型数据和数据库中的关系型的数据通过这个桥梁来相互转化 。

==简单的说就是把我们程序中的实体类和数据库表建立起来对应关系。==

![img](images\01_hibernate5.2\20171126105321823.jpg)

## 1.3、入门程序

### 1.3.1、下载并导入Hibernatesupportjar包

到hibernate官网下载

- 解压目录，目录结构如下：

  ![img](images\01_hibernate5.2\20171127081841661.jpg)

- 建立Java项目
  找到Hibernate根目录下的lib/required，导入里面的所有jar包到项目中：

  ![img](images\01_hibernate5.2\20171127082055654.png)

### 1.3.2、在mysql数据库创建表

```sql
-- 客户表
CREATE TABLE t_customer(
	c_id INT PRIMARY KEY AUTO_INCREMENT,
	c_name VARCHAR(20),
	c_gender CHAR(1),
	c_age INT,
	c_level VARCHAR(20)
);
```



### 1.3.3、建立客户实体类

```java
public class Customer implements Serializable{

	private static final long serialVersionUID = 1L;
	private Integer id;
	private String name;
	private String gender;
	private Integer age;
	private String level;
	// 省略 get / set 方法
}
```



### 1.3.4、写\*.hbm.xml对象映射文件

文件要求：

1. ==实体类名称.hbm.xml==

2. 文件发布的位置：和实体类文件名称发布到同一个目录下

   ![img](images\01_hibernate5.2\20171127082444230.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC 
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping package="com.yiidian.domain">
 	<!-- 
 		name: 类名
 		table: 表名
 	 -->
 	<class name="Customer" table="t_customer">
 		<!-- 主键 -->
 		<id name="id" column="c_id">
 			<generator class="native"></generator>
 		</id>
 		<!-- 其他属性 -->
 		<property name="name" column="c_name"></property>
 		<property name="gender" column="c_gender"></property>
 		<property name="age" column="c_age"></property>
 		<property name="level" column="c_level"></property>
 	</class>		
</hibernate-mapping>   
```



### 1.3.5、写hibernate.cfg.xml文件

> 【注意】：该文件建议项目的src目录下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
	"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
	"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
	
<hibernate-configuration>	
	<!-- 连接数据库的参数 -->
	<session-factory>
		<!-- 1.连接数据库参数 -->
		<property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
		<property name="hibernate.connection.url">jdbc:mysql://localhost:3306/hibernate</property>
		<property name="hibernate.connection.username">root</property>
		<property name="hibernate.connection.password">root</property>
		
		<!-- hibernate方言 -->
		<property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
		
		<!-- 2.hibernate扩展参数 -->
		<property name="hibernate.show_sql">true</property>
		<property name="hibernate.format_sql">true</property>
		<property name="hibernate.hbm2ddl.auto">update</property>
		
		<!-- *.hbm.xml文件 -->
		<mapping resource="com/yiidian/domain/Customer.hbm.xml"/>
	</session-factory>
</hibernate-configuration>	
```

### 1.3.6、写测试代码

```java
@Test
public void test1(){
    Customer customer = new Customer();
    customer.setName("老王");
    customer.setAge(40);
    customer.setGender("男");
    customer.setLevel("VIP客户");

    //1.读取hibernate.cfg.xml文件
    Configuration cfg = new Configuration();
    cfg.configure(); 

    //2.创建SessionFactory工厂
    SessionFactory factory = cfg.buildSessionFactory();

    //3.创建Session对象
    Session session = factory.openSession();

    //4.开启事务
    Transaction tx = session.beginTransaction();

    //5.执行添加操作
    session.save(customer);

    //6.提交事务
    tx.commit();

    //7.关闭资源
    session.close();
}
```

# 2、深入Hibernate

## 2.1、hibernate.cfg.xml

下面对hibernate.cfg.xml配置进行分析。

```xml
<?xml version='1.0' encoding='utf-8'?>  
<!DOCTYPE hibernate-configuration PUBLIC  
        "-//Hibernate/Hibernate Configuration DTD//EN"  
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">  
<hibernate-configuration>  
    <session-factory>  
        <!-- 配置连接数据库的基本信息 jdbc:mysql://localhost:3306/hibernate-->  
        <property name="connection.url">jdbc:mysql://localhost:3306/hibernate</property>  
        <property name="connection.driver_class">com.mysql.jdbc.Driver</property>  
        <property name="connection.username">root</property>  
        <property name="connection.password">root</property>  

        <!-- 配置 hibernate 的基本信息 -->  
        <!--所使用的数据库方言 MySQL5Dialect-->  
        <property name="dialect">org.hibernate.dialect.MySQL5Dialect</property>
        <!-- 执行操作时是否在控制台打印 SQL -->  
        <property name="show_sql">true</property>  
        <!-- 是否对 SQL 进行格式化,例如语句显示会换行 -->  
        <property name="format_sql">true</property>  
        <!-- 指定自动生成数据表的策略 -->  
        <property name="hbm2ddl.auto">update</property>  


        <!-- 配置 C3P0 数据源-->  
        <!--数据库连接池的最大连接数-->  
        <property name="hibernate.c3p0.max_size">10</property>  
        <!--数据库连接池的最小连接数-->  
        <property name="hibernate.c3p0.min_size">5</property>  
        <!--当数据库连接池中的连接耗尽时, 同一时刻获取多少个数据库连接-->  
        <property name="c3p0.acquire_increment">2</property>  
        <!--表示连接池检测线程多长时间检测一次池内的所有链接对象是否超时.  
        连接池本身不会把自己从连接池中移除，而是专门有一个线程按照一定的时间间隔来做这件事，  
        这个线程通过比较连接对象最后一次被使用时间和当前时间的时间差来和 timeout 做对比，进而决定是否销毁这个连接对象。--> 
        <property name="c3p0.idle_test_period">2000</property>  
        <!--数据库连接池中连接对象在多长时间没有使用过后，就应该被销毁-->  
        <property name="c3p0.timeout">2000</property>  
        <!--缓存 Statement 对象的数量-->  
        <property name="c3p0.max_statements">10</property>  


        <!-- 设定 JDBC 的 Statement 读取数据的时候每次从数据库中取出的记录条数 ,取100最好-->  
        <property name="hibernate.jdbc.fetch_size">100</property>  
        <!-- 设定对数据库进行批量删除，批量更新和批量插入的时候的批次大小 取20合适-->  
        <property name="jdbc.batch_size">30</property>  

        <mapping resource="com/yiidian/domain/Customer.hbm.xml"/> 
    </session-factory>  
</hibernate-configuration>  
```

### 2.1.1、hibernate.cfg.xml配置格式

Hibernate配置文件主要用于配置数据库连接和Hibernate运行时所需的各种属性

每个 Hibernate 配置文件对应一个 `Configuration` 对象

Hibernate配置文件可以有两种格式:

1. hibernate.properties（不推荐）
2. hibernate.cfg.xml （推荐）

### 2.1.2、hibernate.cfg.xml的常用属性

#### ①、JDBC 连接属性

1. `connection.url`：数据库URL
2. `connection.username`：数据库用户名
3. `connection.password`：数据库用户密码
4. `connection.driver_class`：数据库JDBC驱动
5. `dialect`：配置数据库的方言，根据底层的数据库不同产生不同的 sql 语句，Hibernate 会针对数据库的特性在访问时进行优化

#### ②、C3P0 数据库连接池属性

1. `hibernate.c3p0.max_size`: 数据库连接池的最大连接数
2. `hibernate.c3p0.min_size`: 数据库连接池的最小连接数
3. `hibernate.c3p0.timeout`:  数据库连接池中连接对象在多长时间没有使用过后，就应该被销毁
4. `hibernate.c3p0.max_statements`:  缓存 `Statement` 对象的数量
5. `hibernate.c3p0.idle_test_period`:  表示连接池检测线程多长时间检测一次池内的所有链接对象是否超时. 连接池本身不会把自己从连接池中移除，而是专门有一个线程按照一定的时间间隔来做这件事，这个线程通过比较连接对象最后一次被使用时间和当前时间的时间差来和 timeout 做对比，进而决定是否销毁这个连接对象。
6. `hibernate.c3p0.acquire_increment`: 当数据库连接池中的连接耗尽时, 同一时刻获取多少个数据库连接
7. `show_sql`：是否将运行期生成的SQL输出到日志以供调试。取值 true | false
8. `format_sql`：是否将 SQL 转化为格式良好的 SQL . 取值 true | false
9. `hbm2ddl.auto`：在启动和停止时自动地创建，更新或删除数据库模式。取值 create | update | create-drop | validate
   -  `validate`：当sessionFactory创建时，自动验证或者schema定义导入数据库。 
   -  `create`：每次启动都drop掉原来的schema，创建新的。 
   -  `create-drop`：当sessionFactory明确关闭时，drop掉schema。 
   -  `update`(常用)：如果没有schema就创建，有就更新。 
10. `hibernate.jdbc.fetch_size`：Hibernate每次从数据库中取出并放到JDBC的Statement中的记录条数。Fetch Size设的越大，读数据库的次数越少，速度越快，Fetch Size越小，读数据库的次数越多，速度越慢
11. `hibernate.jdbc.batch_size`：Hibernate批量插入,删除和更新时每次操作的记录数。Batch Size越大，批量操作的向数据库发送Sql的次数越少，速度就越快，同样耗用内存就越大

## 2.2、*.hbm.xml映射文件

![img](images\01_hibernate5.2\20171127085926777.png)

### 2.2.1、`<class>`标签 

**作用**：用来将类与数据库表建立映射关系

**属性**：

1. `name`：类的全路径
2. `table`：表名.(类名与表名一致,那么table属性也可以省略)
3. `catalog` ：数据库的名称，基本上都会省略不写

### 2.2.2、`<id>`标签 

**作用**：用来将类中的属性与表中的主键建立映射，id标签就是用来配置主键的。

**属性**：

1. `name`：类中属性名
2. `column`：表中的字段名.(如果类中的属性名与表中的字段名一致,那么column可以省略.)
3. `length`：字段的长度，如果数据库已经创建好了，那么length可以不写。如果没有创建好，生成表结构时，length最好指定。

### 2.2.3、`<property>`标签

**作用**：用来将类中的普通属性与表中的字段建立映射.

**属性**：

1. `name`：类中属性名
2. `column`：表中的字段名.(如果类中的属性名与表中的字段名一致,那么column可以省略.)
3. `length`：数据长度
4. `type`：数据类型（一般都不需要编写，如果写需要按着规则来编写）
   - Hibernate的数据类型：`type="string"`
   - Java的数据类型：`type="java.lang.String"`
   - 数据库字段的数据类型： `<column name="name" sql-type="varchar"/>`

### 2.2.4、`<generator>`标签

```xml
<id name="id" column="表主键字段名" type="java.lang.Integer">
    <generator class="设置主键生成策略类型"/>
</id>
```

#### ①、常见的主键策略 

1. **increment**   

   - 对主键值采取自动顺序增长的方式生成新的主键，值默认从1开始。

   - 原理:在当前应用实例中维持一个变量,以保存当前最大值,之后每次需要生成主键值的时候将此值加1作为主键.不依赖于底层的数据库，因此所有的数据库都可以使用

   - > 【注意】缺点：通过increment的生成主键的原理可推断，此种主键生成策略==不适用于集群、同一时段大量用户并发访问的系统==，既当大量用户同一时间段同时进行插入操作的时候，可能存在取得相同的最大值然后再同时+1的情况,这个时候就会造成主键冲突。因此，如果同一数据库有多个实例访问，此方式必须避免使用。

2. **UUID**

   - **原理**：UUID使用128位UUID算法生成主键，能够保证网络环境下的主键唯一性，也就能够保证在不同数据库及不同服务器下主键的唯一性。所以使用于所有数据库。

   - > 【注意】缺点：能够保证数据库中的主键唯一性，但是在生成的主键占用比较多的存贮空间 

3. **Hilo**  

   - **原理**：通过hi/lo 算法(Hilo使用高低位算法生成主键，高低位算法使用一个高位值和一个低位值，然后把算法得到的两个值拼接起来)实现的主键生成机制，需要额外的数据库表保存主键生成历史状态。
   - **特点**：需要额外的数据库表和字段提供高位值来源。默认情况下使用的表是==hibernate_unique_key==，默认字段叫作next_hi。next_hi必须有一条记录否则会出现错误。需要额外的数据库表的支持，能保证同一个数据库中主键的唯一性，但不能保证多个数据库之间主键的唯一性。Hilo主键生成方式由Hibernate 维护，所以Hilo方式与底层数据库无关。

4. **sequence** 

   - sequence实际是就是一张单行单列的表。
   - **实现原理**：调用数据库中底层存在的sequence生成主键，需要底层数据库的支持序列，因此他是依赖于数据库的。
   - 支持sequence的数据库有:Oracle 、DB2(Mysql/SQlServer不支持)、PostgreSql、SAPDb等

5. **identity**

   - 根据底层数据库，来支持自动增长，不同的数据库用不同的主键增长方式。
   - **特点**：与底层数据库有关，要求数据库支持Identity，如MySQl中是auto_increment, SQL Server 中是Identity。支持的数据库有MySql、SQL Server、DB2、Sybase和HypersonicSQL。
   - **好处**：在建表的时候指定了id为自动增长,实际开发中就不需要自己定义插入数据库的主键值,系统会自动顺序递增一个值 。Identity无需Hibernate和用户的干涉，使用较为方便，==但由于依赖于数据库，所以不便于在不同的数据库之间移植程序。==　

6. **native** 

   - **作用**：根据数据库的类型,自动在sequence 、identity和,hilo进行切换。
   - **实现自动切换的依据**：根据Hibernate配置文件中的方言来判断是Oracle还是Mysql、SqlServer,然后针对数据库的类型抉择 sequence还是identity作为主键生成策略。
   - **用处**：由于Hibernate会根据底层数据库采用不同的映射方式，因此灵活性高，便于程序移植，项目中如果用到多个数据库时，可以使用这种方式。

7. **assigned**   

   - **作用**：用于手工分配主键生成器,一旦指定为这个了,Hibernate就不在自动为程序做主键生成器了。没有指定`<generator>`标签时，默认就是assigned主键的生成方式
   - **使用方法**：在程序中session.save();之前,由程序员自己指定主键值为多少。
     例如:user.setId(1);这就是在程序中程序员手动为用户表指定主键值为1。  

8. **foreign**   

   - 只适用基于共享主键的一对一关联映射的时候使用。即一个对象的主键是参照的另一张表的主键生成的。



#### ②、Hibernate主键生成策略分类

- `UUID，increment、Hilo、assigned`：对数据库无依赖
- `identity`：依赖Mysql或sql server，主键值不由hibernate维护
- `sequence`：适合于oracle等支持序列的dbms，主键值不由hibernate维护，由序列产生。
- `native`：根据底层数据库的具体特性选择适合的主键生成策略，如果是mysql或sqlserver，选择identity，如果是oracle，选择sequence。



#### ③、Hibernate主键生成策略的选择  

一般来说推荐`UUID`，因为生成主键唯一，且对数据库无依赖，可移植性强。


由于常用的数据库，如Oracle、DB2、SQLServer、MySql 等，都提供了易用的主键生成机制（Auto-Increase 字段或者Sequence）。我们可以在数据库提供的主键生成机制上，采用native，sequence或者identity的主键生成方式。

不过值得注意的是，一些数据库提供的主键生成机制在效率上未必最佳大量并发insert数据时可能会引起表之间的互锁。

因此，对于并发Insert要求较高的系统，推荐采用uuid作为主键生成机制。

总之，hibernate主键生成器选择，还要具体情况具体分析。一般而言，利用uuid方式生成主键将提供最好的性能和数据库平台适应性。

## 2.3、Configuration

### 2.3.1、Configuration包含的信息

Configuration类负责管理Hibernate的配置信息。

包括如下内容：

- Hibernate运行的底层信息： 数据库的URL、用户名、密码、JDBC驱动类，数据库Dialect,数据库连接池等。
- 持久化类与数据表的映射关系（*.hbm.xml 文件）

### 2.3.2、创建Configuration 的两种方式

属性文件（hibernate.properties）: 

- ```java
  Configuration cfg = new Configuration();
  ```

  

Xml文件（hibernate.cfg.xml）（推荐使用）   

- ```java
  Configuration cfg = new Configuration().configure();
  ```

  

 Configuration 的 configure 方法还支持带参数的访问：  

- ```java
  File file = new File("hibernte.cfg.xml");
  Configuration cfg = new Configuration().configure(file);
  ```

## 2.4、SessionFactory

SessionFactory是工厂类，是生成Session对象的工厂类。

### 2.4.1、SessionFactory类的特点   

- 由Configuration通过加载配置文件创建该对象。
- SessionFactory对象中保存了当前的数据库配置信息和所有映射关系以及预定义的SQL语句。同时，SessionFactory还负责==维护Hibernate的二级缓存==。
- 预定义SQL语句
  使用Configuration类创建了SessionFactory对象是，已经在SessionFacotry对象中缓存了一些SQL语句，常见的SQL语句是增删改查（通过主键来查询），这样做的目的是效率更高   

- 一个SessionFactory实例对应一个数据库，应用从该对象中获得Session实例。
- SessionFactory是==重量级==的，意味着不能随意创建或销毁它的实例。如果只访问一个数据库，==只需要创建一个SessionFactory实例==，且在应用初始化的时候完成。
- SessionFactory需要一个较大的缓存，用来存放预定义的SQL语句及实体的映射信息。另外可以配置一个缓存插件，这个插件被称之为Hibernate的二级缓存，被多线程所共享

###  2.4.2、使用SessionFactory注意事项

一般应用使用一个`SessionFactory`,最好是应用启动时就完成初始化。

## 2.5、Session

### 2.5.1、Session概述

1. `Session`是在Hibernate中使用==最频繁==的接口。也被称之为持久化管理器。它提供了和持久化有关的操作，比如添加、修改、删除、加载和查询实体对象。
2. Session 是应用程序与数据库之间交互操作的一个单线程对象，是Hibernate 运作的中心
3. 所有持久化对象必须在 session 的管理下才可以进行持久化操作
4. Session 对象有一个==一级缓存==，显式执行 flush 之前，所有的持久化操作的数据都缓存在 session 对象处
5. 持久化类与 Session 关联起来后就具有了持久化的能力 

### 2.5.2、Session的特点

1. ==不是线程安全==的。应避免多个线程使用同一个Session实例
2. Session是==轻量级==的，它的创建和销毁不会消耗太多的资源。应为每次客户请求分配独立的Session实例
3. Session有一个缓存，被称之为Hibernate的一级缓存。每个Session实例都有自己的缓存

### 2.5.3、Session常用的方法

- `save(obj)`
- `delete(obj)`  
- `get(Class,id)`
- `update(obj)`
- `saveOrUpdate(obj)`：保存或者修改（如果没有数据，保存数据。如果有，修改数据）
- `createQuery()` ： HQL语句的查询的方式



## 2.6、Transaction

### 2.6.1、Transaction概念

`Transaction`代表一次原子操作，它具有数据库事务的概念。所有持久层都应该在事务管理下进行，即使是只读操作。     

```java
Transaction tx = session.beginTransaction();
```

### 2.6.2、Transaction常用方法

- `commit()`：提交相关联的session实例
- `rollback()`：撤销事务操作

### 2.6.3、Transaction特点   

- Hibernate框架默认情况下事务不自动提交.需要手动提交事务
- 如果没有开启事务，那么每个Session的操作，都相当于一个独立的事务

## 2.7、对象状态

### 2.7.1、Hibernate的持久化类的状态  

Hibernate为了管理持久化类：将持久化类分成了三个状态

|                                     | 持久化标识OID      | 纳入到Session对象的管理 |
| ----------------------------------- | ------------------ | ----------------------- |
| 瞬时态`<Transient  Obje>`           | :x:                | :x:                     |
| 持久态`<Persistent Object>`         | :heavy_check_mark: | :heavy_check_mark:      |
| 脱管态（离线态）`<Detached Object>` | :heavy_check_mark: | :x:                     |

### 2.7.2、持久化对象的状态的转换

![持久化对象的状态转换](images\01_hibernate5.2\5-1Z62416115H48.png)

#### ①、瞬时态

**说明**：没有持久化标识OID, 没有被纳入到Session对象的管理

```java
// 获得瞬时态的对象
User user = new User();

// 瞬时态对象  转换  持久态。下面两种方式
session.save();
session.saveOrUpdate();

// 瞬时态对象  转换  脱管态
user.setId(1)
```



#### ②、持久态

**说明**：有持久化标识OID,已经被纳入到Session对象的管理

```java
// 获得持久态的对象   。下面两种方式   
session.get();
session.load();

// 持久态  转换  瞬时态对象
session.delete(); 

// 持久态对象  转成  脱管态对象   。下面三种方式   
session.close();
session.evict();
session.clear();
```



#### ③、脱管态

**说明**：有持久化标识OID,没有被纳入到Session对象的管理

```java
// 获得托管态对象:不建议直接获得脱管态的对象. 
User user = new User();
user.setId(1);

// 脱管态对象  转换成  持久态对象 。下面三种方式   
session.update();
session.saveOrUpdate();
session.lock();
      
// 脱管态对象  转换成  瞬时态对象
user.setId(null);
```

## 2.8、一级缓存和快照

### 2.8.1、一级缓存

Hibernate的一级缓存就是指Session缓存。通过查看`Session`接口的实现类——`SessionImpl.java`的源码可发现有如下两个类：

![img](images\01_hibernate5.2\20171129091552645.png)

- `actionQueue`它是一个行动队列，它主要记录crud操作的相关信息。
- `persistenceContext`它是持久化上下文，它其实才是真正的缓存。



在`Session`中定义了一系列的集合来存储数据，它们构成了`Session`的缓存。只要`Session`没有关闭，它就会一直存在。当我们通过Hibernate中的`Session`提供的一些API例如`save()、get()、update()`等进行操作时，就会将持久化对象保存到`Session`中，当下一次再去查询缓存中具有的对象(通过OID值来判断)，就不会去从数据库中查询了，而是直接从缓存中获取。Hibernate的一级缓存存在的目的就是为了减少对数据库的访问。 



#### ①、自动更新数据库

```java
public void test2() {
    // 1.得到session
    Session session = HibernateUtil.getSession();
    session.beginTransaction();
    // 查询id=1的Customer对象，如果查询到，会将Customer对象存到一级缓存中
    Customer customer = session.get(Customer.class, 1); 
    customer.setName("Tom"); // 操作持久化对象来修改属性
    
    // 2.事务提交，并关闭session。虽然没有执行save方法，但是Hibernate会自动执行保存
    session.getTransaction().commit();
    session.close();
}
```

为什么持久化对象具有自动更新数据库的能力呢？

- 原因涉及到一个概念——快照，**快照就是当前一级缓存里面对象的散装数据(对象的属性，如name、id……)**。当执行完以下这句代码：

  ```java
  Customer customer = session.get(Customer.class, 1);
  ```

  就会向一级缓存中存储数据，一级缓存其底层使用了一个Map集合来存储，Map的key存储的是一级缓存对象，而value存储的是快照。



#### ②、一级缓存常用API

**一级缓存特点**：

- 当我们通过`session`的`save、update、saveOrUpdate`方法进行操作时，如果一级缓存中没有对象，那么会从数据库中查询到这些对象，并存储到一级缓存中。
- 当我们通过`session`的`load`、`get`、`Query`的`list`等方法进行操作时，会先判断一级缓存中是否存在数据，如果没有才会从数据库获取，并且将查询的数据存储到一级缓存中。
- 当调用`session`的`close`方法时，`session`缓存将清空。



**一级缓存常用的**API：

- `clear()`：清空一级缓存。
- `evict()`：清空一级缓存中指定的某个对象。
- `refresh()`：重新查询数据库，用数据库中的信息来更新一级缓存与快照区。

# 3、映射关系

## 3.1、一对多关系（客户与订单）

### 3.1.1、表关系

![img](images\01_hibernate5.2\20171128082028188.png)

### 3.1.2、实体对象

```java
// 客户（一方）
public class Customer {
	private Integer id;
	private String name;
	private String gender;
	
	//关联订单。【【一对多关系】】
	private Set<Order> orders = new HashSet<Order>();
	// 省略 get / set 方法
}

// 订单（多方）
public class Order {

	private Integer id;
	private String orderno;
	private String productName;
	
	//关联客户。【【多对一关系】】
	private Customer customer;
}
```

### 3.1.3、映射文件

#### ①、一对多：客户

```xml
<hibernate-mapping package="com.yiidian.domain">
    <class name="Customer" table="t_customer">
        <id name="id" column="id">
            <generator class="native"></generator>
        </id>
        <property name="name" column="name"></property>
        <property name="gender" column="gender"></property>

        <!-- 【【注意】】 一对多配置 -->
        <set name="orders">
            <!-- 外键字段名称 -->
            <key column="cust_id"></key>
            <one-to-many class="Order"/>
        </set>
    </class>
</hibernate-mapping>   
```

> 【说明】
>
> 1. `<set>`标签
>    - 作用：映射集合属性
>    - 属性：
>      - `name`：指定集合属性的名称
>      - `table`：指定对方（多方）表的名称。若为多对多映射，则指定为中间表
> 2. `<key>`标签
>    - 作用：指定外键
>    - 属性：
>      - `column`：指定对方表中外键字段的名称
> 3. `<one-to-many>`标签
>    - 作用：描述当前实体和元素集合中的元素之间的关系是一对多
>    - 属性：
>      - `class`：指定的是对方的实体类名称



#### ②、多对一：订单

```xml
<hibernate-mapping package="com.yiidian.domain">
    <class name="Order" table="t_order">
        <id name="id" column="id">
            <generator class="native"></generator>
        </id>
        <property name="orderno" column="orderno"></property>
        <property name="productName" column="product_name"></property>

        <!-- 【【注意】】 多对一配置 -->
        <many-to-one name="customer" class="Customer" column="cust_id"/>
    </class>
</hibernate-mapping>   
```

> 【说明】
>
> 1. `<many-to-one>`标签
>    - 作用：描述多对一的关系
>    - 属性：
>      - `name`：指定实体类中要映射的属性名称
>      - `class`：指定的是对方（一方）所对应的实体类
>      - `cloumn`：指定表中外键字段的名称

## 3.2、多对多关系（用户与角色）

### 3.2.1、表关系

![img](images\01_hibernate5.2\20171128092519666.png)

### 3.2.2、实体对象

```java
// 用户（多方）
public class User implements Serializable{
	private Integer id;
	private String name;
	
	//关联角色。【【多对多关系】】
	private Set<Role> roles = new HashSet<Role>();
}

// 角色（多方）
public class Role implements Serializable{
	private Integer id;
	private String name;
	
	//关联用户。【【多对多关系】】
	private Set<User> users = new HashSet<User>();
}
```

### 3.2.3、映射文件

#### ①、多对多：用户

```xml
<hibernate-mapping package="com.yiidian.domain">
    <class name="User" table="t_user">
        <id name="id" column="id">
            <generator class="native"></generator>
        </id>
        <property name="name" column="name"></property>

        <!-- 多对多映射 -->
        <!-- 
                table:中间表名
        -->
        <set name="roles" table="t_user_role">
            <!-- 当前方在中间表的外键 -->
            <key column="user_id"/>
            <!-- column:对方在中间表的外键 -->
            <many-to-many class="Role" column="role_id"/>
        </set>
    </class>
</hibernate-mapping>   
```



#### ②、多对多：角色

```xml
<hibernate-mapping package="com.yiidian.domain">

    <class name="Role" table="t_role">
        <id name="id" column="id">
            <generator class="native"></generator>
        </id>
        <property name="name" column="name"></property>

        <!-- 多对多映射 -->
        <!-- 
                table:中间表名
        -->
        <set name="users" table="t_user_role" inverse="true">
            <!-- 当前方在中间表的外键 -->
            <key column="role_id"/>
            <!-- column:对方在中间表的外键 -->
            <many-to-many class="User" column="user_id"/>
        </set>
    </class>

</hibernate-mapping> 
```



## 3.3、一对一关系（公民与身份证）

### 3.3.1、唯一外键关联

#### ①、表关系

![img](images\01_hibernate5.2\20171128095347634.png)



#### ②、实体对象<a id="实体对象"></a>

```java
// 公民
public class Person implements Serializable{
    private Integer id;
    private String name;

    //关联身份证
    private Card card;
}

// 身份证
public class Card implements Serializable{
    private Integer id;
    private String name;
    //关联公民
    private Person person;
}
```



#### ③、映射文件

**公民映射**<a id="公民映射"></a>

```xml
<hibernate-mapping package="com.yiidian.domain">
    <class name="Person" table="t_person_pk">
        <id name="id" column="id">
            <generator class="native"></generator>
        </id>
        <property name="name" column="name"></property>

        <!-- 一对一 -->
        <one-to-one name="card" class="Card"/>
    </class>
</hibernate-mapping>  
```



**身份证映射**

```xml
<hibernate-mapping package="com.yiidian.domain">
    <class name="Card" table="t_card_fk">
        <id name="id" column="id">
            <generator class="native"></generator>
        </id>
        <property name="name" column="name"></property>

        <!-- 唯一外键（一对一) -->
        <many-to-one name="person" class="Person" column="person_id" unique="true"/>
    </class>
</hibernate-mapping>   
```

> 【注意】在`<many-to-one>`标签中指定了 `unique`属性（true：约束唯一标识数据库表中的每条记录）

### 3.3.2、主键关联

#### ①、表关系

![img](images\01_hibernate5.2\20171128102909895.png)



#### ②、实体对象

同[3.3.1、唯一外键关联中的【②、实体对象】](#实体对象)



#### ③、映射文件

**公民映射**：同[3.3.1、唯一外键关联 中的【③、映射文件——公民映射】](#公民映射)



**身份证映射**

```xml
<hibernate-mapping package="com.yiidian.domain">
    <class name="Card" table="t_card_pk">
        <id name="id" column="id">
            <generator class="native"></generator>
        </id>
        <property name="name" column="name"></property>

        <!-- 主键关联（一对一) -->
        <one-to-one name="person" class="Person" constrained="true"/>
    </class>
</hibernate-mapping>   
```

> 【注意】在`<one-to-one>`标签中指定了 `constrained`属性（true：表明存在外键与关联表对应，并且关联表中肯定存在对应的键与其对应）

# 4、映射关系高级操作

## 4.1、Cascade和Inverse

### 4.1.1、Cascade

**作用**：级联，就是对一个对象进行操作的时候，会把他相关联的对象也一并进行相应的操作

cascade属性的属性值

- | 属性值            | 描  述                                                  |
  | ----------------- | ------------------------------------------------------- |
  | save-update       | 在执行 save、update 或 saveOrUpdate 吋进行关联操作      |
  | delete            | 在执行 delete 时进行关联操作                            |
  | delete-orphan     | 删除所有和当前解除关联关系的对象                        |
  | all               | 所有情况下均进行关联操作，但不包含 delete-orphan 的操作 |
  | all-delete-orphan | 所有情况下均进行关联操作                                |
  | none              | 所有情况下均不进行关联操作，这是默认值                  |



### 4.1.2、Inverse

**作用**：控制关联的双方由哪一方管理关联关系

inverse 属性值是 boolean 类型的：

- 当取值为 false（默认值）时，表示由当前这一方管理双方的关联关系，如果双方 inverse 属性都为 false 时，双方将同时管理关联关系。
- 取值为 true 时，表示当前一方放弃控制权，由对方管理双方的关联关系。

> 【注意】通常情况下
>
> 1. 在一对多关联关系中，会将==“一”的一方的 inverse 属性取值为 true==，即由“多”的一方维护关联关系，否则会产生多余的 SQL 语句；
> 2. 在多对多的关联关系中，任意设置一方的 inverse 属性为 true 即可。

### 4.1.3、一起联用（客户与订单）

客户映射文件（**一方**）

- ```xml
  <hibernate-mapping package="com.yiidian.domain">
      <class name="Customer" table="t_customer">
          <id name="id" column="id">
              <generator class="native"></generator>
          </id>
          <property name="name" column="name"></property>
          <property name="gender" column="gender"></property>
  
          <!-- 【【注意】】 一对多配置。inverse：放弃维护关联关系 -->
          <set name="orders" inverse="true" cascade="all">
              <!-- 外键字段名称 -->
              <key column="cust_id"></key>
              <one-to-many class="Order"/>
          </set>
      </class>
  </hibernate-mapping>   
  ```

  

订单映射文件（**多方**）

- ```xml
  <hibernate-mapping package="com.yiidian.domain">
      <class name="Order" table="t_order">
          <id name="id" column="id">
              <generator class="native"></generator>
          </id>
          <property name="orderno" column="orderno"></property>
          <property name="productName" column="product_name"></property>
  
          <!-- 【【注意】】 多对一配置 inverse：默认为false，维护级联关系-->
          <many-to-one name="customer" class="Customer" column="cust_id"  cascade="all"/>
      </class>
  </hibernate-mapping>   
  ```

## 4.2、lazy延迟加载

### 4.2.1、延迟加载的概念

当Hibernate从数据库中加载某个对象时，不加载关联的对象，而只是生成了代理对象，获取使用`session`中的`load`的方法(在没有改变`lazy`属性为`false`的情况下)获取到的也是代理对象，所以在上面这几种场景下就是延迟加载。

### 4.2.2、**理解立即加载的概念**

当Hibernate从数据库中加载某个对象时，加载关联的对象，生成的实际对象，获取使用session中的`get`的方法获取到的是实际对象。

### 4.2.3、为什么要使用延迟加载

延迟加载策略能避免加载应用程序不需要访问的关联对象，以提高应用程序的性能。

### 4.2.4、立即加载的缺点

Hibernate在查询某个对象时，立即查询与之关联的对象，我们可以看出这种加载策略存在两大不足：

1. select的语句数目太多，需要频繁的访问数据库，会影响查询的性能。
2. 在应用程序只需要访问要的对象，而不需要访问与他关联的对象的场景下，加载与之关联的对象完全是多余的操作，这些多余的操作是会占内存，这就造成了内存空间的浪费。

### 4.2.5、什么时候使用延迟加载/立即加载

如果程序加载一个持久化对象的目的是为访问他的属性，则可以采用立即加载。

如果程序加载一个持久化对象的目的仅仅是为了获得他的引用，则可以采用延迟加载。

### 4.2.6、关系映射中配置加载策略

#### ①、类级别

`<class>`元素中lazy属性的可选值为`true`(延迟加载)和`false`(立即加载)；

`<class>`元素中的lazy属性的默认值为`true`



#### ②、一对多关联级别：

`<set>`元素中的`lazy`属性的可选值为：`true`(延迟加载)、`extra`(增强延迟加载)和`false`(立即加载);

- 其实增强延迟加载策略与一般的延迟加载策略`lazy="true"`非常相似。他们主要区别在于，我们看到这个名词增强延迟加载，顾名思义就是这个策略能在进一步的帮我延迟加载这个对象，也就是代理对象的初始化时机。

`<set>`元素中的lazy属性的默认值为`true`



#### ③、多对一关联级别：

`<many-to-one>`元素中`lazy`属性的可选值为：`proxy`(延迟加载)、`no-proxy`(无代理延迟加载)和`false`(立即加载)

- `lazy`属性为`no-proxy`时，则可以避免使用由Hibernate提供的Dept代理类实例，是Hibernate对程序提供更加透明的持久化服务

`<many-to-one>`元素中的`lazy`属性的默认值为`proxy`



## 4.3、fetch抓取策略

抓取策略,指的是使用Hibernate查询一个对象的时候，查询其关联对象.应该如何查询.是Hibernate的一种优化手段!

### 4.3.1、连接抓取（Join fetching） 

Hibernate通过 在`SELECT`语句使用`OUTER JOIN` （外连接）来 获得对象的关联实例或者关联集合。

![img](images\01_hibernate5.2\20171130054048323.png)

配置之后，原来查询需要两个SQL（一个主表、一个副表），现在查询用==左连接==的方式一条SQL

### 4.3.2、查询抓取（Select fetching） 

另外发送一条 `SELECT` 语句抓取当前对象的关联实体或集合。除非你显式的指定`lazy="false"`禁止 延迟抓取（lazy fetching），否则只有当你真正访问关联关系的时候，才会执行第二条select语句。

查询抓取, 这种策略是在集合抓取的时候的==默认策略==, 即如果集合需要初始化, 那么会重新发出一条 SQL 语句进行查询; 这是集合默认的抓取策略, 也就是我们常会出现N+1次查询的查询策略;



### 4.3.3、子查询抓取（Subselect fetching） 

另外发送一条`SELECT` 语句抓取在前面查询到（或者抓取到）的所有实体对象的关联集合。除非你显式的指定`lazy="false"` 禁止延迟抓取（lazy fetching），否则只有当你真正访问关联关系的时候，才会执行第二条 `select`语句。

![img](images\01_hibernate5.2\20171130072117538.png)



### 4.3.4、批量抓取（Batch fetching）

 对查询抓取的优化方案， 通过指定一个主键或外键列表，Hibernate使用单条`SELECT`语句获取一批对象实例或集合。

![img](images\01_hibernate5.2\20171130081934473.png)

# 5、检索方式

Hibernate的检索方式主要有五种，包括

1. 导航对象图检索方式
2. OID 检索方式
3. HQL 检索方式
4. QBC 检索方式
5. 本地 SQL 检索方式

## 5.1、导航对象图检索方式

导航对象图检索方式是根据已经加载的对象，导航到其他对象。它利用类与类之间的关系检索对象。

```java
Student student = (Student)session.get(Student.class,1);
Grade grade = student.getGrade(); // 根据已知对象student，获取Grade对象。这里就是导航对象图检索方式
```



## 5.2、OlD检索方式

OID 检索方式是指按照对象的 OID 检索对象。它使用 `Session` 对象的 `get()` 和 `load()` 方法加载某一条记录所对应的对象，其使用的前提是需要事先知道 OID 的值。该检索方式的示例代码如下所示：

```java
public void test1(){
    Session session = HibernateUtil.getSession();
    Transaction tx = session.beginTransaction();

    Customer cust = session.get(Customer.class,1); // get 方法
    // Customer cust = session.load(Customer.class,1); // load 方法
    Set<Order> orders = cust.getOrders();
    System.out.println(cust.getName()+"的订单：");
    for (Order order : orders) {
        System.out.println(order.getOrderno());
    }

    tx.commit();
    session.close();
}
```

### 5.2.1、两者区别

#### ①、load加载方式

当使用`load`方法来得到一个对象时，此时hibernate会使用延迟加载的机制来加载这个对象，即：当我们使用`session.load()`方法来加载一个对象时，此时并不会发出sql语句，当前得到的这个对象其实是一个==代理对象==，这个代理对象只保存了实体对象的id值，只有当我们要==使用这个对象==，得到其它属性时，这个时候==才会发出sql语句==，从数据库中去查询我们的对象。



#### ②、get加载方式

get就==直接==的多，当我们使用`session.get()`方法来得到一个对象时，不管我们使不使用这个对象，此时都会发出sql语句去从数据库中查询出来

### 5.2.2、两者小问题

#### ①、get问题

如果使用get方式来加载对象，当我们试图得到一个==id不存在==的对象时，此时会报`NullPointException`的异常



#### ②、load问题

如果使用load方式来加载对象，当我们试图得到一个==id不存在==的对象时，此时会报`ObjectNotFoundException`异常：

为什么使用load的方式和get的方式来得到一个不存在的对象报的异常不同呢？？
其原因还是因为load的延迟加载机制，使用load时，此时的user对象是一个代理对象，仅仅保存了当前的这个id值，当我们试图得到该对象的username属性时，这个属性其实是不存在的，所以就会报出`ObjectNotFoundException`这个异常了



#### ③、org.hibernate.LazyInitializationException异常

```java
public User loadUser(int id)
{
    Session session = null;
    Transaction tx = null;
    User user =  null;
    
    session = HibernateUtil.openSession();
    tx = session.beginTransaction();
    user = (User)session.load(User.class, 1);
    tx.commit();
    session.close(); // 关闭 session
    
    return user;
}
// 在其他方法中，调用loadUser方法获取User对象时，会出现异常：LazyInitializationException
```

这个异常是什么原因呢？？
因为`load`的延迟加载机制，当我们通过`load()`方法来加载一个对象时，此时并没有发出sql语句去从数据库中查询出该对象，当前这个对象仅仅是一个只有id的代理对象，我们还并没有使用该对象，但是此时我们的`session`已经关闭了，所以当我们在测试用例中使用该对象时就会报`LazyInitializationException`这个异常了。

所以以后我们只要看到控制台报`LazyInitializationException`这种异常，就知道是使用了`load`的方式延迟加载一个对象了，解决这个的方法有两种，一种是将`load`改成`get`的方式来得到该对象，另一种是在表示层来开启我们的`session`和关闭`session`。



## 5.3、HQL检索方式

HQL（Hibernate Query Language）是 Hibernate 查询语言的简称，它是一种面向对象的查询语言，与 SQL 查询语言有些类似，但它使用的是类、对象和属性的概念，而没有表和字段的概念。



HQL 查询与 SQL 查询相比，具有以下优点。

- 直接针对实体类和属性进行查询，不用再编写繁琐的 SQL 语句。
- 查询结果直接保存在 List 集合中，不用再次封装。
- 针对不同的数据库会自动生成不同的 SQL 语句。



在 Hibernate 提供的几种检索方式中，HQL 是官方推荐的查询语言，也是使用最频繁的一种检索方式，其具有以下主要功能。

- 在查询语句中设定各种查询条件。
- 支持投影查询，即仅检索出对象的部分属性。
- 提供内置聚集函数，如 sum()、min() 和 max()。
- 支持分组查询，允许使用 group by 和 having 关键字。
- 支持分页查询。
- 支持子查询，即嵌套查询。
- 支持动态绑定参数。



HQL 的完整语法格式如下所示：

- ```sql
  [select/update/delete...]from...[where...][group by...][having...][order by...][asc/desc]
  ```

> 【注意】表名为映射的实体类名，因此需要区分大小写，而 from 关键字不区分大小写

### 5.3.1、指定别名

HQL 语句与 SQL 语句类似，也可以使用 as 关键字指定别名。在使用别名时，as 关键字可以省略

```java
String hql = "from User as u where u.name='zhangsan '"; // 编写 HQL
Query query = session.createQuery(hql); // 创建 Query对象
List<User> list = query.list(); // 执行查询，获得结果
```

### 5.3.2、投影查询

 Hibernate 的投影查询方式查询对象的部分属性。一共两种方式。

```java
String hql = "select u.name, u.age from User as u"; // 编写 HQL
Query query = session.createQuery(hql); // 创建Query对象
List<User> list = query.list(); // 执行查询，获得结果
```

```java
Query query = session.createQuery("select new com.yiidian.domain.User(name,age) from User");
List<Order> list = query.list();
```

### 5.3.3、条件查询

#### ①、 按参数位置查询

按参数位置查询时，需要在 HQL 语句中使用“?”定义参数的位置，然后通过 Query 对象的 `setXxx()` 方法为其赋值，这类似于 JDBC 的 `PreparedStatement` 对象的参数绑定方式。

Query对象为参数赋值的常用方法

- | 方法名           | 说  明                          |
  | ---------------- | ------------------------------- |
  | `setString()`    | 给映射类型为 String 的参数赋值  |
  | `setDate()`      | 给映射类型为 Date 的参数赋值    |
  | `setDouble()`    | 给映射类型为 double 的参数赋值  |
  | `setBoolean()`   | 给映射类型为 boolean 的参数赋值 |
  | `setInteger()`   | 给映射类型为 int 的参数赋值     |
  | `setTime()`      | 给映射类型为 Date 的参数赋值    |
  | `setParameter()` | 给任意类型的参数赋值            |

```java
String hql = "from User where name like ?"; // 编写 HQL,使用参数查询
Query query = session.createQuery(hql); // 创建 Query对象
query.setString(0, "%ang%"); // 为 HQL中的”？”代表的参数设置值
List<User> list = query.list(); // 执行查询，获得结果
```



#### ②、按参数名字查询

按参数名字查询时，需要在 HQL 语句中定义命名参数，命名参数是==“：”和自定义参数名==的组合。

```java
String hql = "from User where id =:id"; // 编写 HQL
Query query = session.createQuery(hql); // 创建 Query对象
query.setParameter("id", 4); // 添加参数
List<User> list = query.list(); // 执行查询，获得结果
```



### 5.3.4、分页查询

在 Hibernate 的 Query 接口中，提供了两个用于分页显示查询结果的方法，具体如下。

- `setFirstResult（int firstResult）`：设定从哪个对象开始查询，参数 `firstResult` 表示这个对象在查询结果中的索引（索引的起始值为 0）。
- `setMaxResult（int maxResult）`：设定一次返回多少个对象。通常与 `setFirstResult（int firstResult）`方法结合使用，从而限制结果集的范围。默认情况下，返回查询结果中的所有对象。

```java
String hql = "from User"; // 编写 HQL
Query query = session.createQuery(hql); //创建 Query对象
query.setFirstResult(1); // 从第 2 条开始查询
query.setMaxResults(3); // 查询 3 条数据
List<User> list = query.list();
```



### 5.3.5、聚合查询

```java
Query query = session.createQuery("select count(*) from Order");
Long count = (Long)query.uniqueResult();
```

### 5.3.6、多表查询

```java
// inner join 查询
// 注意：多表查询中，两个表为：Customer 和 c.orders
Query query = session.createQuery("select c.name,o.productName from Customer c inner join c.orders o");
List<Object[]> list = query.list();

// left join 查询
Query query = session.createQuery("select c.name,o.productName from Customer c left join c.orders o");
		List<Object[]> list = query.list();

// right join 查询
Query query = session.createQuery("select c.name,o.productName from Order o right join o.customer c");
List<Object[]> list = query.list();
```



## 5.4、QBC检索方式

QBC（Query By Criteria）是 Hibernate 提供的另一种检索对象的方式，它主要由 Criteria 接口、Criterion 接口和 Expression 类组成，并且支持在运行时动态生成查询语句。

QBC 查询主要由 `Criteria` 接口完成，该接口由 `Session` 对象创建，`Criterion` 是 `Criteria` 的查询条件，在 `Criteria` 中提供了 `add（Criterion criterion）`方法添加查询条件。

```java
// 1. 全表查询
Criteria ce = session.createCriteria(Customer.class);
List<Customer> list = ce.list();

// 2. 单条件查询
Criteria ce = session.createCriteria(Order.class);
ce.add( Restrictions.eq("orderno", "201709070003"));
List<Order> list = ce.list();

// 3. 多条件查询
Criteria ce = session.createCriteria(Order.class);
ce.add(Restrictions.and(Restrictions.like("orderno", "%2017%"), Restrictions.like("productName", "%JavaWeb%")));
List<Order> list = ce.list();

// 4. 分页查询
Criteria ce = session.createCriteria(Order.class);
//分页查询
ce.setFirstResult(2);//起始行
ce.setMaxResults(2);//查询行数
List<Order> list = ce.list();

// 5. 查询排序
Criteria ce = session.createCriteria(Order.class);
//排序  order by id desc
ce.addOrder(org.hibernate.criterion.Order.desc("id"));
List<Order> list = ce.list();

// 6. 聚合查询
Criteria ce = session.createCriteria(Order.class);
//查询总记录数  select count(id) 
ce.setProjection(Projections.count("id"));
//查询id的最大值
ce.setProjection(Projections.max("id"));

// 7. 投影查询
Criteria ce = session.createCriteria(Order.class);
ProjectionList pList = Projections.projectionList();
pList.add(Property.forName("orderno"));
pList.add(Property.forName("productName"));
ce.setProjection(pList);
List<Object[]> list = ce.list();
```



## 5.5、本地SQL检索方式

本地 SQL 检索方式就是使用本地数据库的 SQL 查询语句进行查询。在 Hibernate 中，SQL 查询是通过 SQLQuery 接口表示的，该接口是 Query 接口的子接口，因此可以调用 Query 接口的方法。

```java
SQLQuery sqlQuery = session.createSQLQuery("select * from user");
```

# 6、二级缓存

## 6.1、二级缓存简介

与Session相对的是，`SessionFactory`也提供了相应的缓存机制。`SessionFactory`缓存可以依据功能和目的的不同而划分为内置缓存和外置缓存。


`SessionFactory`的内置缓存中存放了映射元数据和预定义SQL语句，映射元数据是映射文件中数据的副本，而预定义SQL语句是在 Hibernate初始化阶段根据映射元数据推导出来的。`SessionFactory`的内置缓存是只读的，应用程序不能修改缓存中的映射元数据和预定义 SQL语句，因此`SessionFactory`不需要进行内置缓存与映射文件的同步。

SessionFactory的外置缓存是一个可配置的插件。在默认情况下，`SessionFactory`不会启用这个插件。外置缓存的数据是数据库数据 的副本，外置缓存的介质可以是内存或者硬盘。`SessionFactory`的外置缓存也被称为Hibernate的二级缓存。

Hibernate的二级缓存的实现原理与一级缓存是一样的，也是通过以ID为key的`Map`来实现对对象的缓存。

由于Hibernate的二级缓存是作用在`SessionFactory`范围内的，因而它比一级缓存的范围更广，可以被所有的`Session`对象所共享。

## 6.2、工作内容

Hibernate的二级缓存同一级缓存一样，也是针对对象ID来进行缓存。所以说，二级缓存的作用范围是针对根据ID获得对象的查询。

二级缓存的工作可以概括为以下几个部分：

- 在执行各种条件查询时，如果所获得的结果集为实体对象的集合，那么就会把所有的数据对象根据ID放入到二级缓存中。
- 当Hibernate根据ID访问数据对象的时候，首先会从Session一级缓存中查找，如果查不到并且配置了二级缓存，那么会从二级缓存中查找，如果还查不到，就会查询数据库，把结果按照ID放入到缓存中。
- 删除、更新、增加数据的时候，同时更新缓存。

## 6.3、适用范围

Hibernate的二级缓存作为一个可插入的组件在使用的时候也是可以进行配置的，但并不是所有的对象都适合放在二级缓存中。

在通常情况下会将具有以下特征的数据放入到二级缓存中：

- 很少被修改的数据。
- 不是很重要的数据，允许出现偶尔并发的数据。
- 不会被并发访问的数据。
- 参考数据。

而对于具有以下特征的数据则不适合放在二级缓存中：

- 经常被修改的数据。
- 财务数据，绝对不允许出现并发。
- 与其他应用共享的数据。

在这里特别要注意的是对放入缓存中的数据不能有第三方的应用对数据进行更改（其中也包括在自己程序中使用其他方式进行数据的修改，例如，JDBC），因为那样Hibernate将不会知道数据已经被修改，也就无法保证缓存中的数据与数据库中数据的一致性。

## 6.4、二级缓存组件

在默认情况下，Hibernate会使用EHCache作为二级缓存组件。但是，可以通过设置 `hibernate.cache.provider_class`属性，指定其他的缓存策略，该缓存策略必须实现`org.hibernate.cache.CacheProvider`接口。

通过实现`org.hibernate.cache.CacheProvider`接口可以提供对不同二级缓存组件的支持。

Hibernate内置支持的二级缓存组件如下表所示。

| 组件            | Provider类                                 | 类型       | 集群     | 查询缓存 |
| --------------- | ------------------------------------------ | ---------- | -------- | -------- |
| Hashtable       | org.hibernate.cache.HashtableCacheProvider | 内存       | 不支持   | 支持     |
| EHCache         | org.hibernate.cache.EhCacheProvider        | 内存，硬盘 | 最新支持 | 支持     |
| OSCache         | org.hibernate.cache.OSCacheProvider        | 内存，硬盘 | 不支持   | 支持     |
| SwarmCache      | org.hibernate.cache.SwarmCacheProvider     | 集群       | 支持     | 不支持   |
| JBoss TreeCache | org.hibernate.cache.TreeCacheProvider      | 集群       | 支持     | 支持     |

Hibernate已经不再提供对JCS（Java Caching System）组件的支持了。

## 6.5、二级缓存的配置

在使用Hibernate的二级缓存时，对于每个需要使用二级缓存的对象都需要进行相应的配置工作。也就是说，只有配置了使用二级缓存的对象才会被放置在二级缓存中。二级缓存是通过`<cache>`元素来进行配置的。`<cache>`元素的属性定义说明如下所示：

```xml
<cache usage="transactional|read-write|nonstrict-read-write|read-only"
  region="RegionName" 
  include="all|non-lazy">
</cache>
```

元素的属性说明如下表所示。

| 序号 | 属性    | 含义和作用                                                   | 必须 | 默认值 |
| ---- | ------- | ------------------------------------------------------------ | ---- | ------ |
| (1)  | usage   | 指定缓存策略，可选的策略包括：transactional，read-write，nonstrict-read-write或read-only | Y    |        |
| (2)  | region  | 指定二级缓存区域名                                           | N    |        |
| (3)  | include | 指定是否缓存延迟加载的对象。all，表示缓存所有对象；non-lazy，表示不缓存延迟加载的对象 | N    | all    |

## 6.6、二级缓存的策略

当多个并发的事务同时访问持久化层的缓存中的相同数据时，会引起并发问题，必须采用必要的事务隔离措施。

在进程范围或集群范围的缓存，即第二级缓存，会出现并发问题。因此可以设定以下4种类型的并发访问策略，每一种策略对应一种事务隔离级别。

### 6.6.1、只读缓存（read-only）

如果应用程序需要读取一个持久化类的实例，但是并不打算修改它们，可以使用read-only缓存。这是最简单，也是实用性最好的策略。

对于从来不会修改的数据，如参考数据，可以使用这种并发访问策略。

### 6.6.2、读/写缓存（read-write）

如果应用程序需要更新数据，可能read-write缓存比较合适。如果需要序列化事务隔离级别，那么就不能使用这种缓存策略。

对于经常被读但很少修改的数据，可以采用这种隔离类型，因为它可以防止脏读这类的并发问题。



### 6.6.3、不严格的读/写缓存（nonstrict-read-write）

如果程序偶尔需要更新数据（也就是说，出现两个事务同时更新同一个条目的现象很不常见），也不需要十分严格的事务隔离，可能适用nonstrict-read-write缓存。

对于极少被修改，并且允许偶尔脏读的数据，可以采用这种并发访问策略。

### 6.6.4、事务缓存（transactional）

transactional缓存策略提供了对全事务的缓存，仅仅在受管理环境中使用。它提供了Repeatable Read事务隔离级别。对于经常被读但很少修改的数据，可以采用这种隔离类型，因为它可以防止脏读和不可重复读这类的并发问题。

在上面所介绍的隔离级别中，事务型并发访问策略的隔离级别最高，然后依次是读/写型和不严格读写型，只读型的隔离级别最低。事务的隔离级别越高，并发性能就越低。

# 7、JPA

## 7.1、Hibernate 与JPA的关系

### 7.1.1、JPA

JPA全称： Java Persistence API
JPA通过JDK 5.0注解或XML描述对象－关系表的映射关系，并将运行期的实体对象持久化到数据库中。

**JPA的出现？**
JPA的出现有两个原因：

- 其一，简化现有Java EE和Java SE应用的对象持久化的开发工作；
- 其二，Sun希望整合对ORM技术，实现持久化领域的统一。

### 7.1.2、JPA提供的技术

**(1)、ORM映射元数据**
JPA支持XML和JDK 5.0注解两种元数据的形式，元数据描述对象和表之间的映射关系，框架据此将实体对象持久化到数据库表中；

**(2)、JPA 的API**
用来操作实体对象，执行CRUD操作，框架在后台替我们完成所有的事情，开发者从繁琐的JDBC和SQL代码中解脱出来。

**(3)、查询语言**
 通过面向对象而非面向数据库的查询语言查询数据，避免程序的SQL语句紧密耦合。 

### 7.1.3、Hibernate

JPA是需要Provider来实现其功能的，Hibernate就是JPA Provider中很强的一个。从功能上来说，JPA现在就是Hibernate功能的一个子集。Hibernate 从3.2开始，就开始兼容JPA。Hibernate3.2获得了Sun TCK的 JPA(Java  Persistence API) 兼容认证。



## 7.2、JPA注解

### 7.2.1、基础注解

`@Entity`：声明一个类为实体Bean。

`@Table`：说明此实体类映射的表名，目录，schema的名字。

`@Id`：声明此表的主键。

`@GeneratedValue`：定义主键的增长策略。我这里一般交给底层数据库处理，所以调用了名叫generator的增长方式，由下边的`@GenericGenerator`实现

```java
@Entity
@Table(name = "t_customer")
public class Customer {
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name = "id")
	private Integer id;
	@Column(name = "name")
	private String name;
	@Column(name = "gender")
	private String gender;
	// 省略get/set方法
}
```

> 【注意】查找映射实体类使用，不是映射文件
>
> ```xml
> <mapping class="com.yiidian.domain.Customer" />
> ```



### 7.2.2、@Transient

默认情况下，JPA 持续性提供程序假设实体的所有字段均为持久字段。

使用 `@Transient `注解==指定实体的非持久字段或属性==，例如，一个在运行时使用但并非实体状态一部分的字段或属性。

JPA 持续性提供程序不会对注解为 `@Transient` 的属性或字段持久保存（或创建数据库模式）。该批注可以与 `@Entity` 、`@MappedSuperclass` 和 `@Embeddable` 一起使用。

```java
@Entity
@Table(name = "t_customer")
public class Customer {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Integer id;
    @Column(name = "name")
    private String name;
    @Column(name = "gender")
    private String gender;

    //这是一个临时字段，不会持久化到数据库表
    @Transient
    private Boolean isMarried;
    // 省略get/set方法
}
```



### 7.2.3、JPA主键策略

JPA的4种策略，分别为：AUTO策略，Sequence策略，Identity策略，Table策略。



#### ①、AUTO策略

auto策略是JPA默认的策略，在hibernate的代码 `GenerationType.AUTO `进行定义。使用 AUTO 策略就是将主键生成的策略交给持久化引擎 (persistence engine) 来决定，由它自己从 Table 策略，Sequence 策略和 Identity策略三种策略中选择最合适的。不同的持久化引擎 、不同的数据库一般策略不同。比如oracle最常用的就是sequence，mysql最常用的就是identify，因为oracle数据库支持sequence，而mysql数据库则 是只支持auto_increment 。

```java
@Entity
@Table(name = "t_customer")
public class Customer {
	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	@Column(name = "id")
	private Integer id;
```



> 【注意】：
>
> 1. 如果是mysql数据库，一定要将主键列设置成自增长的，否则使用AUTO策略的时候，会报错：
>    `org.hibernate.exception.GenericJDBCException: Field 'id' doesn't have a default value`
> 2. 如果是oracle数据库，那么会使用`hibernate_sequence`，这个名称是固定的，不能更改。



#### ②、Sequence策略

对应Hibernate代码 `GenerationType.SEQUENCE` 。一些数据库，如oracle就会内置"序列生成器"。为了使用序列，我们需要使用JPA的sequence策略。

```java
@Entity
@Table(name = "t_customer")
public class Customer {
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "mySeqGenerator")
    @SequenceGenerator(name = "mySeqGenerator", sequenceName = "t_customer_sequence", initialValue = 1000, allocationSize = 50)
    @Column(name = "id")
```

这里需要配合使用`@SequenceGenerator`，用来指定序列的相关信息。

- `name`：序列生成器的名称，会在@GeneratedValue中进行引用
- `sequenceName`：oracle数据库中的序列生成器名称
- `initialValue`：主键的初始值
- `allocationSize`：主键每次增长值的大小

> 【注意】：如果底层数据库不执行序列，会报错：
> `org.hibernate.MappingException: org.hibernate.dialect.MySQLDialect does not support sequences`



#### ③、Identity策略

对应Hibernate代码 `GenerationType.IDENTITY` 。 这个很适合像mysql这样的数据库，提供了对自增主键的支持。

```java
@Entity
@Table(name = "t_customer")
public class Customer {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Integer id;
```



#### ④、Table策略

对应Hibernate代码 `GenerationType.TABLE` 。 使用一张特殊的数据库表，保存插入记录的时，需要的主键值。

```java
@Entity
@Table(name = "t_customer")
public class Customer {
	@Id
	@GeneratedValue(strategy = GenerationType.TABLE, generator = "myTableGenerator")
	@TableGenerator(name = "myTableGenerator", table = "hibernate_Need_Table", pkColumnName = "pk_key", valueColumnName = "pk_value", pkColumnValue = "customerId", initialValue = 100, allocationSize = 1000)
	private Integer id;
```

### 7.2.4、关系映射

#### ①、一对多

一方：客户

- ```java
  @Entity
  @Table(name="t_customer")
  public class Customer {
  	@Id
  	@GeneratedValue(strategy=GenerationType.IDENTITY)
  	@Column(name="id")
  	private Integer id;
  	@Column(name="name")
  	private String name;
  	@Column(name="gender")
  	private String gender;
  	
  	//关联订单
  	@OneToMany(targetEntity=Order.class,mappedBy="customer")
  	private Set<Order> orders = new HashSet<Order>();
  ```

  

多方：订单

- ```java
  @Entity
  @Table(name="t_order")
  public class Order implements Serializable{
  	@Id
  	@GeneratedValue(strategy=GenerationType.IDENTITY)
  	@Column(name="id")
  	private Integer id;
  	
  	@Column(name="orderno")
  	private String orderno;
  	
  	//关联客户
  	@ManyToOne(targetEntity=Customer.class)
  	@JoinColumn(name="cust_id")
  	private Customer customer;
  ```



#### ②、多对多

多方：用户

- ```java
  @Entity
  @Table(name="t_user")
  public class User implements Serializable{
  	@Id
  	@GeneratedValue(strategy=GenerationType.IDENTITY)
  	@Column(name="id")
  	private Integer id;
  	@Column(name="user_name")
  	private String userName;
  	
  	//关联角色
  	@ManyToMany(targetEntity=User.class)
  	//@JoinTable: 用于映射中间表
  	//joinColumns: 当前方在中间表的外键字段名称
  	//inverseJoinColumns：对方在中间表的外键字段名称
  	@JoinTable(
  			name="t_user_role",
  			joinColumns=@JoinColumn(name="user_id"),
  			inverseJoinColumns=@JoinColumn(name="role_id"))
  	private Set<Role> roles = new HashSet<Role>();
  ```

  

多方：角色

- ```java
  @Entity
  @Table(name="t_role")
  public class Role implements Serializable{
  	@Id
  	@GeneratedValue(strategy=GenerationType.IDENTITY)
  	@Column(name="id")
  	private Integer id;
  	@Column(name="role_name")
  	private String roleName;
  	
  	//关联用户
  	@ManyToMany(targetEntity=User.class,mappedBy="roles")
  	private Set<User> users = new HashSet<User>();
  ```

  

#### ③、一对一

一方：公民

- ```java
  @Entity
  @Table(name="t_person")
  public class Person {
  	@Id
  	@GeneratedValue(strategy=GenerationType.IDENTITY)
  	@Column(name="id")
  	private Integer id;
  	
  	@Column(name="name")
  	private String name;
  	
  	//关联身份证
  	@OneToOne(mappedBy="person")
  	private Card card;
  ```

  

一方：身份证

- ```java
  @Entity
  @Table(name="t_card")
  public class Card {
  	@Id
  	@GeneratedValue(strategy=GenerationType.IDENTITY)
  	@Column(name="id")
  	private Integer id;
  	
  	@Column(name="card_no")
  	private String cardno;
  	
  	//关联公民
  	@OneToOne
  	@JoinColumn(name="person_id")
  	private Person person;
  ```

  

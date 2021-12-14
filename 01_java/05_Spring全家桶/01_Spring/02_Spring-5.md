# 引言

## EJB存在的问题

**EJB（Enterprise Java Bean）**

- 运行环境苛刻
- 代码移植性差

> 总结
>
> EJB是重量级的框架

## 什么是Spring

Spring是一个轻量级的JavaEE解决方案，整合了众多优秀的设计模式。

1. 轻量级：

   - 对于运行环境没有额外要求
     - 开源：tomcat、resion、jetty
     - 收费：weblogic、websphere
   - 代码移植性高
     - 不需要实现额外接口

2. JavaEE的解决方案

   ![image-20211125165517597](D:\笔记成长\BaiduNetdiskWorkspace\01_java\05_Spring全家桶\01_Spring\images\02_Spring-5\image-20211125165517597.png)

3. 整合设计模式

   - 工厂模式
   - 代理模式
   - 模板模式
   - 策略模式

# Spring程序

## 环境搭建

1. Spring的jar包

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.2.1.RELEASE</version>
       <type>pom</type>
   </dependency>
   ```

   

2. Spring的配置文件

   - 配置文件的放置位置：任何位置，没有硬性要求

   - 配置文件的命名：没有硬性要求，建议：==applicationContext.xml==

   - 在IDEA中创建配置文件

     ![image-20211125173951898](D:\笔记成长\BaiduNetdiskWorkspace\01_java\05_Spring全家桶\01_Spring\images\02_Spring-5\image-20211125173951898.png)

## Spring的核心API

### ApplicationContext

1. 作用与好处

   - 作用：Spring提供的`ApplicationContext`这个工厂，==用于对象的创建==
   - 好处：解耦合

2. ApplicationContext接口类型

   接口：屏蔽实现的差异

   - 非Web环境：`ClassPathXMLApplicationContext`（main、junit）

     ![image-20211125175236923](D:\笔记成长\BaiduNetdiskWorkspace\01_java\05_Spring全家桶\01_Spring\images\02_Spring-5\image-20211125175236923.png)

   - Web环境：`XmlWebApplicationContext`（需要导入webmvc包）

     ![image-20211125175331683](D:\笔记成长\BaiduNetdiskWorkspace\01_java\05_Spring全家桶\01_Spring\images\02_Spring-5\image-20211125175331683.png)

3. 重量级资源

   - `ApplicationContext`工厂的对象占用大量内存

     不会频繁的创建对象：一个应用只会创建一个`ApplicationContext`工厂对象

   - `ApplicationContext`工厂：一定是线程安全的（多线程并发访问）

## 程序开发

### 简单开发步骤

1. 创建类型

2. 配置文件的配置 applicationContext.xml

   ```xml
   <bean id="person" class="spring.dy01.Person"/>
   ```

   

3. 通过工厂类，获取对象

   ```java
   // 1. 获取Spring的工厂
   ApplicationContext ctx = new ClassPathXmlApplicationContext("/applicationContext.xml");
   // 2. 根据Spring工厂获取对象
   Person person = (Person) ctx.getBean("person");
   ```

### 细节分析

#### 名词解释

Spring工厂创建的对象，叫做bean或者组件（Componet）

#### Spring工厂的相关方法

1. 获取Bean

   ```java
   // 2.1. 需要类型强制转换
   Person person = (Person) ctx.getBean("person");
   
   // 2.2. 指定class后, 无须强制类型转换
   Person person = ctx.getBean("person", Person.class);
   
   // 2.3. 当前Spring的配置文件中, 【注意】: 只能有一个<bean class是Person类型>
   Person person = ctx.getBean( Person.class);
   ```

   

2. 获取`<bean>`标签的id值

   ```java
   // 2.4. 获取 Spring工厂配置文件中的所有的Bean标签的id值
   String[] beanDefinitionNames = ctx.getBeanDefinitionNames();
   for (String beanDefinitionName: beanDefinitionNames) {
       System.out.println("BeanDefinitionName = " + beanDefinitionName); // aaa, bbb, person.....
   }
   
   // 2.5. 根据类型获取 spring配置文件的对应的id值
   String[] beanNamesForType = ctx.getBeanNamesForType(Person.class);
   for (String beanNameForType: beanNamesForType) {
       System.out.println("beanNameForType = " + beanNameForType); // person.....
   }
   ```

   

3. 判断id值对应的`<bean>`标签是否存在

   ```java
   // 2.6. 用于判断是否存在指定id值的bean, 不能判断name的值
   boolean person1 = ctx.containsBeanDefinition("person"); // true
   
   // 2.7. 用于判断是否存在指定id值的bean, 也可以判断name的值
   boolean b = ctx.containsBean("person"); // true
   ```

### 配置文件中需要注意的细节

1. 只配置class属性

   ```markdown
   <bean class="spring.dy01.Person"/>
   ```

   - 上述这种配置，可以根据`ctx.getBean( Person.class);`获取对象
   - 默认id值为：==spring.dy01.Person#0==
   - **应用场景**：
     - 如果这个bean只需要使用一次，那么就可以省略id值
     - 如果这个bean只需要使用多次，或者被其他bean引用则需要设置id值

2. name属性

   - **作用**：用于在Spring配置文件中，为bean对象定义==别名==（小名）

   - **相同**：

     - `ctx.getBean("id|name");`都可以创建Object

     - `<bean id="person" class="spring.dy01.Person"/>`  等效

       `<bean name="person" class="spring.dy01.Person"/>`

   - **不同：**

     - `name`属性可以定义多个，但是`id`属性只能定义一个

       ```xml
       <bean name="person, p, p1" class="spring.dy01.Person"/>
       ```

       

     - XML的`id`属性的值，命名要求：必须以字母开头，后面跟上字母、数字、下划线、连字符，不能以特殊字符开头（如：/person）。（但是现在XML现在都可以了）

       `name`属性的值，命名没有要求，`name`属性会应用在特殊命名的场景下

### Spring工厂的底层实现原理(简易版)

![image-20211126111026785](D:\笔记成长\BaiduNetdiskWorkspace\01_java\05_Spring全家桶\01_Spring\images\02_Spring-5\image-20211126111026785.png)

> 注意
>
> 1. SPring工厂是可以调用私有构造方法的
> 2. 不是所有的类都交给Spring工厂创建，实体对象（entity）不会交给Spring创建，而是由持久层框架进行创建

# Spring5.x与日志框架的整合

1. Spring与日志框架进行整合，日志框架就可以在控制台中，输出Spring框架在运行过程中的一些重要的信息。
2. **好处**：便于了解Spring框架的运行过程，利于程序的调试
3. **默认情况**:
   1. Spring1、2、3早期版本，都是与commons-Logging
   2. spring5.x默认整合的日志框架：logback、log4j2

## 整合log4j

1. 引入pom文件

   ```xml
   <dependency>
       <groupId>org.slf4j</groupId>
       <artifactId>slf4j-log4j12</artifactId>
       <version>1.7.25</version>
   </dependency>
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   ```

   

2. 引入log4j.properties

   ```properties
   ### 配置根 ###
   log4j.rootLogger = DEBUG,Console
   
   ###  输出到控制台  ###
   log4j.appender.Console=org.apache.log4j.ConsoleAppender
   log4j.appender.Console.Target=System.out
   log4j.appender.Console.layout=org.apache.log4j.PatternLayout
   log4j.appender.Console.layout.ConversionPattern= %d{ABSOLUTE} %5p %c{1}:%L - %m%n
   ```

# 注入（Injection）

1. **什么是注入**：通过Spring的工厂及配置文件，为创建对象的成员变量赋值

2. **为什么需要注入**：通过编码方式，为成员变量进行赋值，存在藕合问题

   ![image-20211126113347279](D:\笔记成长\BaiduNetdiskWorkspace\01_java\05_Spring全家桶\01_Spring\images\02_Spring-5\image-20211126113347279.png)

3. **注入好处**：解耦合

## 如何进行注入（步骤）

1. 类为==成员变量==提供 `set/get` 方法

2. 配置Spring的配置文件

   ```xml
   <bean id="person" class="spring.dy01.Person">
       <property name="id">
           <value>10</value>
       </property>
       <property name="name">
           <value>张三</value>
       </property>
   </bean>
   ```

## 注入原理分析（简易版）

Spring通过底层调用对象属性对应的set方法，完成成员变量的赋值，这种方式我们也称之为set注入

![image-20211126114513254](D:\笔记成长\BaiduNetdiskWorkspace\01_java\05_Spring全家桶\01_Spring\images\02_Spring-5\image-20211126114513254.png)

## set注入详解

针对不同类型的成员，在`<property>`标签内，需要嵌套其他标签。

![image-20211126145245820](D:\笔记成长\BaiduNetdiskWorkspace\01_java\05_Spring全家桶\01_Spring\images\02_Spring-5\image-20211126145245820.png)

### JDK内置类型

#### 8种基础类型+String

使用`<value>`标签进行嵌套

```xml
<property name="id">
    <value>10</value>
</property>
<property name="name">
    <value>张三</value>
</property>
```

#### 数组类型

使用`<list>`标签进行嵌套

```java
private String[] emails;
public String[] getEmails() {
    return emails;
}
public void setEmails(String[] emails) {
    this.emails = emails;
}
```

```xml
<property name="emails">
    <list>
        <value>aaa@xx.com</value>
        <value>bbb@xx.com</value>
        <value>ccc@xx.com</value>
    </list>
</property>
```

#### set集合

使用`<set>`标签进行嵌套

```java
private Set<String> tels;    
public Set<String> getTels() {
    return tels;
}
public void setTels(Set<String> tels) {
    this.tels = tels;
}
```

```xml
<property name="tels">
    <set>
        <value>13299999999</value>
        <value>18199999999</value>
        <value>19999999999</value>
    </set>
</property>
```

#### list集合

使用`<list>`标签进行嵌套

```java
private List<String> addresses;
public List<String> getAddresses() {
    return addresses;
}
public void setAddresses(List<String> addresses) {
    this.addresses = addresses;
}
```

```xml
<property name="addresses">
    <list>
        <value>武汉</value>
        <value>上海</value>
        <value>北京</value>
    </list>
</property>
```

#### Map集合

1. 使用`<map>`标签、`<entry>`标签、`<key>`标签进行嵌套。
2. `<value>`标签：根据对应类型选择对应的标签

```java
private List<String> addresses;
public List<String> getAddresses() {
    return addresses;
}
public void setAddresses(List<String> addresses) {
    this.addresses = addresses;
}
```

```java
private Map<String, String> qqs;
public Map<String, String> getQqs() {
    return qqs;
}
public void setQqs(Map<String, String> qqs) {
    this.qqs = qqs;
}
```

```xml
<property name="qqs">
    <map>
        <entry>
            <key><value>zhangsan</value></key>
            <value>1111111</value>
        </entry>
        <entry>
            <key><value>lisi</value></key>
            <value>222222</value>
        </entry>
    </map>
</property>
```

#### Properties集合

`Properties`类型，特殊的`Map`，它的`key`为`String`类型，`value`为`String`类型

```java
private Properties properties;
public Properties getProperties() {
    return properties;
}
public void setProperties(Properties properties) {
    this.properties = properties;
}
```

```xml
<property name="properties">
    <props>
        <prop key="key1">value1</prop>
        <prop key="key2">value2</prop>
    </props>
</property>
```

### 用户自定义类型

#### 方式一：bean标签直接

1. 类中为成员变量提供set get 方法

   ```java
   private UserDao userDao;
   public UserDao getUserDao() {
       return userDao;
   }
   public void setUserDao(UserDao userDao) {
       this.userDao = userDao;
   }
   ```

2. 配置文件中进行注入（赋值）

   ```xml
   <bean id="userService" class="com.wl.service.UserService">
       <property name="userDao">
           <!-- 这里直接使用bean引入 -->
       	<bean class="com.wl.dao.UserDao"/>
       </property>
   </bean>
   ```

> 注意
>
> **存在缺陷**
>
> 1. 配置文件代码冗余。多个引入需要多个`<bean>`标签
> 2. 被注入的对象，多次创建，浪费（JVM）内存资源

#### 方式二：ref标签引用

```xml
<bean id="userDao" class="com.wl.dao.UserDao">
</bean>

<bean id="userService" class="com.wl.service.UserService">
    <property name="userDao">
        <!-- 这里ref引用引入 -->
    	<ref bean="userDao"/>
    </property>
</bean>
```

### set注入的简化写法

#### 基于属性简化

1. 8种基本类型+String

   - 将`<value>`标签 简化为 value属性

   ```xml
   <property name="id">
       <value>10</value>
   </property>
   
   <!-- 将<value>标签 简化为 value属性 -->
   <property name="name" value=张三>
   </property>
   ```

   

2. 自定义类型

   - 将`<ref>`标签 简化为 `ref`属性

   ```xml
   <bean id="userDao" class="com.wl.dao.UserDao">
   </bean>
   
   <bean id="userService" class="com.wl.service.UserService">
       <property name="userDao">
           <!-- 这里ref引用引入 -->
       	<ref bean="userDao"/>
       </property>
   </bean>
   
   <!-- 将<ref>标签 简化为 ref属性 -->
   <bean id="userService" class="com.wl.service.UserService">
       <property name="userDao" ref="userDao"/>
   </bean>
   ```

#### 基于P命名空间简化

1. 8种基本类型+String：==p:属性名==

   ```xml
   <property name="id">
       <value>10</value>
   </property>
   
   <!-- p命名空间 -->
   <bean id="person" class="spring.dy01.Person"
         p:name="zhangsan" p:id="1000"/>
   ```

   

2. 自定义类型：==p:属性名-ref==

   ```xml
   <bean id="userService" class="com.wl.service.UserService">
       <property name="userDao" ref="userDao"/>
   </bean>
   
   <!-- p命名空间 -->
   <bean id="userService" class="com.wl.service.UserService" p:userDao-ref="userDao"/>
   ```


## 构造注入

Spring调用构造方法，通过配置文件为成员变量赋值

### 开发步骤

1. 为类提供有参构造方法

   ```java
   private Integer id;
   private String name;
   public Person(Integer id, String name){
       this.id = id;
       this.name = name;
   }
   ```

   

2. Spring的配置文件

   **注意**：`<constructor-arg>`的==个数==和==顺序==与构造器一致

   ```xml
   <bean id="person" class="spring.dy01.Person">
       <constructor-arg>
           <value>1000</value>
       </constructor-arg>
       <constructor-arg>
           <value>zhangsan</value>
       </constructor-arg>
   </bean>
   ```

### 构造器重载

#### 参数个数不同时

通过控制`<constructor-arg>`标签的数量进行区分

```java
private Integer id;
private String name;

public Person(String name) { // 我只为name进行赋值
    this.name = name;
}

public Person(Integer id, String name){
    this.id = id;
    this.name = name;
}
```

```xml
<bean id="person" class="spring.dy01.Person">
    <constructor-arg>
        <value>zhangsan</value>
    </constructor-arg>
</bean>
```

#### 参数个数相同时

在`<constructor-arg>`标签中引入`type`属性进行类型区分

```java
private Integer id;
private String name;

public Person(String name) { 
    this.name = name;
}

public Person(Integer id) {
    this.id = id;
}
```

```xml
<bean id="person" class="spring.dy01.Person">
    <!-- 若不指定type, 则会按构造器定义顺序进行赋值, 那么就不会为id赋值, 而是赋值给了name -->
    <constructor-arg type="int">
        <value>10001</value>
    </constructor-arg>
</bean>
```

## 注入总结

1. 用set注入和构造注入，那个用的多?
   - 构造注入麻烦（重载）
   - Spring框架底层，大量应用了set注入

2. 总结

   ![image-20211126165912869](D:\笔记成长\BaiduNetdiskWorkspace\01_java\05_Spring全家桶\01_Spring\images\02_Spring-5\image-20211126165912869.png)

# 反转控制与依赖注入

## 反转控制（IOC inverse of Control）

1. 控制：对应==成员变量==赋值的控制权
2. 反转控制：把对应成员变量赋值的控制权，从代码中反转（转移）到Spring工厂和配置文件中完成
   - 好处：解耦合
3. 底层实现：工厂设计模式

![image-20211126170526511](D:\笔记成长\BaiduNetdiskWorkspace\01_java\05_Spring全家桶\01_Spring\images\02_Spring-5\image-20211126170526511.png)

## 依赖注入（Dependency Injection DI）

1. **注入**：通过Spring的工厂及配置文件，为对象（bean、文件）的成员变量赋值

2. **依赖注入**：但一个类需要另一个类时，就意味着==依赖==，一旦出现依赖，就可以把另一类做为本类的成员变量，最终通过Spring配置文件进行注入（赋值）。

   - 好处：解耦合

   ![image-20211126170938046](D:\笔记成长\BaiduNetdiskWorkspace\01_java\05_Spring全家桶\01_Spring\images\02_Spring-5\image-20211126170938046.png)

# Spring创建复杂对象

## 什么是复杂对象

**复杂对象**:值的是不能直接通过new构造方法创建的对象。例如：`Connection`、`SQLSessionFactory`

![image-20211126171846779](D:\笔记成长\BaiduNetdiskWorkspace\01_java\05_Spring全家桶\01_Spring\images\02_Spring-5\image-20211126171846779.png)






































































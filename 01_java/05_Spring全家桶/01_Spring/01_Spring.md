# 1. Spring概述

## 1.1. spring 是什么

Spring 是分层的 Java SE/EE 应用 ==full-stack== 轻量级开源框架，以 ==IoC（Inverse Of Control： 反转控制）和 AOP（Aspect Oriented Programming：面向切面编程）为内核==，提供了展现层 Spring  MVC 和持久层 Spring JDBC 以及业务层事务管理等众多的企业级应用技术，还能整合开源世界众多 著名的第三方框架和类库，逐渐成为使用最多的 Java EE 企业应用开源框架。

## 1.2. Spring 的发展历程

1997 年 IBM 提出了 EJB 的思想 

1998 年，SUN 制定开发标准规范 EJB1.0 

1999 年，EJB1.1 发布 

2001 年，EJB2.0 发布   

2003 年，EJB2.1 发布 

2006 年，EJB3.0 发布 

Rod Johnson（spring 之父）

- Expert One-to-One J2EE Design and Development(2002) 阐述了 J2EE 使用 EJB 开发设计的优点及解决方案 

- Expert One-to-One J2EE Development without EJB(2004) 阐述了 J2EE 开发不使用 EJB 的解决方式（Spring 雏形）

 2017 年 9 月份发布了 spring 的最新版本 spring 5.0 通用版（GA） 

## 1.3. Spring 的优势

**方便解耦，简化开发** 通过 Spring 提供的 IoC 容器，可以将对象间的依赖关系交由 Spring 进行控制，避免硬编码所造 成的过度程序耦合。用户也不必再为单例模式类、属性文件解析等这些很底层的需求编写代码，可 以更专注于上层的应用。

 **AOP 编程的支持** 通过 Spring 的 AOP 功能，方便进行面向切面的编程，许多不容易用传统 OOP 实现的功能可以通过 AOP 轻松应付。 

**声明式事务的支持** 可以将我们从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活的进行事务的管理， 提高开发效率和质量。 

**方便程序的测试** 可以用非容器依赖的编程方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可做的事情。 

**方便集成各种优秀框架** Spring 可以降低各种框架的使用难度，提供了对各种优秀框架（Struts、Hibernate、Hessian、Quartz 等）的直接支持。 

**降低 JavaEE API 的使用难度** Spring 对 JavaEE API（如 JDBC、JavaMail、远程调用等）进行了薄薄的封装层，使这些 API 的 使用难度大为降低。 

**Java 源码是经典学习范例** Spring 的源代码设计精妙、结构清晰、匠心独用，处处体现着大师对 Java 设计模式灵活运用以 及对 Java 技术的高深造诣。它的源代码无意是 Java 技术的最佳实践的范例。

## 1.4. spring 的体系结构

![image-20210606130123064](images\01_Spring\image-20210606130123064.png)

# 2. IoC 的概念和作用

## 2.1. 程序的耦合和解耦

### 2.1.1. 什么是程序的耦合

耦合性(Coupling)，也叫耦合度，是对模块间关联程度的度量。耦合的强弱取决于模块间接口的复杂性、调用模块的方式以及通过界面传送数据的多少。模块间的耦合度是指模块之间的依赖关系，包括控制关系、调用关 系、数据传递关系。模块间联系越多，其耦合性越强，同时表明其独立性越差( 降低耦合性，可以提高其独立 性)。耦合性存在于各个领域，而非软件设计中独有的，但是我们只讨论软件工程中的耦合。 

在软件工程中，耦合指的就是就是对象之间的依赖性。对象之间的耦合越高，维护成本越高。因此对象的设计 应使类和构件之间的耦合最小。软件设计中通常用耦合度和内聚度作为衡量模块独立程度的标准。==划分模块的一个准则就是高内聚低耦合==。

**它有如下分类：** 

1. 内容耦合。当一个模块直接修改或操作另一个模块的数据时，或一个模块不通过正常入口而转入另 一个模块时，这样的耦合被称为内容耦合。内容耦合是最高程度的耦合，应该避免使用之。
2. 公共耦合。两个或两个以上的模块共同引用一个全局数据项，这种耦合被称为公共耦合。在具有大 量公共耦合的结构中，确定究竟是哪个模块给全局变量赋了一个特定的值是十分困难的。
3. 外部耦合 。一组模块都访问同一全局简单变量而不是同一全局数据结构，而且不是通过参数表传 递该全局变量的信息，则称之为外部耦合。 
4. 控制耦合 。一个模块通过接口向另一个模块传递一个控制信号，接受信号的模块根据信号值而进 行适当的动作，这种耦合被称为控制耦合。 
5. 标记耦合 。若一个模块 A 通过接口向两个模块 B 和 C 传递一个公共参数，那么称模块 B 和 C 之间 存在一个标记耦合。 
6. 数据耦合。模块之间通过参数来传递数据，那么被称为数据耦合。数据耦合是最低的一种耦合形 式，系统中一般都存在这种类型的耦合，因为为了完成一些有意义的功能，往往需要将某些模块的输出数据作为另 一些模块的输入数据。 
7. 非直接耦合 。两个模块之间没有直接关系，它们之间的联系完全是通过主模块的控制和调用来实 现的。

**总结**： 耦合是影响软件复杂程度和设计质量的一个重要因素，在设计上我们应采用以下原则：如果模块间必须 存在耦合，就尽量使用数据耦合，少用控制耦合，限制公共耦合的范围，尽量避免使用内容耦合。

**内聚与耦合** 内聚标志一个模块内各个元素彼此结合的紧密程度，它是信息隐蔽和局部化概念的自然扩展。内聚是从 功能角度来度量模块内的联系，一个好的内聚模块应当恰好做一件事。它描述的是模块内的功能联系。耦合是软件 结构中各模块之间相互连接的一种度量，耦合强弱取决于模块间接口的复杂程度、进入或访问一个模块的点以及通 过接口的数据。 程序讲究的是低耦合，高内聚。就是同一个模块内的各个元素之间要高度紧密，但是各个模块之 间的相互依存度却要不那么紧密。 

内聚和耦合是密切相关的，同其他模块存在高耦合的模块意味着低内聚，而高内聚的模块意味着该模块同其他 模块之间是低耦合。在进行软件设计时，应力争做到高内聚，低耦合。

### 2.1.2. 工厂模式解耦

在实际开发中我们可以把三层的对象都使用配置文件配置起来，当启动服务器应用加载的时候，让一个类中的 方法通过读取配置文件，把这些对象创建出来==并存起来==。在接下来的使用的时候，直接拿过来用就好了。 

那么，这个读取配置文件，创建和获取三层对象的类就是工厂。

### 2.1.3. 控制反转-Inversion Of Control

上一小节解耦的思路有 2 个问题： 

1. 存哪去？ 

   - 分析：由于我们是很多对象，肯定要找个集合来存。这时候有 Map 和 List 供选择。 到底选 Map 还是 List 就看我们有没有查找需求。有查找需求，选 Map。 
   - 所以我们的答案就是 在应用加载时，创建一个 Map，用于存放三层对象。
   -  我们把这个 map 称之为==容器==。 

2. 还是没解释什么是工厂？ 

   - 工厂就是负责给我们从容器中获取指定对象的类。这时候我们获取对象的方式发生了改变。

   -  原来： 我们在获取对象时，都是采用 new 的方式。==是主动的==。

     ![image-20210606144624746](images\01_Spring\image-20210606144624746.png)

   - 现在： 我们获取对象时，同时跟工厂要，有工厂为我们查找或者创建对象。==是被动的==。

     ![image-20210606144705700](F:\BaiduNetdiskWorkspace\05_Spring\images\01_Spring\image-20210606144705700.png)

     这种被动接收的方式获取对象的思想就是控制反转，它是 spring 框架的核心之一。



**明确 ioc 的作用**： 削减计算机程序的耦合(解除我们代码中的依赖关系)。

# 3. 使用 spring 的 IOC 解决程序耦合

## 3.1. 基于 XML 的配置（入门案例）

具体步骤：

1. 拷贝必备的 jar 包到工程的 lib 目录中

2. 在类的根路径下创建一个任意名称的 xml 文件（不能是中文）
   给配置文件导入约束： `/spring-framework-5.0.2.RELEASE/docs/spring-framework-reference/html5/core.html`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans 
   		http://www.springframework.org/schema/beans/spring-beans.xsd">
   </beans>
   ```

   

3. 让 spring 管理资源，在配置文件中配置 service 和 dao

   ```xml
   <!-- bean 标签：用于配置让 spring 创建对象，并且存入 ioc 容器之中
        id 属性：对象的唯一标识。
        class 属性：指定要创建对象的全限定类名
   -->
   <!-- 配置 service --> 
   <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"></bean>
   <!-- 配置 dao -->
   <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl"></bean>
   ```

4. 开始使用

   ```Java
   public static void main(String[] args) {
       //1.使用 ApplicationContext 接口，就是在获取 spring 容器
       ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
       //2.根据 bean 的 id 获取对象
       IAccountService aService = (IAccountService) ac.getBean("accountService");
       System.out.println(aService);
       IAccountDao aDao = (IAccountDao) ac.getBean("accountDao");
       System.out.println(aDao);
   }
   ```

   

## 3.2. Spring 基于 XML 的 IOC 细节

### 3.2.1. spring 中工厂的类结构图

![image-20210606150713706](images\01_Spring\image-20210606150713706.png)

![image-20210606150733619](images\01_Spring\image-20210606150733619.png)

#### ①. BeanFactory 和 ApplicationContext 的区别

BeanFactory 才是 Spring 容器中的顶层接口。 

ApplicationContext 是它的子接口。 

BeanFactory 和 ApplicationContext 的区别： 创建对象的时间点不一样。

- ApplicationContext：只要一读取配置文件，默认情况下就会创建对象。 
- BeanFactory：什么使用什么时候创建对象。

#### ②. ApplicationContext 接口的实现类

ClassPathXmlApplicationContext： 它是从类的根路径下加载配置文件 ==推荐使用这种== 

FileSystemXmlApplicationContext： 它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置。 

AnnotationConfigApplicationContext: 当我们使用注解配置容器对象时，需要使用此类来创建 spring 容器。它用来读取注解。

### 3.3.2. IOC 中 bean 标签和管理对象细节 

#### ①. bean 标签

**作用**： 用于配置对象让 spring 来创建的。 ==默认情况下==它调用的是类中的无参构造函数。如果没有无参构造函数则不能创建成功。 

**属性**： 

1. `id`：给对象在容器中提供一个唯一标识。用于获取对象。
2. `class`：指定类的全限定类名。用于反射创建对象。默认情况下调用无参构造函数。 
3. `scope`：指定对象的作用范围。 
   - `singleton` ：默认值，单例的. 
   - `prototype` ：多例的. 
   - `request` ：WEB 项目中，Spring 创建一个 Bean 的对象，将对象存入到 request 域中. 
   - `session` ：WEB 项目中，Spring 创建一个 Bean 的对象，将对象存入到 session 域中
   -  `global session` ：WEB 项目中，应用在 Portlet 环境。如果没有 Portlet 环境那么 globalSession 相当于 session.
4. `init-method`：指定类中的初始化方法名称。
5. `destroy-method`：指定类中销毁方法名称。

#### ②. bean 的作用范围和生命周期

1. 单例对象：`scope="singleton"` 
   - 一个应用只有一个对象的实例。它的作用范围就是整个引用。 
   - 生命周期： 
     - 对象出生：当应用加载，创建容器时，对象就被创建了。 
     - 对象活着：只要容器在，对象一直活着。 
     - 对象死亡：当应用卸载，销毁容器时，对象就被销毁了。 
2. 多例对象：`scope="prototype"` 
   - 每次访问对象时，都会重新创建对象实例。 
   - 生命周期： 
     - 对象出生：当使用对象时，创建新的对象实例。 
     - 对象活着：只要对象在使用中，就一直活着。 
     - 对象死亡：当对象长时间不用时，被 java 的垃圾回收器回收了。

#### ③. 实例化 Bean 的三种方式 

第一种方式：使用默认无参构造函数

- ```xml
  <!--在默认情况下：
  它会根据默认无参构造函数来创建类对象。如果 bean 中没有默认无参构造函数，将会创建失败。-->
  <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"/>
  ```

第二种方式：spring 管理静态工厂-使用静态工厂的方法创建对象

- ```Java
  // 模拟一个静态工厂，创建业务层实现类
  public class StaticFactory {
      public static IAccountService createAccountService(){
      	return new AccountServiceImpl();
      }
  }
  ```

- ```xml
  <!-- 此种方式是:
  使用 StaticFactory 类中的静态方法 createAccountService 创建对象，并存入 spring 容器
  	id 属性：指定 bean 的 id，用于从容器中获取
  	class 属性：指定静态工厂的全限定类名
  	factory-method 属性：指定生产对象的静态方法
  -->
  <bean id="accountService" class="com.itheima.factory.StaticFactory" factory-method="createAccountService"></bean>
  ```

第三种方式：spring 管理实例工厂-使用实例工厂的方法创建对象

- ```xml
  <!-- 此种方式是：
  先把工厂的创建交给 spring 来管理。然后在使用工厂的 bean 来调用里面的方法
  	factory-bean 属性：用于指定实例工厂 bean 的 id。
  	factory-method 属性：用于指定实例工厂中创建对象的方法。
  -->
  <bean id="instancFactory" class="com.itheima.factory.InstanceFactory"></bean>
  <bean id="accountService" factory-bean="instancFactory" factory-method="createAccountService"></bean>
  ```

### 3.3.3. spring 的依赖注入

#### ①. 依赖注入概念

依赖注入：Dependency Injection。它是 spring 框架核心 ioc 的具体实现。 

我们的程序在编写时，通过控制反转，把对象的创建交给了 spring，但是代码中不可能出现没有依赖的情况。 ioc 解耦只是降低他们的依赖关系，但不会消除。例如：我们的业务层仍会调用持久层的方法。 

那这种业务层和持久层的依赖关系，在使用 spring 之后，就让 spring 来维护了。 

简单的说，就是坐等框架把持久层对象传入业务层，而不用我们自己去获取。

#### ②. 构造函数注入

顾名思义，就是使用类中的构造函数，给成员变量赋值。注意，赋值的操作不是我们自己做的，而是通过配置 的方式，让 spring 框架来为我们注入。具体代码如下：

```Java
public class AccountServiceImpl implements IAccountService {
    private String name;
    private Integer age;
    private Date birthday;
    public AccountServiceImpl(String name, Integer age, Date birthday) {
        this.name = name;
        this.age = age;
        this.birthday = birthday;
	}
}
```



```xml
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
    <constructor-arg name="name" value="张三"></constructor-arg>
    <constructor-arg name="age" value="18"></constructor-arg>
    <constructor-arg name="birthday" ref="now"></constructor-arg>
</bean>
<bean id="now" class="java.util.Date"></bean>
```

使用构造函数的方式，给 service 中的属性传值

**要求**：

类中需要提供一个对应参数列表的构造函数。

**涉及的标签**：

`<constructor-arg>`属性：

- `index`：指定参数在构造函数参数列表的索引位置
- `type`：指定参数在构造函数中的数据类型
- `name`：指定参数在构造函数中的名称 用这个找给谁赋值 
- `value`：它能赋的值是基本数据类型和 String 类型 
- `ref`：它能赋的值是其他 bean 类型，也就是说，必须得是在配置文件中配置过的 bean

#### ③. set 方法注入

顾名思义，就是在类中提供需要注入成员的 set 方法。具体代码如下：

```xml
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
    <property name="name" value="test"></property>
    <property name="age" value="21"></property>
    <property name="birthday" ref="now"></property>
</bean>
<bean id="now" class="java.util.Date"></bean>
```

通过配置文件给 bean 中的属性传值：使用 set 方法的方式 

**涉及的标签**： `<property>`

**属性**： 

- `name`：找的是类中 set 方法后面的部分 
- `ref`：给属性赋值是其他 bean 类型的 
- `value`：给属性赋值是基本数据类型和 string 类型的 

实际开发中，此种方式用的较多。

#### ④. 使用 p 名称空间注入数据（本质还是调用 set 方法）

此种方式是通过在 xml 中导入 p 名称空间，使用 `p:propertyName` 来注入数据，它的本质仍然是调用类中的 set 方法实现注入功能。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:p="http://www.springframework.org/schema/p"
 	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 	xsi:schemaLocation=" http://www.springframework.org/schema/beans 
http://www.springframework.org/schema/beans/spring-beans.xsd">
<bean id="accountService"  class="com.itheima.service.impl.AccountServiceImpl4" p:name="test" p:age="21" p:birthday-ref="now"/></beans>
```

#### ⑤. 注入集合属性

顾名思义，就是给类中的集合成员传值，它用的也是set方法注入的方式，只不过变量的数据类型都是集合。 我们这里介绍注入数组、List、Set、Map、Properties。具体代码如下：

```xml
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
<!-- 在注入集合数据时，只要结构相同，标签可以互换 -->
<!-- 给数组注入数据  private String[] myStrs; -->
<property name="myStrs">
    <set>
        <value>AAA</value>
        <value>BBB</value>
        <value>CCC</value>
    </set>
</property>
<!-- 注入 list 集合数据  private List<String> myList; -->
<property name="myList">
    <array>
        <value>AAA</value>
        <value>BBB</value>
        <value>CCC</value>
    </array>
</property>
<!-- 注入 set 集合数据  private Set<String> mySet; -->
<property name="mySet">
    <list>
        <value>AAA</value>
        <value>BBB</value>
        <value>CCC</value>
    </list>
</property>
<!-- 注入 Map 数据 private Map<String,String> myMap; -->
<property name="myMap">
    <props>
        <prop key="testA">aaa</prop>
        <prop key="testB">bbb</prop>
    </props>
</property>
<!-- 注入 properties 数据  private Map<String,String> myMap;-->
    <property name="myProps">
        <map>
            <entry key="testA" value="aaa"></entry>
            <entry key="testB">
                <value>bbb</value>
            </entry>
        </map>
    </property>
```

# 4. 基于注解的 IOC 配置

## 4.1. xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 告知 spring 创建容器时要扫描的包 -->
    <context:component-scan base-package="com.itheima"></context:component-scan>
    <!-- 配置 dbAssit -->
    <bean id="dbAssit" class="com.itheima.dbassit.DBAssit">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!-- 配置数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql:///spring_day02"></property>
        <property name="user" value="root"></property>
        <property name="password" value="1234"></property>
    </bean>
</beans>
```

## 4.2. 常用注解

### 4.2.1. 用于创建对象的 

相当于： `<bean id="" class="">`

#### ①. @Component

**作用**： 把资源让 spring 来管理。相当于在 xml 中配置一个 bean。 

**属性**： `value`：指定 bean 的 id。如果不指定 value 属性，默认 bean 的 id 是当前类的类名。首字母小写。

#### ②. @Controller @Service @Repository

`@Controller`：一般用于表现层的注解。 

`@Service`：一般用于业务层的注解。 

`@Repository`：一般用于持久层的注解。 

**细节**：如果注解中有且只有一个属性要赋值时，且名称是 value，value 在赋值是可以不写。

### 4.2.2. 用于注入数据的

相当于： `<property name="" ref="">`  和  `<property name="" value="">`

#### ①. @Autowired

**作用**： 自动按照类型注入。当使用注解注入属性时，set 方法可以省略。它只能注入其他 bean 类型。当有多个 类型匹配时，使用要注入的对象变量名称作为 bean 的 id，在 spring 容器查找，找到了也可以注入成功。找不到 就报错

#### ②. @Qualifier

**作用**： 在自动按照类型注入的基础之上，再按照 Bean 的 id 注入。它在给字段注入时不能独立使用，必须和 `@Autowire` 一起使用；但是给方法参数注入时，可以独立使用。 

**属性**： `value`：指定 bean 的 id。

#### ③.  @Resource

**作用**： 直接按照 Bean 的 id 注入。它也只能注入其他 bean 类型。 

**属性**： `name`：指定 bean 的 id。

#### ④. @Value

**作用**： 注入基本数据类型和 String 类型数据的 

**属性**： `value`：用于指定值

### 4.2.3. 用于改变作用范围的：

相当于：`<bean id="" class="" scope="">`

#### ①.  @Scope

**作用**： 指定 bean 的作用范围。 

**属性**： `value`：指定范围的值。 取值：==singleton prototype== request session globalsession

### 4.2.4 和生命周期相关的

相当于：`<bean id="" class="" init-method="" destroy-method="" />`

#### ①. @PostConstruct

作用： 用于指定初始化方法。

#### ②.  @PreDestroy

作用： 用于指定销毁方法。

###  4.2.5. 关于 Spring 注解和 XML 的选择问题

**注解的优势**： 配置简单，维护方便（我们找到类，就相当于找到了对应的配置）。 

**XML 的优势**： 修改时，不用改源码。不涉及重新编译和部署。 



Spring 管理 Bean 方式的比较：

|                            | 基于XML配置                                    | 基于注解配置                                                 |
| -------------------------- | ---------------------------------------------- | ------------------------------------------------------------ |
| Bean定义                   | `<bean id="" class="">`                        | @Component<br />衍生类：@Repository、@Service、@Controller   |
| Bean名称                   | 通过id或name指定                               | @Component(“名称”)                                           |
| Bean注入                   | `<property>`或通过p命名空间                    | @Autowired 按类型注入<br />@Qualifier 按名称注入             |
| 生命过程<br />Bean作用范围 | init-method<br />destroy-method 范围 scope属性 | @PostConstruct 初始化<br />@PreDestroy 销毁<br />@Scope 设置作用范围 |
| 适用场景                   | Bean来自第三方，使用其他                       | Bean的实现类由用户自己开发                                   |

## 4.3. spring 管理对象细节

基于注解的 spring IoC 配置中，bean 对象的特点和基于 XML 配置是一模一样的。

## 4.4. spring 的纯注解配置

### 4.4.1 新注解说明

#### ①. @Configuration

**作用**： 用于指定当前类是一个 spring 配置类，当创建容器时会从该类上加载注解。获取容器时需要使用 `AnnotationApplicationContext`(有@Configuration 注解的类.class)。

**属性**： value:用于指定配置类的字节码

```Java
@Configuration
public class SpringConfiguration {
}
```

#### ②. @ComponentScan

**作用**： 用于指定 spring 在初始化容器时要扫描的包。作用和在 spring 的 xml 配置文件中的： 是一样的。 

**属性**： `basePackages`：用于指定要扫描的包。和该注解中的 value 属性作用一样。

```Java
@Configuration
@ComponentScan("com.itheima")
public class SpringConfiguration {
}
```

#### ③. @Bean

**作用**： 该注解只能写在方法上，表明使用此方法创建一个对象，并且放入 spring 容器。 

**属性**： `name`：给当前@Bean 注解方法创建的对象指定一个名称(即 bean 的 id）。

```Java
public class JdbcConfig {
    /**
* 创建一个数据源，并存入 spring 容器中
* @return
*/
    @Bean(name="dataSource")
    public DataSource createDataSource() {
        try {
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setUser("root");
            ds.setPassword("1234");
            ds.setDriverClass("com.mysql.jdbc.Driver");
            ds.setJdbcUrl("jdbc:mysql:///spring_day02");
            return ds;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
    /**
* 创建一个 DBAssit，并且也存入 spring 容器中
* @param dataSource
* @return
*/
    @Bean(name="dbAssit")
    public DBAssit createDBAssit(DataSource dataSource) {
        return new DBAssit(dataSource);
    }
}
```

#### ④. @PropertySource（4和5配合用）

**作用**：用于加载.properties 文件中的配置。例如我们配置数据源时，可以把连接数据库的信息写到 properties 配置文件中，就可以使用此注解指定 properties 配置文件的位置。 

**属性**： `value[]`：用于指定 properties 文件位置。如果是在类路径下，需要写上 classpath:

#### ⑤. @Import 

**作用**： 用于导入其他配置类，在引入其他配置类时，可以不用再写@Configuration 注解。当然，写上也没问 题。 

**属性**： `value[]`：用于指定其他配置类的字节码。

```Java
public class JdbcConfig {

    @Value("${jdbc.driver}")
    private String driver;

    @Value("${jdbc.url}")
    private String url;

    @Value("${jdbc.username}")
    private String username;

    @Value("${jdbc.password}")
    private String password;

    /**
     * 用于创建一个QueryRunner对象
     * @param dataSource
     * @return
     */
    @Bean(name="runner")
    @Scope("prototype")
    public QueryRunner createQueryRunner(@Qualifier("ds2") DataSource dataSource){
        return new QueryRunner(dataSource);
    }

    /**
     * 创建数据源对象
     * @return
     */
    @Bean(name="ds2")
    public DataSource createDataSource(){
        try {
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setDriverClass(driver);
            ds.setJdbcUrl(url);
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    @Bean(name="ds1")
    public DataSource createDataSource1(){
        try {
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setDriverClass(driver);
            ds.setJdbcUrl("jdbc:mysql://localhost:3306/eesy02");
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
}
```



```Java
@ComponentScan("com.itheima")
@Import(JdbcConfig.class)
@PropertySource("classpath:jdbcConfig.properties")
public class SpringConfiguration {}
```

### 4.4.2. 通过注解获取容器：

```Java
ApplicationContext ac = new AnnotationConfigApplicationContext(SpringConfiguration.class);
```

### 4.4.3. 工程结构图

![image-20210607200919928](images\01_Spring\image-20210607200919928.png)、

# 5. Spring 整合 Junit

第一步：拷贝整合 junit 的必备 jar 包到 lib 目录

第二步：使用`@RunWith` 注解替换原有运行器

- ```Java
  @RunWith(SpringJUnit4ClassRunner.class)
  public class AccountServiceTest {
  }
  ```

第三步：使用`@ContextConfiguration` 指定 spring 配置文件的位置

- `@ContextConfiguration` 注解： 
  - `locations` 属性：用于指定配置文件的位置。如果是类路径下，需要用 `classpath:表明` 
  - `classes` 属性：用于指定注解的类。当不使用 xml 配置时，需要用此属性指定注解类的位置。

- ```Java
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(locations= {"classpath:bean.xml"})
  public class AccountServiceTest {
  }
  ```

第四步：使用@Autowired 给测试类中的变量注入数据

- ```Java
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(locations= {"classpath:bean.xml"})
  public class AccountServiceTest {
      @Autowired
      private IAccountService as ;
  }
  
  ```

  

# 6. AOP 的相关概念

## 6.1. AOP 概述

### 6.1.1. 什么是 AOP

AOP：全称是 Aspect Oriented Programming 即：面向切面编程。

简单的说它就是把我们程序重复的代码抽取出来，在需要执行的时候，使用动态代理的技术，在不修改源码的 基础上，对我们的已有方法进行增强。

### 6.1.2. AOP 的作用及优势

**作用**： 在程序运行期间，不修改源码对已有方法进行增强。 

**优势**： 减少重复代码、提高开发效率、维护方便

### 6.1.3. AOP 的实现方式

使用动态代理技术

## 6.2. 动态代理

### 6.2.1.  动态代理的特点

字节码随用随创建，随用随加载。 

它与静态代理的区别也在于此。因为静态代理是字节码一上来就创建好，并完成加载。 装饰者模式就是静态代理的一种体现。

动态代理常用的有两种方式

### 6.2.2. 动态代理常用的有两种方式

**基于接口的动态代理**

提供者：JDK 官方的 Proxy 类。 

要求：被代理类最少实现一个接口。 

**基于子类的动态代理** 

提供者：第三方的 CGLib，如果报 asmxxxx 异常，需要导入 asm.jar。 

要求：被代理类不能用 final 修饰的类（最终类）。

### 6.2.3. 使用 JDK 官方的 Proxy 类创建代理对象

```java 
public class Client {

    public static void main(String[] args) {
        final Producer producer = new Producer();

        /**
         *  如何创建代理对象：
         *      使用Proxy类中的newProxyInstance方法
         *  创建代理对象的要求：
         *      被代理类最少实现一个接口，如果没有则不能使用
         *  newProxyInstance方法的参数：
         *      ClassLoader：类加载器
         *          它是用于加载代理对象字节码的。和被代理对象使用相同的类加载器。固定写法。
         *      Class[]：字节码数组
         *          它是用于让代理对象和被代理对象有相同方法。固定写法。
         *      InvocationHandler：用于提供增强的代码
         *          它是让我们写如何代理。我们一般都是些一个该接口的实现类，通常情况下都是匿名内部类，但不是必须的。
         *          此接口的实现类都是谁用谁写。
         */
       IProducer proxyProducer = (IProducer) Proxy.newProxyInstance(producer.getClass().getClassLoader(),
                producer.getClass().getInterfaces(),
                new InvocationHandler() {
                    /**
                     * 作用：执行被代理对象的任何接口方法都会经过该方法
                     * 方法参数的含义
                     * @param proxy   代理对象的引用
                     * @param method  当前执行的方法
                     * @param args    当前执行方法所需的参数
                     * @return        和被代理对象方法有相同的返回值
                     * @throws Throwable
                     */
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        //提供增强的代码
                        Object returnValue = null;

                        //1.获取方法执行的参数
                        Float money = (Float)args[0];
                        //2.判断当前方法是不是销售
                        if("saleProduct".equals(method.getName())) {
                            returnValue = method.invoke(producer, money*0.8f);
                        }
                        return returnValue;
                    }
                });
        proxyProducer.saleProduct(10000f);
    }
}
```

### 6.2.4. 使用 CGLib 的 Enhancer 类创建代理对象

```java 
public class Client {

    public static void main(String[] args) {
        final Producer producer = new Producer();

        /**
         *  基于子类的动态代理：
         *      涉及的类：Enhancer
         *      提供者：第三方cglib库
         *  如何创建代理对象：
         *      使用Enhancer类中的create方法
         *  创建代理对象的要求：
         *      被代理类不能是最终类
         *  create方法的参数：
         *      Class：字节码
         *          它是用于指定被代理对象的字节码。
         *
         *      Callback：用于提供增强的代码
         *          它是让我们写如何代理。我们一般都是些一个该接口的实现类，通常情况下都是匿名内部类，但不是必须的。
         *          此接口的实现类都是谁用谁写。
         *          我们一般写的都是该接口的子接口实现类：MethodInterceptor
         */
        Producer cglibProducer = (Producer)Enhancer.create(producer.getClass(), new MethodInterceptor() {
            /**
             * 执行北地阿里对象的任何方法都会经过该方法
             * @param proxy
             * @param method
             * @param args
             *    以上三个参数和基于接口的动态代理中invoke方法的参数是一样的
             * @param methodProxy ：当前执行方法的代理对象
             * @return
             * @throws Throwable
             */
            @Override
            public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
                //提供增强的代码
                Object returnValue = null;

                //1.获取方法执行的参数
                Float money = (Float)args[0];
                //2.判断当前方法是不是销售
                if("saleProduct".equals(method.getName())) {
                    returnValue = method.invoke(producer, money*0.8f);
                }
                return returnValue;
            }
        });
        cglibProducer.saleProduct(12000f);
    }
}

```

# 7. Spring 中的 AOP

## 7.1. Spring 中 AOP 的细节

### 7.1.1. AOP 相关术语

==Joinpoint==(连接点)：所谓连接点是指那些被拦截到的点。在 spring 中，这些点指的是方法，因为 spring 只支持方法类型的 连接点。 

==Pointcut==(切入点)：所谓切入点是指我们要对哪些 Joinpoint 进行拦截的定义

==Advice==(通知/增强)： 所谓通知是指拦截到 Joinpoint 之后所要做的事情就是通知。 

- 通知的类型：前置通知、后置通知、异常通知、最终通知、环绕通知。 

==Introduction==(引介)：引介是一种特殊的通知在不修改类代码的前提下, Introduction 可以在运行期为类动态地添加一些方 法或 Field。 

==Target==(目标对象)：代理的目标对象。 

==Weaving==(织入)：是指把增强应用到目标对象来创建新的代理对象的过程。 spring 采用动态代理织入，而 AspectJ 采用编译期织入和类装载期织入。 

==Proxy==（代理）：一个类被 AOP 织入增强后，就产生一个结果代理类。 

==Aspect==(切面)：是切入点和通知（引介）的结合。

### 7.1.2. 学习 spring 中的 AOP 要明确的事

1. 发阶段（我们做的） 

   - 编写核心业务代码（开发主线）：大部分程序员来做，要求熟悉业务需求。
   - 把公用代码抽取出来，制作成通知。（开发阶段最后再做）：AOP 编程人员来做。 
   - 在配置文件中，声明切入点与通知间的关系，即切面。：AOP 编程人员来做。 

2. 运行阶段（Spring 框架完成的） 

   Spring 框架监控切入点方法的执行。一旦监控到切入点方法被运行，使用代理机制，动态创建目标对 象的代理对象，根据通知类别，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行。

## 7.2. 基于 XML 的 AOP 配置

**spring中基于XML的AOP配置步骤：**

1. 把通知Bean也交给spring来管理
2. 使用`aop:config`标签表明开始AOP的配置
3. 使用`aop:aspect`标签表明配置切面
   - `id`属性：是给切面提供一个唯一标识
   - `ref`属性：是指定通知类bean的Id。
4. 在`aop:aspect`标签的内部使用对应标签来配置通知的类型              
   -  `aop:before`：表示配置前置通知                  
     - `method`属性：用于指定Logger类中哪个方法是前置通知                    
     - `pointcut`属性：用于指定切入点表达式，该表达式的含义指的是对业务层中哪些方法增强

```xml
<!-- 配置Logger类 -->
    <bean id="logger" class="com.itheima.utils.Logger"></bean>

    <!--配置AOP-->
    <aop:config>
        <!--配置切面 -->
        <aop:aspect id="logAdvice" ref="logger">
            <!-- 配置通知的类型，并且建立通知方法和切入点方法的关联-->
            <aop:before method="printLog" pointcut="execution(* com.itheima.service.impl.*.*(..))"></aop:before>
        </aop:aspect>
    </aop:config>
```



### 7.2.1. 标签说明

1. `<aop:config>`

   **作用**：用于声明开始 aop 的配置

2. `<aop:aspect>`

   **作用**： 用于配置切面。 

   **属性**： 

   - `id`：给切面提供一个唯一标识。 
   - `ref`：引用配置好的通知类 bean 的 id。

3. `<aop:pointcut>` 

   **作用**： 用于配置切入点表达式。就是指定对哪些类的哪些方法进行增强。 

   **属性**：

   - `expression`：用于定义切入点表达式。 
   - `id`：用于给切入点表达式提供一个唯一标识

4. `<aop:before>`

   **作用**： 用于配置前置通知。指定增强的方法在切入点方法之前执行 

   **属性**：

   - `method`：用于指定通知类中的增强方法名称 
   - `ponitcut-ref`：用于指定切入点的表达式的引用 
   - `poinitcut`：用于指定切入点表达式 

   **执行时间点**： ==切入点方法执行之前执行==

5. `<aop:after-returning>`

   **作用**： 用于配置后置通知 

   **属性**： 

   - `method`：指定通知中方法的名称。 
   - `pointct`：定义切入点表达式 
   - `pointcut-ref`：指定切入点表达式的引用 

   **执行时间点**： ==切入点方法正常执行之后。它和异常通知只能有一个执行==

6. `<aop:after-throwing>`

   **作用**： 用于配置异常通知 

   **属性**： 

   - `method`：指定通知中方法的名称。 
   - `pointct`：定义切入点表达式 
   - `pointcut-ref`：指定切入点表达式的引用 

   **执行时间点**： ==切入点方法执行产生异常后执行。它和后置通知只能执行一个==

7. `<aop:after>`

   **作用**： 用于配置最终通知 

   **属性**： 

   - `method`：指定通知中方法的名称。 
   - `pointct`：定义切入点表达式 
   - `pointcut-ref`：指定切入点表达式的引用

   **执行时间点**： ==无论切入点方法执行时是否有异常，它都会在其后面执行==。

   

### 7.2.2. 切入点表达式说明

==execution==:匹配方法的执行(常用) 

execution(表达式) 

**表达式语法**：`execution([修饰符] 返回值类型 包名.类名.方法名(参数))` 

**写法说明**： 

1. 全匹配方式：
    `public void  com.itheima.service.impl.AccountServiceImpl.saveAccount(com.itheima.domain.Account)` 
2. 访问修饰符可以省略
    `void  com.itheima.service.impl.AccountServiceImpl.saveAccount(com.itheima.domain.Account)`
3. 返回值可以使用*号，表示任意返回值 
   `*  com.itheima.service.impl.AccountServiceImpl.saveAccount(com.itheima.domain.Account)`
4. 包名可以使用\*号，表示任意包，但是有几级包，需要写几个
   `* * *.*.*.*.AccountServiceImpl.saveAccount(com.itheima.domain.Account)`
5. 使用..来表示当前包，及其子包
   `* com..AccountServiceImpl.saveAccount(com.itheima.domain.Account)`
6. 类名可以使用*号，表示任意类 
   `* com..*.saveAccount(com.itheima.domain.Account)`
7. 方法名可以使用*号，表示任意方法 
   `* com..*.*( com.itheima.domain.Account)`
8. 参数列表可以使用*，表示参数可以是任意数据类型，但是必须有参数 
   `* com..*.*(*)`
9. 参数列表可以使用..表示有无参数均可，有参数可以是任意类型
    `* com..*.*(..)`
10. 全通配方式：
     `* *..*.*(..)`

> 【警告】注： 通常情况下，我们都是对业务层的方法进行增强，所以切入点表达式都是切到业务层实现类。 
>
> `execution(* com.itheima.service.impl.*.*(..))`

### 7.2.3 环绕通知

**配置方式**:

- ```xml
  <aop:config>
      <aop:pointcut expression="execution(* com.itheima.service.impl.*.*(..))"id="pt1"/>
      <aop:aspect id="txAdvice" ref="txManager">
          <!-- 配置环绕通知 -->
          <aop:around method="transactionAround" pointcut-ref="pt1"/>
      </aop:aspect>
  </aop:config>
  ```

`<aop:around>`

1. **作用**： 用于配置环绕通知 

2. **属性**：

   -  `method`：指定通知中方法的名称。 
   - `pointct`：定义切入点表达式 
   - `pointcut-ref`：指定切入点表达式的引用 

3. **说明**： 它是 spring 框架为我们提供的一种可以在代码中手动控制增强代码什么时候执行的方式。 

4. > 【危险】注意： 通常情况下，环绕通知都是独立使用的

## 7.3. 基于注解的 AOP 配置

在配置文件中指定 spring 要扫描的包

- ```xml
  <!-- 告知 spring，在创建容器时要扫描的包 -->
  <context:component-scan base-package="com.itheima"></context:component-scan>
  ```



 **配置步骤**

第一步：把通知类也使用注解配置

- ```java
  @Component("txManager")
  public class TransactionManager {
      //定义一个 DBAssit
      @Autowired
      private DBAssit dbAssit ;
  }
  ```

第二步：在通知类上使用`@Aspect` 注解声明为切面

- ```java 
  @Component("txManager")
  @Aspect//表明当前类是一个切面类
  public class TransactionManager {
      //定义一个 DBAssit
      @Autowired
      private DBAssit dbAssit ;
  }
  ```

第三步：在增强的方法上使用注解配置通知

- ```java
  //开启事务
  @Before("execution(* com.itheima.service.impl.*.*(..))")
  public void beginTransaction() {
      try {
          dbAssit.getCurrentConnection().setAutoCommit(false);
      } catch (SQLException e) {
          e.printStackTrace();
      }
  }
  ```

第四步：在 spring 配置文件中开启 spring 对注解 AOP 的支持

- ```xml
  <!-- 开启 spring 对注解 AOP 的支持 -->
  <aop:aspectj-autoproxy/>
  ```

  

### 7.3.1. 注解说明

1. `@Aspect` 

   - **作用**： 把当前类声明为切面类

2. `<@Before>` 

   - **作用**： 把当前方法看成是前置通知。 

   - **属性**： 

     - `value`：用于指定切入点表达式，还可以指定切入点表达式的引用。

   - ```java
     @Before("execution(* com.itheima.service.impl.*.*(..))")
     public void beginTransaction() {
         try {// 方法开始时, 开启事务
             dbAssit.getCurrentConnection().setAutoCommit(false);
         } catch (SQLException e) {
             e.printStackTrace();
         }
     }
     ```

     

3. `<@AfterReturning>`

   -  **作用**： 把当前方法看成是后置通知。 

   - **属性**： 

     - `value`：用于指定切入点表达式，还可以指定切入点表达式的引用

   - ```java
     @AfterReturning("execution(* com.itheima.service.impl.*.*(..))")
     public void commit() {
         try { // 方法要结束时, 提交事务
             dbAssit.getCurrentConnection().commit();
         } catch (SQLException e) {
             e.printStackTrace();
         }
     }
     ```

     

4. `<@AfterThrowing>` 

   - **作用**： 把当前方法看成是异常通知。 

   - **属性**： 

     - `value`：用于指定切入点表达式，还可以指定切入点表达式的引用

   - ```java
     @AfterThrowing("execution(* com.itheima.service.impl.*.*(..))")
     public void rollback() {
         try { // 方法出现异常, 回滚事务
             dbAssit.getCurrentConnection().rollback();
         } catch (SQLException e) {
             e.printStackTrace();
         }
     }
     ```

     

5. `<@After>`

   - **作用**：把当前方法看成是最终通知。
   - **属性**：
     - `value`：用于指定切入点表达式，还可以指定切入点表达式的引用

   - ```java
     @After("execution(* com.itheima.service.impl.*.*(..))")
     public void release() {
         try {
             dbAssit.releaseConnection();
         } catch (Exception e) {
             e.printStackTrace();
         }
     }
     ```

### 7.3.2. 环绕通知注解配置

 `@Around` 

**作用**： 把当前方法看成是环绕通知。 

**属性**： `value`：用于指定切入点表达式，还可以指定切入点表达式的引用。

### 7.3.3. 切入点表达式注解

`@Pointcut` 

**作用**： 指定切入点表达式 

**属性**：`value`：指定表达式的内容

```java
@Pointcut("execution(* com.itheima.service.impl.*.*(..))")
private void pt1() {}

@Around("pt1()")//注意：千万别忘了写括号
public Object transactionAround(ProceedingJoinPoint pjp) {
	//定义返回值
	Object rtValue = null
}
```



## 7.4. 不使用 XML 的配置方式

```java
@Configuration
@ComponentScan(basePackages="com.itheima")
@EnableAspectJAutoProxy // 开启 spring 对注解 AOP 的支持
public class SpringConfiguration {
}
```



# 8. Spring 中的事务控制

## 8.1. Spring 中事务控制的 API 介绍

### 8.1.1. PlatformTransactionManager

`PlatformTransactionManager`接口是 spring 的事务管理器，它里面提供了我们常用的操作事务的方法：

1. `TransactionStatus getTransaction(TransactionDefinetion)`：获取事务状态信息：
2. `void commit(TransactionStatus )`：提交事务
3. `void rollback(TransactionStatus )`：回滚事务

真正管理事务的对象 

1. `org.springframework.jdbc.datasource.DataSourceTransactionManager` 使用 Spring  JDBC 或 iBatis 进行持久化数据时使用 
2. `org.springframework.orm.hibernate5.HibernateTransactionManager` 使用 Hibernate 版本进行持久化数据时使用

### 8.1.2. TransactionDefinition

它是事务的定义信息对象，里面有如下方法：

1. `String getName()`：获取事务对象名称
2. `int getIsolationLevel()`：获取事务隔离级别
3. `int getProPagationBehavior()`：获取事务传播行为
4. `int getTimeout()`：获取事务超时时间
5. `boolean isReadOnly()`：获取事务是否只读
   - 读写型事务：增加、删除、修改开始事务
   - 只读型事务：执行查询时，也会开启事务

#### ①. 事务的隔离级

事务隔离级别反映事物提交并发访问时的处理态度

1. ISOLATION_DEFAULT：默认级别，归属下列某一种
2. ISOLATION_READ_UNCOMMITTED：可以读取未提交的数据
3. ISOLATION_READ_COMMITTED：只能读取已提交数据，解决脏读问题（Oracle默认级别）
4. ISOLATION_REPEATEABLE_READ：是否读取其他事务提交修改后的数据，解决不可重复问题（MySQL默认级别）
5. ISOLATION_SERIALIZABLE：是否读取其他事务提交添加后的数据，解决幻影读问题

#### ②. 事务的传播行为

==REQUIRED：如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。一般的选 择（默认值）== 

==SUPPORTS：支持当前事务，如果当前没有事务，就以非事务方式执行（没有事务）== 

MANDATORY：使用当前的事务，如果当前没有事务，就抛出异常 

REQUERS_NEW：新建事务，如果当前在事务中，把当前事务挂起。 

NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起 

NEVER：以非事务方式运行，如果当前存在事务，抛出异常 

NESTED：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行 REQUIRED 类似的操作。

#### ③. 超时时间

默认值是-1，没有超时限制。如果有，以秒为单位进行设置。

#### ④. 是否是只读事务

建议查询时设置为只读。

### 8.1.3. TransactionStatus

`TransactionStatus`接口提供的是事务具体的运行状态，方法如下：

1. `void flush()`：刷新事务
2. `boolean hasSavepoint()`：获取是否是否存在存储点
3. `boolean isCompleted()`：获取事务是否完成
4. `boolean isNewTransaction()`：获取事务是否为新的事务
5. `boolean isRollbackOnly()`：获取事务是否回滚
6. `void setRollbackOnly()`：设置事务回滚

## 8.2. 基于 XML 的声明式事务控制（配置方式）重点

**准备工作**：导入 aop 和 tx 两个名称空间

- ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:aop="http://www.springframework.org/schema/aop"
   xmlns:tx="http://www.springframework.org/schema/tx"
   xsi:schemaLocation="http://www.springframework.org/schema/beans 
   http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/tx 
   http://www.springframework.org/schema/tx/spring-tx.xsd
   http://www.springframework.org/schema/aop 
   http://www.springframework.org/schema/aop/spring-aop.xsd">
  </beans>
  ```

**配置步骤**

第一步：配置事务管理器

- ```xml
  <!-- 配置一个事务管理器 -->
  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
      <!-- 注入 DataSource -->
      <property name="dataSource" ref="dataSource"></property>
  </bean>
  ```

第二步：配置事务的通知引用事务管理器

- ```xml
  <!-- 事务的配置 -->
  <tx:advice id="txAdvice" transaction-manager="transactionManager">
  </tx:advice>
  ```

第三步：配置事务的属性

- ```xml
  <!--在 tx:advice 标签内部 配置事务的属性 -->
  <tx:attributes>
      <tx:method name="*" read-only="false" propagation="REQUIRED"/>
      <tx:method name="find*" read-only="true" propagation="SUPPORTS"/>
  </tx:attributes>
  ```

- `<tx:attributes>` 指定方法名称：是业务核心方法

  - `read-only`：是否是只读事务。默认 false，不只读。
  - `isolation`：指定事务的隔离级别。默认值是使用数据库的默认隔离级别。
  - `propagation`：指定事务的传播行为。
  - `timeout`：指定超时时间。默认值为：-1。永不超时。
  - `rollback-for`：用于指定一个异常，当执行产生该异常时，事务回滚。产生其他异常，事务不回滚。没有默认值，任何异常都回滚。
  - `no-rollback-for`：用于指定一个异常，当产生该异常时，事务不回滚，产生其他异常时，事务回滚。没有默认值，任何异常都回滚。

第四步：配置 AOP 切入点表达式

- ```xml
  <!-- 配置 aop -->
  <aop:config>
      <!-- 配置切入点表达式 -->
      <aop:pointcut expression="execution(* com.itheima.service.impl.*.*(..))" id="pt1"/>
  </aop:config>
  ```

第五步：配置切入点表达式和事务通知的对应关系

- ```xml
  <!-- 在 aop:config 标签内部：建立事务的通知和切入点表达式的关系 -->
  <aop:advisor advice-ref="txAdvice" pointcut-ref="pt1"/>
  ```

## 8.3. 基于注解的配置方式

**配置步骤**

第一步：配置事务管理器并注入数据源

- ```xml
  <!-- 配置事务管理器 -->
  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
      <property name="dataSource" ref="dataSource"></property>
  </bean>
  ```

第二步：在业务层使用`@Transactional` 注解

- ```java
  @Service("accountService")
  @Transactional(readOnly=true,propagation=Propagation.SUPPORTS)
  public class AccountServiceImpl implements IAccountService {
      @Autowired
      private IAccountDao accountDao;
  }
  ```

- 该注解的属性和 xml 中的属性含义一致。

  - 该注解可以出现在接口上，类上和方法上。 
  - 出现接口上，表示该接口的所有实现类都有事务支持。 
  - 出现在类上，表示类中所有方法有事务支持 出现在方法上，表示方法有事务支持。 
  - 以上三个位置的优先级：==方法 > 类 > 接口==

第三步：在配置文件中开启 spring 对注解事务的支持

- ```xml
  <!-- 开启 spring 对注解事务的支持 -->
  <tx:annotation-driven transaction-manager="transactionManager"/>
  ```

## 8.4. 不使用 xml 的配置方式

```java
@Configuration
@EnableTransactionManagement
public class SpringTxConfiguration {
    //里面配置数据源，配置 JdbcTemplate,配置事务管理器。在之前的步骤已经写过了。
}
```










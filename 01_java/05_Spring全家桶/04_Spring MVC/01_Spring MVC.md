#   1. SpringMVC 的基本概念

## 1.1. 关于三层架构和 MVC

### 1.1.1. 三层架构

我们的开发架构一般都是基于两种形式，一种是 C/S 架构，也就是客户端/服务器，另一种是 B/S 架构，也就 是浏览器服务器。在 JavaEE 开发中，几乎全都是基于 B/S 架构的开发。那么在 B/S 架构中，系统标准的三层架构 包括：表现层、业务层、持久层。三层架构在我们的实际开发中使用的非常多，所以我们课程中的案例也都是基于 三层架构设计的。 

三层架构中，每一层各司其职，接下来我们就说说每层都负责哪些方面： 

==表现层==： 

- 也就是我们常说的web层。它负责接收客户端请求，向客户端响应结果，通常客户端使用http协议请求 web 层，web 需要接收 http 请求，完成 http 响应。 

- 表现层包括展示层和控制层：控制层负责接收请求，展示层负责结果的展示。 
- 表现层依赖业务层，接收到客户端请求一般会调用业务层进行业务处理，并将处理结果响应给客户端。 
- ==表现层的设计一般都使用 MVC 模型==。（MVC 是表现层的设计模型，和其他层没有关系） 

==业务层==： 

- 也就是我们常说的 service 层。它负责业务逻辑处理，和我们开发项目的需求息息相关。web 层依赖业 务层，但是业务层不依赖 web 层。 
- 业务层在业务处理时可能会依赖持久层，如果要对数据持久化需要保证事务一致性。（也就是我们说的， 事务应该放到业务层来控制） 

==持久层==： 

- 也就是我们是常说的 dao 层。负责数据持久化，包括数据层即数据库和数据访问层，数据库是对数据进 行持久化的载体，数据访问层是业务层和持久层交互的接口，业务层需要通过数据访问层将数据持久化到数据库 中。通俗的讲，持久层就是和数据库交互，对数据库表进行曾删改查的。

### 1.1.2. MVC 模型

MVC 全名是 Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写， 是一种用于设计创建 Web 应用程序表现层的模式。MVC 中每个部分各司其职： 

==Model（模型）==： 通常指的就是我们的数据模型。作用一般情况下用于封装数据。 

==View（视图）==： 通常指的就是我们的 jsp 或者 html。作用一般就是展示数据的。 通常视图是依据模型数据创建的。

==Controller（控制器）==： 是应用程序中处理用户交互的部分。作用一般就是处理程序逻辑的。

## 1.2. SpringMVC 概述

### 1.2.1. SpringMVC 是什么

SpringMVC 是一种基于 Java 的实现 MVC 设计模型的请求驱动类型的轻量级 Web 框架，属于 Spring  FrameWork 的后续产品，已经融合在 Spring Web Flow 里面。Spring 框架提供了构建 Web 应用程序的全功 能 MVC 模块。使用 Spring 可插入的 MVC 架构，从而在使用 Spring 进行 WEB 开发时，可以选择使用 Spring 的 Spring MVC 框架或集成其他 MVC 开发框架，如 Struts1(现在一般不用)，Struts2 等。 

SpringMVC 已经成为目前最主流的 MVC 框架之一，并且随着 Spring3.0 的发布，全面超越 Struts2，成 为最优秀的 MVC 框架。 

它通过一套注解，让一个简单的 Java 类成为处理请求的控制器，而无须实现任何接口。同时它还支持 RESTful 编程风格的请求。

### 1.2.2. SpringMVC 在三层架构的位置

![image-20210611154716799](images\01_Spring MVC\image-20210611154716799.png)

### 1.2.3. SpringMVC 的优势

清晰的角色划分： 

1. 前端控制器（DispatcherServlet） 
2. 请求到处理器映射（HandlerMapping） 
3. 处理器适配器（HandlerAdapter） 
4. 视图解析器（ViewResolver） 
5. 处理器或页面控制器（Controller） 
6. 验证器（ Validator） 
7. 命令对象（Command 请求参数绑定到的对象就叫命令对象） 
8. 表单对象（Form Object 提供给表单展示和提交到的对象就叫表单对象）。 

分工明确，而且扩展点相当灵活，可以很容易扩展，虽然几乎不需要。

由于命令对象就是一个 POJO，无需继承框架特定 API，可以使用命令对象直接作为业务对象。

和 Spring 其他框架无缝集成，是其它 Web 框架所不具备的。 

可适配，通过 HandlerAdapter 可以支持任意的类作为处理器。

可定制性，HandlerMapping、ViewResolver 等能够非常简单的定制。 

功能强大的数据验证、格式化、绑定机制。

利用 Spring 提供的 Mock 对象能够非常简单的进行 Web 层单元测试。

本地化、主题的解析的支持，使我们更容易进行国际化和主题的切换。 

强大的 JSP 标签库，使 JSP 编写更容易。

………………还有比如RESTful风格的支持、简单的文件上传、约定大于配置的契约式编程支持、基于注解的零配 置支持等等。

### 1.2.4. SpringMVC 和 Struts2 的优略分析

**共同点**： 

1. 它们都是表现层框架，都是基于 MVC 模型编写的。 
2. 它们的底层都离不开原始 ServletAPI。
3.  它们处理请求的机制都是一个核心控制器。 

**区别**： 

1. Spring MVC 的入口是 Servlet, 而 Struts2 是 Filter  
2. Spring MVC 是基于方法设计的，而 Struts2 是基于类，Struts2 每次执行都会创建一个动作类。所 以 Spring MVC 会稍微比 Struts2 快些。 
3. Spring MVC 使用更加简洁,同时还支持 JSR303, 处理 ajax 的请求更方便 (JSR303 是一套 JavaBean 参数校验的标准，它定义了很多常用的校验注解，我们可以直接将这些注 解加在我们 JavaBean 的属性上面，就可以在需要校验的时候进行校验了。) 
4. Struts2 的 OGNL 表达式使页面的开发效率相比 Spring MVC 更高些，但执行效率并没有比 JSTL 提 升，尤其是 struts2 的表单标签，远没有 html 执行效率高。

# 2. SpringMVC 的入门

## 2.1. 入门

第一步：导入相关jar

第二步：在web.xml中配置核心控制器-一个 Servlet

- ```xml
   <?xml version="1.0" encoding="UTF-8"?>
  <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns="http://java.sun.com/xml/ns/javaee"
           xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
                               http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
           id="WebApp_ID" version="2.5">
      <!-- 配置 spring mvc 的核心控制器 -->
      <servlet>
          <servlet-name>SpringMVCDispatcherServlet</servlet-name>
          <servlet-class>
              org.springframework.web.servlet.DispatcherServlet
          </servlet-class>
          <!-- 配置初始化参数，用于读取 SpringMVC 的配置文件 -->
          <init-param>
              <param-name>contextConfigLocation</param-name>
              <param-value>classpath:SpringMVC.xml</param-value>
          </init-param>
          <!-- 配置 servlet 的对象的创建时间点：应用加载时创建。取值只能是非 0 正整数，表示启动顺序 -->
          <load-on-startup>1</load-on-startup>
      </servlet>
      <servlet-mapping>
          <servlet-name>SpringMVCDispatcherServlet</servlet-name>
          <url-pattern>/</url-pattern>
      </servlet-mapping>
  </web-app>
  ```

第三步：创建 spring mvc 的配置文件

- ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:mvc="http://www.springframework.org/schema/mvc"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans 
                             http://www.springframework.org/schema/beans/spring-beans.xsd
                             http://www.springframework.org/schema/mvc
                             http://www.springframework.org/schema/mvc/spring-mvc.xsd
                             http://www.springframework.org/schema/context 
                             http://www.springframework.org/schema/context/spring-context.xsd">
      <!-- 配置创建 spring 容器要扫描的包 -->
      <context:component-scan base-package="com.itheima"></context:component-scan>
  
      <!-- 配置视图解析器 -->
      <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
          <property name="prefix" value="/WEB-INF/pages/"></property>
          <property name="suffix" value=".jsp"></property>
      </bean>
  </beans>
  ```

## 2.2. 执行过程及原理分析

### 2.2.1. 执行过程

![image-20210611181938032](images\01_Spring MVC\image-20210611181938032.png)

1. 服务器启动，应用被加载。读取到 web.xml 中的配置创建 spring 容器并且初始化容器中的对象。
2. 浏览器发送请求，被 DispatherServlet 捕获，该 Servlet 并不处理请求，而是把请求转发出去。转发 的路径是根据请求 URL，匹配@RequestMapping 中的内容。
3. 匹配到了后，执行对应方法。该方法有一个返回值。
4. 根据方法的返回值，借助 InternalResourceViewResolver 找到对应的结果视图。
5. 渲染结果视图，响应浏览器。

### 2.2.2. SpringMVC 的请求响应流程

![image-20210611182127630](images\01_Spring MVC\image-20210611182127630.png)

## 2.3. 涉及的组件

### 2.3.1. DispatcherServlet：前端控制器

用户请求到达前端控制器，它就==相当于 mvc 模式中的 c==，dispatcherServlet 是整个流程控制的中心，由 它调用其它组件处理用户的请求，dispatcherServlet 的存在降低了组件之间的耦合性。

### 2.3.2. HandlerMapping：处理器映射器

HandlerMapping 负责根据用户请求找到 Handler 即处理器，SpringMVC 提供了不同的映射器实现不同的 映射方式，例如：配置文件方式，实现接口方式，注解方式等

### 2.3.3. Handler：处理器

它就是我们开发中要编写的具体业务控制器。由 DispatcherServlet 把用户请求转发到 Handler。由 Handler 对具体的用户请求进行处理。

### 2.3.4. HandlAdapter：处理器适配器

通过 HandlerAdapter 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理 器进行执行。

![image-20210611182402755](images\01_Spring MVC\image-20210611182402755.png)

### 2.3.5. View Resolver：视图解析器

View Resolver 负责将处理结果生成 View 视图，View Resolver 首先根据逻辑视图名解析成物理视图名 即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。

### 2.3.6. View：视图

SpringMVC 框架提供了很多的 View 视图类型的支持，包括：jstlView、freemarkerView、pdfView 等。我们最常用的视图就是 jsp。 

一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开 发具体的页面。

### 2.3.7 `<mvc:annotation-driven>` 说明

在 SpringMVC 的各个组件中，处理器映射器、处理器适配器、视图解析器称为 SpringMVC 的三大组件。 

使 用  自动加载 `RequestMappingHandlerMapping` （处理映射器） 和 `RequestMappingHandlerAdapter` （ 处 理 适 配 器 ） ， 可 用 在 SpringMVC.xml 配 置 文 件 中 使 用 `<mvc:annotation-driven>>` 替代注解处理器和适配器的配置。 它就相当于在 xml 中配置了：

```xml
<!-- 上面的标签相当于 如下配置-->
<!-- Begin -->
<!-- HandlerMapping -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"></bean>
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>
<!-- HandlerAdapter -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"></bean>
<bean class="org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter"></bean>
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean>
<!-- HadnlerExceptionResolvers -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.ExceptionHandlerExceptionResolver"></bean>
<bean class="org.springframework.web.servlet.mvc.annotation.ResponseStatusExceptionResolver"></bean>
<bean class="org.springframework.web.servlet.mvc.support.DefaultHandlerExceptionResolver"></bean>
<!-- End -->
```

## 2.4. RequestMapping 注解

源码

- ```java
  @Target({ElementType.METHOD, ElementType.TYPE})
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  @Mapping
  public @interface RequestMapping {
  }
  ```

**作用**： 用于建立请求 URL 和处理请求方法之间的对应关系。

**出现位置**： 

1. 类上： 

   - 请求 URL 的第一级访问目录。此处不写的话，就相当于应用的根目录。写的话需要以/开头。 

   - 它出现的目的是为了使我们的 URL 可以按照模块化管理: 

     例如： 

     ​	    账户模块： /account/add、/account/update、/account/delete ... 
     ​       订单模块： /order/add、/order/update、/order/delete 

     （account和order）就是把 RequsetMappding 写在类上，使我们的 URL 更加精细。 

2. 方法上： 请求 URL 的第二级访问目录。 

**属性**： 

1. `value`：用于指定请求的 URL。它和 path 属性的作用是一样的。 

2. `method`：用于指定请求的方式。 

3. `params`：用于指定限制请求参数的条件。它支持简单的表达式。要求请求参数的 key 和 value 必须和 配置的一模一样。 

   例如： params = {"accountName"}，表示请求参数必须有 accountName 
                params = {"moeny!100"}，表示请求参数中 money 不能是 100。 

4. `headers`：用于指定限制请求消息头的条件。 

   > 【危险】注意： 以上四个属性只要出现 2 个或以上时，他们的关系是与的关系。



### 2.4.1. 出现位置

```java
@Controller("accountController")
@RequestMapping("/account")	// 出现在类上
public class AccountController {
	// 出现在方法上
    @RequestMapping("/findAccount")
    public String findAccount() {
        System.out.println("查询了账户。。。。");
        return "success";
    }
}
```

```html
<a href="account/findAccount">查询账户</a>
```

> 【危险】当我们使用此种方式配置时，不要在访问 URL 前面加/，否则无法找到资源。

### 2.4.2.  method 属性

```java
// 请求方式为 POST
@RequestMapping(value="/saveAccount",method=RequestMethod.POST)
public String saveAccount() {
    System.out.println("保存了账户");
    return "success";
}
```

### 2.4.3. params 属性

```Java
@RequestMapping(value="/removeAccount",params= {"accountName","money>100"})
public String removeAccount() {
    System.out.println("删除了账户");
    return "success";
}
```

```html
<!-- 请求参数的示例 -->
<a href="account/removeAccount?accountName=aaa&money>100">删除账户，金额 100</a>
<br/>
<a href="account/removeAccount?accountName=aaa&money>150">删除账户，金额 150</a>
```

当我们点击第一个超链接时,可以访问成功。 

当我们点击第二个超链接时，无法访问。报400错误

# 3. 请求参数的绑定 

## 3.1. 绑定说明

### 3.1.1.  绑定的机制

我们都知道，表单中请求参数都是基于 key=value 的。 

SpringMVC 绑定请求参数的过程是通过把表单提交请求参数，作为控制器中方法参数进行绑定的

- ```xml
  <!-- 请求参数为： accountId=10 -->
  <a href="account/findAccount?accountId=10">查询账户</a>
  ```

- ```Java
  @RequestMapping("/findAccount")
  public String findAccount(Integer accountId) {
      System.out.println("查询了账户。。。。"+accountId);
      return "success";
  }
  ```

### 3.1.2. 支持的数据类型：

**基本类型参数**： 包括基本类型和 String 类型 

**POJO 类型参数**： 包括实体类，以及关联的实体类 

**数组和集合类型参数**： 包括 List 结构和 Map 结构的集合（包括数组）

 SpringMVC 绑定请求参数是自动实现的，但是要想使用，必须遵循使用要求。

### 3.1.3. 使用要求：

**如果是基本类型或者 String 类型**： 要求我们的参数名称必须和控制器中方法的形参名称保持一致。(严格区分大小写) 

**如果是 POJO 类型，或者它的关联对象**： 要求表单中参数名称和 POJO 类的属性名称保持一致。并且控制器方法的参数类型是 POJO 类型。 

**如果是集合类型,有两种方式**：

1. 第一种： 
   - 要求集合类型的请求参数必须在 POJO 中。在表单中请求参数名称要和 POJO 中集合属性名称相同。
   - 给 List 集合中的元素赋值，使用下标。 
   - 给 Map 集合中的元素赋值，使用键值对。 
2. 第二种： 接收的请求参数是 json 格式数据。需要借助一个注解实现。

> 【推荐】它还可以实现一些数据类型自动转换。内置转换器全都在：`org.springframework.core.convert.support` 包下。有：
>
> java.lang.Boolean -> java.lang.String : ObjectToStringConverter 
>
> java.lang.Character -> java.lang.Number : CharacterToNumberFactory 
>
> java.lang.Character -> java.lang.String : ObjectToStringConverter 
>
> java.lang.Enum -> java.lang.String : EnumToStringConverter 
>
> java.lang.Number -> java.lang.Character : NumberToCharacterConverter 
>
> java.lang.Number -> java.lang.Number : NumberToNumberConverterFactory 
>
> java.lang.Number -> java.lang.String : ObjectToStringConverter 
>
> java.lang.String -> java.lang.Boolean : StringToBooleanConverter 
>
> java.lang.String -> java.lang.Character : StringToCharacterConverter 
>
> java.lang.String -> java.lang.Enum : StringToEnumConverterFactory 
>
> java.lang.String -> java.lang.Number : StringToNumberConverterFactory 
>
> java.lang.String -> java.util.Locale : StringToLocaleConverter 
>
> java.lang.String -> java.util.Properties : StringToPropertiesConverter 
>
> java.lang.String -> java.util.UUID : StringToUUIDConverter 
>
> java.util.Locale -> java.lang.String : ObjectToStringConverter
>
> java.util.Properties -> java.lang.String : PropertiesToStringConverter 
>
> java.util.UUID -> java.lang.String : ObjectToStringConverter
>
>  ......
>
> 如遇特殊类型转换要求，需要我们自己编写自定义类型转换器。



### 3.1.4. 使用示例

#### ①. 基本类型和 String 类型作为参数

```xml
<a href="account/findAccount?accountId=10&accountName=zhangsan">查询账户</a>
```

```java
@RequestMapping("/findAccount")
public String findAccount(Integer accountId,String accountName) {
}
```

#### ②. POJO 类型作为参数

```java
public class Account implements Serializable {
    private Integer id;
    private String name;
    private Float money;
    private Address address; // 关联下面那个类
    //getters and setters
}

public class Address implements Serializable {
    private String provinceName;
    private String cityName;
    //getters and setters
}

```

```java
@RequestMapping("/saveAccount")
public String saveAccount(Account account) {
    System.out.println("保存了账户。。。。"+account);
    return "success";
}
```

```xml
<form action="account/saveAccount" method="post">
    账户名称：<input type="text" name="name" ><br/>
    账户金额：<input type="text" name="money" ><br/>
    账户省份：<input type="text" name="address.provinceName" ><br/>
    账户城市：<input type="text" name="address.cityName" ><br/>
    <input type="submit" value="保存">
</form>
```

#### ③. POJO 类中包含集合类型参数

```java
public class User implements Serializable {
    private String username;
    private String password;
    private Integer age;
    private List<Account> accounts;
    private Map<String,Account> accountMap;
}
```

```xml
<form action="account/updateAccount" method="post">
    用户名称：<input type="text" name="username" ><br/>
    用户密码：<input type="password" name="password" ><br/>
    用户年龄：<input type="text" name="age" ><br/>
    账户 1 名称：<input type="text" name="accounts[0].name" ><br/>
    账户 1 金额：<input type="text" name="accounts[0].money" ><br/>
    账户 2 名称：<input type="text" name="accounts[1].name" ><br/>
    账户 2 金额：<input type="text" name="accounts[1].money" ><br/>
    账户 3 名称：<input type="text" name="accountMap['one'].name" ><br/>
    账户 3 金额：<input type="text" name="accountMap['one'].money" ><br/>
    账户 4 名称：<input type="text" name="accountMap['two'].name" ><br/>
    账户 4 金额：<input type="text" name="accountMap['two'].money" ><br/>
    <input type="submit" value="保存">
</form>
```

### 3.1.5. 请求参数乱码问题

post 请求方式： 在 web.xml 中配置一个过滤器

- ```xml
  <!-- 配置 springMVC 编码过滤器 --> 
  <filter> 
      <filter-name>CharacterEncodingFilter</filter-name> 
      <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class> 
      <!-- 设置过滤器中的属性值 --> 
      <init-param> 
          <param-name>encoding</param-name> 
          <param-value>UTF-8</param-value> 
      </init-param> 
      <!-- 启动过滤器 --> 
      <init-param> 
          <param-name>forceEncoding</param-name> 
          <param-value>true</param-value> 
      </init-param> 
  </filter> 
  <!-- 过滤所有请求 --> 
  <filter-mapping> 
      <filter-name>CharacterEncodingFilter</filter-name> 
      <url-pattern>/*</url-pattern> 
  </filter-mapping>
  ```

在 springmvc 的配置文件中可以配置，静态资源不过滤：

- ```xml
  <!-- location 表示路径，mapping 表示文件，**表示该目录下的文件以及子目录的文件 -->
  <mvc:resources location="/css/" mapping="/css/**"/>
  <mvc:resources location="/images/" mapping="/images/**"/>
  <mvc:resources location="/scripts/" mapping="/javascript/**"/>
  ```

get 请求方式： tomacat 对 GET 和 POST 请求处理方式是不同的，GET 请求的编码问题，要改 tomcat 的 ==server.xml== 配置文件，如下：

- ```xml
  <Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/>
  改为：
  <Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443" useBodyEncodingForURI="true"/>
  ```

如果遇到 ajax 请求仍然乱码，请把： `useBodyEncodingForURI="true"`改为 `URIEncoding="UTF-8"` 即可。



## 3.2. 特殊情况 

### 3.2.1. 自定义类型转换器

将字符串日期2018-01-01 转化为为Date类型

```xml
<!-- 特殊情况之：类型转换问题 -->
<a href="account/deleteAccount?date=2018-01-01">根据日期删除账户</a>
```

**具体步骤如下**:

第一步：定义一个类，实现 Converter 接口，该接口有两个泛型。

- ```java
  public interface Converter<S, T> {//S:表示接受的类型，T：表示目标类型
      @Nullable
      T convert(S source);
  }
  /** 自定义类型转换器 */
  public class StringToDateConverter implements Converter<String, Date> {
      /** 用于把 String 类型转成日期类型 */
      public Date convert(String source) {
          DateFormat format = null;
          try {
              if(StringUtils.isEmpty(source)) {
                  throw new NullPointerException("请输入要转换的日期");
              }
              format = new SimpleDateFormat("yyyy-MM-dd");
              Date date = format.parse(source);
              return date;
          } catch (Exception e) {
              throw new RuntimeException("输入日期有误");
          }
      }
  }
  ```

第二步：在 spring 配置文件中配置类型转换器。

- ```xml
  <!-- 配置类型转换器工厂 -->
  <bean id="converterService" class="org.springframework.context.support.ConversionServiceFactoryBean">
      <!-- 给工厂注入一个新的类型转换器 -->
      <property name="converters">
          <array>
              <!-- 配置自定义类型转换器 -->
              <bean class="com.itheima.web.converter.StringToDateConverter"></bean>
          </array>
      </property>
  </bean>
  ```

第三步：在 `<annotation-driven>` 标签中引用配置的类型转换服务 

- ```xml
  <!-- 引用自定义类型转换器 -->
  <mvc:annotation-driven conversion-service="converterService"></mvc:annotation-driven>
  ```

### 3.2.2. 使用 ServletAPI 对象作为方法参数

SpringMVC 还支持使用原始 ServletAPI 对象作为控制器方法的参数。支持原始 ServletAPI 对象有： HttpServletRequest、HttpServletResponse、HttpSession、java.security.Principal、Locale、InputStream 、OutputStream、Reader、Writer

```Java
@RequestMapping("/testServletAPI")
public String testServletAPI(HttpServletRequest request,HttpServletResponse response,HttpSession session) {
    System.out.println(request);
    System.out.println(response);
    System.out.println(session);
    return "success";
}
```



# 4. 常用注解

## 4.1. RequestParam

**作用**： 把请求中指定名称的参数给控制器中的形参赋值。 

**属性**：

1.  `value`：请求参数中的名称。 
2. `required`：请求参数中是否必须提供此参数。默认值：true。表示必须提供，如果不提供将报错。

```java
@RequestMapping("/useRequestParam")
public String useRequestParam(@RequestParam("name")String username, @RequestParam(value="age",required=false)Integer age){
    System.out.println(username+","+age);
    return "success";
}
```

## 4.2. RequestBody

**作用**： 用于获取请求体内容。直接使用得到是 `key=value&key=value...`结构的数据。 get 请求方式不适用。 

**属性**： 

1. `required`：是否必须有请求体。默认值是:true。当取值为 true 时,get 请求方式会报错。如果取值 为 false，get 请求得到是 null。

```java
@RequestMapping("/useRequestBody")
public String useRequestBody(@RequestBody(required=false) String body){
    System.out.println(body); // key=value&key=value.. 有多少个请求参数， 就有几个
    return "success";
}
```

## 4.3. PathVaribale

**作用**： 用于绑定 url 中的占位符。例如：请求 url 中 `/delete/{id}`，这个=={id}就是 url 占位符==。 url 支持占位符是 spring3.0 之后加入的。是 springmvc 支持 rest 风格 URL 的一个重要标志。 

**属性**： 

1. `value`：用于指定 url 中占位符名称。 
2. `required`：是否必须提供占位符。

```java
// 访问地址：localhost:8080/xxxx/usePathVariable/199234234
@RequestMapping("/usePathVariable/{id}")
public String usePathVariable(@PathVariable("id") Integer id){
    System.out.println(id); // 输出：199234234
    return "success";
}
```

### 4.3.1. REST 风格 URL

**什么是 rest**： REST（英文：Representational State Transfer，简称 REST）描述了一个架构样式的网络系统， 比如 web 应用程序。它首次出现在 2000 年 Roy Fielding 的博士论文中，他是 HTTP 规范的主要编写者之 一。在目前主流的三种 Web 服务交互方案中，REST 相比于 SOAP（Simple Object Access protocol，简单 对象访问协议）以及 XML-RPC 更加简单明了，无论是对 URL 的处理还是对 Payload 的编码，REST 都倾向于用更 加简单轻量的方法设计和实现。值得注意的是 REST 并没有一个明确的标准，而更像是一种设计的风格。 它本身并没有什么实用性，其核心价值在于如何设计出符合 REST 风格的网络接口。 

**restful 的优点**： 它结构清晰、符合标准、易于理解、扩展方便，所以正得到越来越多网站的采用。

**restful 的特性**：

1. ==资源（Resources）==：网络上的一个实体，或者说是网络上的一个具体信息。 

   它可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的存在。可以用一个 URI（统一 资源定位符）指向它，每种资源对应一个特定的 URI 。要 获取这个资源，访问它的 URI 就可以，因此 ==URI 即为每一个资源的独一无二的识别符==。 

2. ==表现层（Representation）==：==把资源具体呈现出来的形式==，叫做它的表现层 （Representation）。 

   比如，文本可以用 txt 格式表现，也可以用 HTML 格式、XML 格式、JSON 格式表现，甚至可以采用二 进制格式。 

3. ==状态转化（State Transfer）==：每 发出一个请求，就代表了客户端和服务器的一次交互过程。 

   HTTP 协议，是一个无状态协议，即所有的状态都保存在服务器端。因此，如果客户端想要操作服务器， 必须通过某种手段，让服务器端发生“状态转化”（State Transfer）。而这种转化是建立在表现层之上的，所以 就是 “表现层状态转化”。具体说，==就是 HTTP 协议里面，四个表示操作方式的动词：GET 、POST 、PUT、 DELETE。它们分别对应四种基本操作：GET 用来获取资源，POST 用来新建资源，PUT 用来更新资源，DELETE 用来 删除资源。==

**restful 的示例**：
                    /account/1 HTTP GET ： 得到 id = 1 的 account  
                    /account/1 HTTP DELETE： 删除 id = 1 的 account  
                    /account/1 HTTP PUT： 更新 id = 1 的 account 
                    /account HTTP POST： 新增 account

### 4.3.1. 基于 HiddentHttpMethodFilter

**作用**：  由于浏览器 form 表单只支持 GET 与 POST 请求，而 DELETE、PUT 等 method 并不支持，Spring3.0 添 加了一个过滤器，可以将浏览器请求改为指定的请求方式，发送给我们的控制器方法，使得支持 GET、POST、PUT  与 DELETE 请求。 

**使用方法**： 
                      第一步：在 web.xml 中配置该过滤器。 
                      第二步：请求方式必须使用 post 请求。 
                      第三步：按照要求提供_method 请求参数，该参数的取值就是我们需要的请求方式。

```html
<!-- 删除 -->
<form action="springmvc/testRestDELETE/1" method="post">
    <input type="hidden" name="_method" value="DELETE">
    <input type="submit" value="删除">
</form>

<!-- 查询一个 -->
<form action="springmvc/testRestGET/1" method="post">
    <input type="hidden" name="_method" value="GET">
    <input type="submit" value="查询">
</form>
```

```java
/** post 请求：删除 */
@RequestMapping(value="/testRestDELETE/{id}",method=RequestMethod.DELETE)
public String testRestfulURLDELETE(@PathVariable("id")Integer id){
    System.out.println("rest delete "+id);
    return "success";
}
/** post 请求：查询 /
@RequestMapping(value="/testRestGET/{id}",method=RequestMethod.GET)
public String testRestfulURLGET(@PathVariable("id")Integer id){
    System.out.println("rest get "+id);
    return "success";
} 
```

## 4.4. RequestHeader

**作用**： 用于获取请求消息头。 

**属性**： 

1. `value`：提供消息头名称 
2. `required`：是否必须有此消息头 

> 【警告】 在实际开发中一般不怎么用。

```java
@RequestMapping("/useRequestHeader")
public String useRequestHeader(@RequestHeader(value="Accept-Language", required=false)String requestHeader){
    System.out.println(requestHeader); // 打印输出：zh-CN, zh; q=0.9
    return "success";
}
```

## 4.5. CookieValue

**作用**： 用于把指定 cookie 名称的值传入控制器方法参数。 

**属性**：

1. `value`：指定 cookie 的名称。 
2. `required`：是否必须有此 cookie。

```java
@RequestMapping("/useCookieValue")
public String useCookieValue(@CookieValue(value="JSESSIONID",required=false) String cookieValue){
    System.out.println(cookieValue);
    return "success";
}
```

## 4.6. ModelAttribute

**作用**： 该注解是 SpringMVC4.3 版本以后新加入的。它可以用于修饰方法和参数。 出现在方法上，表示当前方法会在控制器的方法执行之前，先执行。它可以修饰没有返回值的方法，也可以修饰有具体返回值的方法。 出现在参数上，获取指定的数据给参数赋值。 

**属性**： 

1. `value`：用于获取数据的 key。key 可以是 POJO 的属性名称，也可以是 map 结构的 key。 

应用场景： 当表单提交数据不是完整的实体类数据时，保证没有提交数据的字段使用数据库对象原来的数据。 

例如： 我们在编辑一个用户时，用户有一个创建信息字段，该字段的值是不允许被修改的。在提交表单数据是肯定没有此字段的内容，一旦更新会把该字段内容置为 null，此时就可以使用此注解解决问题。

```java
【运行结果】：先执行showModel方法，再执行testModelAttribute方法
/** 被 ModelAttribute 修饰的方法 */
@ModelAttribute
public void showModel(User user) {
System.out.println("执行了 showModel 方法"+user.getUsername());
}
/** 接收请求的方法 */
@RequestMapping("/testModelAttribute")
public String testModelAttribute(User user) {
System.out.println("执行了控制器的方法"+user.getUsername());
return "success";
}
```

## 4.7. SessionAttribute

**作用**： 用于多次执行控制器方法间的参数共享。

 **属性**： 

1. `value`：用于指定存入的属性名称 
2. `type`：用于指定存入的数据类型。

```java
@Controller("sessionAttributeController")
@RequestMapping("/springmvc")
@SessionAttributes(value ={"username","password"},types={Integer.class}) 
public class SessionAttributeController {
    /**
* 把数据存入 SessionAttribute
* @param model
* @return
* Model 是 spring 提供的一个接口，该接口有一个实现类 ExtendedModelMap
* 该类继承了 ModelMap，而 ModelMap 就是 LinkedHashMap 子类
*/
    @RequestMapping("/testPut") 
    public String testPut(Model model){ 
        model.addAttribute("username", "泰斯特"); 
        model.addAttribute("password","123456"); 
        model.addAttribute("age", 31); 
        //跳转之前将数据保存到 username、password 和 age 中，因为注解@SessionAttribute 中有这几个参数 
        return "success"; 
    } 

    @RequestMapping("/testGet") 
    public String testGet(ModelMap model){ 
        System.out.println(model.get("username")+";"+model.get("password")+";"+model.get("age")); 
        return "success"; 
    } 

    @RequestMapping("/testClean") 
    public String complete(SessionStatus sessionStatus){ 
        sessionStatus.setComplete(); 
        return "success"; 
    } 
}
```

# 5. 响应数据和结果视图

## 5.1. 返回值分类

### 5.1.1. 字符串

controller 方法返回字符串可以指定逻辑视图名，通过视图解析器解析为物理视图地址。

- ```java
  //指定逻辑视图名，经过视图解析器解析为 jsp 物理路径：/WEB-INF/pages/success.jsp
  @RequestMapping("/testReturnString")
  public String testReturnString() {
      System.out.println("AccountController 的 testReturnString 方法执行了。。。。");
      return "success";
  }
  ```

### 5.1.2. void

 Servlet 原始 API 可以作为控制器中方法的参数

- ```java
  @RequestMapping("/testReturnVoid")
  public void testReturnVoid(HttpServletRequest request,HttpServletResponse response) 
  throws Exception {
  }
  ```

在 controller 方法形参上可以定义 request 和 response，使用 request 或 response 指定响应结果：

1. 使用 request 转向页面，如下：

   ```Java
   request.getRequestDispatcher("/WEB-INF/pages/success.jsp").forward(request, response);
   ```

2. 也可以通过 response 页面重定向：

   ```java
   response.sendRedirect("testRetrunString")
   ```

3. 也可以通过 response 指定响应结果，例如响应 json 数据：

   ```Java
   response.setCharacterEncoding("utf-8");
   response.setContentType("application/json;charset=utf-8");
   response.getWriter().write("json 串");
   ```

### 5.1.3. ModelAndView

ModelAndView 是 SpringMVC 为我们提供的一个对象，该对象也可以用作控制器方法的返回值。

该对象中有两个方法：

1. `public ModelAndView addObject(String, Object)`：添加模型到对象。在JSP页面上可以使用EL表达式获取
2. `public void setViewName(String)`：用于设置逻辑视图的名称，视图解析器会根据名称前往指定的视图

## 5.2. 转发和重定向

### 5.2.1. forward 转发

controller 方法在提供了 String 类型的返回值之后，默认就是请求转发。我们也可以写成：

- ```Java
  @RequestMapping("/testForward")
  public String testForward() {
      System.out.println("AccountController 的 testForward 方法执行了。。。。");
      return "forward:/WEB-INF/pages/success.jsp";
  }
  ```

> 【危险】需要注意的是，如果用了 `formward：`则路径必须写成实际视图 url，不能写逻辑视图。

它相当于`“request.getRequestDispatcher("url").forward(request,response)”`。使用请求 转发，既可以转发到 jsp，也可以转发到其他的控制器方法。

### 5.2.2. Redirect 重定向

contrller 方法提供了一个 String 类型返回值之后，它需要在返回值里使用：`redirect:`

- ```Java
  @RequestMapping("/testRedirect")
  public String testRedirect() {
      System.out.println("AccountController 的 testRedirect 方法执行了。。。。");
      return "redirect:testReturnModelAndView";
  }
  ```

它相当于`“response.sendRedirect(url)”`。需要注意的是，如果是重定向到 jsp 页面，则 jsp 页面不 能写在 WEB-INF 目录中，否则无法找到。

## 5.3. ResponseBody 响应 json 数据

**作用**： 该注解用于将 Controller 的方法返回的对象，通过 HttpMessageConverter 接口转换为指定格式的 数据如：json,xml 等，通过 Response 响应给客户端

```java
@Controller("jsonController")
public class JsonController {
    @RequestMapping("/testResponseJson")
    public @ResponseBody Account testResponseJson(@RequestBody Account account) {
        System.out.println("异步请求："+account);
        return account;
    }
}
```

# 6. SpringMVC 实现文件上传

## 6.1. 文件上传的回顾

### 6.1.1. 文件上传的必要前提

1. form 表单的 `enctype` 取值必须是：`multipart/form-data` (默认值是:application/x-www-form-urlencoded) 

   `enctype`：是表单请求正文的类型

2. `method` 属性取值必须是 `Post` 

3. 提供一个文件选择域 `<input type=”file”>`

### 6.1.2. 文件上传的原理分析

当 form 表单的 enctype 取值不是默认值后，==request.getParameter()将失效==。 

enctype=”application/x-www-form-urlencoded”时，form 表单的正文内容是： `key=value&key=value&key=value` 

当 form 表单的 enctype 取值为 Mutilpart/form-data 时，请求正文内容就变成： 每一部分都是 MIME 类型描述的正文：

- ```js
  -----------------------------7de1a433602ac         分界符
  Content-Disposition: form-data; name="userName"    协议头
  aaa                                                协议的正文
  -----------------------------7de1a433602ac
  Content-Disposition: form-data; name="file"; 
  filename="C:\Users\zhy\Desktop\fileupload_demofile\b.txt"
  Content-Type: text/plain                            协议的类型（MIME 类型）
  bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
  -----------------------------7de1a433602ac--
  ```

### 6.1.3. 借助第三方组件实现文件上传

使用 Commons-fileupload 组件实现文件上传，需要导入该组件相应的支撑 jar 包：Commons-fileupload 和 commons-io。commons-io 不属于文件上传组件的开发 jar 文件，但Commons-fileupload 组件从 1.1 版本开始，它 工作时需要 commons-io 包的支持。



## 6.2. springmvc 传统方式的文件上传

传统方式的文件上传，指的是我们上传的文件和访问的应用存在于同一台服务器上。 并且上传完成之后，浏览器可能跳转。

第一步：jsp中设置

- ```xml
  <form action="/fileUpload" method="post" enctype="multipart/form-data">
      名称：<input type="text" name="picname"/><br/>
      图片：<input type="file" name="uploadFile"/><br/>
      <input type="submit" value="上传"/>
  </form>
  ```

第二步：controller设置

- ```Java
  @Controller("fileUploadController")
  public class FileUploadController {
      @RequestMapping("/fileUpload")
      public String testResponseJson(String picname,MultipartFile uploadFile,HttpServletRequest request) throws Exception{
          //定义文件名
          String fileName = "";
          //1.获取原始文件名
          String uploadFileName = uploadFile.getOriginalFilename();
          //2.截取文件扩展名
          String extendName = uploadFileName.substring(uploadFileName.lastIndexOf(".")+1, uploadFileName.length());
          //3.把文件加上随机数，防止文件重复
          String uuid = UUID.randomUUID().toString().replace("-", "").toUpperCase();
          //4.判断是否输入了文件名
          if(!StringUtils.isEmpty(picname)) {
              fileName = uuid+"_"+picname+"."+extendName;
          }else {
              fileName = uuid+"_"+uploadFileName;
          }
          System.out.println(fileName);
          //2.获取文件路径
          ServletContext context = request.getServletContext();
          String basePath = context.getRealPath("/uploads");
          //3.解决同一文件夹中文件过多问题
          String datePath = new SimpleDateFormat("yyyy-MM-dd").format(new Date());
          //4.判断路径是否存在
          File file = new File(basePath+"/"+datePath);
          if(!file.exists()) {
              file.mkdirs();
          }
          //5.使用 MulitpartFile 接口中方法，把上传的文件写到指定位置
          uploadFile.transferTo(new File(file,fileName));
          return "success";
      }
  }
  ```

第三步：配置文件解析器

- ```xml
  <!-- 配置文件上传解析器
   	      id 的值是固定的-->
  <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
      <!-- 设置上传文件的最大尺寸为 5MB -->
      <property name="maxUploadSize">
          <value>5242880</value>
      </property>
  </bean>
  ```

> 【危险】注意： 文件上传的解析器 id 是固定的，不能起别的名称，否则无法实现请求参数的绑定。（不光是文件，其他 字段也将无法绑定）

## 6.3. springmvc 跨服务器方式的文件上传

### 6.3.1. 分服务器的目的

在实际开发中，我们会有很多处理不同功能的服务器。例如： 

1. 应用服务器：负责部署我们的应用 
2. 数据库服务器：运行我们的数据库 
3. 缓存和消息服务器：负责处理大并发访问的缓存和消息 
4. 文件服务器：负责存储用户上传文件的服务器。

 ==(注意：此处说的不是服务器集群）==

分服务器处理的目的是让服务器各司其职，从而提高我们项目的运行效率。

![image-20210612140422391](images\01_Spring MVC\image-20210612140422391.png)



### 6.3.2. 具体步骤

第一步：在文件服务器的 tomcat 配置（==Tomcat安装路径下的web.xml==）中加入，允许读写操作。

- ```xml
  <!-- 加入此行的含义是：接收文件的目标服务器可以支持写入操作。 -->
  <servlet>
      <init-param>
          <param-name>readonly</param-name>
          <param-value>false</param-value>
      </init-param>
  </servlet>
  ```

第二步：编写Controller

- ```Java
  @Controller("fileUploadController2")
  public class FileUploadController2 {
      public static final String FILESERVERURL =  "http://localhost:9090/day06_spring_image/uploads/";
      
      @RequestMapping("/fileUpload2")
      public String testResponseJson(String picname,MultipartFile uploadFile) throws
          Exception{
          //定义文件名
          String fileName = "";
          //1.获取原始文件名
          String uploadFileName = uploadFile.getOriginalFilename();
          //2.截取文件扩展名
          String extendName =  uploadFileName.substring(uploadFileName.lastIndexOf(".")+1,  uploadFileName.length());
          //3.把文件加上随机数，防止文件重复
          String uuid = UUID.randomUUID().toString().replace("-", "").toUpperCase();
          //4.判断是否输入了文件名
          if(!StringUtils.isEmpty(picname)) {
              fileName = uuid+"_"+picname+"."+extendName;
          }else {
              fileName = uuid+"_"+uploadFileName;
          }
          System.out.println(fileName);
          //5.创建 sun 公司提供的 jersey 包中的 Client 对象
          Client client = Client.create();
          //6.指定上传文件的地址，该地址是 web 路径
          WebResource resource = client.resource(FILESERVERURL+fileName);
          //7.实现上传
          String result = resource.put(String.class,uploadFile.getBytes());
          System.out.println(result);
          return "success";
      }
  }
  ```

第三步：配置文件

- ```xml
  <!-- 配置文件上传解析器 -->
  <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
      <!-- 设置上传文件的最大尺寸为 5MB -->
      <property name="maxUploadSize">
          <value>5242880</value>
      </property>
  </bean>
  ```

# 7.  SpringMVC 中的异常处理 

## 7.1. 异常处理的思路

系统中异常包括两类：预期异常和运行时异常 RuntimeException，前者通过捕获异常从而获取异常信息， 后者主要通过规范代码开发、测试通过手段减少运行时异常的发生。 

系统的 dao、service、controller 出现都通过 throws Exception 向上抛出，最后由 springmvc 前端 控制器交由异常处理器进行异常处理，如下图：

![image-20210612141117073](images\01_Spring MVC\image-20210612141117073.png)

## 7.2. 步骤

第一步：编写自定义异常

- ```Java
  public class CustomException extends Exception {
      private String message;
      public CustomException(String message) {
          this.message = message;
      }
      public String getMessage() {
          return message;
      }
  }
  ```

第二步：编写显示异常的jsp

- ```jsp
  <body>
      执行失败！
      ${message }
  </body>
  ```

第三步：自定义异常处理器

- ```Java
  public class CustomExceptionResolver implements HandlerExceptionResolver {
      @Override
      public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
          ex.printStackTrace();
          CustomException customException = null;
          //如果抛出的是系统自定义异常则直接转换
          if(ex instanceof CustomException){
              customException = (CustomException)ex;
          }else{
              //如果抛出的不是系统自定义异常则重新构造一个系统错误异常。
              customException = new CustomException("系统错误，请与系统管理 员联系！");
          }
          ModelAndView modelAndView = new ModelAndView();
          modelAndView.addObject("message", customException.getMessage());
          modelAndView.setViewName("error");
          return modelAndView;
      }
  }
  ```

第四步：配置异常处理器

- ```xml
  <!-- 配置自定义异常处理器 -->
  <bean id="handlerExceptionResolver" class="com.itheima.exception.CustomExceptionResolver"/>
  ```

# 8. SpringMVC 中的拦截器

Spring MVC 的处理器拦截器类似于 Servlet 开发中的过滤器 Filter，用于对处理器进行预处理和后处理。 

用户可以自己定义一些拦截器来实现特定的功能。 

谈到拦截器，还要向大家提一个词——拦截器链（Interceptor Chain）。拦截器链就是将拦截器按一定的顺 序联结成一条链。在访问被拦截的方法或字段时，拦截器链中的拦截器就会按其之前定义的顺序被调用。 

说到这里，可能大家脑海中有了一个疑问，这不是我们之前学的过滤器吗？是的它和过滤器是有几分相似，但是也有区别，接下来我们就来说说他们的区别： 

1. 过滤器是 servlet 规范中的一部分，任何 java web 工程都可以使用。 拦截器是 SpringMVC 框架自己的，只有使用了 SpringMVC 框架的工程才能用。 
2. 过滤器在 url-pattern 中配置了/*之后，可以对所有要访问的资源拦截。 拦截器它是只会拦截访问的控制器方法，如果访问的是 jsp，html,css,image 或者 js 是不会进行拦 截的。 它也是 AOP 思想的具体应用。 我们要想自定义拦截器， 要求必须实现：`HandlerInterceptor` 接口。

## 8.1. 具体步骤：

第一步：编写拦截器

- ```Java
  public class HandlerInterceptorDemo1 implements HandlerInterceptor {
      @Override
      public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
          System.out.println("1preHandle 拦截器拦截了");
          return true;
      }
      @Override
      public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
          System.out.println("2postHandle 方法执行了");
      }
      @Override
      public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
          System.out.println("3afterCompletion 方法执行了");
      }
  }
  ```

第二步：配置拦截器

- ```xml
  <!-- 配置拦截器 -->
  <mvc:interceptors>
      <mvc:interceptor>
          <mvc:mapping path="/**"/>
          <bean id="handlerInterceptorDemo1" class="com.itheima.web.interceptor.HandlerInterceptorDemo1"></bean>
      </mvc:interceptor>
  </mvc:interceptors>
  ```

## 8.2. 拦截器的细节

### 8.2.1.  拦截器的放行

放行的含义是指，如果有下一个拦截器就执行下一个，如果该拦截器处于拦截器链的最后一个，则执行控制器 中的方法。

```Java
@Override
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    System.out.println("1preHandle 拦截器拦截了");
    return true; // 【注意】只有当此处返回true时，程序才会继续执行
}
```

### 8.2.2.  拦截器中方法的说明

```Java
public class HandlerInterceptorDemo1 implements HandlerInterceptor {
    /*
        【如何调用】：按拦截器定义顺序调用
        【何时调用】：只要配置了都会调用
        【有什么用】：
               1. 如果程序员决定该拦截器对请求进行拦截处理后还要调用其他的拦截器，或者是业务处理器去
               2. 进行处理，则返回 true。
               3. 如果程序员决定不需要再调用其他的组件去处理请求，则返回 false。
    */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("1preHandle 拦截器拦截了");
        return true;
    }
    /*
        【如何调用】：按拦截器定义逆序调用
        【何时调用】：在拦截器链内所有拦截器返成功调用
        【有什么用】：
               1. 在业务处理器处理完请求后，但是 DispatcherServlet 向客户端返回响应前被调用，
               2. 在该方法中对用户请求 request 进行处理。
    */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("2postHandle 方法执行了");
    }
    /*
        【如何调用】：按拦截器定义逆序调用
        【何时调用】：只有 preHandle 返回 true 才调用
        【有什么用】：
               1. 在 DispatcherServlet 完全处理完请求后被调用，
               2. 可以在该方法中进行一些资源清理的操作。
    */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("3afterCompletion 方法执行了");
    }
}
```

### 8.2.3.  拦截器的作用路径

作用路径可以通过在配置文件中配置。

```xml
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/**" /><!-- 用于指定对拦截的 url -->
        <mvc:exclude-mapping path=""/><!-- 用于指定排除的 url-->
        <bean id="handlerInterceptorDemo1" class="com.itheima.web.interceptor.HandlerInterceptorDemo1"></bean>
    </mvc:interceptor>
</mvc:interceptors>
```

### 8.2.4.  多个拦截器的执行顺序

多个拦截器是按照配置的顺序决定的。

例如：有两个拦截器（拦截器1和拦截器2），perHandle方法均为true，配置顺序为：拦截器1、拦截器2，则执行顺序为：

- ```
  拦截器1：perHandle执行了       
  拦截器2：perHandle执行了       
  控制器方法执行了
  拦截器2：PostHandle执行了
  拦截器1：PostHandle执行了
  拦截器2：afterCompletion执行了
  拦截器1：afterCompletion执行了
  ```

例如：有两个拦截器（拦截器1和拦截器2），拦截器1的perHandle方法为true，拦截器1的perHandle方法为false，配置顺序为：拦截器1、拦截器2，则执行顺序为：

- ```
  拦截器1：perHandle执行了       
  拦截器2：perHandle执行了       
  拦截器1：afterCompletion执行了
  ```

## 8.3. 验证用户登录

```java
public class LoginInterceptor implements HandlerInterceptor{
    @Override
    Public boolean preHandle(HttpServletRequest request,HttpServletResponse response, Object handler) throws Exception {
        //如果是登录页面则放行
        if(request.getRequestURI().indexOf("login.action")>=0){
            return true;
        }
        HttpSession session = request.getSession();
        //如果用户已登录也放行
        if(session.getAttribute("user")!=null){
            return true;
        }
        //用户没有登录挑战到登录页面
        request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request, response);
        return false;
    }
}
```






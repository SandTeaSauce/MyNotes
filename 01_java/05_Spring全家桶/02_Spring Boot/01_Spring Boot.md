# 1、了解SpringBoot

在这一部分，我们主要了解以下3个问题：

- 什么是SpringBoot

- 为什么要学习SpringBoot
- SpringBoot的特点

## 1.1、什么是SpringBoot

SpringBoot是Spring项目中的一个子工程，与我们所熟知的Spring-framework 同属于spring的产品:

![1567675019822](assets/1567675019822.png)

我们可以看到下面的一段介绍：

> Takes an opinionated view of building production-ready Spring applications.  Spring Boot favors convention over configuration and is designed to get you up and running as quickly as possible.
>
> 翻译一下：
>
> 用一些固定的方式来构建生产级别的spring应用。Spring Boot 推崇约定大于配置的方式以便于你能够尽可能快速的启动并运行程序。

其实人们把Spring Boot 称为搭建程序的脚手架。其最主要作用就是帮我们快速的构建庞大的spring项目，并且尽可能的减少一切xml配置，做到开箱即用，迅速上手，让我们关注与业务而非配置。

## 1.2、为什么要学习SpringBoot

java一直被人诟病的一点就是臃肿、麻烦。当我们还在辛苦的搭建项目时，可能Python程序员已经把功能写好了，究其原因注意是两点：

- **复杂的配置**
  项目各种配置其实是开发时的损耗， 因为在思考 Spring 特性配置和解决业务问题之间需要进行思维切换，所以写配置挤占了写应用程序逻辑的时间。

- **一个是混乱的依赖管理。**
  项目的依赖管理也是件吃力不讨好的事情。决定项目里要用哪些库就已经够让人头痛的了，你还要知道这些库的哪个版本和其他库不会有冲突，这难题实在太棘手。并且，依赖管理也是一种损耗，添加依赖不是写应用程序代码。一旦选错了依赖的版本，随之而来的不兼容问题毫无疑问会是生产力杀手。

而SpringBoot让这一切成为过去！

> Spring Boot 简化了基于Spring的应用开发，只需要“run”就能创建一个独立的、生产级别的Spring应用。Spring Boot为Spring平台及第三方库提供开箱即用的设置（提供默认设置，存放默认配置的包就是启动器），这样我们就可以简单的开始。多数Spring Boot应用只需要很少的Spring配置。

我们可以使用SpringBoot创建java应用，并使用java –jar 启动它，就能得到一个生产级别的web工程。

## 1.3、SpringBoot的特点

Spring Boot 主要目标是：

- 为所有 Spring 的开发者提供一个非常快速的、广泛接受的入门体验
- 开箱即用（启动器starter-其实就是SpringBoot提供的一个jar包），但通过自己设置参数（.properties），即可快速摆脱这种方式。
- 提供了一些大型项目中常见的非功能性特性，如内嵌服务器、安全、指标，健康检测、外部化配置等
- 绝对没有代码生成，也无需 XML 配置。

更多细节，大家可以到[官网](http://projects.spring.io/spring-boot/)查看。

 

# 2、快速入门

接下来，我们就来利用SpringBoot搭建一个web工程，体会一下SpringBoot的魅力所在！

## 2.1、创建工程

我们先新建一个空的工程：

![1567675243705](assets/1567675243705.png)

工程名为demo：

![1567675255730](assets/1567675255730.png)

新建一个model：

![1567675275102](assets/1567675275102.png)

使用maven来构建：

![1567675286199](assets/1567675286199.png)

然后填写项目坐标：

![1567675300752](assets/1567675300752.png)

目录结构：

![1567675310453](assets/1567675310453.png)

项目结构：

![1567675322139](assets/1567675322139.png)

## 2.2、添加依赖

看到这里很多同学会有疑惑，前面说传统开发的问题之一就是依赖管理混乱，怎么这里我们还需要管理依赖呢？难道SpringBoot不帮我们管理吗？

别着急，现在我们的项目与SpringBoot还没有什么关联。SpringBoot提供了一个名为spring-boot-starter-parent的工程，里面已经对各种常用依赖（并非全部）的版本进行了管理，我们的项目需要以这个项目为父工程，这样我们就不用操心依赖的版本问题了，需要什么依赖，直接引入坐标即可！

### 2.2.1、添加父工程坐标

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.4.RELEASE</version>
</parent>
```

### 2.2.2、添加web启动器

为了让SpringBoot帮我们完成各种自动配置，我们必须引入SpringBoot提供的自动配置依赖，我们称为启动器。因为我们是web项目，这里我们引入web启动器：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

需要注意的是，我们并没有在这里指定版本信息。因为SpringBoot的父工程已经对版本进行了管理了。

这个时候，我们会发现项目中多出了大量的依赖：

![1567675510838](assets/1567675510838.png)

这些都是SpringBoot根据spring-boot-starter-web这个依赖自动引入的，而且所有的版本都已经管理好，不会出现冲突。

### 2.2.3、管理jdk版本

默认情况下，maven工程的jdk版本是1.5，而我们开发使用的是1.8，因此这里我们需要修改jdk版本，只需要简单的添加以下属性即可：

```xml
<properties>
   <java.version>1.8</java.version>
</properties>
```

### 2.2.4、完整pom

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.leyou.demo</groupId>
    <artifactId>springboot-demo</artifactId>
    <version>1.0-SNAPSHOT</version>
	<!-- 设置maven支持jdk为1.8 -->
    <properties>
        <java.version>1.8</java.version>
    </properties>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.0.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
</project>	
```

## 2.3、启动类

Spring Boot项目通过main函数即可启动，我们需要创建一个启动类：

![1567675772868](assets/1567675772868.png)

然后编写main函数：

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```



## 2.4、编写controller

接下来，我们就可以像以前那样开发SpringMVC的项目了！

我们编写一个controller：

![1567675828445](assets/1567675828445.png)

```java
@RestController
public class HelloController {

    @GetMapping("hello")
    public String hello(){
        return "hello, spring boot!";
    }
}
```

 

## 2.5、启动测试

接下来，我们运行main函数，查看控制台：

![1567676023476](assets/1567676023476.png)

并且可以看到监听的端口信息：

![1567676048123](assets/1567676048123.png)

> 【说明】
>
> 1. 监听的端口是8080
> 2. SpringMVC的映射路径是：/
> 3. /hello路径已经映射到了HelloController中的hello()方法

打开页面访问：http://localhost:8080/hello

![1567676108591](assets/1567676108591.png)

测试成功了！

# 3、Java配置

在入门案例中，我们没有任何的配置，就可以实现一个SpringMVC的项目了，快速、高效！

但是有同学会有疑问，如果没有任何的xml，那么我们如果要配置一个Bean该怎么办？比如我们要配置一个数据库连接池，以前会这么玩：

```xml
<!-- 配置连接池 -->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
      init-method="init" destroy-method="close">
    <property name="url" value="${jdbc.url}" />
    <property name="username" value="${jdbc.username}" />
    <property name="password" value="${jdbc.password}" />
</bean>
```

## 3.1、回顾历史

事实上，在Spring3.0开始，Spring官方就已经开始推荐使用java配置来代替传统的xml配置了，我们不妨来回顾一下Spring的历史：

- Spring1.0时代

在此时因为jdk1.5刚刚出来，注解开发并未盛行，因此一切Spring配置都是xml格式，想象一下所有的bean都用xml配置，细思极恐啊，心疼那个时候的程序员2秒

- Spring2.0时代

Spring引入了注解开发，但是因为并不完善，因此并未完全替代xml，此时的程序员往往是把xml与注解进行结合，貌似我们之前都是这种方式。

- Spring3.0及以后

3.0以后Spring的注解已经非常完善了，因此Spring推荐大家使用完全的java配置来代替以前的xml，不过似乎在国内并未推广盛行。然后当SpringBoot来临，人们才慢慢认识到java配置的优雅。

有句古话说的好：拥抱变化，拥抱未来。所以我们也应该顺应时代潮流，做时尚的弄潮儿，一起来学习下java配置的玩法。

## 3.2、java原生配置

java配置主要靠java类和一些注解，比较常用的注解有：

- `@Configuration`：声明一个类作为配置类，代替xml文件
- `@Bean`：声明在方法上，将方法的返回值加入Bean容器，代替`<bean>`标签
- `@Value`：属性注入
- `@PropertySource`：指定外部属性文件，

我们接下来用java配置来尝试实现连接池配置：

首先引入Druid连接池依赖：

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.6</version>
</dependency>
```

创建一个jdbc.properties文件，编写jdbc属性：

```properties
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://127.0.0.1:3306/leyou
jdbc.username=root
jdbc.password=123
```

然后编写代码：

```java
@Configuration
@PropertySource("classpath:jdbc.properties")
public class JdbcConfig {

    @Value("${jdbc.url}")
    String url;
    @Value("${jdbc.driverClassName}")
    String driverClassName;
    @Value("${jdbc.username}")
    String username;
    @Value("${jdbc.password}")
    String password;

    @Bean
    public DataSource dataSource(){
        DruidDataSource dataSource = new DruidDataSource();

        dataSource.setUrl(url);
        dataSource.setDriverClassName(driverClassName);
        dataSource.setUsername(username);
        dataSource.setPassword(password);

        return dataSource;
    }
}
```

> 【说明】
>
> 1. `@Configuration`：声明我们JdbcConfig是一个配置类
> 2. `@PropertySource`：指定属性文件的路径是:`classpath:jdbc.properties`
> 3. 通过`@Value`为属性注入值
> 4. 通过`@Bean`将 dataSource()方法声明为一个注册Bean的方法，Spring会自动调用该方法，将方法的返回值加入Spring容器中。



然后我们就可以在任意位置通过`@Autowired`注入DataSource了！

我们在HelloController中测试：

```java
@RestController
public class HelloController {

    @Autowired
    private DataSource dataSource;

    @GetMapping("hello")
    public String hello() {
        return "hello, spring boot!" + dataSource;
    }
}
```

然后Debug运行并查看：

![1567677189753](assets/1567677189753.png)

属性注入成功了！

## 3.3、SpringBoot的配置

在 Spring Boot 中，配置文件有两种不同的格式，一个是 properties ，另一个是 yaml 。

### 3.3.1、配置文件位置问题

在 Spring Boot 中，一共有 4 个地方可以存放 application.properties 文件。

1. 当前项目根目录下的 config 目录下（1级）
2. 当前项目的根目录下（2级）
3. resources 目录下的 config 目录下（3级）
4. resources 目录下（4级）



按如上顺序，四个配置文件的优先级依次降低。如下：

![img](images\01_Spring Boot\11-1.png)



也可以在项目启动时自定义配置文件位置。在 resources 目录下创建一个 javaboy 目录下放置配置文件。通过 `spring.config.location` 属性来手动的指定配置文件位置

![img](images\01_Spring Boot\11-2-1623733612774.png)

### 3.3.1、文件名问题

对于 application.properties 而言，它不一定非要叫 application ，但是项目默认是去加载名为 application 的配置文件。创建一个 app.properties 文件，然后在 IDEA 中指定配置文件的文件名：

![img](images\01_Spring Boot\11-3.png)



### 3.3.3. properties配置

#### ①、类型安全的属性注入

在SpringBoot中，提供了一种新的属性注入方式，支持各种java基本数据类型及复杂类型的注入。

**1）我们新建一个类，用来进行属性注入：**

```java
@ConfigurationProperties(prefix = "jdbc") // 已jdbc. 打头的字符串
@Data // lombok 插件，就可以不用写get / set 方法, 在运行时由lombok插件动态生成
/* pom.xml 中加入lombok插件
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
*/
public class JdbcProperties {
    private String url;
    private String driverClassName;
    private String username;
    private String password;
    // ... 略
    // getters 和 setters
}
```

> 【说明】
>
> 1. 在类上通过`@ConfigurationProperties`注解声明当前类为属性读取类
> 2. `prefix="jdbc"`读取属性文件中，前缀为jdbc的值。
> 3. 在类上定义各个属性，名称必须与属性文件中jdbc.后面部分一致
> 4. 需要注意的是，这里我们并没有指定属性文件的地址，所以我们需要把jdbc.properties名称改为application.properties，这是SpringBoot默认读取的属性文件名：
>    ![1567685299811](F:\BaiduNetdiskWorkspace\06_Spring Boot\images\01_Spring Boot\1567685299811.png)



**2）在JdbcConfig中使用这个属性：**

```java
@Configuration
@EnableConfigurationProperties(JdbcProperties.class)
public class JdbcConfig {

    @Bean
    public DataSource dataSource(JdbcProperties jdbc) {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUrl(jdbc.getUrl());
        dataSource.setDriverClassName(jdbc.getDriverClassName());
        dataSource.setUsername(jdbc.getUsername());
        dataSource.setPassword(jdbc.getPassword());
        return dataSource;
    }
}
```

- 通过`@EnableConfigurationProperties(JdbcProperties.class)`来声明要使用`JdbcProperties`这个类的对象

- 然后你可以通过以下三种方式注入`JdbcProperties`：

```java
// [1], @Autowired注入
@Autowired
private JdbcProperties prop;
// [2], 构造函数注入
private JdbcProperties prop;
public JdbcConfig(Jdbcproperties prop){
    this.prop = prop;
}
// [3], 声明有@Bean的方法参数注入
@Bean
public Datasource dataSource(JdbcProperties prop){
    // ...
}
```

本例中，我们采用第三种方式。

**3）测试结果：**

![1567685636236](assets/1567685636236.png)

大家会觉得这种方式似乎更麻烦了，事实上这种方式有更强大的功能，也是SpringBoot推荐的注入方式。两者对比关系：

![1567685653658](assets/1567685653658.png)

**优势**：

- Relaxed binding：松散绑定
- 不严格要求属性文件中的属性名与成员变量名一致。支持驼峰，中划线，下划线等等转换，甚至支持对象引导。比如：user.friend.name：代表的是user对象中的friend属性中的name属性，显然friend也是对象。@value注解就难以完成这样的注入方式。
- meta-data support：元数据支持，帮助IDE生成属性提示（写开源框架会用到）。



#### ②、更优雅的注入

事实上，如果一段属性只有一个Bean需要使用，我们无需将其注入到一个类（JdbcProperties）中。而是直接在需要的地方声明即可：

```java
@Configuration
public class JdbcConfig {

    @Bean
    // 声明要注入的属性前缀，SpringBoot会自动把相关属性通过set方法注入到DataSource中
    @ConfigurationProperties(prefix = "jdbc")
    public DataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        return dataSource;
    }
}
```

我们直接把`@ConfigurationProperties(prefix = "jdbc")`声明在需要使用的`@Bean`的方法上，然后SpringBoot就会自动调用这个Bean（此处是DataSource）的set方法，然后完成注入。使用的前提是：==该类必须有对应属性的set方法！==

我们将jdbc的url改成：/heima，再次测试：

![1567685796362](assets/1567685796362.png)

### 3.3.4、Yaml配置文件

配置文件除了可以使用==application.properties==类型, 还可以使用后缀名为: ==.yml== 或者 ==.yaml==的类型, 也就是==application.yaml== 或者 ==application.yml==

> 正如YAML所表示的YAML. Ain't Markup Language, YAML是一种简洁的非标记语言, YAML已数据为中心, 使用空白缩进, 分行组织数据, 从而使得表示更加简洁易读

基本格式

```yml
jdbc:
 driverClassName: com.mysql.jdbc.Driver
 url: jdbc:mysql://127.0.0.1:3306/heima
 username: root
 password: 123456
```

# 4、自动配置原理

使用SpringBoot之后，一个整合了SpringMVC的WEB工程开发，变的无比简单，那些繁杂的配置都消失不见了，这是如何做到的？

一切魔力的开始，都是从我们的main函数来的，所以我们再次来看下启动类：

![1567686460449](assets/1567686460449.png)

我们发现特别的地方有两个：

-  注解：`@SpringBootApplication`

- run方法：`SpringApplication.run()`

我们分别来研究这两个部分。

## 4.1、了解@SpringBootApplication

点击进入，查看源码：

![1567686525306](assets/1567686525306.png)

这里重点的注解有3个：

- `@SpringBootConfiguration`

- `@EnableAutoConfiguration`

- `@ComponentScan`

### 4.1.1、@SpringBootConfiguration

我们继续点击查看源码：

![1567686565792](assets/1567686565792.png)

通过这段我们可以看出，在这个注解上面，又有一个@Configuration注解。通过上面的注释阅读我们知道：这个注解的作用就是声明当前类是一个配置类，然后Spring会自动扫描到添加了@Configuration的类，并且读取其中的配置信息。而@SpringBootConfiguration是来声明当前类是SpringBoot应用的配置类，项目中只能有一个。所以一般我们无需自己添加。

### 4.1.2、@EnableAutoConfiguration

关于这个注解，官网上有一段说明：

> The second class-level annotation is @EnableAutoConfiguration. This
> annotation tells Spring Boot to “guess” how you want to configure Spring, based
> on the jar dependencies that you have added. Since spring-boot-starter-web
> added Tomcat and Spring MVC, the auto-configuration assumes that you are
> developing a web application and sets up Spring accordingly.
>
> 简单翻译以下：
>
> 第二级的注解@EnableAutoConfiguration，告诉SpringBoot基于你所添加的依赖，去“猜测”你想要如何配置Spring。比如我们引入了spring-boot-starter-web，而这个启动器中帮我们添加了tomcat、SpringMVC的依赖。此时自动配置就知道你是要开发一个web应用，所以就帮你完成了web及SpringMVC的默认配置了！

### 4.1.3、@ComponentScan

我们跟进源码：

![1567686833673](assets/1567686833673.png)

并没有看到什么特殊的地方。我们查看注释：

![1567686845545](assets/1567686845545.png)

> 大概的意思：
>
> 配置组件扫描的指令。提供了类似与<context:component-scan>标签的作用
>
> 通过basePackageClasses或者basePackages属性来指定要扫描的包。如果没有指定这些属性，那么将从声明这个注解的类所在的包开始，扫描包及子包

而我们的@SpringBootApplication注解声明的类就是main函数所在的启动类，因此扫描的包是该类所在包及其子包。因此，==一般启动类会放在一个比较前的包目录中。==

## 4.2、默认配置原理

### 4.2.0、spring.factories

Spring默认配置(Maven:
org.springframework.boot:spring-boot-autoconfigure:2.0.4.RELEASE2)

![1567686921008](assets/1567686921008.png)

### 4.2.1、默认配置类

通过刚才的学习，我们知道@EnableAutoConfiguration会开启SpringBoot的自动配置，并且根据你引入的依赖来生效对应的默认配置。那么问题来了：

- 这些默认配置是在哪里定义的呢？

- 为何依赖引入就会触发配置呢？

其实在我们的项目中，已经引入了一个依赖：spring-boot-autoconfigure，其中定义了大量自动配置类：

![1567686977021](assets/1567686977021.png)

![1567686992670](assets/1567686992670.png)

非常多，几乎涵盖了现在主流的开源框架，例如：

- redis

- jms

- amqp

- jdbc

- jackson

- mongodb

- jpa

- solr

- elasticsearch

... 等等

我们来看一个我们熟悉的，例如SpringMVC，查看mvc 的自动配置类：

![1567687058378](assets/1567687058378.png)

我们看到这个类上的4个注解：

- `@Configuration`：声明这个类是一个配置类

- `@ConditionalOnWebApplication(type = Type.SERVLET)`：ConditionalOn，翻译就是在某个条件下，此处就是满足项目的类是是Type.SERVLET类型，也就是一个普通web工程，显然我们就是

- `@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })`：这里的条件是OnClass，也就是满足以下类存在：Servlet、DispatcherServlet、WebMvcConfigurer，其中Servlet只要引入了tomcat依赖自然会有，后两个需要引入SpringMVC才会有。这里就是判断你是否引入了相关依赖，引入依赖后该条件成立，当前类的配置才会生效！
- `@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)`：这个条件与上面不同，OnMissingBean，是说环境中没有指定的Bean这个才生效。其实这就是自定义配置的入口，也就是说，如果我们自己配置了一个WebMVCConfigurationSupport的类，那么这个默认配置就会失效！

接着，我们查看该类中定义了什么：

视图解析器：

![1567687167775](assets/1567687167775.png)

处理器适配器（HandlerAdapter）：

![1567687179415](assets/1567687179415.png)

还有很多，这里就不一一截图了。

### 4.2.2、默认配置属性

另外，这些默认配置的属性来自哪里呢？

![1567687194017](assets/1567687194017.png)

我们看到，这里通过`@EnableAutoConfiguration`注解引入了两个属性：WebMvcProperties和ResourceProperties。这不正是SpringBoot的属性注入玩法嘛。我们查看这两个属性类：

![1567687218499](assets/1567687218499.png)

找到了内部资源视图解析器的prefix和suffix属性。

ResourceProperties中主要定义了静态资源（.js,.html,.css等)的路径：

![1567756304686](assets/1567756304686.png)

如果我们要覆盖这些默认属性，只需要在application.properties中定义与其前缀prefix和字段名一致的属性即可。

## 4.3、总结

SpringBoot为我们提供了默认配置，而默认配置生效的条件一般有两个：

- 你引入了相关依赖

- 你自己没有配置

**1）启动器**

所以，我们如果不想配置，只需要引入依赖即可，而依赖版本我们也不用操心，因为只要引入了SpringBoot提供的stater（启动器），就会自动管理依赖及版本了。

因此，玩SpringBoot的第一件事情，就是找启动器，SpringBoot提供了大量的默认启动器，参考课前资料中提供的《SpringBoot启动器.txt》

2）全局配置

另外，SpringBoot的默认配置，都会读取默认属性，而这些属性可以通过自定义application.properties文件来进行覆盖。这样虽然使用的还是默认配置，但是配置中的值改成了我们自定义的。

因此，玩SpringBoot的第二件事情，就是通过application.properties来覆盖默认属性值，形成自定义配置。我们需要知道SpringBoot的默认属性key，非常多，参考课前资料提供的：《SpringBoot全局属性.md》

# 5、Thymeleaf快速入门

SpringBoot并不推荐使用jsp，但是支持一些模板引擎技术：

![1567757856836](assets/1567757856836.png)

以前大家用的比较多的是Freemarker，但是我们今天的主角是Thymeleaf！

## 5.1.为什么是Thymeleaf？

简单说， Thymeleaf 是一个跟 Velocity、FreeMarker 类似的模板引擎，它可以完全替代 JSP 。相较与其他的模板引擎，它有如下三个极吸引人的特点：

- 动静结合：Thymeleaf 在有网络和无网络的环境下皆可运行，即它可以让美工在浏览器查看页面的静态效果，也可以让程序员在服务器查看带数据的动态页面效果。这是由于它支持 html 原型，然后在 html 标签里增加额外的属性来达到模板+数据的展示方式。浏览器解释 html 时会忽略未定义的标签属性，所以 thymeleaf 的模板可以静态地运行；当有数据返回到页面时，Thymeleaf 标签会动态地替换掉静态内容，使页面动态显示。

- 开箱即用：它提供标准和spring标准两种方言，可以直接套用模板实现JSTL、 OGNL表达式效果，避免每天套模板、该jstl、改标签的困扰。同时开发人员也可以扩展和创建自定义的方言。

- 多方言支持：Thymeleaf 提供spring标准方言和一个与 SpringMVC 完美集成的可选模块，可以快速的实现表单绑定、属性编辑器、国际化等功能。

- 与SpringBoot完美整合，SpringBoot提供了Thymeleaf的默认配置，并且为Thymeleaf设置了视图解析器，我们可以像以前操作jsp一样来操作Thymeleaf。代码几乎没有任何区别，就是在模板语法上有区别。

接下来，我们就通过入门案例来体会Thymeleaf的魅力：

## 5.2、入门教程

**1）编写一个controller，返回一些用户数据，放入模型中，等会在页面渲染**

```java
@GetMapping("/all")
public String all(ModelMap model) {
    // 查询用户
    List<User> users = this.userService.queryAll();
    // 放入模型
    model.addAttribute("users", users);
    // 返回模板名称（就是classpath:/templates/目录下的html文件名）
    return "users";
}
```

**2）在pom.xml中引入thymeleaf依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

- SpringBoot会自动为Thymeleaf注册一个视图解析器：

  ![1567759361455](images\01_Spring Boot\1567759361455.png)

  与解析JSP的InternalViewResolver类似，Thymeleaf也会根据前缀和后缀来确定模板文件的位置：

  ![1567759374633](images\01_Spring Boot\1567759374633.png)

  - 默认前缀：`classpath:/templates/`
  - 默认后缀：.html

  所以如果我们返回视图：users，会指向到 classpath:/templates/users.html


**3）编写HTML页面**

- 根据上面的文档介绍，模板默认放在classpath下的templates文件夹，我们新建一个html文件放入其中：

  ![1567759404625](assets/1567759404625.png)

  编写html模板，渲染模型中的数据：

  ```html
  <!DOCTYPE html>
  <html xmlns:th="http://www.thymeleaf.org">
  <head>
      <meta charset="UTF-8">
      <title>首页</title>
      <style type="text/css">
          table {border-collapse: collapse; font-size: 14px; width: 80%; margin: auto}
          table, th, td {border: 1px solid darkslategray;padding: 10px}
      </style>
  </head>
  <body>
  <div style="text-align: center">
      <span style="color: darkslategray; font-size: 30px">欢迎光临！</span>
      <hr/>
      <table class="list">
          <tr>
              <th>id</th>
              <th>姓名</th>
              <th>用户名</th>
              <th>年龄</th>
              <th>性别</th>
              <th>生日</th>
              <th>备注</th>
          </tr>
          <tr th:each="user : ${users}">
              <td th:text="${user.id}">1</td>
              <td th:text="${user.name}">张三</td>
              <td th:text="${user.userName}">zhangsan</td>
              <td th:text="${user.age}">20</td>
              <td th:text="${user.sex} == 1 ? '男': '女'">男</td>
              <td th:text="${#dates.format(user.birthday, 'yyyy-MM-dd')}">1980-02-30</td>
              <td th:text="${user.note}">1</td>
          </tr>
      </table>
  </div>
  </body>
  </html>
  ```

> 【注意】把html 的名称空间，改成：`xmlns:th="http://www.thymeleaf.org"` 会有语法提示

**4）测试**

## 5.3、语法

上面使用了以下语法：

- `${}` ：这个类似与el表达式，但其实是ognl的语法，比el表达式更加强大
- `th-指令`：th-是利用了Html5中的自定义属性来实现的。如果不支持H5，可以用`data-th-`来代替
- `th:each`：类似于c:foreach 遍历集合，但是语法更加简洁
- `th:text`：声明标签中的文本
  - 例如`<td th-text='${user.id}'>1</td>`，如果user.id有值，会覆盖默认的1
  - 如果没有值，则会显示td中默认的1。这正是thymeleaf能够动静结合的原因，模板解析失败不影响页面的显示效果，因为会显示默认值！



## 5.4、模板缓存

Thymeleaf会在第一次对模板解析之后进行缓存，极大的提高了并发处理能力。但是这给我们开发带来了不便，修改页面后并不会立刻看到效果，我们开发阶段可以关掉缓存使用：

```properties
# 开发阶段关闭thymeleaf的模板缓存
spring.thymeleaf.cache=false
```

> 【注意】
>
>  在Idea中，我们需要在修改页面后按快捷键：Ctrl + Shift + F9 对项目进行rebuild才可以。
>
>  eclipse中没有测试过。
>
> 我们可以修改页面，测试一下。

# 6、web开发

## 6.1、静态资源配置

### 6.1.1、SpringBoot配置

现在，我们的项目是一个jar工程，那么就没有webapp，我们的静态资源该放哪里呢？

回顾我们上面看的源码，有一个叫做`ResourceProperties`的类，里面就定义了静态资源的默认查找路径：

![1567756531937](assets/1567756531937.png)

默认的静态资源路径为：

1. classpath:/META-INF/resources/
2. classpath:/resources/
3. classpath:/static/
4. classpath:/public

只要静态资源放在这些目录中任何一个，SpringMVC都会帮我们处理。

我们习惯会把静态资源放在classpath:/static/目录下。我们创建目录，并且添加一些静态资源：

![1567756573980](assets/1567756573980.png)

重启项目后测试：

![1567756613830](assets/1567756613830.png)



### 6.1.2、自定义配置

自定义的方式也有两种，可以通过 application.properties 来定义，也可以在 Java 代码中来定义，下面分别来看。

#### ①、properties配置

在 ==application.properties==配置文件中定义的方式比较简单，如下：

```properties
spring.resources.static-locations=classpath:/
spring.mvc.static-path-pattern=/**
```

> 【说明】
>
> 第一行配置：表示定义资源位置
>
> 第二行配置：表示定义请求 URL 规则。

#### ②、Java 代码定义

当然，在Spring Boot中我们也可以通过 Java代码来自定义，方式和 Java 配置的 SSM 比较类似，如下：

```java
@Configuration
public class WebMVCConfig implements WebMvcConfigurer {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/**").addResourceLocations("classpath:/aaa/");
    }
}
```



## 6.2、异常页面

### 6.2.1、静态异常页面

自定义静态异常页面，又分为两种，第一种 是使用 HTTP 响应码来命名页面，例如 404.html、405.html、500.html ….，另一种就是直接定义一个 4xx.html，表示400-499 的状态都显示这个异常页面，5xx.html 表示 500-599 的状态显示这个异常页面。

默认是在 ==classpath:/static/error/== 路径下定义相关页面：

![img](images\01_Spring Boot\4-2.png)

此时，启动项目，如果项目抛出 500 请求错误，就会自动展示 500.html 这个页面，发生 404 就会展示 404.html 页面。如果异常展示页面既存在 5xx.html，也存在 500.html ，此时，发生500异常时，优先展示 500.html 页面。

### 6.2.2、动态异常页面

动态的异常页面定义方式和静态的基本 一致，可以采用的页面模板有 jsp、freemarker、thymeleaf。动态异常页面，也支持 404.html 或者 4xx.html ，但是一般来说，由于动态异常页面可以直接展示异常详细信息，所以就没有必要挨个枚举错误了 ，直接定义 4xx.html（这里使用thymeleaf模板）或者 5xx.html 即可。

> 【注意】动态页面模板，不需要开发者自己去定义控制器，直接定义异常页面即可 ，Spring Boot 中自带的异常处理器会自动查找到异常页面。

页面定义如下：

![img](images\01_Spring Boot\4-3.png)

页面内容如下：

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>5xx</h1>
<table border="1">
    <tr>
        <td>path</td>
        <td th:text="${path}"></td>
    </tr>
    <tr>
        <td>error</td>
        <td th:text="${error}"></td>
    </tr>
    <tr>
        <td>message</td>
        <td th:text="${message}"></td>
    </tr>
    <tr>
        <td>timestamp</td>
        <td th:text="${timestamp}"></td>
    </tr>
    <tr>
        <td>status</td>
        <td th:text="${status}"></td>
    </tr>
</table>
</body>
</html>
```



如果动态页面和静态页面同时定义了异常处理页面，例如 ==classpath:/static/error/404.html== 和 ==classpath:/templates/error/404.html== 同时存在时，默认使用动态页面。即完整的错误页面查找方式应该是这样：发生了500错误 → 查找动态 500.html 页面 → 查找静态 500.html  →  查找动态 5xx.html → 查找静态 5xx.html。

### 6.2.3、自定义异常数据

默认情况下，在Spring Boot 中，所有的异常数据其实就是上文所展示出来的5条数据，这5条数据定义在 `org.springframework.boot.web.reactive.error.DefaultErrorAttributes` 类中，具体定义在 `getErrorAttributes` 方法中 ：

```java
@Override
public Map<String, Object> getErrorAttributes(ServerRequest request,
                boolean includeStackTrace) {
        Map<String, Object> errorAttributes = new LinkedHashMap<>();
        errorAttributes.put("timestamp", new Date());
        errorAttributes.put("path", request.path());
        Throwable error = getError(request);
        HttpStatus errorStatus = determineHttpStatus(error);
        errorAttributes.put("status", errorStatus.value());
        errorAttributes.put("error", errorStatus.getReasonPhrase());
        errorAttributes.put("message", determineMessage(error));
        handleException(errorAttributes, determineException(error), includeStackTrace);
        return errorAttributes;
}
```

DefaultErrorAttributes 类本身则是在org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration 异常自动配置类中定义的，如果开发者没有自己提供一个 ErrorAttributes 的实例的话，那么 Spring Boot 将自动提供一个ErrorAttributes 的实例，也就是 DefaultErrorAttributes 。

基于此 ，开发者自定义 ErrorAttributes 有两种方式 ：

1. 直接实现 ErrorAttributes 接口
2. 继承 DefaultErrorAttributes（推荐），因为 DefaultErrorAttributes 中对异常数据的处理已经完成，开发者可以直接使用。

```java
@Component
public class MyErrorAttributes  extends DefaultErrorAttributes {
    @Override
    public Map<String, Object> getErrorAttributes(WebRequest webRequest, boolean includeStackTrace) {
        Map<String, Object> map = super.getErrorAttributes(webRequest, includeStackTrace);
        if ((Integer)map.get("status") == 500) {
            map.put("message", "服务器内部错误!");
        }
        return map;
    }
}
```

定义好的 ErrorAttributes 一定要注册成一个 Bean ，这样，Spring Boot 就不会使用默认的 DefaultErrorAttributes 了，运行效果如下图：

![img](images\01_Spring Boot\4-5.png)

### 6.2.4、自定义异常视图

异常视图默认就是前面所说的静态或者动态页面，这个也是可以自定义的

首先 ，默认的异常视图加载逻辑在 `org.springframework.boot.autoconfigure.web.servlet.error.BasicErrorController` 类的 `errorHtml` 方法中，这个方法用来返回异常页面+数据，还有另外一个 error 方法，这个方法用来返回异常数据（如果是 ajax 请求，则该方法会被触发）。

```java
@RequestMapping(produces = MediaType.TEXT_HTML_VALUE)
public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
        HttpStatus status = getStatus(request);
        Map<String, Object> model = Collections.unmodifiableMap(getErrorAttributes( request, isIncludeStackTrace(request, MediaType.TEXT_HTML)));
        response.setStatus(status.value());
        ModelAndView modelAndView = resolveErrorView(request, response, status, model);
        return (modelAndView != null) ? modelAndView : new ModelAndView("error", model);
}
```

在该方法中 ，首先会通过 `getErrorAttributes` 方法去获取异常数据（实际上会调用到 ErrorAttributes 的实例 的 getErrorAttributes 方法），然后调用 resolveErrorView 去创建一个 ModelAndView ，如果这里创建失败，那么用户将会看到默认的错误提示页面。

正常情况下， resolveErrorView 方法会来到 DefaultErrorViewResolver 类的 resolveErrorView 方法中：

```java
@Override
public ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status,
                Map<String, Object> model) {
        ModelAndView modelAndView = resolve(String.valueOf(status.value()), model);
        if (modelAndView == null && SERIES_VIEWS.containsKey(status.series())) {
                modelAndView = resolve(SERIES_VIEWS.get(status.series()), model);
        }
        return modelAndView;
}
```

在这里，首先以异常响应码作为视图名分别去查找动态页面和静态页面，如果没有查找到，则再以 4xx 或者 5xx 作为视图名再去分别查找动态或者静态页面。

要自定义异常视图解析，也很容易 ，由于 DefaultErrorViewResolver 是在 ErrorMvcAutoConfiguration 类中提供的实例，即开发者没有提供相关实例时，会使用默认的 DefaultErrorViewResolver ，开发者提供了自己的 ErrorViewResolver 实例后，默认的配置就会失效，因此，自定义异常视图，只需要提供 一个 ErrorViewResolver 的实例即可：

```java
@Component
public class MyErrorViewResolver extends DefaultErrorViewResolver {
    public MyErrorViewResolver(ApplicationContext applicationContext, ResourceProperties resourceProperties) {
        super(applicationContext, resourceProperties);
    }
    @Override
    public ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status, Map<String, Object> model) {
        return new ModelAndView("/aaa/123", model);
    }
}
```

实际上，开发者也可以在这里定义异常数据（直接在 resolveErrorView 方法重新定义一个 model ，将参数中的model 数据拷贝过去并修改，注意参数中的 model 类型为 UnmodifiableMap，即不可以直接修改），而不需要自定义MyErrorAttributes。定义完成后，提供一个名为123的视图，如下图：

![img](F:\BaiduNetdiskWorkspace\06_Spring Boot\images\01_Spring Boot\4-6.png)

如此之后，错误试图就算定义成功了。

## 6.3、CORS解决跨域问题



### 6.3.1、同源策略

很多人对跨域有一种误解，以为这是前端的事，和后端没关系，其实不是这样的，说到跨域，就不得不说说浏览器的同源策略。

同源策略是由Netscape提出的一个著名的安全策略，它是浏览器最核心也最基本的安全功能，现在所有支持JavaScript的浏览器都会使用这个策略。所谓同源是指协议、域名以及端口要相同。同源策略是基于安全方面的考虑提出来的，这个策略本身没问题，但是我们在实际开发中，由于各种原因又经常有跨域的需求，传统的跨域方案是JSONP，JSONP虽然能解决跨域但是有一个很大的局限性，那就是只支持GET请求，不支持其他类型的请求，而今天我们说的CORS（跨域源资源共享）（CORS，Cross-origin resource sharing）是一个W3C标准，它是一份浏览器技术的规范，提供了Web服务从不同网域传来沙盒脚本的方法，以避开浏览器的同源策略，这是JSONP模式的现代版。

在Spring框架中，对于CORS也提供了相应的解决方案，今天我们就来看看SpringBoot中如何实现CORS。

### 6.3.2、测试

模拟：创建两个普通的SpringBoot项目，一个项目提供服务（编写Controller，端口8080），另一个项目消费服务（编写前端，端口8081）

在**提供服务的Controller**中写入请求地址相同，但是请求方式不同的两个方法

```Java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }
    @PostMapping("/hello")
    public String hello2() {
        return "post hello";
    }
}
```

在**消费服务的前端**中写入发送一个简单的ajax请求，如下：

```html
<div id="app"></div>
<input type="button" onclick="btnClick()" value="get_button">
<input type="button" onclick="btnClick2()" value="post_button">
<script>
    function btnClick() {
        $.get('http://localhost:8080/hello', function (msg) {
            $("#app").html(msg);
        });
    }

    function btnClick2() {
        $.post('http://localhost:8080/hello', function (msg) {
            $("#app").html(msg);
        });
    }
</script>
```

然后分别启动两个项目，发送请求按钮，观察浏览器控制台如下：

`Access to XMLHttpRequest at 'http://localhost:8080/hello' from origin 'http://localhost:8081' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.`

可以看到，由于同源策略的限制，请求无法发送成功。

### 6.3.3、CORS使用

使用CORS可以在前端代码不做任何修改的情况下，实现跨域，那么接下来看看在provider中如何配置。首先可以通过`@CrossOrigin`注解配置某一个方法接受某一个域的请求，如下：

```java
@RestController
public class HelloController {
    @CrossOrigin(value = "http://localhost:8081")
    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }

    @CrossOrigin(value = "http://localhost:8081")
    @PostMapping("/hello")
    public String hello2() {
        return "post hello";
    }
}
```

这个注解表示这两个接口接受来自http://localhost:8081地址的请求，配置完成后，重启**服务提供项目**，再次发送请求，浏览器控制台就不会报错了，consumer也能拿到数据了。

此时观察浏览器请求网络控制台，可以看到响应头中多了如下信息：
![img](F:\BaiduNetdiskWorkspace\06_Spring Boot\images\01_Spring Boot\w1-1.png)

这个表示服务端愿意接收来自http://localhost:8081的请求，拿到这个信息后，浏览器就不会再去限制本次请求的跨域了。



**服务提供项目**上，每一个方法上都去加注解未免太麻烦了，在Spring Boot中，还可以通过全局配置一次性解决这个问题，全局配置只需要在配置类中重写addCorsMappings方法即可，如下：

```Java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
        .allowedOrigins("http://localhost:8081")
        .allowedMethods("*")
        .allowedHeaders("*");
    }
}
```

> 【说明】
>
> ①、`/**`表示：本应用的所有方法都会去处理跨域请求
>
> ②、`allowedMethods`表示允许通过的请求数
>
> ③、`allowedHeaders`则表示允许的请求头
>
> 经过这样的配置之后，就不必在每个方法上单独配置跨域了。



### 6.3.4、存在的问题

了解了整个CORS的工作过程之后，我们通过Ajax发送跨域请求，虽然用户体验提高了，但是也有潜在的威胁存在，常见的就是CSRF（Cross-site request forgery）跨站请求伪造。跨站请求伪造也被称为one-click attack 或者 session riding，通常缩写为CSRF或者XSRF，是一种挟制用户在当前已登录的Web应用程序上执行非本意的操作的攻击方法，举个例子：

> 假如一家银行用以运行转账操作的URL地址如下：`http://icbc.com/aa?bb=cc`，那么，一个恶意攻击者可以在另一个网站上放置如下代码：`<img src="http://icbc.com/aa?bb=cc">`，如果用户访问了恶意站点，而她之前刚访问过银行不久，登录信息尚未过期，那么她就会遭受损失。

基于此，浏览器在实际操作中，会对请求进行分类，分为简单请求，预先请求，带凭证的请求等，预先请求会首先发送一个options探测请求，和浏览器进行协商是否接受请求。默认情况下跨域请求是不需要凭证的，但是服务端可以配置要求客户端提供凭证，这样就可以有效避免csrf攻击。



## 6.4、定义系统启动任务

在 Servlet/Jsp 项目中，如果涉及到系统任务，例如在项目启动阶段要做一些数据初始化操作，这些操作有一个共同的特点，只在项目启动时进行，以后都不再执行，这里，容易想到web基础中的三大组件（ Servlet、Filter、Listener ）之一 Listener ，这种情况下，一般定义一个 ServletContextListener，然后就可以监听到项目启动和销毁，进而做出相应的数据初始化和销毁操作，例如下面这样：



```java
public class MyListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        //在这里做数据初始化操作
    }
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        //在这里做数据备份操作
    }
}
```

当然，这是基础 web 项目的解决方案，如果使用了 Spring Boot，那么我们可以使用更为简便的方式。Spring Boot 中针对系统启动任务提供了两种解决方案，分别是 `CommandLineRunner` 和 `ApplicationRunner`，分别来看。



### 6.4.1、CommandLineRunner

使用 CommandLineRunner 时，首先自定义 MyCommandLineRunner1 并且实现 CommandLineRunner 接口：

```java
@Component
@Order(100)
public class MyCommandLineRunner1 implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
    }
}
```

> 【说明】
>
> ①、首先通过 `@Compoent` 注解将 MyCommandLineRunner1 注册为Spring容器中的一个 Bean。
>
> ②、@Order注解，表示这个启动任务的执行优先级，因为在一个项目中，启动任务可能有多个，所以需要有一个排序。@Order 注解中，数字越小，优先级越大，默认情况下，优先级的值为 Integer.MAX_VALUE，表示优先级最低。
>
> ③、在 run 方法中，写启动任务的核心逻辑，当项目启动时，run方法会被自动执行。
>
> ④、run 方法的参数，来自于项目的启动参数，即项目入口类中，main方法的参数会被传到这里。



此时启动项目，run方法就会被执行，至于参数，可以通过两种方式来传递，如果是在 IDEA 中，可以通过如下方式来配置参数：

![img](images\01_Spring Boot\3-1.png)

另一种方式，则是将项目打包，在命令行中启动项目，然后启动时在命令行传入参数，如下：

`java -jar devtools-0.0.1-SNAPSHOT.jar 三国演义 西游记`

> 【注意】这里参数传递时没有key，直接写value即可，执行结果如下：
>
> ![img](images\01_Spring Boot\3-2.png)



### 6.4.2、ApplicationRunner

ApplicationRunner 和 CommandLineRunner 功能一致，用法也基本一致，唯一的区别主要体现在对参数的处理上，ApplicationRunner 可以接收更多类型的参数（ApplicationRunner 除了可以接收 CommandLineRunner 的参数之外，还可以接收 key/value形式的参数）。

使用 ApplicationRunner ，自定义类实现 ApplicationRunner 接口即可，组件注册以及组件优先级的配置都和 CommandLineRunner 一致，如下：

```Java
@Component
@Order(98)
public class MyApplicationRunner1 implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        List<String> nonOptionArgs = args.getNonOptionArgs();
        System.out.println("MyApplicationRunner1>>>"+nonOptionArgs);
        Set<String> optionNames = args.getOptionNames();
        for (String key : optionNames) {
            System.out.println("MyApplicationRunner1>>>"+key + ":" + args.getOptionValues(key));
        }
        String[] sourceArgs = args.getSourceArgs();
        System.out.println("MyApplicationRunner1>>>"+Arrays.toString(sourceArgs));
    }
}
```

当项目启动时，这里的 run 方法就会被自动执行，关于 run 方法的参数 ApplicationArguments ，我说如下几点：

1. args.getNonOptionArgs();可以用来获取命令行中的无key参数（和CommandLineRunner一样）。
2. args.getOptionNames();可以用来获取所有key/value形式的参数的key。
3. args.getOptionValues(key));可以根据key获取key/value 形式的参数的value。
4. args.getSourceArgs(); 则表示获取命令行中的所有参数。

ApplicationRunner 定义完成后，传启动参数也是两种方式，参数类型也有两种，第一种和 CommandLineRunner 一致，第二种则是 –key=value 的形式，在 IDEA 中定义方式如下：

![img](images\01_Spring Boot\3-3.png)

或者使用 如下启动命令：

```
java -jar devtools-0.0.1-SNAPSHOT.jar 三国演义 西游记 --age=99
```

运行结果如下：

![img](images\01_Spring Boot\3-4.png)

## 6.5、定时任务

在 Spring + SpringMVC 环境中，一般来说，要实现定时任务，我们有两中方案，一种是使用 Spring 自带的定时任务处理器 `@Scheduled` 注解，另一种就是使用第三方框架 Quartz ，Spring Boot 源自 Spring+SpringMVC ，因此天然具备这两个 Spring 中的定时任务实现策略，当然也支持 Quartz，本文我们就来看下 Spring Boot 中两种定时任务的实现方式。



### 6.5.1、@Scheduled

#### ①、使用

使用 @Scheduled 非常容易，直接创建一个 Spring Boot 项目，并且添加 web 依赖 `spring-boot-starter-web`，项目创建成功后，添加 `@EnableScheduling` 注解，开启定时任务：

```Java
@SpringBootApplication
@EnableScheduling  // 启动定时任务
public class ScheduledApplication {
    public static void main(String[] args) {
        SpringApplication.run(ScheduledApplication.class, args);
    }
}
```

接下来配置定时任务：

```java
@Scheduled(fixedRate = 2000)
public void fixedRate() {
    System.out.println("fixedRate>>>"+new Date());    
}
@Scheduled(fixedDelay = 2000)
public void fixedDelay() {
    System.out.println("fixedDelay>>>"+new Date());
}
@Scheduled(initialDelay = 2000,fixedDelay = 2000)
public void initialDelay() {
    System.out.println("initialDelay>>>"+new Date());
}
```

> 【说明】
>
> ①、首先使用 `@Scheduled` 注解开启一个定时任务。
>
> ②、`fixedRate` 表示任务执行之间的时间间隔，具体是指两次任务的开始时间间隔，即第二次任务开始时，第一次任务可能还没结束。
>
> ③、`fixedRate` 表示任务执行之间的时间间隔，具体是指两次任务的开始时间间隔，即第二次任务开始时，第一次任务可能还没结束。
>
> ④、`initialDelay` 表示首次任务启动的延迟时间。
>
> ⑤、所有时间的单位都是毫秒。

#### ②、cron表达式

上面这是一个基本用法，除了这几个基本属性之外，`@Scheduled` 注解也支持 ==cron 表达式==，使用 cron 表达式，可以非常丰富的描述定时任务的时间。cron 表达式格式如下：`[秒] [分] [小时] [日] [月] [周] [年]`

具体取值如下：

| 序号 | 说明 | 是否必填 | 允许填写的值    | 允许的通配符 |
| :--- | :--- | :------- | :-------------- | :----------- |
| 1    | 秒   | 是       | 0-59            | - * /        |
| 2    | 分   | 是       | 0-59            | - * /        |
| 3    | 时   | 是       | 0-23            | - * /        |
| 4    | 日   | 是       | 1-31            | - * ? / L W  |
| 5    | 月   | 是       | 1-12 or JAN-DEC | - * /        |
| 6    | 周   | 是       | 1-7 or SUN-SAT  | - * ? / L #  |
| 7    | 年   | 否       | 1970-2099       | - * /        |

**这一块需要大家注意的是，月份中的日期和星期可能会起冲突，因此在配置时这两个得有一个是 `?`**

**通配符含义：**

1. `?` 表示不指定值，即不关心某个字段的取值时使用。需要注意的是，月份中的日期和星期可能会起冲突，因此在配置时这两个得有一个是 `?`
2. `*` 表示所有值，例如:在秒的字段上设置 `*`,表示每一秒都会触发
3. `,` 用来分开多个值，例如在周字段上设置 “MON,WED,FRI” 表示周一，周三和周五触发
4. `-` 表示区间，例如在秒上设置 “10-12”,表示 10,11,12秒都会触发
5. `/` 用于递增触发，如在秒上面设置”5/15” 表示从5秒开始，每增15秒触发(5,20,35,50)
6. `#` 序号(表示每月的第几个周几)，例如在周字段上设置”6#3”表示在每月的第三个周六，(用 在母亲节和父亲节再合适不过了)
7. 周字段的设置，若使用英文字母是不区分大小写的 ，即 MON 与mon相同
8. `L` 表示最后的意思。在日字段设置上，表示当月的最后一天(依据当前月份，如果是二月还会自动判断是否是润年), 在周字段上表示星期六，相当于”7”或”SAT”（注意周日算是第一天）。如果在”L”前加上数字，则表示该数据的最后一个。例如在周字段上设置”6L”这样的格式,则表示”本月最后一个星期五”
9. `W` 表示离指定日期的最近工作日(周一至周五)，例如在日字段上设置”15W”，表示离每月15号最近的那个工作日触发。如果15号正好是周六，则找最近的周五(14号)触发, 如果15号是周未，则找最近的下周一(16号)触发，如果15号正好在工作日(周一至周五)，则就在该天触发。如果指定格式为 “1W”,它则表示每月1号往后最近的工作日触发。如果1号正是周六，则将在3号下周一触发。(注，”W”前只能设置具体的数字,不允许区间”-“)
10. `L` 和 `W` 可以一组合使用。如果在日字段上设置”LW”,则表示在本月的最后一个工作日触发(一般指发工资 )

例如，在 @Scheduled 注解中来一个简单的 cron 表达式，每隔5秒触发一次，如下：

```java
@Scheduled(cron = "0/5 * * * * *")
public void cron() {
    System.out.println(new Date());
}
```

### 6.5.2、Quartz

#### ①、添加Quartz依赖

一般在项目中，除非定时任务涉及到的业务实在是太简单，使用 @Scheduled 注解来解决定时任务，否则大部分情况可能都是使用 Quartz 来做定时任务。在 Spring Boot 中使用 Quartz ，只需要在创建项目时，添加 Quartz 依赖即可：

![img](images\01_Spring Boot\6-1.png)

项目创建完成后，也需要添加开启定时任务的注解：

```Java
@SpringBootApplication
@EnableScheduling
public class QuartzApplication {
    public static void main(String[] args) {
        SpringApplication.run(QuartzApplication.class, args);
    }
}
```

#### ②、使用Quartz

Quartz 在使用过程中，有两个关键概念，一个是JobDetail（要做的事情），另一个是触发器（什么时候做），要定义 JobDetail，需要先定义 Job，继承 QuartzJobBean 并实现默认的方法：

```Java
public class MyJob2 extends QuartzJobBean {
    HelloService helloService;
    public HelloService getHelloService() {
        return helloService;
    }
    public void setHelloService(HelloService helloService) {
        this.helloService = helloService;
    }
    @Override
    protected void executeInternal(JobExecutionContext jobExecutionContext) throws JobExecutionException {
        helloService.sayHello();
    }
}
public class HelloService {
    public void sayHello() {
        System.out.println("hello service >>>"+new Date());
    }
}
```

Job 有了之后，接下来创建类，配置 JobDetail 和 Trigger 触发器，如下：

```Java
@Configuration
public class QuartzConfig {
    @Bean
    MethodInvokingJobDetailFactoryBean methodInvokingJobDetailFactoryBean() {
        MethodInvokingJobDetailFactoryBean bean = new MethodInvokingJobDetailFactoryBean();
        bean.setTargetBeanName("myJob1");
        bean.setTargetMethod("sayHello");
        return bean;
    }
    @Bean//使用 JobDetailFactoryBean 可以配置 JobDetail ，任务类继承自 QuartzJobBean ，这种方式支持传参，将参数封装在 JobDataMap 中进行传递。
    JobDetailFactoryBean jobDetailFactoryBean() {
        JobDetailFactoryBean bean = new JobDetailFactoryBean();
        bean.setJobClass(MyJob2.class);
        JobDataMap map = new JobDataMap();
        map.put("helloService", helloService());
        bean.setJobDataMap(map);
        return bean;
    }
    @Bean // SimpleTrigger 有点类似于前面说的 @Scheduled 的基本用法。
    SimpleTriggerFactoryBean simpleTriggerFactoryBean() {
        SimpleTriggerFactoryBean bean = new SimpleTriggerFactoryBean();
        bean.setStartTime(new Date());
        bean.setRepeatCount(5);
        bean.setJobDetail(methodInvokingJobDetailFactoryBean().getObject());
        bean.setRepeatInterval(3000);
        return bean;
    }
    @Bean // CronTrigger 则有点类似于 @Scheduled 中 cron 表达式的用法。
    CronTriggerFactoryBean cronTrigger() {
        CronTriggerFactoryBean bean = new CronTriggerFactoryBean();
        bean.setCronExpression("0/10 * * * * ?");
        bean.setJobDetail(jobDetailFactoryBean().getObject());
        return bean;
    }
    @Bean
    SchedulerFactoryBean schedulerFactoryBean() {
        SchedulerFactoryBean bean = new SchedulerFactoryBean();
        bean.setTriggers(cronTrigger().getObject(), simpleTriggerFactoryBean().getObject());
        return bean;
    }
    @Bean
    HelloService helloService() {
        return new HelloService();
    }
}
```

> 【说明】
>
> ①、JobDetail 的配置有两种方式：`MethodInvokingJobDetailFactoryBean` 和 `JobDetailFactoryBean` 
>
> ②、使用 `MethodInvokingJobDetailFactoryBean` 可以配置目标 Bean 的名字和目标方法的名字，这种方式==不支持传参==
>
> ③、使用 JobDetailFactoryBean 可以配置 JobDetail ，任务类继承自 QuartzJobBean ，这种方式支持传参，将参数封装在 JobDataMap 中进行传递
>
> ④、`Trigger` 是指触发器，Quartz 中定义了多个触发器，这里向大家展示其中两种的用法，SimpleTrigger 和 CronTrigger 。
>
> ⑤、`SimpleTrigger` 有点类似于前面说的 `@Scheduled` 的基本用法。
>
> ⑥、`CronTrigger` 则有点类似于 `@Scheduled` 中 cron 表达式的用法。

全部定义完成后，启动 Spring Boot 项目就可以看到定时任务的执行了。



## 6.6、接口文档——Swagger2

前后端分离后，维护接口文档基本上是必不可少的工作。一个理想的状态是设计好后，接口文档发给前端和后端，大伙按照既定的规则各自开发，开发好了对接上了就可以上线了。当然这是一种非常理想的状态，实际开发中却很少遇到这样的情况，接口总是在不断的变化之中，有变化就要去维护，做过的小伙伴都知道这件事有多么头大！还好，有一些工具可以减轻我们的工作量，Swagger2就是其中之一，至于其他类似功能但是却收费的软件，这里就不做过多介绍了。本文主要和大伙来聊下在Spring Boot中如何整合Swagger2。

### 6.6.1、快速入门

#### ①、工程创建

当然，首先是创建一个Spring Boot项目，加入web依赖，创建成功后，加入两个Swagger2相关的依赖，完整的依赖如下：

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

#### ②、Swagger2配置

Swagger2的配置也是比较容易的，在项目创建成功之后，只需要开发者自己提供一个Docket的Bean即可，如下：

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .pathMapping("/")
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.nvn.controller"))
                .paths(PathSelectors.any())
                .build().apiInfo(new ApiInfoBuilder()
                        .title("SpringBoot整合Swagger")
                        .description("SpringBoot整合Swagger，详细信息......")
                        .version("9.0")
                        .contact(new Contact("啊啊啊啊","blog.csdn.net","aaa@gmail.com"))
                        .license("The Apache License")
                        .licenseUrl("http://www.baidu.com")
                        .build());
    }
}
```

这里提供一个配置类，首先通过`@EnableSwagger2`注解启用Swagger2，然后配置一个Docket Bean，这个Bean中，配置映射路径和要扫描的接口的位置，在apiInfo中，主要配置一下Swagger2文档网站的信息，例如网站的title，网站的描述，联系人的信息，使用的协议等等。

如此，Swagger2就算配置成功了，非常方便。

此时启动项目，输入http://localhost:8080/swagger-ui.html，能够看到如下页面，说明已经配置成功了

#### ③、应用

接下来就是创建接口了，Swagger2相关的注解其实并不多，而且很容易懂，下面我来分别向小伙伴们举例说明：

```Java
@RestController
@Api(tags = "用户管理相关接口")
@RequestMapping("/user")
public class UserController {

    @PostMapping("/")
    @ApiOperation("添加用户的接口")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "username", value = "用户名", defaultValue = "李四"),
            @ApiImplicitParam(name = "address", value = "用户地址", defaultValue = "深圳", required = true)
    }
    )
    public RespBean addUser(String username, @RequestParam(required = true) String address) {
        return new RespBean();
    }

    @GetMapping("/")
    @ApiOperation("根据id查询用户的接口")
    @ApiImplicitParam(name = "id", value = "用户id", defaultValue = "99", required = true)
    public User getUserById(@PathVariable Integer id) {
        User user = new User();
        user.setId(id);
        return user;
    }
    @PutMapping("/{id}")
    @ApiOperation("根据id更新用户的接口")
    public User updateUserById(@RequestBody User user) {
        return user;
    }
}
```

> 【说明】
>
> ①、`@Api`注解可以用来标记当前Controller的功能。
>
> ②、`@ApiOperation`注解用来标记一个方法的作用。
>
> ③、`@ApiImplicitParam`注解用来描述一个参数，可以配置参数的中文含义，也可以给参数设置默认值，这样在接口测试的时候可以避免手动输入。
>
> ​         如果有多个参数，则需要使用多个`@ApiImplicitParam`注解来描述，多个`@ApiImplicitParam`注解需要放在一个`@ApiImplicitParams`注解中。
>
> > 【注意】需要注意的是，`@ApiImplicitParam`注解中虽然可以指定参数是必填的，但是却不能代替@`RequestParam(required = true)`，前者的必填只是在Swagger2框架内必填，抛弃了Swagger2，这个限制就没用了，所以假如开发者需要指定一个参数必填，`@RequestParam(required = true)`注解还是不能省略。
>
> ④、如果参数是一个对象（例如上文的更新接口），对于参数的描述也可以放在实体类中。例如下面一段代码：
>
> ```java 
> @ApiModel
> public class User {
>     @ApiModelProperty(value = "用户id")
>     private Integer id;
>     @ApiModelProperty(value = "用户名")
>     private String username;
>     @ApiModelProperty(value = "用户地址")
>     private String address;
>     //getter/setter
> }
> ```



#### ④、运行

好了，经过如上配置之后，接下来，刷新刚刚打开的页面，可以看到如下效果：

![img](images\01_Spring Boot\12-2.png)

可以看到，所有的接口这里都列出来了，包括接口请求方式，接口地址以及接口的名字等，点开一个接口，可以看到如下信息：

![img](images\01_Spring Boot\12-3.png)

可以看到，接口的参数，参数要求，参数默认值等等统统都展示出来了，参数类型下的query表示参数以key/value的形式传递，点击右上角的Try it out，就可以进行接口测试：

![img](images\01_Spring Boot\12-4.png)

点击Execute按钮，表示发送请求进行测试。测试结果会展示在下面的Response中。

小伙伴们注意，参数类型下面的query表示参数以key/value的形式传递，这里的值也可能是body，body表示参数以请求体的方式传递，例如上文的更新接口，如下：

![img](images\01_Spring Boot\12-5.png)

当然还有一种可能就是这里的参数为path，表示参数放在路径中传递，例如根据id查询用户的接口：

![img](images\01_Spring Boot\12-6.png)

当然，除了这些之外，还有一些响应值的注解，都比较简单，小伙伴可以自己摸索下。

### 6.6.2、在Security中的配置

如果我们的Spring Boot项目中集成了Spring Security，那么如果不做额外配置，Swagger2文档可能会被拦截，此时只需要在Spring Security的配置类中重写configure方法，添加如下过滤即可：

```java
@Override
public void configure(WebSecurity web) throws Exception {
    web.ignoring()
            .antMatchers("/swagger-ui.html")
            .antMatchers("/v2/**")
            .antMatchers("/swagger-resources/**");
}
```

如此之后，Swagger2文件就不需要认证就能访问了。



## 6.7、SpringMVC配置

用过 Spring Boot 的小伙伴都知道，我们只需要在项目中引入 `spring-boot-starter-web` 依赖，SpringMVC 的一整套东西就会自动给我们配置好，但是，真实的项目环境比较复杂，系统自带的配置不一定满足我们的需求，往往我们还需要结合实际情况自定义配置。

自定义配置就有讲究了，由于 Spring Boot 的版本变迁，加上这一块本身就有几个不同写法，很多小伙伴在这里容易搞混，今天松哥就来和大家说一说这个问题。

### 6.7.1、概览

首先我们需要明确，跟自定义 SpringMVC 相关的类和注解主要有如下四个：

- WebMvcConfigurerAdapter
- WebMvcConfigurer
- WebMvcConfigurationSupport
- @EnableWebMvc

这四个中，除了第四个是注解，另外三个两个类一个接口，里边的方法看起来好像都类似，但是实际使用效果却大不相同，

### 6.7.2、WebMvcConfigurerAdapter

我们先来看 `WebMvcConfigurerAdapter`，这个是在 Spring Boot 1.x 中我们自定义 SpringMVC 时继承的一个抽象类，这个抽象类本身是实现了 WebMvcConfigurer 接口，然后抽象类里边都是空方法，我们来看一下这个类的声明：

```java
public abstract class WebMvcConfigurerAdapter implements WebMvcConfigurer {
    //各种 SpringMVC 配置的方法
}
```

再来看看这个类的注释：

```
/**
 * An implementation of {@link WebMvcConfigurer} with empty methods allowing
 * subclasses to override only the methods they're interested in.
 * @deprecated as of 5.0 {@link WebMvcConfigurer} has default methods (made
 * possible by a Java 8 baseline) and can be implemented directly without the
 * need for this adapter
 */
```

这段注释关于这个类说的很明白了。同时我们也看到，从 Spring5 开始，由于我们要使用 Java8，而 Java8 中的接口允许存在 default 方法，因此官方建议我们直接实现 WebMvcConfigurer 接口，而不是继承 WebMvcConfigurerAdapter 。

==也就是说，在 Spring Boot 1.x 的时代，如果我们需要自定义 SpringMVC 配置，直接继承 WebMvcConfigurerAdapter 类即可。==

### 6.7.3、WebMvcConfigurer

`WebMvcConfigurer` 是我们在 Spring Boot 2.x 中实现自定义配置的方案。

`WebMvcConfigurer` 是一个接口，接口中的方法和 WebMvcConfigurerAdapter 中定义的空方法其实一样，所以用法上来说，基本上没有差别，从 Spring Boot 1.x 切换到 Spring Boot 2.x ，只需要把继承类改成实现接口即可。

### 6.7.4、WebMvcConfigurationSupport

在 WebMvcConfigurationSupport 类中，提供了用 Java 配置 SpringMVC 所需要的所有方法。我们来看一下这个方法的摘要：

![img](images\01_Spring Boot\37-1.png)

在这里首先大家需要明确的是，WebMvcConfigurationSupport 类本身是没有问题的，我们自定义 SpringMVC 的配置是可以通过继承 WebMvcConfigurationSupport 来实现的。但是继承 WebMvcConfigurationSupport 这种操作我们一般只在 Java 配置的 SSM 项目中使用，Spring Boot 中基本上不会这么写，为什么呢？

小伙伴们知道，Spring Boot 中，SpringMVC 相关的自动化配置是在 WebMvcAutoConfiguration 配置类中实现的，那么我们来看看这个配置类的生效条件：

```java
@Configuration
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
		ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {
}
```

我们从这个类的注解中可以看到，它的生效条件有一条，就是当不存在 WebMvcConfigurationSupport 的实例时，这个自动化配置才会生生效。因此，如果我们在 Spring Boot 中自定义 SpringMVC 配置时选择了继承 WebMvcConfigurationSupport，就会导致 Spring Boot 中 SpringMVC 的自动化配置失效。

**Spring Boot 给我们提供了很多自动化配置，很多时候当我们修改这些配置的时候，并不是要全盘否定 Spring Boot 提供的自动化配置，我们可能只是针对某一个配置做出修改，其他的配置还是按照 Spring Boot 默认的自动化配置来，而继承 WebMvcConfigurationSupport 来实现对 SpringMVC 的配置会导致所有的 SpringMVC 自动化配置失效，因此，一般情况下我们不选择这种方案。**

在 Java 搭建的 SSM 项目中([纯 Java 代码搭建 SSM 环境](https://mp.weixin.qq.com/s/NC_0oaeBzRjCB34U_ZWxIQ))，因为本身就没什么自动化配置，所以我们使用了继承 WebMvcConfigurationSupport。

### 6.7.5、@EnableWebMvc

`@EnableWebMvc` 注解，这个注解很好理解，它的作用就是==启用 WebMvcConfigurationSupport==。我们来看看这个注解的定义：

```
/**
 * Adding this annotation to an {@code @Configuration} class imports the Spring MVC
 * configuration from {@link WebMvcConfigurationSupport}, e.g.:
```

可以看到，加了这个注解，就会自动导入 WebMvcConfigurationSupport，所以在 Spring Boot 中，我们也不建议使用 @EnableWebMvc 注解，因为它一样会导致 Spring Boot 中的 SpringMVC 自动化配置失效。



## 6.8、日志级别控制

日志级别控制

```yaml
logging:
  level:
    cn.itcast: debug 
    org.springframework: debug
```

## 6.9、修改端口

查看SpringBoot的全局属性可知，端口通过以下方式配置：

```yaml
server:
  port: 80 		# 设置端口
  servlet:
    path: "*.do"	# 设置请求
```

重启服务后测试：

![1567756502587](assets/1567756502587.png)



## 6.10、添加拦截器

拦截器也是我们经常需要使用的，在SpringBoot中该如何配置呢？

拦截器不是一个普通属性，而是一个类，所以就要用到java配置方式了。在SpringBoot官方文档中有这么一段说明：

> If you want to keep Spring Boot MVC features and you want to add additional MVC configuration (interceptors, formatters, view controllers, and other features), you can add your own @Configuration class of type WebMvcConfigurer but without @EnableWebMvc. If you wish to provide custom instances of RequestMappingHandlerMapping, RequestMappingHandlerAdapter, or ExceptionHandlerExceptionResolver, you can declare a WebMvcRegistrationsAdapter instance to provide such components.
> If you want to take complete control of Spring MVC, you can add your own @Configuration annotated with @EnableWebMvc.
>
> 翻译：
>
> 如果你想要保持Spring Boot 的一些默认MVC特征，同时又想自定义一些MVC配置（包括：拦截器，格式化器, 视图控制器、消息转换器 等等），你应该让一个类实现WebMvcConfigurer，并且添加@Configuration注解，但是千万不要加@EnableWebMvc注解。如果你想要自定义HandlerMapping、HandlerAdapter、ExceptionResolver等组件，你可以创建一个WebMvcRegistrationsAdapter实例 来提供以上组件。
>
> 如果你想要完全自定义SpringMVC，不保留SpringBoot提供的一切特征，你可以自己定义类并且添加@Configuration注解和@EnableWebMvc注解

总结：通过实现WebMvcConfigurer并添加@Configuration注解来实现自定义部分SpringMvc配置。

首先我们定义一个拦截器：

```java
@Slf4j
public class MyInterceptor implements HandlerInterceptor {
    // 使用 @Slf4j 可以不用下面注释的代码
    // private static final Logger log = LoggerFactory.getLogger(MyInterceptor.class);

    /**
     * 在业务处理器处理请求之前被调用。预处理，可以进行编码、安全控制、权限校验等处理；
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        log.debug("preHandle method is running ");
        return true;
    }

    /**
     * 在业务处理器处理请求执行完成后，生成视图之前执行。后处理（调用了Service并返回ModelAndView，但未进行页面渲染），有机会修改ModelAndView （这个博主就基本不怎么用了）；
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        log.debug("postHandle method is running ");
    }

    /**
     * 在DispatcherServlet完全处理完请求后被调用，可用于清理资源等。返回处理（已经渲染了页面）；
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        log.debug("afterCompletion method is running ");
    }
}
```

然后，我们定义配置类，注册拦截器：

```java
@Configuration
public class MvcConfig implements WebMvcConfigurer {
    /**
     * 添加 拦截器
     */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // 添加拦截器, 并设置 拦截所有内容
        registry.addInterceptor(new MyInterceptor()).addPathPatterns("/**");
    }
}
```

结构如下：

![1567756882786](assets/1567756882786.png)

接下来运行并查看日志：

你会发现日志中什么都没有，因为我们记录的log级别是debug，默认是显示info以上，我们需要进行配置。

SpringBoot通过logging.level.*=debug来配置日志级别，*填写包名

```yaml
# 设置com.leyou包的日志级别为debug
logging.level.com.leyou=debug
```

再次运行查看：

```commonlisp
2018-05-05 17:50:01.811 DEBUG 4548 --- [p-nio-80-exec-1] com.leyou.interceptor.LoginInterceptor : preHandle method is now running!
2018-05-05 17:50:01.854 DEBUG 4548 --- [p-nio-80-exec-1] com.leyou.interceptor.LoginInterceptor : postHandle method is now running!
2018-05-05 17:50:01.854 DEBUG 4548 --- [p-nio-80-exec-1] com.leyou.interceptor.LoginInterceptor : afterCompletion method is now running!
```

# 7、持久层开发

## 7.1、整合jdbc和事务

SpringBoot默认支持

spring中的jdbc连接和事务是配置中的重要一环，在SpringBoot中该如何处理呢？

答案是不需要处理，我们只要找到SpringBoot提供的启动器即可：

```xml
<!-- 连接池 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

当然，不要忘了数据库驱动，SpringBoot并不知道我们用的什么数据库，这里我们选择MySQL：

```xml
<!-- mysql 数据库连接驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.15</version>
</dependency>
```

至于事务，SpringBoot中通过注解来控制。就是我们熟知的`@Transactional`

```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    public User queryById(Long id){
        return this.userMapper.selectByPrimaryKey(id);
    }

    @Transactional
    public void deleteById(Long id){
        this.userMapper.deleteByPrimaryKey(id);
    }
}
```

## 7.2、整合连接池

其实，在刚才引入jdbc启动器的时候，SpringBoot已经自动帮我们引入了一个连接池：

![1567757362572](assets/1567757362572.png)

HikariCP应该是目前速度最快的连接池了，我们看看它与c3p0的对比：

![1567757377659](assets/1567757377659.png)

因此，我们只需要指定连接池参数即可：

连接四大参数

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/leyou?serverTimezone=GMT%2B8
    username: root
    password: 123456
```

可省略，SpringBoot自动推断

```properties
spring.datasource.driverClassName=com.mysql.jdbc.Driver

spring.datasource.hikari.idle-timeout=60000
spring.datasource.hikari.maximum-pool-size=30
spring.datasource.hikari.minimum-idle=10
```

当然，如果你更喜欢Druid连接池，也可以使用Druid官方提供的启动器：

```xml
<!-- Druid连接池 -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.6</version>
</dependency>
```

而连接信息的配置与上面是类似的，只不过在连接池特有属性上，方式略有不同：

```properties
#初始化连接数
spring.datasource.druid.initial-size=1
#最小空闲连接
spring.datasource.druid.min-idle=1
#最大活动连接
spring.datasource.druid.max-active=20
#获取连接时测试是否可用
spring.datasource.druid.test-on-borrow=true
#监控页面启动
spring.datasource.druid.stat-view-servlet.allow=true
```

## 7.3、整合mybatis

### 7.3.1、mybatis

SpringBoot官方并没有提供Mybatis的启动器，不过Mybatis[官网](https://github.com/mybatis/spring-boot-starter)自己实现了：

```xml
<!--mybatis -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.2</version>
</dependency>
```

配置，基本没有需要配置的：

```yaml
mybatis:
  # 开启驼峰功能
  configuration:
    map-underscore-to-camel-case: true
  # mapper.xml文件位置,如果没有映射文件，请注释掉 单表操作直接代码(Mapper), 多表用 xml 配置
  mapper-locations: mapper/*.xml
  # mybatis 别名扫描
  type-aliases-package: cn.itcast.pojo
```

需要注意，这里没有配置mapper接口扫描包，因此我们需要给每一个Mapper接口添加@Mapper注解，才能被识别。

```java
@Mapper
public interface UserMapper {
}
```

### 7.3.2、通用mapper

通用Mapper的作者也为自己的插件编写了启动器，我们直接引入即可：

使用通用Mapper后, 可以将 pom.xml 中的 jdbc 和 mybatis 取消, 因为 Mapper中引入了这些, 并且可以在 application.yaml 中将 开启驼峰规则 的命令删除, 因为 Mapper 默认支持 开启驼峰规则.

最后, 在启动类中的@MapperScan 注解要使用 通用Mapper 的, 不能使用Mybatis.

```xml
<!-- 通用mapper -->
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-spring-boot-starter</artifactId>
    <version>2.0.2</version>
</dependency>
```

不需要做任何配置就可以使用了。

```java
@Mapper
public interface UserMapper extends tk.mybatis.mapper.common.Mapper<User>{
}
```

# 8、NoSQL开发

## 8.1、整合Redis

使用 Java 操作 Redis 的方案很多，Jedis 是目前较为流行的一种方案，除了 Jedis ，还有很多其他解决方案，如下：

![img](images\01_Spring Boot\13-1.png)

除了这些方案之外，还有一个使用也相当多的方案，就是 Spring Data Redis。

在传统的 SSM 中，需要开发者自己来配置 Spring Data Redis ，这个配置比较繁琐，主要配置 3 个东西：连接池、连接器信息以及 key 和 value 的序列化方案。

在 Spring Boot 中，默认集成的 Redis 就是 Spring Data Redis，默认底层的连接池使用了 lettuce ，开发者可以自行修改为自己的熟悉的，例如 Jedis。

Spring Data Redis 针对 Redis 提供了非常方便的操作模板 RedisTemplate 。这是 Spring Data 擅长的事情，那么接下来我们就来看看 Spring Boot 中 Spring Data Redis 的具体用法。

### 8.1.1、方案一：Spring Data Redis

#### ①、创建工程

创建工程，引入 Redis 依赖：

![img](F:\BaiduNetdiskWorkspace\06_Spring Boot\images\01_Spring Boot\13-2.png)

创建成功后，还需要手动引入 commos-pool2 的依赖，因此最终完整的 pom.xml 依赖如下：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
</dependency>
```

这里主要就是引入了 Spring Data Redis + 连接池。

#### ②、配置 Redis 信息

接下来配置 Redis 的信息，信息包含两方面，一方面是 Redis 的基本信息，另一方面则是连接池信息:

```properties
spring.redis.database=0
spring.redis.password=123
spring.redis.port=6379
spring.redis.host=192.168.66.128
spring.redis.lettuce.pool.min-idle=5
spring.redis.lettuce.pool.max-idle=10
spring.redis.lettuce.pool.max-active=8
spring.redis.lettuce.pool.max-wait=1ms
spring.redis.lettuce.shutdown-timeout=100ms
```

#### ③、自动配置

当开发者在项目中引入了 Spring Data Redis ，并且配置了 Redis 的基本信息，此时，自动化配置就会生效。

我们从 Spring Boot 中 Redis 的自动化配置类中就可以看出端倪：

```java
@Configuration
@ConditionalOnClass(RedisOperations.class)
@EnableConfigurationProperties(RedisProperties.class)
@Import({ LettuceConnectionConfiguration.class, JedisConnectionConfiguration.class })
public class RedisAutoConfiguration {
        @Bean
        @ConditionalOnMissingBean(name = "redisTemplate")
        public RedisTemplate<Object, Object> redisTemplate(
                        RedisConnectionFactory redisConnectionFactory) throws UnknownHostException {
                RedisTemplate<Object, Object> template = new RedisTemplate<>();
                template.setConnectionFactory(redisConnectionFactory);
                return template;
        }
        @Bean
        @ConditionalOnMissingBean
        public StringRedisTemplate stringRedisTemplate(
                        RedisConnectionFactory redisConnectionFactory) throws UnknownHostException {
                StringRedisTemplate template = new StringRedisTemplate();
                template.setConnectionFactory(redisConnectionFactory);
                return template;
        }
}
```

> 【说明】
>
> ①、首先标记这个是一个配置类，同时该配置在 RedisOperations 存在的情况下才会生效(即项目中引入了 Spring Data Redis)
>
> ②、然后导入在 application.properties 中配置的属性
>
> ③、然后再导入连接池信息（如果存在的话）
>
> ④、最后，提供了两个 Bean ，RedisTemplate 和 StringRedisTemplate ，其中 StringRedisTemplate 是 RedisTemplate 的子类，两个的方法基本一致，不同之处主要体现在操作的数据类型不同，RedisTemplate 中的两个泛型都是 Object ，意味者存储的 key 和 value 都可以是一个对象，而 StringRedisTemplate 的 两个泛型都是 String ，意味者 StringRedisTemplate 的 key 和 value 都只能是字符串。如果开发者没有提供相关的 Bean ，这两个配置就会生效，否则不会生效。



#### ④、使用

接下来，可以直接在 Service 中注入 StringRedisTemplate 或者 RedisTemplate 来使用：

```java
@Service
public class HelloService {
    @Autowired
    RedisTemplate redisTemplate;
    public void hello() {
        ValueOperations ops = redisTemplate.opsForValue();
        ops.set("k1", "v1");
        Object k1 = ops.get("k1");
        System.out.println(k1);
    }
}
```

Redis 中的数据操作，大体上来说，可以分为两种：

1. 针对 key 的操作，相关的方法就在 RedisTemplate 中
2. 针对具体数据类型的操作，相关的方法需要首先获取对应的数据类型，获取相应数据类型的操作方法是 opsForXXX

调用该方法就可以将数据存储到 Redis 中去了，如下：

![img](images\01_Spring Boot\13-3.png)

k1 前面的字符是由于使用了 RedisTemplate 导致的，RedisTemplate 对 key 进行序列化之后的结果。

RedisTemplate 中，key 默认的序列化方案是 `JdkSerializationRedisSerializer` 。

而在 StringRedisTemplate 中，key 默认的序列化方案是 `StringRedisSerializer` ，因此，如果使用 StringRedisTemplate ，默认情况下 key 前面不会有前缀。

不过开发者也可以自行修改 RedisTemplate 中的序列化方案，如下:

```java
@Service
public class HelloService {
    @Autowired
    RedisTemplate redisTemplate;
    public void hello() {
        redisTemplate.setKeySerializer(new StringRedisSerializer());//修改序列化方式
        ValueOperations ops = redisTemplate.opsForValue();
        ops.set("k1", "v1");
        Object k1 = ops.get("k1");
        System.out.println(k1);
    }
}
```

当然也可以直接使用 StringRedisTemplate：

```java
@Service
public class HelloService {
    @Autowired
    StringRedisTemplate stringRedisTemplate;
    public void hello2() {
        ValueOperations ops = stringRedisTemplate.opsForValue();
        ops.set("k2", "v2");
        Object k1 = ops.get("k2");
        System.out.println(k1);
    }
}
```

另外需要注意 ，Spring Boot 的自动化配置，只能配置单机的 Redis 

### 8.1.2、方案二：Spring cache

# 9、权限管理开发



# 附录1.启动器

==Spring Boot application starters==

spring-boot-starter-thymeleaf

使用Thymeleaf视图构建MVC Web应用程序

 

spring-boot-starter-ws

使用Spring Web服务。1.4不推荐使用，推荐使用spring-boot-starter-web-services

 

spring-boot-starter-data-couchbase

Starter for using Couchbase document-oriented database and Spring Data Couchbase

 

spring-boot-starter-artemis

使用Apache Artemis启动JMS消息传递

 

spring-boot-starter-web-services

使用Spring Web服务

 

spring-boot-starter-mail

支持使用Java Mail和Spring Framework发送电子邮件

 

spring-boot-starter-data-redis

使用Redis键值数据存储与Spring Data Redis和Jedis客户端

 

spring-boot-starter-web

启动器构建web，包括RESTful，使用Spring MVC的应用程序。使用Tomcat作为默认嵌入式容器

 

spring-boot-starter-data-gemfire

Starter for using GemFire distributed data store and Spring Data GemFire

 

spring-boot-starter-activemq

使用Apache ActiveMQ启动JMS消息传递

 

spring-boot-starter-data-elasticsearch

使用Elasticsearch搜索和分析引擎和Spring Data Elasticsearch

 

spring-boot-starter-integration

Starter for using Spring Integration

 

spring-boot-starter-test

Spring Boot应用程序用于测试包括JUnit，Hamcrest和Mockito

 

spring-boot-starter-hornetq

使用HornetQ启动JMS消息传递。1.4已弃用，推荐使用spring-boot-starter-artemis

 

spring-boot-starter-jdbc

使用JDBC与Tomcat JDBC连接池

 

spring-boot-starter-mobile

使用Spring Mobile构建Web应用程序的入门

 

spring-boot-starter-validation

使用Java Bean校验与Hibernate校验器

 

spring-boot-starter-hateoas

使用Spring MVC和Spring HATEOAS构建基于超媒体的RESTful Web应用程序的入门

 

spring-boot-starter-jersey

使用JAX-RS和Jersey构建RESTful Web应用程序的入门。 spring-boot-starter-web的替代品

 

spring-boot-starter-data-neo4j

使用Neo4j图数据库和Spring Data Neo4j

 

spring-boot-starter-websocket

使用Spring Framework的WebSocket支持构建WebSocket应用程序

 

spring-boot-starter-aop

使用Spring AOP和AspectJ进行面向方面编程

 

spring-boot-starter-amqp

使用Spring AMQP和Rabbit MQ的入门

 

spring-boot-starter-data-cassandra

使用Cassandra分布式数据库和Spring Data Cassandra

 

spring-boot-starter-social-facebook

使用Spring Social Facebook

 

spring-boot-starter-jta-atomikos

使用Atomikos进行JTA事务

 

spring-boot-starter-security

使用Spring Security

 

spring-boot-starter-mustache

使用Mustache视图构建MVC Web应用程序

 

spring-boot-starter-data-jpa

使用Spring Data JPA与Hibernate

 

spring-boot-starter

核心启动器，包括自动配置支持，日志记录和YAML

 

spring-boot-starter-velocity

使用Velocity视图构建MVC Web应用程序。1.4已弃用

 

spring-boot-starter-groovy-templates

使用Groovy模板视图构建MVC Web应用程序

 

spring-boot-starter-freemarker

使用FreeMarker视图构建MVC Web应用程序

 

spring-boot-starter-batch

使用Spring Batch

 

spring-boot-starter-redis

使用Redis键值数据存储与Spring Data Redis和Jedis客户端的入门。1.4已弃用，建议使用spring-boot-starter-data-redis

 

spring-boot-starter-social-linkedin

Stater for using Spring Social LinkedIn

 

spring-boot-starter-cache

支持使用Spring Framework的缓存

 

spring-boot-starter-data-solr

使用带有Spring Data Solr的Apache Solr搜索平台

 

spring-boot-starter-data-mongodb

使用MongoDB和Spring Data MongoDB

 

spring-boot-starter-jooq

使用jOOQ访问SQL数据库。 spring-boot-starter-data-jpa或spring-boot-starter-jdbc的替代方法

 

spring-boot-starter-jta-narayana

Spring Boot启动Narayana JTA

 

spring-boot-starter-cloud-connectors

启动者使用Spring Cloud连接器，简化了连接到云平台中的服务，如Cloud Foundry和Heroku

 

spring-boot-starter-jta-bitronix

使用Bitronix进行JTA事务

 

spring-boot-starter-social-twitter

使用Spring Social Twitter

 

spring-boot-starter-data-rest

使用Spring Data REST通过REST暴露Spring数据存储库

 

\2.    Spring Boot production starters

spring-boot-starter-actuator

使用Spring Boot的Actuator，提供生产就绪的功能，以帮助您监视和管理您的应用程序

 

spring-boot-starter-remote-shell

使用CRaSH远程shell通过SSH监视和管理您的应用程序

 

\3.    Spring Boot technical starters

spring-boot-starter-undertow

使用Undertow作为嵌入式servlet容器。 spring-boot-starter-tomcat的替代方法

 

spring-boot-starter-jetty

使用Jetty作为嵌入式servlet容器的。 spring-boot-starter-tomcat的替代方法

 

spring-boot-starter-logging

使用Logback进行日志记录。 默认日志启动器

 

spring-boot-starter-tomcat

使用Tomcat作为嵌入式servlet容器。 spring-boot-starter-web使用的默认servlet容器

 

spring-boot-starter-log4j2

使用Log4j2进行日志记录。 spring-boot-starter-logging的替代方法

 

```properties

附录2.springBoot全局属性
# ===================================================================
# COMMON SPRING BOOT PROPERTIES
#
# This sample file is provided as a guideline. Do NOT copy it in its
# entirety to your own application.            ^^^
# ===================================================================


# ----------------------------------------
# CORE PROPERTIES
# ----------------------------------------
debug=false # Enable debug logs.
trace=false # Enable trace logs.

# LOGGING
logging.config= # Location of the logging configuration file. For instance, `classpath:logback.xml` for Logback.
logging.exception-conversion-word=%wEx # Conversion word used when logging exceptions.
logging.file= # Log file name (for instance, `myapp.log`). Names can be an exact location or relative to the current directory.
logging.file.max-history=0 # Maximum of archive log files to keep. Only supported with the default logback setup.
logging.file.max-size=10MB # Maximum log file size. Only supported with the default logback setup.
logging.level.*= # Log levels severity mapping. For instance, `logging.level.org.springframework=DEBUG`.
logging.path= # Location of the log file. For instance, `/var/log`.
logging.pattern.console= # Appender pattern for output to the console. Supported only with the default Logback setup.
logging.pattern.dateformat=yyyy-MM-dd HH:mm:ss.SSS # Appender pattern for log date format. Supported only with the default Logback setup.
logging.pattern.file= # Appender pattern for output to a file. Supported only with the default Logback setup.
logging.pattern.level=%5p # Appender pattern for log level. Supported only with the default Logback setup.
logging.register-shutdown-hook=false # Register a shutdown hook for the logging system when it is initialized.

# AOP
spring.aop.auto=true # Add @EnableAspectJAutoProxy.
spring.aop.proxy-target-class=true # Whether subclass-based (CGLIB) proxies are to be created (true), as opposed to standard Java interface-based proxies (false).

# IDENTITY (ContextIdApplicationContextInitializer)
spring.application.name= # Application name.

# ADMIN (SpringApplicationAdminJmxAutoConfiguration)
spring.application.admin.enabled=false # Whether to enable admin features for the application.
spring.application.admin.jmx-name=org.springframework.boot:type=Admin,name=SpringApplication # JMX name of the application admin MBean.

# AUTO-CONFIGURATION
spring.autoconfigure.exclude= # Auto-configuration classes to exclude.

# BANNER
spring.banner.charset=UTF-8 # Banner file encoding.
spring.banner.location=classpath:banner.txt # Banner text resource location.
spring.banner.image.location=classpath:banner.gif # Banner image file location (jpg or png can also be used).
spring.banner.image.width=76 # Width of the banner image in chars.
spring.banner.image.height= # Height of the banner image in chars (default based on image height).
spring.banner.image.margin=2 # Left hand image margin in chars.
spring.banner.image.invert=false # Whether images should be inverted for dark terminal themes.

# SPRING CORE
spring.beaninfo.ignore=true # Whether to skip search of BeanInfo classes.

# SPRING CACHE (CacheProperties)
spring.cache.cache-names= # Comma-separated list of cache names to create if supported by the underlying cache manager.
spring.cache.caffeine.spec= # The spec to use to create caches. See CaffeineSpec for more details on the spec format.
spring.cache.couchbase.expiration=0ms # Entry expiration. By default the entries never expire. Note that this value is ultimately converted to seconds.
spring.cache.ehcache.config= # The location of the configuration file to use to initialize EhCache.
spring.cache.infinispan.config= # The location of the configuration file to use to initialize Infinispan.
spring.cache.jcache.config= # The location of the configuration file to use to initialize the cache manager.
spring.cache.jcache.provider= # Fully qualified name of the CachingProvider implementation to use to retrieve the JSR-107 compliant cache manager. Needed only if more than one JSR-107 implementation is available on the classpath.
spring.cache.redis.cache-null-values=true # Allow caching null values.
spring.cache.redis.key-prefix= # Key prefix.
spring.cache.redis.time-to-live=0ms # Entry expiration. By default the entries never expire.
spring.cache.redis.use-key-prefix=true # Whether to use the key prefix when writing to Redis.
spring.cache.type= # Cache type. By default, auto-detected according to the environment.

# SPRING CONFIG - using environment property only (ConfigFileApplicationListener)
spring.config.additional-location= # Config file locations used in addition to the defaults.
spring.config.location= # Config file locations that replace the defaults.
spring.config.name=application # Config file name.

# HAZELCAST (HazelcastProperties)
spring.hazelcast.config= # The location of the configuration file to use to initialize Hazelcast.

# PROJECT INFORMATION (ProjectInfoProperties)
spring.info.build.location=classpath:META-INF/build-info.properties # Location of the generated build-info.properties file.
spring.info.git.location=classpath:git.properties # Location of the generated git.properties file.

# JMX
spring.jmx.default-domain= # JMX domain name.
spring.jmx.enabled=true # Expose management beans to the JMX domain.
spring.jmx.server=mbeanServer # MBeanServer bean name.

# Email (MailProperties)
spring.mail.default-encoding=UTF-8 # Default MimeMessage encoding.
spring.mail.host= # SMTP server host. For instance, `smtp.example.com`.
spring.mail.jndi-name= # Session JNDI name. When set, takes precedence over other mail settings.
spring.mail.password= # Login password of the SMTP server.
spring.mail.port= # SMTP server port.
spring.mail.properties.*= # Additional JavaMail session properties.
spring.mail.protocol=smtp # Protocol used by the SMTP server.
spring.mail.test-connection=false # Whether to test that the mail server is available on startup.
spring.mail.username= # Login user of the SMTP server.

# APPLICATION SETTINGS (SpringApplication)
spring.main.banner-mode=console # Mode used to display the banner when the application runs.
spring.main.sources= # Sources (class names, package names, or XML resource locations) to include in the ApplicationContext.
spring.main.web-application-type= # Flag to explicitly request a specific type of web application. If not set, auto-detected based on the classpath.

# FILE ENCODING (FileEncodingApplicationListener)
spring.mandatory-file-encoding= # Expected character encoding the application must use.

# INTERNATIONALIZATION (MessageSourceProperties)
spring.messages.always-use-message-format=false # Whether to always apply the MessageFormat rules, parsing even messages without arguments.
spring.messages.basename=messages # Comma-separated list of basenames (essentially a fully-qualified classpath location), each following the ResourceBundle convention with relaxed support for slash based locations.
spring.messages.cache-duration= # Loaded resource bundle files cache duration. When not set, bundles are cached forever. If a duration suffix is not specified, seconds will be used.
spring.messages.encoding=UTF-8 # Message bundles encoding.
spring.messages.fallback-to-system-locale=true # Whether to fall back to the system Locale if no files for a specific Locale have been found.
spring.messages.use-code-as-default-message=false # Whether to use the message code as the default message instead of throwing a "NoSuchMessageException". Recommended during development only.

# OUTPUT
spring.output.ansi.enabled=detect # Configures the ANSI output.

# PID FILE (ApplicationPidFileWriter)
spring.pid.fail-on-write-error= # Fails if ApplicationPidFileWriter is used but it cannot write the PID file.
spring.pid.file= # Location of the PID file to write (if ApplicationPidFileWriter is used).

# PROFILES
spring.profiles.active= # Comma-separated list of active profiles. Can be overridden by a command line switch.
spring.profiles.include= # Unconditionally activate the specified comma-separated list of profiles (or list of profiles if using YAML).

# QUARTZ SCHEDULER (QuartzProperties)
spring.quartz.jdbc.initialize-schema=embedded # Database schema initialization mode.
spring.quartz.jdbc.schema=classpath:org/quartz/impl/jdbcjobstore/tables_@@platform@@.sql # Path to the SQL file to use to initialize the database schema.
spring.quartz.job-store-type=memory # Quartz job store type.
spring.quartz.properties.*= # Additional Quartz Scheduler properties.

# REACTOR (ReactorCoreProperties)
spring.reactor.stacktrace-mode.enabled=false # Whether Reactor should collect stacktrace information at runtime.

# SENDGRID (SendGridAutoConfiguration)
spring.sendgrid.api-key= # SendGrid API key.
spring.sendgrid.proxy.host= # SendGrid proxy host.
spring.sendgrid.proxy.port= # SendGrid proxy port.


# ----------------------------------------
# WEB PROPERTIES
# ----------------------------------------

# EMBEDDED SERVER CONFIGURATION (ServerProperties)
server.address= # Network address to which the server should bind.
server.compression.enabled=false # Whether response compression is enabled.
server.compression.excluded-user-agents= # List of user-agents to exclude from compression.
server.compression.mime-types=text/html,text/xml,text/plain,text/css,text/javascript,application/javascript # Comma-separated list of MIME types that should be compressed.
server.compression.min-response-size=2048 # Minimum "Content-Length" value that is required for compression to be performed.
server.connection-timeout= # Time that connectors wait for another HTTP request before closing the connection. When not set, the connector's container-specific default is used. Use a value of -1 to indicate no (that is, an infinite) timeout.
server.error.include-exception=false # Include the "exception" attribute.
server.error.include-stacktrace=never # When to include a "stacktrace" attribute.
server.error.path=/error # Path of the error controller.
server.error.whitelabel.enabled=true # Whether to enable the default error page displayed in browsers in case of a server error.
server.http2.enabled=false # Whether to enable HTTP/2 support, if the current environment supports it.
server.jetty.acceptors= # Number of acceptor threads to use.
server.jetty.accesslog.append=false # Append to log.
server.jetty.accesslog.date-format=dd/MMM/yyyy:HH:mm:ss Z # Timestamp format of the request log.
server.jetty.accesslog.enabled=false # Enable access log.
server.jetty.accesslog.extended-format=false # Enable extended NCSA format.
server.jetty.accesslog.file-date-format= # Date format to place in log file name.
server.jetty.accesslog.filename= # Log filename. If not specified, logs redirect to "System.err".
server.jetty.accesslog.locale= # Locale of the request log.
server.jetty.accesslog.log-cookies=false # Enable logging of the request cookies.
server.jetty.accesslog.log-latency=false # Enable logging of request processing time.
server.jetty.accesslog.log-server=false # Enable logging of the request hostname.
server.jetty.accesslog.retention-period=31 # Number of days before rotated log files are deleted.
server.jetty.accesslog.time-zone=GMT # Timezone of the request log.
server.jetty.max-http-post-size=0 # Maximum size, in bytes, of the HTTP post or put content.
server.jetty.selectors= # Number of selector threads to use.
server.max-http-header-size=0 # Maximum size, in bytes, of the HTTP message header.
server.port=8080 # Server HTTP port.
server.server-header= # Value to use for the Server response header (if empty, no header is sent).
server.use-forward-headers= # Whether X-Forwarded-* headers should be applied to the HttpRequest.
server.servlet.context-parameters.*= # Servlet context init parameters.
server.servlet.context-path= # Context path of the application.
server.servlet.application-display-name=application # Display name of the application.
server.servlet.jsp.class-name=org.apache.jasper.servlet.JspServlet # The class name of the JSP servlet.
server.servlet.jsp.init-parameters.*= # Init parameters used to configure the JSP servlet.
server.servlet.jsp.registered=true # Whether the JSP servlet is registered.
server.servlet.path=/ # Path of the main dispatcher servlet.
server.servlet.session.cookie.comment= # Comment for the session cookie.
server.servlet.session.cookie.domain= # Domain for the session cookie.
server.servlet.session.cookie.http-only= # "HttpOnly" flag for the session cookie.
server.servlet.session.cookie.max-age= # Maximum age of the session cookie. If a duration suffix is not specified, seconds will be used.
server.servlet.session.cookie.name= # Session cookie name.
server.servlet.session.cookie.path= # Path of the session cookie.
server.servlet.session.cookie.secure= # "Secure" flag for the session cookie.
server.servlet.session.persistent=false # Whether to persist session data between restarts.
server.servlet.session.store-dir= # Directory used to store session data.
server.servlet.session.timeout= # Session timeout. If a duration suffix is not specified, seconds will be used.
server.servlet.session.tracking-modes= # Session tracking modes (one or more of the following: "cookie", "url", "ssl").
server.ssl.ciphers= # Supported SSL ciphers.
server.ssl.client-auth= # Whether client authentication is wanted ("want") or needed ("need"). Requires a trust store.
server.ssl.enabled= # Enable SSL support.
server.ssl.enabled-protocols= # Enabled SSL protocols.
server.ssl.key-alias= # Alias that identifies the key in the key store.
server.ssl.key-password= # Password used to access the key in the key store.
server.ssl.key-store= # Path to the key store that holds the SSL certificate (typically a jks file).
server.ssl.key-store-password= # Password used to access the key store.
server.ssl.key-store-provider= # Provider for the key store.
server.ssl.key-store-type= # Type of the key store.
server.ssl.protocol=TLS # SSL protocol to use.
server.ssl.trust-store= # Trust store that holds SSL certificates.
server.ssl.trust-store-password= # Password used to access the trust store.
server.ssl.trust-store-provider= # Provider for the trust store.
server.ssl.trust-store-type= # Type of the trust store.
server.tomcat.accept-count=0 # Maximum queue length for incoming connection requests when all possible request processing threads are in use.
server.tomcat.accesslog.buffered=true # Whether to buffer output such that it is flushed only periodically.
server.tomcat.accesslog.directory=logs # Directory in which log files are created. Can be absolute or relative to the Tomcat base dir.
server.tomcat.accesslog.enabled=false # Enable access log.
server.tomcat.accesslog.file-date-format=.yyyy-MM-dd # Date format to place in the log file name.
server.tomcat.accesslog.pattern=common # Format pattern for access logs.
server.tomcat.accesslog.prefix=access_log # Log file name prefix.
server.tomcat.accesslog.rename-on-rotate=false # Whether to defer inclusion of the date stamp in the file name until rotate time.
server.tomcat.accesslog.request-attributes-enabled=false # Set request attributes for the IP address, Hostname, protocol, and port used for the request.
server.tomcat.accesslog.rotate=true # Whether to enable access log rotation.
server.tomcat.accesslog.suffix=.log # Log file name suffix.
server.tomcat.additional-tld-skip-patterns= # Comma-separated list of additional patterns that match jars to ignore for TLD scanning.
server.tomcat.background-processor-delay=30s # Delay between the invocation of backgroundProcess methods. If a duration suffix is not specified, seconds will be used.
server.tomcat.basedir= # Tomcat base directory. If not specified, a temporary directory is used.
server.tomcat.internal-proxies=10\\.\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}|\\
        192\\.168\\.\\d{1,3}\\.\\d{1,3}|\\
        169\\.254\\.\\d{1,3}\\.\\d{1,3}|\\
        127\\.\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}|\\
        172\\.1[6-9]{1}\\.\\d{1,3}\\.\\d{1,3}|\\
        172\\.2[0-9]{1}\\.\\d{1,3}\\.\\d{1,3}|\\
        172\\.3[0-1]{1}\\.\\d{1,3}\\.\\d{1,3} # Regular expression matching trusted IP addresses.
server.tomcat.max-connections=0 # Maximum number of connections that the server accepts and processes at any given time.
server.tomcat.max-http-header-size=0 # Maximum size, in bytes, of the HTTP message header.
server.tomcat.max-http-post-size=0 # Maximum size, in bytes, of the HTTP post content.
server.tomcat.max-threads=0 # Maximum number of worker threads.
server.tomcat.min-spare-threads=0 # Minimum number of worker threads.
server.tomcat.port-header=X-Forwarded-Port # Name of the HTTP header used to override the original port value.
server.tomcat.protocol-header= # Header that holds the incoming protocol, usually named "X-Forwarded-Proto".
server.tomcat.protocol-header-https-value=https # Value of the protocol header indicating whether the incoming request uses SSL.
server.tomcat.redirect-context-root= # Whether requests to the context root should be redirected by appending a / to the path.
server.tomcat.remote-ip-header= # Name of the HTTP header from which the remote IP is extracted. For instance, `X-FORWARDED-FOR`.
server.tomcat.resource.cache-ttl= # Time-to-live of the static resource cache.
server.tomcat.uri-encoding=UTF-8 # Character encoding to use to decode the URI.
server.tomcat.use-relative-redirects= # Whether HTTP 1.1 and later location headers generated by a call to sendRedirect will use relative or absolute redirects.
server.undertow.accesslog.dir= # Undertow access log directory.
server.undertow.accesslog.enabled=false # Whether to enable the access log.
server.undertow.accesslog.pattern=common # Format pattern for access logs.
server.undertow.accesslog.prefix=access_log. # Log file name prefix.
server.undertow.accesslog.rotate=true # Whether to enable access log rotation.
server.undertow.accesslog.suffix=log # Log file name suffix.
server.undertow.buffer-size= # Size of each buffer, in bytes.
server.undertow.direct-buffers= # Whether to allocate buffers outside the Java heap.
server.undertow.io-threads= # Number of I/O threads to create for the worker.
server.undertow.eager-filter-init=true # Whether servlet filters should be initialized on startup.
server.undertow.max-http-post-size=0 # Maximum size, in bytes, of the HTTP post content.
server.undertow.worker-threads= # Number of worker threads.

# FREEMARKER (FreeMarkerProperties)
spring.freemarker.allow-request-override=false # Whether HttpServletRequest attributes are allowed to override (hide) controller generated model attributes of the same name.
spring.freemarker.allow-session-override=false # Whether HttpSession attributes are allowed to override (hide) controller generated model attributes of the same name.
spring.freemarker.cache=false # Whether to enable template caching.
spring.freemarker.charset=UTF-8 # Template encoding.
spring.freemarker.check-template-location=true # Whether to check that the templates location exists.
spring.freemarker.content-type=text/html # Content-Type value.
spring.freemarker.enabled=true # Whether to enable MVC view resolution for this technology.
spring.freemarker.expose-request-attributes=false # Whether all request attributes should be added to the model prior to merging with the template.
spring.freemarker.expose-session-attributes=false # Whether all HttpSession attributes should be added to the model prior to merging with the template.
spring.freemarker.expose-spring-macro-helpers=true # Whether to expose a RequestContext for use by Spring's macro library, under the name "springMacroRequestContext".
spring.freemarker.prefer-file-system-access=true # Whether to prefer file system access for template loading. File system access enables hot detection of template changes.
spring.freemarker.prefix= # Prefix that gets prepended to view names when building a URL.
spring.freemarker.request-context-attribute= # Name of the RequestContext attribute for all views.
spring.freemarker.settings.*= # Well-known FreeMarker keys which are passed to FreeMarker's Configuration.
spring.freemarker.suffix=.ftl # Suffix that gets appended to view names when building a URL.
spring.freemarker.template-loader-path=classpath:/templates/ # Comma-separated list of template paths.
spring.freemarker.view-names= # White list of view names that can be resolved.

# GROOVY TEMPLATES (GroovyTemplateProperties)
spring.groovy.template.allow-request-override=false # Whether HttpServletRequest attributes are allowed to override (hide) controller generated model attributes of the same name.
spring.groovy.template.allow-session-override=false # Whether HttpSession attributes are allowed to override (hide) controller generated model attributes of the same name.
spring.groovy.template.cache=false # Whether to enable template caching.
spring.groovy.template.charset=UTF-8 # Template encoding.
spring.groovy.template.check-template-location=true # Whether to check that the templates location exists.
spring.groovy.template.configuration.*= # See GroovyMarkupConfigurer
spring.groovy.template.content-type=text/html # Content-Type value.
spring.groovy.template.enabled=true # Whether to enable MVC view resolution for this technology.
spring.groovy.template.expose-request-attributes=false # Whether all request attributes should be added to the model prior to merging with the template.
spring.groovy.template.expose-session-attributes=false # Whether all HttpSession attributes should be added to the model prior to merging with the template.
spring.groovy.template.expose-spring-macro-helpers=true # Whether to expose a RequestContext for use by Spring's macro library, under the name "springMacroRequestContext".
spring.groovy.template.prefix= # Prefix that gets prepended to view names when building a URL.
spring.groovy.template.request-context-attribute= # Name of the RequestContext attribute for all views.
spring.groovy.template.resource-loader-path=classpath:/templates/ # Template path.
spring.groovy.template.suffix=.tpl # Suffix that gets appended to view names when building a URL.
spring.groovy.template.view-names= # White list of view names that can be resolved.

# SPRING HATEOAS (HateoasProperties)
spring.hateoas.use-hal-as-default-json-media-type=true # Whether application/hal+json responses should be sent to requests that accept application/json.

# HTTP message conversion
spring.http.converters.preferred-json-mapper= # Preferred JSON mapper to use for HTTP message conversion. By default, auto-detected according to the environment.

# HTTP encoding (HttpEncodingProperties)
spring.http.encoding.charset=UTF-8 # Charset of HTTP requests and responses. Added to the "Content-Type" header if not set explicitly.
spring.http.encoding.enabled=true # Whether to enable http encoding support.
spring.http.encoding.force= # Whether to force the encoding to the configured charset on HTTP requests and responses.
spring.http.encoding.force-request= # Whether to force the encoding to the configured charset on HTTP requests. Defaults to true when "force" has not been specified.
spring.http.encoding.force-response= # Whether to force the encoding to the configured charset on HTTP responses.
spring.http.encoding.mapping= # Locale in which to encode mapping.

# MULTIPART (MultipartProperties)
spring.servlet.multipart.enabled=true # Whether to enable support of multipart uploads.
spring.servlet.multipart.file-size-threshold=0 # Threshold after which files are written to disk. Values can use the suffixes "MB" or "KB" to indicate megabytes or kilobytes, respectively.
spring.servlet.multipart.location= # Intermediate location of uploaded files.
spring.servlet.multipart.max-file-size=1MB # Max file size. Values can use the suffixes "MB" or "KB" to indicate megabytes or kilobytes, respectively.
spring.servlet.multipart.max-request-size=10MB # Max request size. Values can use the suffixes "MB" or "KB" to indicate megabytes or kilobytes, respectively.
spring.servlet.multipart.resolve-lazily=false # Whether to resolve the multipart request lazily at the time of file or parameter access.

# JACKSON (JacksonProperties)
spring.jackson.date-format= # Date format string or a fully-qualified date format class name. For instance, `yyyy-MM-dd HH:mm:ss`.
spring.jackson.default-property-inclusion= # Controls the inclusion of properties during serialization. Configured with one of the values in Jackson's JsonInclude.Include enumeration.
spring.jackson.deserialization.*= # Jackson on/off features that affect the way Java objects are deserialized.
spring.jackson.generator.*= # Jackson on/off features for generators.
spring.jackson.joda-date-time-format= # Joda date time format string. If not configured, "date-format" is used as a fallback if it is configured with a format string.
spring.jackson.locale= # Locale used for formatting.
spring.jackson.mapper.*= # Jackson general purpose on/off features.
spring.jackson.parser.*= # Jackson on/off features for parsers.
spring.jackson.property-naming-strategy= # One of the constants on Jackson's PropertyNamingStrategy. Can also be a fully-qualified class name of a PropertyNamingStrategy subclass.
spring.jackson.serialization.*= # Jackson on/off features that affect the way Java objects are serialized.
spring.jackson.time-zone= #  Time zone used when formatting dates. For instance, "America/Los_Angeles" or "GMT+10".

# GSON (GsonProperties)
spring.gson.date-format= # Format to use when serializing Date objects.
spring.gson.disable-html-escaping= # Whether to disable the escaping of HTML characters such as '<', '>', etc.
spring.gson.disable-inner-class-serialization= # Whether to exclude inner classes during serialization.
spring.gson.enable-complex-map-key-serialization= # Whether to enable serialization of complex map keys (i.e. non-primitives).
spring.gson.exclude-fields-without-expose-annotation= # Whether to exclude all fields from consideration for serialization or deserialization that do not have the "Expose" annotation.
spring.gson.field-naming-policy= # Naming policy that should be applied to an object's field during serialization and deserialization.
spring.gson.generate-non-executable-json= # Whether to generate non executable JSON by prefixing the output with some special text.
spring.gson.lenient= # Whether to be lenient about parsing JSON that doesn't conform to RFC 4627.
spring.gson.long-serialization-policy= # Serialization policy for Long and long types.
spring.gson.pretty-printing= # Whether to output serialized JSON that fits in a page for pretty printing.
spring.gson.serialize-nulls= # Whether to serialize null fields.

# JERSEY (JerseyProperties)
spring.jersey.application-path= # Path that serves as the base URI for the application. If specified, overrides the value of "@ApplicationPath".
spring.jersey.filter.order=0 # Jersey filter chain order.
spring.jersey.init.*= # Init parameters to pass to Jersey through the servlet or filter.
spring.jersey.servlet.load-on-startup=-1 # Load on startup priority of the Jersey servlet.
spring.jersey.type=servlet # Jersey integration type.

# SPRING LDAP (LdapProperties)
spring.ldap.anonymous-read-only=false # Whether read-only operations should use an anonymous environment.
spring.ldap.base= # Base suffix from which all operations should originate.
spring.ldap.base-environment.*= # LDAP specification settings.
spring.ldap.password= # Login password of the server.
spring.ldap.urls= # LDAP URLs of the server.
spring.ldap.username= # Login username of the server.

# EMBEDDED LDAP (EmbeddedLdapProperties)
spring.ldap.embedded.base-dn= # List of base DNs.
spring.ldap.embedded.credential.username= # Embedded LDAP username.
spring.ldap.embedded.credential.password= # Embedded LDAP password.
spring.ldap.embedded.ldif=classpath:schema.ldif # Schema (LDIF) script resource reference.
spring.ldap.embedded.port=0 # Embedded LDAP port.
spring.ldap.embedded.validation.enabled=true # Whether to enable LDAP schema validation.
spring.ldap.embedded.validation.schema= # Path to the custom schema.

# MUSTACHE TEMPLATES (MustacheAutoConfiguration)
spring.mustache.allow-request-override=false # Whether HttpServletRequest attributes are allowed to override (hide) controller generated model attributes of the same name.
spring.mustache.allow-session-override=false # Whether HttpSession attributes are allowed to override (hide) controller generated model attributes of the same name.
spring.mustache.cache=false # Whether to enable template caching.
spring.mustache.charset=UTF-8 # Template encoding.
spring.mustache.check-template-location=true # Whether to check that the templates location exists.
spring.mustache.content-type=text/html # Content-Type value.
spring.mustache.enabled=true # Whether to enable MVC view resolution for this technology.
spring.mustache.expose-request-attributes=false # Whether all request attributes should be added to the model prior to merging with the template.
spring.mustache.expose-session-attributes=false # Whether all HttpSession attributes should be added to the model prior to merging with the template.
spring.mustache.expose-spring-macro-helpers=true # Whether to expose a RequestContext for use by Spring's macro library, under the name "springMacroRequestContext".
spring.mustache.prefix=classpath:/templates/ # Prefix to apply to template names.
spring.mustache.request-context-attribute= # Name of the RequestContext attribute for all views.
spring.mustache.suffix=.mustache # Suffix to apply to template names.
spring.mustache.view-names= # White list of view names that can be resolved.

# SPRING MVC (WebMvcProperties)
spring.mvc.async.request-timeout= # Amount of time before asynchronous request handling times out.
spring.mvc.contentnegotiation.favor-parameter=false # Whether a request parameter ("format" by default) should be used to determine the requested media type.
spring.mvc.contentnegotiation.favor-path-extension=false # Whether the path extension in the URL path should be used to determine the requested media type.
spring.mvc.contentnegotiation.media-types.*= # Map file extensions to media types for content negotiation. For instance, yml to text/yaml.
spring.mvc.contentnegotiation.parameter-name= # Query parameter name to use when "favor-parameter" is enabled.
spring.mvc.date-format= # Date format to use. For instance, `dd/MM/yyyy`.
spring.mvc.dispatch-trace-request=false # Whether to dispatch TRACE requests to the FrameworkServlet doService method.
spring.mvc.dispatch-options-request=true # Whether to dispatch OPTIONS requests to the FrameworkServlet doService method.
spring.mvc.favicon.enabled=true # Whether to enable resolution of favicon.ico.
spring.mvc.formcontent.putfilter.enabled=true # Whether to enable Spring's HttpPutFormContentFilter.
spring.mvc.ignore-default-model-on-redirect=true # Whether the content of the "default" model should be ignored during redirect scenarios.
spring.mvc.locale= # Locale to use. By default, this locale is overridden by the "Accept-Language" header.
spring.mvc.locale-resolver=accept-header # Define how the locale should be resolved.
spring.mvc.log-resolved-exception=false # Whether to enable warn logging of exceptions resolved by a "HandlerExceptionResolver".
spring.mvc.message-codes-resolver-format= # Formatting strategy for message codes. For instance, `PREFIX_ERROR_CODE`.
spring.mvc.pathmatch.use-registered-suffix-pattern=false # Whether suffix pattern matching should work only against extensions registered with "spring.mvc.contentnegotiation.media-types.*".
spring.mvc.pathmatch.use-suffix-pattern=false # Whether to use suffix pattern match (".*") when matching patterns to requests.
spring.mvc.servlet.load-on-startup=-1 # Load on startup priority of the dispatcher servlet.
spring.mvc.static-path-pattern=/** # Path pattern used for static resources.
spring.mvc.throw-exception-if-no-handler-found=false # Whether a "NoHandlerFoundException" should be thrown if no Handler was found to process a request.
spring.mvc.view.prefix= # Spring MVC view prefix.
spring.mvc.view.suffix= # Spring MVC view suffix.

# SPRING RESOURCES HANDLING (ResourceProperties)
spring.resources.add-mappings=true # Whether to enable default resource handling.
spring.resources.cache.cachecontrol.cache-private= # Indicate that the response message is intended for a single user and must not be stored by a shared cache.
spring.resources.cache.cachecontrol.cache-public= # Indicate that any cache may store the response.
spring.resources.cache.cachecontrol.max-age= # Maximum time the response should be cached, in seconds if no duration suffix is not specified.
spring.resources.cache.cachecontrol.must-revalidate= # Indicate that once it has become stale, a cache must not use the response without re-validating it with the server.
spring.resources.cache.cachecontrol.no-cache= # Indicate that the cached response can be reused only if re-validated with the server.
spring.resources.cache.cachecontrol.no-store= # Indicate to not cache the response in any case.
spring.resources.cache.cachecontrol.no-transform= # Indicate intermediaries (caches and others) that they should not transform the response content.
spring.resources.cache.cachecontrol.proxy-revalidate= # Same meaning as the "must-revalidate" directive, except that it does not apply to private caches.
spring.resources.cache.cachecontrol.s-max-age= # Maximum time the response should be cached by shared caches, in seconds if no duration suffix is not specified.
spring.resources.cache.cachecontrol.stale-if-error= # Maximum time the response may be used when errors are encountered, in seconds if no duration suffix is not specified.
spring.resources.cache.cachecontrol.stale-while-revalidate= # Maximum time the response can be served after it becomes stale, in seconds if no duration suffix is not specified.
spring.resources.cache.period= # Cache period for the resources served by the resource handler. If a duration suffix is not specified, seconds will be used.
spring.resources.chain.cache=true # Whether to enable caching in the Resource chain.
spring.resources.chain.enabled= # Whether to enable the Spring Resource Handling chain. By default, disabled unless at least one strategy has been enabled.
spring.resources.chain.gzipped=false # Whether to enable resolution of already gzipped resources.
spring.resources.chain.html-application-cache=false # Whether to enable HTML5 application cache manifest rewriting.
spring.resources.chain.strategy.content.enabled=false # Whether to enable the content Version Strategy.
spring.resources.chain.strategy.content.paths=/** # Comma-separated list of patterns to apply to the content Version Strategy.
spring.resources.chain.strategy.fixed.enabled=false # Whether to enable the fixed Version Strategy.
spring.resources.chain.strategy.fixed.paths=/** # Comma-separated list of patterns to apply to the fixed Version Strategy.
spring.resources.chain.strategy.fixed.version= # Version string to use for the fixed Version Strategy.
spring.resources.static-locations=classpath:/META-INF/resources/,classpath:/resources/,classpath:/static/,classpath:/public/ # Locations of static resources.

# SPRING SESSION (SessionProperties)
spring.session.store-type= # Session store type.
spring.session.servlet.filter-order=-2147483598 # Session repository filter order.
spring.session.servlet.filter-dispatcher-types=async,error,request # Session repository filter dispatcher types.

# SPRING SESSION HAZELCAST (HazelcastSessionProperties)
spring.session.hazelcast.flush-mode=on-save # Sessions flush mode.
spring.session.hazelcast.map-name=spring:session:sessions # Name of the map used to store sessions.

# SPRING SESSION JDBC (JdbcSessionProperties)
spring.session.jdbc.cleanup-cron=0 * * * * * # Cron expression for expired session cleanup job.
spring.session.jdbc.initialize-schema=embedded # Database schema initialization mode.
spring.session.jdbc.schema=classpath:org/springframework/session/jdbc/schema-@@platform@@.sql # Path to the SQL file to use to initialize the database schema.
spring.session.jdbc.table-name=SPRING_SESSION # Name of the database table used to store sessions.

# SPRING SESSION MONGODB (MongoSessionProperties)
spring.session.mongodb.collection-name=sessions # Collection name used to store sessions.

# SPRING SESSION REDIS (RedisSessionProperties)
spring.session.redis.cleanup-cron=0 * * * * * # Cron expression for expired session cleanup job.
spring.session.redis.flush-mode=on-save # Sessions flush mode.
spring.session.redis.namespace=spring:session # Namespace for keys used to store sessions.

# THYMELEAF (ThymeleafAutoConfiguration)
spring.thymeleaf.cache=true # Whether to enable template caching.
spring.thymeleaf.check-template=true # Whether to check that the template exists before rendering it.
spring.thymeleaf.check-template-location=true # Whether to check that the templates location exists.
spring.thymeleaf.enabled=true # Whether to enable Thymeleaf view resolution for Web frameworks.
spring.thymeleaf.enable-spring-el-compiler=false # Enable the SpringEL compiler in SpringEL expressions.
spring.thymeleaf.encoding=UTF-8 # Template files encoding.
spring.thymeleaf.excluded-view-names= # Comma-separated list of view names (patterns allowed) that should be excluded from resolution.
spring.thymeleaf.mode=HTML # Template mode to be applied to templates. See also Thymeleaf's TemplateMode enum.
spring.thymeleaf.prefix=classpath:/templates/ # Prefix that gets prepended to view names when building a URL.
spring.thymeleaf.reactive.chunked-mode-view-names= # Comma-separated list of view names (patterns allowed) that should be the only ones executed in CHUNKED mode when a max chunk size is set.
spring.thymeleaf.reactive.full-mode-view-names= # Comma-separated list of view names (patterns allowed) that should be executed in FULL mode even if a max chunk size is set.
spring.thymeleaf.reactive.max-chunk-size=0 # Maximum size of data buffers used for writing to the response, in bytes.
spring.thymeleaf.reactive.media-types= # Media types supported by the view technology.
spring.thymeleaf.servlet.content-type=text/html # Content-Type value written to HTTP responses.
spring.thymeleaf.suffix=.html # Suffix that gets appended to view names when building a URL.
spring.thymeleaf.template-resolver-order= # Order of the template resolver in the chain.
spring.thymeleaf.view-names= # Comma-separated list of view names (patterns allowed) that can be resolved.

# SPRING WEBFLUX (WebFluxProperties)
spring.webflux.date-format= # Date format to use. For instance, `dd/MM/yyyy`.
spring.webflux.static-path-pattern=/** # Path pattern used for static resources.

# SPRING WEB SERVICES (WebServicesProperties)
spring.webservices.path=/services # Path that serves as the base URI for the services.
spring.webservices.servlet.init= # Servlet init parameters to pass to Spring Web Services.
spring.webservices.servlet.load-on-startup=-1 # Load on startup priority of the Spring Web Services servlet.
spring.webservices.wsdl-locations= # Comma-separated list of locations of WSDLs and accompanying XSDs to be exposed as beans.



# ----------------------------------------
# SECURITY PROPERTIES
# ----------------------------------------
# SECURITY (SecurityProperties)
spring.security.filter.order=-100 # Security filter chain order.
spring.security.filter.dispatcher-types=async,error,request # Security filter chain dispatcher types.
spring.security.user.name=user # Default user name.
spring.security.user.password= # Password for the default user name.
spring.security.user.roles= # Granted roles for the default user name.

# SECURITY OAUTH2 CLIENT (OAuth2ClientProperties)
spring.security.oauth2.client.provider.*= # OAuth provider details.
spring.security.oauth2.client.registration.*= # OAuth client registrations.

# ----------------------------------------
# DATA PROPERTIES
# ----------------------------------------

# FLYWAY (FlywayProperties)
spring.flyway.baseline-description= #
spring.flyway.baseline-on-migrate= #
spring.flyway.baseline-version=1 # Version to start migration
spring.flyway.check-location=true # Whether to check that migration scripts location exists.
spring.flyway.clean-disabled= #
spring.flyway.clean-on-validation-error= #
spring.flyway.dry-run-output= #
spring.flyway.enabled=true # Whether to enable flyway.
spring.flyway.encoding= #
spring.flyway.error-handlers= #
spring.flyway.group= #
spring.flyway.ignore-future-migrations= #
spring.flyway.ignore-missing-migrations= #
spring.flyway.init-sqls= # SQL statements to execute to initialize a connection immediately after obtaining it.
spring.flyway.installed-by= #
spring.flyway.locations=classpath:db/migration # The locations of migrations scripts.
spring.flyway.mixed= #
spring.flyway.out-of-order= #
spring.flyway.password= # JDBC password to use if you want Flyway to create its own DataSource.
spring.flyway.placeholder-prefix= #
spring.flyway.placeholder-replacement= #
spring.flyway.placeholder-suffix= #
spring.flyway.placeholders.*= #
spring.flyway.repeatable-sql-migration-prefix= #
spring.flyway.schemas= # schemas to update
spring.flyway.skip-default-callbacks= #
spring.flyway.skip-default-resolvers= #
spring.flyway.sql-migration-prefix=V #
spring.flyway.sql-migration-separator= #
spring.flyway.sql-migration-suffix=.sql #
spring.flyway.sql-migration-suffixes= #
spring.flyway.table= #
spring.flyway.target= #
spring.flyway.undo-sql-migration-prefix= #
spring.flyway.url= # JDBC url of the database to migrate. If not set, the primary configured data source is used.
spring.flyway.user= # Login user of the database to migrate.
spring.flyway.validate-on-migrate= #

# LIQUIBASE (LiquibaseProperties)
spring.liquibase.change-log=classpath:/db/changelog/db.changelog-master.yaml # Change log configuration path.
spring.liquibase.check-change-log-location=true # Whether to check that the change log location exists.
spring.liquibase.contexts= # Comma-separated list of runtime contexts to use.
spring.liquibase.default-schema= # Default database schema.
spring.liquibase.drop-first=false # Whether to first drop the database schema.
spring.liquibase.enabled=true # Whether to enable Liquibase support.
spring.liquibase.labels= # Comma-separated list of runtime labels to use.
spring.liquibase.parameters.*= # Change log parameters.
spring.liquibase.password= # Login password of the database to migrate.
spring.liquibase.rollback-file= # File to which rollback SQL is written when an update is performed.
spring.liquibase.url= # JDBC URL of the database to migrate. If not set, the primary configured data source is used.
spring.liquibase.user= # Login user of the database to migrate.

# COUCHBASE (CouchbaseProperties)
spring.couchbase.bootstrap-hosts= # Couchbase nodes (host or IP address) to bootstrap from.
spring.couchbase.bucket.name=default # Name of the bucket to connect to.
spring.couchbase.bucket.password=  # Password of the bucket.
spring.couchbase.env.endpoints.key-value=1 # Number of sockets per node against the key/value service.
spring.couchbase.env.endpoints.query=1 # Number of sockets per node against the query (N1QL) service.
spring.couchbase.env.endpoints.view=1 # Number of sockets per node against the view service.
spring.couchbase.env.ssl.enabled= # Whether to enable SSL support. Enabled automatically if a "keyStore" is provided unless specified otherwise.
spring.couchbase.env.ssl.key-store= # Path to the JVM key store that holds the certificates.
spring.couchbase.env.ssl.key-store-password= # Password used to access the key store.
spring.couchbase.env.timeouts.connect=5000ms # Bucket connections timeouts.
spring.couchbase.env.timeouts.key-value=2500ms # Blocking operations performed on a specific key timeout.
spring.couchbase.env.timeouts.query=7500ms # N1QL query operations timeout.
spring.couchbase.env.timeouts.socket-connect=1000ms # Socket connect connections timeout.
spring.couchbase.env.timeouts.view=7500ms # Regular and geospatial view operations timeout.

# DAO (PersistenceExceptionTranslationAutoConfiguration)
spring.dao.exceptiontranslation.enabled=true # Whether to enable the PersistenceExceptionTranslationPostProcessor.

# CASSANDRA (CassandraProperties)
spring.data.cassandra.cluster-name= # Name of the Cassandra cluster.
spring.data.cassandra.compression=none # Compression supported by the Cassandra binary protocol.
spring.data.cassandra.connect-timeout= # Socket option: connection time out.
spring.data.cassandra.consistency-level= # Queries consistency level.
spring.data.cassandra.contact-points=localhost # Cluster node addresses.
spring.data.cassandra.fetch-size= # Queries default fetch size.
spring.data.cassandra.keyspace-name= # Keyspace name to use.
spring.data.cassandra.load-balancing-policy= # Class name of the load balancing policy.
spring.data.cassandra.port= # Port of the Cassandra server.
spring.data.cassandra.password= # Login password of the server.
spring.data.cassandra.pool.heartbeat-interval=30s # Heartbeat interval after which a message is sent on an idle connection to make sure it's still alive. If a duration suffix is not specified, seconds will be used.
spring.data.cassandra.pool.idle-timeout=120s # Idle timeout before an idle connection is removed. If a duration suffix is not specified, seconds will be used.
spring.data.cassandra.pool.max-queue-size=256 # Maximum number of requests that get queued if no connection is available.
spring.data.cassandra.pool.pool-timeout=5000ms # Pool timeout when trying to acquire a connection from a host's pool.
spring.data.cassandra.read-timeout= # Socket option: read time out.
spring.data.cassandra.reconnection-policy= # Reconnection policy class.
spring.data.cassandra.repositories.type=auto # Type of Cassandra repositories to enable.
spring.data.cassandra.retry-policy= # Class name of the retry policy.
spring.data.cassandra.serial-consistency-level= # Queries serial consistency level.
spring.data.cassandra.schema-action=none # Schema action to take at startup.
spring.data.cassandra.ssl=false # Enable SSL support.
spring.data.cassandra.username= # Login user of the server.

# DATA COUCHBASE (CouchbaseDataProperties)
spring.data.couchbase.auto-index=false # Automatically create views and indexes.
spring.data.couchbase.consistency=read-your-own-writes # Consistency to apply by default on generated queries.
spring.data.couchbase.repositories.type=auto # Type of Couchbase repositories to enable.

# ELASTICSEARCH (ElasticsearchProperties)
spring.data.elasticsearch.cluster-name=elasticsearch # Elasticsearch cluster name.
spring.data.elasticsearch.cluster-nodes= # Comma-separated list of cluster node addresses.
spring.data.elasticsearch.properties.*= # Additional properties used to configure the client.
spring.data.elasticsearch.repositories.enabled=true # Whether to enable Elasticsearch repositories.

# DATA LDAP
spring.data.ldap.repositories.enabled=true # Whether to enable LDAP repositories.

# MONGODB (MongoProperties)
spring.data.mongodb.authentication-database= # Authentication database name.
spring.data.mongodb.database= # Database name.
spring.data.mongodb.field-naming-strategy= # Fully qualified name of the FieldNamingStrategy to use.
spring.data.mongodb.grid-fs-database= # GridFS database name.
spring.data.mongodb.host= # Mongo server host. Cannot be set with URI.
spring.data.mongodb.password= # Login password of the mongo server. Cannot be set with URI.
spring.data.mongodb.port= # Mongo server port. Cannot be set with URI.
spring.data.mongodb.repositories.type=auto # Type of Mongo repositories to enable.
spring.data.mongodb.uri=mongodb://localhost/test # Mongo database URI. Cannot be set with host, port and credentials.
spring.data.mongodb.username= # Login user of the mongo server. Cannot be set with URI.

# DATA REDIS
spring.data.redis.repositories.enabled=true # Whether to enable Redis repositories.

# NEO4J (Neo4jProperties)
spring.data.neo4j.auto-index=none # Auto index mode.
spring.data.neo4j.embedded.enabled=true # Whether to enable embedded mode if the embedded driver is available.
spring.data.neo4j.open-in-view=true # Register OpenSessionInViewInterceptor. Binds a Neo4j Session to the thread for the entire processing of the request.
spring.data.neo4j.password= # Login password of the server.
spring.data.neo4j.repositories.enabled=true # Whether to enable Neo4j repositories.
spring.data.neo4j.uri= # URI used by the driver. Auto-detected by default.
spring.data.neo4j.username= # Login user of the server.

# DATA REST (RepositoryRestProperties)
spring.data.rest.base-path= # Base path to be used by Spring Data REST to expose repository resources.
spring.data.rest.default-media-type= # Content type to use as a default when none is specified.
spring.data.rest.default-page-size= # Default size of pages.
spring.data.rest.detection-strategy=default # Strategy to use to determine which repositories get exposed.
spring.data.rest.enable-enum-translation= # Whether to enable enum value translation through the Spring Data REST default resource bundle.
spring.data.rest.limit-param-name= # Name of the URL query string parameter that indicates how many results to return at once.
spring.data.rest.max-page-size= # Maximum size of pages.
spring.data.rest.page-param-name= # Name of the URL query string parameter that indicates what page to return.
spring.data.rest.return-body-on-create= # Whether to return a response body after creating an entity.
spring.data.rest.return-body-on-update= # Whether to return a response body after updating an entity.
spring.data.rest.sort-param-name= # Name of the URL query string parameter that indicates what direction to sort results.

# SOLR (SolrProperties)
spring.data.solr.host=http://127.0.0.1:8983/solr # Solr host. Ignored if "zk-host" is set.
spring.data.solr.repositories.enabled=true # Whether to enable Solr repositories.
spring.data.solr.zk-host= # ZooKeeper host address in the form HOST:PORT.

# DATA WEB (SpringDataWebProperties)
spring.data.web.pageable.default-page-size=20 # Default page size.
spring.data.web.pageable.max-page-size=2000 # Maximum page size to be accepted.
spring.data.web.pageable.one-indexed-parameters=false # Whether to expose and assume 1-based page number indexes.
spring.data.web.pageable.page-parameter=page # Page index parameter name.
spring.data.web.pageable.prefix= # General prefix to be prepended to the page number and page size parameters.
spring.data.web.pageable.qualifier-delimiter=_ # Delimiter to be used between the qualifier and the actual page number and size properties.
spring.data.web.pageable.size-parameter=size # Page size parameter name.
spring.data.web.sort.sort-parameter=sort # Sort parameter name.

# DATASOURCE (DataSourceAutoConfiguration & DataSourceProperties)
spring.datasource.continue-on-error=false # Whether to stop if an error occurs while initializing the database.
spring.datasource.data= # Data (DML) script resource references.
spring.datasource.data-username= # Username of the database to execute DML scripts (if different).
spring.datasource.data-password= # Password of the database to execute DML scripts (if different).
spring.datasource.dbcp2.*= # Commons DBCP2 specific settings
spring.datasource.driver-class-name= # Fully qualified name of the JDBC driver. Auto-detected based on the URL by default.
spring.datasource.generate-unique-name=false # Whether to generate a random datasource name.
spring.datasource.hikari.*= # Hikari specific settings
spring.datasource.initialization-mode=embedded # Initialize the datasource with available DDL and DML scripts.
spring.datasource.jmx-enabled=false # Whether to enable JMX support (if provided by the underlying pool).
spring.datasource.jndi-name= # JNDI location of the datasource. Class, url, username & password are ignored when set.
spring.datasource.name= # Name of the datasource. Default to "testdb" when using an embedded database.
spring.datasource.password= # Login password of the database.
spring.datasource.platform=all # Platform to use in the DDL or DML scripts (such as schema-${platform}.sql or data-${platform}.sql).
spring.datasource.schema= # Schema (DDL) script resource references.
spring.datasource.schema-username= # Username of the database to execute DDL scripts (if different).
spring.datasource.schema-password= # Password of the database to execute DDL scripts (if different).
spring.datasource.separator=; # Statement separator in SQL initialization scripts.
spring.datasource.sql-script-encoding= # SQL scripts encoding.
spring.datasource.tomcat.*= # Tomcat datasource specific settings
spring.datasource.type= # Fully qualified name of the connection pool implementation to use. By default, it is auto-detected from the classpath.
spring.datasource.url= # JDBC URL of the database.
spring.datasource.username= # Login username of the database.
spring.datasource.xa.data-source-class-name= # XA datasource fully qualified name.
spring.datasource.xa.properties= # Properties to pass to the XA data source.

# JEST (Elasticsearch HTTP client) (JestProperties)
spring.elasticsearch.jest.connection-timeout=3s # Connection timeout.
spring.elasticsearch.jest.multi-threaded=true # Whether to enable connection requests from multiple execution threads.
spring.elasticsearch.jest.password= # Login password.
spring.elasticsearch.jest.proxy.host= # Proxy host the HTTP client should use.
spring.elasticsearch.jest.proxy.port= # Proxy port the HTTP client should use.
spring.elasticsearch.jest.read-timeout=3s # Read timeout.
spring.elasticsearch.jest.uris=http://localhost:9200 # Comma-separated list of the Elasticsearch instances to use.
spring.elasticsearch.jest.username= # Login username.

# H2 Web Console (H2ConsoleProperties)
spring.h2.console.enabled=false # Whether to enable the console.
spring.h2.console.path=/h2-console # Path at which the console is available.
spring.h2.console.settings.trace=false # Whether to enable trace output.
spring.h2.console.settings.web-allow-others=false # Whether to enable remote access.

# InfluxDB (InfluxDbProperties)
spring.influx.password= # Login password.
spring.influx.url= # URL of the InfluxDB instance to which to connect.
spring.influx.user= # Login user.

# JOOQ (JooqProperties)
spring.jooq.sql-dialect= # SQL dialect to use. Auto-detected by default.

# JDBC (JdbcProperties)
spring.jdbc.template.fetch-size=-1 # Number of rows that should be fetched from the database when more rows are needed.
spring.jdbc.template.max-rows=-1 # Maximum number of rows.
spring.jdbc.template.query-timeout= # Query timeout. Default is to use the JDBC driver's default configuration. If a duration suffix is not specified, seconds will be used.

# JPA (JpaBaseConfiguration, HibernateJpaAutoConfiguration)
spring.data.jpa.repositories.enabled=true # Whether to enable JPA repositories.
spring.jpa.database= # Target database to operate on, auto-detected by default. Can be alternatively set using the "databasePlatform" property.
spring.jpa.database-platform= # Name of the target database to operate on, auto-detected by default. Can be alternatively set using the "Database" enum.
spring.jpa.generate-ddl=false # Whether to initialize the schema on startup.
spring.jpa.hibernate.ddl-auto= # DDL mode. This is actually a shortcut for the "hibernate.hbm2ddl.auto" property. Defaults to "create-drop" when using an embedded database and no schema manager was detected. Otherwise, defaults to "none".
spring.jpa.hibernate.naming.implicit-strategy= # Fully qualified name of the implicit naming strategy.
spring.jpa.hibernate.naming.physical-strategy= # Fully qualified name of the physical naming strategy.
spring.jpa.hibernate.use-new-id-generator-mappings= # Whether to use Hibernate's newer IdentifierGenerator for AUTO, TABLE and SEQUENCE.
spring.jpa.mapping-resources= # Mapping resources (equivalent to "mapping-file" entries in persistence.xml).
spring.jpa.open-in-view=true # Register OpenEntityManagerInViewInterceptor. Binds a JPA EntityManager to the thread for the entire processing of the request.
spring.jpa.properties.*= # Additional native properties to set on the JPA provider.
spring.jpa.show-sql=false # Whether to enable logging of SQL statements.

# JTA (JtaAutoConfiguration)
spring.jta.enabled=true # Whether to enable JTA support.
spring.jta.log-dir= # Transaction logs directory.
spring.jta.transaction-manager-id= # Transaction manager unique identifier.

# ATOMIKOS (AtomikosProperties)
spring.jta.atomikos.connectionfactory.borrow-connection-timeout=30 # Timeout, in seconds, for borrowing connections from the pool.
spring.jta.atomikos.connectionfactory.ignore-session-transacted-flag=true # Whether to ignore the transacted flag when creating session.
spring.jta.atomikos.connectionfactory.local-transaction-mode=false # Whether local transactions are desired.
spring.jta.atomikos.connectionfactory.maintenance-interval=60 # The time, in seconds, between runs of the pool's maintenance thread.
spring.jta.atomikos.connectionfactory.max-idle-time=60 # The time, in seconds, after which connections are cleaned up from the pool.
spring.jta.atomikos.connectionfactory.max-lifetime=0 # The time, in seconds, that a connection can be pooled for before being destroyed. 0 denotes no limit.
spring.jta.atomikos.connectionfactory.max-pool-size=1 # The maximum size of the pool.
spring.jta.atomikos.connectionfactory.min-pool-size=1 # The minimum size of the pool.
spring.jta.atomikos.connectionfactory.reap-timeout=0 # The reap timeout, in seconds, for borrowed connections. 0 denotes no limit.
spring.jta.atomikos.connectionfactory.unique-resource-name=jmsConnectionFactory # The unique name used to identify the resource during recovery.
spring.jta.atomikos.connectionfactory.xa-connection-factory-class-name= # Vendor-specific implementation of XAConnectionFactory.
spring.jta.atomikos.connectionfactory.xa-properties= # Vendor-specific XA properties.
spring.jta.atomikos.datasource.borrow-connection-timeout=30 # Timeout, in seconds, for borrowing connections from the pool.
spring.jta.atomikos.datasource.concurrent-connection-validation= # Whether to use concurrent connection validation.
spring.jta.atomikos.datasource.default-isolation-level= # Default isolation level of connections provided by the pool.
spring.jta.atomikos.datasource.login-timeout= # Timeout, in seconds, for establishing a database connection.
spring.jta.atomikos.datasource.maintenance-interval=60 # The time, in seconds, between runs of the pool's maintenance thread.
spring.jta.atomikos.datasource.max-idle-time=60 # The time, in seconds, after which connections are cleaned up from the pool.
spring.jta.atomikos.datasource.max-lifetime=0 # The time, in seconds, that a connection can be pooled for before being destroyed. 0 denotes no limit.
spring.jta.atomikos.datasource.max-pool-size=1 # The maximum size of the pool.
spring.jta.atomikos.datasource.min-pool-size=1 # The minimum size of the pool.
spring.jta.atomikos.datasource.reap-timeout=0 # The reap timeout, in seconds, for borrowed connections. 0 denotes no limit.
spring.jta.atomikos.datasource.test-query= # SQL query or statement used to validate a connection before returning it.
spring.jta.atomikos.datasource.unique-resource-name=dataSource # The unique name used to identify the resource during recovery.
spring.jta.atomikos.datasource.xa-data-source-class-name= # Vendor-specific implementation of XAConnectionFactory.
spring.jta.atomikos.datasource.xa-properties= # Vendor-specific XA properties.
spring.jta.atomikos.properties.allow-sub-transactions=true # Specify whether sub-transactions are allowed.
spring.jta.atomikos.properties.checkpoint-interval=500 # Interval between checkpoints, expressed as the number of log writes between two checkpoint.
spring.jta.atomikos.properties.default-jta-timeout=10000ms # Default timeout for JTA transactions.
spring.jta.atomikos.properties.default-max-wait-time-on-shutdown=9223372036854775807 # How long should normal shutdown (no-force) wait for transactions to complete.
spring.jta.atomikos.properties.enable-logging=true # Whether to enable disk logging.
spring.jta.atomikos.properties.force-shutdown-on-vm-exit=false # Whether a VM shutdown should trigger forced shutdown of the transaction core.
spring.jta.atomikos.properties.log-base-dir= # Directory in which the log files should be stored.
spring.jta.atomikos.properties.log-base-name=tmlog # Transactions log file base name.
spring.jta.atomikos.properties.max-actives=50 # Maximum number of active transactions.
spring.jta.atomikos.properties.max-timeout=300000ms # Maximum timeout that can be allowed for transactions.
spring.jta.atomikos.properties.recovery.delay=10000ms # Delay between two recovery scans.
spring.jta.atomikos.properties.recovery.forget-orphaned-log-entries-delay=86400000ms # Delay after which recovery can cleanup pending ('orphaned') log entries.
spring.jta.atomikos.properties.recovery.max-retries=5 # Number of retry attempts to commit the transaction before throwing an exception.
spring.jta.atomikos.properties.recovery.retry-interval=10000ms # Delay between retry attempts.
spring.jta.atomikos.properties.serial-jta-transactions=true # Whether sub-transactions should be joined when possible.
spring.jta.atomikos.properties.service= # Transaction manager implementation that should be started.
spring.jta.atomikos.properties.threaded-two-phase-commit=false # Whether to use different (and concurrent) threads for two-phase commit on the participating resources.
spring.jta.atomikos.properties.transaction-manager-unique-name= # The transaction manager's unique name.

# BITRONIX
spring.jta.bitronix.connectionfactory.acquire-increment=1 # Number of connections to create when growing the pool.
spring.jta.bitronix.connectionfactory.acquisition-interval=1 # Time, in seconds, to wait before trying to acquire a connection again after an invalid connection was acquired.
spring.jta.bitronix.connectionfactory.acquisition-timeout=30 # Timeout, in seconds, for acquiring connections from the pool.
spring.jta.bitronix.connectionfactory.allow-local-transactions=true # Whether the transaction manager should allow mixing XA and non-XA transactions.
spring.jta.bitronix.connectionfactory.apply-transaction-timeout=false # Whether the transaction timeout should be set on the XAResource when it is enlisted.
spring.jta.bitronix.connectionfactory.automatic-enlisting-enabled=true # Whether resources should be enlisted and delisted automatically.
spring.jta.bitronix.connectionfactory.cache-producers-consumers=true # Whether producers and consumers should be cached.
spring.jta.bitronix.connectionfactory.class-name= # Underlying implementation class name of the XA resource.
spring.jta.bitronix.connectionfactory.defer-connection-release=true # Whether the provider can run many transactions on the same connection and supports transaction interleaving.
spring.jta.bitronix.connectionfactory.disabled= # Whether this resource is disabled, meaning it's temporarily forbidden to acquire a connection from its pool.
spring.jta.bitronix.connectionfactory.driver-properties= # Properties that should be set on the underlying implementation.
spring.jta.bitronix.connectionfactory.failed= # Mark this resource producer as failed.
spring.jta.bitronix.connectionfactory.ignore-recovery-failures=false # Whether recovery failures should be ignored.
spring.jta.bitronix.connectionfactory.max-idle-time=60 # The time, in seconds, after which connections are cleaned up from the pool.
spring.jta.bitronix.connectionfactory.max-pool-size=10 # The maximum size of the pool. 0 denotes no limit.
spring.jta.bitronix.connectionfactory.min-pool-size=0 # The minimum size of the pool.
spring.jta.bitronix.connectionfactory.password= # The password to use to connect to the JMS provider.
spring.jta.bitronix.connectionfactory.share-transaction-connections=false #  Whether connections in the ACCESSIBLE state can be shared within the context of a transaction.
spring.jta.bitronix.connectionfactory.test-connections=true # Whether connections should be tested when acquired from the pool.
spring.jta.bitronix.connectionfactory.two-pc-ordering-position=1 # The position that this resource should take during two-phase commit (always first is Integer.MIN_VALUE, always last is Integer.MAX_VALUE).
spring.jta.bitronix.connectionfactory.unique-name=jmsConnectionFactory # The unique name used to identify the resource during recovery.
spring.jta.bitronix.connectionfactory.use-tm-join=true # Whether TMJOIN should be used when starting XAResources.
spring.jta.bitronix.connectionfactory.user= # The user to use to connect to the JMS provider.
spring.jta.bitronix.datasource.acquire-increment=1 # Number of connections to create when growing the pool.
spring.jta.bitronix.datasource.acquisition-interval=1 # Time, in seconds, to wait before trying to acquire a connection again after an invalid connection was acquired.
spring.jta.bitronix.datasource.acquisition-timeout=30 # Timeout, in seconds, for acquiring connections from the pool.
spring.jta.bitronix.datasource.allow-local-transactions=true # Whether the transaction manager should allow mixing XA and non-XA transactions.
spring.jta.bitronix.datasource.apply-transaction-timeout=false # Whether the transaction timeout should be set on the XAResource when it is enlisted.
spring.jta.bitronix.datasource.automatic-enlisting-enabled=true # Whether resources should be enlisted and delisted automatically.
spring.jta.bitronix.datasource.class-name= # Underlying implementation class name of the XA resource.
spring.jta.bitronix.datasource.cursor-holdability= # The default cursor holdability for connections.
spring.jta.bitronix.datasource.defer-connection-release=true # Whether the database can run many transactions on the same connection and supports transaction interleaving.
spring.jta.bitronix.datasource.disabled= # Whether this resource is disabled, meaning it's temporarily forbidden to acquire a connection from its pool.
spring.jta.bitronix.datasource.driver-properties= # Properties that should be set on the underlying implementation.
spring.jta.bitronix.datasource.enable-jdbc4-connection-test= # Whether Connection.isValid() is called when acquiring a connection from the pool.
spring.jta.bitronix.datasource.failed= # Mark this resource producer as failed.
spring.jta.bitronix.datasource.ignore-recovery-failures=false # Whether recovery failures should be ignored.
spring.jta.bitronix.datasource.isolation-level= # The default isolation level for connections.
spring.jta.bitronix.datasource.local-auto-commit= # The default auto-commit mode for local transactions.
spring.jta.bitronix.datasource.login-timeout= # Timeout, in seconds, for establishing a database connection.
spring.jta.bitronix.datasource.max-idle-time=60 # The time, in seconds, after which connections are cleaned up from the pool.
spring.jta.bitronix.datasource.max-pool-size=10 # The maximum size of the pool. 0 denotes no limit.
spring.jta.bitronix.datasource.min-pool-size=0 # The minimum size of the pool.
spring.jta.bitronix.datasource.prepared-statement-cache-size=0 # The target size of the prepared statement cache. 0 disables the cache.
spring.jta.bitronix.datasource.share-transaction-connections=false #  Whether connections in the ACCESSIBLE state can be shared within the context of a transaction.
spring.jta.bitronix.datasource.test-query= # SQL query or statement used to validate a connection before returning it.
spring.jta.bitronix.datasource.two-pc-ordering-position=1 # The position that this resource should take during two-phase commit (always first is Integer.MIN_VALUE, and always last is Integer.MAX_VALUE).
spring.jta.bitronix.datasource.unique-name=dataSource # The unique name used to identify the resource during recovery.
spring.jta.bitronix.datasource.use-tm-join=true # Whether TMJOIN should be used when starting XAResources.
spring.jta.bitronix.properties.allow-multiple-lrc=false # Whether to allow multiple LRC resources to be enlisted into the same transaction.
spring.jta.bitronix.properties.asynchronous2-pc=false # Whether to enable asynchronously execution of two phase commit.
spring.jta.bitronix.properties.background-recovery-interval-seconds=60 # Interval in seconds at which to run the recovery process in the background.
spring.jta.bitronix.properties.current-node-only-recovery=true # Whether to recover only the current node.
spring.jta.bitronix.properties.debug-zero-resource-transaction=false # Whether to log the creation and commit call stacks of transactions executed without a single enlisted resource.
spring.jta.bitronix.properties.default-transaction-timeout=60 # Default transaction timeout, in seconds.
spring.jta.bitronix.properties.disable-jmx=false # Whether to enable JMX support.
spring.jta.bitronix.properties.exception-analyzer= # Set the fully qualified name of the exception analyzer implementation to use.
spring.jta.bitronix.properties.filter-log-status=false # Whether to enable filtering of logs so that only mandatory logs are written.
spring.jta.bitronix.properties.force-batching-enabled=true #  Whether disk forces are batched.
spring.jta.bitronix.properties.forced-write-enabled=true # Whether logs are forced to disk.
spring.jta.bitronix.properties.graceful-shutdown-interval=60 # Maximum amount of seconds the TM waits for transactions to get done before aborting them at shutdown time.
spring.jta.bitronix.properties.jndi-transaction-synchronization-registry-name= # JNDI name of the TransactionSynchronizationRegistry.
spring.jta.bitronix.properties.jndi-user-transaction-name= # JNDI name of the UserTransaction.
spring.jta.bitronix.properties.journal=disk # Name of the journal. Can be 'disk', 'null', or a class name.
spring.jta.bitronix.properties.log-part1-filename=btm1.tlog # Name of the first fragment of the journal.
spring.jta.bitronix.properties.log-part2-filename=btm2.tlog # Name of the second fragment of the journal.
spring.jta.bitronix.properties.max-log-size-in-mb=2 # Maximum size in megabytes of the journal fragments.
spring.jta.bitronix.properties.resource-configuration-filename= # ResourceLoader configuration file name.
spring.jta.bitronix.properties.server-id= # ASCII ID that must uniquely identify this TM instance. Defaults to the machine's IP address.
spring.jta.bitronix.properties.skip-corrupted-logs=false # Skip corrupted transactions log entries.
spring.jta.bitronix.properties.warn-about-zero-resource-transaction=true # Whether to log a warning for transactions executed without a single enlisted resource.

# NARAYANA (NarayanaProperties)
spring.jta.narayana.default-timeout=60s # Transaction timeout. If a duration suffix is not specified, seconds will be used.
spring.jta.narayana.expiry-scanners=com.arjuna.ats.internal.arjuna.recovery.ExpiredTransactionStatusManagerScanner # Comma-separated list of expiry scanners.
spring.jta.narayana.log-dir= # Transaction object store directory.
spring.jta.narayana.one-phase-commit=true # Whether to enable one phase commit optimization.
spring.jta.narayana.periodic-recovery-period=120s # Interval in which periodic recovery scans are performed. If a duration suffix is not specified, seconds will be used.
spring.jta.narayana.recovery-backoff-period=10s # Back off period between first and second phases of the recovery scan. If a duration suffix is not specified, seconds will be used.
spring.jta.narayana.recovery-db-pass= # Database password to be used by the recovery manager.
spring.jta.narayana.recovery-db-user= # Database username to be used by the recovery manager.
spring.jta.narayana.recovery-jms-pass= # JMS password to be used by the recovery manager.
spring.jta.narayana.recovery-jms-user= # JMS username to be used by the recovery manager.
spring.jta.narayana.recovery-modules= # Comma-separated list of recovery modules.
spring.jta.narayana.transaction-manager-id=1 # Unique transaction manager id.
spring.jta.narayana.xa-resource-orphan-filters= # Comma-separated list of orphan filters.

# EMBEDDED MONGODB (EmbeddedMongoProperties)
spring.mongodb.embedded.features=sync_delay # Comma-separated list of features to enable.
spring.mongodb.embedded.storage.database-dir= # Directory used for data storage.
spring.mongodb.embedded.storage.oplog-size= # Maximum size of the oplog, in megabytes.
spring.mongodb.embedded.storage.repl-set-name= # Name of the replica set.
spring.mongodb.embedded.version=3.2.2 # Version of Mongo to use.

# REDIS (RedisProperties)
spring.redis.cluster.max-redirects= # Maximum number of redirects to follow when executing commands across the cluster.
spring.redis.cluster.nodes= # Comma-separated list of "host:port" pairs to bootstrap from.
spring.redis.database=0 # Database index used by the connection factory.
spring.redis.url= # Connection URL. Overrides host, port, and password. User is ignored. Example: redis://user:password@example.com:6379
spring.redis.host=localhost # Redis server host.
spring.redis.jedis.pool.max-active=8 # Maximum number of connections that can be allocated by the pool at a given time. Use a negative value for no limit.
spring.redis.jedis.pool.max-idle=8 # Maximum number of "idle" connections in the pool. Use a negative value to indicate an unlimited number of idle connections.
spring.redis.jedis.pool.max-wait=-1ms # Maximum amount of time a connection allocation should block before throwing an exception when the pool is exhausted. Use a negative value to block indefinitely.
spring.redis.jedis.pool.min-idle=0 # Target for the minimum number of idle connections to maintain in the pool. This setting only has an effect if it is positive.
spring.redis.lettuce.pool.max-active=8 # Maximum number of connections that can be allocated by the pool at a given time. Use a negative value for no limit.
spring.redis.lettuce.pool.max-idle=8 # Maximum number of "idle" connections in the pool. Use a negative value to indicate an unlimited number of idle connections.
spring.redis.lettuce.pool.max-wait=-1ms # Maximum amount of time a connection allocation should block before throwing an exception when the pool is exhausted. Use a negative value to block indefinitely.
spring.redis.lettuce.pool.min-idle=0 # Target for the minimum number of idle connections to maintain in the pool. This setting only has an effect if it is positive.
spring.redis.lettuce.shutdown-timeout=100ms # Shutdown timeout.
spring.redis.password= # Login password of the redis server.
spring.redis.port=6379 # Redis server port.
spring.redis.sentinel.master= # Name of the Redis server.
spring.redis.sentinel.nodes= # Comma-separated list of "host:port" pairs.
spring.redis.ssl=false # Whether to enable SSL support.
spring.redis.timeout= # Connection timeout.

# TRANSACTION (TransactionProperties)
spring.transaction.default-timeout= # Default transaction timeout. If a duration suffix is not specified, seconds will be used.
spring.transaction.rollback-on-commit-failure= # Whether to roll back on commit failures.



# ----------------------------------------
# INTEGRATION PROPERTIES
# ----------------------------------------

# ACTIVEMQ (ActiveMQProperties)
spring.activemq.broker-url= # URL of the ActiveMQ broker. Auto-generated by default.
spring.activemq.close-timeout=15s # Time to wait before considering a close complete.
spring.activemq.in-memory=true # Whether the default broker URL should be in memory. Ignored if an explicit broker has been specified.
spring.activemq.non-blocking-redelivery=false # Whether to stop message delivery before re-delivering messages from a rolled back transaction. This implies that message order is not preserved when this is enabled.
spring.activemq.password= # Login password of the broker.
spring.activemq.send-timeout=0ms # Time to wait on message sends for a response. Set it to 0 to wait forever.
spring.activemq.user= # Login user of the broker.
spring.activemq.packages.trust-all= # Whether to trust all packages.
spring.activemq.packages.trusted= # Comma-separated list of specific packages to trust (when not trusting all packages).
spring.activemq.pool.block-if-full=true # Whether to block when a connection is requested and the pool is full. Set it to false to throw a "JMSException" instead.
spring.activemq.pool.block-if-full-timeout=-1ms # Blocking period before throwing an exception if the pool is still full.
spring.activemq.pool.create-connection-on-startup=true # Whether to create a connection on startup. Can be used to warm up the pool on startup.
spring.activemq.pool.enabled=false # Whether a PooledConnectionFactory should be created, instead of a regular ConnectionFactory.
spring.activemq.pool.expiry-timeout=0ms # Connection expiration timeout.
spring.activemq.pool.idle-timeout=30s # Connection idle timeout.
spring.activemq.pool.max-connections=1 # Maximum number of pooled connections.
spring.activemq.pool.maximum-active-session-per-connection=500 # Maximum number of active sessions per connection.
spring.activemq.pool.reconnect-on-exception=true # Reset the connection when a "JMSException" occurs.
spring.activemq.pool.time-between-expiration-check=-1ms # Time to sleep between runs of the idle connection eviction thread. When negative, no idle connection eviction thread runs.
spring.activemq.pool.use-anonymous-producers=true # Whether to use only one anonymous "MessageProducer" instance. Set it to false to create one "MessageProducer" every time one is required.

# ARTEMIS (ArtemisProperties)
spring.artemis.embedded.cluster-password= # Cluster password. Randomly generated on startup by default.
spring.artemis.embedded.data-directory= # Journal file directory. Not necessary if persistence is turned off.
spring.artemis.embedded.enabled=true # Whether to enable embedded mode if the Artemis server APIs are available.
spring.artemis.embedded.persistent=false # Whether to enable persistent store.
spring.artemis.embedded.queues= # Comma-separated list of queues to create on startup.
spring.artemis.embedded.server-id= # Server ID. By default, an auto-incremented counter is used.
spring.artemis.embedded.topics= # Comma-separated list of topics to create on startup.
spring.artemis.host=localhost # Artemis broker host.
spring.artemis.mode= # Artemis deployment mode, auto-detected by default.
spring.artemis.password= # Login password of the broker.
spring.artemis.port=61616 # Artemis broker port.
spring.artemis.user= # Login user of the broker.

# SPRING BATCH (BatchProperties)
spring.batch.initialize-schema=embedded # Database schema initialization mode.
spring.batch.job.enabled=true # Execute all Spring Batch jobs in the context on startup.
spring.batch.job.names= # Comma-separated list of job names to execute on startup (for instance, `job1,job2`). By default, all Jobs found in the context are executed.
spring.batch.schema=classpath:org/springframework/batch/core/schema-@@platform@@.sql # Path to the SQL file to use to initialize the database schema.
spring.batch.table-prefix= # Table prefix for all the batch meta-data tables.

# SPRING INTEGRATION (IntegrationProperties)
spring.integration.jdbc.initialize-schema=embedded # Database schema initialization mode.
spring.integration.jdbc.schema=classpath:org/springframework/integration/jdbc/schema-@@platform@@.sql # Path to the SQL file to use to initialize the database schema.

# JMS (JmsProperties)
spring.jms.jndi-name= # Connection factory JNDI name. When set, takes precedence to others connection factory auto-configurations.
spring.jms.listener.acknowledge-mode= # Acknowledge mode of the container. By default, the listener is transacted with automatic acknowledgment.
spring.jms.listener.auto-startup=true # Start the container automatically on startup.
spring.jms.listener.concurrency= # Minimum number of concurrent consumers.
spring.jms.listener.max-concurrency= # Maximum number of concurrent consumers.
spring.jms.pub-sub-domain=false # Whether the default destination type is topic.
spring.jms.template.default-destination= # Default destination to use on send and receive operations that do not have a destination parameter.
spring.jms.template.delivery-delay= # Delivery delay to use for send calls.
spring.jms.template.delivery-mode= # Delivery mode. Enables QoS (Quality of Service) when set.
spring.jms.template.priority= # Priority of a message when sending. Enables QoS (Quality of Service) when set.
spring.jms.template.qos-enabled= # Whether to enable explicit QoS (Quality of Service) when sending a message.
spring.jms.template.receive-timeout= # Timeout to use for receive calls.
spring.jms.template.time-to-live= # Time-to-live of a message when sending. Enables QoS (Quality of Service) when set.

# APACHE KAFKA (KafkaProperties)
spring.kafka.admin.client-id= # ID to pass to the server when making requests. Used for server-side logging.
spring.kafka.admin.fail-fast=false # Whether to fail fast if the broker is not available on startup.
spring.kafka.admin.properties.*= # Additional admin-specific properties used to configure the client.
spring.kafka.admin.ssl.key-password= # Password of the private key in the key store file.
spring.kafka.admin.ssl.keystore-location= # Location of the key store file.
spring.kafka.admin.ssl.keystore-password= # Store password for the key store file.
spring.kafka.admin.ssl.truststore-location= # Location of the trust store file.
spring.kafka.admin.ssl.truststore-password= # Store password for the trust store file.
spring.kafka.bootstrap-servers= # Comma-delimited list of host:port pairs to use for establishing the initial connection to the Kafka cluster.
spring.kafka.client-id= # ID to pass to the server when making requests. Used for server-side logging.
spring.kafka.consumer.auto-commit-interval= # Frequency with which the consumer offsets are auto-committed to Kafka if 'enable.auto.commit' is set to true.
spring.kafka.consumer.auto-offset-reset= # What to do when there is no initial offset in Kafka or if the current offset no longer exists on the server.
spring.kafka.consumer.bootstrap-servers= # Comma-delimited list of host:port pairs to use for establishing the initial connection to the Kafka cluster.
spring.kafka.consumer.client-id= # ID to pass to the server when making requests. Used for server-side logging.
spring.kafka.consumer.enable-auto-commit= # Whether the consumer's offset is periodically committed in the background.
spring.kafka.consumer.fetch-max-wait= # Maximum amount of time the server blocks before answering the fetch request if there isn't sufficient data to immediately satisfy the requirement given by "fetch.min.bytes".
spring.kafka.consumer.fetch-min-size= # Minimum amount of data, in bytes, the server should return for a fetch request.
spring.kafka.consumer.group-id= # Unique string that identifies the consumer group to which this consumer belongs.
spring.kafka.consumer.heartbeat-interval= # Expected time between heartbeats to the consumer coordinator.
spring.kafka.consumer.key-deserializer= # Deserializer class for keys.
spring.kafka.consumer.max-poll-records= # Maximum number of records returned in a single call to poll().
spring.kafka.consumer.properties.*= # Additional consumer-specific properties used to configure the client.
spring.kafka.consumer.ssl.key-password= # Password of the private key in the key store file.
spring.kafka.consumer.ssl.keystore-location= # Location of the key store file.
spring.kafka.consumer.ssl.keystore-password= # Store password for the key store file.
spring.kafka.consumer.ssl.truststore-location= # Location of the trust store file.
spring.kafka.consumer.ssl.truststore-password= # Store password for the trust store file.
spring.kafka.consumer.value-deserializer= # Deserializer class for values.
spring.kafka.jaas.control-flag=required # Control flag for login configuration.
spring.kafka.jaas.enabled=false # Whether to enable JAAS configuration.
spring.kafka.jaas.login-module=com.sun.security.auth.module.Krb5LoginModule # Login module.
spring.kafka.jaas.options= # Additional JAAS options.
spring.kafka.listener.ack-count= # Number of records between offset commits when ackMode is "COUNT" or "COUNT_TIME".
spring.kafka.listener.ack-mode= # Listener AckMode. See the spring-kafka documentation.
spring.kafka.listener.ack-time= # Time between offset commits when ackMode is "TIME" or "COUNT_TIME".
spring.kafka.listener.client-id= # Prefix for the listener's consumer client.id property.
spring.kafka.listener.concurrency= # Number of threads to run in the listener containers.
spring.kafka.listener.idle-event-interval= # Time between publishing idle consumer events (no data received).
spring.kafka.listener.log-container-config= # Whether to log the container configuration during initialization (INFO level).
spring.kafka.listener.monitor-interval= # Time between checks for non-responsive consumers. If a duration suffix is not specified, seconds will be used.
spring.kafka.listener.no-poll-threshold= # Multiplier applied to "pollTimeout" to determine if a consumer is non-responsive.
spring.kafka.listener.poll-timeout= # Timeout to use when polling the consumer.
spring.kafka.listener.type=single # Listener type.
spring.kafka.producer.acks= # Number of acknowledgments the producer requires the leader to have received before considering a request complete.
spring.kafka.producer.batch-size= # Number of records to batch before sending.
spring.kafka.producer.bootstrap-servers= # Comma-delimited list of host:port pairs to use for establishing the initial connection to the Kafka cluster.
spring.kafka.producer.buffer-memory= # Total bytes of memory the producer can use to buffer records waiting to be sent to the server.
spring.kafka.producer.client-id= # ID to pass to the server when making requests. Used for server-side logging.
spring.kafka.producer.compression-type= # Compression type for all data generated by the producer.
spring.kafka.producer.key-serializer= # Serializer class for keys.
spring.kafka.producer.properties.*= # Additional producer-specific properties used to configure the client.
spring.kafka.producer.retries= # When greater than zero, enables retrying of failed sends.
spring.kafka.producer.ssl.key-password= # Password of the private key in the key store file.
spring.kafka.producer.ssl.keystore-location= # Location of the key store file.
spring.kafka.producer.ssl.keystore-password= # Store password for the key store file.
spring.kafka.producer.ssl.truststore-location= # Location of the trust store file.
spring.kafka.producer.ssl.truststore-password= # Store password for the trust store file.
spring.kafka.producer.transaction-id-prefix= # When non empty, enables transaction support for producer.
spring.kafka.producer.value-serializer= # Serializer class for values.
spring.kafka.properties.*= # Additional properties, common to producers and consumers, used to configure the client.
spring.kafka.ssl.key-password= # Password of the private key in the key store file.
spring.kafka.ssl.keystore-location= # Location of the key store file.
spring.kafka.ssl.keystore-password= # Store password for the key store file.
spring.kafka.ssl.truststore-location= # Location of the trust store file.
spring.kafka.ssl.truststore-password= # Store password for the trust store file.
spring.kafka.template.default-topic= # Default topic to which messages are sent.

# RABBIT (RabbitProperties)
spring.rabbitmq.addresses= # Comma-separated list of addresses to which the client should connect.
spring.rabbitmq.cache.channel.checkout-timeout= # Duration to wait to obtain a channel if the cache size has been reached.
spring.rabbitmq.cache.channel.size= # Number of channels to retain in the cache.
spring.rabbitmq.cache.connection.mode=channel # Connection factory cache mode.
spring.rabbitmq.cache.connection.size= # Number of connections to cache.
spring.rabbitmq.connection-timeout= # Connection timeout. Set it to zero to wait forever.
spring.rabbitmq.dynamic=true # Whether to create an AmqpAdmin bean.
spring.rabbitmq.host=localhost # RabbitMQ host.
spring.rabbitmq.listener.direct.acknowledge-mode= # Acknowledge mode of container.
spring.rabbitmq.listener.direct.auto-startup=true # Whether to start the container automatically on startup.
spring.rabbitmq.listener.direct.consumers-per-queue= # Number of consumers per queue.
spring.rabbitmq.listener.direct.default-requeue-rejected= # Whether rejected deliveries are re-queued by default.
spring.rabbitmq.listener.direct.idle-event-interval= # How often idle container events should be published.
spring.rabbitmq.listener.direct.prefetch= # Number of messages to be handled in a single request. It should be greater than or equal to the transaction size (if used).
spring.rabbitmq.listener.direct.retry.enabled=false # Whether publishing retries are enabled.
spring.rabbitmq.listener.direct.retry.initial-interval=1000ms # Duration between the first and second attempt to deliver a message.
spring.rabbitmq.listener.direct.retry.max-attempts=3 # Maximum number of attempts to deliver a message.
spring.rabbitmq.listener.direct.retry.max-interval=10000ms # Maximum duration between attempts.
spring.rabbitmq.listener.direct.retry.multiplier=1 # Multiplier to apply to the previous retry interval.
spring.rabbitmq.listener.direct.retry.stateless=true # Whether retries are stateless or stateful.
spring.rabbitmq.listener.simple.acknowledge-mode= # Acknowledge mode of container.
spring.rabbitmq.listener.simple.auto-startup=true # Whether to start the container automatically on startup.
spring.rabbitmq.listener.simple.concurrency= # Minimum number of listener invoker threads.
spring.rabbitmq.listener.simple.default-requeue-rejected= # Whether rejected deliveries are re-queued by default.
spring.rabbitmq.listener.simple.idle-event-interval= # How often idle container events should be published.
spring.rabbitmq.listener.simple.max-concurrency= # Maximum number of listener invoker threads.
spring.rabbitmq.listener.simple.prefetch= # Number of messages to be handled in a single request. It should be greater than or equal to the transaction size (if used).
spring.rabbitmq.listener.simple.retry.enabled=false # Whether publishing retries are enabled.
spring.rabbitmq.listener.simple.retry.initial-interval=1000ms # Duration between the first and second attempt to deliver a message.
spring.rabbitmq.listener.simple.retry.max-attempts=3 # Maximum number of attempts to deliver a message.
spring.rabbitmq.listener.simple.retry.max-interval=10000ms #  Maximum duration between attempts.
spring.rabbitmq.listener.simple.retry.multiplier=1 # Multiplier to apply to the previous retry interval.
spring.rabbitmq.listener.simple.retry.stateless=true # Whether retries are stateless or stateful.
spring.rabbitmq.listener.simple.transaction-size= # Number of messages to be processed in a transaction. That is, the number of messages between acks. For best results, it should be less than or equal to the prefetch count.
spring.rabbitmq.listener.type=simple # Listener container type.
spring.rabbitmq.password=guest # Login to authenticate against the broker.
spring.rabbitmq.port=5672 # RabbitMQ port.
spring.rabbitmq.publisher-confirms=false # Whether to enable publisher confirms.
spring.rabbitmq.publisher-returns=false # Whether to enable publisher returns.
spring.rabbitmq.requested-heartbeat= # Requested heartbeat timeout; zero for none. If a duration suffix is not specified, seconds will be used.
spring.rabbitmq.ssl.enabled=false # Whether to enable SSL support.
spring.rabbitmq.ssl.key-store= # Path to the key store that holds the SSL certificate.
spring.rabbitmq.ssl.key-store-password= # Password used to access the key store.
spring.rabbitmq.ssl.key-store-type=PKCS12 # Key store type.
spring.rabbitmq.ssl.trust-store= # Trust store that holds SSL certificates.
spring.rabbitmq.ssl.trust-store-password= # Password used to access the trust store.
spring.rabbitmq.ssl.trust-store-type=JKS # Trust store type.
spring.rabbitmq.ssl.algorithm= # SSL algorithm to use. By default, configured by the Rabbit client library.
spring.rabbitmq.template.exchange= # Name of the default exchange to use for send operations.
spring.rabbitmq.template.mandatory= # Whether to enable mandatory messages.
spring.rabbitmq.template.receive-timeout= # Timeout for `receive()` operations.
spring.rabbitmq.template.reply-timeout= # Timeout for `sendAndReceive()` operations.
spring.rabbitmq.template.retry.enabled=false # Whether publishing retries are enabled.
spring.rabbitmq.template.retry.initial-interval=1000ms # Duration between the first and second attempt to deliver a message.
spring.rabbitmq.template.retry.max-attempts=3 # Maximum number of attempts to deliver a message.
spring.rabbitmq.template.retry.max-interval=10000ms # Maximum duration between attempts.
spring.rabbitmq.template.retry.multiplier=1 # Multiplier to apply to the previous retry interval.
spring.rabbitmq.template.routing-key= # Value of a default routing key to use for send operations.
spring.rabbitmq.username=guest # Login user to authenticate to the broker.
spring.rabbitmq.virtual-host= # Virtual host to use when connecting to the broker.


# ----------------------------------------
# ACTUATOR PROPERTIES
# ----------------------------------------

# MANAGEMENT HTTP SERVER (ManagementServerProperties)
management.server.add-application-context-header=false # Add the "X-Application-Context" HTTP header in each response.
management.server.address= # Network address to which the management endpoints should bind. Requires a custom management.server.port.
management.server.port= # Management endpoint HTTP port (uses the same port as the application by default). Configure a different port to use management-specific SSL.
management.server.servlet.context-path= # Management endpoint context-path (for instance, `/management`). Requires a custom management.server.port.
management.server.ssl.ciphers= # Supported SSL ciphers. Requires a custom management.port.
management.server.ssl.client-auth= # Whether client authentication is wanted ("want") or needed ("need"). Requires a trust store. Requires a custom management.server.port.
management.server.ssl.enabled= # Whether to enable SSL support. Requires a custom management.server.port.
management.server.ssl.enabled-protocols= # Enabled SSL protocols. Requires a custom management.server.port.
management.server.ssl.key-alias= # Alias that identifies the key in the key store. Requires a custom management.server.port.
management.server.ssl.key-password= # Password used to access the key in the key store. Requires a custom management.server.port.
management.server.ssl.key-store= # Path to the key store that holds the SSL certificate (typically a jks file). Requires a custom management.server.port.
management.server.ssl.key-store-password= # Password used to access the key store. Requires a custom management.server.port.
management.server.ssl.key-store-provider= # Provider for the key store. Requires a custom management.server.port.
management.server.ssl.key-store-type= # Type of the key store. Requires a custom management.server.port.
management.server.ssl.protocol=TLS # SSL protocol to use. Requires a custom management.server.port.
management.server.ssl.trust-store= # Trust store that holds SSL certificates. Requires a custom management.server.port.
management.server.ssl.trust-store-password= # Password used to access the trust store. Requires a custom management.server.port.
management.server.ssl.trust-store-provider= # Provider for the trust store. Requires a custom management.server.port.
management.server.ssl.trust-store-type= # Type of the trust store. Requires a custom management.server.port.

# CLOUDFOUNDRY
management.cloudfoundry.enabled=true # Whether to enable extended Cloud Foundry actuator endpoints.
management.cloudfoundry.skip-ssl-validation=false # Whether to skip SSL verification for Cloud Foundry actuator endpoint security calls.

# ENDPOINTS GENERAL CONFIGURATION
management.endpoints.enabled-by-default= # Whether to enable or disable all endpoints by default.

# ENDPOINTS JMX CONFIGURATION (JmxEndpointProperties)
management.endpoints.jmx.domain=org.springframework.boot # Endpoints JMX domain name. Fallback to 'spring.jmx.default-domain' if set.
management.endpoints.jmx.exposure.include=* # Endpoint IDs that should be included or '*' for all.
management.endpoints.jmx.exposure.exclude= # Endpoint IDs that should be excluded.
management.endpoints.jmx.static-names= # Additional static properties to append to all ObjectNames of MBeans representing Endpoints.
management.endpoints.jmx.unique-names=false # Whether to ensure that ObjectNames are modified in case of conflict.

# ENDPOINTS WEB CONFIGURATION (WebEndpointProperties)
management.endpoints.web.exposure.include=health,info # Endpoint IDs that should be included or '*' for all.
management.endpoints.web.exposure.exclude= # Endpoint IDs that should be excluded.
management.endpoints.web.base-path=/actuator # Base path for Web endpoints. Relative to server.servlet.context-path or management.server.servlet.context-path if management.server.port is configured.
management.endpoints.web.path-mapping= # Mapping between endpoint IDs and the path that should expose them.

# ENDPOINTS CORS CONFIGURATION (CorsEndpointProperties)
management.endpoints.web.cors.allow-credentials= # Whether credentials are supported. When not set, credentials are not supported.
management.endpoints.web.cors.allowed-headers= # Comma-separated list of headers to allow in a request. '*' allows all headers.
management.endpoints.web.cors.allowed-methods= # Comma-separated list of methods to allow. '*' allows all methods. When not set, defaults to GET.
management.endpoints.web.cors.allowed-origins= # Comma-separated list of origins to allow. '*' allows all origins. When not set, CORS support is disabled.
management.endpoints.web.cors.exposed-headers= # Comma-separated list of headers to include in a response.
management.endpoints.web.cors.max-age=1800s # How long the response from a pre-flight request can be cached by clients. If a duration suffix is not specified, seconds will be used.

# AUDIT EVENTS ENDPOINT (AuditEventsEndpoint)
management.endpoint.auditevents.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.auditevents.enabled=true # Whether to enable the auditevents endpoint.

# BEANS ENDPOINT (BeansEndpoint)
management.endpoint.beans.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.beans.enabled=true # Whether to enable the beans endpoint.

# CONDITIONS REPORT ENDPOINT (ConditionsReportEndpoint)
management.endpoint.conditions.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.conditions.enabled=true # Whether to enable the conditions endpoint.

# CONFIGURATION PROPERTIES REPORT ENDPOINT (ConfigurationPropertiesReportEndpoint, ConfigurationPropertiesReportEndpointProperties)
management.endpoint.configprops.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.configprops.enabled=true # Whether to enable the configprops endpoint.
management.endpoint.configprops.keys-to-sanitize=password,secret,key,token,.*credentials.*,vcap_services # Keys that should be sanitized. Keys can be simple strings that the property ends with or regular expressions.

# ENVIRONMENT ENDPOINT (EnvironmentEndpoint, EnvironmentEndpointProperties)
management.endpoint.env.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.env.enabled=true # Whether to enable the env endpoint.
management.endpoint.env.keys-to-sanitize=password,secret,key,token,.*credentials.*,vcap_services # Keys that should be sanitized. Keys can be simple strings that the property ends with or regular expressions.

# FLYWAY ENDPOINT (FlywayEndpoint)
management.endpoint.flyway.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.flyway.enabled=true # Whether to enable the flyway endpoint.

# HEALTH ENDPOINT (HealthEndpoint, HealthEndpointProperties)
management.endpoint.health.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.health.enabled=true # Whether to enable the health endpoint.
management.endpoint.health.roles= # Roles used to determine whether or not a user is authorized to be shown details. When empty, all authenticated users are authorized.
management.endpoint.health.show-details=never # When to show full health details.

# HEAP DUMP ENDPOINT (HeapDumpWebEndpoint)
management.endpoint.heapdump.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.heapdump.enabled=true # Whether to enable the heapdump endpoint.

# HTTP TRACE ENDPOINT (HttpTraceEndpoint)
management.endpoint.httptrace.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.httptrace.enabled=true # Whether to enable the httptrace endpoint.

# INFO ENDPOINT (InfoEndpoint)
info= # Arbitrary properties to add to the info endpoint.
management.endpoint.info.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.info.enabled=true # Whether to enable the info endpoint.

# JOLOKIA ENDPOINT (JolokiaProperties)
management.endpoint.jolokia.config.*= # Jolokia settings. Refer to the documentation of Jolokia for more details.
management.endpoint.jolokia.enabled=true # Whether to enable the jolokia endpoint.

# LIQUIBASE ENDPOINT (LiquibaseEndpoint)
management.endpoint.liquibase.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.liquibase.enabled=true # Whether to enable the liquibase endpoint.

# LOG FILE ENDPOINT (LogFileWebEndpoint, LogFileWebEndpointProperties)
management.endpoint.logfile.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.logfile.enabled=true # Whether to enable the logfile endpoint.
management.endpoint.logfile.external-file= # External Logfile to be accessed. Can be used if the logfile is written by output redirect and not by the logging system itself.

# LOGGERS ENDPOINT (LoggersEndpoint)
management.endpoint.loggers.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.loggers.enabled=true # Whether to enable the loggers endpoint.

# REQUEST MAPPING ENDPOINT  (MappingsEndpoint)
management.endpoint.mappings.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.mappings.enabled=true # Whether to enable the mappings endpoint.

# METRICS ENDPOINT (MetricsEndpoint)
management.endpoint.metrics.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.metrics.enabled=true # Whether to enable the metrics endpoint.

# PROMETHEUS ENDPOINT (PrometheusScrapeEndpoint)
management.endpoint.prometheus.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.prometheus.enabled=true # Whether to enable the prometheus endpoint.

# SCHEDULED TASKS ENDPOINT (ScheduledTasksEndpoint)
management.endpoint.scheduledtasks.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.scheduledtasks.enabled=true # Whether to enable the scheduledtasks endpoint.

# SESSIONS ENDPOINT (SessionsEndpoint)
management.endpoint.sessions.enabled=true # Whether to enable the sessions endpoint.

# SHUTDOWN ENDPOINT (ShutdownEndpoint)
management.endpoint.shutdown.enabled=false # Whether to enable the shutdown endpoint.

# THREAD DUMP ENDPOINT (ThreadDumpEndpoint)
management.endpoint.threaddump.cache.time-to-live=0ms # Maximum time that a response can be cached.
management.endpoint.threaddump.enabled=true # Whether to enable the threaddump endpoint.

# HEALTH INDICATORS
management.health.db.enabled=true # Whether to enable database health check.
management.health.cassandra.enabled=true # Whether to enable Cassandra health check.
management.health.couchbase.enabled=true # Whether to enable Couchbase health check.
management.health.defaults.enabled=true # Whether to enable default health indicators.
management.health.diskspace.enabled=true # Whether to enable disk space health check.
management.health.diskspace.path= # Path used to compute the available disk space.
management.health.diskspace.threshold=0 # Minimum disk space, in bytes, that should be available.
management.health.elasticsearch.enabled=true # Whether to enable Elasticsearch health check.
management.health.elasticsearch.indices= # Comma-separated index names.
management.health.elasticsearch.response-timeout=100ms # Time to wait for a response from the cluster.
management.health.influxdb.enabled=true # Whether to enable InfluxDB health check.
management.health.jms.enabled=true # Whether to enable JMS health check.
management.health.ldap.enabled=true # Whether to enable LDAP health check.
management.health.mail.enabled=true # Whether to enable Mail health check.
management.health.mongo.enabled=true # Whether to enable MongoDB health check.
management.health.neo4j.enabled=true # Whether to enable Neo4j health check.
management.health.rabbit.enabled=true # Whether to enable RabbitMQ health check.
management.health.redis.enabled=true # Whether to enable Redis health check.
management.health.solr.enabled=true # Whether to enable Solr health check.
management.health.status.http-mapping= # Mapping of health statuses to HTTP status codes. By default, registered health statuses map to sensible defaults (for example, UP maps to 200).
management.health.status.order=DOWN,OUT_OF_SERVICE,UP,UNKNOWN # Comma-separated list of health statuses in order of severity.

# HTTP TRACING (HttpTraceProperties)
management.trace.http.enabled=true # Whether to enable HTTP request-response tracing.
management.trace.http.include=request-headers,response-headers,cookies,errors # Items to be included in the trace.

# INFO CONTRIBUTORS (InfoContributorProperties)
management.info.build.enabled=true # Whether to enable build info.
management.info.defaults.enabled=true # Whether to enable default info contributors.
management.info.env.enabled=true # Whether to enable environment info.
management.info.git.enabled=true # Whether to enable git info.
management.info.git.mode=simple # Mode to use to expose git information.

# METRICS
management.metrics.binders.files.enabled=true # Whether to enable files metrics.
management.metrics.binders.integration.enabled=true # Whether to enable Spring Integration metrics.
management.metrics.binders.jvm.enabled=true # Whether to enable JVM metrics.
management.metrics.binders.logback.enabled=true # Whether to enable Logback metrics.
management.metrics.binders.processor.enabled=true # Whether to enable processor metrics.
management.metrics.binders.uptime.enabled=true # Whether to enable uptime metrics.
management.metrics.distribution.percentiles-histogram.*= # Whether meter IDs starting-with the specified name should be publish percentile histograms.
management.metrics.distribution.percentiles.*= # Specific computed non-aggregable percentiles to ship to the backend for meter IDs starting-with the specified name.
management.metrics.distribution.sla.*= # Specific SLA boundaries for meter IDs starting-with the specified name. The longest match wins, the key `all` can also be used to configure all meters.
management.metrics.enable.*= # Whether meter IDs starting-with the specified name should be enabled. The longest match wins, the key `all` can also be used to configure all meters.
management.metrics.export.atlas.batch-size=10000 # Number of measurements per request to use for this backend. If more measurements are found, then multiple requests will be made.
management.metrics.export.atlas.config-refresh-frequency=10s # Frequency for refreshing config settings from the LWC service.
management.metrics.export.atlas.config-time-to-live=150s # Time to live for subscriptions from the LWC service.
management.metrics.export.atlas.config-uri=http://localhost:7101/lwc/api/v1/expressions/local-dev # URI for the Atlas LWC endpoint to retrieve current subscriptions.
management.metrics.export.atlas.connect-timeout=1s # Connection timeout for requests to this backend.
management.metrics.export.atlas.enabled=true # Whether exporting of metrics to this backend is enabled.
management.metrics.export.atlas.eval-uri=http://localhost:7101/lwc/api/v1/evaluate # URI for the Atlas LWC endpoint to evaluate the data for a subscription.
management.metrics.export.atlas.lwc-enabled=false # Whether to enable streaming to Atlas LWC.
management.metrics.export.atlas.meter-time-to-live=15m # Time to live for meters that do not have any activity. After this period the meter will be considered expired and will not get reported.
management.metrics.export.atlas.num-threads=2 # Number of threads to use with the metrics publishing scheduler.
management.metrics.export.atlas.read-timeout=10s # Read timeout for requests to this backend.
management.metrics.export.atlas.step=1m # Step size (i.e. reporting frequency) to use.
management.metrics.export.atlas.uri=http://localhost:7101/api/v1/publish # URI of the Atlas server.
management.metrics.export.datadog.api-key= # Datadog API key.
management.metrics.export.datadog.application-key= # Datadog application key. Not strictly required, but improves the Datadog experience by sending meter descriptions, types, and base units to Datadog.
management.metrics.export.datadog.batch-size=10000 # Number of measurements per request to use for this backend. If more measurements are found, then multiple requests will be made.
management.metrics.export.datadog.connect-timeout=1s # Connection timeout for requests to this backend.
management.metrics.export.datadog.descriptions=true # Whether to publish descriptions metadata to Datadog. Turn this off to minimize the amount of metadata sent.
management.metrics.export.datadog.enabled=true # Whether exporting of metrics to this backend is enabled.
management.metrics.export.datadog.host-tag=instance # Tag that will be mapped to "host" when shipping metrics to Datadog.
management.metrics.export.datadog.num-threads=2 # Number of threads to use with the metrics publishing scheduler.
management.metrics.export.datadog.read-timeout=10s # Read timeout for requests to this backend.
management.metrics.export.datadog.step=1m # Step size (i.e. reporting frequency) to use.
management.metrics.export.datadog.uri=https://app.datadoghq.com # URI to ship metrics to. If you need to publish metrics to an internal proxy en-route to Datadog, you can define the location of the proxy with this.
management.metrics.export.ganglia.addressing-mode=multicast # UDP addressing mode, either unicast or multicast.
management.metrics.export.ganglia.duration-units=milliseconds # Base time unit used to report durations.
management.metrics.export.ganglia.enabled=true # Whether exporting of metrics to Ganglia is enabled.
management.metrics.export.ganglia.host=localhost # Host of the Ganglia server to receive exported metrics.
management.metrics.export.ganglia.port=8649 # Port of the Ganglia server to receive exported metrics.
management.metrics.export.ganglia.protocol-version=3.1 # Ganglia protocol version. Must be either 3.1 or 3.0.
management.metrics.export.ganglia.rate-units=seconds # Base time unit used to report rates.
management.metrics.export.ganglia.step=1m # Step size (i.e. reporting frequency) to use.
management.metrics.export.ganglia.time-to-live=1 # Time to live for metrics on Ganglia. Set the multi-cast Time-To-Live to be one greater than the number of hops (routers) between the hosts.
management.metrics.export.graphite.duration-units=milliseconds # Base time unit used to report durations.
management.metrics.export.graphite.enabled=true # Whether exporting of metrics to Graphite is enabled.
management.metrics.export.graphite.host=localhost # Host of the Graphite server to receive exported metrics.
management.metrics.export.graphite.port=2004 # Port of the Graphite server to receive exported metrics.
management.metrics.export.graphite.protocol=pickled # Protocol to use while shipping data to Graphite.
management.metrics.export.graphite.rate-units=seconds # Base time unit used to report rates.
management.metrics.export.graphite.step=1m # Step size (i.e. reporting frequency) to use.
management.metrics.export.graphite.tags-as-prefix= # For the default naming convention, turn the specified tag keys into part of the metric prefix.
management.metrics.export.influx.auto-create-db=true # Whether to create the Influx database if it does not exist before attempting to publish metrics to it.
management.metrics.export.influx.batch-size=10000 # Number of measurements per request to use for this backend. If more measurements are found, then multiple requests will be made.
management.metrics.export.influx.compressed=true # Whether to enable GZIP compression of metrics batches published to Influx.
management.metrics.export.influx.connect-timeout=1s # Connection timeout for requests to this backend.
management.metrics.export.influx.consistency=one # Write consistency for each point.
management.metrics.export.influx.db=mydb # Tag that will be mapped to "host" when shipping metrics to Influx.
management.metrics.export.influx.enabled=true # Whether exporting of metrics to this backend is enabled.
management.metrics.export.influx.num-threads=2 # Number of threads to use with the metrics publishing scheduler.
management.metrics.export.influx.password= # Login password of the Influx server.
management.metrics.export.influx.read-timeout=10s # Read timeout for requests to this backend.
management.metrics.export.influx.retention-policy= # Retention policy to use (Influx writes to the DEFAULT retention policy if one is not specified).
management.metrics.export.influx.step=1m # Step size (i.e. reporting frequency) to use.
management.metrics.export.influx.uri=http://localhost:8086 # URI of the Influx server.
management.metrics.export.influx.user-name= # Login user of the Influx server.
management.metrics.export.jmx.enabled=true # Whether exporting of metrics to JMX is enabled.
management.metrics.export.jmx.step=1m # Step size (i.e. reporting frequency) to use.
management.metrics.export.newrelic.account-id= # New Relic account ID.
management.metrics.export.newrelic.api-key= # New Relic API key.
management.metrics.export.newrelic.batch-size=10000 # Number of measurements per request to use for this backend. If more measurements are found, then multiple requests will be made.
management.metrics.export.newrelic.connect-timeout=1s # Connection timeout for requests to this backend.
management.metrics.export.newrelic.enabled=true # Whether exporting of metrics to this backend is enabled.
management.metrics.export.newrelic.num-threads=2 # Number of threads to use with the metrics publishing scheduler.
management.metrics.export.newrelic.read-timeout=10s # Read timeout for requests to this backend.
management.metrics.export.newrelic.step=1m # Step size (i.e. reporting frequency) to use.
management.metrics.export.newrelic.uri=https://insights-collector.newrelic.com # URI to ship metrics to.
management.metrics.export.prometheus.descriptions=true # Whether to enable publishing descriptions as part of the scrape payload to Prometheus. Turn this off to minimize the amount of data sent on each scrape.
management.metrics.export.prometheus.enabled=true # Whether exporting of metrics to Prometheus is enabled.
management.metrics.export.prometheus.step=1m # Step size (i.e. reporting frequency) to use.
management.metrics.export.signalfx.access-token= # SignalFX access token.
management.metrics.export.signalfx.batch-size=10000 # Number of measurements per request to use for this backend. If more measurements are found, then multiple requests will be made.
management.metrics.export.signalfx.connect-timeout=1s # Connection timeout for requests to this backend.
management.metrics.export.signalfx.enabled=true # Whether exporting of metrics to this backend is enabled.
management.metrics.export.signalfx.num-threads=2 # Number of threads to use with the metrics publishing scheduler.
management.metrics.export.signalfx.read-timeout=10s # Read timeout for requests to this backend.
management.metrics.export.signalfx.source= # Uniquely identifies the app instance that is publishing metrics to SignalFx. Defaults to the local host name.
management.metrics.export.signalfx.step=10s # Step size (i.e. reporting frequency) to use.
management.metrics.export.signalfx.uri=https://ingest.signalfx.com # URI to ship metrics to.
management.metrics.export.simple.enabled=true # Whether, in the absence of any other exporter, exporting of metrics to an in-memory backend is enabled.
management.metrics.export.simple.mode=cumulative # Counting mode.
management.metrics.export.simple.step=1m # Step size (i.e. reporting frequency) to use.
management.metrics.export.statsd.enabled=true # Whether exporting of metrics to StatsD is enabled.
management.metrics.export.statsd.flavor=datadog # StatsD line protocol to use.
management.metrics.export.statsd.host=localhost # Host of the StatsD server to receive exported metrics.
management.metrics.export.statsd.max-packet-length=1400 # Total length of a single payload should be kept within your network's MTU.
management.metrics.export.statsd.polling-frequency=10s # How often gauges will be polled. When a gauge is polled, its value is recalculated and if the value has changed (or publishUnchangedMeters is true), it is sent to the StatsD server.
management.metrics.export.statsd.port=8125 # Port of the StatsD server to receive exported metrics.
management.metrics.export.statsd.publish-unchanged-meters=true # Whether to send unchanged meters to the StatsD server.
management.metrics.export.statsd.queue-size=2147483647 # Maximum size of the queue of items waiting to be sent to the StatsD server.
management.metrics.export.wavefront.api-token= # API token used when publishing metrics directly to the Wavefront API host.
management.metrics.export.wavefront.batch-size=10000 # Number of measurements per request to use for this backend. If more measurements are found, then multiple requests will be made.
management.metrics.export.wavefront.connect-timeout=1s # Connection timeout for requests to this backend.
management.metrics.export.wavefront.enabled=true # Whether exporting of metrics to this backend is enabled.
management.metrics.export.wavefront.global-prefix= # Global prefix to separate metrics originating from this app's white box instrumentation from those originating from other Wavefront integrations when viewed in the Wavefront UI.
management.metrics.export.wavefront.num-threads=2 # Number of threads to use with the metrics publishing scheduler.
management.metrics.export.wavefront.read-timeout=10s # Read timeout for requests to this backend.
management.metrics.export.wavefront.source= # Unique identifier for the app instance that is the source of metrics being published to Wavefront. Defaults to the local host name.
management.metrics.export.wavefront.step=10s # Step size (i.e. reporting frequency) to use.
management.metrics.export.wavefront.uri=https://longboard.wavefront.com # URI to ship metrics to.
management.metrics.use-global-registry=true # Whether auto-configured MeterRegistry implementations should be bound to the global static registry on Metrics.
management.metrics.web.client.max-uri-tags=100 # Maximum number of unique URI tag values allowed. After the max number of tag values is reached, metrics with additional tag values are denied by filter.
management.metrics.web.client.requests-metric-name=http.client.requests # Name of the metric for sent requests.
management.metrics.web.server.auto-time-requests=true # Whether requests handled by Spring MVC or WebFlux should be automatically timed.
management.metrics.web.server.requests-metric-name=http.server.requests # Name of the metric for received requests.


# ----------------------------------------
# DEVTOOLS PROPERTIES
# ----------------------------------------

# DEVTOOLS (DevToolsProperties)
spring.devtools.livereload.enabled=true # Whether to enable a livereload.com-compatible server.
spring.devtools.livereload.port=35729 # Server port.
spring.devtools.restart.additional-exclude= # Additional patterns that should be excluded from triggering a full restart.
spring.devtools.restart.additional-paths= # Additional paths to watch for changes.
spring.devtools.restart.enabled=true # Whether to enable automatic restart.
spring.devtools.restart.exclude=META-INF/maven/**,META-INF/resources/**,resources/**,static/**,public/**,templates/**,**/*Test.class,**/*Tests.class,git.properties,META-INF/build-info.properties # Patterns that should be excluded from triggering a full restart.
spring.devtools.restart.log-condition-evaluation-delta=true # Whether to log the condition evaluation delta upon restart.
spring.devtools.restart.poll-interval=1s # Amount of time to wait between polling for classpath changes.
spring.devtools.restart.quiet-period=400ms # Amount of quiet time required without any classpath changes before a restart is triggered.
spring.devtools.restart.trigger-file= # Name of a specific file that, when changed, triggers the restart check. If not specified, any classpath file change triggers the restart.

# REMOTE DEVTOOLS (RemoteDevToolsProperties)
spring.devtools.remote.context-path=/.~~spring-boot!~ # Context path used to handle the remote connection.
spring.devtools.remote.proxy.host= # The host of the proxy to use to connect to the remote application.
spring.devtools.remote.proxy.port= # The port of the proxy to use to connect to the remote application.
spring.devtools.remote.restart.enabled=true # Whether to enable remote restart.
spring.devtools.remote.secret= # A shared secret required to establish a connection (required to enable remote support).
spring.devtools.remote.secret-header-name=X-AUTH-TOKEN # HTTP header used to transfer the shared secret.


# ----------------------------------------
# TESTING PROPERTIES
# ----------------------------------------

spring.test.database.replace=any # Type of existing DataSource to replace.
spring.test.mockmvc.print=default # MVC Print option.
```


















































































































































































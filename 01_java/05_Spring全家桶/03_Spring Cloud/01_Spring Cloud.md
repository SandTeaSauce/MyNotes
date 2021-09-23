# 1、前提知识

## 1.1、系统架构演变

随着互联网的发展，网站应用的规模不断扩大。需求的激增，带来的是技术上的压力。系统架构也因此也不断的演进、升级、迭代。从单一应用，到垂直拆分，到分布式服务，到SOA，以及现在火热的微服务架构，还有在Google带领下来势汹涌的Service Mesh。我们到底是该乘坐微服务的船只驶向远方，还是偏安一隅得过且过？

其实生活不止眼前的苟且，还有诗和远方。所以我们今天就回顾历史，看一看系统架构演变的历程；把握现在，学习现在最火的技术架构；展望未来，争取成为一名优秀的Java工程师。

### 1.1.1、集中式架构

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是影响项目开发的关键。

![1567760086685](assets/1567760086685.png)

存在的问题：

- 代码耦合，开发维护困难
- 无法针对不同模块进行针对性优化
- 无法水平扩展
- 单点容错率低，并发能力差

### 1.1.2、垂直拆分

当访问量逐渐增大，单一应用无法满足需求，此时为了应对更高的并发和业务需求，我们根据业务功能对系统进行拆分：

![1567760144199](assets/1567760144199.png)

优点：

- 系统拆分实现了流量分担，解决了并发问题

- 可以针对不同模块进行优化

- 方便水平扩展，负载均衡，容错率提高

缺点：

- 系统间相互独立，会有很多重复开发工作，影响开发效率

### 1.1.3、分布式服务

当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的分布式调用是关键。

![1567760183874](assets/1567760183874.png)

优点：

- 将基础服务进行了抽取，系统间相互调用，提高了代码复用和开发效率

缺点：

- 系统间耦合度变高，调用关系错综复杂，难以维护

### 1.1.4、服务治理（SOA<流动计算架构>）

当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。此时，用于提高机器利用率的资源调度和治理中心(SOA)是关键

![1567760231305](assets/1567760231305.png)

以前出现了什么问题？

-  服务越来越多，需要管理每个服务的地址
- 调用关系错综复杂，难以理清依赖关系
- 服务过多，服务状态难以管理，无法根据服务情况动态管理

服务治理要做什么？

- 服务注册中心，实现服务自动注册和发现，无需人为记录服务地址
- 服务自动订阅，服务列表自动推送，服务调用透明化，无需关心依赖关系
- 动态监控服务状态监控报告，人为控制服务状态

缺点：

- 服务间会有依赖关系，一旦某个环节出错会影响较大
- 服务关系复杂，运维、测试部署困难，不符合DevOps思想

### 1.1.5、微服务

前面说的SOA，英文翻译过来是面向服务。微服务，似乎也是服务，都是对系统进行拆分。因此两者非常容易混淆，但其实缺有一些差别：

|                               |                                             |
| ----------------------------- | ------------------------------------------- |
| **微服务**                    | **SOA**                                     |
| 能拆分的就拆分                | 是整体的, 服务能放一起的都放一起            |
| 纵向业务划分                  | 是水平分多层                                |
| 由单一组织负责                | 按层级划分不同部门的组织负责                |
| 细颗粒                        | 粗颗粒                                      |
| 两句话可以解释明白            | 几百字只相当于SOA的目录                     |
| 独立的子公司                  | 类似大公司里面划分了一些业务单元(BU)        |
| 组件小                        | 存在较复杂的组件                            |
| 业务逻辑存在于每一个服务中    | 业务逻辑横跨多个业务逻辑                    |
| 使用轻量级的通讯方式, 如 Http | 企业服务产总线(ESB)充当了服务之间通信的角色 |

微服务的特点：

- **单一职责**：微服务中每一个服务都对应唯一的业务能力，做到单一职责

- **微**：微服务的服务拆分粒度很小，例如一个用户管理就可以作为一个服务。每个服务虽小，但“五脏俱全”。

- **面向服务**：面向服务是说每个服务都要对外暴露服务接口API。并不关心服务的技术实现，做到与平台和语言无关，也不限定用什么技术实现，只要提供Rest的接口即可。

- **自治**：自治是说服务间互相独立，互不干扰
  - 团队独立：每个服务都是一个独立的开发团队，人数不能过多。
  - 技术独立：因为是面向服务，提供Rest接口，使用什么技术没有别人干涉
  - 前后端分离：采用前后端分离开发，提供统一Rest接口，后端不用再为PC、移动段开发不同接口
  - 数据库分离：每个服务都使用自己的数据源
  - 部署独立，服务间虽然有调用，但要做到服务重启不影响其它服务。有利于持续集成和持续交付。每个服务都是独立的组件，可复用，可替换，降低耦合，易维护

微服务结构图：

![1567760487630](assets/1567760487630.png)

## 1.2、远程调用方式

无论是微服务还是SOA，都面临着服务间的远程调用。那么服务间的远程调用方式有哪些呢？

常见的远程调用方式有以下几种：

- **RPC**：Remote Produce Call远程过程调用，类似的还有RMI。自定义数据格式，基于原生TCP通信，速度快，效率高。早期的webservice，现在热门的dubbo，都是RPC的典型
- **Http**：http其实是一种网络传输协议，基于TCP，规定了数据传输的格式。现在客户端浏览器与服务端通信基本都是采用Http协议。也可以用来进行远程服务调用。缺点是消息封装臃肿。优势是对服务的提供方和调用方没有任何技术限定, 自由灵活, 更符合微服务理念.

现在热门的**Rest**风格，就可以通过**http协议**来实现。

### 1.2.1、认识RPC

**RPC**，即 Remote Procedure Call（远程过程调用），是一个计算机通信协议。 该协议允许运行于一台计算机的程序调用另一台计算机的子程序，而程序员无需额外地为这个交互作用编程。说得通俗一点就是：A计算机提供一个服务，B计算机可以像调用本地服务那样调用A计算机的服务。

通过上面的概念，我们可以知道，实现RPC主要是做到两点：

- 实现远程调用其他计算机的服务

- 要实现远程调用，肯定是通过网络传输数据。A程序提供服务，B程序通过网络将请求参数传递给A，A本地执行后得到结果，再将结果返回给B程序。这里需要关注的有两点：

  - 1）采用何种网络通讯协议？

    现在比较流行的RPC框架，都会采用TCP作为底层传输协议

  - 2）数据传输的格式怎样？

    两个程序进行通讯，必须约定好数据传输格式。就好比两个人聊天，要用同一种语言，否则无法沟通。所以，我们必须定义好请求和响应的格式。另外，数据在网路中传输需要进行序列化，所以还需要约定统一的序列化的方式。

- 像调用本地服务一样调用远程服务

- 如果仅仅是远程调用，还不算是RPC，因为RPC强调的是过程调用，调用的过程对用户而言是应该是透明的，用户不应该关心调用的细节，可以像调用本地服务一样调用远程服务。所以RPC一定要对调用的过程进行封装

RPC调用流程图：

![1567760647124](assets/1567760647124.png)

想要了解详细的RPC实现，给大家推荐一篇文章：[自己动手实现RPC](https://legacy.gitbook.com/book/huge0612/tour-of-rpc/details)

### 1.2.2、认识Http

Http协议：超文本传输协议，是一种应用层协议。规定了网络传输的请求格式、响应格式、资源定位和操作的方式等。但是底层采用什么网络传输协议，并没有规定，不过现在都是采用TCP协议作为底层传输协议。说到这里，大家可能觉得，Http与RPC的远程调用非常像，都是按照某种规定好的数据格式进行网络通信，有请求，有响应。没错，在这点来看，两者非常相似，但是还是有一些细微差别。

- RPC并没有规定数据传输格式，这个格式可以任意指定，不同的RPC协议，数据格式不一定相同。
- Http中还定义了资源定位的路径，RPC中并不需要
- 最重要的一点：RPC需要满足像调用本地服务一样调用远程服务，也就是对调用过程在API层面进行封装。Http协议没有这样的要求，因此请求、响应等细节需要我们自己去实现。
- 优点：RPC方式更加透明，对用户更方便。Http方式更灵活，没有规定API和语言，跨语言、跨平台
- 缺点：RPC方式需要在API层面进行封装，限制了开发的语言环境。

例如我们通过浏览器访问网站，就是通过Http协议。只不过浏览器把请求封装，发起请求以及接收响应，解析响应的事情都帮我们做了。如果是不通过浏览器，那么这些事情都需要自己去完成。

![1567760739297](assets/1567760739297.png)

### 1.2.3、如何选择？

既然两种方式都可以实现远程调用，我们该如何选择呢？

- 速度来看，RPC要比http更快，虽然底层都是TCP，但是http协议的信息往往比较臃肿，不过可以采用gzip压缩。

- 难度来看，RPC实现较为复杂，http相对比较简单

- 灵活性来看，http更胜一筹，因为它不关心实现细节，跨平台、跨语言。

因此，两者都有不同的使用场景：

- 如果对效率要求更高，并且开发过程使用统一的技术栈，那么用RPC还是不错的。

- 如果需要更加灵活，跨语言、跨平台，显然http更合适

那么我们该怎么选择呢？

微服务，更加强调的是独立、自治、灵活。而RPC方式的限制较多，因此微服务框架中，一般都会采用基于Http的Rest风格服务。

## 1.3、Http客户端工具

既然微服务选择了Http，那么我们就需要考虑自己来实现对请求和响应的处理。不过开源世界已经有很多的http客户端工具，能够帮助我们做这些事情，例如：

- HttpClient

- OKHttp

- URLConnection

接下来，我们就一起了解一款比较流行的客户端工具：HttpClient

### 1.3.1、HttpClient

#### ①、介绍

HttpClient是Apache公司的产品，是Http Components下的一个组件。

[官网地址：http://hc.apache.org/index.html](http://hc.apache.org/index.html)

![1567760843588](assets/1567760843588.png)

特点：

- 基于标准、纯净的Java语言。实现了Http1.0和Http1.1

- 以可扩展的面向对象的结构实现了Http全部的方法（GET, POST, PUT, DELETE, HEAD, OPTIONS, and TRACE）

- 支持HTTPS协议。

- 通过Http代理建立透明的连接。

- 自动处理Set-Cookie中的Cookie。

#### ②、使用

我们导入课前资料提供的demo工程：《http-demo》

发起get请求：

```java
@Test
public void testGet() throws IOException {
   HttpGet request = new HttpGet("http://www.baidu.com");
   String response = this.httpClient.execute(request, new BasicResponseHandler());
   System.out.println(response);
}
```

发起Post请求：

```java
@Test
public void testPost() throws IOException {
    HttpPost request = new HttpPost("http://www.oschina.net/");
    request.setHeader("User-Agent",
                      "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36");
    String response = this.httpClient.execute(request, new BasicResponseHandler());
    System.out.println(response);
}
```

尝试访问昨天编写的接口：http://localhost/hello

这个接口返回一个User对象

```java
@Test
public void testGetPojo() throws IOException {
    HttpGet request = new HttpGet("http://localhost/hello");
    String response = this.httpClient.execute(request, new BasicResponseHandler());
    System.out.println(response);
}
```

我们实际得到的是一个json字符串：

```json
{
    "id": 8,
    "userName": "liuyan",
    "password": "123456",
    "name": "柳岩",
    "age": 21,
    "sex": 2,
    "birthday": "1995-08-07T16:00:00.000+0000",
    "created": "2014-09-20T03:41:15.000+0000",
    "updated": "2014-09-20T03:41:15.000+0000",
    "note": "柳岩同学在传智播客学表演"
}
```

如果想要得到对象，我们还需要手动进行Json反序列化，这一点比较麻烦。

### 1.3.2、Json转换工具

HttpClient请求数据后是json字符串，需要我们自己把Json字符串反序列化为对象，我们会使用JacksonJson工具来实现。

JacksonJson是SpringMVC内置的json处理工具，其中有一个`ObjectMapper`类，可以方便的实现对json的处理：

#### ①、对象转json

```java
// json处理工具
    private ObjectMapper mapper = new ObjectMapper();
    @Test
    public void testJson() throws JsonProcessingException {
        User user = new User();
        user.setId(8L);
        user.setAge(21);
        user.setName("柳岩");
        user.setUserName("liuyan");
        // 序列化
        String json = mapper.writeValueAsString(user);
        System.out.println("json = " + json);
    }
```

结果：

![1567761126910](assets/1567761126910.png)

#### ②、json转普通对象

```java
// json处理工具
private ObjectMapper mapper = new ObjectMapper();
@Test
public void testJson() throws IOException {
    User user = new User();
    user.setId(8L);
    user.setAge(21);
    user.setName("柳岩");
    user.setUserName("liuyan");
    // 序列化
    String json = mapper.writeValueAsString(user);

    // 反序列化，接收两个参数：json数据，反序列化的目标类字节码
    User result = mapper.readValue(json, User.class);
    System.out.println("result = " + result);
}
```

结果：

![1567761158961](assets/1567761158961.png)

#### ③、json转集合

json转集合比较麻烦，因为你无法同时把集合的class和元素的class同时传递到一个参数。

因此Jackson做了一个类型工厂，用来解决这个问题：

```java
// json处理工具
private ObjectMapper mapper = new ObjectMapper();
@Test
public void testJson() throws IOException {
    User user = new User();
    user.setId(8L);
    user.setAge(21);
    user.setName("柳岩");
    user.setUserName("liuyan");

    // 序列化,得到对象集合的json字符串
    String json = mapper.writeValueAsString(Arrays.asList(user, user));

    // 反序列化，接收两个参数：json数据，反序列化的目标类字节码
    List<User> users = mapper.readValue(json, mapper.getTypeFactory().constructCollectionType(List.class, User.class));
    for (User u : users) {
        System.out.println("u = " + u);
    }
}
```



#### ④、json转任意复杂类型

当对象泛型关系复杂时，类型工厂也不好使了。这个时候Jackson提供了TypeReference来接收类型泛型，然后底层通过反射来获取泛型上的具体类型。实现数据转换。

```java
// json处理工具
private ObjectMapper mapper = new ObjectMapper();
@Test
public void testJson() throws IOException {
    User user = new User();
    user.setId(8L);
    user.setAge(21);
    user.setName("柳岩");
    user.setUserName("liuyan");

    // 序列化,得到对象集合的json字符串
    String json = mapper.writeValueAsString(Arrays.asList(user, user));

    // 反序列化，接收两个参数：json数据，反序列化的目标类字节码
    List<User> users = mapper.readValue(json, new TypeReference<List<User>>(){});
    for (User u : users) {
        System.out.println("u = " + u);
    }
}
```

结果：

![1567761276928](assets/1567761276928.png)

### 1.3.3、Spring的RestTemplate

Spring提供了一个RestTemplate模板工具类，对基于Http的客户端进行了封装，并且实现了对象与json的序列化和反序列化，非常方便。RestTemplate并没有限定Http的客户端类型，而是进行了抽象，目前常用的3种都有支持：

- HttpClient
- OkHttp
- JDK原生的URLConnection（默认的）

首先在项目中注册一个RestTemplate对象，可以在启动类位置注册：

```java
@SpringBootApplication
public class HttpDemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(HttpDemoApplication.class, args);
    }

    @Bean
    public RestTemplate restTemplate() {
        // 默认的RestTemplate，底层是走JDK的URLConnection方式。
        return new RestTemplate();
    }
}
```

在测试类中直接@Autowired注入：

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = HttpDemoApplication.class)
public class HttpDemoApplicationTests {

   @Autowired
   private RestTemplate restTemplate;

   @Test
   public void httpGet() {
      // 通过 get 请求 调用连接<http://localhost/hello>, 返回 json 自动转化为 User 对象
      User user = this.restTemplate.getForObject("http://localhost/hello", User.class);
      System.out.println(user);
   }
}
```

> 【说明】通过`RestTemplate`的`getForObject()`方法，传递url地址及实体类的字节码，RestTemplate会自动发起请求，接收响应，并且帮我们对响应结果进行反序列化。

![1567761455840](assets/1567761455840.png)

 

# 2、初始SpringCloud

微服务是一种架构方式，最终肯定需要技术架构去实施。

微服务的实现方式很多，但是最火的莫过于Spring Cloud了。为什么？

- **后台硬**：作为Spring家族的一员，有整个Spring全家桶靠山，背景十分强大。

- **技术强**：Spring作为Java领域的前辈，可以说是功力深厚。有强力的技术团队支撑，一般人还真比不了

- **群众基础好**：可以说大多数程序员的成长都伴随着Spring框架，试问：现在有几家公司开发不用Spring？SpringCloud与Spring的各个框架无缝整合，对大家来说一切都是熟悉的配方，熟悉的味道。

- **使用方便**：相信大家都体会到了SpringBoot给我们开发带来的便利，而SpringCloud完全支持SpringBoot的开发，用很少的配置就能完成微服务框架的搭建

## 2.1、简介

SpringCloud是Spring旗下的项目之一，[官网地址：http://projects.spring.io/spring-cloud/](http://projects.spring.io/spring-cloud/)

Spring最擅长的就是集成，把世界上最好的框架拿过来，集成到自己的项目中。

SpringCloud也是一样，它将现在非常流行的一些技术整合到一起，实现了诸如：配置管理，服务发现，智能路由，负载均衡，熔断器，控制总线，集群状态等等功能。其主要涉及的组件包括：

Netflix 公司旗下的

- Eureka：注册中心

- Zuul：服务网关

- Ribbon：负载均衡

- Feign：服务调用

- Hystix：熔断器、

  以上只是其中一部分，架构图：

![1567761567403](assets/1567761567403.png)

## 2.2、版本

SpringCloud的版本命名比较特殊，因为它不是一个组件，而是许多组件的集合，它的命名是以A到Z的为首字母的一些单词组成：

![1567761608444](assets/1567761608444.png)

我们在项目中，会是以Finchley的版本。

其中包含的组件，也都有各自的版本，如下表：

| Component                 | Edgware.SR3    | Finchley.RC1     | Finchley.BUILD-SNAPSHOT |
| ------------------------- | -------------- | ---------------- | ----------------------- |
| spring-cloud-aws          | 1.2.2.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-bus          | 1.3.2.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-cli          | 1.4.1.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-commons      | 1.3.3.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-contract     | 1.2.4.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-config       | 1.4.3.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-netflix      | 1.4.4.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-security     | 1.2.2.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-cloudfoundry | 1.1.1.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-consul       | 1.3.3.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-sleuth       | 1.3.3.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-stream       | Ditmars.SR3    | Elmhurst.RELEASE | Elmhurst.BUILD-SNAPSHOT |
| spring-cloud-zookeeper    | 1.2.1.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-boot               | 1.5.10.RELEASE | 2.0.1.RELEASE    | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-task         | 1.2.2.RELEASE  | 2.0.0.RC1        | 2.0.0.RELEASE           |
| spring-cloud-vault        | 1.1.0.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-gateway      | 1.0.1.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-openfeign    |                | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |

接下来，我们就一一学习SpringCloud中的重要组件。

## 2.3、微服务场景模拟

首先，我们需要模拟一个服务调用的场景。方便后面学习微服务架构

### 2.3.1、服务提供者

我们新建一个项目，对外提供查询用户的服务。

#### ①、Spring脚手架创建工程

借助于Spring提供的快速搭建工具：

![1567761729081](assets/1567761729081.png)

填写项目信息：

![1567761743281](assets/1567761743281.png)

添加web依赖：

![1567761754453](assets/1567761754453.png)

添加mybatis依赖：

![1567761764693](assets/1567761764693.png)

填写项目位置：

![1567761774706](assets/1567761774706.png)

生成的项目结构：

![1567761786682](assets/1567761786682.png)

依赖也已经全部自动引入：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.leyou.demo</groupId>
    <artifactId>user-service-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>user-service-demo</name>
    <description>Demo project for Spring Boot</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.1.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.3.2</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

当然，因为要使用通用mapper，所以我们需要手动加一条依赖：

```xml
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-spring-boot-starter</artifactId>
    <version>2.0.2</version>
</dependency>
```

非常快捷啊！

#### ②、编写代码

添加一个对外查询的接口：

```java
@RestController
@RequestMapping("user")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping("/{id}")
    public User queryById(@PathVariable("id") Long id) {
        return this.userService.queryById(id);
    }
```

Service：

```xml
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    public User queryById(Long id) {
        return this.userMapper.selectByPrimaryKey(id);
    }
}
```

mapper:

```java
@Mapper
public interface UserMapper extends tk.mybatis.mapper.common.Mapper<User>{
}
```

实体类：

```xml
@Table(name = "tb_user")
public class User implements Serializable {

    private static final long serialVersionUID = 1L;

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // 用户名
    private String userName;

    // 密码
    private String password;

    // 姓名
    private String name;

    // 年龄
    private Integer age;

    // 性别，1男性，2女性
    private Integer sex;

    // 出生日期
    private Date birthday;

    // 创建时间
    private Date created;

    // 更新时间
    private Date updated;

    // 备注
    private String note;

   // 。。。省略getters和setters
}
```

属性文件,这里我们采用了yaml语法，而不是properties：

```yaml
server:
  port: 8081
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydb01
    username: root
    password: 123
    hikari:
      maximum-pool-size: 20
      minimum-idle: 10
mybatis:
  type-aliases-package: com.leyou.userservice.pojo
```

项目结构：

![1567761986282](assets/1567761986282.png)

#### ③、启动并测试：

启动项目，访问接口：http://localhost:8081/user/7

![1567762046171](assets/1567762046171.png)



### 2.3.2、服务调用者

#### ①、创建工程

与上面类似，这里不再赘述，需要注意的是，我们调用user-service的功能，因此不需要mybatis相关依赖了。

pom：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.leyou.demo</groupId>
    <artifactId>user-consumer-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>user-consumer-demo</name>
    <description>Demo project for Spring Boot</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.1.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- 添加OkHttp支持 -->
        <dependency>
            <groupId>com.squareup.okhttp3</groupId>
            <artifactId>okhttp</artifactId>
            <version>3.9.0</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

#### ②、编写代码

首先在启动类中注册RestTemplate：

```java
@SpringBootApplication
public class UserConsumerDemoApplication {

    @Bean
    public RestTemplate restTemplate() {
        // 这次我们使用了OkHttp客户端,只需要注入工厂即可
        return new RestTemplate(new OkHttp3ClientHttpRequestFactory());
    }

    public static void main(String[] args) {
        SpringApplication.run(UserConsumerDemoApplication.class, args);
    }
}
```

然后编写UserDao，注意，这里不是调用mapper查数据库，而是通过RestTemplate远程查询user-service-demo中的接口：

```java
@Component
public class UserDao {

    @Autowired
    private RestTemplate restTemplate;

    public User queryUserById(Long id){
        String url = "http://localhost:8081/user/" + id;
        return this.restTemplate.getForObject(url, User.class);
    }
}
```

然后编写user-service，循环查询UserDAO信息：

```java
@Service
public class UserService {

    @Autowired
    private UserDao userDao;

    public List<User> querUserByIds(List<Long> ids){
        List<User> users = new ArrayList<>();
        for (Long id : ids) {
            User user = this.userDao.queryUserById(id);
            users.add(user);
        }
        return users;
    }
}
```

编写controller：

```java
@RestController
@RequestMapping("consume")
public class ConsumerController {

    @Autowired
    private UserService userService;

    @GetMapping
    public List<User> consume(@RequestParam("ids") List<Long> ids) {
        return this.userService.queryUserByIds(ids);
    }
}
```

#### ③、启动测试：

因为我们没有配置端口，那么默认就是8080，我们访问：http://localhost:8080/consume?ids=6,7,8

![1567762202675](assets/1567762202675.png)



一个简单的远程服务调用案例就实现了。

### 2.3.3、有没有问题？

简单回顾一下，刚才我们写了什么：

- use-service-demo：一个提供根据id查询用户的微服务
- consumer-demo：一个服务调用者，通过RestTemplate远程调用user-service-demo

流程如下：

![1567762760508](assets/1567762760508.png)

存在什么问题？

- 在consumer中，我们把url地址硬编码到了代码中，不方便后期维护
- consumer需要记忆user-service的地址，如果出现变更，可能得不到通知，地址将失效
- consumer不清楚user-service的状态，服务宕机也不知道
- user-service只有1台服务，不具备高可用性
- 即便user-service形成集群，consumer还需自己实现负载均衡

其实上面说的问题，概括一下就是分布式服务必然要面临的问题：

- 服务管理
  - 如何自动注册和发现
  - 如何实现状态监管
  - 如何实现动态路由

- 服务如何实现负载均衡

- 服务如何解决容灾问题

- 服务如何实现统一配置

以上的问题，我们都将在 ==SpringCloud==中得到答案。

# 3、注册中心Eureka

## 3.1、认识Eureka

首先我们来解决第一问题，==服务的管理==。

问题分析

在刚才的案例中，user-service对外提供服务，需要对外暴露自己的地址。而consumer（调用者）需要记录服务提供者的地址。将来地址出现变更，还需要及时更新。这在服务较少的时候并不觉得有什么，但是在现在日益复杂的互联网环境，一个项目肯定会拆分出十几，甚至数十个微服务。此时如果还人为管理地址，不仅开发困难，将来测试、发布上线都会非常麻烦，这与DevOps的思想是背道而驰的。

《网约车》
这就好比是 网约车出现以前，人们出门叫车只能叫出租车。一些私家车想做出租却没有资格，被称为黑车。而很多人想要约车，但是无奈出租车太少，不方便。私家车很多却不敢拦，而且满大街的车，谁知道哪个才是愿意载人的。一个想要，一个愿意给，就是缺少引子，缺乏管理啊。
此时滴滴这样的网约车平台出现了，所有想载客的私家车全部到滴滴注册，记录你的车型（服务类型），身份信息（联系方式）。这样提供服务的私家车，在滴滴那里都能找到，一目了然。
此时要叫车的人，只需要打开APP，输入你的目的地，选择车型（服务类型），滴滴自动安排一个符合需求的车到你面前，为你服务，完美！

Eureka做什么？

Eureka就好比是滴滴，负责管理、记录服务提供者的信息。服务调用者无需自己寻找服务，而是把自己的需求告诉Eureka，然后Eureka会把符合你需求的服务告诉你。

同时，服务提供方与Eureka之间通过“心跳”机制进行监控，当某个服务提供方出现问题，Eureka自然会把它从服务列表中剔除。

这就实现了服务的自动注册、发现、状态监控。

## 3.2、原理图

基本架构：

![1567762903782](assets/1567762903782.png)

- Eureka：就是服务注册中心（可以是一个集群），对外暴露自己的地址
- 提供者：启动后向Eureka注册自己信息（地址，提供什么服务）
- 消费者：向Eureka订阅服务，Eureka会将对应服务的所有提供者地址列表发送给消费者，并且定期更新
- 心跳(续约)：提供者定期通过http方式向Eureka刷新自己的状态

## 3.3、入门案例

### 3.3.1、编写Eureka服务器（EurekaServer）

接下来我们创建一个项目，启动一个EurekaServer：

依然使用spring提供的快速搭建工具：

![1567762950128](assets/1567762950128.png)

选择依赖：

![1567762959475](assets/1567762959475.png)

完整的Pom文件：

```xml
<dependencies>
    <!-- Eureka服务端 -->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
    </dependency>
</dependencies>

<dependencyManagement>
    <dependencies>
        <!-- SpringCloud依赖，一定要放到dependencyManagement中，起到管理版本的作用即可 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Finchley.RC1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

编写启动类：

```java
@SpringBootApplication
@EnableEurekaServer // 声明这个应用是一个EurekaServer 
public class EurekaDemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(EurekaDemoApplication.class, args);
    }
}
```

编写配置：

```yaml
server:
  port: 10086 # 端口
spring:
  application:
    name: eureka-server # 应用名称，会在Eureka中显示
eureka:
  client:
    register-with-eureka: false # 是否注册自己的信息到EurekaServer，默认是true
    fetch-registry: false       # 是否拉取其它服务的信息，默认是true
    service-url:                # EurekaServer的地址，现在是自己的地址，如果是集群，需要加上其它Server的地址。
      defaultZone: http://127.0.0.1:${server.port}/eureka
```

启动服务，并访问：http://127.0.0.1:10086/eureka

![1567763080460](assets/1567763080460.png)

![1567763086984](assets/1567763086984.png)

### 3.3.2、编写服务提供者，并将服务提供者（user-service）注册到Eureka服务器

注册服务，就是在服务上添加Eureka的客户端依赖，客户端代码会自动把服务注册到EurekaServer中。

我们在user-service-demo中添加Eureka客户端依赖：

先添加SpringCloud依赖：

```xml
<!-- SpringCloud的依赖 -->
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Finchley.RC1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
<!-- Spring的仓库地址 -->
<repositories>
    <repository>
        <id>spring-milestones</id>
        <name>Spring Milestones</name>
        <url>https://repo.spring.io/milestone</url>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </repository>
</repositories>
```

然后是Eureka客户端：

```xml
<!-- Eureka客户端 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

在启动类上开启Eureka客户端功能

通过添加`@EnableDiscoveryClient`表示当前服务是一个 Eureka 的客户端。

> 【注意】 @EnableEurekaClient 只能使用 Eureka 框架的, 但是, 用 @EnableDiscoveryClient 就可以使用 Eureka / Zookeeper / Consul 都可以使用

```java
@SpringBootApplication
@EnableDiscoveryClient // 开启EurekaClient功能
public class UserServiceDemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceDemoApplication.class, args);
    }
}
```

编写配置

```yaml
server:
  port: 8081
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydb01
    username: root
    password: 123
    hikari:
      maximum-pool-size: 20
      minimum-idle: 10
  application:
    name: user-service # 应用名称
mybatis:
  type-aliases-package: com.leyou.userservice.pojo
eureka:
  client:
    service-url:            # EurekaServer地址
      defaultZone: http://127.0.0.1:10086/eureka
  instance:
    prefer-ip-address: true # 当调用getHostname获取实例的hostname时，返回ip而不是host名称
    ip-address: 127.0.0.1   # 指定自己的ip信息，不指定的话会自己寻找
```

> 【注意】
>
> ①、这里我们添加了`spring.application.name`属性来指定应用名称，将来会作为应用的id使用。
>
> ②、不用指定`register-with-eureka`和`fetch-registry`，因为默认是true



重启项目，访问[Eureka监控页面](http://127.0.0.1:10086/eureka)查看

![1567777430987](assets/1567777430987.png)

 我们发现user-service服务已经注册成功了

### 3.3.3、编写服务消费者，并让服务消费者（consumer-demo）从Eureka服务器中获取服务

接下来我们修改consumer-demo，尝试从EurekaServer获取服务。

方法与消费者类似，只需要在项目中添加EurekaClient依赖，就可以通过服务名称来获取信息了！

**1）添加依赖：**

先添加SpringCloud依赖：

```xml
<!-- SpringCloud的依赖 -->
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Finchley.RC1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
<!-- Spring的仓库地址 -->
<repositories>
    <repository>
        <id>spring-milestones</id>
        <name>Spring Milestones</name>
        <url>https://repo.spring.io/milestone</url>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </repository>
</repositories>
```

 然后是Eureka客户端：

```xml
<!-- Eureka客户端 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

**2）在启动类开启Eureka客户端**

```java
@SpringBootApplication
@EnableDiscoveryClient // 开启Eureka客户端
public class UserConsumerDemoApplication {
    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate(new OkHttp3ClientHttpRequestFactory());
    }
    public static void main(String[] args) {
        SpringApplication.run(UserConsumerDemoApplication.class, args);
    }
}
```

**3）修改配置：**

```yaml
server:
  port: 8080
spring:
  application:
    name: consumer # 应用名称
eureka:
  client:
    service-url: # EurekaServer地址
      defaultZone: http://127.0.0.1:10086/eureka
  instance:
    prefer-ip-address: true # 当其它服务获取地址时提供ip而不是hostname
    ip-address: 127.0.0.1   # 指定自己的ip信息，不指定的话会自己寻找
```

**4）修改代码，用`DiscoveryClient`类的方法，根据服务名称，获取服务实例：**

```java
@Service
public class UserService {

    @Autowired
    private RestTemplate restTemplate;

    @Autowired
    private DiscoveryClient discoveryClient;// Eureka客户端，可以获取到服务实例信息

    public List<User> queryUserByIds(List<Long> ids) {
        List<User> users = new ArrayList<>();
        // String baseUrl = "http://localhost:8081/user/";
        // 根据服务名称，获取服务实例
        List<ServiceInstance> instances = discoveryClient.getInstances("user-service");
        // 因为只有一个UserService,因此我们直接get(0)获取
        ServiceInstance instance = instances.get(0);
        // 获取ip和端口信息
        String baseUrl = "http://"+instance.getHost() + ":" + instance.getPort()+"/user/";
        ids.forEach(id -> {
            // 我们测试多次查询，
            users.add(this.restTemplate.getForObject(baseUrl + id, User.class));
            // 每次间隔500毫秒
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        return users;
    }
}
```

5）Debug跟踪运行：

![1567777674123](assets/1567777674123.png)

生成的URL：

![1567777706802](assets/1567777706802.png)

访问结果：

![1567777717925](assets/1567777717925.png)

## 3.4、Eureka详解

接下来我们详细讲解Eureka的原理及配置。

### 3.4.1、基础架构

Eureka架构中的三个核心角色：

- 服务注册中心

  Eureka的服务端应用，提供服务注册和发现功能，就是刚刚我们建立的eureka-demo

- 服务提供者

  提供服务的应用，可以是SpringBoot应用，也可以是其它任意技术实现，只要对外提供的是Rest风格服务即可。本例中就是我们实现的user-service-demo

- 服务消费者

  消费应用从注册中心获取服务列表，从而得知每个服务方的信息，知道去哪里调用服务方。本例中就是我们实现的consumer-demo

### 3.4.2、高可用的Eureka Server

Eureka Server即服务的注册中心，在刚才的案例中，我们只有一个EurekaServer，事实上EurekaServer也可以是一个集群，形成高可用的Eureka中心。

- 服务同步

  多个Eureka Server之间也会互相注册为服务，当服务提供者注册到Eureka Server集群中的某个节点时，该节点会把服务的信息同步给集群中的每个节点，从而实现**数据同步**。因此，无论客户端访问到Eureka Server集群中的任意一个节点，都可以获取到完整的服务列表信息。

![1567777866682](assets/1567777866682.png)

动手搭建高可用的EurekaServer

我们假设要搭建两条EurekaServer的集群，端口分别为：10086和10087

**1）我们修改原来的EurekaServer配置：**

```
server:
  port: 10086 # 端口
spring:
  application:
    name: eureka-server # 应用名称，会在Eureka中显示
eureka:
  client:
    service-url: # 配置其他Eureka服务的地址，而不是自己，比如10087
      defaultZone: http://127.0.0.1:10087/eureka
```

所谓的高可用注册中心，其实就是把EurekaServer自己也作为一个服务进行注册，这样多个EurekaServer之间就能互相发现对方，从而形成集群。因此我们做了以下修改：

1. ==删除了`register-with-eureka=false`和f`etch-registry=false`==两个配置。因为默认值是true，这样就会吧自己注册到注册中心了。

2. 把==service-url的值改成了另外一台EurekaServer==的地址，而不是自己

**2）另外一台配置恰好相反：**

```yaml
server:
  port: 10087 # 端口
spring:
  application:
    name: eureka-server # 应用名称，会在Eureka中显示
eureka:
  client:
    service-url: # 配置其他Eureka服务的地址，而不是自己，比如10087
      defaultZone: http://127.0.0.1:10086/eureka
```

注意：idea中一个应用不能启动两次，我们需要重新配置一个启动器：

![1567777974665](assets/1567777974665.png)

![1567777981948](assets/1567777981948.png)

![1567777988971](assets/1567777988971.png)

然后启动即可。

**3）启动测试：**

![1567778007447](assets/1567778007447.png)



**4）客户端注册服务到集群**

因为EurekaServer不止一个，因此注册服务的时候，service-url参数需要变化：

```yaml
eureka:
  client:
    service-url: # EurekaServer地址,多个地址以','隔开
      defaultZone: http://127.0.0.1:10086/eureka,http://127.0.0.1:10087/eureka
```

### 3.4.3、服务提供者

服务提供者要向EurekaServer注册服务，并且完成服务续约等工作。

**1) 服务注册**

服务提供者在启动时，会检测配置属性中的：`eureka.client.register-with-erueka=true`参数是否正确，事实上默认就是true。如果值确实为true，则会向EurekaServer发起一个Rest请求，并携带自己的元数据信息，Eureka Server会把这些信息保存到一个双层Map结构中。

- 第一层Map的Key就是服务名称，一般是配置中的`spring.application.name`属性
- 第二层Map的key是服务的实例id。一般host + serverid + port, 例如: localhost:user0service:8081
- 值则是服务的实例对象, 也就是说一个服务，可以同时启动多个不同实例, 形成集群。

 

**2) 服务续约**

在注册服务完成以后，服务提供者会维持一个心跳（定时向EurekaServer发起Rest请求），告诉EurekaServer：“我还活着”。这个我们称为服务的续约（renew）；

有两个重要参数可以修改服务续约的行为：

```yaml
eureka:
  instance:
    lease-expiration-duration-in-seconds: 90
    lease-renewal-inter val-in-seconds: 30
```

> 【说明】
>
> ①、`lease-renewal-interval-in-seconds`：最小服务续约(renew)的间隔，默认为30秒
>
> ②、`lease-expiration-duration-in-seconds`：服务失效时间，默认值90秒

也就是说，默认情况下每个30秒服务会向注册中心发送一次心跳，证明自己还活着。如果超过90秒没有发送心跳，EurekaServer就会认为该服务宕机，会从服务列表中移除，这两个值在生产环境不要修改，默认即可。

但是在开发时，这个值有点太长了，经常我们关掉一个服务，会发现Eureka依然认为服务在活着。所以我们在开发阶段可以适当调小。

```yaml
eureka:
  instance:
    lease-expiration-duration-in-seconds: 10 # 10秒即过期
    lease-renewal-interval-in-seconds: 5 # 5秒一次心跳
```

**3）实例id**

先来看一下服务状态信息：

在Eureka监控页面，查看服务注册信息：

![1567778241457](assets/1567778241457.png)

在status一列中，显示以下信息：

- UP(1)：代表现在是启动了1个示例，没有集群
- DESKTOP-2MVEC12:user-service:8081：是示例的名称（instance-id），
- 默认格式是：`${hostname} + ${spring.application.name} + ${server.port}`
- instance-id是区分同一服务的不同实例的唯一标准，因此不能重复。

我们可以通过instance-id属性来修改它的构成：

```yaml
eureka:
  instance:
    instance-id: ${spring.application.name}:${server.port}
```

重启服务再试试看：

![1567778329305](assets/1567778329305.png)

### 3.4.4、服务消费者

**1）获取服务列表**

当服务消费者启动是，会检测`eureka.client.fetch-registry=true`参数的值，如果为true，则会从Eureka Server服务的列表只读备份，然后缓存在本地。并且每隔30秒会重新获取并更新数据。我们可以通过下面的参数来修改：

```yaml
eureka:
  client:
    registry-fetch-interval-seconds: 5
```

生产环境中，我们不需要修改这个值。

但是为了开发环境下，能够快速得到服务的最新状态，我们可以将其设置小一点。

### 3.4.5、失效剔除和自我保护(Eureka服务中配置)

**1）失效剔除**

有些时候，我们的服务提供方并不一定会正常下线，可能因为内存溢出、网络故障等原因导致服务无法正常工作。Eureka Server需要将这样的服务剔除出服务列表。因此它会开启一个定时任务，每隔60秒对所有失效的服务（超过90秒未响应）进行剔除。

可以通过`eureka.server.eviction-interval-timer-in-ms`参数对其进行修改，单位是毫秒，生成环境不要修改。

这个会对我们开发带来极大的不变，你对服务重启，隔了60秒Eureka才反应过来。开发阶段可以适当调整，比如10S

**2）自我保护**

我们关停一个服务，就会在Eureka面板看到一条警告：

![1567778434313](assets/1567778434313.png)

这是触发了Eureka的自我保护机制。当一个服务未按时进行心跳续约时，Eureka会统计最近15分钟心跳失败的服务实例的比例是否超过了85%。在生产环境下，因为网络延迟等原因，心跳失败实例的比例很有可能超标，但是此时就把服务剔除列表并不妥当，因为服务可能没有宕机。Eureka就会把当前实例的注册信息保护起来，不予剔除。生产环境下这很有效，保证了大多数服务依然可用。

但是这给我们的开发带来了麻烦， 因此开发阶段我们都会关闭自我保护模式：

```yaml
eureka:
  server:
    enable-self-preservation: false # 关闭自我保护模式（缺省为打开）
    eviction-interval-timer-in-ms: 1000 # 扫描失效服务的间隔时间（缺省为60*1000ms）
```

## 3.2、nacos

# 4、负载均衡Robbin

在刚才的案例中，我们启动了一个user-service，然后通过DiscoveryClient来获取服务实例信息，然后获取ip和端口来访问。

但是实际环境中，我们往往会开启很多个user-service的集群。此时我们获取的服务列表中就会有多个，到底该访问哪一个呢？

一般这种情况下我们就需要编写负载均衡算法，在多个实例列表中进行选择。



目前主流的负载方案分为以下两种：

1. 集中式负载均衡，在消费者和服务提供方中间使用独立的代理方式进行负载，有硬件的（比如 F5），也有软件的（比如 Nginx）。
2. 客户端自己做负载均衡，根据自己的请求情况做负载，Ribbon 就属于客户端自己做负载。



不过Eureka中已经帮我们集成了负载均衡组件：Ribbon，简单修改代码即可使用。

什么是Ribbon：

![1567778523449](assets/1567778523449.png)

## 4.1、启动两个服务实例

首先我们启动两个user-service实例，一个8081，一个8082。

![1567778646988](assets/1567778646988.png)

Eureka监控面板：

![1567778657591](assets/1567778657591.png)

## 4.2、开启负载均衡

因为Eureka中已经集成了Ribbon，所以我们无需引入新的依赖。直接修改代码：

在RestTemplate的配置方法上添加`@LoadBalanced`注解：

```java
@Bean
@LoadBalanced
public RestTemplate restTemplate() {
    return new RestTemplate(new OkHttp3ClientHttpRequestFactory());
}
```

修改调用方式，不再手动获取ip和端口，而是直接通过服务名称调用：

```java
@Service
public class UserService {

    @Autowired
    private RestTemplate restTemplate;

    @Autowired
    private DiscoveryClient discoveryClient;

    public List<User> queryUserByIds(List<Long> ids) {
        List<User> users = new ArrayList<>();
        // 地址直接写服务名称即可
        String baseUrl = "http://user-service/user/";
        ids.forEach(id -> {
            // 我们测试多次查询，
            users.add(this.restTemplate.getForObject(baseUrl + id, User.class));
            // 每次间隔500毫秒
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        return users;
    }
}
```

访问页面，查看结果：

![1567778727392](assets/1567778727392.png)

## 4.3、源码跟踪

为什么我们只输入了service名称就可以访问了呢？之前还要获取ip和端口。

显然有人帮我们根据service名称，获取到了服务实例的ip和端口。它就是LoadBalancerInterceptor

我们进行源码跟踪：

![1567778754624](assets/1567778754624.png)

继续跟入execute方法：发现获取了8082端口的服务

![1567778770048](assets/1567778770048.png)

再跟下一次，发现获取的是8081：

![1567778779974](assets/1567778779974.png)

## 4.4、负载均衡策略

Ribbon默认的负载均衡策略是简单的轮询，我们可以测试一下：

编写测试类，在刚才的源码中我们看到拦截中是使用RibbonLoadBalanceClient来进行负载均衡的，其中有一个choose方法，是这样介绍的：

![1567778800846](assets/1567778800846.png)

现在这个就是负载均衡获取实例的方法。

我们对注入这个类的对象，然后对其测试：

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = UserConsumerDemoApplication.class)
public class LoadBalanceTest {

    @Autowired
    RibbonLoadBalancerClient client;

    @Test
    public void test(){
        for (int i = 0; i < 100; i++) {
            ServiceInstance instance = this.client.choose("user-service");
            System.out.println(instance.getHost() + ":" + instance.getPort());
        }
    }
}
```

结果：

![1567778831611](assets/1567778831611.png)

符合了我们的预期推测，==确实是轮询方式==。

我们是否可以修改负载均衡的策略呢？

继续跟踪源码，发现这么一段代码：

![1567778849734](assets/1567778849734.png)

我们看看这个rule是谁：

![1567778861305](assets/1567778861305.png)

这不就是轮询的意思嘛。

我们注意到，这个类其实是实现了接口IRule的，查看一下：

![1567778872928](assets/1567778872928.png)

定义负载均衡的规则接口。

它有以下实现：

![1567778884173](assets/1567778884173.png)

SpringBoot也帮我们提供了修改负载均衡规则的配置入口：

```yaml
user-service: # 这个是服务名称
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
```

==格式是：`{服务名称}.ribbon.NFLoadBalancerRuleClassName`，值就是IRule的实现类。==

再次测试，发现结果变成了随机：

![1567778923012](assets/1567778923012.png)

## 4.5、重试机制

Eureka的服务治理强调了CAP原则中的AP，即可用性和可靠性。它与Zookeeper这一类强调CP（一致性，可靠性）的服务治理框架最大的区别在于：Eureka为了实现更高的服务可用性，牺牲了一定的一致性，极端情况下它宁愿接收故障实例也不愿丢掉健康实例，正如我们上面所说的自我保护机制。

但是，此时如果我们调用了这些不正常的服务，调用就会失败，从而导致其它服务不能正常工作！这显然不是我们愿意看到的。

我们现在关闭一个user-service实例：

![1567778987073](assets/1567778987073.png)

因为服务剔除的延迟，consumer并不会立即得到最新的服务列表，此时再次访问你会得到错误提示：

![1567778998359](assets/1567778998359.png)

但是此时，8081服务其实是正常的。

因此Spring Cloud 整合了Spring Retry 来增强RestTemplate的重试能力，当一次服务调用失败后，不会立即抛出一次，而是再次重试另一个服务。

只需要简单配置即可实现Ribbon的重试：

```yaml
spring:
  cloud:
    loadbalancer:
      retry:
        enabled: true # 开启Spring Cloud的重试功能
user-service:
  ribbon:
    ConnectTimeout: 250 # Ribbon的连接超时时间
    ReadTimeout: 1000 # Ribbon的数据读取超时时间
    OkToRetryOnAllOperations: true # 是否对所有操作都进行重试
    MaxAutoRetriesNextServer: 1 # 切换实例的重试次数
    MaxAutoRetries: 1 # 对当前实例的重试次数
```

 根据如上配置，当访问到某个服务超时后，它会再次尝试访问下一个服务实例，如果不行就再换一个实例，如果不行，则返回失败。切换次数取决于MaxAutoRetriesNextServer参数的值

引入spring-retry依赖

```xml
<dependency>
    <groupId>org.springframework.retry</groupId>
    <artifactId>spring-retry</artifactId>
</dependency>
```

我们重启user-consumer-demo，测试，发现即使user-service2宕机，也能通过另一台服务实例获取到结果！

![1567779048404](assets/1567779048404.png)



# 5、Hystix

## 5.1、简介

Hystix，即熔断器。

主页：https://github.com/Netflix/Hystrix/

![1525658740266](assets/1525658740266.png)



Hystix是Netflix开源的一个延迟和容错库，用于隔离访问远程服务、第三方库，防止出现级联失败。

![1525658562507](assets/1525658562507.png)

## 5.2、熔断器的工作机制：

![1525658640314](assets/1525658640314.png)

正常工作的情况下，客户端请求调用服务API接口：

![1525658906255](assets/1525658906255.png)

当有服务出现异常时，直接进行失败回滚，服务降级处理

当服务繁忙时，如果服务出现异常，不是粗暴的直接报错，而是返回一个友好的提示，虽然拒绝了用户的访问，但是会返回一个结果。

这就好比去买鱼，平常超市买鱼会额外赠送杀鱼的服务。等到逢年过节，超时繁忙时，可能就不提供杀鱼服务了，这就是服务的降级。

系统特别繁忙时，一些次要服务暂时中断，优先保证主要服务的畅通，一切资源优先让给主要服务来使用，在双十一、618时，京东天猫都会采用这样的策略。



## 5.3、动手实践

### 5.3.1、引入依赖

首先在服务消费者（user-consumer）中引入Hystix依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

### 5.3.2、开启熔断-改造消费者

我们改造服务消费者（user-consumer），添加一个用来访问的user服务的DAO，并且声明一个失败时的回滚处理函数：

```java
@Component
public class UserDao {

    @Autowired
    private RestTemplate restTemplate;

    private static final Logger logger = LoggerFactory.getLogger(UserDao.class);

    @HystrixCommand(fallbackMethod = "queryUserByIdFallback")
    public User queryUserById(Long id){
        long begin = System.currentTimeMillis();
        String url = "http://user-service/user/" + id;
        User user = this.restTemplate.getForObject(url, User.class);
        long end = System.currentTimeMillis();
        // 记录访问用时：
        logger.info("访问用时：{}", end - begin);
        return user;
    }

    public User queryUserByIdFallback(Long id){
        User user = new User();
        user.setId(id);
        user.setName("用户信息查询出现异常！");
        return user;
    }
}
```

> 【说明】
>
> ①、`@HystrixCommand(fallbackMethod="queryUserByIdFallback")`：声明一个失败回滚处理函数`queryUserByIdFallback`，当`queryUserById`执行超时（默认是1000毫秒），就会执行fallback函数，返回错误提示。
>
> ②、为了方便查看熔断的触发时机，我们记录请求访问时间。



在原来的业务逻辑中调用这个DAO：

```java
@Service
public class UserService {

    @Autowired
    private UserDao userDao;

    public List<User> queryUserByIds(List<Long> ids) {
        List<User> users = new ArrayList<>();
        ids.forEach(id -> {
            // 我们测试多次查询，
            users.add(this.userDao.queryUserById(id));
        });
        return users;
    }
}
```

### 5.3.3、改造服务提供者

改造服务提供者（user-service），随机休眠一段时间，以触发熔断：

```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    public User queryById(Long id) throws InterruptedException {
        // 为了演示超时现象，我们在这里然线程休眠,时间随机 0~2000毫秒
        Thread.sleep(new Random().nextInt(2000));
        return this.userMapper.selectByPrimaryKey(id);
    }
}

```

### 5.3.4、启动测试

然后运行并查看日志：

id为9、10、11的访问时间分别是：

 ![1525661641660](assets/1525661641660.png)

id为12的访问时间：

 ![1525661669136](assets/1525661669136.png)

因此，只有12是正常访问，其它都会触发熔断，我们来查看结果：

![1525661720656](assets/1525661720656.png)



### 5.3.5、优化

虽然熔断实现了，但是我们的重试机制似乎没有生效，是这样吗？

其实这里是因为我们的Ribbon超时时间设置的是1000ms:

​	![1525666632542](assets/1525666632542.png)

而Hystix的超时时间默认也是1000ms，因此重试机制没有被触发，而是先触发了熔断。

所以，Ribbon的超时时间一定要小于Hystix的超时时间。

我们可以通过`hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds`来设置Hystrix超时时间。

```yaml
hystrix:
  command:
  	default:
        execution:
          isolation:
            thread:
              timeoutInMillisecond: 6000 # 设置hystrix的超时时间为6000ms
```



# 6、Feign

在前面的学习中，我们使用了Ribbon的负载均衡功能，大大简化了远程调用时的代码：

```java
String baseUrl = "http://user-service/user/";
User user = this.restTemplate.getForObject(baseUrl + id, User.class)
```

如果就学到这里，你可能以后需要编写类似的大量重复代码，格式基本相同，无非参数不一样。有没有更优雅的方式，来对这些代码再次优化呢？

这就是我们接下来要学的Feign的功能了。

## 6.1、简介

有道词典的英文解释：

​	![1525662976679](assets/1525662976679.png)

为什么叫伪装？

Feign可以把Rest的请求进行隐藏，伪装成类似SpringMVC的Controller一样。你不用再自己拼接url，拼接参数等等操作，一切都交给Feign去做。



项目主页：https://github.com/OpenFeign/feign

![1525652009416](assets/1525652009416.png)

## 6.2、快速入门

### 6.2.1、导入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

### 6.2.2、Feign的客户端

```java
@FeignClient("user-service")
public interface UserFeignClient {

    @GetMapping("/user/{id}")
    User queryUserById(@PathVariable("id") Long id);
}
```

> 【说明】
>
> ①、首先这是一个接口，Feign会通过动态代理，帮我们生成实现类。这点跟mybatis的mapper很像
>
> ②、`@FeignClient`，声明这是一个Feign客户端，类似`@Mapper`注解。同时通过`value`属性指定服务名称
>
> ③、接口中的定义方法，完全采用SpringMVC的注解，Feign会根据注解帮我们生成URL，并访问获取结果



改造原来的调用逻辑，不再调用UserDao：

```java
@Service
public class UserService {

    @Autowired
    private UserFeignClient userFeignClient;

    public List<User> queryUserByIds(List<Long> ids) {
        List<User> users = new ArrayList<>();
        ids.forEach(id -> {
            // 我们测试多次查询，
            users.add(this.userFeignClient.queryUserById(id));
        });
        return users;
    }
}
```

### 6.2.3、开启Feign功能

我们在启动类上，添加注解，开启Feign功能

```java
@SpringBootApplication
@EnableDiscoveryClient
@EnableHystrix
@EnableFeignClients // 开启Feign功能
public class UserConsumerDemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserConsumerDemoApplication.class, args);
    }
}
```

- 你会发现RestTemplate的注册被我删除了。Feign中已经自动集成了Ribbon负载均衡，因此我们不需要自己定义RestTemplate了

### 6.2.4、启动测试：

访问接口：

 ![1525666476326](assets/1525666476326.png)

正常获取到了结果。

## 6.3、负载均衡

Feign中本身已经集成了Ribbon依赖和自动配置：

​	![1525672070679](assets/1525672070679.png)

因此我们不需要额外引入依赖，也不需要再注册`RestTemplate`对象。

另外，我们可以像上节课中讲的那样去配置Ribbon，可以通过`ribbon.xx`来进行全局配置。也可以通过`服务名.ribbon.xx`来对指定服务配置：

```yaml
user-service:
  ribbon:
    ConnectTimeout: 250 # 连接超时时间(ms)
    ReadTimeout: 1000 # 通信超时时间(ms)
    OkToRetryOnAllOperations: true # 是否对所有操作重试
    MaxAutoRetriesNextServer: 1 # 同一服务不同实例的重试次数
    MaxAutoRetries: 1 # 同一实例的重试次数
```

## 6.4、Hystix支持

Feign默认也有对Hystix的集成：

​	![1525672466192](assets/1525672466192.png)

只不过，默认情况下是关闭的。我们需要通过下面的参数来开启：

```yaml
feign:
  hystrix:
    enabled: true # 开启Feign的熔断功能
```

但是，Feign中的Fallback配置不像Ribbon中那样简单了。

**1）首先，我们要定义一个类，实现刚才编写的UserFeignClient，作为fallback的处理类**

```java
@Component
public class UserFeignClientFallback implements UserFeignClient {
    @Override
    public User queryUserById(Long id) {
        User user = new User();
        user.setId(id);
        user.setName("用户查询出现异常！");
        return user;
    }
}
```

**2）然后在UserFeignClient中，指定刚才编写的实现类**

```java
@FeignClient(value = "user-service", fallback = UserFeignClientFallback.class)
public interface UserFeignClient {

    @GetMapping("/user/{id}")
    User queryUserById(@PathVariable("id") Long id);
}

```

**3）重启测试：**

我们关闭user-service服务，然后在页面访问：

![1525673049875](assets/1525673049875.png)

## 6.5、请求压缩(了解)

Spring Cloud Feign 支持对请求和响应进行GZIP压缩，以减少通信过程中的性能损耗。通过下面的参数即可开启请求与响应的压缩功能：

```yaml
feign:
  compression:
    request:
      enabled: true # 开启请求压缩
    response:
      enabled: true # 开启响应压缩
```

同时，我们也可以对请求的数据类型，以及触发压缩的大小下限进行设置：

```yaml
feign:
  compression:
    request:
      enabled: true # 开启请求压缩
      mime-types: text/html,application/xml,application/json # 设置压缩的数据类型
      min-request-size: 2048 # 设置触发压缩的大小下限
```

注：上面的数据类型、压缩大小下限均为默认值。



## 6.6、日志级别(了解)

前面讲过，通过`logging.level.xx=debug`来设置日志级别。然而这个对Fegin客户端而言不会产生效果。因为`@FeignClient`注解修改的客户端在被代理时，都会创建一个新的Fegin.Logger实例。我们需要额外指定这个日志的级别才可以。

1）设置com.leyou包下的日志级别都为debug

```yaml
logging:
  level:
    com.leyou: debug
```

2）编写配置类，定义日志级别

```java
@Configuration
public class FeignConfig {
    @Bean
    Logger.Level feignLoggerLevel(){
        return Logger.Level.FULL;
    }
}
```

这里指定的Level级别是FULL，Feign支持4种级别：

​	![1525674373507](assets/1525674373507.png)

- NONE：不记录任何日志信息，这是默认值。
- BASIC：仅记录请求的方法，URL以及响应状态码和执行时间
- HEADERS：在BASIC的基础上，额外记录了请求和响应的头信息
- FULL：记录所有请求和响应的明细，包括头信息、请求体、元数据。



3）在FeignClient中指定配置类：

```java
@FeignClient(value = "user-service", fallback = UserFeignClientFallback.class, configuration = FeignConfig.class)
public interface UserFeignClient {
    @GetMapping("/user/{id}")
    User queryUserById(@PathVariable("id") Long id);
}
```

4）重启项目，即可看到每次访问的日志：

![1525674544569](assets/1525674544569.png)



# 7、Zuul网关

通过前面的学习，使用Spring Cloud实现微服务的架构基本成型，大致是这样的：

![1525674644660](assets/1525674644660.png)



我们使用Spring Cloud Netflix中的Eureka实现了服务注册中心以及服务注册与发现；而服务间通过Ribbon或Feign实现服务的消费以及均衡负载；通过Spring Cloud Config实现了应用多环境的外部化配置以及版本管理。为了使得服务集群更为健壮，使用Hystrix的融断机制来避免在微服务架构中个别服务出现异常时引起的故障蔓延。

在该架构中，我们的服务集群包含：内部服务Service A和Service B，他们都会注册与订阅服务至Eureka Server，而Open Service是一个对外的服务，通过均衡负载公开至服务调用方。我们把焦点聚集在对外服务这块，直接暴露我们的服务地址，这样的实现是否合理，或者是否有更好的实现方式呢

先来说说这样架构需要做的一些事儿以及存在的不足：

- 首先，破坏了服务无状态特点。
  - 为了保证对外服务的安全性，我们需要实现对服务访问的权限控制，而开放服务的权限控制机制将会贯穿并污染整个开放服务的业务逻辑，这会带来的最直接问题是，破坏了服务集群中REST API无状态的特点。
  - 从具体开发和测试的角度来说，在工作中除了要考虑实际的业务逻辑之外，还需要额外考虑对接口访问的控制处理。
- 其次，无法直接复用既有接口。
  - 当我们需要对一个即有的集群内访问接口，实现外部服务访问时，我们不得不通过在原有接口上增加校验逻辑，或增加一个代理调用来实现权限控制，无法直接复用原有的接口。
- **为什么要使用网关**：不同的微服务一般会有不同的网络地址，而外部客户端（例如手机APP）可能需要调用多个服务的接口才能完成一个业务需求。例如一个电影购票的手机APP，可能会调用多个微服务的接口，才能完成一次购票的业务流程，如下图所示。

面对类似上面的问题，我们要如何解决呢？答案是：服务网关！

为了解决上面这些问题，我们需要将权限控制这样的东西从我们的服务单元中抽离出去，而最适合这些逻辑的地方就是处于对外访问最前端的地方，我们需要一个更强大一些的均衡负载器的 服务网关。

服务网关是微服务架构中一个不可或缺的部分。通过服务网关统一向外系统提供REST API的过程中，除了具备服务路由、均衡负载功能之外，它还具备了==权限控制==等功能。Spring Cloud Netflix中的Zuul就担任了这样的一个角色，为微服务架构提供了前门保护的作用，同时将权限控制这些较重的非业务逻辑内容迁移到服务路由层面，使得服务集群主体能够具备更高的可复用性和可测试性。



## 7.1、简介

官网：https://github.com/Netflix/zuul

​	![1525675037152](assets/1525675037152.png)

Zuul：维基百科：

电影《捉鬼敢死队》中的怪兽，Zuul，在纽约引发了巨大骚乱。

事实上，在微服务架构中，Zuul就是守门的大Boss！一夫当关，万夫莫开！

![1525675168152](assets/1525675168152.png)





## 7.2、Zuul加入后的架构

![1525675648881](assets/1525675648881.png)



不管是来自于客户端（PC或移动端）的请求，还是服务内部调用。一切对服务的请求都会经过Zuul这个网关，然后再由网关来实现 鉴权、动态路由等等操作。Zuul就是我们服务的统一入口。

## 7.3、快速入门

### 7.3.1、新建工程

填写基本信息：

![1525675928548](assets/1525675928548.png)

添加Zuul依赖：

![1525675991833](assets/1525675991833.png)

### 7.3.2、编写启动类

通过`@EnableZuulProxy `注解开启Zuul的功能：

```java
@SpringBootApplication
@EnableZuulProxy // 开启Zuul的网关功能
public class ZuulDemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(ZuulDemoApplication.class, args);
	}
}
```

### 7.3.3、编写配置

```yaml
server:
  port: 10010 #服务端口
spring: 
  application:  
    name: api-gateway #指定服务名
```

### 7.3.4、编写路由规则

我们需要用Zuul来代理user-service服务，先看一下控制面板中的服务状态：

![1525676797879](assets/1525676797879.png)

- ip为：127.0.0.1
- 端口为：8081

映射规则：

```yaml
zuul:
  routes:
    user-service: # 这里是路由id，随意写
      path: /user-service/** # 这里是映射路径
      url: http://127.0.0.1:8081 # 映射路径对应的实际url地址
```

我们将符合`path` 规则的一切请求，都代理到 `url`参数指定的地址

本例中，我们将 `/user-service/**`开头的请求，代理到http://127.0.0.1:8081

### 7.3.5、启动测试：

访问的路径中需要加上配置规则的映射路径，我们访问：http://127.0.0.1:10010/user-service/user/10

​	![1525677046705](assets/1525677046705.png)



## 7.4、面向服务的路由

在刚才的路由规则中，我们把路径对应的服务地址写死了！如果同一服务有多个实例的话，这样做显然就不合理了。

我们应该根据服务的名称，去Eureka注册中心查找 服务对应的所有实例列表，然后进行动态路由才对！



### 7.4.1、添加Eureka客户端依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```



### 7.4.2、开启Eureka客户端发现功能

```java
@SpringBootApplication
@EnableZuulProxy // 开启Zuul的网关功能
@EnableDiscoveryClient
public class ZuulDemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(ZuulDemoApplication.class, args);
	}
}
```

### 7.4.3、添加Eureka配置，获取服务信息

```yaml
eureka:
  client:
    registry-fetch-interval-seconds: 5 # 获取服务列表的周期：5s
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
  instance:
    prefer-ip-address: true
    ip-address: 127.0.0.1
```

### 7.4.4、修改映射配置，通过服务名称获取

因为已经有了Eureka客户端，我们可以从Eureka获取服务的地址信息，因此映射时无需指定IP地址，而是通过服务名称来访问，而且Zuul已经集成了Ribbon的负载均衡功能。

```yaml
zuul:
  routes:
    user-service: # 这里是路由id，随意写
      path: /user-service/** # 这里是映射路径
      serviceId: user-service # 指定服务名称
```



### 7.4.5、启动测试

再次启动，这次Zuul进行代理时，会利用Ribbon进行负载均衡访问：

​	![1525677821212](assets/1525677821212.png)

日志中可以看到使用了负载均衡器：

![1525677891119](assets/1525677891119.png)



## 7.5、简化的路由配置

在刚才的配置中，我们的规则是这样的：

- `zuul.routes.<route>.path=/xxx/**`： 来指定映射路径。`<route>`是自定义的路由名
- `zuul.routes.<route>.serviceId=/user-service`：来指定服务名。

而大多数情况下，我们的`<route>`路由名称往往和 服务名会写成一样的。因此Zuul就提供了一种简化的配置语法：`zuul.routes.<serviceId>=<path>`

比方说上面我们关于user-service的配置可以简化为一条：

```yaml
zuul:
  routes:
    user-service: /user-service/** # 这里是映射路径
```

省去了对服务名称的配置。

## 7.6、默认的路由规则

在使用Zuul的过程中，上面讲述的规则已经大大的简化了配置项。但是当服务较多时，配置也是比较繁琐的。因此Zuul就指定了默认的路由规则：默认情况下，一切服务的映射路径就是服务名本身。

- 例如服务名为：`user-service`，则默认的映射路径就是：`/user-service/**`

也就是说，刚才的映射规则我们完全不配置也是OK的，不信就试试看。

## 7.7、路由前缀

配置示例：

```yaml
zuul:
  prefix: /api # 添加路由前缀
  routes:
      user-service: # 这里是路由id，随意写
        path: /user-service/** # 这里是映射路径
        service-id: user-service # 指定服务名称
```

我们通过`zuul.prefix=/api`来指定了路由的前缀，这样在发起请求时，路径就要以/api开头。

路径`/api/user-service/user/1`将会被代理到`/user-service/user/1`



## 7.8、过滤器

Zuul作为网关的其中一个重要功能，就是实现请求的鉴权。而这个动作我们往往是通过Zuul提供的过滤器来实现的。

### 7.8.1、ZuulFilter

ZuulFilter是过滤器的顶级父类。在这里我们看一下其中定义的4个最重要的方法：

```java
public abstract ZuulFilter implements IZuulFilter{
    abstract public String filterType();
    abstract public int filterOrder();
    boolean shouldFilter();// 来自IZuulFilter
    Object run() throws ZuulException;// IZuulFilter
}
```

> 【说明】
>
> ①、`filterType`：返回字符串，代表过滤器的类型。包含以下4种：
>
> - `pre`：请求在被路由之前执行
> - `routing`：在路由请求时调用
> - `post`：在routing和errror过滤器之后调用
> - `error`：处理请求时发生错误调用
>
> ②、`filterOrder`：通过返回的int值来定义过滤器的执行顺序，数字越小优先级越高。
>
> ③、`shouldFilter`：返回一个`Boolean`值，判断该过滤器是否需要执行。返回true执行，返回false不执行。
>
> ④、`run`：过滤器的具体业务逻辑。





### 7.8.2、过滤器执行生命周期：

这张是Zuul官网提供的请求生命周期图，清晰的表现了一个请求在各个过滤器的执行顺序。

​	![1525681866862](assets/1525681866862.png)

**正常流程**：

- 请求到达首先会经过pre类型过滤器，而后到达routing类型，进行路由，请求就到达真正的服务提供者，执行请求，返回结果后，会到达post过滤器。而后返回响应。

**异常流程**：

- 整个过程中，pre或者routing过滤器出现异常，都会直接进入error过滤器，再error处理完毕后，会将请求交给POST过滤器，最后返回给用户。
- 如果是error过滤器自己出现异常，最终也会进入POST过滤器，而后返回。
- 如果是POST过滤器出现异常，会跳转到error过滤器，但是与pre和routing不同的时，请求不会再到达POST过滤器了。

所有内置过滤器列表：

​	![1525682427811](assets/1525682427811.png)

### 378.3、使用场景

场景非常多：

- 请求鉴权：一般放在pre类型，如果发现没有访问权限，直接就拦截了
- 异常处理：一般会在error类型和post类型过滤器中结合来处理。
- 服务调用时长统计：pre和post结合使用。

## 7.9、自定义过滤器

接下来我们来自定义一个过滤器，模拟一个登录的校验。基本逻辑：如果请求中有access-token参数，则认为请求有效，放行。

### 7.9.1、定义过滤器类

```java
@Component
public class LoginFilter extends ZuulFilter{
    @Override
    public String filterType() {
        // 登录校验，肯定是在前置拦截
        return "pre";
    }

    @Override
    public int filterOrder() {
        // 顺序设置为1
        return 1;
    }

    @Override
    public boolean shouldFilter() {
        // 返回true，代表过滤器生效。
        return true;
    }

    @Override
    public Object run() throws ZuulException {
        // 登录校验逻辑。
        // 1）获取Zuul提供的请求上下文对象
        RequestContext ctx = RequestContext.getCurrentContext();
        // 2) 从上下文中获取request对象
        HttpServletRequest req = ctx.getRequest();
        // 3) 从请求中获取token
        String token = req.getParameter("access-token");
        // 4) 判断
        if(token == null || "".equals(token.trim())){
            // 没有token，登录校验失败，拦截
            ctx.setSendZuulResponse(false);
            // 返回401状态码。也可以考虑重定向到登录页。
            ctx.setResponseStatusCode(HttpStatus.UNAUTHORIZED.value());
        }
        // 校验通过，可以考虑把用户信息放入上下文，继续向后执行
        return null;
    }
}

```



### 7.9.2、测试

没有token参数时，访问失败：

​	![1525683285697](assets/1525683285697.png)

添加token参数后：

​	![1525683354113](assets/1525683354113.png)

## 7.10、负载均衡和熔断

Zuul中默认就已经集成了Ribbon负载均衡和Hystix熔断机制。但是所有的超时策略都是走的默认值，比如熔断超时时间只有1S，很容易就触发了。因此建议我们手动进行配置：

```yaml
zuul:
  retryable: true
ribbon:
  ConnectTimeout: 250 # 连接超时时间(ms)
  ReadTimeout: 2000 # 通信超时时间(ms)
  OkToRetryOnAllOperations: true # 是否对所有操作重试
  MaxAutoRetriesNextServer: 2 # 同一服务不同实例的重试次数
  MaxAutoRetries: 1 # 同一实例的重试次数
hystrix:
  command:
  	default:
        execution:
          isolation:
            thread:
              timeoutInMillisecond: 6000 # 熔断超时时长：6000ms
```

# 8、配置中心config

## 8.1、为什么要使用配置中心

集中管理配置。一个使用微服务架构的应用系统可能会包含成百上千个微服务，因此集中管理配置是非常有必要的；

不同环境，不同配置。例如，数据源配置在不同的环境（开发、测试、预发布、生产等）中是不同的；

运行期间可动态调整。例如，我们可根据各个微服务的负载情况，动态调整数据源连接池大小或熔断阈值，并且在调整配置时不停止微服务；

配置修改后可自动更新。如配置内容发生变化，微服务能够自动更新配置。

## 8.2、Spring Cloud Config简介

Spring Cloud Config为分布式系统外部化配置提供了服务器端和客户端的支持，它包括Config Server和Config Client两部分。由于Config Server和Config Client都实现了对Spring Environment和PropertySource抽象的映射，因此，Spring Cloud Config非常适合Spring应用程序，当然也可与任何其他语言编写的应用程序配合使用。

Config Server是一个可横向扩展、集中式的配置服务器，它用于集中管理应用程序各个环境下的配置，默认使用Git存储配置内容（也可使用Subversion、MySQL、本地文件系统或Vault存储配置，本博客以Git为例进行讲解），因此可以很方便地实现对配置的版本控制与内容审计。

Config Client是Config Server的客户端，用于操作存储在Config Server中的配置属性。引入Spring Cloud Config后的架构如下：

![Spring Cloud Config架构图](images\01_Spring Cloud\spring-cloud-config.png)

## 8.3、编写Config Server

### 8.3.1、示例

加依赖

```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

加注解：`@EnableConfigServer`

写配置：

```yaml
server:
  port: 8080
spring:
  application:
    name: microservice-config-server
  cloud:
    config:
      server:
        git:
          # Git仓库地址
          uri: https://git.oschina.net/itmuch/spring-cloud-config-repo.git
          # Git仓库账号
          username:
          # Git仓库密码
          password:    
```

### 8.3.2、路径规则

Spring Cloud Config Server提供了RESTful API，可用来访问存放在Git仓库中的配置文件。

```
/{application}/{profile}[/{label}]
/{application}-{profile}.yml
/{label}/{application}-{profile}.yml
/{application}-{profile}.properties
/{label}/{application}-{profile}.properties
```

其中的{appliation}、{profile}、{label} 都是占位符。

**TIPS**

事实上，可使用Spring Cloud Config实现配置的“继承”与“组合”，举个例子——

假设有一个应用：`microservice-foo` ，其profile是dev，那么其实Spring Cloud Config会查找如下几个文件：

- `microservice-foo-dev.yml`
- `microservice-foo.yml`
- `application-dev.yml`
- `application.yml`

对于相同属性的配置，从上至下优先级逐渐递减；最终获得的配置属性是四个文件的组合。由此，不难分析，可如下规划几个配置文件：

- `microservice-foo-dev.yml` 作为指定应用在指定profile下的配置文件
- `microservice-foo.yml` 作为制定应用在任何profile下都通用的配置文件
- `application-dev.yml` 作为所有应用在指定profile下的配置文件
- `application.yml` 作为所有应用在任何profile下都通用的配置文件

### 测试

- 访问`http://localhost:8080/microservice-foo-dev.yml` 可访问到Git仓库的`microservice-foo-dev.properties` 并组合`application.properties`。

## 集成Config Client

### 编码

- 加依赖

  ```
  <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
  </dependency>
  ```

- 加配置：applicaiton.yml

  ```
  server:
    port: 8081
  ```

- 加配置：bootstrap.yml

  ```
  spring:
    application:
      name: microservice-foo    # 对应config server所获取的配置文件的{application}
    cloud:
      config:
        uri: http://localhost:8080/
        profile: dev            # profile对应config server所获取的配置文件中的{profile} 
        label: master           # 指定Git仓库的分支，对应config server所获取的配置文件的{label}
  ```

  其中：

  spring.application.name：对应Config Server所获取的配置文件中的{application} ；

  spring.cloud.config.uri：指定Config Server的地址，默认是[http://localhost:8888](http://localhost:8888/) ；

  spring.cloud.config.profile：profile对应Config Server所获取的配置文件中的{profile} ；

  spring.cloud.config.label：指定Git仓库的分支，对应Config Server所获取配置文件的{label}。

  值得注意的是，以上属性应配置在bootstrap.yml，而不是application.yml中。如果配置在application.yml中，该部分配置就不能正常工作。例如，Config Client会连接spring.cloud.config.uri的默认值 `http://localhost:8888` ，而并非我们配置的 `http://localhost:8080/` 。

  Spring Cloud有一个“引导上下文”的概念，这是主应用程序上下文（Application Context）的父上下文。引导上下文负责从配置服务器加载配置属性，以及解密外部配置文件中的属性。和主应用程序加载`application.*` (yml或properties)中的属性不同，引导上下文加载`bootstrap.*` 中的属性。配置在`bootstrap.*` 中的属性有更高的优先级，因此默认情况下它们不能被本地配置覆盖。

- 写代码

  ```
  @RestController
  public class ConfigClientController {
    @Value("${profile}")
    private String profile;
  
    @GetMapping("/profile")
    public String hello() {
      return this.profile;
    }
  }
  ```

  ### 测试

访问`http://localhost:8081/profile` 可返回Git仓库中的配置属性。






































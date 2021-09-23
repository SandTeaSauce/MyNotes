#  1. Tomcat

## 1.1. web服务器软件

**服务器**：安装了服务器软件的计算机

**服务器软件**：接受用户的请求，处理请求，做出响应

**web服务器软件**：接受用户的请求，处理请求，做出响应

- 在web服务器软件中，可以部署web项目，让用户通过浏览器来访问这些项目
- 又称 web容器



常见的Java相关的web服务器软件：

- webLogic：oracle公司，大型的JavaEE服务器，支持所有的JavaEE规范（Java语音在企业级开发中使用的技术规范的综合，一个规定了13项大的规范），收费的
- webSphere：IBM公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的
- JBOSS：JBOSS公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的
- Tomcat：Apache基金组织，中小型的JavaEE服务器，仅仅支持少量的JavaEE规范Servlet/jsp。开源的，免费的



## 1.2. Tomcat

### 1.2.1. 安装与卸载

**下载地址**：http://tomcat.apache.org/



**安装**：解压压缩包即可。

> 【危险】安装目录建议不要有中文和空格



**卸载**：删除目录就行了

### 1.2.2. 目录结构

| 目录     | 用途                                                         |
| -------- | ------------------------------------------------------------ |
| /bin     | 存放启动和关闭Tomcat的脚本文件                               |
| /conf    | 存放Tomcat服务器的各种==配置文件==，其中包括server.xml（Tomcat的主要配置文件）、tomcat-users.xml和web.xml等配置文件 |
| /lib     | 存放Tomcat服务器和所有Web应用程序需要访问的==JAR文件==       |
| /logs    | 存放Tomcat的==日志文件==                                     |
| /temp    | 存放Tomcat运行时产生的临时文件                               |
| /webapps | 当发布Web应用程序时，通常把==Web应用程序==的目录及文件放到这个目录下 |
| /work    | Tomcat对JSP页面编译后生成的Servlet源文件和字节码文件放到这个目录下 |

### 1.2.3. 启动

**启动**：去`bin/startup.bat`，双击运行该文件

访问：浏览器输入：http://localhost:8080 

> 【推荐】**常见问题**：
>
> 1. 黑窗口一闪而过：
>
>    - 原因：没有正确配置JAVA_HOME环境变量
>    - 方法：正确配置JAVA_HOME环境变量
>
> 2. 启动窗口中出现中文乱码：
>
>    ![image-20210601100833179](images\01_Servlet开发\image-20210601100833179.png)
>
>    - 修改==conf/logging.properties==文件
>
>      ```properties
>      java.util.logging.ConsoleHandler.encoding=UTF-8
>      # 将上面一行修改为下面一行
>      java.util.logging.ConsoleHandler.encoding=GBK
>      ```
>
> 3. 启动报错：`BindExceptin：Address already in use；bind`，端口号冲突
>
>    一般会将Tomcat的默认端口号修改为80。80端口号是http协议的默认端口号
>
>    - 暴力：找到占用的端口号，并且找到对应的进程，杀死该进程
>
>      ```shell
>      # cmd 执行， 找PID为5488的值，然后在资源管理器/进程中结束进程
>      netstat -ano
>      ```
>
>    - 温柔：修改自身的端口号（`conf/server.xml`）
>
>      ```xml
>      <Connector port="80" protocol="HTTP/1.1"
>                 connectionTimeout="20000"
>                 redirectPort="8443" />
>      ```
>
> 

### 1.2.4. 关闭

正常关闭（推荐）：两种方法

1. ==bin/shutdown.bat==
2. `ctrl + C`

强制关闭：点击启动窗口的`X`

### 1.2.5. 配置

部署项目的方式：

1. 直接将项目放到==webapps==目录下即可

   - `/xiangmu`：项目的访问路径 → 虚拟路径
   - 简易部署：将项目打成一个war包，再将war包放置到webapps目录下。war包会自动解压
   - 缺点：项目访问路径名与war包名一致，war包必须放置在webapps目录下

2. 配置==conf/server.xml==文件：在`<Host>`标签体中配置

   ```xml
   <Context docBase="D:\hello" path="/hehe" />
   # docbase：项目存放的路径
     path：虚拟目录
   ```

   - 缺点：核心配置文件容易出问题

3. 在==conf\Catalina\localhost==下创建任意名称的xml文件。在文件中编写（推荐，热部署，不用关闭Tomcat）

   ```xml
   <Context docBase="D:\hello" />
   # 虚拟目录：xml文件的名称
   ```



Java项目目录结构

| 目录                     | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| \xiangmu                 | Web应用程序的根目录，属于此Web应用程序的所有文件都存在这个目录下 |
| \xiangmu\WEB-INF         | 存放Web应用程序的部署描述符文件 web.xml                      |
| \xiangmu\WEB-INF\classes | 存放Servlet和其他有用的类文件                                |
| \xiangmu\WEB-INF\lib     | 存放Web应用程序需要用到的==jar文件==，这些jar文件中可以包含Servlet、Bean和其他有用的类文件 |
| \xiangmu\WEB-INF\web.xml | web.xml文件中包含Web应用程序的配置和部署信息                 |

### 1.2.6. 集成到IDEA

在IDEA软件中，点 击`Run → Edit Configurations...`

![image-20210601164232335](images\01_Servlet开发\image-20210601164232335.png)

![image-20210601164335040](images\01_Servlet开发\image-20210601164335040.png)

### 1.2.7. IDEA创建Javaweb项目

新版本创建项目，没有==Java Enterprise==这个选项。步骤如下：

1. 创建普通Java项目

2. 有项目，选择`Add Framework Support...`，勾选后即可

   ![image-20210601165247323](images\01_Servlet开发\image-20210601165247323.png)



运行项目

![image-20210601170634805](images\01_Servlet开发\image-20210601170634805.png)

![image-20210601170722172](images\01_Servlet开发\image-20210601170722172.png)

### 1.2.8. tomcat控制台中文乱码

![在这里插入图片描述](images\01_Servlet开发\20200721111626689.png)

# 2. Servlet

## 2.1. Servlet 概念

Servlet全称：  server applet（运行在服务器端的小程序）

Servlet：是一个借口，定义了Java类被浏览器访问到（Tomcat识别）的规则

将来我们自定义的类，要实现Servlet接口，复写方法

## 2.2. 创建Servlet类

1. 创建JavaEE项目

2. 定义一个类，实现Servlet接口

   ```Java
   public class ServletDemo1 implements Servlet
   ```

3. 实现接口中的方法

4. 配置Servlet在web.xml中的配置

   ```xml
   <!--配置Servlet-->
   <servlet>
       <servlet-name>demo1</servlet-name>
       <servlet-class>cn.itcast.web.servlet.ServletDemo1</servlet-class>
   </servlet>
   <!--配置Servlet与映射-->
   <servlet-mapping>
       <servlet-name>demo1</servlet-name>
       <url-pattern>/demo1</url-pattern>
   </servlet-mapping>
   ```



绑定图解：

![Servlet](images\01_Servlet开发\Servlet.bmp)

## 2.3. 执行原理

执行图解：

![Servlet执行原理](images\01_Servlet开发\Servlet执行原理.bmp)

执行步骤：

1. 当服务器接受到客户端浏览器的请求后，会解析请求URL路径，获取访问的Servlet的资源路径
2. 查找web.xml文件，是否有对应的`<url-pattern>`标签体内容
3. 如果有，则再找对应的`<servlet-Class>`全类名
4. Tomcat会将字节码文件加载进内存中，并且创建其对象
5. 调用其方法（servlet方法）

## 2.4. Servlet的生命周期

### 2.4.1. 被创建

**被创建**：执行`init()`方法，只执行一次



Servlet什么时候被创建？

- 默认情况下，第一次被访问时，Servlet被创建

- 可以配置执行Servlet的创建时机。==web.xml==的`<servlet>`标签中配置

  - 第一次被访问时，创建

    ```
    <load-on-startup> 的值为负值
    ```

  - 在服务器启动时，创建  

    ```
    <load-on-startup> 的值为正值 或 0
    ```

  

> 【危险】
>
> 问题：Servlet的`init()`方法，只执行一次，说明一个Servlet在内存中只存在一个对象，Servlet是单例的。
>
> 方法：尽量在Servlet中定义成员变量。即使定义了成员变量了，也不要对其修改

### 2.4.2. 提供服务

**提供服务**：执行`service()`方法，执行多次

- 每次访问Servlet时，`service()`方法都会被调用一次

### 2.4.3. 被销毁

**被销毁**：执行`destroy()`方法，只执行一次

- Servlet被销毁时执行。服务器关闭时，Servlet被销毁

- 只有服务器==正常关闭==时，才会下`destroy()`方法
- `destroy()`方法在Servlet销毁之前执行，一般用于释放资源



## 2.5. Servlet3.0

好处：支持注解配置。可以不需要web.xml

**步骤**：

1. 创建JavaEE项目，选择Servlet的版本3.0以上，可以不创建web.xml
2. 定义一个类，实现Servlet接口
3. 复写方法
4. 在类上使用`@WebServlet(urlPatterns="/demo")`注解或`@WebServlet("/demo")`进行配置

```Java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface WebServlet {
    String name() default "";
    String[] value() default {};
    String[] urlPatterns() default {};
    int loadOnStartup() default -1;
    WebInitParam[] initParams() default {};
    boolean asyncSupported() default false;
    String smallIcon() default "";
    String largeIcon() default "";
    String description() default "";
    String displayName() default "";
}
```



## 2.6. IDEA与Tomcat的相关配置

IDEA会为每一个Tomcat部署的项目单独创建一份配置文件
查看控制台的log：`Using CATALINA_BASE:   "C:\Users\shang\AppData\Local\JetBrains\IntelliJIdea2021.1\tomcat\d338f8db-4e41-4a33-8c71-03f5987f7771"`

项目有两个地方存储：工作空间项目 和 Tomcat部署的web项目

- Tomcat真正访问的是==Tomcat部署的web项目==，==Tomcat部署的web项目==对应着==工作空间项目==的web目录下的所有资源
- WEB-INF目录下的资源不能被浏览器直接访问



## 2.7. Servlet的题型结构

### 2.7.1. GenericServlet

GenericServlet是Servlet的子类，抽象类。

- 将Servlet接口中的其他的方法做了默认空实现，只将`service()`方法作为抽象 
- 将来定义Servlet类时，可以继承GenericServlet，实现`service()`方法即可

### 2.7.2. HttpServlet

![image-20210602095630958](images\01_Servlet开发\image-20210602095630958.png)

HttpServlet是GenericServlet 的子类，抽象类。

- 对http协议的一种封装，简化操作

```Java
protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {        
    String method = req.getMethod();        
    long lastModified;        
    if (method.equals("GET")) {            
        lastModified = this.getLastModified(req);           
        if (lastModified == -1L) {               
            this.doGet(req, resp);            
        } else {                
            long ifModifiedSince;                
            try {                    i
                fModifiedSince = req.getDateHeader("If-Modified-Since");                
                } catch (IllegalArgumentException var9) {                    
                ifModifiedSince = -1L;                
            }                
            if (ifModifiedSince < lastModified / 1000L * 1000L) {                   
                this.maybeSetLastModified(resp, lastModified);                  
                this.doGet(req, resp);             
            } else {                   
                resp.setStatus(304);           
            }        
        }      
    } else if (method.equals("HEAD")) {    
        lastModified = this.getLastModified(req);    
        this.maybeSetLastModified(resp, lastModified);   
        this.doHead(req, resp);      
    } else if (method.equals("POST")) {          
        this.doPost(req, resp);      
    } else if (method.equals("PUT")) {      
        this.doPut(req, resp);    
    } else if (method.equals("DELETE")) {     
        this.doDelete(req, resp);     
    } else if (method.equals("OPTIONS")) {          
        this.doOptions(req, resp);     
    } else if (method.equals("TRACE")) {       
        this.doTrace(req, resp);      
    } else {          
        String errMsg = lStrings.getString("http.method_not_implemented");     
        Object[] errArgs = new Object[]{method};       
        errMsg = MessageFormat.format(errMsg, errArgs);        
        resp.sendError(501, errMsg);       
    }   
}
```



编写Servlet类

- 定义类继承HttpServlet
- 复写`doGet/doPost`方法



## 2.8. Servlet相关配置

**`@WebServlet`配置**

1. urlPartten：Servlet访问路径
   - 一个Servlet可以定义多个访问路径：`@WebServlet({"/d1", "/d2", "/d3"})`
   - 路径定义规则：
     - `/xxxx`
     - `/xxxx/xxxx`：多层路径，目录结构
     - `*.do`    不要有`/`，否则报错

# 3. HTTP

## 3.1. 概念

**概念**：Hyper Text Transfer Protocol 超文本传输协议

- 协议规定：定义了，客户端和服务器端通信时，发送数据的格式
- 特点：
  - 基于TCP/IP的高级协议
  - 默认端口号：80
  - 基于请求/响应模型的：一次请求对应一次响应
  - 无状态：每次请求之间互相独立，不能交互数据

![image-20210602100918340](images\01_Servlet开发\image-20210602100918340.png)

## 3.2. 历史版本

历史版本：

- 1.0：每一次请求响应都会建立一次新的连接
- 1.1：复用连接

## 3.3. 数据格式

### 3.3.1. 请求数据格式

请求消息：客户端发送给服务器端的数据

请求数据格式：请求行、请求头、请求空行、请求体

POST /login.html	HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:60.0) Gecko/20100101 Firefox/60.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Referer: http://localhost/login.html
Connection: keep-alive
Upgrade-Insecure-Requests: 1
		

#### ①. 请求行

请求方式  请求url  请求协议/版本

`GET   /login.html  HTTP/1.1`

- 请求方式：HTTP协议中有7种请求方式，常见的有2种
  1. get：
     - 请求参数在==请求头==中，在URL后
     - 请求的URL长度有限制的
     - 不太安全
  2. post：
     - 请求参数在==请求体==中
     - 请求的URL长度没有限制
     - 相对安全



#### ②. 请求头

作用：客户端浏览器告诉服务器一些信息

格式：`请求头名称: 请求头值`

常见的请求头：

1. User-Agen：浏览器告诉服务器，我访问你使用的浏览器版本信息

   可以在服务器端获取该头的信息，解决浏览器的兼容性问题

   火狐浏览器：`User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:60.0) Gecko/20100101 Firefox/60.0`

2. Referer: http://localhost/login.html

   告诉服务器，我（当前请求）从哪里来？

   作用：

   - 防盗链
   - 统计工作



#### ③. 请求空行

作用：用于分割POST请求的请求头和请求体的



#### ④. 请求体（正文）

作用：封装POST请求消息的请求参数的

### 3.3.2. 请求数据格式

响应信息：服务器发送给客户端的数据

数据格式：响应行、响应头、响应空行、响应体

#### ①. 响应行

组成：协议/版本   响应状态码   状态码描述
`HTTP/1.1 200 OK`

响应状态码：服务器告诉客户端浏览器本次请求和响应的一个状态

1. 状态都是3位数字
2. 分类：
   - 1xx：服务器接收客户端消息，但没有接受完成，等待一段时间后，发送1xx状态码
   - 2xx：成功。代表：200
   - 3xx：重定向。代表：302（重定向）、304（访问缓存）
   - 4xx：客户端错误。代表：404（请求路径没有对应的资源）、405（请求方式没有对应的doXxx方法）
   - 5xx：服务器端错误。代表：500（服务器内部出现异常）、



#### ②. 响应头

格式：`头名称 : 值`

常见的响应头：

1. Content-Type：服务器告诉客户端本次响应体数据格式以及编码格式
2. Content-disposition：服务器告诉客户端以什么格式打开响应体数据
   - in-line:默认值,在当前页面内打开
   - attachment;filename=xxx：以附件形式打开响应体。文件下载



#### ③. 响应空行



#### ④. 响应体

传输的数据

# 4. Request

## 4.1. 请求响应流程

![image-20210602133317066](images\01_Servlet开发\image-20210602133317066.png)

## 4.2. Request对象和Response对象的原理

request 和 response 对象由服务器创建。我们来使用它

request 对象是来获取请求消息

response 对象来设置响应信息

## 4.3. Request对象继承体系结构

 ServletRequest （接口）
           ↓ 继承
HttpServletRequest （接口）
           ↓ 实现
org.apache.catalina.connector.RequestFacade 类(tomcat) （类）



## 4.4. Request功能

Request功能主要分两部分：获取请求消息数据 和 其他功能

### 4.4.1. 获取请求消息数据

#### ①. 获取请求行数据

请求行格式：`GET /day14/demo1?name=zhangsan HTTP/1.1`

方法：

1. `String getMethod()`：获取请求方式。如：Get

2. :heavy_check_mark:`String getContextPath( )`：获取虚拟目录。如：/day14

3. `String getServletPath( )`：获取Servlet路径。如：/requestDemo1 （@WebServlet注解内容）

4. `String getQueryString( )`：获取请求参数。如：name=zhangsan

5. :heavy_check_mark:`String getRequestURI( )`：获取请求URI。如：/day14/demo1

   `StringBuffer getRequestURL( )`：获取请求URI。如：http://localhost/day14/demo1

   - URL：统一资源定位符： http://localhost/day14/demo1	比如：中华人民共和国
     URI：统一资源标识符： /day14/demo1				                   	比如：共和国
   - 以后权限控制可以用

6. `String getProtocol( )`：获取协议及版本。如：HTTP/1.1

7. `String getRemoteAddr( )`：获取客户机的IP地址。如：127.0.0.1



#### ②. 获取请求头数据

方法：

1. :heavy_check_mark:`String getHeader(String name)`：通过请求头的名称获取请求头的值
2. `Enumeration<String> getHeaderNames( )`：获取所有的请求头名称



#### ③. 获取请求体数据

请求体：只有POST请求方式，才有请求体，在请求体中封装了POST请求的请求参数

步骤：

1. 获取流对象
   - `BufferedReader getReader( )`：获取字符输入流，只能操作字符数据
   - `ServletInputStream getInputStream( )`：获取字节输入流，可以操作所有类型数据
2. 再从对象中获取数据



### 4.4.2. 其他功能

#### ①. 获取请求参数。通用方式

1. :heavy_check_mark:`String getParameter(String name)`：根据参数名称获取参数值。
2. `String[] getParameterValues(String name)`：根据参数名称获取参数值的数组。复选框
3. `Enumeration<String> getParameterNames( )`：获取所有请求的参数名称。
4. :heavy_check_mark:`Map<String, String[]> getParameterMap( )`：获取所有参数的map集合。

> 【推荐】
>
> 问题：获取的数据中文乱码
>
> 解决：
>
> 1. get方式：Tomcat8 已经将get方式乱码问题解决了
> 2. post方式：在获取参数之前，设置request的编码 `request.setCharacterEncoding("utf-8")`



#### ②. 请求转发

请求转发：一种在服务器内部的资源跳转方式

![image-20210602142858706](images\01_Servlet开发\image-20210602142858706.png)

**步骤**：

1. 通过request对象获取请求转发器对象：`RequestDispatcher getRequestDispatcher(String path)`
2. 使用RequestDispatcher对象来进行转发：`forward(ServletRequest request, ServletResponse response)` 

```Java
request.getRequestDispatcher("/lianjie").forward(request, response)
```



**特点**：

1. 浏览器地址栏路径不发生变化
2. 只能转发到当前服务器内部资源中
3. 转发是一次请求



#### ③. 共享数据

域对象：一个有作用范围的对象，可以在范围内共享数据

 request域：代表一次请求的范围，一般用于==请求转发==的多个资源中共享数据			

方法：
   				1. `void setAttribute(String name,Object obj)`：存储数据
   				2. `Object getAttitude(String name)`：通过键获取值
  				3. `void removeAttribute(String name)`：通过键移除键值对



#### ④. 获取ServletContext

 `ServletContext getServletContext()`



# 5. Response

## 5.1. Response功能

功能：设置响应消息

### 5.1.1. 设置响应行

格式：HTTP/1.1 200 ok

设置状态码：`setStatus(int sc)` 

### 5.1.2. 设置响应头

`setHeader(String name, String value)` 

### 5.1.3. 设置响应体

使用步骤

1. 获取输出流

   - 字符输出流：`PrintWriter getWriter()`

   - 字节输出流：`ServletOutputStream getOutputStream()`

2. 使用输出流，将数据输出到客户端浏览器

## 5.2. Response功能

### 5.2.1. 重定向

**重定向方式**；

1. 方式一

   ```java
   //1. 设置状态码为302
   response.setStatus(302);
   //2.设置响应头location
   response.setHeader("location","/day15/responseDemo2");
   ```

2. 方式二、简单的重定向方法
   `response.sendRedirect("/day15/responseDemo2");`



**重定向特点**：`sendRedirect()`

1. 地址栏发生变化
2. 重定向可以访问其他站点(服务器)的资源
3. 重定向是两次请求。不能使用request对象来共享数据



**路径分类**

1. 相对路径：通过相对路径不可以确定唯一资源
   - 如：`./index.html`
   - 不以/开头，以.开头路径
   - 规则：找到当前资源和目标资源之间的相对位置关系
     - `./`：当前目录
     - `../`：后退一级目录

2. :heavy_check_mark:绝对路径：通过绝对路径可以确定唯一资源
   - 如：`http://localhost/day15/responseDemo2`		`/day15/responseDemo2`
   - 以/开头的路径
   - 规则：判断定义的路径是给谁用的？判断请求将来从哪儿发出
     - 给客户端浏览器使用：需要加虚拟目录(项目的访问路径)
       - 建议虚拟目录动态获取：`request.getContextPath()`
       - `<a> , <form>` 重定向...
     - 给服务器使用：不需要加虚拟目录
       - 转发路径



### 5.2.2. 服务器输出字符数据到浏览器

 步骤：
   			1. 获取字符输出流
  			2. 输出数据

```Java
// 1. 获取字节输入流
PrintWriter pw = response.getWriter();
// 2. 输出数据
pw.writer("输出。。。。");
```



> 【推荐】
>
> 乱码问题：
>
>   			1. 设置该流的默认编码
>   	    `response.setCharacterEncoding("utf-8");`
>   			2. 告诉浏览器响应体使用的编码
>   	   `response.setContentType("text/html;charset=utf-8");` 
>   			3. `PrintWriter pw = response.getWriter();`// 获取的流的默认编码是ISO-8859-1



###    5.2.3. 服务器输出字节数据到浏览器

​	步骤：

1. 获取字节输出流
  2. 输出数据

```Java
// 1. 获取字节流输入
ServletOutputStream sos = response.getOutputStream();
// 2. 输出数据
sos.write("你好".getBytes(utf-8));
```

## 5.3. 验证码

本质：图片

目的：防止恶意表单注册

```Java
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        int width = 100;
        int height = 50;

        //1.创建一对象，在内存中图片(验证码图片对象)
        BufferedImage image = new BufferedImage(width,height,BufferedImage.TYPE_INT_RGB);

        //2.美化图片
        //2.1 填充背景色
        Graphics g = image.getGraphics();//画笔对象
        g.setColor(Color.PINK);//设置画笔颜色
        g.fillRect(0,0,width,height);
        //2.2画边框
        g.setColor(Color.BLUE);
        g.drawRect(0,0,width - 1,height - 1);
        //2.3写验证码
        String str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghigklmnopqrstuvwxyz0123456789";
        //生成随机角标
        Random ran = new Random();
        for (int i = 1; i <= 4; i++) {
            int index = ran.nextInt(str.length());
            //获取字符
            char ch = str.charAt(index);//随机字符
            //写验证码
            g.drawString(ch+"",width/5*i,height/2);
        }
        //2.4画干扰线
        g.setColor(Color.GREEN);
        //随机生成坐标点
        for (int i = 0; i < 10; i++) {
            int x1 = ran.nextInt(width);
            int x2 = ran.nextInt(width);

            int y1 = ran.nextInt(height);
            int y2 = ran.nextInt(height);
            g.drawLine(x1,y1,x2,y2);
        }
        //3.将图片输出到页面展示
        ImageIO.write(image,"jpg",response.getOutputStream());
    }
```

# 6. ServletContext对象

概念：代表整个web应用，可以和程序的容器(服务器)来通信

## 6.1. 获取

1.  通过request对象获取
      		`request.getServletContext();`
2. 通过HttpServlet获取
   		`this.getServletContext();`

## 6.2. 功能

获取MIME类型

- MIME类型：在互联网通信过程中定义的一种文件数据类型
- 格式：`大类型/小类型`   例如： text/html   image/jpeg
- `String getMimeType(String type)`：获取MIME类型



域对象：共享数据

- 方法
  - `setAttribute(String name,Object value)`
  - `getAttribute(String name)`
  - `removeAttribute(String name)`
- ServletContext对象范围：所有用户所有请求的数据



获取文件的真实（服务器）路径

- 方法：`String getRealPath(String path)`  

  ```java
  String b = context.getRealPath("/b.txt");//web目录下资源访问
  System.out.println(b);
  
  String c = context.getRealPath("/WEB-INF/c.txt");//WEB-INF目录下的资源访问
  System.out.println(c);
  
  String a = context.getRealPath("/WEB-INF/classes/a.txt");//src目录下的资源访问
  System.out.println(a);
  ```

  

## 6.3. 文件下载

### 6.3.1. 文件下载需求

 	1. 页面显示超链接
 	2. 点击超链接后弹出下载提示框
 	3. 完成图片文件下载

### 6.3.2. 分析

1. 超链接指向的资源如果能够被浏览器解析，则在浏览器中展示，如果不能解析，则弹出下载提示框。不满足需求
2. 任何资源都必须弹出下载提示框
3. 使用响应头设置资源的打开方式：
   `content-disposition:attachment;filename=xxx`

### 6.3.3. 步骤

 	1. 定义页面，编辑超链接href属性，指向Servlet，传递资源名称filename
 	2. 定义Servlet
  1. 获取文件名称
  2. 使用字节输入流加载文件进内存
  3. 指定response的响应头： `content-disposition:attachment;filename=xxx`
  4. 将数据写出到response输出流

### 6.3.4. 代码实现

```java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    //1.获取请求参数，文件名称
    String filename = request.getParameter("filename");
    //2.使用字节输入流加载文件进内存
    //2.1找到文件服务器路径
    ServletContext servletContext = this.getServletContext();
    String realPath = servletContext.getRealPath("/img/" + filename);
    //2.2用字节流关联
    FileInputStream fis = new FileInputStream(realPath);

    //3.设置response的响应头
    //3.1设置响应头类型：content-type
    String mimeType = servletContext.getMimeType(filename);//获取文件的mime类型
    response.setHeader("content-type",mimeType);
    //3.2设置响应头打开方式:content-disposition

    //解决中文文件名问题
    //1.获取user-agent请求头、
    String agent = request.getHeader("user-agent");
    //2.使用工具类方法编码文件名即可
    filename = DownLoadUtils.getFileName(agent, filename);

    response.setHeader("content-disposition","attachment;filename="+filename);
    //4.将输入流的数据写出到输出流中
    ServletOutputStream sos = response.getOutputStream();
    byte[] buff = new byte[1024 * 8];
    int len = 0;
    while((len = fis.read(buff)) != -1){
        sos.write(buff,0,len);
    }

    fis.close();
}
```

```html
<a href="/day15/downloadServlet?filename=九尾.jpg">图片1</a>
<a href="/day15/downloadServlet?filename=1.avi">视频</a>
```



### 6.3.5. 问题

	* 解决思路：
		 	1. 获取客户端使用的浏览器版本信息
	 	2. 根据不同的版本信息，设置filename的编码方式不同



# 7. 会话技术

会话：一次会话中包含多次请求和响应

- 一次会话：浏览器第一次给服务器资源发送请求，会话建立，直到有一方断开为止

功能：在一次会话的范围内的多次请求间，共享数据

方式：

1. 客户端会话技术：Cookie
2. 服务器端会话技术：Session

## 7.1. Cookie

概念：客户端会话技术，将数据保存到客户端

### 7.1.1. 使用Cookie

使用步骤

1. 创建Cookie对象，绑定数据

   `new Cookie(String name, String value)` 

2. 发送Cookie对象

   `response.addCookie(Cookie cookie)` 

3. 获取Cookie，拿到数据

   `Cookie[]  request.getCookies()`  

### 7.1.2. Cookie使用原理

原理：基于响应头 `set-cookie` 和 请求头 `cookie` 实现

![image-20210603100840567](images\01_Servlet开发\image-20210603100840567.png)

### 7.1.3. Cookie的细节

一次发送多个Cookie：创建多个Cookie对象，使用response调用多次`addCookie()`方法发送Cookie即可

```Java
//1.创建Cookie对象
Cookie c1 = new Cookie("msg","hello");
Cookie c2 = new Cookie("name","zhangsan");
//2.发送Cookie
response.addCookie(c1);
response.addCookie(c2);
```



Cookie在浏览器中保存时间

1. 默认情况下，当浏览器关闭后，Cookie数据被销毁

2. 持久化存储

   `setMaxAge(in seconds)`：

   - 正数：将Cookie数据写在硬盘的文件中。持久化存储。Cookie存活时间（单位：秒）
   - 负数：默认值
   - 零：删除Cookie信息



Cookie存储中文

- 在Tomcat 8之前Cookie中不能直接存储中文数据。

  - 需要将中文数据转码---一般采用URL编码(%十六进制    %E3)

- 在tomcat 8 之后，cookie支持中文数据。特殊字符还是不支持，建议使用URL编码存储，URL解码解析

  `URLEncoder.encode(String, String)`：编码。第一个String是需要编码的字符串，第二个String是解码格式，如utf-8

  `URLEncoder.decode(String, String)`：解码



Cookie共享问题

1. 假设在一个tomcat服务器中，部署了多个web项目，那么在这些web项目中cookie能不能共享？

   - 默认情况下，Cookie不能共享
   - `setPath(String path)`：设置Cookie的获取范围。默认情况下。设置当前的虚拟目录
     -  如果要共享，则可以将path设置为`"/"`：`cookie.setPath("/");`

2. 不同的tomcat服务器间cookie共享问题

   - `setDomain(String path)`：如果设置一级域名相同，那么多个服务器之间cookie可以共享

     ```Java
     // 那么tieba.baidu.com（百度贴吧）和news.baidu.com（百度新闻）中cookie可以共享
     cookie.setDomain(".baidu.com");
     ```

     

### 7.1.4. Cookie的特点与作用

特点：

1. cookie存储数据在客户端浏览器
2. 浏览器对于单个cookie 的大小有限制(4kb) 以及 对同一个域名下的总cookie数量也有限制(20个)

作用：

1. cookie一般用于存出少量的不太敏感的数据
2. 在不登录的情况下，完成服务器对客户端的身份识别



## 7.2. Session

### 7.2.1. 概念

概念：服务器端会话技术，在一次会话的多次请求间共享数据，将数据保存在服务器端的对象中。HttpSession

### 7.2.2. 快速入门

1. 获取HttpSession对象：
	`HttpSession session = request.getSession();`
	
2. 使用HttpSession对象：
	`Object getAttribute(String name)  `
	
	`void setAttribute(String name, Object value)`
	
	`void removeAttribute(String name)`  

### 7.2.3. 原理

Session的实现是依赖于Cookie的。

![image-20210603110643526](images\01_Servlet开发\image-20210603110643526.png)

### 7.2.4. 细节

当客户端关闭后，服务器不关闭，两次获取session是否为同一个？

- 默认情况下。不是。

- 如果需要相同，则可以创建Cookie,键为JSESSIONID，设置最大存活时间，让cookie持久化保存

  ```Java
  Cookie c = new Cookie("JSESSIONID",session.getId());
  c.setMaxAge(60*60);
  response.addCookie(c);
  ```

  

客户端不关闭，服务器关闭后，两次获取的session是同一个吗？

- 不是同一个，但是要确保数据不丢失。tomcat自动完成以下两步工作
  1. session的钝化：在服务器正常关闭之前，将session对象系列化到硬盘上
     2. session的活化：在服务器启动后，将session文件转化为内存中的session对象即可。



session什么时候被销毁？

1. 服务器关闭

2. session对象调用`invalidate()`销毁Session 。

3. session默认失效时间 30分钟
   			选择性配置修改	

   ```xml
   <session-config>
       <session-timeout>30</session-timeout>
   </session-config>
   ```

### 7.2.5. 特点

session的特点
  	 1. session用于存储一次会话的多次请求的数据，存在服务器端
 	 2. session可以存储任意类型，任意大小的数据



session与Cookie的区别：

1. session存储数据在服务器端，Cookie在客户端
2. session没有数据大小限制，Cookie有
3. session数据安全，Cookie相对于不安全

# 8. JSP

## 8.1. JSP基础

### 8.1.1. 概念

Java Server Pages： java服务器端页面
* 可以理解为：一个特殊的页面，其中既可以指定定义html标签，又可以定义java代码
* 用于简化书写！！！

### 8.1.2. 原理

JSP本质上就是一个Servlet

![image-20210603104713476](images\01_Servlet开发\image-20210603104713476.png)

### 8.1.3. JSP的脚本

JSP的脚本：JSP定义Java代码的方式
 1. `<%  代码 %>`：定义的java代码，==在service方法==中。service方法中可以定义什么，该脚本中就可以定义什么。

    ```jsp
    <%  
    	System.out.println("测试");
    %>
    ```

2. `<%! 代码 %>`：定义的java代码，在jsp转换后的java类的==成员位置==。（不推荐用）

   ```jsp
   <%! 
   	int i = 3;
   	// 成员变量，方法，代码块等
   %>
   ```

3. `<%= 代码 %>`：定义的java代码，==会输出到页面==上。输出语句中可以定义什么，该脚本中就可以定义什么。

   ```jsp
   <h1><%= i %><h1>
   ```



## 8.2. 指令

作用：用于配置JSP页面，导入资源文件

格式：`<%@ 指令名称 属性名1=属性值1 属性名2=属性值2 ... %>`

分类：

1. page：配置JSP页面的

   - `contentType`：等同于`response.setContentType()`
     - 设置响应体的mime类型以及字符集
     - 设置当前JSP页面的编码（只能是高级的IDE才能生效，如果使用低级工具，则需要设置`pageEncoding`属性设置当前页面的字符集）
   - `language`：指定语言，就java。鸡肋
   - `buffer`：缓存区大小设置
   - `import`：导包
   - `errorPage`：当前页面发生异常后，会自动跳转到指定的错误页面
   - `isErrorPage`：标识当前也是是否是错误页面。
     - true：是，可以使用内置对象exception
     - false：否。默认值。不可以使用内置对象exception

2. include：页面包含的。导入页面的资源文件

    `<%@include file="top.jsp"%>`

3. taglib：导入资源。一般用于导入标签库

   `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`

   - prefix：前缀，自定义的

## 8.3. 注释

html注释：
		`<!-- -->`：只能注释html代码片段

:heavy_check_mark:jsp注释：推荐使用
		`<%-- --%>`：可以注释所有





## 8.4. 内置对象

在jsp页面中不需要获取和创建，可以直接使用的对象

jsp一共有9个内置对象。

| 变量名      | 真实类型            | 作用                                         |
| ----------- | ------------------- | -------------------------------------------- |
| pageContext | PageContext         | 当前页面共享数据，还可以获取其他八个内置对象 |
| request     | HttpServletRequest  | 一次请求访问的多个资源（转发）               |
| session     | HttpSession         | 一次会话的多个请求间                         |
| application | ServletContext      | 所有用户间共享数据                           |
| response    | HttpServletResponse | 响应对象                                     |
| page        | Object              | 当前页面（servlet）对象。this                |
| out         | JspWriter           | 输出对象，输出数据到页面上                   |
| config      | ServletConfig       | Servlet的配置对象                            |
| exception   | Throwable           | 异常对象                                     |



其中重要详解：

1. out：字符输出流对象。可以将数据输出到页面上。和`response.getWriter()`类似
   - `response.getWriter()`和`out.write()`的区别：
     - 在tomcat服务器真正给客户端做出响应之前，会先找response缓冲区数据，再找out缓冲区数据。
     - `response.getWriter()`数据输出永远在`out.write()`之前



# 9. 三层架构

## 9.1 MVC

jsp演变历史

1. 早期只有servlet，只能使用response输出标签数据，非常麻烦
2. 后来又jsp，简化了Servlet的开发，如果过度使用jsp，在jsp中即写大量的java代码，有写html表，造成难于维护，难于分工协作
3. 再后来，java的web开发，借鉴mvc开发模式，使得程序的设计更加合理性



MVC：

1. M：Model，模型。JavaBean
   		* 完成具体的业务操作，如：查询数据库，封装对象
  2.  V：View，视图。JSP
          		* 展示数据
  3. C：Controller，控制器。Servlet
       - 获取用户的输入
       - 调用模型		
       - 将数据交给视图进行展示

![image-20210603162522042](images\01_Servlet开发\image-20210603162522042.png)



优缺点：
  1. 优点：

     - 耦合性低，方便维护，可以利于分工协作

      - 重用性高

	2. 缺点：
	 - 使得项目架构变得复杂，对开发人员要求高

## 9.2 三层架构：软件设计架构

界面层(表示层)：用户看的得界面。用户可以通过界面上的组件和服务器进行交互

业务逻辑层：处理业务逻辑的。

数据访问层：操作数据存储文件。

![image-20210603172538679](images\01_Servlet开发\image-20210603172538679.png)

# 10. EL表达式

**概念**：Expression Language 表达式语言

**作用**：替换和简化jsp页面中java代码的编写

**语法**：`${表达式}`

**注意**：jsp默认支持el表达式的。如果要忽略el表达式

- 设置jsp中page指令中：`isELIgnored="true"` 忽略当前jsp页面中所有的el表达式
- `\${表达式}` ：忽略当前这个el表达式

## 10.1. 使用

### 10.1.1. 运算符

1. 算数运算符： `+    -    *    /或div    %或mod`
2. 比较运算符： `>    <    >=    <=    ==    !=`
3. 逻辑运算符： `&&或and    ||或or    !或not`
  4. 空运算符： `empty`
       * 功能：用于判断字符串、集合、数组对象是否为null或者长度是否为0
      * `${empty list}`：判断字符串、集合、数组对象是否为null或者长度为0
     * `${not empty string}`：表示判断字符串、集合、数组对象是否不为null 并且 长度>0



### 10.1.2. 获取值

el表达式只能从域对象中获取值

语法：

1. `${域名称.键名}`：从指定域中获取指定键的值

   - 域名称：

     | 域名称             | 内置对象                      |
     | ------------------ | ----------------------------- |
     | pageScope          | pageContext                   |
     | requestScope       | request                       |
     | sessionScope       | session                       |
     | applicationScope a | application（ServletContext） |

   - 举例：在request域中存储了name=张三

     获取：`${requestScope.name}`

2. `${键名}`：表示依次从最小的域中查找是否有该键对应的值，直到找到为止。

3. 获取对象、List集合、Map集合的值
      - 对象：`${域名称.键名.属性名}`
           - 本质上会去调用对象的getter方法

      * List集合：
           * ${域名称.键名}：全部
           * `${域名称.键名[索引]}`：根据索引取
      * Map集合：
          * `${域名称.键名.key名称}`
         * `${域名称.键名["key名称"]}`

###  10.1.3. 隐式对象：

el表达式中有11个隐式对象	

pageContext：
   * 获取jsp其他八个内置对象
			* `${pageContext.request.contextPath}`：动态获取虚拟目录

# 11. JSTL标签

**概念**：JavaServer Pages Tag Library  JSP标准标签库

-  是由Apache组织提供的开源的免费的jsp标签	<标签>



**作用**：用于简化和替换jsp页面上的java代码		



**使用步骤**：

1. 导入jstl相关jar包
2. 引入标签库：taglib指令：  `<%@ taglib %>`
3. 使用标签



**常用的JSTL标签**

 1. `if`：相当于java代码的if语句

     - 属性：
        - test 必须属性，接受boolean表达式
        - 如果表达式为true，则显示if标签体内容，如果为false，则不显示标签体内容
       - 一般情况下，test属性值会结合el表达式一起使用
    - 注意：`c:if`标签没有else情况，想要else情况，则可以在定义一个`c:if`标签

2. `choose`：相当于java代码的switch语句

   - 使用`choose`标签声明，相当于switch声明
   - 使用`when`标签做判断，相当于case
   - 使用`otherwise`标签做其他情况的声明，相当于default

   ```jsp
    <c:choose>
        <c:when test="${number == 1}">星期一</c:when>
        <c:when test="${number == 2}">星期二</c:when>
        <c:when test="${number == 3}">星期三</c:when>
        <c:when test="${number == 4}">星期四</c:when>
        <c:when test="${number == 5}">星期五</c:when>
        <c:when test="${number == 6}">星期六</c:when>
        <c:when test="${number == 7}">星期天</c:when>
        <c:otherwise>数字输入有误</c:otherwise>
   </c:choose>
   ```

   

3. `foreach`：相当于java代码的for语句

   - 完成重复的操作
     -  begin：开始值
     - end：结束值
     -  var：临时变量
     -  step：步长
     -  varStatus：循环状态对象
       - index:容器中元素的索引，从0开始
       - count:循环次数，从1开始

     ```jsp
     <c:forEach begin="1" end="10" var="i" step="2" varStatus="s">
         ${i} <h3>${s.index}<h3> <h4> ${s.count} </h4><br>
     </c:forEach>
     ```

     

   - 遍历容器

     - items：容器对象
     - var：容器中元素的临时变量
     - varStatus:循环状态对象
       - index:容器中元素的索引，从0开始
       - count:循环次数，从1开始

     ```jsp
     <c:forEach items="${list}" var="str" varStatus="s">
         ${s.index} ${s.count} ${str}<br>
     </c:forEach>
     ```

# 12. Filter

## 12.1. 概念

生活中的过滤器：净水器、空气净化器、土匪

web中的过滤器：当访问服务器的资源时，过滤器可以将请求拦截下来，完成一些特殊的功能。

过滤器的作用：一般用于完成通用的操作。如：登录验证、统一编码处理、敏感字符过滤...

## 12.2. 快速入门

  		1. 定义一个类，实现接口`Filter`
  		2. 复写方法
  		3. 配置==拦截路径==
  - web.xml
  - 注解 ：`@WebFilter()`

```Java
public class FilterDemo1 implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("filterDemo1被执行了....");       
        //放行
        filterChain.doFilter(servletRequest,servletResponse);
    }

    @Override
    public void destroy() {
    }
}
```



## 12.3. 细节

### 12.3.1. web.xml配置	

```xml
<filter>
    <filter-name>demo1</filter-name>
    <filter-class>cn.itcast.web.filter.FilterDemo1</filter-class>
</filter>
<filter-mapping>
    <filter-name>demo1</filter-name>
    <!-- 拦截路径 -->
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

### 12.3.2. 执行流程

1. 执行过滤器
2. 执行放行后的资源
3. 回来执行过滤器放行代码下边的代码

```Java
public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
	// 【1】对Request对象的请求消息的增强    
	// 【2】放行
	filterChain.doFilter(servletRequest,servletResponse);
 	// 【3】对Response对象的响应信息的增强
}
```

### 12.3.3. 生命周期

`init`：在服务器启动后，会创建Filter对象，然后调用init方法。只执行一次。用于加载资源
`doFilter`：每一次请求被拦截资源时，会执行。执行多次
`destroy`：在服务器关闭后，Filter对象被销毁。如果服务器是正常关闭，则会执行destroy方法。只执行一次。用于释放资源

### 12.3.4. 过滤器配置详解

**拦截路径配置**：

1. 具体资源路径： `/index.jsp`   只有访问index.jsp资源时，过滤器才会被执行
2. 拦截目录： `/user/*`	访问/user下的所有资源时，过滤器都会被执行
3. 后缀名拦截： `*.jsp`		访问所有后缀名为jsp资源时，过滤器都会被执行
4. 拦截所有资源：`/`		访问所有资源时，过滤器都会被执行



**拦截方式配置**：资源被访问的方式

1. 注解配置：设置`dispatcherTypes`属性
   - REQUEST：默认值。浏览器直接请求资源
   - FORWARD：转发访问资源
   - INCLUDE：包含访问资源
   - ERROR：错误跳转资源
   - ASYNC：异步访问资源
2. web.xml配置
   在`<filter-mapping>`中设置`<dispatcher></dispatcher>`标签即可

### 12.3.5. 过滤器链（配置多个过滤器）

执行顺序：如果有两个过滤器——过滤器1和过滤器2
   			1. 过滤器1执行
  			2. 过滤器2执行
 			3. 资源执行
			4. 过滤器2执行
			5. 过滤器1 执行



过滤器先后顺序问题：
1. 注解配置：按照类名的字符串比较规则比较，类名的字符串值小的先执行
		* 如： AFilter 和 BFilter（A与B比较大小），AFilter就先执行了。
2. web.xml配置： `<filter-mapping>`谁定义在上边，谁先执行

# 13. Listener

**概念**：web的三大组件之一。

事件监听机制
  * 事件：一件事情
 * 事件源：事件发生的地方
* 监听器：一个对象
* 注册监听：将事件、事件源、监听器绑定在一起。 当事件源上发生某个事件后，执行监听器代码



ServletContextListener：监听ServletContext对象的创建和销毁

* 方法：
	* void contextDestroyed(ServletContextEvent sce) ：ServletContext对象被销毁之前会调用该方法
	* void contextInitialized(ServletContextEvent sce) ：ServletContext对象创建后会调用该方法
* 步骤：
	1. 定义一个类，实现ServletContextListener接口
	2. 复写方法
	3. 配置
		- web.xml
		
		   ```xml
		   <listener>
		   	<listener-class>cn.itcast.web.listener.ContextLoaderListener</listener-class>
		   </listener>
		   ```
		
		      - 指定初始化参数<context-param>
		
		 - 注解：
		
		    - `@WebListener`






























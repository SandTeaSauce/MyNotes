# 1. Maven 介绍

## 1.1. 什么是 Maven

### 1.1.1. 什么是 Maven 

Maven 的正确发音是[ˈmevən]，而不是“马瘟”以及其他什么瘟。Maven 在美国是一个口语化的词 语，代表专家、内行的意思。

 一个对 Maven 比较正式的定义是这么说的：Maven 是一个项目管理工具，它包含了一个**项目对象模型 (POM：Project Object Model)**，一组标准集合，一个项目生命周期(Project Lifecycle)，一个依赖管 理系统(Dependency Management System)，和用来运行定义在生命周期阶段(phase)中插件(plugin)目标 (goal)的逻辑。

### 1.1.2 Maven 能解决什么问题

可以用更通俗的方式来说明。我们知道，项目开发不仅仅是写写代码而已，期间会伴随着各种 必不可少的事情要做，下面列举几个感受一下： 

1. 我们需要引用各种 jar 包，尤其是比较大的工程，引用的 jar 包往往有几十个乃至上百个， 每用 到一种 jar 包，都需要手动引入工程目录，而且经常遇到各种让人抓狂的 jar 包冲突，版本冲突。
2. 我们辛辛苦苦写好了 Java 文件，可是只懂 0 和 1 的白痴电脑却完全读不懂，需要将它编译成二 进制字节码。好歹现在这项工作可以由各种集成开发工具帮我们完成，Eclipse、IDEA 等都可以将代 码即时编译。当然，如果你嫌生命漫长，何不铺张，也可以用记事本来敲代码，然后用 javac 命令一 个个地去编译，逗电脑玩。
3. 世界上没有不存在 bug 的代码，计算机喜欢 bug 就和人们总是喜欢美女帅哥一样。为了追求美为 了减少 bug，因此写完了代码，我们还要写一些单元测试，然后一个个的运行来检验代码质量。
4. 再优雅的代码也是要出来卖的。我们后面还需要把代码与各种配置文件、资源整合到一起，定型 打包，如果是 web 项目，还需要将之发布到服务器，供人蹂躏。 

试想，如果现在有一种工具，可以把你从上面的繁琐工作中解放出来，能帮你构建工程，管理 jar 包，编译代码，还能帮你自动运行单元测试，打包，生成报表，甚至能帮你部署项目，生成 Web 站 点，你会心动吗？Maven 就可以解决上面所提到的这些问题。

### 1.1.3 Maven 的优势举例

前面我们通过 Web 阶段项目，要能够将项目运行起来，就必须将该项目所依赖的一些 jar 包添加到 工程中，否则项目就不能运行。试想如果具有相同架构的项目有十个，那么我们就需要将这一份 jar 包复制到十个不同的工程中。我们一起来看一个 CRM项目的工程大小。 使用传统 Web 项目构建的 CRM 项目如下：

![image-20210604090743405](images\Maven\image-20210604090743405.png)

原因主要是因为上面的 WEB 程序要运行，我们必须将项目运行所需的 Jar 包复制到工程目录中，从而导致了工程很大。 同样的项目，如果我们使用 Maven 工程来构建，会发现总体上工程的大小会少很多。如下图

![image-20210604090811684](images\Maven\image-20210604090811684.png)

小结：可以初步推断它里面一定没有 jar 包，继续思考，没有 jar 包的项目怎么可能运行呢？

## 1.2. Maven 的两个精典作用

### 1.2.1 Maven 的依赖管理 

Maven 的一个核心特性就是依赖管理。当我们涉及到多模块的项目（包含成百个模块或者子项目），管理依赖就变成 一项困难的任务。Maven 展示出了它对处理这种情形的高度控制。 

传统的 WEB 项目中，我们必须将工程所依赖的 jar 包复制到工程中，导致了工程的变得很大。那么 maven 工程是如何使得工程变得很少呢？ 分析如下：

![image-20210604091033773](images\Maven\image-20210604091033773.png)

通过分析发现：maven 工程中不直接将 jar 包导入到工程中，而是通过在 pom.xml 文件中添加所需 jar 包的坐标，这样就很好的避免了 jar 直接引入进来，在需要用到 jar 包的时候，只要查找 pom.xml 文 件，再通过 pom.xml 文件中的坐标，到一个专门用于”存放 jar 包的仓库”(maven 仓库)中根据坐标从 而找到这些 jar 包，再把这些 jar 包拿去运行。 

那么问题来了 

1. ”存放 jar 包的仓库”长什么样？
2. 通过读取 pom.xml 文件中的坐标，再到仓库中找到 jar 包，会不会很慢？从而导致这种方式 不可行！

第一个问题：存放 jar 包的仓库长什么样，这一点我们后期会分析仓库的分类，也会带大家去看我们 的本地的仓库长什么样。 

第二个问题：通过 pom.xml 文件配置要引入的 jar 包的坐标，再读取坐标并到仓库中加载 jar 包，这 样我们就可以直接使用 jar 包了，为了解决这个过程中速度慢的问题，maven 中也有索引的概念，通 过建立索引，可以大大提高加载 jar 包的速度，使得我们认为 jar 包基本跟放在本地的工程文件中再 读取出来的速度是一样的。这个过程就好比我们查阅字典时，为了能够加快查找到内容，书前面的 目录就好比是索引，有了这个目录我们就可以方便找到内容了，一样的在 maven 仓库中有了索引我 们就可以认为可以快速找到 jar 包。

### 1.2.2 项目的一键构建 

我们的项目，往往都要经历编译、测试、运行、打包、安装 ，部署等一系列过程。

什么是构建？ 指的是项目从编译、测试、运行、打包、安装 ，部署整个过程都交给 maven 进行管理，这个 过程称为构建。

一键构建 指的是整个构建过程，使用 maven 一个命令可以轻松完成整个工作。

Maven 规范化构建流程如下：
![image-20210604091530705](images\Maven\image-20210604091530705.png)

我们一起来看 Hello-Maven 工程的一键运行的过程。通过 tomcat:run 的这个命令，我们发现现在的 工程编译，测试，运行都变得非常简单。

# 2. Maven 的使用  

## 2.1 Maven 的安装

### 2.1.1 Maven 软件的下载

为了使用 Maven 管理工具，我们首先要到官网去下载它的安装软件。通过百度搜索“Maven“如下：

![image-20210604091714364](images\Maven\image-20210604091714364.png)

点击 Download 链接，就可以直接进入到 Maven 软件的下载页面：

![image-20210604091730882](images\Maven\image-20210604091730882.png)

目前最新版是 apache-maven-3.5.3 版本，我们当时使用的是 apache-maven-3.5.2 版本，大家也可以下 载最新版本。 Apache-maven-3.5.2 下载地址：http://archive.apache.org/dist/maven/maven-3/ 下载后的版本如下：

![image-20210604091750091](images\Maven\image-20210604091750091.png)

### 2.1.2 Maven 软件的安装

Maven 下载后，将 Maven 解压到一个没有中文没有空格的路径下，比如 D:\software\maven 下面。 解压后目录结构如下：

![image-20210604091825727](images\Maven\image-20210604091825727.png)

| 文件夹名 | 功能                                                     |
| -------- | -------------------------------------------------------- |
| bin      | 存放了 maven 的命令，比如我们前面用到的 `mvn tomcat:run` |
| boot     | 存放了一些 maven 本身的引导程序，如类加载器等            |
| conf     | 存放了 maven 的一些配置文件，如 setting.xml 文件         |
| lib      | 存放了 maven 本身运行所需的一些 jar 包                   |

至此我们的 maven 软件就可以使用了，前提是你的电脑上之前已经安装并配置好了 JDK。

### 2.1.3. Maven 及 JDK 配置

 电脑上需安装 java 环境，安装 JDK1.7 + 版本 （将JAVA_HOME/bin 配置环境变量 path ），我们使 用的是 JDK8 相关版本

配置 `MAVEN_HOME` ，变量值就是你的 maven 安装 的路径（bin 目录之前一级目录）

![image-20210604092126911](images\Maven\image-20210604092126911.png)

上面配置了我们的 Maven 软件，注意这个目录就是之前你解压 maven 的压缩文件包在的的目录，最好不要有中文和空格。 



### 2.1.4. Maven 软件版本测试

通过 `mvn -v`命令检查 maven 是否安装成功，看到 maven 的版本为 3.5.2 及 java 版本为 1.8 即为安装 成功。 

找开 cmd 命令，输入 `mvn –v`命令，如下图：

![image-20210604092310032](images\Maven\image-20210604092310032.png)

我们发现 maven 的版本，及 jdk 的版本符合要求，这样我们的 maven 软件安装就成功了。

# 2.2. Maven 仓库

### 2.2.1. Maven 仓库的分类

maven 的工作需要从仓库下载一些 jar 包，如下图所示，本地的项目 A、项目 B 等都会通过 maven 软件从远程仓库（可以理解为互联网上的仓库）下载 jar 包并存在本地仓库，本地仓库 就是本地文 件夹，当第二次需要此 jar 包时则不再从远程仓库下载，因为本地仓库已经存在了，可以将本地仓库 理解为缓存，有了本地仓库就不用每次从远程仓库下载了

下图描述了 maven 中仓库的类型：
![image-20210604092437013](images\Maven\image-20210604092437013.png)

- **本地仓库** ：用来存储从远程仓库或中央仓库下载的插件和 jar 包，项目使用一些插件或 jar 包，优先从本地仓库查找
  - 默认本地仓库位置在 `${user.dir}/.m2/repository`，`${user.dir}`表示 windows 用户目录。
- **远程仓库**：如果本地需要插件或者 jar 包，本地仓库没有，默认去远程仓库下载。 远程仓库可以在互联网内也可以在局域网内。
- **中央仓库** ：在 maven 软件中内置一个远程仓库地址 http://repo1.maven.org/maven2 ，它是中 央仓库，服务于整个互联网，它是由 Maven 团队自己维护，里面存储了非常全的 jar 包，它包 含了世界上大部分流行的开源项目构件。

### 2.2.2 Maven 本地仓库的配置

在 ==MAVE_HOME/conf/settings.xml== 文件中配置本地仓库位置（maven 的安装目录下）：

打开 settings.xml文件，配置如下：

```xml
<localRepository>E:\mavenCK</localRepository>
```

### 2.2.3 全局 setting 与用户 setting

maven 仓库地址、私服等配置信息需要在 setting.xml 文件中配置，分为全局配置和用户配置。 

在 maven 安装目录下的有 conf/setting.xml 文件，此 setting.xml 文件用于 maven 的所有 project 项目，它作为 maven 的全局配置。

如需要个性配置则需要在用户配置中设置，用户配置的 setting.xml 文件默认的位置在：`${user.dir}  /.m2/settings.xml` 目录中，`${user.dir}` 指 windows 中的用户目录。

maven 会先找用户配置，如果找到则以用户配置文件为准，否则使用全局配置文件。

![image-20210604092931936](images\Maven\image-20210604092931936.png)

## 2.3 Maven 工程的认识

### 2.3.1 Maven 工程的目录结构

![image-20210604093007568](images\Maven\image-20210604093007568.png)

作为一个 maven 工程，它的 src 目录和 pom.xml 是必备的。 进入 src 目录后，我们发现它里面的目录结构如下：

![image-20210604093034664](images\Maven\image-20210604093034664.png)

| 文件夹名           | 功能                                            |
| ------------------ | ----------------------------------------------- |
| src/main/java      | 存放项目的.java 文件                            |
| src/main/resources | 存放项目资源文件，如 spring, hibernate 配置文件 |
| src/test/java      | 存放所有单元测试.java 文件，如 JUnit 测试类     |
| src/test/resources | 测试资源文件                                    |
| target             | 项目输出位置，编译后的 class 文件会输出到此目录 |
| pom.xml            | maven 项目核心配置文件                          |

> 【危险】如果是普通的 java 项目，那么就没有 webapp 目录。

### 2.3.2 Maven 工程的运行

进入 maven 工程目录（当前目录有 pom.xml 文件），运行 `tomcat:run` 命令。

![image-20210604093603462](images\Maven\image-20210604093603462.png)



### 2.3.3 问题处理

如果本地仓库配置错误会报下边的错误

分析： maven 工程运行先从本地仓库找 jar 包，本地仓库没有再从中央仓库找，上边提示 downloading… 表示 从中央仓库下载 jar，由于本地没有联网，报错。

解决： 在 maven 安装目录的 conf/setting.xml 文件中配置本地仓库

# 3. Maven 常用命令  

我们可以在 cmd 中通过一系列的 maven 命令来对我们的 maven-helloworld 工程进行编译、测试、运 行、打包、安装、部署。

## 3.1. 常用命令  

### 3.1.1. compile 

`compile` 是 maven 工程的==编译命令==，作用是将 src/main/java 下的文件编译为 class 文件输出到 target 目录下。 cmd 进入命令状态，执行 `mvn compile`，如下图提示成功：

![image-20210604093955129](images\Maven\image-20210604093955129.png)

查看 target 目录，class 文件已生成，编译完成。

### 3.1.2. test 

`test` 是 maven 工程的==测试命令== `mvn test`，会执行 src/test/java 下的单元测试类。 cmd 执行 mvn test 执行 src/test/java 下单元测试类，下图为测试结果，运行 1 个测试用例，全部成功。

![image-20210604094035993](images\Maven\image-20210604094035993.png)

### 3.1.3. clean

`clean` 是 maven 工程的清理命令，执行 clean 会删除 target 目录及内容。

### 3.1.4. package 

`package` 是 maven 工程的打包命令，对于 java 工程执行 package 打成 jar 包，对于 web 工程打成 war 包。

### 3.1.5. install 

`install` 是 maven 工程的安装命令，执行 install 将 maven 打成 jar 包或 war 包发布到本地仓库。 

从运行结果中，可以看出：当后面的命令执行时，前面的操作过程也都会自动执行，

## 3.2. Maven 指令的生命周期

maven 对项目构建过程分为三套相互独立的生命周期，请注意这里说的是“三套”，而且“相互独立”， 这三套生命周期分别是： 

1. Clean Lifecycle 在进行真正的构建之前进行一些清理工作。 
2. Default Lifecycle 构建的核心部分，编译，测试，打包，部署等等。
3. Site Lifecycle 生成项目报告，站点，发布站点。

## 3.3. maven 的概念模型

Maven 包含了一个项目对象模型 (Project Object Model)，一组标准集合，一个项目生命周期(Project  Lifecycle)，一个依赖管理系统(Dependency Management System)，和用来运行定义在生命周期阶段 (phase)中插件(plugin)目标(goal)的逻辑

![image-20210604094552987](images\Maven\image-20210604094552987.png)

1.  项目对象模型 (Project Object Model)

    一个 maven 工程都有一个 pom.xml 文件，通过 pom.xml 文件定义项目的坐标、项目依赖、项目信息、 插件目标等。

2. 依赖管理系统(Dependency Management System) 

   通过 maven 的依赖管理对项目所依赖的 jar 包进行统一管理。 比如：项目依赖 junit4.9，通过在 pom.xml 中定义 junit4.9 的依赖即使用 junit4.9，如下所示是 junit4.9 的依赖定义：

   ```xml
   <!-- 依赖定义 -->
   <dependencies>
       <!-- 此项目运行使用junit，使用此项目依赖junit -->
       <dependencie>
           <!-- Junit的项目名称 -->
           <groupId>junit</groupId>
           <!-- Junit的模块名称 -->
           <artifactId>junit</artifactId>
           <!-- Junit的版本 -->
           <version>4.9</version>
           <!-- 依赖范围：单元测试时使用Junit -->
           <scope>test</scope>
       </dependencie>
   ```

   

3. 一个项目生命周期(Project Lifecycle) 

   使用 maven 完成项目的构建，项目构建包括：清理、编译、测试、部署等过程，maven 将这些 过程规范为一个生命周期，如下所示是生命周期的各各阶段：

   ![image-20210604095124043](images\Maven\image-20210604095124043.png)

   maven 通过执行一些简单命令即可实现上边生命周期的各各过程，比如执行 `mvn compile` 执行编译、 执行 `mvn clean` 执行清理。

4. 一组标准集合 

   maven 将整个项目管理过程定义一组标准，比如：通过 maven 构建工程有标准的目录结构，有 标准的生命周期阶段、依赖管理有标准的坐标定义等。

5. 插件(plugin)目标(goal) 

   maven 管理项目生命周期过程都是基于插件完成的。

# 4. idea 开发 maven 项目

maven 项目 在实战的环境中，我们都会使用流行的工具来开发项目。

## 4.1. idea 的 maven 配置 

### 4.1.1. 集成maven

打开→File→Settings 配置 maven

依据图片指示，选择本地 maven 安装目录，指定 maven 安装目录下 conf 文件夹中 settings 配置文件。

![image-20210604100140476](images\Maven\image-20210604100140476.png)

### 4.1.2. maven创建web项目

打开 idea，选择创建一个新工程

![image-20210604100310219](images\Maven\image-20210604100310219.png)

![image-20210604101043336](images\Maven\image-20210604101043336.png)

![image-20210604101054754](images\Maven\image-20210604101054754.png)

![image-20210604101325518](images\Maven\image-20210604101325518.png)

> 【警告】没有Java目录
>
> 解决：手动添加 src/main/java 目录，如下图右键 main 文件夹→New→Directory

![image-20210604101534900](images\Maven\image-20210604101534900.png)

![image-20210604101545563](images\Maven\image-20210604101545563.png)

![image-20210604101602184](images\Maven\image-20210604101602184.png)

## 4.2. pom.xml 文件说明

### 4.2.1. 项目坐标定义

#### ①. 坐标定位声明

```xml
<!-- 项目名称,定义为 组织名+项目名,类似包名 -->
<groupId>org.example</groupId>
<!--模块名-->
<artifactId>mavendemo</artifactId>
<!--当前项目版本号,snapshot为快照版本即非正式版本,  release为正式发布版本-->
<version>1.0-SNAPSHOT</version>
<!--打包类型
    jar: 执行package会打成jar包
    war: 执行package会打成war包
  -->
<packaging>war</packaging>
```

#### ②. 坐标的来源方式

 添加依赖需要指定依赖 jar 包的坐标，但是很多情况我们是不知道 jar 包的的坐标，可以通过如下方 式查询：

从互联网搜索

1. http://search.maven.org/
2. http://mvnrepository.com

### 4.2.2. 依赖范围

A 依赖 B，需要在 A 的 pom.xml 文件中添加 B 的坐标，添加坐标时需要指定依赖范围，依赖范围包括：

1. `compile`：编译范围，指 A 在编译时依赖 B，此范围为默认依赖范围。编译范围的依赖会用在 编译、测试、运行，由于运行时需要所以编译范围的依赖会被打包。
2. `provided`：provided 依赖只有在当 JDK 或者一个容器已提供该依赖之后才使用， provided 依 赖在编译和测试时需要，在运行时不需要，比如：servlet api 被 tomcat 容器提供。 
3. `runtime`：runtime 依赖在运行和测试系统的时候需要，但在编译的时候不需要。比如：jdbc 的驱动包。由于运行时需要所以 runtime 范围的依赖会被打包。 
4. `test`：test 范围依赖 在编译和运行时都不需要，它们只有在测试编译和测试运行阶段可用， 比如：junit。由于运行时不需要所以 test范围依赖不会被打包。  
5. `system`：system 范围依赖与 provided 类似，但是你必须显式的提供一个对于本地系统中 JAR 文件的路径，需要指定 systemPath 磁盘路径，system依赖不推荐使用。

| 依赖范围 | 对于编译classpath有效 | 对于测试classpath有效 | 对于运行时classpath有效 | 例子                            |
| -------- | --------------------- | --------------------- | ----------------------- | ------------------------------- |
| compile  | Y                     | Y                     | Y                       | spring-core                     |
| test     | ——                    | Y                     | ——                      | Junit                           |
| provided | Y                     | Y                     | ——                      | servlet-api                     |
| runtime  | ——                    | Y                     | Y                       | JDBC驱动                        |
| system   | Y                     | Y                     | ——                      | 本地的<br />Maven仓库之外的类库 |

在 maven-web 工程中测试各各 `scope`。

 测试总结： 

1. 默认引入 的 jar 包 ------- compile 【默认范围 可以不写】（编译、测试、运行 都有效 ）
2. servlet-api 、jsp-api ------- provided （编译、测试 有效， 运行时无效 防止和 tomcat 下 jar 冲突）
3.  jdbc 驱动 jar 包 ---- runtime （测试、运行 有效 ）
4. junit ----- test （测试有效） 

依赖范围由强到弱的顺序是：compile>provided>runtime>test

```xml
<dependencies>
    <dependencie>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.9</version>
        <!-- 依赖范围：单元测试时使用Junit -->
        <scope>test</scope>
    </dependencie>
```

### 4.2.3. 设置jdk编译版本

使用 jdk1.8，需要设置编译版本为 1.8，这里需要使用 maven 的插件来设置： 在 pom.xml 中加入：

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
    </plugins>
</build>
```



### 4.2.4. pom 基本配置

pom.xml 是 Maven 项目的核心配置文件，位于每个工程的根目录，基本配置如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <!-- 项目名称,定义为 组织名+项目名,类似包名 -->
  <groupId>org.example</groupId>
  <!--模块名-->
  <artifactId>mavendemo</artifactId>
  <!--当前项目版本号,snapshot为快照版本即非正式版本,  release为正式发布版本-->
  <version>1.0-SNAPSHOT</version>
  <!--打包类型
    jar: 执行package会打成jar包
    war: 执行package会打成war包
  -->
  <packaging>war</packaging>
  <!--项目的名称, Maven产生的文档用，可省略-->
  <name>mavendemo Maven Webapp</name>
  <description>项目描述，常用于 Maven 生成的文档</description>
  <!-- 项目主页的URL, Maven产生的文档用 ，可省略 -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>
  <!--项目依赖构件配置，配置项目依赖构件的坐标-->
  <dependencies>
  </dependencies>
  <!--项目构建配置，配置编译、运行插件等-->
  <build>
  </build>
</project>
```


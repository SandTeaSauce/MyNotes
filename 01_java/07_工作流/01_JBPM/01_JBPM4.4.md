# 1、工作流基础

## 1.1、工作流相关概念

**工作流**（Workflow），就是“业务过程的部分或整体在计算机应用环境下的自动化”，它主要解决的是“使在多个参与者之间按照某种预定义的规则传递文档、信息或任务的过程自动进行，从而实现某个预期的业务目标，或者促使此目标的实现”。



**工作流管理系统**（WfMS，Workflow Management System）的主要功能是通过计算机技术的支持去定义、执行和管理工作流，协调工作流执行过程中工作之间以及群体成员之间的信息交互。工作流需要依靠工作流管理系统来实现。工作流管理系统是定义、创建、执行工作流的系统，应能提供以下三个方面的功能支持：

1. 定义工作流：包括具体的活动、规则等
2. 运行控制功能：在运行环境中管理工作流过程，对工作流过程中的活动进行调度
3. 运行交互功能：指在工作流运行中，WfMS与用户（活动的参与者）及外部应用程序工具交互的功能。

 

**采用工作流管理系统的优点**

1. 提高系统的柔性，适应业务流程的变化 
2. 实现更好的业务过程控制，提高顾客服务质量
3. 降低系统开发和维护成本

 

**工作流框架**有：Jbpm、OSWorkflow、ActiveBPEL、YAWL等

 

OA（办公自动化）主要技术之一就是工作流。

## 1.2、开源工作流jBPM4.4介绍

jBPM 即java Business Process Management，是基于java的业务流程管理系统。jBPM是市面上相当流行的一款开源工作流引擎，引擎底层基于Active Diagram模型。jBPM4.4使用了hibernate（3.3.1版），因此可以很好的支持主流数据库。jBPM4.4共有18张表。

jBPM官方主页*：**http://www.jboss.org/jbpm*

![image-20210630104414550](images\01_JBPM4.4\image-20210630104414550.png)

# 2、核心概念与相关API

## 2.1、核心概念

### 2.1.1、流程定义（Process definition）

ProcessDefinition，流程定义：==一个流程的步骤说明==。对业务过程步骤的描述，在JBPM4中它表现为若干“活动”节点通过“转移”线条串联。

如一个请假流程、报销流程、借款流程等，是==一个规则==。

例：一个金融公司有一个信贷流程定义，描述公司如何处理信贷请求的步骤。
![img](images\01_JBPM4.4\clip_image002.jpg)

### 2.1.2、流程实例（Process instance）

ProcessInstance，流程实例：==代表流程定义的一次执行==。表示流程定义在运行时特有的执行过程。

> 【说明】可以把流程定义理解为Java Classpath定义，而流程实例则可以理解为由Java Class定义实例化生成的Java Object对象

例：上周五李某提出贷款买车申请，即代表一个信贷流程定义被实例化了——一个信贷流程的实例产生了。

![img](images\01_JBPM4.4\clip_image002-1625037623164.jpg)

### 2.1.3、执行（Execution）

Execution，执行：==具有指向当前执行活动的指针——执行==

一般情况下，一个流程实例可以理解为一颗“执行树”。当一个流程实例启动时，最初的执行处于这课执行树的根节点位置，之后可以根据定义的需要产生子执行，即树枝。

使用树状结构的原因在于， 这一概念只有==一条执行路径==， 使用起来更简单。 业务API不需要了解流程实例和执行之间功能的区别。 因此， API里只有一个执行类型来引用流程实例和执行。

假设汇款和存档可以同时执行，那么主流程实例就包含了2个用来跟踪状态的子节点：

![img](images\01_JBPM4.4\clip_image002-1625037938143.jpg)

## 2.2、相关API

### 2.2.1、ProcessEngine

`ProcessEngine`：流程引擎对象，是JBPM4所有Service API之源。

1. 由Configuration类构建，即工作流引擎根据配置产生
2. 是线程安全



获取ProcessEngine对象方式：

1. 使用默认的配置文件（jbpm.cfg.xml）生成Configuration并构建ProcessEngine：

   ```java
   ProcessEngine processEngine = new Configuration().buildProcessEngine();
   ```

2. 使用如下代码获取使用默认配置文件的、单例的ProcessEngine对象：

   ```java
   ProcessEngine processEngine = Configuration.getProcessEngine();
   ```

3. 使用指定的配置文件(要放到classPath下):

   ```java
   ProcessEngine processEngine = new Configuration()
         .setResource("my-own-configuration-file.xml")
         .buildProcessEngine();
   ```

### 2.2.2、jBPM Service API

==jBPM所有的操作都是通过Service完成的==，以下是获取Service的方式：

```java
RepositoryService repositoryService = processEngine.getRepositoryService();

ExecutionService executionService = processEngine.getExecutionService();

TaskService taskService = processEngine.getTaskService();

HistoryService historyService = processEngine.getHistoryService();

ManagementService managementService = processEngine.getManagementService();
```



各个Service的作用：

| JBPM Service          | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| ==RepositoryService== | ==流程资源服务接口。提供对流程定义部署、查询、删除等操作==   |
| ==ExecutionService==  | ==流程执行服务的接口。提供启动流程实例、“执行”推荐、设置流程变量等操作== |
| ==TaskService==       | ==人工任务服务的接口。提供对任务（Task）的创建、提交、查询、保存、删除等操作== |
| HistoryService        | 流程历史服务的接口。（执行完的数据管理，主要是查询）         |
| IdentityService       | 身份认证服务的接口。提供对流程用户、用户组以及组成员关系的相关服务 |
| ManagementService     | 流程管理控制服务的接口。                                     |

### 2.2.3、API风格

方法调用链。

一个方法就是一个关于流程的业务操作。

每一个方法都是流程有关的一个业务操作，默认是一个独立的事务。





# 3、流程服务

## 3.1、流程引擎

与 jBPM 的交互通过服务发生。服务接口`ProcessEngine`可以 从`Configuration`



获取ProcessEngine对象三种方式：

```java
// 1. 使用默认的配置文件（jbpm.cfg.xml）生成Configuration并构建ProcessEngine：
ProcessEngine processEngine = new Configuration().buildProcessEngine();

// 2. 使用如下代码获取使用默认配置文件的、单例的ProcessEngine对象：
ProcessEngine processEngine = Configuration.getProcessEngine();

// 3. 使用指定的配置文件(要放到classPath下)
ProcessEngine processEngine = new Configuration()
      .setResource("my-own-configuration-file.xml")
      .buildProcessEngine();
```



ProcessEngine可以获得以下服务（这些服务用来操作JBPM）：

```java
RepositoryService repositoryService = processEngine.getRepositoryService();
ExecutionService executionService = processEngine.getExecutionService();
TaskService taskService = processEngine.getTaskService();
HistoryService historyService = processEngine.getHistoryService();
ManagementService managementService = processEngine.getManagementService();
```



## 3.2、部署流程

部署流程要用到：`RepositoryService`

> 【说明】`RepositoryService：`流程资源服务接口。提供对流程定义部署、查询、删除等操作

在部署过程中，流程引擎会把一个ID分配给流程定义。这个ID的格式为`{key}-{version}`，即流程键和流程版本之间通过字符连接。

如果没有指定版本号，则流程引擎自动分配一个版本号。部署key已存在的流程定义，其版本号自动递增；新部署的流程定义自动分配1。

没有指定key值属性的流程定义

| 属性名称 | 属性值            | 来源         |
| -------- | ----------------- | ------------ |
| name     | Insurance claim   | 流程定义文件 |
| key      | Insurance claim   | 系统生成     |
| version  | 1                 | 系统生成     |
| id       | Insurance claim-1 | 系统生成     |

 

流程部署两种方法：

```java
// 1. 从类路径（classpath）部署一个流程
String deploymentId = processEngine.getRepositoryService()
    .createDeployment()
    .addResourceFromClasspath("process/test.jpdl.xml")
    .addResourceFromClasspath("process/test.png")
    .deploy();

// 2. 一次部署多个流程（定义有关文件要先打成zip包）
String deploymentId = processEngine.getRepositoryService()
    .createDeployment()
    .addResourcesFromZipInputStream(zipInputStream)
    .deploy();
```

> 【说明】
>
> `addResourceFromClasspath(resource);` 可以调用多次以添加多个文件。文件重复添加也不会报错。
>
> `addResourceFromInputStream(resourceName, inputStream);`添加一个文件（使用InputStream）
>
> `addResourcesFromZipInputStream(zipInputStream);`添加多个文件，里面也可以有文件夹。
>
> ==以上方法可以在一起调用。==



## 3.3、删除流程

删除流程两种方式：

```java
// 1. 物理删除，即在数据库中彻底销毁这条流程定义的记录
// 【注意】但是不能删除还存在未完成的流程实例，否则会抛出异常
repositoryService.deleteDeployment(deploymentId);

// 2. 级联删除一个已经发布的流程定义及其产生的所有实例
repositoryService.deleteDeploymentCascade();
```

## 3.4、启动一个新的流程实例

说明：流程实例创建后，直接就到开始活动后的第一个活动，不会在开始活动停留。

#### 3.4.1、常规方法

查询key为ICL的最新版本流程定义，然后根据最新的版本的ICL流程定义启动流程实例

```java
ProcessInstance pi = executionService.startProcessInstanceByKey("ICL");
```



如果您想在非常特定的版本中启动一个新流程实例，您可以使用流程定义的 id

```java
ProcessInstance processInstance = executionService.startProcessInstanceById("ICL-1");
```



#### 3.4.2、指定业务键

业务键：用户执行流程的时候根据业务定义的。==一个业务流程必须在此流程定义所有版本的流程实例范围内是唯一==

业务键可以被用来创建流程实例的ID，格式为`{processDefinitionKey}`，下面的ID为1213213123

```java
ProcessInstance pi = executionService.startProcessInstanceByKey("ICL", "1213213123");
```



若使用的是常规方法，可以使用如下代码发起流程实例并获取流程实例ID

```java
ProcessInstance pi = executionService.startProcessInstanceByKey("ICL");
String pid = pi.getId();
```

> 【注意】最好指定一个业务键发起流程实例。常规方法需要根据流程变量来执行流程实例的搜索，十分消耗执行效率

#### 3.4.3、指定变量

如果新的流程实例需要一些输入参数启动。则在发起流程时传入流程变量对象（`Map<String, Object>`）

```java 
Map<String, String> map = new HashMap<String, String>();
map.put("owner",owner);
ProcessInstance pi = executionService.startProcessInstanceByKey(flowinfo.getJbpmKey(), map);
```



## 3.5、发出等待执行的信号

使用==state==活动时，执行（或流程实例）将在到达某个状态时停止，等待信号触发（也称为外部触发器）。`signalExecution`该方法用于此。执行由执行 ID（字符串）引用。

捕获正确执行的首选方法是将事件侦听器与状态活动相关联

- ```xml
  <状态名称="等待">
    <on event="开始">
      <event-listener class="org.jbpm.examples.StartExternalWork" />
    </on>
    ...
  </状态>
  ```

在事件侦听器中，`StartExternalWork`可以启动需要在外部完成的工作。在该事件侦听器中，您还可以使用`execution.getId()`. 当外部工作完成时，您需要稍后提供该信号的 executionId：

- ```java
  executionService.signalExecutionById(executionId);
  ```



## 3.6、任务服务

`TaskService` 的主要目的是提供对任务列表的访问

### 3.6.1、查询任务列表

#### ①、查询个人任务列表

```java
// 方式一：根据获取userId的用户的任务列表
List<Task> taskList = TaskService.findPersonalTasks(userId);

// 方式二：
List<Task> list = taskService.createTaskQuery()
    .assignee(userId)
    .list();
```

#### ②、查询任务组列表

```java
// 方式1： 
taskService.findGroupTasks(userId);

// 方式2： 
List<Task> list = processEngine.getTaskService()//
    .createTaskQuery()//
    .candidate(userId)//
    .list();
```

### 3.6.2、任务变量操作

通常，任务与表单相关联并显示在某些用户界面中。表单需要能够读写与任务相关的数据。

#### ①、读取任务变量

```java
// 读取任务变量
Set<String> variableNames = taskService.getVariableNames(taskId); 
Map<String, Object> variables = taskService.getVariables(taskId, variableNames);
```

#### ②、写入任务变量

```java
// 写入任务变量
variables = new HashMap<String, Object>(); 
variables.put("category", "small"); 
variables.put("lires", 923874893); 
taskService.setVariables(taskId, variables);
```

### 3.6.3、完成任务

#### ①、正常完成任务

完成任务的四种方式：

```java
String taskId = "420001";
// 1. 根据指定的任务ID 完成任务
processEngine.getTaskService().completeTask(taskId);
// 2. 根据指定的任务ID , 同时设置变量. 完成任务
processEngine.getTaskService().completeTask(taskId, variables);
// 3. 指定outcome, 即下一步的转移路径, 完成任务
processEngine.getTaskService().completeTask(taskId, outcome);
// 4. 指定outcome, 即下一步的转移路径, 同时设置变量, 完成任务
processEngine.getTaskService().completeTask(taskId, outcome, variables);
```

#### ②、自行控制任务

自行控制任务完成后是否可向后流转

```java
String taskId = "420001";

// 1，设置为false代表：办理完任务后不向后移动（默认为true）
TaskImpl taskImpl = (TaskImpl) processEngine.getTaskService().getTask(taskId);
taskImpl.setSignalling(false); 

// 2，办理完任务
processEngine.getTaskService().completeTask(taskId);
```

### 3.6.4、拾取任务

```java
// 拾取任务。拾取组任务到个人任务列表中，如果任务有assignee，则会抛异常
taskService.takeTask(taskId, userId);

// 转交任务给其他人。（如果任务有assignee，则执行这个方法代表重新分配。也可以把assignee设为null表示组任务没有人办理了）
taskService.assignTask(taskId, userrid);
```

## 3.7、历史任务

在流程实例的运行时执行期间，会生成事件。从这些事件中，有关正在运行和已完成的流程执行的历史信息都收集在历史表中。在`HistoryService`可以访问这些信息。

### 3.7.1、查询特点所有流程

```java
List<HistoryProcessInstance> historyProcessInstance = historyService
    .createHistoryProcessInstanceQuery()
    .processDefinitionId("ICL-1") // 查询 步骤实例编号（ICL-1）
    .orderAsc(HistoryTaskQuery.PROPERTY_CREATETIME) // 返回的结果集按开始时间正序排序
    .list();
```

### 3.7.2、查询个人活动流程

```java
List<HistoryActivityInstance> histActInsts = historyService 
    .createHistoryActivityInstanceQuery() 
    .processDefinitionId("ICL-1")  // 查询 步骤实例编号（ICL-1）
    .activityName("a")  // 为给定的活动选择活动实例
    .list();
```

### 3.7.3、查询流程实例执行的节点

```java
List<HistoryActivityInstance> histActInsts = historyService 
    .createHistoryActivityInstanceQuery() 
    .processInstanceId("ICL.12345") 
    .list();
```



## 3.8、查询流程定义

### 3.8.1、查询所有流程定义

```java
// 1，构建查询
ProcessDefinitionQuery pdQuery = processEngine.getRepositoryService() 
    .createProcessDefinitionQuery() // 
    .orderAsc(ProcessDefinitionQuery.PROPERTY_NAME) // 
    .orderDesc(ProcessDefinitionQuery.PROPERTY_VERSION);

// 2，查询出总数量与数据列表
long count = pdQuery.count();
List<ProcessDefinition> list = pdQuery.page(0, 100).list();// 分页：取出前100条记录

// 3，显示结果
System.out.println(count);
for (ProcessDefinition pd : list) {
    System.out.println("id=" + pd.getId()// 格式为：{key}-{version}，例如：zhangsan-1
                       + ", name=" + pd.getName()// .jpdl.xml中根元素的name属性的值。
                       + ", key=" + pd.getKey()// .jpdl.xml中根元素的key属性的值。如果不写，默认为name属性的值。
                       + ", version=" + pd.getVersion()// .jpdl.xml中根元素的version属性的值。如果不写，默认的效果为同名称的第一个version为1，以后的就递增。（一般不要写）
                       + ", deploymentId=" + pd.getDeploymentId()); // 所属的Deployment部署对象的id
}
```

> 【说明】
>
> ①、ProcessDefinition：流程定义对象，是解析 .jpdl.xml 文件得到流程步骤信息
>               没有
>
> 

### 3.8.2、查询所有最新版本的流程定义列表

```java
// 1，查询，按version升序排序，则最大版本排在最后面
List<ProcessDefinition> all = processEngine.getRepositoryService()//
    .createProcessDefinitionQuery()//
    .orderAsc(ProcessDefinitionQuery.PROPERTY_VERSION)
    .list();
// 2，过滤出所有不同Key的最新版本（因为最大版本在最后面）
Map<String, ProcessDefinition> map = new HashMap<String, ProcessDefinition>(); // map的key是流程定义的key，map的vlaue是流程定义对象
for (ProcessDefinition pd : all) {
    map.put(pd.getKey(), pd);
}
Collection<ProcessDefinition> result = map.values();
// 3，显示结果
for (ProcessDefinition pd : result) {
    System.out.println("deploymentId=" + pd.getDeploymentId()// 
                       + ",\t id=" + pd.getId()// 流程定义的id，格式：{key}-{version}
                       + ",\t name=" + pd.getName()
                       + ",\t key=" + pd.getKey()
                       + ",\t version=" + pd.getVersion());
}
```

## 3.9、绘制当前流程的流程图

![image-20210709104027478](images\01_JBPM4.4\image-20210709104027478.png)

### 3.9.1获取流程中的文件资源

获取流程图文件

```java
// 用于加载流程图（resourceName：资源的名称，就是 jbpm4_lob 表中的 NAME_ 列表值）
InputStream in = repositoryService.getResourceAsStream(deploymentId, resourceName);
```

### 3.9.2、获取流程图中某活动的坐标

获取活动节点的坐标，用于绘制红色方格（根据坐标在前端javascript中绘制）

```java
String processDefinitionId = "test-1"; // 流程定义的id
String activityName = "start1"; // 活动的名称

ActivityCoordinates c = processEngine.getRepositoryService()
    .getActivityCoordinates(processDefinitionId, activityName);
System.out.println("x=" + c.getX() 
                   + ",y=" + c.getY() 
                   + ",width=" + c.getWidth() 
                   + ",height=" + c.getHeight());
```

# 4、流程定义语言

## 4.1、process

### 4.1.1、属性

是==.jpdl.xml==的根元素，可以指定的属性有：

| 属性    | 类型               | 默认                        | 是否必需                 | 描述                                                         |
| ------- | ------------------ | --------------------------- | ------------------------ | ------------------------------------------------------------ |
| name    | 文本               |                             | :heavy_check_mark:       | 用于在用户交互中显示进程名称的进程名称或标签。               |
| key     | 字母、数字、下划线 | 如果省略，根据name_数字生成 | :heavy_multiplication_x: | 识别不同的过程定义。可以部署具有相同密钥的进程的多个版本。对于所有部署的版本，key:name 组合必须保持完全相同。 |
| version | 整数               | ​如果省略，从1开始的最高版本 | :heavy_multiplication_x: | 此进程的版本号                                               |

### 4.1.2、元素

| 元素        | 多样性 | 描述                                                         |
| ----------- | ------ | ------------------------------------------------------------ |
| description | 0..1   | 说明文字                                                     |
| activitys   | 1..*   | 任何活动类型的列表都可以放在这里。`start`必须至少存在一项活动。 |



## 4.2、Transition

一个活动中可以指定0个或多个Transition。

- Start活动中只能有一个Transition。
- End活动中没有Transition。
- 其他活动中有1条或多条Transition

如果只有一个，则可以不指定名称（名称是null）；==如果有多个，则要分别指定唯一的名称==。

## 4.3、流转控制活动

活动种类：

1. start：开始活动
2. state：状态活动
3. decision：判断活动
4. fork - join：分支/聚合活动
5. end：结束活动
6. task：人工任务活动
7. sub-process：子流程活动
8. custom：自定义活动

### 4.3.1、start

指示此流程的执行==开始的位置==。通常，流程中只有一个`start`活动，且必须至少有一个`start`活动。`start`活动必须有一个`transition`，并且在流程执行开始时进行该`transition`。

> 【注意】不能有Transition指向开始活动

```xml
<start g="618,-6,48,48" name="开始">
    <transition g="-24,-10" name="承包单位" to="承包单位"/>
</start>
```

start活动的属性：

- | 属性 | 类型 | 默认值 | 是否必需                 | 描述                                                         |
  | ---- | ---- | ------ | ------------------------ | ------------------------------------------------------------ |
  | name | 文本 | 无     | :heavy_multiplication_x: | start活动的名称，在无流入转移指向start活动的情况下，name属性可以不指定 |

start活动的元素：

- | 元素       | 个数 | 描述                           |
  | ---------- | ---- | ------------------------------ |
  | transition | 1    | 流出转移，指向流程的下一个步骤 |

  

### 4.3.2、state

等待状态。流程执行将等到通过 API 提供外部触发器

```xml
<process name="test" xmlns="http://jbpm.org/4.4/jpdl">
    <start name="start1">
        <transition name="to state1" to="state1"/>
    </start>
    <end name="end1"/>
    <state name="state1">
        <transition name="to end1" to="end1"/>
    </state>
</process>
```



```java
public class AppTest {
    private ProcessEngine processEngine = Configuration.getProcessEngine();

    @Test
    public void test() throws Exception {
        // [1]. 部署流程定义
        InputStream in = getClass().getResourceAsStream("test.jpdl.xml"); 
        processEngine.getRepositoryService()//	
            .createDeployment()//
            .addResourceFromInputStream("test.jpdl.xml", in)//
            .deploy();

        // [2]. 启动流程实例
        ProcessInstance pi = processEngine.getExecutionService().startProcessInstanceByKey("test");
        System.out.println("当前正在执行的活动：" + pi.findActiveActivityNames()); // 等待下一步激活
    }

    // 向后执行一步
    @Test
    public void testSignal() throws Exception {
        String executionId = processInstance.findActiveExecutionIn("start1").getId();
        processEngine.getExecutionService().signalExecutionById(executionId, "to end1");
    }
}
```



### 4.3.3、end、end-error、end-cancel

默认情况下，`end`活动将结束整个流程实例。如果多个并发执行在同一个流程实例中仍然处于活动状态，则所有这些都将结束。

==可以有多个==，也可以没有。如果有多个，则到达任一个`end`活动，整个流程就都结束了；如果没有，则到达最后那个没有Transition的活动，流程就结束了。



### 4.3.4、task

作用：为任务组件中的人员创建任务。

#### ①、assignee

> 【说明】
>
> 作用：将分配给特定用户的简单任务

| 属性     | 类型   | 默认 | 是否必须                 | 描述                                |
| -------- | ------ | ---- | ------------------------ | ----------------------------------- |
| assignee | 表达式 |      | :heavy_multiplication_x: | userId 指的是负责完成此任务的人员。 |

1. 在 jpdl.xml 中声明 `assignee="#{employee}"`

   ```xml
   <process name="test" xmlns="http://jbpm.org/4.4/jpdl">
       <start name="start1">
           <transition name="to 提交申请" to="提交申请" />
       </start>
       <end name="end1"/>
       <task assignee="#{employee}" name="提交申请">
           <transition name="to end1" to="end1" />
       </task>
   </process>
   ```

2. 在启动流程实例的时候，指定处理人员

   ```java
   public void test() throws Exception {
       // 【1】部署流程定义。省略
   
       // 【2】启动流程实例
       Map<String, Object> variables = new HashMap<String, Object>();
       variables.put("employee", "张三"); // 指定处理人
       ProcessInstance pi = processEngine.getExecutionService().startProcessInstanceByKey("test", variables);
       System.out.println("当前正在执行的活动：" + pi.findActiveActivityNames());
   }
   ```



#### ②、candidate

> 【说明】
>
> 作用：将提供给一组用户的任务。然后其中一个用户应该接受任务以完成它。

| 属性             | 类型   | 默认 | 是否必需                 | 描述                                                         |
| ---------------- | ------ | ---- | ------------------------ | ------------------------------------------------------------ |
| candidate-groups | 表达式 |      | :heavy_multiplication_x: | 解析为逗号分隔的 groupId 列表。小组中的所有人都将成为这项任务的候选人。 |
| candidate-users  | 表达式 |      | :heavy_multiplication_x: | 解析为逗号分隔的 userId 列表。所有用户都将成为此任务的候选人。 |

1. 使用任务候选，在xml中添加 `Candidate-groups`

   ```xml
   <process name="TaskCandidates"> 
       <start> 
           <transition to="review" /> 
       </start> 
       <task name="review" Candidate-groups="sales-dept" >  【指定groups】
           <transition to="wait" /> 
       </task> 
       <state name="wait"/> 
   </process>
   ```

   启动后，将创建一个任务。该任务不会出现在任何人的个人任务列表中

   ```java
   taskService.getPersonalTasks("johndoe"); // 查询个人任务列表为空 
   ```

   但该任务会出现在该组所有成员的组任务列表中`sales-dept` 。

2. 为 `sales-dept` 添加成员

   ```java
   // identityService：用于公开 jBPM 使用的（可配置的）身份组件的接口。
   // 1. createGroup：创建组 新组
   String  dept = identityService.createGroup("sales-dept");
   // 2. createUser：创建用户
   identityService.createUser("johndoe", "John", "Doe");
   identityService.createUser("joesmoe", "Joe", "Smoe");
   // 3. createMembership：使给定用户成为具有给定角色的给定组的成员。 角色可以为空。
   identityService.createMembership("johndoe", dept, "developer");
   identityService.createMembership("joesmoe", dept, "developer");
   // 4. 部署流程
   deploymentId = repositoryService.createDeployment()
       .addResourceFromClasspath("org/jbpm/examples/task/candidates/process.jpdl.xml")
       .deploy();
   ```

   所以在进程创建后，任务会同时出现在用户johndoe和joesmoe的组任务中

   ```java
   taskService.getPersonalTasks("johndoe"); // 可以到查询个人任务列表
   ```



3. 候选人必须先接受一项任务，然后才能开始工作。这将防止两个候选人开始处理同一任务。用户界面只能为组任务列表中的任务提供“执行”操作。

   ```java
   taskService.takeTask(task.getDbid(), "johndoe");
   ```

   当用户接受一项任务时，该任务的受托人将被设置为给定的用户。该任务将从所有候选人的组任务列表中消失，而将出现在用户分配的任务中。



#### ③、AssignmentHandler

一个`AssignmentHandler`可以用来计算受让人和编程任务的候选人。

2. AssignmentHandler，需要在`<task>`元素中写`<assignment-handler class="AssignmentHandlerImpl"/>`子元素。

   ```xml
   <process name="test" xmlns="http://jbpm.org/4.4/jpdl">
       <start name="start1">
           <transition name="to 提交申请" to="提交申请"/>
       </start>
   
       <task name="提交申请">
           <assignment-handler class="cn.itcast.i_task_2.AssignmentHandlerImpl"/>
           <transition name="to end1" to="end1"/>
       </task>
   
       <end name="end1"/>
   </process>
   ```

   

2. 指定的类要实现AssignmentHandler接口

  ```java
  public class AssignmentHandlerImpl implements AssignmentHandler {
  	/**  计算并指定任务的办理人  */
  	public void assign(Assignable assignable, OpenExecution execution) throws Exception {
  		System.out.println("---------> AssignmentHandlerImpl.assign()");
  		// 计算办理人
  		String userId = "赵文剑";
  		// 分配任务
  		assignable.setAssignee(userId); // 指定此任务的办理人
          
  		// assignable.addCandidateUser("经理A"); // 添加一个候选人，这就是一个组任务了
  		// assignable.addCandidateUser("经理B"); // 添加一个候选人，这就是一个组任务了
  		// assignable.addCandidateUser("经理C"); // 添加一个候选人，这就是一个组任务了
  	}
  }
  ```

3. 在其中可以使用Assignable.setAssignee(String)，分配个人任务。

  ```java
  public void testAssignTask() throws Exception {
      String taskId = "350008";
      String userId = "刘建平";
      processEngine.getTaskService().assignTask(taskId, userId);
  }
  ```



#### ④、swimlane

流程中的多个任务应分配给同一用户或候选人。流程中的多个任务可以关联到单个泳道。流程实例将记住在泳道中执行第一个任务的候选人和用户。同一泳道中的后续任务将分配给这些用户和候选人。

泳道也可以被视为流程角色。在某些情况下，这可能归结为身份组件中的授权角色。但请记住，它并不总是相同的。

**task属性**

- | 属性    | 类型           | 默认 | 是否必需                 | 描述                 |
  | ------- | -------------- | ---- | ------------------------ | -------------------- |
  | simlane | 泳道（字符串） |      | :heavy_multiplication_x: | 指在进程中声明的泳道 |

**swimlane属性**

- | 属性             | 类型   | 默认 | 是否必需                 | 描述                                                         |
  | ---------------- | ------ | ---- | ------------------------ | ------------------------------------------------------------ |
  | name             | 字符串 |      | :heavy_check_mark:       | 此泳道的名称。这是任务泳道属性将引用的名称。                 |
  | assignee         | 表达式 |      | :heavy_multiplication_x: | userId 指的是负责完成此任务的人员。                          |
  | candidate-groups | 表达式 |      | :heavy_multiplication_x: | 解析为逗号分隔的 groupId 列表。组中的所有人员都将成为此泳道中任务的候选人。 |
  | candidate-users  | 表达式 |      | :heavy_multiplication_x: | 解析为逗号分隔的 userId 列表。所有用户都将成为此泳道中任务的候选人。 |



#### ⑤、电子邮件

当任务被添加到他们的列表时，可以向受让人提供通知，以及在特定时间间隔提醒。每封电子邮件都是从模板生成的。模板可以内联或在`process-engine-context`配置文件的部分中指定。

**task元素：**

- | 元素         | 多样性 | 描述                                                         |
  | ------------ | ------ | ------------------------------------------------------------ |
  | notification | 0..1   | 分配任务时发送通知消息。如果没有内联引用或提供模板，邮件支持将退回到名为*task-notification*的模板。 |
  | reminder     | 0..1   | 以特定时间间隔发送提醒消息。如果没有内联引用或提供模板，邮件支持将退回到名为*task-reminder*的模板。 |

 **notification属性：**

- | 属性     | 类型                       | 默认 | 是否必需                 | 描述                                             |
  | -------- | -------------------------- | ---- | ------------------------ | ------------------------------------------------ |
  | continue | {sync \|async \|exclusive} | sync | :heavy_multiplication_x: | 指定是否应在发送此通知电子邮件之前引入异步延续。 |

**reminder属性：**

- | 属性     | 类型                       | 默认 | 是否必需                 | 描述                                             |
  | -------- | -------------------------- | ---- | ------------------------ | ------------------------------------------------ |
  | duedate  | 持续时间                   |      | :heavy_check_mark:       | 在发送提醒电子邮件之前延迟。                     |
  | repeat   | 持续时间                   |      | :heavy_multiplication_x: | 应发送后续提醒电子邮件后的延迟                   |
  | continue | {sync \|async \|exclusive} | sync | :heavy_multiplication_x: | 指定是否应在发送此通知电子邮件之前引入异步延续。 |



1. 接受默认模板

   ```xml
   <task name="review" 
         assignee="#{order.owner}" 
        <notification/>    【分配任务时，发送通知信息】
        <reminder duedate="2 days" repeat="1 day"/> 
   </task>
   ```

   

2. 创建用户时，指定邮箱

   ```java
   identityService.createUser("johndoe", "John", "Doe", "john@doe");
   ```

   

### 4.3.5、decision

根据条件在多个流转路径中选择其一通过，也就是做一个决定性的判断，这时要使用decision活动。

 使用决定性判断有两种方式：

1. 使用expression，如：`expr="#{'to state2'}"`
2. 使用Handler，要实现`DecisionHandler`接口

> 【注意】如果同时配置了expression与Handler，则expression有效，忽略Handler。

#### ①、expression

```xml
<process name="test" xmlns="http://jbpm.org/4.4/jpdl">
    <start  name="start1">
        <transition to="部门经理[审批]"/>
    </start>
    <end name="end1"/>
    <task assignee="部门经理" name="部门经理[审批]">
        <transition to="exclusive1"/>
    </task>
    <task assignee="总经理" name="总经理[审批]">
        <transition name="to end1" to="end1"/>
    </task>
    <decision name="exclusive1" expr="#{money>1000?'to 总经理[审批]':'to end1'}">  【使用表达式】
        <transition name="to 总经理[审批]" to="总经理[审批]"/>
        <transition name="to end1" to="end1"/>
    </decision>
</process>
```

```java
Map<String, Object> variables = new HashMap<String, Object>(); 
variables.put("money", "30000"); 
ProcessInstance processInstance = executionService.startProcessInstanceByKey("test", variables);
```



#### ②、Handler

```xml
<process name="test" xmlns="http://jbpm.org/4.4/jpdl">
    <start  name="start1">
        <transition to="部门经理[审批]"/>
    </start>
    <end name="end1"/>
    <task assignee="部门经理" name="部门经理[审批]">
        <transition to="exclusive1"/>
    </task>
    <task assignee="总经理" name="总经理[审批]">
        <transition name="to end1" to="end1"/>
    </task>
    <decision name="exclusive1">
        <handler class="cn.itcast.g_decision.DecisionHandlerImpl" />       【指定Handler】
        <transition name="to 总经理[审批]" to="总经理[审批]"/>
        <transition name="to end1" to="end1"/>
    </decision>
</process>
```



决策处理程序是一个实现`DecisionHandler`接口的 Java 类

```java
// decision 标签指定的 自动处理类
public class DecisionHandlerImpl implements DecisionHandler {
    /** 计算并返回要使用的Transition的名称 */
    public String decide(OpenExecution execution) {
        // 获取流程变量
        int money = (Integer) execution.getVariable("报销金额");

        // 计算要使用的路线并返回
        if (money > 1000) {
            return "to 总经理[审批]";
        } else {
            return "to end1";
        }
    }
}
```

```java
Map<String, Object> variables = new HashMap<String, Object>();
// variables.put("报销金额", 500);
variables.put("报销金额", 5000);
ProcessInstance pi = processEngine.getExecutionService().startProcessInstanceByKey("test", variables);

// 查询出本流程实例中当前情况下仅有一个任务（部门经理审批）
Task task = processEngine.getTaskService()//
    .createTaskQuery()//
    .processInstanceId(pi.getId())// 
    .uniqueResult();

// 办理任务，使用指定的Transition离开
processEngine.getTaskService().completeTask(task.getId());
```



### 4.3.6、fork、join

这是多个==分支并行==（同时）执行的，并且所有的分支Execution==都到Join活动后==才继续向后执行。

1. `fork`：分支。一条分多条

2. `join`：聚合。多条合一条

   | 属性         | 类型                                         | 默认          | 是否必须                 | 描述                                                         |
   | ------------ | -------------------------------------------- | ------------- | ------------------------ | ------------------------------------------------------------ |
   | multiplicity | 整数/表达式                                  | 传入转化的nbr | :heavy_multiplication_x: | 在连接激活并将执行推出连接的单个传出转换之前应到达此连接的执行次数。 |
   | lockmode     | {none, read, upgrade, upgrade_nowait, write} | upgrade       | :heavy_multiplication_x: | 应用于父执行的休眠锁定模式，以防止 2 个并发事务在尚未到达连接时看到彼此，从而导致进程死锁。 |

### 4.3.7、sub-process

创建一个子流程实例并等待它完成。当子流程实例完成时，子流程中的执行将继续。

**sub-process属性：**

- | 属性            | 类型          | 默认 | 是否必须                        | 描述                                                         |
  | --------------- | ------------- | ---- | ------------------------------- | ------------------------------------------------------------ |
  | sub-process-id  | 字符串/表达式 |      | 需要此或子流程键                | 通过id标识子进程。这意味着引用了流程定义的特定版本。子进程 id 可以指定为简单的文本或 EL 表达式。 |
  | sub-process-key | 字符串/表达式 |      | 需要此或子流程键                | 通过键标识子进程。这意味着引用了具有给定键的流程定义的最新版本。每次活动执行时都会查找流程的最新版本。子流程键可以指定为简单文本或 EL 表达式。 |
  | outcome         | 表达式        |      | `outcome-value`指定了转换时需要 | 子流程实例结束时计算的表达式。然后将该值用于结果转换映射。将`outcome-value`元素添加到此`sub-process`活动的传出转换。 |

**sub-process元素：**

- | 元素          | 多样性 | 描述                                                       |
  | ------------- | ------ | ---------------------------------------------------------- |
  | parameter-in  | 0..*   | 声明一个在创建时传递给子流程实例的变量。                   |
  | parameter-out | 0..*   | 声明一个变量，该变量将在子流程结束时在超级流程执行中设置。 |

 **parameter-in属性：**

- | 属性   | 类型   | 默认 | 是否必需                          | 描述                                                         |
  | ------ | ------ | ---- | --------------------------------- | ------------------------------------------------------------ |
  | subvar | string |      | :heavy_check_mark:                | 在其中设置值的子流程变量的名称。                             |
  | var    | string |      | 需要 {'var', 'expr'} 之一来指定值 | 父流程执行上下文中变量的名称。                               |
  | expr   | string |      | 需要 {'var', 'expr'} 之一来指定值 | 将在**父**流程执行上下文中解析的表达式。结果值将设置在子流程变量中。 |
  | lang   | string | juel | :heavy_multiplication_x:          | 应解析表达式的脚本语言。                                     |

 **parameter-out属性：**

- | 属性   | 类型   | 默认 | 是否必需                          | 描述                                                         |
  | ------ | ------ | ---- | --------------------------------- | ------------------------------------------------------------ |
  | subvar | string |      | :heavy_check_mark:                | 将在其中设置值的父流程执行上下文中的变量名称。               |
  | var    | string |      | 需要 {'var', 'expr'} 之一来指定值 | 从中获取值的子流程变量的名称。                               |
  | expr   | string |      | 需要 {'var', 'expr'} 之一来指定值 | 将在**子**流程执行上下文中解析的表达式。结果值将设置在父过程变量中。 |
  | lang   | string | juel | :heavy_multiplication_x:          | 应解析表达式的脚本语言。                                     |

**transition结果变量映射的额外元素：**

- | 元素          | 多样性 | 描述                                                         |
  | ------------- | ------ | ------------------------------------------------------------ |
  | outcome-value | 0..*   | 如果`outcome`匹配该值，则在子流程结束后进行此转换。该值由一个子元素指定。 |



#### ①、变量

1. 父流程 xml文件

   ![子流程文档示例流程](images\01_JBPM4.4\process.task.png)

   ```xml
   <process name="SubProcessDocument" xmlns="http://jbpm.org/4.4/jpdl">
       <start>
           <transition to="review" />
       </start>
   
       <sub-process name="review" sub-process-key="SubProcessReview"> 【指定子流程的name】
           <parameter-in var="document" subvar="document" />
           <parameter-out var="reviewResult" subvar="result" />
           <transition to="wait" />
       </sub-process>
   </process>
   ```

   

2. 子流程 xml文件

   ![子流程审核示例流程](images\01_JBPM4.4\process.subprocess.review.png)

   ```xml
   <process name="SubProcessReview" xmlns="http://jbpm.org/4.4/jpdl">
       <start g="20,20,48,48">
           <transition to="get approval"/>
       </start>
   
       <task name="get approval" assignee="johndoe">
           <transition to="end"/>
       </task>
   
       <end name="end" />
   </process>
   ```

   

3.  

   ```java
   // 1. 启动父流程
   Map<String, Object> variables = new HashMap<String, Object>();
   variables.put("document", "这份文件描述了我们如何赚更多的钱......");
   ProcessInstance processInstance = executionService
       .startProcessInstanceByKey("SubProcessDocument", variables);
   
   // 2. 然后父流程执行将到达子流程活动。子流程实例被创建并与父流程执行链接。当SubProcessReview流程实例启动时，它到达task. 将创建一个任务johndoe。
   List<Task> taskList = taskService.findPersonalTasks("johndoe");
   Task task = taskList.get(0);
   
   // 3. 从父流程实例传递到了子流程实例
   String document = (String) taskService.getVariable(task.getId(), "document");
   
   // 4. 在任务上设置一个变量。这通常是通过表单完成的。但在这里我们将展示它是如何以编程方式完成的。
   variables = new HashMap<String, Object>();
   variables.put("result", "accept");
   taskService.setVariables(task.getId(), variables);
   
   // 5. 完成此任务，将导致子流程实例结束。
   taskService.completeTask(task.getId());
   
   // 6. 从子流程实例传递到了父流程实例
   String result = (String) executionService.getVariable(processInstance.getId(), "reviewResult");
   assertEquals("accept", result);
   ```



#### ②、子流程结果影响父流程

1. 父流程 xml文件

   ![子流程文档示例流程](images\01_JBPM4.4\process.subprocess.document.png)

   ```xml
   <process name="SubProcessDocument"> 
   
       <start> 
           <transition to="review" /> 
       </start> 
   
       <sub-process name="review"  sub-process-key="SubProcessReview" result="#{result}" >  【指定子流程name】
           <transition name="ok" to="next step" /> 
           <transition name="nok" to="update" /> 
           <transition name="reject" to="close" /> 
       </sub-process> 
       <state name="next step" /> 
       <state name="update" /> 
       <state name="close"/> 
   </process>
   ```

   

2. 子流程 xml文件

   ![结果值的子流程审查示例流程](images\01_JBPM4.4\process.subprocess.review-1625818980607.png)

   ```xml
   <process name="SubProcessReview" xmlns="http://jbpm.org/4.4/jpdl"> 
       <start> 
           <transition to="get审批"/> 
       </start> 
   
       <task name="get审批" assignee= "johndoe"> 
           <transition to="end"/> 
       </task> 
   
       <end name="end" /> 
   </process>
   ```

   

3.  

   ```java 
   // 1. 启动父流程的实例 
   ProcessInstance processInstance = executionService
       .startProcessInstanceByKey("SubProcessDocument");
   
   // 2. 然后从johndoe的任务列表中获取任务
   List<Task> taskList = taskService.findPersonalTasks("johndoe");
   Task task = taskList.get(0);
   
   // 3. 然后设置result变量并完成任务。
   Map<String, Object> variables = new HashMap<String, Object>();
   variables.put("result", "ok");
   taskService.setVariables(task.getId(), variables);
   taskService.completeTask(task.getId());
   // 在这种情况下，ok转换是在子流程审查活动之外的父流程中进行的。
   ```

   

#### ③、子流程结束节点影响父流程

1. 父流程 xml文件

   ![结果活动的子流程文档示例流程](images\01_JBPM4.4\process.subprocess.document-1625819698315.png)

   ```xml
   <process name="SubProcessDocument"> 
   
       <start> 
           <transition to="review" /> 
       </start> 
   
       <sub-process name="review"  sub-process-key="SubProcessReview">   【设置子流程name】
           <transition name="ok" to="next step" /> 
           <transition name="nok" to="update" /> 
           <transition name="reject" to="close" /> 
       </sub-process> 
   
       <state name="next step" /> 
       <state name="update" /> 
       <state name="close" /> 
   </process>
   ```

   

2. 子流程 xml文件

   ![结果活动的子流程审查示例流程](images\01_JBPM4.4\process.subprocess.outcomeactivity.review.png)

   ```xml
   <process name="SubProcessReview" xmlns="http://jbpm.org/4.4/jpdl"> 
       <start> 
           <transition to="get审批"/> 
       </start> 
   
       <task name="get审批"  assignee= "johndoe"> 
           <transition name="ok" to="ok"/> 
           <transition name="nok" to="nok"/> 
           <transition name="reject" to="reject"/> 
       </task> <end name="ok" /> 
       <end name="nok" /> 
       <end name="reject" /> 
   </process>
   ```

   

3.  

   ```java
   // 1. 启动 父流程
   ProcessInstance processInstance = executionService
       .startProcessInstanceByKey("SubProcessDocument");
   
   // 2. 从johndoe的任务列表中获取任务
   List<Task> taskList = taskService.findPersonalTasks("johndoe");
   Task task = taskList.get(0);
   
   // 3. 任务完成，结果为ok
   taskService.completeTask(task.getId(), "ok");
   // 这将导致子流程以最终活动结束ok。父进程执行然后将传出转换ok 到next step.
   ```

## 4.4、Custom

在`<custom>`元素中指定class属性为指定的类。

这个类要实现`ExternalActivityBehaviour`接口，其中有两个方法：

1. `execute(ActivityExecution)`，节点的功能代码
2. `signal(ActivityExecution, String, Map)`，在当前节点等待时，外部发信号时的行为
3. 在execute()方法中，可以调用以下方法对流程进行控制
   - `ActivityExecution.waitForSignal()`，在当前节点等待。
   - `ActivityExecution.takeDefaultTransition()`，使用默认的Transition离开，当前节点中定义的第一个为默认的。
   - `ActivityExecution.take(String transitionName)`，使用指定的Transition离开
   - `ActivityExecution.end()`，结束流程实例
4. 也可以实现ActivityBehaviour接口，只有一个方法execute(ActivityExecution)，这样就不能等待，否则signal时会有类转换异常。

 

```xml
<process name="test" xmlns="http://jbpm.org/4.4/jpdl">
    <start  name="start1">
        <transition name="to 发送短信" to="发送短信"  />
    </start>

    <end  name="end1"/>
    <custom name="发送短信"  class="cn.itcast.k_custom.ExternalActivityBehaviourImpl">
        <transition name="to end1" to="end1" "/>
    </custom>
</process>
```

```java
public class ExternalActivityBehaviourImpl implements ExternalActivityBehaviour {
    /** 到达本活动后要执行的操作（也就是本活动的行为）。
	 * 默认是执行完本方法就离开本活动！ */
    public void execute(ActivityExecution execution) throws Exception {
        System.out.println("--------> ExternalActivityBehaviourImpl.execute()");
        System.out.println("发送短信");

        // 使用默认的Transition离开本活动（这也是默认的行为）
        // execution.takeDefaultTransition();

        // 使用指定名称的Transition离开本活动
        // execution.take(transitionName);

        // 不离开本活动，而是在本活动上等待，只有在外部调用了signalExecutionById()时才会离开本活动.
        execution.waitForSignal();
    }

    /**
	 * 离开本活动前要执行的操作（如果名称为beforeSignal()就好理解了）。
	 * 如果执行完execute()方法就离开了，没有在本活动停留，此方法是不会执行的。
	 * 只有在本活动上停留了，外部调用signalExecutionById()离开本活动时，在真正离开前才会调用这个方法.
	 */
    public void signal(ActivityExecution execution, String signalName, Map<String, ?> parameters) throws Exception {
        System.out.println("--------> ExternalActivityBehaviourImpl.signal()");
    }
}
```



## 4.5、自动流程控制

### 4.5.1、java

Java 任务。流程执行将执行在此活动中配置的类的方法。

**java属性：**

- | 属性   | 类型   | 默认 | 是否必需            | 描述                               |
  | ------ | ------ | ---- | ------------------- | ---------------------------------- |
  | class  | 类     |      | :必须指定class/expr | 完全限定的类名                     |
  | expr   | 表达式 |      | 必须指定class/expr  | 返回应调用方法的目标对象的表达式。 |
  | method | 方法名 |      | :heavy_check_mark:  | 要调用的方法的名称                 |
  | var    | 变量名 |      | :x:                 | 应存储返回值的变量的名称。         |

**java元素：**

- | 元素  | 多样性 | 描述                                     |
  | ----- | ------ | ---------------------------------------- |
  | field | 0..*   | 描述在调用方法之前注入成员字段的配置值。 |
  | arg   | 0..*   | 方法参数                                 |



1. 流程 xml

   ![一个java任务](images\01_JBPM4.4\process.java.png)

   ```xml
   <process name="Java" xmlns="http://jbpm.org/4.4/jpdl">
       <start g="20,20,48,48">
           <transition to="greet" />
       </start>
   
       <java name="greet"
             class="org.jbpm.examples.java.JohnDoe"
             method="hello"
             var="answer">
           <field name="state"><string value="fine"/></field>
           <arg><string value="Hi, how are you?"/></arg>
           <transition to="shake hand" />
       </java>
   
       <java name="shake hand"
             expr="#{hand}"
             method="shake"
             var="hand">
           <arg><object expr="#{joesmoe.handshakes.force}"/></arg>
           <arg><object expr="#{joesmoe.handshakes.duration}"/></arg>
           <transition to="wait" />
       </java>
       <state name="wait" />
   </process>
   ```

   

2. 绑定的java类

   ```java
   public class JohnDoe implements Serializable {
       String state;
   
       public String hello(String msg) {
           if ( (msg.indexOf("how are you?")!=-1)
              ) {
               return "I'm "+state+", thank you.";
           }
           return null;
       }
   }
   ```

3. 若是使用 `expr`属性

   ```java
   Map<String, Object> variables = new HashMap<String, Object>();
   variables.put("hand", new Hand());
   variables.put("joesmoe", new JoeSmoe());
   
   ProcessInstance processInstance = executionService.startProcessInstanceByKey("Java", variables);
   ```

   

### 4.5.2、script

脚本活动评估脚本

#### ①、expression

**script 表达式属性：**

- | 属性 | 类型               | 默认                     | 是否必需           | 描述                     |
  | ---- | ------------------ | ------------------------ | ------------------ | ------------------------ |
  | expr | 文本               |                          | :heavy_check_mark: | 要评估的表达式文本       |
  | lang | 定义的脚本语言名称 | 定义的默认**表达式**语言 | :x:                | 指定表达式的语言。       |
  | var  | 变量名             |                          | :x:                | 应存储返回值的变量的名称 |



1. 流程xml文件

   ```xml
   <process name="ScriptExpression" xmlns="http://jbpm.org/4.4/jpdl">
       <start>
           <transition to="invoke script" />
       </start>
   
       <script name="invoke script"
               expr="Send packet to #{order.address}"
               var="text">
           <transition to="wait" />
       </script>
   
       <state name="wait" />
   </process>
   ```

   

2. 脚本类

   ```java
   public class Order implements Serializable{
     private static final long serialVersionUID = 1L;
   
     String address;
   
     public Order(String address) {
       this.address = address;
     }
   
     public String getAddress() {
       return address;
     }
   
     public void setAddress(String address) {
       this.address = address;
     }
   }
   ```

   

3. 使用

   ```java
   // 当为此流程启动流程实例时，我们将给定的地址属性作为变量提供给一个人person。
   Map<String, Object> variables = new HashMap<String, Object>();
   variables.put("order", new Order("Berlin"));
   
   Execution execution = executionService.startProcessInstanceByKey("ScriptExpression", variables);
   String executionId = execution.getId();
   // 执行脚本活动后，变量text 将包含“向檀香山发送数据包”。
   ```

   

#### ②、text

| 属性 | 类型               | 默认                     | 是否必需 | 描述                     |
| ---- | ------------------ | ------------------------ | -------- | ------------------------ |
| lang | 定义的脚本语言名称 | 定义的默认**表达式**语言 | :x:      | 指定表达式的语言。       |
| var  | 变量名             |                          | :x:      | 应存储返回值的变量的名称 |

```xml
<process name="ScriptText" xmlns="http://jbpm.org/4.4/jpdl"> 

    <start> 
        <transition to="invoke script" /> 
    </start> 
    <script name="invoke script"   var= "text"> 
        <text>
            发送数据包到 #{person.address} 
        </text> 
        <transition to="wait" /> 
    </script> 
    <state name="wait"/> 
</process>
```

这个过程的执行和上面的脚本表达式完全一样。



### 4.5.3、hql

通过该`hql`活动，可以对数据库执行 HQL 查询，并将结果存储在流程变量中。

 **hql属性：**

- | 属性   | 类型          | 默认  | 是否必需           | 描述                                                         |
  | ------ | ------------- | ----- | ------------------ | ------------------------------------------------------------ |
  | var    | 变量名        |       | :heavy_check_mark: | 存储结果的变量的名称                                         |
  | unique | {true, false} | false | :x:                | 值为 true 意味着应该使用 method 获取休眠查询的结果`uniqueResult()`。<br />默认值为 false，在这种情况下，该`list()` 方法将用于获取结果。 |

**hql元素：**

- | 元素      | 多样性 | 描述     |
  | --------- | ------ | -------- |
  | query     | 1      | HQL 查询 |
  | parameter | 0..*   | 查询参数 |

  

1. 流程 xml

   ![hql 示例流程](images\01_JBPM4.4\process.hql.png)

   ```xml
   <process name="Hql" xmlns="http://jbpm.org/4.4/jpdl">
       <start>
           <transition to="get task names" />
       </start>
   
       <hql name="get task names"
            var="tasknames with i">
           <query>
               select task.name
               from org.jbpm.pvm.internal.task.TaskImpl as task
               where task.name like :taskName
           </query>
           <parameters>
               <string name="taskName" value="%i%" />
           </parameters>
           <transition to="count tasks" />
       </hql>
   
       <hql name="count tasks"
            var="tasks"
            unique="true">
           <query>
               select count(*)
               from org.jbpm.pvm.internal.task.TaskImpl
           </query>
           <transition to="wait" />
       </hql>
   
       <state name="wait"/>
   </process>
   ```

   

2. 使用

   ```java
   ProcessInstance processInstance = executionService.startProcessInstanceByKey("Hql");
   String processInstanceId = processInstance.getId();
   
   Set<String> expectedTaskNames = new HashSet<String>();
   expectedTaskNames.add("dishes");
   expectedTaskNames.add("iron");
   Collection<String> taskNames = (Collection<String>) executionService.getVariable(processInstanceId,"tasknames with i");
   ```

### 4.5.4、sql

该`sql`活动与`hql`同的 `session.createSQLQuery(...)`是使用。

### 4.5.5、mail

省略



## 7.5、事件

在根元素中，或在节点元素中，使用`<on event="">`元素指定事件，其中event属性代表事件的类型。

在`<on>`中用子元素`<event-listener class="EventListenerImpl" />`，指定处理的类，要求指定的类要实现EventListener接口

事件类型：

1. `<on>`元素放在根元素（`<process>`）中，可以指定event为start或end，表示流程的开始与结束。
2. `<on>`元素放在节点元素中，可以指定event为start或end，表示节点的进入与离开
3. 在Start节点中只有end事件，在End节点中只有start事件。
4. 在`<transition>`元素中直接写`<event-listener class="">`，就是配置事件。（因为在这里只有一个事件，所以不用写on与类型）
5. 在`<task>`元素中还可以配置assign事件，是在分配任务时触发的。

```xml
<process name="test" xmlns="http://jbpm.org/4.4/jpdl">
    <!-- 流程实例的启动的事件 -->
    <on event="start">
        <event-listener class="cn.itcast.l_event.EventListenerImpl"/>
    </on>
    <!-- 流程实例的结束的事件 -->
    <on event="end">
        <event-listener class="cn.itcast.l_event.EventListenerImpl"/>
    </on>

    <start g="99,23,10,2" name="start1">
        <!-- 活动的离开事件 -->
        <on event="end">
            <event-listener class="cn.itcast.l_event.EventListenerImpl"/>
        </on>
        <transition g="-71,-17" name="to 经理审批" to="经理审批"/>
    </start>

    <task assignee="张经理" g="78,118,92,52" name="经理审批">
        <!-- 活动的进入事件 -->
        <on event="start">
            <event-listener class="cn.itcast.l_event.EventListenerImpl"/>
        </on>
        <!-- 活动的离开事件 -->
        <on event="end">
            <event-listener class="cn.itcast.l_event.EventListenerImpl"/>
        </on>
        <transition g="-47,-17" name="to end1" to="end1"/>
    </task>

    <end g="100,213,48,48" name="end1">
        <!-- 活动的进入事件 -->
        <on event="start">
            <event-listener class="cn.itcast.l_event.EventListenerImpl"/>
        </on>
    </end>
</process>
```

```java
public class EventListenerImpl implements EventListener {
    @Override
    public void notify(EventListenerExecution execution) throws Exception {
        System.out.println(" ---> 事件触发了：" + execution.getActivity());
    }
}
```

# 8、表结构

## 8.1、表结构说明与描述

Jbpm4 共有18张表，如下，其中==红色的表为经常使用的表==

### 8.1.1、资源库与运行时表结构

1.  ==JBPM4_DEPLOYMENT 流程定义表==
2.  ==JBPM4_DEPLOYPROP 流程定义属性表==
3.  ==JBPM4_EXECUTION  流程实例表==
    - 作用：主要存放JBPM4的执行信息，Execution机制代替了JBPM3的Token机制
4.  JBPM4_PROPERTY  流程引擎表
5.  ==JBPM4_TASK 任务表==
    - 作用：存放需要人来完成的Activities，需要人来参与完成的Activity称之为Task
6.  ==JBPM4_VARIABLE 上下文表==
    - 作用：存放进行时的临时变量
7.  ==JBPM4_JOB  定时表==
    - 作用：存放Timer的定义
8.  ==JBPM4_LOB  存储表==
9.  ==JBPM4_SWIMLANE泳道表==
    - Swim Lane是一种Runtime Process Role。通过Swim Lane，多个Task可以一次分配到同一Actor身上。
10.  JBPM4_PARTICIPATION 参与者表

### 8.1.2、历史数据表

1. ==JBPM4_HIST_ACTINST 流程活动(节点)实例表==
   - 作用：存放Activity Instance的历史记录
2. ==JBPM4_HIST_DETAIL  流程历史详细表== 
   - 作用：保存Variable的变更记录
3. ==JBPM4_HIST_PROCINST 流程实例历史表==
   - 作用：存放Process Instance的历史记录
4. ==JBPM4_HIST_TASK  流程任务实例历史表==
   - 作用：Task的历史信息
5. ==JBPM4_HIST_VAR 流程变量（上下文）历史表==
   - 作用：保存历史的变量

### 8.1.3、身份认证表结构

1. JBPM4_ID_GROUP 组表
2. JBPM4_ID_MEMBERSHIP 用户角色表
3. JBPM4_ID_USER  用户表

这三张表很常见，基本的权限控制，关于用户认证方面建议还是自己开发一套，组件自带的功能太简单，使用中有很多需求难以满足

## 8.2、操作信息变化

> 【注意】：以下操作步骤向表中增加记录的顺序（经过测试）

1. 发布一个流程deploy后
   - jbpm4_deployment(流程定义)：新增一条记录
   - jbpm4_lob(存储表)： 新增一条记录
   - jbpm4_deployprop(流程定义属性表)：新增四条记录 



2. 上传一个zip包（包含png和jpdl.xml）后
   - JBPM4_DEPLOYMENT多一条记录
   - JBPM4_DEPLOYPROP 多三条
   - JBPM4_LOB多两条。



3. 开始一个流程startProcessInstanceByKey后
   - jbpm4_execution(流程实例表)：新增一条记录
   - jbpm4_hist_procinst(流程实例历史表)：新增一条记录
   - jbpm4_variable (上下表)：新增一条记录
   - jbpm4_task (任务表)：新增一条记录
   - jbpm4_hist_task(任务历史表)：新增一条记录
   - jbpm4_hist_actinst (活动节点实例表)：新增一条记录

 

4. 填写申请信息
   - jbpm4_variable(上下表) ： 新增N条记录,根据表单信息决定
   - jbpm4_task (任务表)：新增一条记录
   - jbpm4_hist_task(任务历史表)：新增一条记录
   - jbpm4_hist_actinst (活动节点实例表)：新增一条记录



5. 审批申请信息
   1. 同意：
      - jbpm4_hist_actinst (活动节点实例表)：新增一条记录
   2. 驳回：
      - jbpm4_task (任务表)：新增一条记录
      - jbpm4_hist_task(任务历史表)：新增一条记录
      - jbpm4_hist_actinst (活动节点实例表)：新增一条记录



6. 审批结束
   - jbpm4_hist_actinst (活动节点实例表)：新增一条记录




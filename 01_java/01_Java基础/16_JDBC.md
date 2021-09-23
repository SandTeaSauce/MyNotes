# 1. JDBC基础

JDBC：Java Database Connectivity，即Java数据库连接
1. 它是一种可以执行SQL语句的Java API
2. 程序可以通过JDBC API连接到关系数据库，并使用SQL来完成对数据库的查询、更新



## 1.1. JDBC简介

Sun提供的JDBC可以完成的3个基本工作
1. 建立与数据的连接
2. 执行SQL语句
3. 获取SQL语句的执行结果



## 1.2. JDBC驱动程序

数据库驱动程序：JDBC程序和数据库之间的转换层，数据库驱动程序负责将JDBC调用映射成特定的数据库调用

![image-20191222151212642](https://java-picgo-img.oss-cn-beijing.aliyuncs.com/01_java%E5%9F%BA%E7%A1%80/16_JDBC/20210515105146.png)。

JDBC驱动通常有如下4种类型：

1. JDBC-ODBC桥：Java8中已删除
2. 直接将JDBC API映射成数据库特定的客户端API。这种驱动包含特定数据库的本地代码，用于访问特定数据库的客户端
3. 支持三层结构的JDBC访问方式，主要用于applet阶段
4. 存Java，直接与数据库实例进行交互，智能。知道数据库使用的底层协议，JDBC中最流行的



JDBC比ODBC的优势：

1. ODBC更复杂
2. JDBC比ODBC安全性更高、更易部署



# 2. JDBC的典型用法

## 2.1. JDBC4.2 常用接口和类简介

Java8支持JDBC4.2标准，Java8新增功能：



`DriverManager`：用于管理JDBC驱动的服务类。该类可以获取`Connection`对象

1. `getConnection(url，user，pass)`：获取url对应的数据库的连接

2. `Connection`：代表数据库连接对象，每个Connection代表一个物理链接会话。想要访问数据库，必须获取数据库的连接

  - `createStatement`：返回一个Statement对象

  - `prepareStatement（String）`：返回预编译的Statement对象，即将SQL提交到数据库进行==预编译==

  - `prepareCall（String）`：返回CallableStatement对象，该对象用于调用==存储过程==

    

`Connection`：控制事务的方法

1. `setSavepoint`：创建一个保存点
2. `setSavepoint（String）`：以指定名字来创建一个保存点
3. `setTransactionIslation（int）`：设置事务的隔离级别
4. `rollback`：回滚事务
5. `rollback（Savepoint）`：将事务回滚到指定保存点
6. `setAutoCommit（Boolean）`：关闭自动提交，打开事务
7. `commit`：提交事务
8. Java7新增方法
  - `setSchema（String）`：控制该Connection访问的数据库Schema
  - `getSchema`：获取该Connection访问的数据库Schema的连接
  - `getNetworkTimeout（Executor，int）`：控制数据库连接超时行为
  - `setNetworkTimeout`：控制数据库连接超时行为



`Statement`：用于执行SQL语句的工具接口

1. `executeQuery（String）`：执行查询语句，返回查询结果为`ResultSet`对象
2. `executeUpdate（String）`：用于执行DML，并返回受影响的行数，也可以执行DDL语句，返回0
3. `execute（String）`：可执行任何SQL。

  - 执行结果若为`ResultSet`，则返回true
  - 执行结果若为受影响行数，则返回false
4. Java7新增方法
  - `closeOnCompletion`：当所有依赖于该Statement的ResultSet关闭时，该Statement会自动关闭
  - `isCloseOnCompletion`：判断该Statement是否打开了closeOnCompletion
5. Java8新增方法
  - `executeLargeUpdate`：相当于增强版`executeUpdate`，返回值类型为long，但DML影响数超过`Integer.MAX_VALUE`，则使用者方法

    

`PreparedStatement`：预编译的Statement对象。运行数据库预编译SQL语句。相当于Statement来说，在执行SQL语句时，无须再传入SQL语句，只要为预编译的SQL语句传入参数数值即可

1. `setXxxx（int， Xxx）`：根据传入的参数值的类型不同，根据索引Int传给指定位置
2. 同样有executeUpdate、executeQuery、execute、executeLargeUpdate方法



`ResultSet`：结果集对象。该对象包含查询结果的方法，可以通过列索引或列名获取列数据

1. `close`：释放ResultSet对象
2. `absolute（int）`：将结果集的记录指针移动到第Int行，Int为负数，则移动到倒数第Int行。移动后的记录指针指向有效记录返回true
3. `first`：将记录指针移动到第一行。移动后的记录指针指向有效记录返回true
4. `previous`：将记录指针移动上一行。移动后的记录指针指向有效记录返回true
5. `next`：将记录指针移动下一行。移动后的记录指针指向有效记录返回true
6. `last`：将记录指针移动最后一行。移动后的记录指针指向有效记录返回true
7. `afterLast`：将记录指针移动到最后一行之后
8. `getXxx（Int）/ getXxx（String）`：获取当前行、指定列的值
9. Java7新增方法
  - `getObject（int，Class）/ getObject（String，Class）`：获取任意类型的值



## 2.2. JDBC编程步骤

JDBC编译步骤：

1. 加载数据库驱动

   ```Java
   Class.forName("com.mysql.jdbc.Driver");
   ```

   

2. 通过DriverManager获取数据库连接

   ```Java
   Connection conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/select_test?useSSL=true", "root" , "32147");
   ```

   `mysql8需要加入: serverTimezone=GMT&characterEncoding=utf-8`

3. 通过Connection对象创建Statement对象，方法如下：

   - `createStatement`：创建基本的Statement
   - `prepareStatement（String）`：根据传入SQL创建预编译的Statement对象
   - `prepareCall（String）`：根据传入SQL创建CallableStatement对象

4. 使用Statement执行SQL，方法如下：

   - execute：执行任何SQL
   - executeUpdate：执行DML和DDL语句
   - executeQuery：执行查询语句，返回ResultSet

5. 操作结果集（只有查询语句）

   - next、previous、first、last、beforeFirst、afterLast、absolute等移动记录指针方法
   - getXxx获取记录指针指向行、特定的值

6. 回收数据库资源

   - ResultSet、Statement、Connection等资源



# 3. 执行SQL语句的方式

1. executeLargeUpdate

   ```Java
   // 加载驱动
   Class.forName(driver);
   try (
       // 获取数据库连接
       Connection conn = DriverManager.getConnection(url, user, pass);
       // 使用Connection来创建一个Statment对象
       Statement stmt = conn.createStatement()) {
       // 执行DML,返回受影响的记录条数
       return stmt.executeUpdate(sql);
   }
   ```

   

2. execute

   ```Java
   // 加载驱动
   Class.forName(driver);
   try (
       // 获取数据库连接
       Connection conn = DriverManager.getConnection(url, user, pass);
       // 使用Connection来创建一个Statement对象
       Statement stmt = conn.createStatement()) {
       // 执行SQL,返回boolean值表示是否包含ResultSet
       boolean hasResultSet = stmt.execute(sql);
       // 如果执行后有ResultSet结果集
       if (hasResultSet) {
           try (
               // 获取结果集
               ResultSet rs = stmt.getResultSet()) {
               // ResultSetMetaData是用于分析结果集的元数据接口
               ResultSetMetaData rsmd = rs.getMetaData();
               int columnCount = rsmd.getColumnCount();
               // 迭代输出ResultSet对象
               while (rs.next()) {
                   // 依次输出每列的值
                   for (int i = 0; i < columnCount; i++) {
                       System.out.print(rs.getString(i + 1) + "\t");
                   }
                   System.out.print("\n");
               }
           }
       } else {
           System.out.println("该SQL语句影响的记录有" + stmt.getUpdateCount() + "条");
       }
   }
   ```

   

3. PreparedStatement

   ```Java
   // 获取数据库连接
   Connection conn = DriverManager.getConnection(url, user, pass);
   // 使用Connection来创建一个PreparedStatement对象
   PreparedStatement pstmt = conn.prepareStatement("insert into student_table values(null,?,1)");
   // 100次为PreparedStatement的参数设值，就可以插入100条记录
   for (int i = 0; i < 100; i++) {
       pstmt.setString(1, "姓名" + i);
       pstmt.executeUpdate();
   }
   System.out.println("使用PreparedStatement费时:"  + (System.currentTimeMillis() - start));
   ```

   PreparedStatement比使用Statement好处：

   - PreparedStatement预编译SQL，性能更好
   - PreparedStatement无须“拼接”SQL，编程更简单
   - PreparedStatement可以防止SQL注入，安全性更好

   

4. CallableStatement调用存储过程

   - 执行存储sql `{call 存储过程名称(参数)}`
   
   ```Java
   // 获取数据库连接
   Connection conn = DriverManager.getConnection(url, user, pass);
   // 使用Connection来创建一个CallableStatment对象
   CallableStatement cstmt = conn.prepareCall("{call add_pro(?,?,?)}");
   cstmt.setInt(1, 4);
   cstmt.setInt(2, 5);
   // 注册CallableStatement的第三个参数是int类型
   cstmt.registerOutParameter(3, Types.INTEGER);
   // 执行存储过程
   cstmt.execute();
   // 获取，并输出存储过程传出参数的值。
   System.out.println("执行结果是: " + cstmt.getInt(3));
   ```
   
   

# 4. 管理结果集

## 4.1. 可滚动、可更新的结果集

默认方式打开的ResultSet是不可更新。如果需要创建可更新的ResultSet，在创建时Statement或PreparedStatement时传入额外的参数

Connection在创建Statement或PreparedStatement时还可以额外传入如下参数

1. resultSetType：控制ResultSet的类型，该参数可以取如下三个值
   - ResultSet.TYPE_FORWARD_ONLY：该变量控制记录指针可以自由移动。jdk1.4之前默认值
   - ResultSet.TYPE_SCROLL_INSENSITIVE：该变量控制记录指针可以自由移动，但底层数据的改变不会影响ResultSet的内容
   - ResultSet.TYPE_SCROLL_SENSITIVE：该变量控制记录指针可以自由移动，但底层数据的改变会影响ResultSet的内容
2. resultSetConcurrency：控制ResultSet的并发类型
   - ResultSet.CONCUR_READ_ONLY：制度的并发模式（默认）
   - ResultSet.CONCUR_UPDATABLE：可更新的并发模式

```java
// 使用 Connection 创建一个 PrepareStatement (传入控制结果集 可滚动、可更新 的参数)
pstmt = conn.prepareStatement(sql, ResultSet.TYPE_SCROLL_INSENSITIVE, ResultSet.CONCUR_UPDATABLE);
```



> 【警告】可更新的结果集满足条件：
>
> 1. 所有数据应该来自一张表
> 2. 选出的数据集必须包含主键列



可调用 ResultSet的 `updateXxxx（int，Xxxx）`来修改记录指针所指记录、特定列的值。

最后使用：ResultSet的 updateRow（）来提交修改



Java8为 ResultSet增加了 updateObject（String，Object，SQLType）和 updateObject（int，Object，SQLType）：用Object来修改记录指针所记录、特定列的值，其中SQLType用于指定数据列的类型。



## 4.2. 处理Blob类型的数据

Blob（Binary Long Object），二进制长对象，用于存储大文件，可以把图片、声音等文件的二进制数据保存在数据库中

Blob数据插入数据库需要使用PreparedStatement，setBinaryStream（int，InputStream）：将指定参数传入二进制输入流

ResultSet取出Blob：
1. ResultSet的getBlob（int）：返回一个iBlob对象

2. Blob的getBinaryStream（）：获取该Blob数据的输入流

3. Blob的getBytes（）：取出Blob对象封装的二进制数据

   

Mysql中使用 `mediumblob`类型和 `blob`类型
1. blob：只能存储64kb，不推荐
2. mediumblob：可以存储16mb



## 4.3. ResultSetMetaData分析结果集

ResultSet使用中不清楚包含哪些数据列、数据类型，则需要通过 ResultSetMetaData来获取ResultSet的描述信息

2. `ResultSet的getMetaData（）`：获取ResultSet对应的ResutlSetMetaData对象
3. ResutlSetMetaData方法：
   - `getColumnCount`：返回该ResultSet的列数量
   - `getColumnName（int）`：返回指定索引的列明
   - `getColumnType（int）`：返回指定索引的列类型



# 5. Java7新增的RowSet

RowSet接口继承了ResultSet接口，RowSet接口包含：JdbcRowSet（保持与数据库的连接）；CachedRowSet、FilteredRowSet、JoinRowSet、WebRowSet这四个都是离线的RowSet，无须保持与数据库连接

RowSet默认是可滚动、可更新、可序列化的结果集，比ResultSet强，性能更好



## 5.1. Java7新增RowSetFactory与RowSet

Java7新增：
1. RowSetProvider：创建RowSetFactory
2. RowSetFactory：创建RowSet实例



RowSetFactory创建RowSet实例方法：（==注意：RowSetFactory创建的RowSet没有装填数据==）
1. `createCachedRowSet（）`：创建一个默认的CacheRowSet
2. `createFilteredRowSet（）`：创建一个默认的FilteredRowSet
3. `createJdbcRowSet（）`：创建一个默认的JdbcRowSet
4. `createJoinRowSet（）`：创建一个默认的JoinRowSet
5. `createWebRowSet（）`：创建一个默认的WebRowSet



RowSet抓取数据需要设置数据库URL、用户名、密码。因此RowSet接口中定义方法如下：

1. `setUrl（String）`：设置要访问的数据库的URL
2. `setUsername（String）`：设置要访问的数据库的用户名
3. `setPassword（String）`：设置要访问的数据库的密码
4. `setCommand（String）`：设置Sql的查询结果来填充该RowSet
5. `execute（）`：执行查询

```Java
// 加载驱动
Class.forName(driver);
// 使用RowSetProvider创建RowSetFactory
RowSetFactory factory = RowSetProvider.newFactory();
try(
    // 使用RowSetFactory创建默认的JdbcRowSet实例
    JdbcRowSet jdbcRs = factory.createJdbcRowSet()) {
    // 设置SQL查询语句
    jdbcRs.setCommand(sql);
    // 设置必要的连接信息
    jdbcRs.setUrl(url);
    jdbcRs.setUsername(user);
    jdbcRs.setPassword(pass);

    // 执行查询
    jdbcRs.execute();
    jdbcRs.afterLast();
    // 向前滚动结果集
    while (jdbcRs.previous()) {
        System.out.println(jdbcRs.getString(1)+ "\t" + jdbcRs.getString(2) + "\t" + jdbcRs.getString(3));
        if (jdbcRs.getInt("student_id") == 3) {
            // 修改指定记录行
            jdbcRs.updateString("student_name", "孙悟空");
            jdbcRs.updateRow();
        }
    }
}
```



## 5.2. 离线RowSet

Connection关闭之后不能操作 ResultSet，这时就需要使用 ==离线RowSet==

```java
Class.forName(driver);
// 【1】、获取数据库连接
Connection conn = DriverManager.getConnection(url , user , pass);
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery(sql);

// 【2】、使用RowSetProvider创建RowSetFactory
RowSetFactory factory = RowSetProvider.newFactory();
// 【3】、创建默认的CachedRowSet实例
CachedRowSet cachedRs = factory.createCachedRowSet();
// 【4】、使用ResultSet装填RowSet
cachedRs.populate(rs);    // ① 这里才是重点
// 【5】、关闭资源
rs.close();
stmt.close();
conn.close();
return cachedRs;
// 【6】、操作 RowSet
cachedRs..afterLast();
```

这里任然可以操作（读取、修改）RowSet，因为他是离线的，但是不会同步数据库

同步数据库

```Java
// 重新获取数据库连接
Connection conn = DriverManager.getConnection(url, user , pass);
conn.setAutoCommit(false);
// 把对RowSet所做的修改同步到底层数据库
rs.acceptChanges(conn);
```



## 5.3. 离线RowSet查询分页

CachedRowSet控制分页方法：
1. `populate（ResultSet，int）`：使用指定ResultSet填充RowSet，从第int条记录开始填充
2. `setPageSize（int）`：设置CachedRowSet每次返回多少条记录
3. `previousPage`：读取上一页记录（ResultSet可用）
4. `nextPage`：读取下一页记录（ResultSet可用）





# 6. 事务处理

## 6.1. 基础

事务四大特性：原子性、一致性、隔离性、持续性

事务提交方式：
1. 显示提交：使用commit
2. 自动提交：执行DDL、DCL、程序

事务回滚方式：
1. 显示回滚：使用 rollback
2. 自动回滚：系统错误或强行退出



## 6.2. JDBC的事务支持

JDBC连接的事务支持由Connection提供，默认自动提交，即关闭事务

Connection的setAutoCommit（）方法来关闭自动提交，开启事务

```Java
// 【1】、开启事务
connection.setAutoCommit(false);
// 【2】、执行多条DML语句
statment.executeUpdate(sql);
statment.executeUpdate(sql);
statment.executeUpdate(sql);
// 【3】、提交事务  // 回滚事务
connection.commit();// conn.rollback();
```



当Connection遇到一个未处理的SQLException时，系统会退出，事务自动回滚

Connection提供了创建中间的方法

1. `setSavepoint（）`：在当前事务中创建一个未命名的中间点，并返回中间点的SavePoint对象
2. `setSavepoint（String）`：在当前事务中创建一个名为String的中间点，并返回中间点的SavePoint对象



Connection可以回滚到指定的中间点：`rollback（Savepoint）`



## 6.3. Java8增强的批量更新

`Statement`对象的 `addBatch`（）方法将多条SQL收集起来。最后调用Java8提供的`executeLargeBatch`（推荐使用）或者 `executeBatch` 进行sql操作

```Java
try(
    Connection conn = DriverManager.getConnection(url , user , pass))
{
    // 关闭自动提交，开启事务
    conn.setAutoCommit(false);
    // 保存当前的自动的提交模式
    boolean autoCommit = conn.getAutoCommit();
    // 关闭自动提交
    conn.setAutoCommit(false);
    try(
        // 使用Connection来创建一个Statement对象
        Statement stmt = conn.createStatement())
    {
        // 循环多次执行SQL语句
        for (String sql : sqls)
        {
            stmt.addBatch(sql);
        }
        // 同时提交所有的SQL语句
        stmt.executeLargeBatch();
        // 提交修改
        conn.commit();
        // 恢复原有的自动提交模式
        conn.setAutoCommit(autoCommit);
    }
    // 提交事务
    conn.commit();
```



# 7. 使用连接池管理连接

==问题==：数据库连接的建立及关闭是及其耗费系统资源的操作
==解决==：当应用程序启动时，系统主动创建足够的数据库连接，并将这些连接组成一个连接池。每次应用程序请求数据库时，无须重新打开，而是从数据库中取出已有的连接使用，使用完毕后再将其连接返回连接池。

数据库连接池是Connection对象的工厂，数据库连接池的常用参数：
1. 数据库的初始连接数
2. 连接池的最大连接数
3. 连接池的最小连接数
4. 连接池每次增加的容量

jdbc的数据库连接池使用`javax.sql.DataSource`，`DataSource`：由商用服务器或开源组织提供



## 7.1. DBCP数据源

DBCP获取数据库方式

```Java
// 创建数据源对象，配置参数
BasicDataSource dataSource = new BasicDataSource();
dataSource.setDriverClassName("com.mysql.jdbc.Driver");
dataSource.setUrl("jdbc:mysql://localhost:3306/mydb3");
dataSource.setUsername("root");
dataSource.setPassword("123");

// 设置最大活动连接数、最小空闲连接、最大等待时间
dataSource.setMaxActive(20);
dataSource.setMinIdle(3);
dataSource.setMaxWait(1000);
```



获取数据库连接

```sql
Connection connection = dataSource.getConnection();
```



当数据库访问结束后，关闭数据库资源

```Java
connection.close();
```

> 【危险】这里没有关闭资源，而是归还给连接池



## 7.2. C3P0

数据源性能比DBCP更好，推荐使用（C3P0不仅自动清理不再使用的Connection，还可以自动清理Statement和ResultSet）

使用C3P0

```Java
// 创建数据源对象，并配置参数
ComboPooledDataSource dataSource = new ComboPooledDataSource();
dataSource.setDriverClass("com.mysql.jdbc.Driver");
dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/zhanghanlun");
dataSource.setUser("zhanghanlun");
dataSource.setPassword("123456");

// 设置初始化连接数、最大连接数、最小连接数、
dataSource.setInitialPoolSize(3);
dataSource.setMaxPoolSize(10);
dataSource.setMinPoolSize(3);
```

3. 获取数据库连接

   ```sql
   Connection connection = dataSource.getConnection();
   ```

   

4. 当数据库访问结束后，关闭数据库资源

   ```Java
   connection.close();
   ```


> 【危险】这里没有关闭资源，而是归还给连接池


























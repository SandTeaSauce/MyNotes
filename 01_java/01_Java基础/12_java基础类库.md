# 1、与用户互动

## 1.1、Scanner

`Scanner`类：获取键盘输入

1. `hasNextXxx`（）：是否还有下一个输入项

2. `nextXxx`（）：获取下一个输入项

   

`Scanner`的读取操作可能被阻塞来等待消息的输入

1. `useDelimiter（String）`：设置分割符

2. `hasNextLine（）`：返回输入源中是否还有下一行

3. `nextLine（）`：返回输入源中下一行的字符串

   

`Scanner`还可以读取文件输入



# 2、系统相关

## 2.1、System类

`System`类代表当前Java程序的运行平台，程序不能创建`System`类的对象，`System`类提供了一些类变量和类方法

`System`类提供了代表标准输入、标准输出、错误输出的类变量  。并提供了静态方法访问环境变量、系统属性

## 2.2、Runtime类

`Runtime`类代表Java程序的运行时环境，每个Java程序都有一个与之对应的Runtime实例，应用程序通过该对象与其运行时环境相连接。



# 3、常用类

## 3.1、Object 类

`Object`类是所有类、数组、枚举类的父类。

`Object`常用方法：

1. `equals`：判断指定对象与该对象是否相等。Object中的相等规则：两个对象是同一个对象
2. `finalize`：当系统中没有引用变量引用该对象时，垃圾回收机制调用此方法清除该对象的资源
3. `getClass`：返回该对象的运行时类`hashCode`：返回该对象的`HashCode`值。默认根据对该对象的地址来计算
4. `toString`：返回该对象的字符串表示。
5. `clone`：用于帮助其他对象来实现自我克隆。==这里的克隆只是浅克隆，即只能克隆基础类型变量，引用变量只能克隆引用变量地址==



## 3.2、java7 新增的Objects类

`Objects`工具类，提供了一些工具方法来操作对象，工具方法大多为空指针安全的

```java
// 输出一个null对象的hashCode值，输出0
System.out.println(Objects.hashCode(obj));
// 输出一个null对象的toString，输出null
System.out.println(Objects.toString(obj));
// 要求obj不能为null，如果obj为null则引发异常
System.out.println(Objects.requireNonNull(obj, "obj参数不能是null！"));
```



## 3.3、String、StringBuffer、StringBuilder

### 3.3.1. String

`String`：==不可变类==，即一旦一个String对象被创建以后，包含在这个对象的字符序列不可改变，直到对象被销毁

> 【危险】
>
> ```java
> String str1 = “Java”;
> str1 = str1 + "struts";
> str1 = str1 + "Spring";
> ```
>
> 这里由于String是不可变，会额外产生2个临时变量，所以建议使用StringBuffer或StringBuilder



构造器与方法：

1. `String（）`：创建一个包含0个字符串序列的`String`对象
2. `String（byte，Charset）`：使用指定的字符集将指定`byte`数组解码为新的`String`对象
3. `String（byte，int，int，String）`：使用指定的字符集将指定的`byte`从`int`（第一个）开始，长度为`int`（第二个）的子数组解码成一个新的`String`
4. `String（byte，String）`：使用指定字符集将byte数组解码为String对象
5. `String（char，int，int）`：将指定字符数组从int（第一个）开始，长度为int（第二个）的字符元素连串成String对象
6. `String（String）`：根据字符串直接量创建一个String对象
7. `compareTo`：比较两个字符串的大小。相等=0；不想等<>0
8. `concat（String）`：将当前String对象与实参String对象链接在一起
9. `contentEquals（StringBuffer）`：比较
10. `copyValueOf（char）`：将char数组合并为字符串
11. `copyValueOf（char，int，int）`：将char数组从int（第一个）开始，长度为int（第二个）合并为字符串
12. `endsWith（String）`：返回该String是否以String结尾
13. `equals（Object）`：将该String与指定对象进行比较
14. `equalsIgnoreCase（String）`：与equals相似，忽略大小写
15. `getBytes`：将该String转为byte数组
16. `getChars（int，int，char，int）`：将String中的int（第一个）开始，到int（第二个）结束的字符串复制到char数组中，int（第三个）为目标char数组的起始复制位置
17. `indexOf（char）`：该字符串中第一次出现位置
18. `indexOf（char，int）`：从int开始的第一次出现位置
19. `infexOf（String）`：该字符串中第一次出现位置
20. `indexOf（String，int）`：从int开始的第一次出现位置
21. `lastIndexOf（char）`：该字符串最后一次出现位置
22. `lastIndexOf（char，int）`：从int开始的最后一次出现位置
23. `lastIndexOf（String）`：该字符串最后一次出现位置
24. `lastIndexOf（String，int）`：从int开始的最后一次出现位置length：返回当前字符串长度
25. `replace（char，char）`：将字符串中的第一个char（第一个）替换为char（第二个）
26. `startsWith（String）`：是否以String开始
27. `startsWith（String，int）`：从int开始是否以String开始
28. `toCharArray（）`：将String转化为char数组
29. `toLowerCase`：将字符串转小写
30. `toUpperCase`：将字符串转大写
31. `valueOf（X）`：将基本类型转String

### 3.3.2. StringBuffer

StringBuffer：==字符序列可变==的字符串。

- 想要生成最终的字符串，通过 `toString`将其转化为String对象
- ==线程安全，性能较低==
- 属性
  - length：字符序列长度
  - capacity：StringBuilde的容量

### 3.3.3. StringBuilder

StringBuilder：==jdk1.5新增，可变字符串对象==，

- ==线程不安全，性能较高==
- 方法与StringBuffer相同



## 3.4、Math类

```Java
/*---------下面是三角运算---------*/
// 将弧度转换角度
System.out.println("Math.toDegrees(1.57)："
                   + Math.toDegrees(1.57));
// 将角度转换为弧度
System.out.println("Math.toRadians(90)："
                   + Math.toRadians(90));
// 计算反余弦，返回的角度范围在 0.0 到 pi 之间。
System.out.println("Math.acos(1.2)：" + Math.acos(1.2));
// 计算反正弦；返回的角度范围在 -pi/2 到 pi/2 之间。
System.out.println("Math.asin(0.8)：" + Math.asin(0.8));
// 计算反正切；返回的角度范围在 -pi/2 到 pi/2 之间。
System.out.println("Math.atan(2.3)：" + Math.atan(2.3));
// 计算三角余弦。
System.out.println("Math.cos(1.57)：" + Math.cos(1.57));
// 计算值的双曲余弦。
System.out.println("Math.cosh(1.2 )：" + Math.cosh(1.2 ));
// 计算正弦
System.out.println("Math.sin(1.57 )：" + Math.sin(1.57 ));
// 计算双曲正弦
System.out.println("Math.sinh(1.2 )：" + Math.sinh(1.2 ));
// 计算三角正切
System.out.println("Math.tan(0.8 )：" + Math.tan(0.8 ));
// 计算双曲正切
System.out.println("Math.tanh(2.1 )：" + Math.tanh(2.1 ));
// 将矩形坐标 (x, y) 转换成极坐标 (r, thet));
System.out.println("Math.atan2(0.1, 0.2)：" + Math.atan2(0.1, 0.2));
/*---------下面是取整运算---------*/
// 取整，返回小于目标数的最大整数。
System.out.println("Math.floor(-1.2 )：" + Math.floor(-1.2 ));
// 取整，返回大于目标数的最小整数。
System.out.println("Math.ceil(1.2)：" + Math.ceil(1.2));
// 四舍五入取整
System.out.println("Math.round(2.3 )：" + Math.round(2.3 ));
/*---------下面是乘方、开方、指数运算---------*/
// 计算平方根。
System.out.println("Math.sqrt(2.3 )：" + Math.sqrt(2.3 ));
// 计算立方根。
System.out.println("Math.cbrt(9)：" + Math.cbrt(9));
// 返回欧拉数 e 的n次幂。
System.out.println("Math.exp(2)：" + Math.exp(2));
// 返回 sqrt(x2 +y2)
System.out.println("Math.hypot(4 , 4)：" + Math.hypot(4 , 4));
// 按照 IEEE 754 标准的规定，对两个参数进行余数运算。
System.out.println("Math.IEEEremainder(5 , 2)："
                   + Math.IEEEremainder(5 , 2));
// 计算乘方
System.out.println("Math.pow(3, 2)：" + Math.pow(3, 2));
// 计算自然对数
System.out.println("Math.log(12)：" + Math.log(12));
// 计算底数为 10 的对数。
System.out.println("Math.log10(9)：" + Math.log10(9));
// 返回参数与 1 之和的自然对数。
System.out.println("Math.log1p(9)：" + Math.log1p(9));
/*---------下面是符号相关的运算---------*/
// 计算绝对值。
System.out.println("Math.abs(-4.5)：" + Math.abs(-4.5));
// 符号赋值，返回带有第二个浮点数符号的第一个浮点参数。
System.out.println("Math.copySign(1.2, -1.0)："
                   + Math.copySign(1.2, -1.0));
// 符号函数；如果参数为 0，则返回 0；如果参数大于 0，
// 则返回 1.0；如果参数小于 0，则返回 -1.0。
System.out.println("Math.signum(2.3)：" + Math.signum(2.3));
/*---------下面是大小相关的运算---------*/
// 找出最大值
System.out.println("Math.max(2.3 , 4.5)：" + Math.max(2.3 , 4.5));
// 计算最小值
System.out.println("Math.min(1.2 , 3.4)：" + Math.min(1.2 , 3.4));
// 返回第一个参数和第二个参数之间与第一个参数相邻的浮点数。
System.out.println("Math.nextAfter(1.2, 1.0)："
                   + Math.nextAfter(1.2, 1.0));
// 返回比目标数略大的浮点数
System.out.println("Math.nextUp(1.2 )：" + Math.nextUp(1.2 ));
// 返回一个伪随机数，该值大于等于 0.0 且小于 1.0。
System.out.println("Math.random()：" + Math.random());
```



## 3.5、Java7的ThreadLocalRandom与Random

`Random`类：生成一个伪随机数

- 两个构造器：
  - 使用默认的种子
  - 程序员显示传入一个long类型整数种子
  
  

`ThreadLocalRandom`类：与Random类相似

- current：静态方法。获取ThreadLocalRandom对象
- nextXxx：获取伪随机数



## 3.6、BigDecimal类

Java的double类型会发生精度丢失

> 【危险】
>
> 1. 不建议使用构造器BigDecimal（double）获取，因为double不精确，只能为一个近似值
> 2. ==建议使用构造器BigDecimal（String）获取，可以获取较为精确的值，推荐使用==



方法

```Java
System.out.println("0.05 + 0.01 = " + f3.add(f2));
System.out.println("0.05 - 0.01 = " + f3.subtract(f2));
System.out.println("0.05 * 0.01 = " + f3.multiply(f2));
System.out.println("0.05 / 0.01 = " + f3.divide(f2));
```





# 4、日期、时间类

## 4.1、Date类

`Date`类处理日期、时间。jdk1.0就有，构造器和方法大多过时，==不推荐使用==

建议构造器：
1. `Date（）`：生成一个代表当前时间类型的Date对象，构造器底层调用`System.currentTimeMillis（）`获取long整数作为日期参数
2. `Date（long）`：根据指定long生成Date对象

建议方法：
1. `after（Date）`：该日期是否在指定Date之前
2. `before（Date）`：该日期是否在指定Date之后
3. `setTime（long）`：设置Date对象的时间



## 4.2、Calendar 类

`Calendar`类：抽象类，表示日历

提供了大量访问、修改日期时间的方法：
1. `add（int，int）`：根据日期规则，为给定的日历字段增加或减去指定的时间量
2. `get（int）`：获取指定日历字段的值
3. `getActualMaximum（int）`：获取指定日历字段拥有的最大量
4. `getActualMinimum（int）`：获取指定日历字段拥有的最小量
5. `roll（int，int）`：与add一样，但int（第二个）为最大范围，不能超出
6. `set（int，int）`：将指定的日历字段设置为给定值
7. `set（int，int，int）`：设置Calendar对象的年、月、日
8. `set（int，int，int，int，int，int）`：设置Calendar对象的年、月、日、时、分、秒

## 4.3. java8新增日期、时间包

Java8开始新增一个`Java.time`包

1. `Clock`：获取指定时区的当前日期、时间
2. `Duration`：代表持续时间
3. `Instant`：代表一个具体的时刻，可以精确到纳秒
  - 通过静态方法`now（）`获取当前时间
  - 静态方法`now（Clock）`获取Clock对应的可
  - `minuxXxx（）`：在当前时刻基础上减去一段时间
  - `plusXxx（）`：在当前时刻基础上加去一段时间
4. `LocalDate`：代表不带时区的日期
5. `LocalTime`：代表不同时区的时间
6. `LocalDateTime`：代表不同时区的日期、时间
7. `YearMonth`：仅代表年月
8. `ZonedDateTime`：代表一个时区的日期、时间
9. `ZoneId`：代码一个时区
10. `DayOfWeek`：枚举，代表周日到周六的枚举值
11. `Month`：枚举，代表1月到12月的枚举



# 5、Format格式化

## 5.1、MessageFormat处理含有占位符的字符串

`MessageFormat`是抽象类`Format`的子类，

静态方法：

1. `format（String，Object）`：返回后面的多个参数值填充前面的String

   占位符：`{n}`。例如：你好，{0}！

## 5.2、NumberFormat格式化数字

`NumberFormat`与`DateFormat`都是抽象类`Format`的子类

`NumberFormat`和`DateFormat`都有两个方法：

1. `format`：将数字、日期格式化为字符串
2. `parse`：将字符串解析为数字、日期

![image-20210531101819589](https://java-picgo-img.oss-cn-beijing.aliyuncs.com/01_Java/01_java%E5%9F%BA%E7%A1%80/12_java%E5%9F%BA%E7%A1%80%E7%B1%BB%E5%BA%93/20210531102303.png)。



`NumberFormat`也是一个抽象类，可以通过如下类方法获取`NumberFormat`对象

1. getCurrencyInstance：返回默认的Locale的货币格式器
2. getIntegerInstance：返回默认的Locale的整数格式器
3. getNumberInstance：返回默认的Locale的通用数值格式器
4. getPercenInstance：返回默认的Locale的百分比格式器

```java
public static void main(String[] args){
    // 需要被格式化的数字
    double db = 1234000.567;
    // 创建四个Locale，分别代表中国、日本、德国、美国
    Locale[] locales = {Locale.CHINA, Locale.JAPAN, Locale.GERMAN,  Locale.US};
    NumberFormat[] nf = new NumberFormat[12];
    // 为上面四个Locale创建12个NumberFormat对象
    // 每个Locale分别有通用数值格式器、百分比格式器、货币格式器
    for (int i = 0 ; i < locales.length ; i++){
        nf[i * 3] = NumberFormat.getNumberInstance(locales[i]);
        nf[i * 3 + 1] = NumberFormat.getPercentInstance(locales[i]);
        nf[i * 3 + 2] = NumberFormat.getCurrencyInstance(locales[i]);
    }
    for (int i = 0 ; i < locales.length ; i++){
        String tip = i == 0 ? "----中国的格式----" : i == 1 ? "----日本的格式----" : i == 2 ? "----德国的格式----" :"----美国的格式----";
        System.out.println(tip);
        System.out.println("通用数值格式："  + nf[i * 3].format(db));
        System.out.println("百分比数值格式：" + nf[i * 3 + 1].format(db));
        System.out.println("货币数值格式："  + nf[i * 3 + 2].format(db));
    }
}
```



## 5.3、DateFormat日期时间格式化

`DateFormat`是抽象类，通过类方法获取`DateFormat`对象

1. getDateInstance：返回一个日期格式器
2. getTimeInstance：返回一个时间格式器
3. getDateTimeInstance：返回一个日期时间格式器

```java
public static void main(String[] args)
    throws ParseException {
    // 需要被格式化的时间
    Date dt = new Date();
    // 创建两个Locale，分别代表中国、美国
    Locale[] locales = {Locale.CHINA, Locale.US};
    DateFormat[] df = new DateFormat[16];
    // 为上面两个Locale创建16个DateFormat对象
    for (int i = 0; i < locales.length; i++) {
        df[i * 8] = DateFormat.getDateInstance(SHORT, locales[i]);
        df[i * 8 + 1] = DateFormat.getDateInstance(MEDIUM, locales[i]);
        df[i * 8 + 2] = DateFormat.getDateInstance(LONG, locales[i]);
        df[i * 8 + 3] = DateFormat.getDateInstance(FULL, locales[i]);
        df[i * 8 + 4] = DateFormat.getTimeInstance(SHORT, locales[i]);
        df[i * 8 + 5] = DateFormat.getTimeInstance(MEDIUM, locales[i]);
        df[i * 8 + 6] = DateFormat.getTimeInstance(LONG, locales[i]);
        df[i * 8 + 7] = DateFormat.getTimeInstance(FULL, locales[i]);
    }
    for (int i = 0; i < locales.length; i++) {
        String tip = i == 0 ? "----中国日期格式----" : "----美国日期格式----";
        System.out.println(tip);
        System.out.println("SHORT格式的日期格式：" + df[i * 8].format(dt));
        System.out.println("MEDIUM格式的日期格式：" + df[i * 8 + 1].format(dt));
        System.out.println("LONG格式的日期格式："  + df[i * 8 + 2].format(dt));
        System.out.println("FULL格式的日期格式：" + df[i * 8 + 3].format(dt));
        System.out.println("SHORT格式的时间格式："  + df[i * 8 + 4].format(dt));
        System.out.println("MEDIUM格式的时间格式："  + df[i * 8 + 5].format(dt));
        System.out.println("LONG格式的时间格式："  + df[i * 8 + 6].format(dt));
        System.out.println("FULL格式的时间格式："  + df[i * 8 + 7].format(dt));
    }
}
```



## 5.4、SimpleDateFormat格式化日期

`SimpleDateFormat`时`DateFormat`的子类

```java
public static void main(String[] args)
    throws ParseException {
    Date d = new Date();
    // 创建一个SimpleDateFormat对象
    SimpleDateFormat sdf1 = new SimpleDateFormat("Gyyyy年中第D天");
    // 将d格式化成日期，输出：公元2017年中第282天
    String dateStr = sdf1.format(d);
    System.out.println(dateStr);
    // 一个非常特殊的日期字符串
    String str = "14###3月##21";
    SimpleDateFormat sdf2 = new SimpleDateFormat("y###MMM##d");
    // 将日期字符串解析成日期，输出：Fri Mar 21 00:00:00 CST 2014
    System.out.println(sdf2.parse(str));
}
```



# 6、Java8 新增日期、时间格式器

`java.time.format`包下的`DateTimeFormatter`格式器



获取`DateTimeFormatter`方式：

1. 直接使用静态变量创建：`ISO_LOCAL_DATE、ISO_LOCAL_TIME、ISO_LOCAL_DATE_TIME`
2. 使用代表不同风格的枚举创建：`FormatStyle`枚举类中有：`FULL、LONG、MEDIUM、SHORT`
3. 根据模式字符串来创建，类似`SimpleDateFormat`



`DateTimeFormatter`格式化

1. format：将日期、时间转为字符串方法


```java
DateTimeFormatter[] formatters = new DateTimeFormatter[]{
    // 直接使用常量创建DateTimeFormatter格式器
    DateTimeFormatter.ISO_LOCAL_DATE,
    DateTimeFormatter.ISO_LOCAL_TIME,
    DateTimeFormatter.ISO_LOCAL_DATE_TIME,
    // 使用本地化的不同风格来创建DateTimeFormatter格式器
    DateTimeFormatter.ofLocalizedDateTime(FormatStyle.FULL, FormatStyle.MEDIUM),
    DateTimeFormatter.ofLocalizedDate(FormatStyle.LONG),
    // 根据模式字符串来创建DateTimeFormatter格式器
    DateTimeFormatter.ofPattern("Gyyyy%%MMM%%dd HH:mm:ss")
};
LocalDateTime date = LocalDateTime.now();
// 依次使用不同的格式器对LocalDateTime进行格式化
for (int i = 0; i < formatters.length; i++) {
    // 下面两行代码的作用相同
    System.out.println(date.format(formatters[i]));
    System.out.println(formatters[i].format(date));
}
```

2. parse：DateTimeFormatter解析字符串

```java
 // 定义一个任意格式的日期时间字符串
String str1 = "2014==04==12 01时06分09秒";
// 根据需要解析的日期、时间字符串定义解析所用的格式器
DateTimeFormatter fomatter1 = DateTimeFormatter.ofPattern("yyyy==MM==dd HH时mm分ss秒");
// 执行解析
LocalDateTime dt1 = LocalDateTime.parse(str1, fomatter1);
System.out.println(dt1); // 输出 2014-04-12T01:06:09
// ---下面代码再次解析另一个字符串---
String str2 = "2014$$$4月$$$13 20小时";
DateTimeFormatter fomatter2 = DateTimeFormatter.ofPattern("yyy$$$MMM$$$dd HH小时");
LocalDateTime dt2 = LocalDateTime.parse(str2, fomatter2);
System.out.println(dt2); // 输出 2014-04-13T20:00
```








# 1. 类和对象

## 1.1. 定义类

1. **定义类**


   ```java
    [修饰符] class 类名{
        零个到多个构造器定义;
        零个到多个成员变量;
        零个到多个方法;
    }
   ```

    - 修饰符：`public、final、abstract`
    - 成员：构造器、成员变量、方法、代码块、内部类（接口、枚举）

   > 【警告】
   >
   > 1. `static`修饰符到成员不能访问没有`static`修饰的成员

   

2. **定义成员变量**

   ```Java
   [修饰符] 类型 成员变量名 [= 默认值]
   ```

   - 修饰符：`public、protected、private`（最多一个）；`static、final`（可以同时）

   - 类型：基础类型、引用类型

     

3. **定义方法**

   ```Java
   [修饰符] 方法返回值类型 方法名 (形参列表){
       // 方法体
   }
   ```

   - 修饰符：`public、protected、private`（最多一个）；`abstract、final`（其中一个）；`static`

   >  【警告】
   >
   > 1. `static`关键字：修饰方法和成员变量。属于类，不属于单个实例。类变量、类方法

   

4. 定义构造器

   ```Java
   [修饰符] 构造器名 (形参列表){
       // 构造器执行体
   }
   ```

   - 修饰符：`public、protected、private`（最多一个）
   - 构造器名：必须与类名一致



## 1.2. this

`this`指向调用该方法的对象

`this`作为对象的默认引用两种情况

1. 构造器中引用该构造器正在初始化的对象
2. 在方法中引用调用该方法的对象

static修饰的方法中不能使用`this`引用==静态变量不能直接访问非静态成员==

可省略`this`



# 2. 方法

## 2.1. 形参个数可变的方法

`jdk1.5`定义方法时，在最后一个形参的类型后增加三点（...），则表示该形参可以接受多个参数值，多个参数值被当成数组传入

```java
public static void test(int var0, String... var1) {
    String[] var2 = var1;
    int var3 = var1.length;
}
```

>  【危险】
>
> 1. 个数可变的形参只能处于形参列表最后
> 2. 一个方法最多只能一个个数可变的形参
> 3. 可变形参实质为数组类型的形参

## 2.2. 递归方法

方法递归：一个方法体内调用它自身

## 2.3. 方法重载

方法重载：两同一不同（其他部分<返回值类型等>无关）

- 两同：同一个类下方法同名
- 一不同：形参列表不同

# 3. 成员变量与局部变量

变量类型：

1. 成员变量
   - 实例变量（不以static修饰）：实例创建到实例被销毁 ==与实例共存亡==
     - 访问方式：`实例名.实例变量`
   - 类变量（以static修饰）：整个类的生存范围（从类准备阶段到销毁） ==与类共存亡==
     - 访问方式：`类名.类变量`
2. 局部变量
   - 形参（方法签名中定义的变量）：整个方法有效
   - 方法局部变量（在方法内定义）：从定义地方开始都方法结束
   - 代码块局部变量（在代码块内定义）：从定义地方开始都代码块结束

## 3.1. 成员变量的初始化

当系统加载类或创建类的实例时，系统自动为成员变量分配内存空间，并分配内存空间后，自动为成员变量指定初始值

## 3.2. 局部变量的初始化

系统不会为其分配内存空间，等到程序对变量赋初始值时，系统才会为局部变量分配内存，并将初始值保存内存中

局部变量不属于任何类和实例，所以存放在方法的栈内存中

1. 基本类型：变量值保存在该变量对应的内存中
2. 引用变量：变量中保存地址

栈内存变量无需系统垃圾回收，方法和代码块运行结束而结束

# 4. 隐藏与封装

## 4.1. 封装

封装：状态信息隐藏在对象内部，不允许外界直接访问，通过该类提供的方法进行内部信息的操作和访问

访问控制符号（从小到大）：

 1. private：当前类访问权限
 2. default：包访问权限。同包下可以被其他类访问
 3. protected：子类访问权限。同包下可以被其他类访问 + 不同包中的子类访问
 4. public：公共访问权限

访问控制符号使用原则

1. 类属性用public，工具方法用private
2. 主要用作其他类的父类用protected
3. 外界访问用public

## 4.2. import static

Java默认为所有源文件导入`java.lang`包下的所有类

分别用于导入指定类的单个静态成员变量、方法和全部静态成员变量、方法。`jdk1.5`

```java
import static java.lang.System.*;
import static java.lang.Math.*;

public class StaticImportTest
{
   public static void main(String[] args)
   {
      // out是java.lang.System类的静态成员变量，代表标准输出
      // PI是java.lang.Math类的静态成员变量，表示π常量
      out.println(PI);
      // 直接调用Math类的sqrt静态方法
      out.println(sqrt(256));
   }
}
```

导入指定类的单个静态成员变量和方法：`import static java.lang.Math.PI|sqrt;`

# 5. 构造器

构造器：特殊方法。用于创建实例时执行初始化

> 【警告】
>
> 1. 默认提供无参构造器
> 2. 一旦自定义构造器后，默认构造器没有了



构造器重载：

1. 同一个类具有形参列表不同的多个构造器
2. 使用this可以调用另一个重载的构造器，==且只能在构造器中使用。并且必须作为构造器执行体的第一条语句==

# 6. 类的继承

## 6.1. 继承的特点

通过 `extends`关键来实现

实现继承的类为子类，被继承的类为父类、基类、超类

## 6.2. 重写父类方法

子类包含与父类同名的方法称之为方法重写（Override）、方法覆盖

两同两小一大：

1. 两同：
   - 方法名相同
   - 形参列表相同
2. 两小：
   - 子类==方法返回值类型==比父类方法返回值类型更小或相等
   - 子类方法==声明抛出的异常类==应比父类方法声明抛出异常更小或相等
3. 一大：子类==方法的访问权限==比父类方法的访问权限更大或相等

> 【警告】
>
> 1. 重写方法两个类型一致，要么都是类方法，要么都是实例方法
> 2. 重写后，子类需要访问父类方法：使用super（被重写是实例方法），父类类名（被重写的是类方法）
> 3. 父类方法具有private访问权限，则不能重写



## 6.3. super限定

super用法：用于限定该对象调用它从父类继承得到的实例变量和方法

> 【警告】
>
> 1. super不能出现在static修饰的方法中
> 2. 子类中定义了父类变量同名的变量，子类中定义的变量会隐藏父类中定义的变量 ==注意不是完全覆盖==

## 6.4. 调用父类构造器

子类调用父类构造器使用`super`

调用情况

1. 子类在构造器==第一行==使用`super`==显式调用==构造器
2. 子类构造器中既没有super调用，也没有this调用，系统会在调用子类构造器时==隐式调用==父类构造器

创建任何对象总是从该类所在继承树的最顶层类的构造器开始执行，然后依次向下执行，最后执行本地的构造器

# 7. 多态

java引用变量两种类型

1. 编译时类型：声明该变量时使用类型决定
2. 运行时类型：实际赋予该变量的对象决定

多态：编译时类型与运行时类型不一致（相同类型的变量、调用同一个方法时呈现出多种不同的行为特征，这就是多态）

## 7.1. 引用变量的强制类型转换

问题：引用变量只能调用编译时类型的方法，不能调用运行时类型方法

解决：强制类型转换

```Java
(type类型) variable变量
```

注意事项：

1. 基本类型之间的转换只能在数值类型之间转换
2. 引用类型之间的转换只能在具有继承关系的两个类型之间转换（否则`ClassCastException`异常）

## 7.2. instanceof

`instanceof` 运算符判断是否成功转换

`instanceof` 运算符应用

1. 前一个操作数为：引用类型变量

2. 后一个操作数为：类（或接口）

   ```java
   Object hello = "Hello";
   // String与Object类存在继承关系，可以进行instanceof运算。返回true。
   System.out.println("字符串是否是Object类的实例：" + (hello instanceof Object));
   ```


# 8. 继承与组合

## 8.1. 继承

继承是实现类复用的重要手段

坏处：破坏封装

设计父类遵循原则：

1. 尽量隐藏父类的内部数据
2. 不要让子类随意访问、修改父类方法
3. 尽量不要再父类构造器中调用将要被子类重写的方法

## 8.2. 最终类

最终类：使用 `final`修饰。jdk中有：`java.lang.String` 类 和 `java.lang.System` 类 

何时生成子类

1. 子类需要额外增加属性，而不仅仅是属性值的改变
2. 子类需要添加自己独有的方法



利用**组合**实现复用

1. 将一个旧类作为成员变量放置在新类中
2. 在新类中使用 `private`修饰被组合的旧类对象 



二者**区别**

1. 继承要表达的 `是（is-a）`的关系
2. 组合要表达的 `有（has-a）`的关系



# 9. 初始化块

初始化块是java类中可出现的==第四种成员==（前面依次有：成员变量、方法、构造器）

一个类中可以定义==多个==初始化块

相同类型初始化块的顺序：前面定义的先执行，后面定义的后执行

初始化块修饰符只有 `static`，使用`static`修饰的初始化块称为静态代码块

执行顺序：系统在==类初始化阶段执行==静态初始化块，而不是创建实例时执行。而且系统会先执行顶级类（`java.lang.Object`）的静态代码块（如果有），然后执行父类的静态代码块，最后才执行该类的静态代码块。

作用：通常用于对类变量进行初始化处理

初始化块中代码可以包含可执行的语句：

1. 定义局部变量
2. 调用其他对象的方法
3. 使用分支和循环语句

系统会==最先执行代码块==。当java创建一个对象时，先为所有实例变量分配内存，接着开始对这些实例变量执行初始化（初始化顺序：先执行初始化块代码或声明实例变量时指定的初始值）



初始化块和构造器

1. 初始化块可以看为构造器的补充，==初始化块总是在构造器之前执行==
2. 不同之处：初始化块是一段固定执行的代码，不能接受任何参数

当JVM第一次主动使用某个类时，系统会在类准备阶段为该类所有静态成员变量分配内存；在初始化阶段则负责初始化这些静态成员变量，初始化静态成员变量就是执行类初始化或者声明类成员变量时指定的初始值

# 10. 包装类

## 10.1. 类型

| 基本数据类型 | 包装类    |
| ------------ | --------- |
| byte         | Byte      |
| short        | Short     |
| int          | Integer   |
| long         | Long      |
| char         | Character |
| float        | Float     |
| double       | Double    |
| boolean      | Boolean   |

## 10.2. 转换

在`JDK 1.5`以前，把基本数据类型变量变成包装类实例需要通过对应包装类的`valueOf()`静态方法来实现

![image-20210524175625121](https://java-picgo-img.oss-cn-beijing.aliyuncs.com/01_Java/01_java%E5%9F%BA%E7%A1%80/11_Java%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86/20210524175643.png)



当`JDK 1.5`之后提供了自动装箱和自动拆箱功能后，大大简化了基本类型变量和包装类对象之间的转换过程

1. ==自动装箱==：可以把一个基本型变量直接赋值给对应的包装类变量
2. ==自动拆箱==：可以直接将包装类对象直接赋予一个对应的基本类型变量



除此之外，包装类还可实现==基本类型变量和字符串==之间的转换。把字符串类型的值转换为基本类型的值有两种方式。

1. 利用包装类提供的`parseXxx(String s)`静态方法（除`Character`之外的所有包装类都提供了该方法。
2.  利用包装类提供的`valueOf(String s)`静态方法。

![image-20210524180000618](https://java-picgo-img.oss-cn-beijing.aliyuncs.com/01_Java/01_java%E5%9F%BA%E7%A1%80/11_Java%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86/20210524180002.png)

> 【推荐】
>
> 1. -128～127之间的同一个整数自动装箱成`Integer`实例时，永远都是引用cache数组的同一个数组元素，所以它们全部相等；但每次把一个不在-128～127范围内的整数自动装箱成Integer实例时，系统总是重新创建一个Integer实例，则不相等。

## 10.3. 新增方法

### 10.3.1. jdk7新增

`compare(xxx val1, xxx val2)` 方法来比较两个基本类型值的大小

### 10.3.2. jdk8新增

1. `Integer` 和 `Long` 新增
   -  `static String toUnsignedString(int/long i)`：该方法将指定int或long型整数转换为无符号整数对应的字符串。
   - `static String toUnsignedString(int i/long,int radix)`：该方法将指定int或long型整数转换为指定进制的无符号整数对应的字符串。
   - `static xxx parseUnsignedXxx(String s)`：该方法将指定字符串解析成无符号整数。当调用类为Integer时，xxx代表int；当调用类是Long时，xxx代表long。
   - `static xxx parseUnsignedXxx(String s, int radix)`：该方法将指定字符串按指定进制解析成无符号整数。当调用类为Integer时，xxx代表int；当调用类是Long时，xxx代表long。
   - `static int compareUnsigned(xxx x, xxx y)`：该方法将x、y两个整数转换为无符号整数后比较大小。当调用类为Integer时，xxx代表int；当调用类是Long时，xxx代表long。
   - `static long divideUnsigned(long dividend, long divisor)`：该方法将x、y两个整数转换为无符号整数后计算它们相除的商。当调用类为Integer时，xxx代表int；当调用类是Long时，xxx代表long。
   - `static long remainderUnsigned(long dividend, long divisor)`：该方法将x、y两个整数转换为无符号整数后计算它们相除的余数。当调用类为Integer时，xxx代表int；当调用类是Long时，xxx代表long。
2. `Byte` 和 `Short` 新增
   - `toUnsignedInt（xxx x）`：指定byte或short类型的变量
   - `toUnsignedLong（yyy x）`：值转换成无符号的int或long值

# 11. 处理对象

## 11.1. toString方法

通常用于输出该对象的 ==自我描述==信息

`Object`类提供的`toString()`方法总是返回该对象实现类的“==类名+@+hashCode==”值

## 11.2. == 和 equals方法

`Object`类提供的`equals()`方法没有太大的实际意义，如果希望采用自定义的相等标准，则可采用重写`equals`方法来实现

> 【警告】
>
> 1. 基本类型变量，且都是数值类型（不一定要求数据类型严格相同），则只要两个变量的值相等，就将返回`true`
> 2. 引用类型变量，只有它们指向同一个对象时，`==`判断才会返回true。不可用于比较类型上没有父子关系的两个对象



重写`equals()`方法应该满足下列条件。

- 自反性：对任意x, x.equals(x)一定返回true。
- 对称性：对任意x和y, 如果y.equals(x)返回true，则x.equals(y)也返回true。
- 传递性：对任意x, y, z, 如果x.equals(y)返回ture，y.equals(z)返回true，则x.equals(z)一定返回true。
- 一致性：对任意x和y, 如果对象中用于等价比较的信息没有改变, 那么无论调用x.equals(y)多少次，返回的结果应该保持一致，要么一直是true，要么一直是false。
- 对任何不是null的x, x.equals(null)一定返回false。

# 12. 类成员

`static`修饰的成员就是类成员。类成员属于整个类，而不属于单个对象

对`static`关键字而言，有一条非常重要的**规则**：类成员（包括成员变量、方法、初始化块、内部类和内部枚举）不能访问实例成员（包括成员变量、方法、初始化块、内部类和内部枚举）

**生命周期**：当系统第一次准备使用该类，系统会为该类分配内存空间，类变量开始生效，直到该类被卸载，该类的类变量所占内存才会被系统的垃圾回收机制回收。 ==类变量生命周期等同于该类的生命周期==

**类变量**：通过类名来调用。若通过实例来访问类变量，实际系统底层转换为该类来访问类变量

**类方法**：使用类类来调用类方法。若通过实例来访问类方法，依旧如类变量一样，即使实例没有初始化，也可以直接访问

## 12.1. 单例类（Singleton）

单例类：

1. 一个类始终只创建一个实例
2. 类的构造器使用`private`修饰
3. 必须缓存已经创建的对象，否则该类无法知道是否曾经创建过对象，也就无法保证只创建一个对象

# 13. final

`final` 关键字可用于修饰类、变量和方法

final关键字表示一旦获得了初始值，则不可被改变

## 13.1. final成员变量

final成员变量：==必须由程序员显式指定初始化==

- 类变量：必须在==静态初始化块==中指定初始值或声明该==类变量时指定初始值==
- 实例变量：必须在==非静态初始化块==、==声明该实例变量==或==构造器==中指定初始值

## 13.2. final局部变量

final局部变量：定义时可以不用初始化，但是后面代码必须对其进行初始化，且只能一次，不能重复赋值

final修饰基本类型和引用类型的区别

- final修饰的基本类型 ==不能重写赋值==
- final修饰的引用类型，引用类型保存的为地址引用，所以 ==只要引用地址不变，对象内容完全可以改变==



**宏变量**。当定义`final`变量时就为该变量指定了初始值，而且该初始值可以在编译时就确定下来，那么这个`final`变量本质上就是一个“宏变量”，编译器会把程序中所有用到该变量的地方直接替换成该变量的值。



## 13.3. final方法

final修饰的方法不可被重写

Object类中有一个final方法：`getClass（）`

## 13.4. final类

final修饰的类不可以有子类

`java.lang.Math`类就是一个final类

## 13.5. 不可变类

不可变类：创建对象实例之后，该对象实例的内部的实例变量时==不可改变==的

java提供的==8个包装类==和`java.lang.String` 类都是不可变类



自定义不可变类规则

1. 使用`private` 和 `final`修饰该类的成员变量
2. 提供带参数构造器，用于根据传入参数来初始化成员变量
3. 仅提供`get` 方法
4. 如有必要，需要重写`hashCode` 和 `equals` 方法

# 14. 抽象类

抽象方法和抽象类规则：

1. 抽象类必须使用`abstract`修饰。抽象方法也必须使用`abstract`修饰，且不能有方法体

2. 抽象类不能实例化，即使抽象类中没有抽象方法，也不能实例化

3. 抽象类可以包含：成员变量、方法（普通方法和抽象方法）、构造器（不能用于创建实例，主要用于被其子类调用）、初始化块、内部类（接口、枚举）

4. ==含有抽象方法的类只能是抽象类==

   

> 【危险】
>
> 1. `final 和 abstract` 不能同时出现
> 2. `static 和 abstract` 不能同时出现，但是可以同时修饰内部类
> 3. `private`和`abstract`不能同时修饰方法



抽象类的作用：抽象类体现了模板模式的设计

# 15. 接口

## 15.1. 接口定义

`interface` 关键字



```Java
[修饰符] interface 接口名 extends 父接口1, 父接口2 ... {
    零个到多个常量定义;
    零个到多个抽象方法定义;
    零个到多个内部类、接口、枚举定义;
    零个到多个私有方法、默认方法或类方法;
}
```

**接口里的内容**：

1. 成员变量：==静态变量==
   - 系统自动添加 `public static final`

2. 方法：==抽象方法、类方法、默认方法、私有方法==
   - 抽象方法：自动添加 `public abstract`
   - 类方法：必须使用`static`修饰，不能使用`default`

3. 内部类：==包括内部接口、枚举==



**定义接口语法**：

1. 修饰符可以是public或者省略。默认采用==包权限访问控制符==
2. 接口名与类名采用相同的命名规则
3. 一个接口可以有多个父类，但接口只能继承接口，不能继承类

## 15.2. 接口中的特殊方法

`jdk 9`允许加入 ==私有方法==

- 作用：工具方法，为接口中的默认方法或者类方法提供支持

- 特点：拥有方法体、不能被`default`修饰。可以使用`static`

- 既可以是类方法，也可以是实例方法

  ```java
  // 定义私有方法
  private void foo()
  {
      System.out.println("foo私有方法");
  }
  ```

  

`jdk 8`允许加入 ==默认方法==

- 特点：必须使用 `default`修饰，不能使用 `static` 修饰。总是使用 `public` 修饰

  ```java
  // 在接口中定义默认方法，需要使用default修饰
  default void print(String... msgs)
  {
      for (String msg : msgs)
      {
          System.out.println(msg);
      }
  }
  ```




## 15.3. 接口的继承

接口支持多继承。一个接口继承多个父接口时，多个父接口排在`extends`关键字之后，多个父接口之间以英文逗号`（,）`隔开



## 15.4. 接口的使用

用途：

1. 定义变量
2. 调用接口中定义的常量
3. 被其他类实现

一个类可以实现多个接口。`implements`关键字

> 【警告】
>
> 一个类必须实现接口的所有抽象方法
>
> 实现：相当于实现类继承了一个彻底抽象类（除默认方法外）



实现类肯定是Object的显式或隐式子类



接口和抽象类的区别

| 接口                                                       | 抽象类                                           |
| ---------------------------------------------------------- | ------------------------------------------------ |
| 不可实例化                                                 | 不可实例化                                       |
| 包含抽象方法                                               | 包含抽象方法                                     |
| 体现规范                                                   | 体现模板                                         |
| 包含抽象方法、静态方法、默认方法和私有方法，不能有普通方法 | 可以包含普通方法                                 |
| 只能定义静态变量                                           | 既可以普通成员变量，也可以静态变量               |
| 不含构造器                                                 | 含有构造器，但不用于创建对象，而且子初始化抽象类 |
| 不含代码块                                                 | 含有代码块                                       |
| 多实现                                                     | 单继承                                           |



# 16. 内部类

`jdk 1.1`引入内部类

**作用**：

1. 提供了更好的封装
2. 可以直接访问外部类的私有数据
3. 匿名内部类适合用于创建那些仅需使用一次的类
4. 内部类比外部类多使用三个修饰符：`private、protected、static`。外部类不可以使用这三个修饰符
5. 非静态内部类不能拥有静态成员



内部类分类：

1. 非静态内部类：不使用`static`修饰
2. 静态内部类：使用`static`修饰

## 16.1. 非静态内部类

当在非静态内部类的方法内访问某个变量：

1. 先看方法中是否有该名称的局部变量；
2. 然后看内部类中是否有该名称的局部变量；
3. 再看外部类中是有该名称的局部变量；
4. 最后都不存在就报错



外部类成员变量、内部类成员变量与内部类里的方法的局部变量==同名==

1. 外部类成员变量：`类名.this.XXXX`

2. 内部类成员变量：`this.xxxx`

   ```Java
   public class Outer {
       private int outProp = 9;
       
       public void accessInnerProp() {
           System.out.println("内部类的inProp值:" + (new Outer.Inner()).inProp);
       }
       class Inner {
           public void acessOuterProp() {
               System.out.println("外部类的outProp值:" + Outer.this.outProp);
           }
       }
   }
   ```

   

非静态内部类的成员可以访问外部类的`private`成员

外部类则不能访问

## 16.2. 静态内部类

`static` 修饰的内部类

> 【警告】
>
> 1. 可以有静态成员，也可以有非静态成员
> 2. 静态内部类的实例方法不能访问外部类实例变量



## 16.3. 使用内部类

在外部类内部使用内部类：与普通的类一样

> 【危险】不要在为外部类的静态成员中使用非静态内部类



在外部类以外使用非静态内部类

1. 对访问修饰符控制（`default、protected、public`）

2. 在外部类之外定义内部类

   `OterClasss.InnerClass verName`

3. 在外部类之外创建内部类

   `OterClasss.new InnerClass()`

4. 最终使用

   ```java
   class Out
   {
   	// 定义一个内部类，不使用访问控制符，
   	// 即只有同一个包中其他类可访问该内部类
   	class In
   	{
   		public In(String msg)
   		{
   			System.out.println(msg);
   		} }
   }
   public class CreateInnerInstance
   {
   	public static void main(String[] args)
   	{
   		Out.In in = new Out().new In("测试信息");
   		/*
   		上面代码可改为如下三行代码：
   		使用OutterClass.InnerClass的形式定义内部类变量
   		Out.In in;
   		创建外部类实例，非静态内部类实例将寄存在该实例中
   		Out out = new Out();
   		通过外部类实例和new来调用内部类构造器创建非静态内部类实例
   		in = out.new In("测试信息");
   		*/
   	}
   }
   ```

   

在外部类以外使用静态内部类

- 在外部类以外创建静态内部类实例：`new OuterClass.InnerConstructor()`


> 【危险】外部类的子类不能重写父类的内部类



## 16.4. 局部内部类

局部内部类：把一个内部类放在方法中定义

特点：

1. 局部内部类不能被外部类的方法以外的地方使用

2. 局部内部类不能使用访问控制符和static修饰符

3. ==鸡助语法==

   ```java
   public class LocalInnerClass
   {
   	public static void main(String[] args)
   	{
   		// 定义局部内部类
   		class InnerBase
   		{
   			int a;
   		}
   		// 定义局部内部类的子类
   		class InnerSub extends InnerBase
   		{
   			int b;
   		}
   		// 创建局部内部类的对象
   		InnerSub is = new InnerSub();
   		is.a = 5;
   		is.b = 8;
   		System.out.println("InnerSub对象的a和b实例变量是："
   			+ is.a + "," + is.b);
   	}
   }
   ```



## 16.5. `jdk 8` 改进的匿名内部类

匿名内部类会创建一个该类实例，这个类定义立即消失，匿名内部类不能重复使用、

匿名内部类**规则**：

- 不能为抽象类
- 不能定义构造器
- 必须继承一个父类或实现一个接口（不能多实现）

- 局部内部类和匿名内部类访问的局部变量必须使用`final`修饰，`Java8` 取消，==自动添加final==

  ```java
  interface Product {
  	public double getPrice();
  	public String getName();
  }
  public class AnonymousTest {
  	public void test(Product p)
  	{
  		System.out.println("购买了一个" + p.getName()
  			+ "，花掉了" + p.getPrice());
  	}
  	public static void main(String[] args)
  	{
  		AnonymousTest ta = new AnonymousTest();
  		// 调用test()方法时，需要传入一个Product参数，
  		// 此处传入其匿名实现类的实例
  		ta.test(new Product()
  		{
  			public double getPrice()
  			{
  				return 567.8;
  			}
  			public String getName()
  			{
  				return "AGP显卡";
  			}
  		});
  	} }
  ```



# 17. Java 8 新增的Lambda表达式

Lambda表达式代码块会代替实现抽象方法的方法体，Lambda表达式就相当于一个匿名方法

组成：

1. 形参列表。形参列表运行省略形参类型。形参列表只有一个参数，可以省略圆括号；
2. 箭头（`->`）
3. 代码块。
   - 若为一条语句，可以省略花括号。
   - 若只有一个return语句，可以省略return关键字
   - 若需要返回值，且只有一个return语句，则可以省略return语句，自动返回这条语句的值

```java
public class LombdaClass {
	public static void main(String[] args) {
		LombdaClass lq = new LombdaClass();
		// 【1】、Lambda表达式的代码块只有一条语句，可以省略花括号。
		lq.eat(() -> System.out.println("苹果的味道不错！"));
		
		// 【2】、Lambda表达式的形参列表只有一个形参，省略圆括号
		lq.drive(weather -> {
			System.out.println("今天天气是：" + weather);
			System.out.println("直升机飞行平稳");
		});
		
		// Lambda表达式的代码块只有一条语句，省略花括号
		// 【3】、代码块中只有一条语句，即使该表达式需要返回值，也可以省略return关键字。
		lq.test((a, b) -> a + b);
		lq.test((a, b) -> {
			int c = a + b;
			c ++;
			return c;
		});
	}
		
	/**
	 *  调用该方法需要Eatable对象
	 */
	public void eat(Eatable e) {
		System.out.println(e);
		e.taste();
	}

	/**
	 *  调用该方法需要Flyable对象
	 */
	public void drive(Flyable f) {
		System.out.println("我正在驾驶：" + f);
		f.fly("【碧空如洗的晴日】");
	}

	/**
	 *  调用该方法需要Addable对象
	 */
	public void test(Addable add) {
		System.out.println("5与3的和为：" + add.add(5, 3));
	}
}

/**
 * 三个函数接口
 * @author shang
 *
 */
interface Eatable {
	void taste();
}
interface Flyable {
	void fly(String weather);
}
interface Addable {
	int add(int a, int b);
}
```

## 17.1. lambda表达式与函数式接口

Lambda表达式的类型，也被称为“目标类型（target type）”，Lambda表达式的目标类型必须是“函数式接口（functional interface）”

函数式接口：包含多个默认方法和类方法，但==只能声明一个抽象方法==

Runnable是Java本身提供的一个函数式接口。

`java8`为函数式接口提供了 @`FunctionalInterface`注解：检查该接口必须为函数式接口，否则报错

```java
@FunctionalInterface	// 【1】、加上注解声明
interface FkTest {
	void run();
}

public class LambdaTest {
	public static void main(String[] args)
	{
		// Runnable接口中只包含一个无参数的方法
		// Lambda表达式代表的匿名方法实现了Runnable接口中唯一的、无参数的方法
		// 因此下面的Lambda表达式创建了一个Runnable对象
		Runnable r = () -> {
			for(int i = 0 ; i < 100 ; i ++)
			{
				System.out.println();
			}
		};
//		// 下面代码报错: 不兼容的类型: Object不是函数接口
//		Object obj = () -> {
//			for(int i = 0 ; i < 100 ; i ++)
//			{
//				System.out.println();
//			}
//		};

		Object obj1 = (Runnable)() -> {
			for(int i = 0 ; i < 100 ; i ++)
			{
				System.out.println();
			}
		};

		// 同样的Lambda表达式可以被当成不同的目标类型，唯一的要求是：
		// Lambda表达式的形参列表与函数式接口中唯一的抽象方法的形参列表相同
		Object obj2 = (FkTest)() -> {
			for(int i = 0 ; i < 100 ; i ++)
			{
				System.out.println();
			}
		};
    } }
```



Lambda表达式限制

1. Lambda表达式的目标类型必须是明确的函数式接口
2. Lambda表达式只能为函数式接口创建对象



Lambda表达式的目标类型是一个明确的函数式接口，方式

1. 将Lambda表达式复制给函数式接口类型的变量
2. 将Lambda表达式作为函数式接口类型的参数传给某个方法
3. 使用函数式接口对Lambda表达式进行强制类型转换



java8在`java.util.function`包下预定义了大量函数式接口

1.  `XxxFunction`：这类接口中通常包含一个`apply()`抽象方法，该方法对参数进行处理、转换（`apply()`方法的处理逻辑由Lambda表达式来实现），然后返回一个新的值。该函数式接口通常用于对指定数据进行转换处理。
2. `XxxConsumer`：这类接口中通常包含一个`accept()`抽象方法，该方法与`XxxFunction`接口中的`apply()`方法基本相似，也负责对参数进行处理，只是该方法不会返回处理结果。
3.  `XxxxPredicate`：这类接口中通常包含一个`test()`抽象方法，该方法通常用来对参数进行某种判断（`test()`方法的判断逻辑由Lambda表达式来实现），然后返回一个`boolean`值。该接口通常用于判断参数是否满足特定条件，经常用于进行筛滤数据。
4. `XxxSupplier`：这类接口中通常包含一个`getAsXxx()`抽象方法，该方法不需要输入参数，该方法会按某种逻辑算法（`getAsXxx ()`方法的逻辑算法由Lambda表达式来实现）返回一个数据。



## 17.2. 方法引用与构造器引用

方法引用和构造器引用都需要使用两个英文冒号

| 类型         | 语法                 | 对应的Lambda表达式                     |
| ------------ | -------------------- | -------------------------------------- |
| 静态方法引用 | `类名::staticMethod` | `(args) -> 类名.staticMethod(args)`    |
| 实例方法引用 | `inst::instMethod`   | `(args) -> inst.instMethod(args)`      |
| 对象方法引用 | `类名::instMethod`   | `(inst,args) -> 类名.instMethod(args)` |
| 构建方法引用 | `类名::new`          | `(args) -> new 类名(args)`             |



引用类方法

```Java
@FunctionalInterface
interface Converter{
	Integer convert(String from);
}
public class MethodRefer
{
	public static void main(String[] args)
	{
		// 下面代码使用Lambda表达式创建Converter对象
		Converter converter1 = from -> Integer.valueOf(from);
		Integer val = converter1.convert("99");
		System.out.println(val); // 输出整数99
		// 方法引用代替Lambda表达式：引用类方法。
		// 函数式接口中被实现方法的全部参数传给该类方法作为参数。
		Converter converter1 = Integer::valueOf;
		Integer val = converter1.convert("99");
		System.out.println(val); // 输出整数99
    }}
```



引用特定对象的实例方法

```java
// 下面代码使用Lambda表达式创建Converter对象
Converter converter2 = from -> "fkit.org".indexOf(from);
// 方法引用代替Lambda表达式：引用特定对象的实例方法。
// 函数式接口中被实现方法的全部参数传给该方法作为参数。
Converter converter2 = "fkit.org"::indexOf;
Integer value = converter2.convert("it");
System.out.println(value); // 输出2
```



引用某类对象的实例方法

```Java
@FunctionalInterface
interface MyTest {
	String test(String a , int b , int c);
}

// 下面代码使用Lambda表达式创建MyTest对象
MyTest mt = (a , b , c) -> a.substring(b , c);
// 方法引用代替Lambda表达式：引用某类对象的实例方法。
// 函数式接口中被实现方法的第一个参数作为调用者，
// 后面的参数全部传给该方法作为参数。
MyTest mt = String::substring;
String str = mt.test("Java I Love you" , 2 , 9);
System.out.println(str); // 输出:va I Lo
```



引用构造器

```Java
@FunctionalInterface
interface YourTest {
	JFrame win(String title);
}

// 下面代码使用Lambda表达式创建YourTest对象
YourTest yt = (String a) -> new JFrame(a);
// 构造器引用代替Lambda表达式。
// 函数式接口中被实现方法的全部参数传给该构造器作为参数。
YourTest yt = JFrame::new;
JFrame jf = yt.win("我的窗口");
System.out.println(jf);
```



## 17.3. lambda表达式与匿名内部类的联系和区别

联系：

1. 两者都可以访问局部变量和外部类的成员变量
2. 两者都可以直接从接口中继承默认方法

区别：

| 匿名内部类                     | lambda                     |
| ------------------------------ | -------------------------- |
| 任意接口创建实例，只要实现方法 | 只能为函数式接口创建实例   |
| 可为抽象类甚至普通类创建实例   | 只能为函数式接口创建实例   |
| 允许调用接口中的默认方法       | 不允许调用接口中的默认方法 |



# 18. 枚举

枚举：一个类的对象是有限而且固定的，`java5`新增关键字 `enum`关键字

特点：

1. 特殊的类
2. 可以有成员变量，方法
3. 可以实现多个接口
4. 可以定义自己的构造器
5. 只能使用`public`访问权限

枚举与普通类的区别

1. 枚举可实现多个接口，默认继承与`java.lang.Enum`类，而非Object
2. 使用`enum`定义的非抽象枚举默认使用 `final` 修饰符
3. 枚举构造器只能使用 `private` 访问修饰符
4. 枚举所有实例在第一行显示列出，否则这个实例永远不会产生实例。实例默认加上 `public static final`

`java.lang.Enum`提供的方法

1. `int compareTo(E o)`：该方法用于与指定枚举对象比较顺序，同一个枚举实例只能与相同类型的枚举实例进行比较。如果该枚举对象位于指定枚举对象之后，则返回正整数；如果该枚举对象位于指定枚举对象之前，则返回负整数，否则返回零。
2. `String name()`：返回此枚举实例的名称，这个名称就是定义枚举类时列出的所有枚举值之一。与此方法相比，大多数程序员应该优先考虑使用`toString()`方法，因为`toString()`方法返回更加用户友好的名称。
3. `int ordinal()`：返回枚举值在枚举类中的索引值（就是枚举值在枚举声明中的位置，第一个枚举值的索引值为零）。
4. `String toString()`：返回枚举常量的名称，与`name`方法相似，但`toString()`方法更常用。
5. `public static<T extends Enum<T>> T valueOf(Class<T> enumType，String name)`：这是一个静态方法，用于返回指定枚举类中指定名称的枚举值。名称必须与在该枚举类中声明枚举值时所用的标识符完全匹配，不允许使用额外的空白字符。

## 18.1. 枚举的成员

建议：

1. 枚举类的成员变量都使用 `private final` 修饰

2. 为枚举显式定义带参数的构造器，列出枚举时必须对应地传入参数

  ```java
  public enum Gender {
      MALE("男"),	// 这里是调用【2】中的构造器创建的枚举对象
      FEMALE("女");
  	// 【1】、成员变量
      private final String name;
  	// 【2】、构造器
      private Gender(String var3) {
          this.name = var3;
      }
  	// 【3】、方法
      public String getName() {
          return this.name;
      }
  }
  ```


## 18.2. 枚举实现接口

枚举可以实现接口，与普通类一样

## 18.3. 包含抽象方法的枚举

> 【危险】枚举类里定义抽象方法时不能使用`abstract`关键字将枚举类定义成抽象类（因为系统自动会为它添加`abstract`关键字），但因为枚举类需要显式创建枚举值，而不是作为父类，所以定义每个枚举值时必须为抽象方法提供实现，否则将出现编译错误。

```Java
public enum Operation {
    // 【2】、需要实现抽象方法
    PLUS {
        public double eval(double var1, double var3) {
            return var1 + var3;
        }
    },
    MINUS {
        public double eval(double var1, double var3) {
            return var1 - var3;
        }
    },
    TIMES {
        public double eval(double var1, double var3) {
            return var1 * var3;
        }
    },
    DIVIDE {
        public double eval(double var1, double var3) {
            return var1 / var3;
        }
    };

    private Operation() {
    }
	// 【1】、定义的抽象方法
    public abstract double eval(double var1, double var3);

    public static void main(String[] var0) {
        System.out.println(PLUS.eval(3.0D, 4.0D));
        System.out.println(MINUS.eval(5.0D, 4.0D));
        System.out.println(TIMES.eval(5.0D, 4.0D));
        System.out.println(DIVIDE.eval(5.0D, 4.0D));
    }   
}
```



# 19、对象与垃圾回收

当程序创建对象、数组等引用类型实体时，在==堆内存==中为之分配一块内存区，对象保存在这块内存中，当内存不在被任何引用变量引用时，这块内存变成垃圾，等待垃圾回收机制进行回收



垃圾回收机制的特性：

1. 只负责回收堆内存中的对象，==不回收任何物理资源==（数据库链接、网络、IO）
2. 当对象永久性失去引用后，系统就会在==合适的时候==回收他所占用的内存
3. 在垃圾回收机制回收任何对象之前，先调用他的`finalize()`方法，该方法可能使该对象重新复活（让一个引用变量重新引用该对象），从而导致垃圾回收机制取消回收



## 19.1. 对象在内存中的状态：

![image-20210531085555749](https://java-picgo-img.oss-cn-beijing.aliyuncs.com/01_Java/01_java%E5%9F%BA%E7%A1%80/11_Java%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86/20210531085615.png)

**可达状态**：==有引用变量引用对象==，可以通过引用变量来调用该对象的实例

**可恢复状态**：==无引用变量引用对象==，在回收该对象之前，系统调用所有可恢复状态对象的`finalize`方法进行资源清理。

1. 如果系统在调用`finalize`方法时重新让一个引用变量引用该对象，则这个对象再次变为 ==可达状态==
2. 否则该对象进入 ==不可达状态==

**不可达状态**：==无引用变量引用对象，并且调用`finalize`方法后任然无法变为可达状态==。但对象处于不可达状态时，系统才会真正回收该对象所占用的所有资源



## 19.2. 强制垃圾回收

==只是通知系==统进行垃圾回收，但系统是否进行垃圾回收依然不确定。

强制垃圾回收方式：
1. 调用 `System` 类的 `gc()` 静态方法：`System.gc()`
2. 调用 `Runtime` 独享的 `gc()` 实例方法：`Runtime.getRuntime().gc()`

## 19.3. finalize方法

在垃圾回收机制回收某个对象占用内存之前，通常要求调用程序适当的方法来清理资源。Java提供默认机制来清理该对象的资源，这个机制就是`finalize`方法

任何Java类都可以重写`Object`类中的`finalize`方法，在该方法中清理该类占用的资源

finalize方法特点：
1. ==不会主动调用==某个对象的`finalize`方法，该方法由垃圾回收机制调用
2. finalize何时调用，是否被调用都是==不确定==的
3. 当`JVM`执行可恢复对象的`finalize`方法时，可能是该对象进入可达状态
4. 当`JVM`执行`finalize`方法时出现异常，垃圾回收机制不会报告异常，程序继续执行

## 19.4. 对象的软、弱和虚引用

`java.lang.ref` 包下提供了3个类：`SoftReference`、`PhantomReference`、`WeakReference`

Java对象的引用方式：
1. 强引用（StrongReference）：创建一个对象，并对其赋给一个引用变量。有>=1引用变量引用，处于可达状态
2. 软引用（SoftReference）：==需要通过`SoftReference`类来实现==。可能会被垃圾回收机制回收：当系统内存满足时，不会回收；内存不足时，系统可能回收。==通常用于对内存敏感的程序中==
3. 弱引用（WeakReference）：==通过`WeakReference`类实现==，与软引用相似，但引用级别更低。当系统垃圾回收机制运行时，不管系统内存是否足够，总会被回收该对象所占的内存
4. 虚引用（PhantomReference）：==通过`PhantomReference`类实现==，完全类似于没有引用。虚引用==主要用于跟踪对象被垃圾回收的状态，虚引用不能单独使用，必须和引用队列（ReferenceQueue）联合使用==

三种引用类都包含`get()`方法，用于获取被它所引用的对象

注意：使用特殊引用类，就不能保留对象的强引用；保留对象的强引用，就浪费了引用类所提供的任何好处



# 20、修饰符的使用范围

![image-20210531090207767](https://java-picgo-img.oss-cn-beijing.aliyuncs.com/01_Java/01_java%E5%9F%BA%E7%A1%80/11_Java%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86/20210531090212.png)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     

`strictfp`：FP-strict，精确浮点。

1. 在JVM进行浮点运算时，如果没有指定strictfp关键字，则浮点运算不一定令人满意
2. 在修饰类、接口、方法时，使用了strictfp，则会按照浮点规范IEEE-754执行

`native`：用于修饰一个方法，类似抽象方法，与抽象方法不同的是，native方法通常采用C语音来实现

1. 一旦Java程序中包含了native方法，则该程序将失去跨平台的功能






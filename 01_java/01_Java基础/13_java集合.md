# 1. Java集合概述

主要负责保存、盛装其他数据。又名 ==容器类==

Java集合大致可分为`Set`、`List`、`Queue`和`Map`四种体系，其中`Set`代表无序、不可重复的集合；`List`代表有序、重复的集合；而`Map`则代表具有映射关系的集合，Java 5又增加了`Queue`体系集合，代表一种队列集合实现。

Java集合类主要由两个接口派生

1. Collection

   ![image-20210531102943991](https://java-picgo-img.oss-cn-beijing.aliyuncs.com/01_Java/01_java%E5%9F%BA%E7%A1%80/13_java%E9%9B%86%E5%90%88/20210531103045.png)

2. Map

   ![image-20210531103118179](https://java-picgo-img.oss-cn-beijing.aliyuncs.com/01_Java/01_java%E5%9F%BA%E7%A1%80/13_java%E9%9B%86%E5%90%88/20210531103120.png)



# 2. Collection和Iterator接口

Collection接口是List、Set、Queue接口的父接口

Collection接口方法
1. `add（Object）`：向集合中添加一个元素
2. `addAll（Collection）`：向集合中添加一个集合
3. `clear`：清除集合所有的元素
4. `contains（Object）`：返回集合中是否包含指定元素
5. `containsAll（Collection）`：返回集合中是否包含指定集合
6. `isEmpty`：返回集合是否为空
7. `iterator`：返回一个`Itrerator`对象
8. `remove（Object）`：删除集合中指定元素
9. `removeAll（Collection）`：从集合中删除指定集合
10. `retainAll（Collection）`：从集合中删除指定集合中没有的元素
11. `size`：返回集合元素长度
12. 同`Array`：将集合转化为数组



## 2.1. 使用Lambda表达式遍历集合

1. Java 8 为`Iterable`接口新增`forEach（Consumer）`默认方法，这个是一个函数式接口，而且`Iterable`接口是Collection接口的父接口

   ```java
   // 创建一个集合
   Collection books = new HashSet();
   books.add("轻量级Java EE企业应用实战");
   books.add("疯狂Java讲义");
   books.add("疯狂Android讲义");
   // 调用forEach()方法遍历集合
   books.forEach(obj -> System.out.println("迭代集合元素：" + obj));
   ```

   

## 2.2. Iterator遍历集合元素

`Iterator`接口：主要用于遍历`Collection`集合中的元素。又名==迭代器==

Iterator中的方法：
1. `hasNext`：如果被迭代的集合没有迭代完，返回true
2. `next`：返回集合中的下一个元素
3. `remove`：删除集合中上一次next方法返回的元素
4. `forEachRemaining（Consumer）`：==Java8==新增方法，使用Lambda表达式遍历集合元素



## 2.3. Lambda表达式遍历Iterator

`forEachRemaining（Consumer）`方法

```Java
// 创建集合、添加元素的代码与前一个程序相同
Collection books = new HashSet();
books.add("轻量级Java EE企业应用实战");
books.add("疯狂Java讲义");
books.add("疯狂Android讲义");
// 获取books集合对应的迭代器
Iterator it = books.iterator();
// 使用Lambda表达式（目标类型是Comsumer）来遍历集合元素
it.forEachRemaining(obj -> System.out.println("迭代集合元素：" + obj));
```



## 2.4、foreach循环遍历集合元素

Java5 提供 foreach循环 `for (Object obj : books)`

> 【危险】foreach循环不能修改集合元素，否则引发 `ConcurrentModificationException`异常



## 2.5、Java8新增操作

Predicate 删除

Java8在`Collection`中新增了`removeIf（Predicate）`方法：将会批量删除符合`Predicate`条件的所有元素

```Java
books.removeIf(ele -> ((String)ele).length() < 10);
```



Stream、XxxxStream  流式API，代表多个支持串行和并行聚集操作的元素



# 3. Set集合

==set不允许包含重复元素==，若添加两个相同元素，则执行`add（）`方法之后，返回false



## 3.1. HashSet类

特点：
1. ==不能保证元素排列顺序==，且可能与添加时顺序不一样
2. ==不同步==，多线程访问同一个HashSet，不同步，需要代码处理
3. 集合==元素值可以为null==

HashSet保存原理：根据对象的`HashCode（）`方法得到`HashCode`，然后根据`HashCode`值决定保存位置

HashSet唯一校验原理：根据对象的equals方法比较HashCode



> 【危险】如果某个类需要存放在`HashSet` 中，在重写这个类的`equals`和`hashCode`方法时，要保证两个方法的返回值一致

重写hashCode方法规则

1. 在程序运行中，同一个对象多次调用`hashCode`方法应该返回相同值
2. 当两个对象通过`equals`方法返回`true`，那么`hashCode`也要返回一样的值
3. 对象中的`equals`方法比较标准实例变量，都用于计算`hashCode`



## 3.2. LinkedHashSet类

特点：
1. 根据hashCode决定元素储存位置
2. 根据链表维护，==有顺序==（插入顺序保存）
3. 性能略低HashSet



## 3.3. TreeSet 类

`SortedSet`接口的实现类：可以确保集合元素处于排序状态

`TreeSet`额外方法：

1. `comparator`：如果`TreeSet`采用定制排序，则该方法返回定制排序所送的`Comparator`，如果采用自然排序，则返回null

2. `first`：返回集合第一个元素

3. `last`：返回集合最后一个元素

4. `lower（Object）`：返回集合中位于指定元素之前的元素

5. `higher（Object）`：返回集合中位于指定元素之后的元素

6. `subSet（Object，Object）`：返回此范围中的元素（包含第一个，不包含第二个）

7. `headSet（Object）`：返回小于指定元素的子集合

8. `tailSet（Object）`：返回大于或等于指定元素的子集合、

   

**自然排序**：调用集合元素的`compareTo（Object）`方法来比较元素的大小关系，然后按元素升序排列

- 比较大小的标准：
  - `BigDecimal、BigInteger`以及所有数值对应包装类：按数值大小比较
  - `Character`：按字符的UNICODE值比较
  - `Boolean`：true大于false
  - `Date、Time`：后面时间比较前面时间
  
  > 【危险】如果希望`TreeSet`能正常运转，则必须添加一种类型。若添加其他类型，则抛出`ClassCastException`



**定制排序**：在创建`TreeSet`集合时，提供一个`Comparator`对象（函数式接口，使用Lambda表达式）

```java
TreeSet ts = new TreeSet((o1 , o2) -> {
    M m1 = (M)o1;
    M m2 = (M)o2;
    // 根据M对象的age属性来决定大小，age越大，M对象反而越小
    return m1.age > m2.age ? -1
        : m1.age < m2.age ? 1 : 0;
});
ts.add(new M(5));
ts.add(new M(-3));
ts.add(new M(9));
System.out.println(ts);
```



## 3.4. EnumSet类

专门为枚举所设计的集合类，所有元素必须为指定枚举类型。有序（按`Enum`类中定义顺序决定）。不允许插入null（否则跑【抛出`NullPointerException`）

EnumSet内存采用向量形式存储

EnumSet对象采用类方法创建：
1. `allOff（Class）`：创建指定枚举类型的所有枚举值的`EnumSet`集合
2. `complementOf（EnumSet）`：创建一个枚举类型与`EnumSet`一致的`EnumSet`集合，但元素是`EnumSet`中不含有
3. `copyOf（Collection）`：使用普通集合创建`EnumSet`
4. `copyOf（EnumSet）`：创建指定相同集合
5. `noneOf（Class）`：创建指定枚举类型的空`EnumSet`
6. `of（E，E）`：创建一个或多个枚举值的`EnumSet`，注意：必须为同一枚举类型
7. `range（E，E）`：创建这个范围内的所有枚举值的`EnumSet`集合



## 3.5. 各Set比较

HashSet 比 TreeSet性能好，但需要保持顺序，则使用TreeSet

HashSet的子类LinkedHashSet，性能较弱，因为需要维护链表

EnumSet时所有Set中性能最好，但它只能保存一种枚举类型

HashSet、TreeSet、EnumSet都是线程不安全，若多线程下，则需要手动维护，例如

```java
SortedSet s = Collection.sysnchronizedSortedSet(new TreeSet（。。。。）)
```



# 4. List集合

有序、可重复的集合



## 4.1. Java8改进List接口和ListIterator接口

`List`集合中新增操作集合的方法

1. `add（int，Object）`：将Object插入到List集合int处
2. `addAll（int，Collection）`：将Collection插入到List集合int处
3. `get（int）`：返回集合int出的元素
4. `indexOf（Object）`：返回Object在集合中第一次出现的位置索引
5. `lastIndexOf（Object）`：返回Object在集合中最后一次出现的位置索引
6. `remove（int）`：删除并返回int索引处的元素
7. `set（int，Object）`：将int处的元素替换为Object，并返回原对象
8. `subList（int，int）`：返回索引int（包含）到int（不包含）处的子集合
9. `replacAll（UnaryOperator）`：==jdk8新增，函数式接口==。根据UnaryOperator指定的计算规则重新设置List集合
10. `sort（Comparator）`：==jdk8新增，函数式接口==。根据Comparator对List集合进行元素排序

```Java
List books = new ArrayList();
// 向books集合中添加4个元素
books.add(new String("轻量级Java EE企业应用实战"));
books.add(new String("疯狂Java讲义"));
books.add(new String("疯狂Android讲义"));
books.add(new String("疯狂iOS讲义"));
// 使用目标类型为Comparator的Lambda表达式对List集合排序
books.sort((o1, o2)->((String)o1).length() - ((String)o2).length());
System.out.println(books);
// 使用目标类型为UnaryOperator的Lambda表达式来替换集合中所有元素
// 该Lambda表达式控制使用每个字符串的长度作为新的集合元素
books.replaceAll(ele->((String)ele).length());
System.out.println(books); // 输出[7, 8, 11, 16]
```



List有额外的`listIterator`方法：返回一个`ListIterator`对象，在Iterator基础上新增如下方法

1. `hasPrevious`：返回该迭代器管理集合是否还有上一元素
2. `previous`：返回该迭代器的上一元素
3. `add（Object）`：在指定位置插入一个元素



## 4.2. ArrayList和Vecotr

两个类都是List的实现，都是基于数组实现，都能动态的、运行再分配Object数组

两个类通过`initalCapacity`参数来设置该数组大小，程序员无需关系。默认大小为10此外两个类还提供了方法重新分配数组

1. `ensureCapacity（int）`：两个类中数组长度大小增加大于或等于int
2. `trimToSize`：调整两个类中数组大小，减少占用内存

Vector是古老集合，不推荐使用

Vector与ArrayList区别：
1. ArrayList线程不安全，性能相对较高

2. Vector线程安全，性能相对较低

   

Stack：Vector的子类，==栈（后进先出LIFO）==，具体方法

1. peek：返回栈的第一个元素，不能将对象 pop出栈
2. pop：返回栈的第一个元素，并将对象 pop出栈
3. push（Object）：将一个元素进栈

古老类，线程安全，性能较差



固定长度List
- Arrays操作数组工具类中提供 `asList（Object）`方法：将数组转化为List，这个List既不是ArrayList，也不是Vecotr，而是一个固定长度的List集合，只能遍历，不能添加和删除集合元素



# 5. Queue集合

Queue：用于模拟队列，先进先出（FIFO）。头部存放时间最长。尾部存放时间最短。队列不允许随机访问队列中的元素。

Queue中定义的方法
1. `add（Object）`：将Object加入队列尾部
2. `element`：获取队列头部元素，不删除该元素
3. `offer（Object）`：将Object加入队列尾部。在使用有容量限制队列时，比add方法好
4. `peek`：获取队列头部元素，不删除
5. `poll`：获取队列头部元素，删除
6. `remove`：获取队列头部元素，删除



Queue接口还有一个实现类：`PriorityQueue`，还有一个`Deque`接口：双端队列，从两端来添加、删除元素



## 5.1. PriorityQueue实现类

PriorityQueue时一个比较标准的队列实现类。

1. 保存顺序按队列元素的大小进行排序	

2. peek方法和poll取队列元素：不是按最先进入队列的元素，取最小的元素

PriorityQueue不允许插入NULL元素

PriorityQueue排序方式：

1. 自然排序：必须实现Comparable接口
2. 定制排序：创建时传入Comparator对象，该对象负责队列中的所有元素进行排序



## 5.2. Deque接口与ArrayDeque实现类

Deque接口：双端队列

Deque方法：
1. `addFirst（Object）`：将Object插入该队列的开头
2. `addLast（Object）`：将Object插入该队列的末尾
3. `descendingIterator`：返回该队列对应的迭代器，逆向顺序
4. `getFirst`：获取第一个元素，不删除
5. `getLast`：获取最后一个元素，不删除
6. `offerFirst（Object）`：将Object插入该队列的开头，无返回null
7. `offerLast（Object）`：将Object插入该队列的结尾，无返回null
8. `peekFirst`：获取第一个元素，不删除，无返回null
9. `peekLast`：获取最后一个元素，不删除，无返回null
10. `pollFirst`：获取第一个元素，删除，无返回null
11. `pollLast`：获取最后一个元素，删除，无返回null
12. `pop`：栈方法，相当于removeFirst
13. `push（Object）`：栈方法，相当于addFirst
14. `removeFirst`：获取并删除第一个元素
15. `removeFirstOccurrence（Object）`：删除第一次出现的Object
16. `removeLast`：获取并删除最后一个元素
17. `removeLastOccurence（Object）`：删除最后最后一次出现的Object



Deque接口的实现类：`ArrayDeque`，

1. 基于数组的双端队列
2. numElements参数：指定数组长度，默认16



LinkedList类

1. List接口实现类，根据索引来随机访问集合中的元素
2. 采用链表的形式来保存集合中的元素



## 5.3. 各种线性表的性能分析

List接口是线性表接口：ArrayList（基于数组的线性表）和LinkedList（基于链表的线性表）
1. 数组：随机访问时性能最好
2. 链表：执行插入、删除操作较好
3. 总体：ArrayList性能比LinkedList的性能要好

使用List集合建议：

1. 如果需要遍历List集合
   - 对于ArrayList和Vector，使用随访访问方法 get， 性能好一些
   - 对于LinkedList集合，采用迭代器 Iterator来遍历
   - 如果需要经常执行插入、删除作为改变包括大量数组的List集合大小

2. 考虑使用LinkedList集合
   - 使用ArrayList和Vecotr激活需要经常重新分配内部数组大小，性能较差
   - 如果多线程同时访问List集合中的元素：考虑使用Collection将集合包装为线程安全的集合



# 6. Map集合

Map用于保存具有映射关系的数组。
1. 其中，key不允许重复，相当于Set集合；value允许重复

2. 又名：字典、关联数组

   

常用方法：
1. `clear`：删除所有key-value
2. `containsKey（Object）`：是否包含Object这样的key
3. `containsValue（Object）`：是否包含Object这样的value
4. `entrySet`：返回包含key-value所组成的Set集合，元素为Map.Entry对象
5. `get（Object）`：返回key对应的value，无返回null
6. `isEmpty`：Map是否为空，空返回true
7. `keySet`：返回key组成的Set
8. `put（Object，Object）`：添加一个key-value对，若key存在，则覆盖
9. `putAll（Map）`：将新Map复制到本Map中
10. `remove（Object）`：删除并返回指定key的key-value，key不存在返回null
11. `size`：返回key-value的个数
12. `values`：返回所有value组成的Collection



Entry：Map中内部类，封装了key-value

1. getKey：返回Entry里的key
2. getValue：返回Entry里的value
3. setValue：设置新的value值



## 6.1. jdk8新增方法

`compute（Object，BiFunction）`：使用BiFunction根据原key-value计算新的value

1. 新value不为null，则替换原来value
2. 原value不为null，新value为null，则删除原key-value
3. 若都为null，则不改变任何key-value，直接返回null

`computeIfAbsent（Object，Function）`：

1. 若Object所对应的value若为null，则使用Function计算出新的value保存
2. 若不为null，则覆盖原来的value
3. 若Object没有对应的key，则创建一个新的key-value

`computeIfPresent（Object，BiFunction）`：如果Object在Map中对应的value不为null，则使用BiFunction生成新value

1. 新value不为null，则覆盖原value
2. 为null，则删除key-value

`forEach（BiConsumer）`：遍历key-value

`getOrDefault（Object，V）`：获取指定Object对应的value，如果key不存在则返回V

`merge（Object，Object，BiFunction）`：根据Object（第一个）获取value

1. 若value为null，则传入Object（第二个）为value
2. 若value不为null，则使用BiFunction生成新的value覆盖掉

`putIfAbsent（Object，Object）`：根据Object（第一个）对应的value是否null，为null 则将Object（第二个）替换掉

`replace（Object，Object）`：将指定Object（第一个）对应的value替换为Object（第二个），同put，但是Object（第一个）key不存在，则不新增

`replace（K，V，V）`：将指定K中对应的V（第一个）替换为V（第二个），替换返回true

`replaceAll（BiFunction）`：使用BiFunction计算原key-value，结果作为新的value值

```Java
Map map = new HashMap();
// 成对放入多个key-value对
map.put("疯狂Java讲义" , 109);
map.put("疯狂iOS讲义" , 99);
map.put("疯狂Ajax讲义" , 79);
// 尝试替换key为"疯狂XML讲义"的value，由于原Map中没有对应的key，
// 因此对Map没有改变，不会添加新的key-value对
map.replace("疯狂XML讲义" , 66);
System.out.println(map);
// 使用原value与参数计算出来的结果覆盖原有的value
map.merge("疯狂iOS讲义" , 10 ,  (oldVal , param) -> (Integer)oldVal + (Integer)param);
System.out.println(map); // "疯狂iOS讲义"的value增大了10
// 当key为"Java"对应的value为null（或不存在时），使用计算的结果作为新value
map.computeIfAbsent("Java" , (key)->((String)key).length());
System.out.println(map); // map中添加了 Java=4 这组key-value对
// 当key为"Java"对应的value存在时，使用计算的结果作为新value
map.computeIfPresent("Java", (key , value) -> (Integer)value * (Integer)value);
System.out.println(map); // map中 Java=4 变成 Java=16
```



## 6.2. HashMap和HashTable

Hashtable是古老Map实现类，jdk1就存在了

提供两个繁琐的方法：
1. `elements`：类似Map.values
2. `keys`：类似keySet方法

HashMap在`jdk8`有了新改进，使用HashMap存在key冲突时依然有较好性能

Hashtable和HashMap区别
1. Hashtable线程安全，多线程访问同一个Map，Hashtable较好；HashMap线程不安全，性能较前者高一些
2. HashTable不允许使用null作为key和value，否则NullPointerException；HashMap都可以为null（key只能一个null，value可以多个）

两个类新方法：`containsValue`。判断是否包含指定value
1. Hashtable判断相等依据：value与一个对象进行equals方法
2. 所以：重写自定义类时，注意equals方法和hashCode方法



## 6.3. LinkedHashMap

HashMap的子类。通过双向链表来进行key-value对的次序，该链表负责维护Map的迭代顺序，与key-value插入顺序一致

LinkedHashMap若维护插入顺序，则性能略低于HashMap的性能



## 6.4. Properties读写属性文件

Properties时Hashtable子类，可以把Map对象（key-value）和属性文件（属性名=属性值）关联起来

方法修改Properties的key、value

1. `getProperties（String）`：获取Properties指定String的属性值
2. `getProperties（String，String）`：与前面方法一致，但是，若String（第一个）不存在时，返回String（第二个）为默认值
3. `setProperties（String，String）`：Hashtable.put方法一致
4. `load（InputStream）`：从属性文件中加载key-value对，不保证顺序
5. `store（OutputStream，String）`：将指定String的key-value输出到指定属性文件中

```Java
Properties props = new Properties();
// 向Properties中增加属性
props.setProperty("username" , "yeeku");
props.setProperty("password" , "123456");
// 将Properties中的key-value对保存到a.ini文件中
props.store(new FileOutputStream("a.ini") , "comment line");   //
// 新建一个Properties对象
Properties props2 = new Properties();
// 向Properties中增加属性
props2.setProperty("gender" , "male");
// 将a.ini文件中的key-value对追加到props2中
props2.load(new FileInputStream("a.ini") );   //
System.out.println(props2);
```

 

## 6.5. SortedMap接口和TreeMap实现类

Set接口派生了SortedSet子接口，SortedSet接口有个TreeSet实现类

TreeSet是红黑树数据结构，每个key-value对即作为一个红黑树的一个节点，根据key对节点进行排序

TreeSet排序方式：
1. 自然排序：key必须实现Comparable接口，而且key必须为同一类，否则抛出ClassCastException
2. 定制排序：创建TreeMap时，传入一个Comparator对象，且key不要求实现Comparable接口

   > 【危险】TreeMap的对象的equals和hashCode方法结果必须一致

TreeMap提供的方法
1. `firstEntry`：返回最小的key-value，Map为空返回null
2. `firstKey`：返回最小的key值，Map为空返回null
3. `lastEntry`：返回最大的key-value，Map为空返回null
4. `lastKey`：返回最大的key，Map为空返回null
5. `higherEntry（Object）`：返回位于key后一位的key-value，Map为空或不存在返回null
6. `higherKey（Object）`：返回位于key后一位的key，Map为空或不存在返回null
7. `lowerEntry（Object）`：返回位于key前一位的key-value，Map为空或不存在返回null
8. `lowerKey（Object）`：返回位于key前一位的key，Map为空或不存在返回null
9. `subMap（Object，Boolean，Object，Boolean）`：返回从Object（第一个，是否包含看Boolean<第一个>）到Object（第二个，是否包含看Boolean<第二个>）的子Map
10. `subMap（Object，Object）`：返回从Object（第一个，包含）到Object（第二个，不包含）的子Map
11. `tailMap（Object）`：返回大于等于Object的子Map
12. `tailMap（Object，Boolean）`：返回大于Object（Boolean决定是否包含）的子Map
13. `headMap（Object）`：返回小于Object（不包含）的子Map
14. `headMap（Object，Boolean）`：返回小于Object（Boolean决定是否包含）的子Map



## 6.6. WeakHashMap实现类

与HashMap用法一样，区别：
1. HashMap采用强引用：HashMap对象销毁，key才会被回收
2. WeakHashMap采用弱引用：key所引用的对象没有被其他强引用变量所引用，则key的所引用的对象可能被回收

## 6.7. IdentityHashMap实现类

与HashMap相似，但是处理两个key相等比较独特：
1. IdentityHashMap：key1 == key2
2. HashMasp：equals为true，且hashCode一样





## 6.8. EnumMap实现类

EnumMap是枚举Map，所有key必须是单个枚举类的值

特征：
1. 内部以数组形式保存
2. 根据key的自然顺序（枚举定义顺序），可以根据keySet、entrySet、values等方法遍历EnumMap
3. 不允许key为null，但允许value为null。否则抛出NullPointerException

## 6.9. Map实现类性能分析

HashMap与Hashtable的实现机制一样，但Hashtable是古老、线程安全的集合，HashMap较之快一些

TreeMap比HashMap、Hashtable要慢，尤其是删除、插入

TreeMap好处：key-value总是有序



# 7. HashSet和HashMap的性能选项

1. hash表可以存储元素的位置称为 ==桶==



# 8. Collection

对List集合元素排序类方法：

1. `reverse（List）`：反转List集合顺序
2. `shuffle（List）`：对List进行随机排序
3. `sort（List）`：根据元素自然顺序对List集合按升序排序
4. `sort（List，Comparator）`：指定Comparator产生的顺序对List进行排序
5. `swap（List，int，int）`：将List的int（第一个）与int（第二个）进行交换
6. `rotate（List，int）`；int为整数，将List集合后的int个元素整体前移，为负数，则后移，不改变集合长度



查找、替换操作的类方法

1. `binarySearch（List，Object）`：通过二分搜索法查询List集合。想要此方法有效，必须保证List处于有序状态
2. `max（Collection）`：根据元素自然排序获取最大元素
3. `max（Collection，Comparator）`：根据Comparator指定顺序返回集合最大元素
4. `min（Collection）`：根据元素自然排序获取最小元素
5. `min（Collection，Comparator）`：根据Comparator指定顺序返回集合最小元素
6. `fill（List，Object）`：将指定元素Object替换指定List的所有元素
7. `frequency（Collection，Object）`：返回Object在Collection中出现的次数
8. `indexOfSubList（List，List）`：返回List（第一个）在List（第二个）中的第一次出现的位置，若没有，返回-1
9. `lastIndexOfSubList（List，List）`：返回List（第一个）在List（第二个）中的最后一次出现的位置，若没有，返回-1
10. `replaceAll（List，Object，Object）`：将Object（第一个）替换掉List中所有的Object（第二个）



同步控制：Collection中提供了多个`synchronizeXxxx`（）方法，将指定集合包装成线程同步的集合，从而解决多线程并发访问集合的线程安全问题

```Java
// 下面程序创建了四个线程安全的集合对象
Collection c = Collections.synchronizedCollection(new ArrayList());
List list = Collections.synchronizedList(new ArrayList());
Set s = Collections.synchronizedSet(new HashSet());
Map m = Collections.synchronizedMap(new HashMap());
```



设置不可变集合：Collection提供了三类方法返回不可变集合

1. `emptyXxxx`：返回一个空的、不可变的集合对象（List、SortedSet、Set、Map、SortedMap等）
2. `singletonXxx`：返回一个只包含指定对象的、不可变的集合对象（List、Map）
3. `unmodifiableXxxx`：返回指定集合对象的不可变视图（List、Set、SortedSet、Map、SortedMap等）

```Java
// 创建一个空的、不可改变的List对象
List unmodifiableList = Collections.emptyList();
// 创建一个只有一个元素，且不可改变的Set对象
Set unmodifiableSet = Collections.singleton("疯狂Java讲义");
// 创建一个普通Map对象
Map scores = new HashMap();
scores.put("语文" , 80);
scores.put("Java" , 82);
// 返回普通Map对象对应的不可变版本
Map unmodifiableMap = Collections.unmodifiableMap(scores);
// 下面任意一行代码都将引发UnsupportedOperationException异常
unmodifiableList.add("测试元素");   //①
unmodifiableSet.add("测试元素");    //②
unmodifiableMap.put("语文" , 90);   //③
```





# 9. Enumeration

Iterator迭代器古老版本，jdk1开始就有了，不推荐使用








































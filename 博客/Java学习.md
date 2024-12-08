# Java学习

### 数组初始化

数组初始化就是在内存中，为数组容器开辟空间，并将数据存入容器中的过程。

数组声明

```
double[] myList;         // 首选的方法
或
double myList[];         //  效果相同，但不是首选方法
```

#### 静态初始化

```
dataType[] arrayRefVar = new dataType[arraySize];
dataType[] arrayRefVar = {value0, value1, ..., valuek};
```

### Arrays 类

java.util.Arrays 类能方便地操作数组，它提供的所有方法都是静态的。

具有以下功能：

- 给数组赋值：通过 fill 方法。
- 对数组排序：通过 sort 方法,按升序。
- 比较数组：通过 equals 方法比较数组中元素值是否相等。
- 查找数组元素：通过 binarySearch 方法能对排序好的数组进行二分查找法操作。

具体说明请查看下表：

| 序号 | 方法和说明                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **public static int binarySearch(Object[] a, Object key)** 用二分查找算法在给定数组中搜索给定值的对象(Byte,Int,double等)。数组在调用前必须排序好的。如果查找值包含在数组中，则返回搜索键的索引；否则返回 (-(*插入点*) - 1)。 |
| 2    | **public static boolean equals(long[] a, long[] a2)** 如果两个指定的 long 型数组彼此*相等*，则返回 true。如果两个数组包含相同数量的元素，并且两个数组中的所有相应元素对都是相等的，则认为这两个数组是相等的。换句话说，如果两个数组以相同顺序包含相同的元素，则两个数组是相等的。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 |
| 3    | **public static void fill(int[] a, int val)** 将指定的 int 值分配给指定 int 型数组指定范围中的每个元素。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 |
| 4    | **public static void sort(Object[] a)** 对指定对象数组根据其元素的自然顺序进行升序排列。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 |

binarySearch(Object[], int fromIndex, int toIndex, Object key)

方法的object[]参数为要查找的数组，fromindex参数为开始索引（包括），toindex为结束索引（不包括），两个参数之间为查找的范围。key为要查找的key。

方法的返回值有几种：

1.找到的情况下：如果key在数组中，则返回搜索值的索引。

2.找不到的情况下：

 [1] 该搜索键在范围内，但不是数组元素，由1开始计数，得“ - 插入点索引值”；
 [2] 该搜索键在范围内，且是数组元素，由0开始计数，得搜索值的索引值；
 [3] 该搜索键不在范围内，且小于范围（数组）内元素，返回–(fromIndex + 1)；
 [4] 该搜索键不在范围内，且大于范围（数组）内元素，返回 –(toIndex + 1)。

```
 int a[] = new int[] {1, 3, 4, 6, 8, 9};  
        int x1 = Arrays.binarySearch(a, 1, 4, 5);  
        int x2 = Arrays.binarySearch(a, 1, 4, 4);  
        int x3 = Arrays.binarySearch(a, 1, 4, 2);  
        int x4 = Arrays.binarySearch(a, 1, 4, 10);
        
  结果为   x1=-4    x2=2    x3=-2    x4=-5
```



如果不使用循环直接输出数组的所有值，可以利用`Arrays.toString()`方法

```
import java.util.Arrays;
public class PrintArray {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};
        System.out.println(Arrays.toString(arr));
    }
}
```



Java 中的 `Arrays` 类提供了许多有用的方法来操作数组。以下是一些常见的 `Arrays` 类实用方法：

1. `toString()` - 将数组转换为字符串形式，方便打印输出。
2. `equals()` - 比较两个数组是否相等。
3. `sort()` - 对数组进行排序。
4. `binarySearch()` - 在已排序的数组中进行二分查找。
5. `fill()` - 使用指定的值填充整个数组。
6. `copyOf()` - 创建原始数组的副本。
7. `copyOfRange()` - 创建原始数组一部分的副本。
8. `asList()` - 将数组转换为固定大小的列表。
9. `setAll()` - 使用给定的生成器函数填充数组的每个元素。
10. `stream()` - 返回一个流，用于对数组进行更高级的操作。

```
fill()
用途：使用指定的值填充整个数组。
语法：public static void fill(Object[] a, Object val)
示例：Arrays.fill(arr, "填充值");

copyOf()
用途：创建原始数组的副本，可以指定新数组的大小，如果新大小大于原始数组，额外的空间将被初始化为默认值。
语法：public static <T,U> T[] copyOf(U[] original, int newLength, Class<? extends T[]> newType)
示例：int[] copy = Arrays.copyOf(originalArray, newLength, int[].class);

copyOfRange()
用途：创建原始数组一部分的副本，可以指定起始和结束索引。
语法：public static <T,U> T[] copyOfRange(U[] original, int start, int end, Class<? extends T[]> newType)
示例：int[] subArray = Arrays.copyOfRange(originalArray, startIndex, endIndex, int[].class);

asList()
用途：将数组转换为固定大小的列表，这个列表是不可修改的，即不能添加或删除元素。
语法：public static <T> List<T> asList(T... a)
示例：List<String> list = Arrays.asList("元素1", "元素2", "元素3");

setAll()
用途：使用给定的生成器函数填充数组的每个元素。生成器是一个接受整数索引并返回相应元素的函数。
语法：public static void setAll(IntFunction<T> generator, T[] array)
示例：Arrays.setAll(arr, index -> (T) (index * 2));

stream()
用途：返回一个流，用于对数组进行更高级的操作，如过滤、映射、归约等。
语法：public static <T> Stream<T> stream(T[] array)
示例：Stream<Integer> stream = Arrays.stream(array);
```



##### 两个小例题

1. **问题**

以下输出是什么 ？

```
class TestIt
{
    public static void main ( String[] args )
    {
        int[] myArray = {1, 2, 3, 4, 5};
        ChangeIt.doIt( myArray );
        for(int j=0; j<myArray.length; j++)
            System.out.print( myArray[j] + " " );
    }
}
class ChangeIt
{
    static void doIt( int[] z ) 
    {
        z = null ;
    }
}
```

结果：	 1 2 3 4 5

解析：java 基本数据类型传递参数时是值传递 ；引用类型传递参数时是引用传递 。然而数组虽然是引用传递 ，但是将引用 z = null 只是将引用z不指向任何对象 ，并不会对原先指向的对象数据进行修改 。

**2. 问题**

以下输出是什么 ？

```
class LowHighSwap
{
    static void doIt( int[] z )
    {
        int temp = z[z.length-1];
        z[z.length-1] = z[0];
        z[0] = temp;
    }
}

class TestIt
{
    public static void main( String[] args )
    {
        int[] myArray = {1, 2, 3, 4, 5};
        LowHighSwap.doIt(myArray);
        for (int i = 0; i < myArray.length; i++)
        {
            System.out.print(myArray[i] + " ");
        }
    }
}
```

结果：**5 2 3 4 1**

解析：	数组作为参数是引用传递 ，在 doIt 方法中可以修改数组的值 。

### 在 Java 中，如何跳出当前的多重嵌套循环

在Java中，要想跳出多重循环，可以在外面的循环语句前定义一个标号，然后在里层循环体的代码中使用带有标号的break 语句，即可跳出外层循环。例如：

```java
public static void main(String[] args) {
    ok:
    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 10; j++) {
            System.out.println("i=" + i + ",j=" + j);
            if (j == 5) {
                break ok;
            }
 
        }
    }
}
```

### OOP面向对象 的 特性

**抽象：**抽象是将一类对象的共同特征总结出来构造类的过程，包括数据抽象和行为抽象两方面。抽象只关注对象有哪些属性和行为，并不关注这些行为的细节是什么。

**封装：**隐藏对象的属性和实现细节，仅对外提供公共访问方式，将变化隔离，便于使用，提高复用性和安全性。

**继承：**继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承可以提高代码复用性。继承是多态的前提。

**多态：**多态指的是一个类的对象可以具有多个形态的能力。在面向对象编程中，多态通常指的是方法的重载（Overloading）和重写（Overriding）。重载允许一个类中有多个同名的方法，但参数列表不同；而重写则是在子类中重新定义父类的方法，以适应子类的需要。多态使得编写更灵活、可扩展的代码成为可能。

--------------

### 方法重载与方法重写

两者都是多态性的表现形式

方法重载：在同一类中定义多个**同名**方法，但这些方法的**参数列表**（即参数的数量、类型或顺序）不同。通过参数列表来实现区分。

方法重写：在子类中重新定义父类中的方法，使得子类可以提供自己的实现版本。

---------

### 抽象类和接口

抽象类是用来捕捉子类的通用特性的。接口是抽象方法的集合。从设计层面来说，抽象类是对类的抽象，是一种模板设计，接口是行为的抽象，是一种行为的规范。

**相同点**

```
接口和抽象类都不能实例化
都位于继承的顶端，用于被其他实现或继承
都包含抽象方法，其子类都必须覆写这些抽象方法
```

不同点：

![img](https://i-blog.csdnimg.cn/blog_migrate/37fdd7a5371f052a21fc14358340d827.png)

## HTTP以及HTTPS的学习

#### HTTP和HTTPS的区别

通信方式

- **HTTP**：使用明文传输数据，这意味着在客户端和服务器之间的所有数据都是未加密的，容易被截获和篡改。
- **HTTPS**：使用加密的方式传输数据，通过 SSL/TLS 协议对通信内容进行加密，增加了数据传输的安全性。

端口号

- **HTTP**：默认使用端口 80 进行通信。
- **HTTPS**：默认使用端口 443 进行通信。

安全性

- **HTTP**：由于数据未加密，存在被窃听的风险，不适用于涉及敏感信息的传输，如登录凭证、信用卡信息等。
- **HTTPS**：通过对称加密和非对称加密技术确保数据的安全性，防止中间人攻击、数据篡改和窃听。

认证机制

- **HTTP**：没有认证机制，无法验证服务器的身份。
- **HTTPS**：使用数字证书（通常由受信任的第三方机构签发）来验证服务器的身份，确保客户端与真正的服务器进行通信。

性能影响

- **HTTP**：由于无需加密解密过程，理论上来说 HTTP 的性能比 HTTPS 更好，尤其是在带宽受限的情况下。
- **HTTPS**：加密解密过程会增加一定的计算负担，但在现代硬件和优化算法的支持下，这种差异已经变得越来越小。同时，HTTPS 还利用了 HTTP/2 和 HTTP/3 的多路复用等特性，提高了传输效率。

标准化

- **HTTP**：是互联网上应用最为广泛的一种网络协议，用于传输超文本等信息。
- **HTTPS**：随着网络安全意识的提高，HTTPS 已经成为 Web 安全的标准配置，许多浏览器和服务商会优先推荐或强制要求使用 HTTPS。

实现成本

- **HTTP**：实现相对简单，不需要额外的成本。
- **HTTPS**：需要购买和维护 SSL/TLS 证书，可能需要额外的计算资源来处理加密解密任务。

用户信任度

- **HTTP**：网站可能会被认为是不够安全的，特别是在涉及到个人信息交换的情况下。

- **HTTPS**：网站会显得更加可信，浏览器通常会对 HTTPS 网站给予安全标识，增加用户的信任度。

  ---------

#### TCP三次握手

  TCP连接的建立需要经过三次握手，以确保双方都能正确地发送和接收数据。三次握手的过程如下：

  1. **第一次握手（SYN）**：

     - 客户端向服务器发送一个SYN（同步）段，表示请求建立连接，并随机选择一个初始序号`ISN`。客户端进入`SYN_SENT`状态。
     - 消息格式：SYN，Seq = ISN。

  2. **第二次握手（SYN+ACK）**：

     - 服务器收到客户端的SYN段后，发送一个SYN+ACK段作为应答，表示同意建立连接。服务器进入`SYN_RECEIVED`状态。
     - 消息格式：SYN，ACK，Seq = ISN+1，Ack = ISN+1。

  3. **第三次握手（ACK）**：

     - 客户端收到服务器的SYN+ACK段后，发送一个ACK段确认服务器的SYN，并进入`ESTABLISHED`状态。
     - 消息格式：ACK，Seq = ISN+1，Ack = ISN+2。

     ----
     
### TCP四次挥手

TCP连接的终止需要经过四次挥手来完成，这是因为TCP是全双工的，即数据可以在两个方向上同时传输。四次挥手的过程如下：

1. **第一次挥手（FIN）**：
   - 主动关闭方（假设是客户端）发送一个FIN（终止）段，表示希望关闭连接，并进入`FIN_WAIT_1`状态。
   - 消息格式：FIN，Seq = U。
2. **第二次挥手（ACK）**：
   - 被动关闭方（服务器）接收到FIN段后，发送一个ACK段作为确认，并进入`CLOSE_WAIT`状态。
   - 消息格式：ACK，Seq = V，Ack = U+1。
3. **第三次挥手（FIN）**：
   - 被动关闭方（服务器）完成所有数据传输后，发送一个FIN段表示同意关闭连接，并进入`LAST_ACK`状态。
   - 消息格式：FIN，ACK，Seq = W，Ack = U+1。
4. **第四次挥手（ACK）**：
   - 主动关闭方（客户端）接收到FIN段后，发送一个ACK段作为确认，并进入`TIME_WAIT`状态。一段时间后，客户端关闭连接，进入`CLOSED`状态。
   - 消息格式：ACK，Seq = U+1，Ack = W+1。


---------

-------------

## Tomcat

#### 什么是tomcat

是一个开放源码的Servlet容器，且能够作为独立的应用服务器运行

------------------

#### Tomcat的工作原理是什么？

Tomcat监听端口接收HTTP请求，解析请求并将请求转发给相应的Servlet容器处理。Servlet容器执行对应的Servlet或JSP页面，并返回响应给客户端。

--------

#### 配置与部署

WAR文件放置在`webapps`目录下，Tomcat会自动解压部署。

**`server.xml`文件里有哪些重要配置？**

- `<Server>`标签定义了Tomcat的运行环境，如端口号、工作目录等。

- `<Service>`标签包含了`Connector`和`Container`，定义了Tomcat的服务。

- `<Connector>`标签配置了Tomcat的连接器，如监听端口、协议等。

- `<Engine>`、`<Host>`、`<Context>`标签定义了Tomcat的容器层次结构。

  -----

  

-----------------

------------

## Java方向

#### `String` 类是不可继承的：

这是因为 `String` 类被声明为 `final` 类，意味着它不能有子类。

-------

#### String，Stringbuffer，StringBuilder的区别。

```
String

`String` 类表示不可变的字符序列。这意味着一旦一个 `String` 对象被创建，它的值就不能被改变。如果你尝试改变一个 `String` 对象的内容，将会创建一个新的 `String` 对象。因此，频繁地修改字符串会导致不必要的内存消耗。

特点：

- 不可变性（Immutable）：一旦创建后不能改变。
- 性能：频繁修改字符串会导致性能下降。
- 安全性：适用于多线程环境，因为不可变性保证了字符串的安全性。

StringBuffer

`StringBuffer` 类类似于 `String` 类，但是它是可变的，也就是说你可以修改它的内容而不必创建新的对象。此外，`StringBuffer` 中的方法是线程安全的，这意味着它可以在多线程环境中安全地使用。

特点：

- 可变性（Mutable）：内容可以被修改。
- 线程安全：适合多线程环境。
- 性能：由于线程安全，性能略低于 `StringBuilder`。

StringBuilder

`StringBuilder` 类与 `StringBuffer` 非常相似，也是用来表示可变的字符序列。不同之处在于 `StringBuilder` 的方法不是线程安全的，因此它在单线程环境中使用更为合适。

特点：

- 可变性（Mutable）：内容可以被修改。
- 性能：在单线程环境中性能优于 `StringBuffer`。
- 线程安全：不提供线程安全措施，因此不适合多线程环境。

选择哪个类？

- 如果你需要一个不可变的字符串，使用 `String`。
- 如果你需要在单线程环境中修改字符串，并且希望获得最佳性能，使用 `StringBuilder`。
- 如果你需要在一个多线程环境中修改字符串，并且关心线程安全，使用 `StringBuffer`。
```

|               | 可变性 | 线程安全 | 性能 |
| ------------- | ------ | -------- | ---- |
| String        | no     | max      | 不行 |
| StringBuffer  | y      | little   | 较差 |
| StringBuilder | y      | no       | 较好 |

-----

#### 数组和链表数据结构描述，各自的时间复杂度

数组：线性的数据结构，固定数据类型元素组成。内存区间相邻连续存储，通过索引访问

时间复杂度：

​	访问元素：O(1)

​	插入元素：O(n)

​	删除元素：O(n)

​	搜索元素：O(n)

链表：线性的数据结构，固定数据类型元素组成。不在内存中连续存储，彼此通过指针链接

时间复杂度：

​	访问元素：O(n)

​	插入元素：O(1)

​	删除元素：O(1)

​	搜索元素：O(n)

----

#### ArrayList和LinkedList有什么区别。

Java 中常用的列表实现，都实现了 `List` 接口

##### **ArrayList**

`ArrayList` 是基于动态数组实现的列表。它在内部维护了一个动态大小的数组来存储元素。当数组空间不足时，会自动扩容。

特点：

- **数据结构**：基于数组。
- **索引访问**：提供较快的随机访问速度，因为可以使用索引来直接访问元素。
- **插入和删除**：在列表的开头或中间插入或删除元素时较慢，因为需要移动大量元素来保持索引的连续性。但在列表尾部添加元素较快。
- **内存占用**：相比 `LinkedList`，`ArrayList` 在内存使用上可能稍微紧凑一些，因为它不需要额外的指针来维护链表关系。
- **迭代**：迭代效率较高，因为可以直接通过索引访问每个元素。

##### LinkedList

`LinkedList` 是基于双向链表实现的列表。每个元素都有指向其前后元素的引用。

特点：

- **数据结构**：基于链表。
- **索引访问**：较慢，因为需要从头节点开始遍历链表来找到指定位置的元素。
- **插入和删除**：较快，因为只需要改变前后节点的引用即可，不需要移动大量元素。
- **内存占用**：每个元素需要额外的空间来存储前后节点的引用，因此内存占用相对较大。
- **迭代**：使用迭代器遍历时效率高，因为可以方便地从当前节点移动到下一个节点。



##### ArrayList线程安全？将其变成安全的方法有哪些？

不安全，因为其存在三个问题：

- 部分值为null（我们并没有add null进去）
- 索引越界异常
- size与我们add的数量不符

三种情况都是如何产生的:

- 部分值为nul!:当线程1走到了扩容那里发现当前size是9，而数组容量是10，所以不用扩容，这时候cpu让出执行权，线程2也进来了，发现size是9，而数组容量是10，所以不用扩容，这时候线程1继续执行，将数组下标索引为9的位置set值了，还没有来得及执行size++，这时候线程2也来执行了，又把数组下标索引为9的位置set了一遍，这时候两个先后进行size++，导致下标索引10的地方就为null了。
- 索引越界异常:线程1走到扩容那里发现当前size是9，数组容量是10不用扩容，cpu让出执行权，线程2也发现不用扩容，这时候数组的容量就是10，而线程1 set完之后size++，这时候线程2再进来size就是10，数组的大小只有10，而你要设置下标索引为10的就会越界(数组的下标索引从0开始):size与我们add的数量不符:这个基本上每次都会发生，这个理解起来也很简单，因为size++本身就不是原子操作，可以分为三步:获取size的值，将size的值加1，将新的size值覆盖掉原来的，线程1和线程2拿到一样的size值加完了同时覆盖，就会导致一次没有加上，所以肯定不会与我们add的数量保持一致的;

安全方法：

使用Collections类的synchronizedList方法将ArrayList包装成线程安全的List：

使用CopyOnWriteArrayList类代替ArrayList，它是一个线程安全的List实现

使用Vector类代替ArrayList，Vector是线程安全的List实现

-----

讲讲类的实例化顺序，比如父类静态数据，构造函数，字段，子类静态数据，构造函数，字段，当new的时候，他们的执行顺序。

#### **实例化顺序**

1. **加载类**：首先，Java虚拟机（JVM）会加载父类和子类的字节码文件。
2. **静态初始化**：
   - 执行父类的静态初始化部分（静态变量和静态代码块）。
   - 执行子类的静态初始化部分（静态变量和静态代码块）。
3. **创建对象**：
   - 当使用 `new` 关键字创建对象时，首先会调用 `new` 操作符为对象分配内存空间。
4. **父类构造之前初始化**：
   - 初始化父类中的非静态字段（默认值或显式赋值）。
5. **调用父类构造函数**：
   - 如果子类构造函数中没有显式调用父类构造函数，则会隐式调用父类无参构造函数。
   - 子类构造函数中的 `super()` 或 `super(args)` 语句会调用父类的构造函数。
6. **父类构造函数内的初始化**：
   - 在父类构造函数中执行初始化代码（包括实例变量的初始化）。
7. **子类构造之前的初始化**：
   - 初始化子类中的非静态字段（默认值或显式赋值）。
8. **调用子类构造函数**：
   - 执行子类构造函数中的初始化代码（包括实例变量的初始化）。
9. **子类构造函数内的初始化**：
   - 在子类构造函数中执行初始化代码。

----

#### Java程序的启动

1. java程序编译生成class文件
2. 执行java.exe程序调用jvm.dll文件创建java虚拟机
3. 创建一个引导类加载器实例
4. 使用虚拟机JVM创建其他的类加载器
5. 获取当前类的实例
6. 加载要运行的类
7. 执行test类方法中的main()方法

------

##### 用过哪些Map类，都有什么区别，HashMap是线程安全的吗,并发下使用的Map是什么

|                       | 特点                                                 | 线程安全性                                                   | 存储数据顺序           |
| --------------------- | ---------------------------------------------------- | ------------------------------------------------------------ | ---------------------- |
| **HashMap**           | 提供了快速的存取性能。它允许使用`null`值和`null`键。 | no                                                           | 不保证元素的存储顺序   |
| **HashTable**         | 不允许`null`键和`null`值                             | 是线程安全的，但是由于其所有方法都被同步，所以在高并发情况下性能较低。 | 不保证元素的存储顺序   |
| **ConcurrentHashMap** | 允许一个`null`值，但不允许`null`键。                 | 是线程安全的，它通过分段锁来减少锁的竞争，提高了并发性能。   | 不保证元素的存储顺序。 |

-----

#### 抽象类和接口的区别，类可以继承多个类么，接口可以继承多个接口么,类可以实现多个接口么。

区别：抽象类是用来捕捉子类的通用特性的。接口是抽象方法的集合。从设计层面来说，抽象类是对类的抽象，是一种模板设计，接口是行为的抽象，是一种行为的规范。

类**不可以**继承多个类（单继承性extends ）；接口可以继承多个接口，类可以实现多个接口

----

#### 继承和聚合的区别在哪。

**继承** extends ：是一种类之间的关系，其中一个类（子类或派生类）继承另一个类（父类或基类）的属性和方法。

优点：

- 代码重用：子类可以直接使用父类的方法和属性，减少了重复代码。
- 多态性：子类对象可以当作父类对象来使用，提高了代码的灵活性。

缺点：

- 紧耦合：子类与父类之间紧密绑定，修改父类可能会影响所有继承它的子类。
- 继承层次过于复杂可能导致难以维护的代码。

**聚合**：聚合是一种类之间的关系，其中一个类（容器类）包含另一个类（组件类）的对象作为其成员。聚合表达的是“has-a”（拥有一个）关系。

优点：

- 松耦合：容器类和组件类之间没有直接的继承关系，可以独立地修改和使用。
- 灵活性：可以在运行时改变容器类中组件类的对象，增强了代码的灵活性。

缺点：

- 相比继承，聚合可能需要更多的代码来管理对象间的交互。
- 需要手动管理对象生命周期。

```java
// 组件类
class Engine {
    void start() {
        System.out.println("Engine started.");
    }
}

// 容器类
class Car {
    private Engine engine;
    
    public Car() {
        this.engine = new Engine();
    }
    
    void honk() {
        System.out.println("Honking the car.");
    }
    
    void startEngine() {
        engine.start(); // Engine started.
    }
}

// 测试类
public class AggregationTest {
    public static void main(String[] args) {
        Car car = new Car();
        car.startEngine();
        car.honk(); // Honking the car.
    }
}
```

------

#### **final**的用途。最终修饰符

final ： 最终修饰符，一旦被赋予初始值，就无法更改。对于基本类型，这意味着其值不能改变；

​	特点：

1. 禁止覆盖：`final` 修饰的方法不能被子类覆盖，`final` 修饰的类不能被继承
2. 修饰局部变量时：不可变性，局部变量如果被声明为 `final`，则必须在声明时或在构造方法中初始化，之后不能更改。
3. `final` 变量具有线程安全性

----

#### == 和equals的区别

**`==` 是一个二元操作符**：用于比较两个变量的引用是否指向同一个对象。换句话说，`==` 比较的是两个对象的内存地址是否相同。

当比较的是**基本数据**类型的时候：比较的是两边的值的大小是否相同

当比较的是**复合数据**类型的时候：比较的是他们存放在内存中的存放地址

**equals方法**:是object类的一个方法，可以被子类覆盖已提供定制行为，默认情况下其功能和**==**相同

**注意：**String是特殊的字符类型，**==**比较的是他们的地址，而**equals**比较的是内容是否相同

------

#### 饰符public、private、protected、default 在应用设计中的作用

**public**：表示声明为 `public` 的成员可以在任何地方被访问，不受包或继承的影响。通常用于公共接口、方法或构造函数，特别是那些需要对外提供服务的部分。

**private：**表示声明为 `private` 的成员只能在其所在的类内被访问。通常用于希望对类外部隐藏的数据字段，这样可以保护内部状态，防止外部直接修改，有助于实现封装原则。

**protected：**表示声明为 `protected` 的成员可以在其所在的类、同一个包中的其他类以及不同包中的子类中被访问。当一个成员需要在子类中被访问或者希望被同一个包内的其他类访问时，使用 `protected` 可以达到目的。这在实现继承时特别有用。

**default：**如果没有指定任何访问修饰符，则默认情况下，成员只能在同一包中的类中被访问。适用于那些只在特定包内使用的类或者成员，可以避免不必要的暴露给外部包，增强代码的安全性。

------

#### 深拷贝和浅拷贝区别。

**浅拷贝：**指创建一个新的对象，然后将现有对象的非引用类型的属性值复制到新对象中。如果属性是引用类型，则只是复制了一个指向原对象的引用，而不是复制对象本身。（指向原对象）

**深拷贝：**指创建一个全新的对象，并递归地复制原始对象的所有属性及其引用的对象。不仅仅是复制顶层对象的引用，而是复制整个对象树，确保新对象与原对象完全独立。（创造一个全新的复制品）

---

#### 什么是Spring的依赖注入？

依赖注入，是IOC的一个方面，是个通常的概念，它有多种解释。这概念是说你不用创建对象，而只需要描述它如何被创建。你不在代码里直接组装你的组件和服务，但是要在配置文件里描述哪些组件需要哪些服务，之后一个容器（IOC容器）负责把他们组装起来。

有哪些不同类型的IOC（依赖注入）方式？

- **构造器依赖注入：**构造器依赖注入通过容器触发一个类的构造器来实现的，该类有一系列参数，每个参数代表一个对其他类的依赖。
- **Setter方法注入：**Setter方法注入是容器通过调用无参构造器或无参static工厂 方法实例化bean之后，调用该bean的setter方法，即实现了基于setter的依赖注入。

哪种依赖注入方式你建议使用，构造器注入，还是 Setter方法注入？

你两种依赖方式都可以使用，构造器注入和Setter方法注入。最好的解决方案是用构造器参数实现强制依赖，setter方法实现可选依赖。

##### **设计模式**：**AOP和IOC**

------

-----

## 注解

作用简化配置和开发；注解本质是一个继承了Annotation的特殊接口，其具体实现类是Java运行时生成的动态代理类。通过反射获取注解时，返回的是Java运行时生成的动态代理对象。通过代理对象调用自定义注解的方法，会最终调用AnnotationInvocationHandler的invoke方法。该方法会从memberValues这个Map中索引出对应的值。而memberValues的来源是Java常量池。

### @Bean

- **作用**: 用于定义 Spring 容器管理的 Bean，通常在 `@Configuration` 类中使用。
- **实现**: Spring 容器通过解析 `@Bean` 注解的方法，将返回的对象作为一个 Bean 注册到容器中。

### 声明bean的注解

#### 什么是bean

是一种遵循特定约定的类。

- **可序列化**：JavaBean通常实现`Serializable`接口，以便能够进行序列化和反序列化。

- **无参构造函数**：JavaBean必须提供一个无参构造函数，这样可以通过反射来创建实例。

- **私有属性**：JavaBean的属性通常是私有的，以确保封装性。

- **getter和setter方法**：JavaBean通过公共的getter和setter方法来访问和修改其私有属性。

------

#### **@Component**

**作用**: 表示一个类是 Spring 管理的组件。Spring 容器会自动检测带有该注解的类，并将其作为一个 Bean。

**实现**: Spring 的 **`ComponentScan`** 机制会扫描指定包下的所有类，检测到使用 `@Component` 注解的类后，将其注册为 Spring 容器中的 Bean。

#### **@Controller**

- **作用**: 用于标识 Spring MVC 中的控制器类，处理 web 请求。
- **实现**: 继承自 `@Component`，属于 Spring MVC 框架的一部分。Spring 会将这个类映射为处理 HTTP 请求的控制器，并根据 URL 请求将其映射到具体的处理方法。

#### **@Service**

- **作用**: 标识业务逻辑层的类，通常是服务层。
- **实现**: 继承自 `@Component`，表示这是一个业务逻辑组件，Spring 通过它进行依赖注入。

#### @Repository

是Spring框架中的一个注解，用于标识一个数据访问层的组件。它的主要作用包括：

1. **标识持久化层**：`@Repository`注解用于标识一个类是数据访问对象（DAO），使其成为Spring上下文中的一个Spring Bean。
2. **异常转换**：使用`@Repository`注解的类会自动捕获数据访问层抛出的异常（例如JDBC异常），并将其转换为Spring的DataAccessException层次结构，简化了异常处理。
3. **自动扫描**：当使用Spring的组件扫描功能时，标记为`@Repository`的类会被自动注册为Spring的Bean，便于依赖注入。

#### @ResponseBody

- 注解的作用是将控制器方法的返回值转换为 JSON 或 XML 格式（通常是 JSON），然后直接返回给客户端。
- 可以标注在方法上，也可以标注在类上（如果标注在类上，表示该类中所有方法的返回值都会被当作响应体处理）。

### @RestController

- **`@Controller`**：标记一个类为 Spring MVC 的控制器，处理客户端发来的请求。
- **`@ResponseBody`**：表示控制器中的每个方法的返回值将直接作为 HTTP 响应体返回，而不需要再进行视图解析。

**@Controller**和**@RestController**两者区别：

**`@Controller`**：用于 MVC 模式，返回视图名，视图由视图解析器解析成 HTML 页面。

**`@RestController`**：返回的数据将被自动序列化为 JSON 或 XML（通常是 JSON），直接返回给客户端，主要用于构建 RESTful API。

-----

### **@Autowired**

- **作用**: 自动注入依赖。Spring 会自动将容器中与该属性类型匹配的 Bean 注入进来。
- **实现**: Spring 的 **`BeanPostProcessor`** 机制在 Bean 创建时会扫描字段、构造器或 setter 方法上标注的 `@Autowired`，并根据类型进行匹配注入。

-----

### **@Configuration**

- **作用**: 声明一个类作为配置类，类似于 XML 配置文件。通常与 `@Bean` 配合使用。
- **实现**: Spring 使用 CGLIB 代理类技术来增强带有 `@Configuration` 注解的类。代理类会确保 `@Bean` 注解的方法返回单例对象，而不是每次调用都创建新的实例。



------

### **@Data**

是 **Lombok** 提供的注解，用于简化 Java 类中常见的样板代码，如 **getter**、**setter**、**toString**、**equals**、**hashCode** 等方法的生成。这个注解是用在类上面的，添加后，Lombok 会自动生成这些方法，而无需手动编写。

注意事项

- `@Data` 自动生成的 `Setter` 方法会将对象设为可变，如果你只需要只读对象，建议用 `@Getter` 或 `@Value`（Lombok 的不可变注解）。
- 在 `equals()` 和 `hashCode()` 方法中，Lombok 默认会基于所有字段生成这两个方法，如果类中包含复杂对象或者数组，可能需要自定义这些方法来确保对象比较的合理性。

-----

### @Aspect 

声明一个切面（类上） 使用@After、@Before、@Around定义建言（advice），可直接将拦截规则（切点）作为参数。

-----

## Java计时器

设置定时器，Java中最方便、最高效的实现方式是用java.util.Timer工具类，再通过调度java.util.TimerTask任务。

**1、简介**
Timer是一种工具，线程用其安排以后在后台线程中执行的任务。可安排任务执行一次，或者定期重复执行。实际上是个线程，定时调度所拥有的TimerTasks。
TimerTask是一个[抽象类](https://so.csdn.net/so/search?q=抽象类&spm=1001.2101.3001.7020)，它的子类由 Timer 安排为一次执行或重复执行的任务。实际上就是一个拥有run方法的类，需要定时执行的代码放到run方法体内。

```java
**2、调用方法**
1
Timer timer = Timer(true);   
// 注意，javax.swing包中也有一个Timer类，如果import中用到swing包,要注意名字的冲突。   
  
TimerTask task = new TimerTask() {   
    public void run() {   
        ... //每次需要执行的代码放到这里面。   
    }   
};   
  
//以下是几种常用调度task的方法：   
  
timer.schedule(task, time);   
// time为Date类型：在指定时间执行一次。   
  
timer.schedule(task, firstTime, period);   
// firstTime为Date类型,period为long   
// 从firstTime时刻开始，每隔period毫秒执行一次。   
  
timer.schedule(task, delay)   
// delay 为long类型：从现在起过delay毫秒执行一次   
  
timer.schedule(task, delay, period)   
// delay为long,period为long：从现在起过delay毫秒以后，每隔period   
// 毫秒执行一次。
123456789101112131415161718192021222324
```

schedule()与scheduleAtFixedRate()的区别？
首先schedule(TimerTask task,Date time)与schedule(TimerTask task,long delay)都只是单次执行操作，并不存在多次调用任务的情况，所以没有提供scheduleAtFixedRate方法的调用方式。它们实现的功能都一样，那区别在哪里呢？
（1）schedule()方法更注重保持间隔时间的稳定：保障每隔period时间可调用一次。
（2）scheduleAtFixedRate()方法更注重保持执行频率的稳定：保障多次调用的频率趋近于period时间，如果某一次调用时间大于period，下一次就会尽量小于period，以保障频率接近于period。
**3、示例演示**
定制任务:

```cpp
import java.util.Timer;
import java.util.TimerTask;   
  
public class TimerTaskTest extends TimerTask{  
  
@Override  
public void run() {  
   // TODO Auto-generated method stub  
   System.out.println("执行任务……");  
}  
}
1234567891011
```

**调用java.util.Timer:**

```cpp
import java.util.Timer;  
/**
* 安排指定的任务task在指定的时间firstTime开始进行重复的固定速率period执行
* 每天中午12点都执行一次 
*/ 
  
public class Test {  
public static void main(String[] args){  
   Timer timer = new Timer();  
   Calendar calendar = Calendar.getInstance();
   calendar.set(Calendar.HOUR_OF_DAY, 12);//控制小时
   calendar.set(Calendar.MINUTE, 0);//控制分钟
   calendar.set(Calendar.SECOND, 0);//控制秒
   Date time = calendar.getTime();//执行任务时间为12:00:00
        
   Timer timer = new Timer(); 
   //每天定时12:00执行操作，延迟一天后再执行
   timer.schedule(new TimerTaskTest(), time, 1000 * 60 * 60 * 24);  
}  
}
```

-----------

---------------

## 异常

Java异常类层次结构图：

![img](https://cdn.xiaolincoding.com//picgo/1720683900898-1d0ce69d-4b5d-41a6-a5df-022e42f8f4c5.webp)

### 非运行时异常：

这类异常在编译时期就必须被捕获或者声明抛出。它们通常是外部错误，如文件不存在（FileNotFoundException）、类未找到（ClassNotFoundException）等。非运行时异常强制程序员处理这些可能出现的问题，增强了程序的健壮性。

### 运行时异常：

这类异常包括运行时异常（RuntimeException）和错误（Error）。运行时异常由程序错误导致，如空指针访问（NullPointerException）、数组越界（ArrayIndexOutOfBoundsException）等。运行时异常是不需要在编译时强制捕获或声明的。

lllegalArgument：通常在传递给方法的参数无效或不合适时抛出。这个异常用于指示方法接收到非法或不合适的参数值。

### 异常中`finally` 块的调用时刻

1. **正常执行**：如果 `try` 块中的代码没有抛出异常，`finally` 块会在 `try` 块执行完毕后立即执行。
2. **异常抛出**：如果 `try` 块中的代码抛出异常并且被 `catch` 块捕获，`finally` 块会在 `catch` 块执行完毕后立即执行。
3. **异常未被捕获**：如果 `try` 块中的代码抛出异常但没有被任何 `catch` 块捕获，`finally` 块仍然会在异常传播到调用栈的更高层之前执行。
4. **`return` 语句**：即使 `try` 或 `catch` 块中有 `return` 语句，`finally` 块也会在 `return` 语句执行之前执行。

### try{return “a”} fianlly{return “b”}这条语句返回啥

finally块中的return语句会覆盖try块中的return返回，因此，该语句将返回"b"。

----

---

## 单例模式

确保一个类只有一个实例，并提供一个全局访问点来访问这个唯一的实例。

单例模式的主要特点：

1. **唯一性**：一个类只能有一个实例。
2. **私有构造函数**：为了防止外部代码通过new操作符直接创建对象，单例类的构造函数通常是私有的。
3. **静态工厂方法**：提供一个公共的静态方法来获取该类的唯一实例。
4. **线程安全**：在多线程环境下，单例模式需要保证线程安全，避免多个线程同时创建多个实例。

### 实现方式：

- **懒汉式（Lazy Initialization）**：在第一次使用时才初始化实例。这种方式可以实现延迟加载，但是需要解决多线程下的并发问题。
- **饿汉式（Eager Initialization）**：在类加载时就完成了初始化，所以类加载比较慢，但获取对象的速度快，而且是线程安全的。
- **双重检查锁定（Double-Checked Locking）**：在Java等语言中，可以通过这种方法来优化懒汉式的性能，减少不必要的同步开销。
- **静态内部类**：利用Java的类加载机制来保证初始化实例时的线程安全，同时达到延迟加载的目的。
- **枚举**：在Java中，还可以使用枚举来实现单例模式，这种方式不仅能避免多线程同步问题，还能防止反序列化重新创建新的对象。

### 示例代码（Java）：

#### 懒汉式（非线程安全）

```
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

#### 饿汉式

```
public class Singleton {
    private static final Singleton INSTANCE = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

#### 双重检查锁定

```
public class Singleton {
    private volatile static Singleton uniqueInstance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (uniqueInstance == null) {
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```

#### 静态内部类

```
public class Singleton {
    private Singleton() {}

    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```

#### 枚举

```
public enum Singleton {
    INSTANCE;

    // 可以添加任何其他方法或字段
}
```

# Java基础八股

## 󠀲󠀲一、Java基础面试篇

### 1、数据类型

#### 引用类型

```
类：Class

接口：Interface

数组：Array

枚举：Enum
```

自动装箱：int -> Integer
自动拆箱：Integer -> int

```
// 下面代码会先自动拆箱将sum转为int，然后执行 + 操作。然后发生自动装箱操作将int转为Integer操作。
Integer sum = 0; for(int i=1000; i<5000; i++){ sum+=i; }
// 其内部变化如下：
int result = sum.intValue() + i; Integer sum = new Integer(result);
// 由于我们这里声明的sum为Integer类型，在上面的循环中会创建将近4000个无用的Integer对象，在这样庞大的循环中，会降低程序的性能并且加重了垃圾回收的工作量。
```


看起来Integer非常不好。那为什么Java还要有Integer？
因为Integer是int的包装类。Java的绝大部分方法或类都是用来处理类类型对象的，比如ArrayList只能以类作为他的存储对象(所以只能用ArrayList而不能用List)。

> 泛型中的应用： 此外，泛型中也只能使用引用类型包装类。所以包装类还是很有存在的必要的。

```
List<Integer> list = new ArrayList<>();
list.add(3);
list.add(1);
list.add(2);
Collections.sort(list);System.out.println(list);

```

#### 转换中的应用

在Java中，基本类型和引用类型不能直接转换，必须使用包装类实现。例如将 int 转为 String，需要int -> Integer -> String

#### 集合中的应用

集合也是只能存储对象而不能存储基本数据类型。因此想存int，就要存Integer。如果有一个列表，想要求所有元素的和，如果用int，需要一个循环来遍历。如果是Integer包装类，就可以直接用stream()方法计算所有元素的和。

```
List<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);
int sum = list.stream().mapToInt(Integer::intValue).sum();
```


Integer的缓存
Java的Integer类内部实现了一个静态缓存池，用于存储特定范围内的整数值对应的Integer对象。这个范围为-128~127。当通过Integer.valueOf(int)方法创建一个在这个范围内的整数对象时，会复用缓存中的现有对象，直接从内存中取出，不用重新new。

### 2、面向对象

面向对象的三大特征：

- 封装

- 继承

- 多态

  1、重载 (编译时多态)
  2、重写 (运行时多态)

  #### 多态体现在哪些方面？

1、方法重载：
	同一类中可以有多个同名方法，且有不同的参数列表。(eg, add(int a, int b) 以及add(int a, int b, int c)
2、方法重写：
	子类能够提供对父类中同名方法的具体实现。在运行时，JVM会根据对象的数据类型确定调用哪个版本的方法。(eg, 在一个动物类中，定义一个sound方法， 子类Dog可以重写该方法实现bark，而Cat可以实现meow)
3、接口的实现
	多个类可以实现同一个接口，并用接口的引用来调用这些类的方法。(eg, 多个类，如Cat Dog 都实现了 Animal 接口，当用 Animal 类型的引用来调用 makeSound 方法时，会出发对应的实现)
4、向上转型和向下转型
	向上转型：使用父类类型的引用指向子类对象。向上转型可以在运行时期采用不同的子类实现。
	向下转型：将父类引用转回子类类型。但在执行前需要确认引用实际指向的对象类型来避免ClassCastException

#### 多态解决了什么问题？

多态指子类可以替换父类，在实际的代码运行过程中，调用子类的方法实现。
多态可以提高代码的扩展性和复用性。是很多设计模式、设计原则、编程技巧的代码实现基础。如策略模式、基于接口而非实现变成、依赖倒置原则、里式替换原则、利用多态去掉冗长的 if-else 语句等。

#### 重载和重写有什么区别？

重载是在同一个类中定义多个同名不同参数方法。
重写是子类重新定义父类中的方法。

#### 抽象类和普通类的区别

- 实例化：普通类可以直接实例化对象，而抽象类不能被实例化。

- 方法实现：普通类中的方法可以有具体的实现，抽象类中的方法可以有实现也可以没有实现。
- 继承：一个类可以继承一个普通类 & 多个接口；一个类只能继承一个抽象类 & 多个接口
- 实现限制：普通类可以被其他类继承和使用，而抽象类一般用于作为基类，需要被其他类继承和扩展使用。 （因此抽象类不能被final修饰，因为final修饰符禁止类被继承或方法被重写）
- 抽象类和接口的区别

**两者的特点**

- 抽象类用于描述类的共同特性和行为，可以有成员变量、构造方法和具体方法。适用于有明显继承关系的场景。

- 接口用于定义行为规范，可以多实现，只能有常量、抽象方法、默认方法和静态方法。

**两者的区别**

- 实现方式：一个类可以实现多个接口，继承一个抽象类。所以使用接口可以间接的实现多重继承。

- 方法方式：接口只能有定义不能有方法的实现。java1.8中引入定义default方法体。抽象类可以有定义+实现。
- 访问修饰符：接口的成员变量默认为public static final，必须赋初值，不能被修改。其所有成员方法都是public abstract的。抽象类的成员变量默认为default。抽象方法由于被abstract修饰不能被private, static, synchronized, native修饰。抽象方法必须以分号结尾，不能带花括号。
- 变量：抽象类中可以包含实例变量和静态变量，接口只能包含常量（静态常量）

#### 接口里可以定义哪些方法？ 不能定义具体方法体吗？

- 抽象方法：在接口中定义的方法默认都是public abstract 修饰的。这些修饰符可以省略。

- 默认方法：用default修饰，就可以允许接口提供具体实现。实现类可以选择重写默认方法

```
public interface Animal {
	void makeSound();
	default void sleep() {
		System.out.println("Sleeping...");
	}
}
```

- 
  私有方法：私有方法是在Java 9 中引用，用于在接口中为默认方法或其他私有方法提供辅助功能。私有方法不能被实现类访问，只能在接口内部使用。

```
public interface Animal {
	void makeSound();
	default void sleep() {
		System.out.println("Sleeping...");
	}
	private void logSleep() {
		System.out.println("Logging sleep");
	}
}
```

#### 解释Java中的静态变量和静态方法

静态变量和静态方法是与类本身关联的，而不是与类的实例（对象）关联。他们在内存中只有一份，可以被类的所有实例共享。
静态是用static修饰的。

**静态变量**

- 共享性：所有该类的实例共享同一个静态变量。

- 初始化：静态变量在类被加载时初始化，只会对其分配一次
- 访问方式：可以直接通过类名方式访问。推荐。

**静态方法**

- 无实例依赖：可以直接通过类名访问，无需创建类实例。静态方法不能直接访问非静态的成员变量或方法。

- 访问静态成员：静态方法可以直接访问静态变量和静态方法。但是不能直接访问非静态成员。
- 多态性：静态方法不支持重写，可以被隐藏。

**使用场景**
静态变量：常用于在所有对象间共享的数据。如计数器，常量等。
静态方法：常用于助手方法，获取类级别的信息或者是没有依赖于实例的数据处理。

```
// 静态变量如何访问非静态变量和方法
class Car {
    // 非静态变量和方法
    String model = "Toyota";

    public void displayModel() {
        System.out.println("Model: " + model);
    }
    
    // 静态方法
    public static void showCarInfo() {
        // 不能直接访问非静态变量和方法
        // System.out.println(model); // 编译错误
        // displayModel(); // 编译错误
    
        // 通过实例访问非静态变量和方法
        Car car = new Car();  // 创建对象
        System.out.println(car.model);  // 访问非静态变量
        car.displayModel();  // 调用非静态方法
    }

}

public class Main {
    public static void main(String[] args) {
        // 调用静态方法
        Car.showCarInfo();
    }
}
```



> 有一个父类和子类，都有静态的成员变量、静态构造方法和静态方法，在我new一个子类对象的时候，加载顺序是怎么样的？

1. 父类静态成员变量、静态代码块（如果有）

2. 子类静态成员变量、静态代码块（如果有）
3. 父类构造方法（实例化对象时）
4. 子类构造方法（实例化对象时）

### **对象**

#### Java创建对象除了 new 还有什么方式？

- 通过反射创建对象。

可以使用 Class 类的 newInstance() 方法或者通过 Constructor 类来创建对象呢

```
public class MyClass {
	public MyClass() {// Constructor
	}
}
public class Main {
	public static void main(String[] args) throws Exception {
		Class<?> clazz = MyClass.class;
		MyClass obj = (MyClass) clazz.newInstance();
	}
}
```

- 
  通过反序列化创建对象：

通过将对象序列化（保存到文件或网络传输），然后再反序列化（从文件或网络传输中读取对象）的方式来创建对象，对象能被序列化和反序列化的前提是实现Serializable接口

```
import java.io.*;

public class MyClass implements Serializable {
    // Class definition
}

public class Main {
    public static void main(String[] args) throws Exception {
        // Serialize object
        MyClass obj = new MyClass();
        ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("object.ser"));
        out.writeObject(obj);
        out.close();

        // Deserialize object
        ObjectInputStream in = new ObjectInputStream(new FileInputStream("object.ser"));
        MyClass newObj = (MyClass) in.readObject();
        in.close();
    }

}
```

#### New出的对象什么时候回收？

Java的对象回收机制是由垃圾回收器根据一些算法决定的，会周期性地检测不再被引用的对象，并将其回收，释放内存。具体的算法有：

1. 引用计数法：引用计数为0就回收

2. 可达性分析算法：从根对象出发，通过对象之间的引用链遍历，如果存在一条引用链到达某个对象，则说明该对象是可达的。反之就回收这个对象

--------------

### 3、反射

反射机制是在运行状态中，对于任意一个类，都能知道这个类中的所有属性和方法，对于任意一个对象，都能调用它的方法和属性。这种动态获取的信息和动态调用对象方法的功能称为Java的反射机制。

反射具有如下特性：

1. 运行时类信息访问：反射机制允许程序在运行时获取类的完整结构信息，包括类名，包名，父类，实现的接口，构造函数，方法和字段等。

2. 动态对象创建：可以使用反射API动态地创建对象实例，即使在编译时不知道具体的类名。这是通过Class类的newInstance()方法或Constructor对象的newInstance()方法实现的。
3. 动态方法调用：可以在运行时动态地调用对象的方法，包括私有方法。这通过Method类的invoke()方法实现。允许你传入对象实例和参数值来执行方法。
4. 访问和修改字段值：私有字段也可以访问和修改。这是通过Field类的get()和set()方法完成的。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/59fd984d58db42ad9673b4a52854da48.png)

#### 平时写代码和框架中反射有哪些应用场景？

- 加载数据库

项目可能有的用mysql，有的用oracle，需要动态地根据实际情况加载驱动类。这个时候在使用JDBC连接数据库时，使用Class.forName()通过反射加载数据库的驱动程序，如果是mysql则传入mysql的驱动类，而如果是oracle则传入的参数就变成另一个

```
// DriverManager.registerDriver(new com.mysql.cj.jdbc.Driver());
Class.forName("com.mysql.cj.jdbc.Driver");
```

- 
  配置文件加载

​	Spring框架的IOC控制反转（动态加载管理bean），Spring通过配置文件配置各种各样的bean，需	要什么就配什么，spring容器会根据你的需求动态加载。
Spring通过XML配置模式装载Bean的过程：

1. 将程序中所有的XML或properties配置文件加载入内存

2. Java类里面解析xml或者properties里面的内容，得到对应实体类的字节码字符串以及相关的属性信息
3. 使用反射机制，根据这个字符串获取某个类的Class实例
4. 动态配置实例的属性。

eg：
配置文件：

```
className=com.example.reflectdemo.TestInvoke
methodName=printlnState
```


实体类：

```
public class TestInvoke {
	private void printlnState() {
		System.out.println("I am fine");
	}
}
```


解析配置文件内容：

```
public static String getName(String key) throws IOException {
    // 创建 Properties 对象
    Properties properties = new Properties();
    

    // 读取 properties 文件
    FileInputStream in = new FileInputStream("D:\\language-specification\\src\\main\\resources\\application.properties");
    properties.load(in);
    in.close();
    
    // 根据 key 获取属性值
    return properties.getProperty(key);

}
```


利用反射获取实体类的Class实例，创建实体类的实例对象，调用方法

```
public static void main(String[] args) throws NoSuchMethodException, InvocationTargetException, IllegalAccessException, IOException, ClassNotFoundException, InstantiationException {
    // 使用反射机制获取 Class 对象
    Class<?> c = Class.forName(getName("className"));
    System.out.println(c.getSimpleName());

    // 获取类中的方法
    Method method = c.getDeclaredMethod(getName("methodName"));
    
    // 绕过安全检查
    method.setAccessible(true);
    
    // 创建实例对象
    TestInvoke testInvoke = (TestInvoke) c.newInstance();
    
    // 调用方法
    method.invoke(testInvoke);

}
```


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/845ede82be5e4f9ca7309af5982c3744.png)

----------

### 4、注解

注解本质是一个继承了Annotation的特殊接口，其具体实现类是Java运行时生成的动态代理类。

我们通过反射获取注解时，返回的是Java运行时生成的动态代理对象。通过代理对象调用自定义注解的方法，会最终调用AnnotationInvocationHandler的invoke方法。该方法会从memberValues这个Map中索引出对应的值。而memberValues的来源是Java常量池。
（当你调用一个注解的方法时，实际上是通过动态代理对象调用AnnotationInvocationHandler的invoke方法，这个方法不是直接执行单吗返回结果，而是从一个内部的Map，即memberValues中查找与注解方法对应的值。）

### 5、异常

Java的异常类层次结构：![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d068e1f1014a4673b850be20545829f0.png)

Java的异常主要基于Throwable类及其子类。

1. Error(错误)：表示运行时环境的错误。错误是指程序无法处理的严重问题，如系统崩溃，虚拟机错误，动态连接失败等。通常，程序不应该捕获这类错误，如OutOfMemoryError, StackOverflowError等。

2. Exception（异常）：表示程序本身可以处理的异常条件，分为两大类
   1. 非运行时异常：这类异常中编译时就必须被捕获或声明抛出。他们通常是外部错误，如文件不存在（FileNotFoundException）、类未找到（ClassNotFoundException）等。
   2. 运行时异常：这类包括运行时异常（RuntimeException）和错误（Error）。运行时异常由程序错误导致，如空指针访问（NullPointerException）、数组越界（ArrayIndexOutOfBoundsException）等。运行时异常是不需要在编译时强制捕获或声明的。

#### Java的异常处理有哪些？

- try-catch语句块：用于捕获并处理可能抛出的异常。try块中包含可能抛出的异常的代码，catch块用于捕获并处理特定类型的异常。可以有多个catch块来处理不同类型的异常

```
try {
    // 可能抛出异常的代码块
} catch (ExceptionType1 e1) {
    // 处理异常类型1的逻辑
} catch (ExceptionType2 e2) {
    // 处理异常类型2的逻辑
} catch (ExceptionType3 e3) {
    // 处理异常类型3的逻辑
} finally {
    // 可选的finally块，用于无论发生何种异常都需要执行的代码
}
```

- 
  throw语句：用于手动抛出异常。可以根据需要在代码中使用throw语句主动抛出特定类型的异常。

```
throw new ExceptionType("Exception message");
```

- throws关键字：用于在方法生命中声明可能抛出的异常类型。如果一个方法可能抛出异常，但不想再方法内部处理，可以使用throws关键字将异常传递给调用者来处理

```
public void methodName() throws ExceptionType {
	// 方法体
}
```

- 
  finally块：用于定义无论是否发生异常都会执行的代码块。通常用于释放资源，确保资源的正确关闭。

```
try {
	// 可能抛出异常的代码
} catch (ExceptionType e) {
	// 处理异常的逻辑
} finally {
	// 无论是否发生异常都会执行的代码
}
```

#### 抛出异常什么时候不用throws？

如果异常是未检查异常或者在方法内部被捕获和处理了，那就不需要用throws。

- Unchecked Exceptions: 未检查异常是继承RuntimeException类和Error类的异常，编译器不强制要求异常处理，如NullPointerException, ArrayIndexOutOfBoundsException等。

- 捕获和处理异常：如果在方法内部捕获了可能出现的异常并在方法内部处理他们，就不用在方法签名处throws

#### try{return "a"} finally {return "b"} 这条语句返回什么？

finally块中的return语句会覆盖try中的return返回，因此将返回 b

-------------------

### 6、object

#### == 和 equal有什么区别？

- 对于字符串变量，使用 == 和 equals比较字符串，其比较方法不同。 ==比较两个变量本身的值，即两个对象在内存中的首地址，equals比较字符串中包含的内容是否相同

- 对于非字符串变量，== 和 equals 的作用一样，都是比较首地址，即两个引用变量是否指向同一个对象。

总结：

- ==：比较的是字符串内存地址是否相同，属于地址比较
- equals()：比较的是两个字符串的内容，属于内容比较。

#### StringBuffer和StringBuild的区别是什么？

- StringBuffer：是线程安全的，适用于多线程

- StringBuilder：线程不安全的，适用于单线程

速度：
从快到慢依次为：StringBuilder > StringBuffer > String

### 7、Java 1.8 新特性

#### **Java中的stream的API简单介绍**

> 案例1： 过滤并收集满足条件的元素

```
List<String> originalList = Arrays.asList("apple", "fig", "banana", "kiwi);
List<String> filteredList = originalList.stream()
										.filter(s -> s.length() > 3)
										.collect(Collectors.toList());
/*
	这里可以直接在原始列表上调用 .stream() 方法创建一个流，使用 .filter() 中间操作筛选出长度大于3的字符串，最后使用 .collect(Collectors.toList()) 终端操作将结果收集到一个新的列表中。
*/
```



> 案例2：计算列表中所有数字的和

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

int sum = numbers.stream()
				 .mapToInt(Integer::intValue)
				 .sum();
```


通过stream API，可以先使用 .mapToInt() 将Integer流转为IntStream（这是为了搞笑处理基本类型），然后直接调用 .sum() 方法来计算总和。

#### Stream流的并行API是什么？

Stream流的并行API是 ParallelStream
并行流(ParallelStream) 就是将元数据分为多个子流对象进行多线程操作，然后将处理的结果再汇总为一个流对象，底层使用通用的fork/join池来实现，即将一个任务拆分为多个小任务并行计算，再把多个小任务的结果合并成总的计算结果。

Stream’串行流和并行流的主要区别：![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/524dc110f0f446d5b933178ecaf724f8.png)

对于cpu密集型，适用于并行流。
对于i/o密集型且任务数相对线程数较大，那么直接用ParallelStream并不是很好的选择。

#### completableFuture怎么用的？

completableFuture是由Java 8 引用的，在Java8之前一般通过Future实现异步。
Future用于表示异步计算的结果，只能通过阻塞或者轮询的方式获取结果，而且不支持回调方法。每一步的处理逻辑都嵌套在前一步的回调函数中，增加了理解和维护的难度。
CompletableFuture对Future进行了扩展，可以通过设置回调的方式处理计算结果，同时也支持组合操作，支持进一步的编排，同时一定程度解决了回调地狱的问题。
CompletableFuture的实现如下：

```
// 创建一个具有5个线程的固定大小的线程池
ExecutorService executor = Executors.newFixedThreadPool(5);

// 创建第一个异步任务：打印并返回"step1 result"
CompletableFuture<String> cf1 = CompletableFuture.supplyAsync(() -> {
    System.out.println("执行step 1");
    return "step1 result";
}, executor);

// 创建第二个异步任务：打印并返回"step2 result"
CompletableFuture<String> cf2 = CompletableFuture.supplyAsync(() -> {
    System.out.println("执行step 2");
    return "step2 result";
}, executor);

// 将两个异步任务的结果组合，当两个任务都完成后执行
cf1.thenCombine(cf2, (result1, result2) -> {
    // 打印两个任务的结果
    System.out.println(result1 + " + " + result2);
    System.out.println("执行step 3");
    // 返回新的结果，供下一步使用
    return "step3 result";
}).thenAccept(result3 -> {
    // 接收最终的结果并打印
    System.out.println(result3);
});
```



### 8、序列化

#### 怎么把一个对象从一个jvm转移到另一个jvm？

- 使用序列化和反序列化。将对象序列化为字节流，并将其发送到另一个JVM，然后在另一个JVM中反序列化字节流恢复对象。可以通过Java的 ObjectOutputStream 和ObjectInputStream实现。
- 使用消息传递机制：利用消息队列（RabbitMQ、Kafka）或者通过网络套接字进行通信，将对象从一个JVM发送到另一个。这需要自定义协议来序列化对象并在另一个JVM中反序列化。
- 使用远程方法调用（RPC）：可以使用远程方法调用框架，如gRPC，来实现对象在不同JVM之间的传输。远程方法调用可以在分布式系统中调用远程JVM上的对象的方法。
- 使用共享数据库或缓存。将对象存储在共享数据库（Mysql， PostgreSQL）或共享缓存（如Redis）中，让不同的JVM可以访问这些共享数据。这种方法适用于需要共享数据但不需要直接传输数据的场景。

#### 序列化和反序列化让你自己实现你会怎么实现？

Java默认的序列化虽然实现方便，但却存在安全漏洞，不跨语言以及性能差等缺陷。
所以选择用主流序列化框架更好，如FastJson，Protobuf来替代Java序列化。
如果追求性能的话，Protobuf 序列化框架会比较合适，Protobuf 的这种数据存储格式，不仅压缩存储数据的效果好， 在编码和解码的性能方面也很高效。Protobuf 的编码和解码过程结合.proto 文件格式，加上 Protocol Buffer 独特的编码格式，只需要简单的数据运算以及位移等操作就可以完成编码与解码。可以说 Protobuf 的整体性能非常优秀。

#### 将对象转为二进制字节流具体怎么实现？

其实是定义了序列化后，反序列化前的字节流格式，然后对此格式进行操作，生成符合格式的字节流或者将字节流解析为对象。

在Java中通过序列化对象流来完成序列化和反序列化

- ObjectOutputStream：通过writeObject() 方法做序列化操作。

- ObjectInputStream：通过readObject() 方法做反序列化操作。

只有实现了Serializable或Externalizable接口的类的对象才能被序列化，否则抛出异常。

实现对象序列化的步骤：

- 让类实现Serializable接口：

```
import java.io.Serializable;

public class MyClass implements Serializable {
    // class code
}
```

- 创建输出流并写入对象

```
import java.io.FileOutputStream;
import java.io.ObjectOutputStream;
import java.io.IOException;

// 创建MyClass类型的对象
MyClass obj = new MyClass();

try {
    // 创建一个FileOutputStream来指向一个文件，这里的文件名为"object.ser"
    FileOutputStream fileOut = new FileOutputStream("object.ser");	// .ser一般用于表示序列化对象的文件

    // 创建一个ObjectOutputStream，它封装了FileOutputStream
    // ObjectOutputStream用于将对象写入到文件输出流
    ObjectOutputStream out = new ObjectOutputStream(fileOut);
    
    // 使用ObjectOutputStream的writeObject方法将obj对象序列化并写入到文件中
    out.writeObject(obj);
    
    // 关闭ObjectOutputStream流
    out.close();
    
    // 关闭FileOutputStream流
    fileOut.close();

} catch (IOException e) {
    // 捕获IOException异常，如果在打开、写入或关闭文件过程中出现问题，将执行此块
    e.printStackTrace();
}
```

- 实现对象反序列化：创建输入流并读取对象

```
import java.io.FileInputStream;
import java.io.ObjectInputStream;

// 声明一个 MyClass 类型的变量，初始为 null
MyClass newObj = null;

try {
    // 创建一个 FileInputStream 来从指定的文件（这里是 "object.ser"）读取数据
    FileInputStream fileIn = new FileInputStream("object.ser");

    // 创建一个 ObjectInputStream，它封装了 FileInputStream
    // ObjectInputStream 用于从文件中读取并恢复对象
    ObjectInputStream in = new ObjectInputStream(fileIn);
    
    // 使用 ObjectInputStream 的 readObject 方法读取先前序列化的对象
    // 需要将返回的 Object 类型强制转换为 MyClass 类型
    newObj = (MyClass) in.readObject();
    
    // 关闭 ObjectInputStream
    in.close();
    
    // 关闭 FileInputStream
    fileIn.close();

} catch (IOException | ClassNotFoundException e) {
    // 捕获 IOException 或 ClassNotFoundException 异常
    // IOException 可能由文件操作引发，如文件不存在、数据不完整等
    // ClassNotFoundException 由于找不到序列化对象的类定义而抛出
    e.printStackTrace();
}
```

### 9、设计模式

#### volatile和sychronized如何实现单例模式？（volatile和synchronized都是保证同步的。volatile更轻便，synchronized成为重量级锁）

```
public class SingleTon {

    // volatile 关键字修饰变量 防止指令重排序
    private static volatile SingleTon instance = null;
    private SingleTon(){}
     
    public static  SingleTon getInstance(){
        if(instance == null){
            //同步代码块 只有在第一次获取对象的时候会执行到 ，
            //第二次及以后访问时 instance变量均非null故不会往下执行了 直接返回啦
            synchronized(SingleTon.class){
                if(instance == null){
                    instance = new SingleTon();
                }
            }
        }
        return instance;
    }

}
```


正确的双重检查锁定模式需要使用volatile。volatile包含两个功能：

- 保证可见性。使用volatile定义的变量，将会保证对所有的线程的可见性。

- 禁止指定重排序优化。

由于volatile禁止对象创建时指定之间重排序，所以其他线程不会访问到一个未初始化的对象，从而保证安全性。

#### 代理模式和适配器模式有什么区别？（代理模式和适配器模式都是Java设计模式中的）

适配器模式：将一个接口转换成客户希望的另一个接口，使原本不兼容的接口类可以一起工作
代理模式：给一个对象提供一个代理对象，并由代理对象控制对原对象的引用，使客户不能直接与真正的目标对象通信
结构不同：代理模式一般包含抽象主题，真实主题和代理 三种角色，适配器模式包含目标接口，适配器和被适配者 三种角色
应用场景不同：代理模式常用于添加额外功能或控制对对象的访问，适配器模式常用于让不兼容的接口协同工作。

### 10、I/O （⭐️⭐️看不懂）

#### BIO、NIO、AIO区别是什么？

- BIO（blocking IO）：就是传统的java.io包，是基于流模型实现的，交互方式是同步、阻塞方式，也就是说在读入输入流或输出流时，在读写动作完成前，线程会一直阻塞在哪里，他们之间的调用是可靠的线性顺序

- NIO（non-blocking IO）：是java 1.4引入的java.nio包。可以构建多路复用、同步非阻塞的IO程序，同时提供了更接近操作系统底层的数据操作方式。
- AIO（Asynchronous IO）：是Java1.7后引入的NIO的升级版本，提供了异步非阻塞的IO操作方式，是基于事件和回调机制实现的。应用操作后会直接返回，不会阻塞在那里，当后台处理完成后操作系统会通知响应线程进行后续操作。

> #### ava 是如何实现网络IO高并发编程的？

可以使用Java NIO，是一种同步非阻塞的I/O模型，也是I/O多路复用的基础
NIO是基于I/O多路复用实现的，它可以只用一个线程处理多个客户端I/O，如果需要管理成千上万个连接，但是每个连接只发送少量数据例如一个聊天服务器，用NIO实现会好点

#### NIO 是如何实现的？

NIO是一种同步非阻塞的IO模型，所以也可以叫NON-BLOCKINGIO。（同步是指线程不断轮询IO事件是否就绪，非阻塞是指线程在等待IO的时候，可以同时做其他任务。）
NIO主要有三个核心部分：Channel（通道）、Buffer（缓冲区），Selector（选择区）。传统IO基于字节流和字符流进行操作，而NIO基于Channel和Buffer进行操作。数据总是从通道读取到缓冲区中，或从缓冲区写入到通道。Selector用于监听多个通道事件（如连接打开，数据到达）。因此单个线程可以监听多个数据通道。

NIO有一个专门的线程处理所有的IO时间，并负责分发。事件驱动机制，事件到来时出发操作，不需要阻塞的监视时间，线程之间通过wait，notify通信，减少线程切换。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a7112074f3dc496f9683bb738aea4808.png)

#### 哪个框架用到了NIO？

Netty。

### 11、其他问题

#### 有一个学生类，想按照分数降序排序，再按照学号升序排序，如何实现？

可以使用Comparable接口来实现按照分数排序，再按照学号排序。

```
public class Student implements Comparable<Student> {
	private int id;
	private int score;
	

// 构造方法和其他属性、方法省略

    @Override
    public int compareTo(Student other) {
        if(this.score != other.score)
            return Integer.compare(other.score, this.score);	// 按照分数降序排序
        else
            return Integer.compare(this.id, other.id);	// 如果分数相同，则按照学号升序排序
    }

}
```


然后在需要对学生列表排序的地方，使用Collections.sort() 方法对学生列表进行排序即可。

```
List<Student> students = new ArrayList<>();
// 添加学生对象列表中
Collections.sort(students);

```

#### 如何对一个List进行排序？

- 使用 Collections.sort()

  如果列表元素实现了Comparable接口，那么可以直接使用它们的自然顺序进行排序。你也可以提供一个自定义的Comparator来指定排序的具体规则。

  ```
  // 使用元素自然顺序
  
  List<Integer> list = new ArrayList<>(Arrays.asList(5, 3, 1, 4, 2));
  Collections.sort(list);
  System.out.println(list); // 输出排序后的列表: [1, 2, 3, 4, 5]
  
  //使用自定义比较器
  List<String> list = new ArrayList<>(Arrays.asList("banana", "apple", "cherry"));
  Collections.sort(list, new Comparator<String>() {
      public int compare(String o1, String o2) {
          return o1.length() - o2.length(); // 根据字符串长度排序
      }
  });
  System.out.println(list); // 输出排序后的列表: ["apple", "banana", "cherry"]
  ```

  

- 使用 List.sort()

```
// 使用元素自然顺序

List<Integer> list = new ArrayList<>(Arrays.asList(5, 3, 1, 4, 2));
list.sort(null); // 等同于 list.sort(Comparator.naturalOrder());
System.out.println(list); // 输出: [1, 2, 3, 4, 5]

//使用自定义比较器

List<String> list = new ArrayList<>(Arrays.asList("banana", "apple", "cherry"));
list.sort((s1, s2) -> s1.length() - s2.length()); // 使用Lambda表达式简化Comparator的编写
System.out.println(list); // 输出: ["apple", "banana", "cherry"]
```



#### Native方法解释一下

在Java中，native方法是一种特殊类型的方法， 他允许Java代码调用外部的本地代码，如C C++编写的代码。
在Java类中，native方法看起来和其他方法类似，只是其方法体由native关键字代替，没有实际的实现代码。例如：

```
public class NativeExample {
	public native void nativeMethod();
}

```

实现Native方法需要完成的步骤：

生成JNI头文件
编写本地代码：用别的语言
编译本地代码：将C/C++代码编译成动态链接库（DLL for Windows），共享库（SO for linux）
加载本地库：在Java中使用System.loadLibrary()方法来加载你编译好的本地库，这样JVM就可以找到并调用native方法的实现。

------

## 二、Java集合面试篇

### 1、概念

#### 数组和集合的区别，用过哪些？

数组和集合的区别：

- 数组是固定长度的数据结构，长度无法改变。而集合是动态长度的数据结构。

- 数组可以包含基本数据类型和对象，而集合只能包含对象。
- 数组可以直接访问元素，而集合需要通过迭代器或其他方法访问元素

我用过的Java集合类：

1. ArrayList：动态数组，实现了List接口，支持动态增长

2. LinkedList：双向链表，也实现了List接口，支持快速插入和删除。
3. HashMap：基于哈希表的Map实现，存储键值对，通过键快速查找值。
4. HashSet：基于HashMap实现的Set集合。用于存储唯一元素
5. TreeMap：基于红黑树实现的有序Map集合，可以按照键的顺序进行排序。
6. LinkedHashMap：基于哈希表和双向链表实现的Map集合，保持插入顺序或访问顺序。
7. PriorityQueue：优先队列，可以按照比较器或元素的自然顺序进行排序。

#### 说说Java中的集合

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1b95f23588e645719e240182b21b4975.png)

List是有序的Collection。实现List的类有LinkedList，ArrayList，Vector，Stack。

- ArrayList是非线程安全列表，底层用数组实现。ArrayList随机访问很快，但插入和删除很慢。

- LinkedList本质是一个双向链表，与ArrayList相比，插入和删除速度快，但随机访问速度变慢。

**Set**是去重集合。Set无序，常用的实现有 HashSet，LinkedHashSet，TreeSet。

- HashSet通过HashMap实现，HashMap的key即HashSet存储的元素。所有的Key都使用相同Value：一个名为PRESENT的Object类型变量。使用Key保证元素唯一性，但不保证有序。HashSet线程不安全

- LinkedHashSet继承HashSet，通过LinkedHashMap实现，使用双向链表维护元素插入顺序
- TreeSet通过TreeMap实现，添加元素到集合时按照比较规则将其插入到合适位置，保证插入后集合仍有序。

Map是一个键值对集合，存储键、值和之间的映射。Key无序、唯一；value无序，可重复。
Map没有继承Collection接口。实现有TreeMap，HashMap，HashTable，LinkedHashMap，ConcurrentHashMap。

- HashMap：长度小于阈值（默认为8）时使用数组+链表实现，大于时将链表转为红黑树，以减少搜索时间。
- LinkedHashMap：继承自HashMap，所以其底层仍然基于拉链式散列结构，即由数组和链表或红黑树组成。LinkedHashMap还加了一条双向链表，使其可以保持键值对的插入顺序。
- HashTable：数组+链表组成，数组是HashTable的主体，链表则是主要为了解决哈希冲突而存在的。
- TreeMap：红黑树（自平衡的排序二叉树）
- ConcurrentHashMap：Node数组+链表+红黑树实现，是线程安全的（1.8后volatile + CAS 或者synchronized）

#### Java中线程安全的集合是什么？

在java.util 包中的线程安全的类主要 2 个，其他都是非线程安全的：

- Vector：其内部方法基本都经过synchronized修饰。Vector内部是使用对象数组来保存数据。当数组已满时，会创建新的数组，并拷贝原有数组数据。如果不需要线程安全，并不建议选择，毕竟同步是有额外开销的。

- Hashtable：线程安全的哈希表。其枷锁方法是给每个方法加上synchronized关键字，这样锁住的是整个Table对象，不支持null键和值。很少使用。如果要保证线程安全的哈希表，可以使用ConcurrentHashMap

java.util.concurrent 包提供的都是线程安全的集合：

并发Map：

- ConcurrentHashMap：在Jdk1.8，它直接在table元素上加锁，实现对每一行进行加锁，进一步见笑了并发冲突和概率。对于put操作，①如果key对应的数组元素为null，则通过CAS（Compare and Swap）将其设置为当前值。②如果key对应的数组元素不为null，则对该元素使用synchronized关键字申请锁，从而提高寻址效率。
- ConcurrentSkipListMap：实现了一个基于SkipList（跳表）算法的可排序的并发集合，SkipList是一种可以在对数预期时间内完成搜索、插入和删除熬做的数据结构，是通过维护多个指向其他元素的跳跃链接来实现高校查找。

并发Set：

- ConcurrentSkipListSet：线程安全的有序集合。底层使用ConcurrentSkipListMap。

- CopyOnWriteArraySet：是线程安全的HashSet。CopyOnWriteArraySet和HashSet都继承共同的父类AbstractSet，但HashSet是通过散列表实现，CopyOnWriteArraySet通过动态数组（CopyOnWriteArrayList）实现的，并不是散列表。

并发List：

- CopyOnWriteArrayList：它是ArrayList的线程安全的变体，当对象进行写操作时，使用Lock锁做同步处理，同步拷贝了原数组，并在新数组上进行添加操作，最后将新数组替换旧数组。若读操作，则直接返回结果，操作过程不需要进行同步处理。

并发Queue：

- ConcurrentLinkedQueue：是一个适用于高并发场景下的队列，它通过无锁的方式（CAS），实现了高并发状态下的高性能。

- BlockingQueue：主要用于简化多线程间的数据共享。BlockingQueue提供一种读写阻塞等待的机制，即如果消费者速度较快则BlockingQueue可能被清空，此时消费线程再试图从BlockingQueue读取数据时会被阻塞。反之如果生产队列较快，则BlockingQueue可能会被装满，此时生产线程再试图像BlockingQueue队列装入数据时便会被阻塞等待。

并发Deque：

- LinkedBlockingDeque：线程安全的双端队列。内部使用链表实现，每个节点有前驱节点后继结点。LinkedBlockingDeque没有进行读写锁的分离，因此同一时间只能有一个线程对其操作。

- ConcurrentLinkedDeque：ConcurrentLinkedDeque是一种基于链表节点的无限并发链表。可以安全地并发执行插入、删除和访问操作。当许多线程访问一个公共集合时，ConcurrentLinkedDeque是一个合适的选择。

#### Collections和Collection的区别

- Collection是所有集合类的基础接口，定义了通用的操作，如添加、遍历和删除等。Collection接口有许多实现类，如List，Set和Queue等。

- Collections是位于java.util包中的工具类。提供了一系列静态方法，用于对集合进行操作和算法。包含排序、查找、替换、反转、随机化等。这些方法可以实现对Collection接口的集合进行操作，如List和Set。

#### 集合遍历的方法有哪些？

在Java中，集合的遍历方法主要有下面几种：

- 普通 for 循环

```
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");

for (int i = 0; i < list.size(); i++) {
    String element = list.get(i);
    System.out.println(element);
}
```

- 增强 for 循环（for-each循环）

```
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");

for (String element : list) {
    System.out.println(element);
}
```

- 
  Iterator 迭代器： 可以使用迭代器来遍历集合，特别适用于需要删除元素的情况。

```
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");

Iterator<String> iterator = list.iterator();
while(iterator.hasNext()) {
    String element = iterator.next();
    System.out.println(element);
}
```

- 
  ListIterator 列表迭代器： ListIterator是迭代器的子类，可以双向访问列表并在迭代过程中修改元素。

```
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");

ListIterator<String> listIterator= list.listIterator();
while(listIterator.hasNext()) {
    String element = listIterator.next();
    System.out.println(element);
}
```

- 使用 forEach 方法

```
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");

list.forEach(element -> System.out.println(element));
```

- 
  Stream API： Java 8的Stream API提供了丰富的功能，可以对集合进行函数式操作，如过滤、映射等。

```
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");

list.stream().forEach(element -> System.out.println(element));
```

--------------

### 2、List

#### 把ArrayList变成线程安全有哪些方法？

使用Collections类的synchronizedList方法将ArrayList包装成线程安全的List：

```
List<String> synchronizedList = Collections.synchronizedList(arrayList);
```


使用CopyOnWriteArrayList类代替ArrayList，它是一个线程安全的List实现：

```
CopyOnWriteArrayList<String> copyOnWriteArrayList = new CopyOnWriteArrayList<>(arrayList);
```


使用Vector类代替ArrayList，Vector是线程安全的List实现：

```
Vector<String> vector = new Vector<>(arrayList);
```



#### ArrayList线程不安全的体现在哪里？

在高并发下，ArrayList会暴露三个问题：

- 部分值为null

- 索引越界异常
- size和我们add的数量不符。

三种情况分别是如何产生的：

- 部分值为null：当线程1走到了扩容那里发现当前size是9，而数组容量是10，所以不用扩容，这时候cpu让出执行权，线程2也进来了，发现size是9，而数组容量是10，所以不用扩容，这时候线程1继续执行，将数组下标索引为9的位置set值了，还没有来得及执行size++，这时候线程2也来执行了，又把数组下标索引为9的位置set了一遍，这时候两个先后进行size++，导致下标索引10的地方就为null了。

- 索引越界异常：线程1走到扩容那里发现当前size是9，数组容量是10不用扩容，cpu让出执行权，线程2也发现不用扩容，这时候数组的容量就是10，而线程1 set完之后size++，这时候线程2再进来size就是10，数组的大小只有10，而你要设置下标索引为10的就会越界（数组的下标索引从0开始）；
- size与我们add的数量不符：这个基本上每次都会发生，这个理解起来也很简单，因为size++本身就不是原子操作，可以分为三步：获取size的值，将size的值加1，将新的size值覆盖掉原来的，线程1和线程2拿到一样的size值加完了同时覆盖，就会导致一次没有加上，所以肯定不会与我们add的数量保持一致的；

#### ArrayList的扩容机制是怎么样的

主要步骤如下：

1. 计算新的容量：新的容量会扩大为原容量的1.5倍，然后检查是否超过了最大容量限制。

2. 创建新的数组：根据计算得到的新容量，创建一个新的更大的数组
3. 将元素复制：将原来数组的元素逐个复制到新数组中。
4. 更新引用：将ArrayList内部指向原数组的引用指向新数组。
5. 完成扩容：扩容完成后，可以继续添加新元素。

ArrayList的扩容操作涉及到数组的复制，内存的重新分配，所以在频繁添加大量元素时，扩容操作可能会影响性能。为了减少扩容的性能损耗，可以在初始化ArrayList时预分配足够大的容量，避免频繁触发扩容操作。

扩容1.5倍是因为 1.5 可以充分利用移位操作，减少浮点数或运算时间和运算次数。

```
// 新容量计算
int newCapacity = oldCapacity + (oldCapacity >> 1);
```



> 线程安全的List，CopyonWriteArraylist 是如何实现线程安全的？

CopyonWriteArraylist 底层也是通过一个数组保存数据，使用volatile关键字修饰数组，保证当前线程对数组对象重新赋值后，其他线程可以及时感知到：

```
private transient volatile Object[] array;
```

在写操作，加入了一把互斥锁 ReentrantLock 以保证线程安全。
在读操作是没有加锁的，所以读是一直都能读。

---------------

### 3、Map

#### HashMap的原理

JDK 1.7之前，HashMap的数据结构是数组和链表，通过哈希算法将元素的键 key 映射到数组中的槽位（bucket）。如果冲突，会以链表的形式存储在一个槽位上。![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c238d3473d4c4ce6b78c4092d73f066a.png)

在 JDK 1.8 版本后，当一个链表的长度超过 8 时，将链表转为红黑树，查找为O(logn)。然后在数量小于6时，将红黑树转回链表。![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/438cb6745f8046a6a16fb76a13e79f56.png)

> 哈希冲突解决方法有哪些？

- 链接法

- 开放寻址法：包含线性探测、二次探测和双重散列。
- 再哈希法
- 哈希桶扩容：当哈希冲突过多时，可以动态地扩大哈希桶的数量，重新分配键值对以减少冲突的概率。

#### HashMap是线程安全的吗？

- 数组+链表存储，多线程背景下数组扩容时，存在 Entry 链死循环和数据丢失问题。
- 数组+链表+红黑二叉树，多线程背景下，put方法存在数据覆盖的问题。

要保证线程安全，可以通过这些方法来保证：

- 使用Collections.synchronizedMap同步加锁方式，还可以使用ConcurrentHashMap。

- ConcurrentHashMap在JDK1.7使用Segment+HashEntry分段锁的方式实现，1.8则改为用CAS+synchronized+Node实现，同样加入了红黑树，以避免链表过长导致的性能问题。

#### HashMap的put过程介绍一下

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/30392a39a7b24605a196d492e43c56ce.png)

1.根据要添加的键计算哈希，得到这数组中的索引。
2.检查该位置是否为空（有无键值对存在）

- 如果为空，则直接在该位置创建一个新的Entry对象来存储键值对。将HashMap的修改次数（modCount）加1，以便在进行迭代时发现和并发修改。

3.如果该位置已存在键值对，检查该位置第一个键值对的哈希码和键是否与要添加的键值对相同？

- 如果相同，则直接将新的值替换旧的值（键都是相同的），完成更新动作。

4.如果第一个键值对的哈希码和键不同，则需要遍历链表或红黑树来潮找是否有相同的键：

- 是链表：则从头部逐个比较键的哈希玛和equals()方法，直到找到相同的键或到达链表末尾。

​	找到相同的键：用新值代替旧值，更新键对应的值。
​	找不到相同的键：则将新的键值对添加到链表头

- 是红黑树：在红黑树中使用哈希码和equals()方法进行查找。根据键的哈希码，定位到某个节点，然后逐个比较键，直到找到相同的键或到达红黑树的末端。

5.检查链表长度是否到达阈值 8

- 如果链表长度超过8，且HashMap的数组长度大于等于64，则会将链表转为红黑树。

6.检查负载因子是否超过阈值（默认为0.75）

- 如果键值对的数量（size）和数组的长度的比值大于阈值，则需要进行扩容操作。

7.扩容操作：

- ​	创建一个新的 2 倍大小的数组
- ​	将旧数组中的键值对重新计算哈希码并分配到新数组。
- ​	更新HashMap的数组引用和阈值参数。

8.完成添加操作。

#### HashMap调用get方法一定安全吗？为什么？

- 空指针异常：如果HashMap没有被初始化，则会抛出空指针异常。反之则使用null作为键是允许的，因为HashMap支持null。
- 线程安全：HashMap本身不是线程安全的。如果在多线程中用HashMap，可以用ConcurrentHashMap。

#### HashMap为什么一般用String作为key？

用String做key，是因为String对象是不可变的，一旦创建就不能被修改，保证了key的稳定性。

#### 为什么HashMap要用红黑树而不是平衡二叉树？

- 平衡二叉树过于严格，任何节点的左右字数高度差不会超过1。但是每次插入/删除时，几乎都会破坏规则，所以都需要通过左旋 右旋来调整。

- 红黑树追求一种弱平衡：整个树最长路径不会长过最短路径的 2 倍。和平衡二叉树不同的是，红黑树在插入、删除等操作时，不会像平衡二叉树那样频繁破坏规则，所以不需要频繁调整，这也是我们大多数情况下用红黑树的原因。

#### HashMap的key可以为null吗？

可以为null。

- hashMap使用hash()方法来计算key的哈希值，当key为null时，直接令key的哈希值是0，不走key.hashCode()方法：![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d510566557064a7c8a221d9efde71bfd.png)

- hashMap虽然支持key和value为null，但是null作为key只能有一个，null作为value可以有多个；

- 因为hashMap中，如果key值一样，那么会覆盖相同key值的value为最新，所以key为null只能有一个。

  #### 重写HashMap的equal和hashcode方法需要注意什么？

从HashMap中取值时，要用到key对象的hashCode()和equals方法。
此外，所有不允许重复存储数据的集合类都使用hashCode()和equals()去查找重复。equals()和hashCode()的实现应遵循如下规则：

- 如果o1.equals(o2)，则o1.hashCode() == o2.hashCode()总为true。

- 如果o1.hashCode() == o2.hashCode()，并不一定o1.equals(o2)成立。

#### 重写HashMap的equal方法不当会出现什么问题？

HashMap在比较元素时，会先通过hashCode进行比较，相同的情况下再通过equals进行比较。所以 equals相等的两个对象，hashCode一定相等。hashCode相等的两个对象，equals不一定相等（比如散列冲突的情况）

重写了equals方法，不重写hashCode方法时，可能会出现equals方法返回为true，而hashCode方法却返回false，这样的一个后果会导致在hashmap等类中存储多个一模一样的对象，导致出现覆盖存储的数据的问题，这与hashmap只能有唯一的key的规范不符合。

列举HashMap在多线程中存在的问题：

- JDK1.7中的 HashMap 使用头插法插入元素，在多线程的环境下，扩容的时候有可能导致环形链表的出现，形成死循环。因此，JDK1.8使用尾插法插入元素，在扩容时会保持链表元素原本的顺序，不会出现环形链表的问题。

- 多线程同时执行 put 操作，如果计算出来的索引位置是相同的，那会造成前一个 key 被后一个 key 覆盖，从而导致元素的丢失。此问题在JDK 1.7和 JDK 1.8 中都存在。

#### HashMap的扩容机制介绍一下（不懂⭐️）

hashMap默认的负载因子是0.75，即如果hashmap中的元素个数超过了总容量75%，则会触发扩容，扩容分为两个步骤：

1. 对哈希表长度的扩展（2倍）

2. 是将旧哈希表中的数据放到新的哈希表中。

因为我们使用的是2次幂的扩展(指长度扩为原来2倍)，所以，元素的位置要么是在原位置，要么是在原位置再移动2次幂的位置。![请添加图片描述](https://i-blog.csdnimg.cn/direct/45e008cd69b84460b521bcdf13cd776c.webp)

因此，我们在扩充HashMap的时候，不需要重新计算hash，只需要看看原来的hash值新增的那个bit是1还是0就好了，是0的话索引没变，是1的话索引变成“原索引+oldCap”。可以看看下图为16扩充为32的resize示意图：![请添加图片描述](https://i-blog.csdnimg.cn/direct/74722b428b014191b5c9bc27fb028942.webp)

> HashMap的大小为什么是2的n次方大小呢？（不懂⭐️）

而在 JDK 1.8 中，HashMap 对扩容操作做了优化。由于扩容数组的长度是 2 倍关系，所以对于假设初始 tableSize = 4 要扩容到 8 来说就是 0100 到 1000 的变化（左移一位就是 2 倍），在扩容中只用判断原来的 hash 值和左移动的一位（newtable 的值）按位与操作是 0 或 1 就行，0 的话索引不变，1 的话索引变成原索引加上扩容前数组。（旧哈希值和新哈希值右边的位数都完全一样。）

#### 往hashmap存20个元素，会扩容几次？

初始容量：16

插入第 1 到第 12 个元素时，不需要扩容。
插入第 13 个元素时，达到负载因子限制，需要扩容。此时，HashMap 的容量从 16 扩容到 32。
扩容后的容量：32

插入第 14 到第 24 个元素时，不需要扩容。
因此，总共会进行一次扩容。

#### Hashmap和Hashtable有什么不一样的？Hashmap一般怎么用？

- HashMap线程不安全，效率高一点，可以存储null的key和value，null的key只能有一个，null的value可以有多个。默认初始容量为16，每次扩充变为原来2倍。创建时如果给定了初始容量，则扩充为2的幂次方大小。底层数据结构为数组+链表，插入元素后如果链表长度大于阈值（默认为8），先判断数组长度是否小于64，如果小于，则扩充数组，则链表转化为红黑树，以减少搜索时间。如果负载因子大于0.75，则扩容。

- HashTable线程安全，效率低一点，其内部方法基本都经过synchronized修饰，不可以有null的key和value。默认初始容量为11，每次扩容变为原来的2n+1。创建时给定了初始容量，会直接用给定的大小。底层数据结构为数组+链表。它基本被淘汰了，要保证线程安全可以用ConcurrentHashMap。

#### ConcurrentHashMap怎么实现的？

- JDK 1.7 ConcurrentHashMap

​	使用数组+链表形式实现的。数组又分为：大数组Segment和小数组HashEntry。Segment是一种可重入锁（ReentrantLock），在ConcurrentHashMap中扮演锁的角色。
​	HashEntry则用于存储键值对数据。一个ConcurrentHashMap里包含一个Segment**数组**（多个Segment元素），一个Segment里包含一个HashEntry**数组**，每个HashEntry是一个链表结构元素。
JDK1.7ConcurrentHashMap 分段锁技术将数据分成一段段存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据时，其他段数据也能被其他线程访问，能够实现真正的并发访问。![请添加图片描述](https://i-blog.csdnimg.cn/direct/183d7b16ab414d2192698c1e426e6018.webp)

- 
  JDK 1.8 ConcurrentHashMap

  JDK1.8使用数组+链表/红黑树的方式优化ConcurrentHashMap，优化数据比较多情况下访问慢的情况。

  ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e9309598e99840e5ae3df936bb6f34d7.png)

JDK 1.8 ConCurrentHashMap 主要通过 volatile + CAS 或者synchronized来实现的线程安全。添加元素时首先会判断容器是否为空：

- 如果为空则使用 volatile + CAS 来初始化。
- 如果容器不为空，则根据存储的元素来计算该位置是否为空。

1. 如果根据存储的元素计算结果为空，则利用CAS设置该节点；
2. 如果根据存储的元素计算结果不为空，则使用synchronized，然后遍历桶中的数据，并替换或新增节点到桶中，最后再判断是否需要转为红黑树，这样就能保证并发访问时的线程安全了。

如果把上面的一句话归纳，就是ConcurrentHashMap通过对头节点（根节点）加锁来保证线程安全，锁的粒度相比Segment来说更小了，发生冲突和加锁的频率降低了。并发操作的性能就提高了。

#### 分段锁怎么加锁的？

在 ConcurrentHashMap 中，将整个数据结构分为多个 Segment，每个 Segment 都类似于一个小的 HashMap，每个 Segment 都有自己的锁，不同 Segment 之间的操作互不影响，从而提高并发性能。

在 ConcurrentHashMap 中，对于插入、更新、删除等操作，需要先定位到具体的 Segment，然后再在该 Segment 上加锁，而不是像传统的 HashMap 一样对整个数据结构加锁。这样可以使得不同 Segment 之间的操作并行进行，提高了并发性能。

#### 分段锁是可重入的吗？

JDK 1.7 ConcurrentHashMap中的分段锁是用了 ReentrantLock，是一个可重入的锁。
（**什么是可重入**：可重入就是说某个线程已经获得某个锁，可以再次获取锁而不会出现死锁
换言之：可重入就是一个线程不用释放，可以重复的获取一个锁n次，只是在释放的时候，也需要相应的释放n次。（简单来说：A线程在某上下文中或得了某锁，当A线程想要在次获取该锁时，不会应为锁已经被自己占用，而需要先等到锁的释放）假使A线程即获得了锁，又在等待锁的释放，就会造成死锁。）

#### 什么是悲观锁和乐观锁？

1.悲观锁：
	顾名思义，就是比较悲观的锁，总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁（共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程）。
悲观锁的实现方式为：synchronized和ReentrantLock。
2.乐观锁
	总是假设最好的情况，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号机制和CAS算法实现。乐观锁适用于多读的应用类型，这样可以提高吞吐量，像数据库提供的类似于write_condition机制，其实都是提供的乐观锁。
乐观锁实现方式：版本号控制、CAS算法
CAS算法是compare and swap（比较和交换）

#### ConcurrentHashMap用了悲观锁还是乐观锁?

添加元素时首先会判断容器是否为空。

- 如果为空则使用volatile加CAS（乐观锁）来初始化。

- 如果容器不为空，则根据存储的元素计算该位置是否为空。
- 如果根据存储的元素计算结果为空，则利用 CAS（乐观锁） 设置该节点；
- 如果根据存储的元素计算结果不为空，则使用 synchronized（悲观锁） ，然后，遍历桶中的数据，并替换或新增节点到桶中，最后再判断是否需要转为红黑树，这样就能保证并发访问时的线程安全了。

#### HashTable底层实现原理是什么？

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/683c1f7c20364ebe8d0d2e7f448a6b29.png)

Hashtable的底层数据结构主要是数组加链表。数组是主体，链表是解决hash冲突存在的。
HashTable是线程安全的，实现方式是Hashtable的所有公共方法均采用synchronized关键字（悲观锁）。当一个线程访问同步方法，另一个线程也访问的时候，就会陷入阻塞或轮询的状态。

#### HashTable线程安全是怎么实现的？

可以看到，Hashtable是通过使用了 synchronized 关键字来保证其线程安全。

在Java中，可以使用synchronized关键字来标记一个方法或者代码块，当某个线程调用该对象的synchronized方法或者访问synchronized代码块时，这个线程便获得了该对象的锁，其他线程暂时无法访问这个方法，只有等待这个方法执行完毕或者代码块执行完毕，这个线程才会释放该对象的锁，其他线程才能执行这个方法或者代码块。

#### hashtable和concurrentHashMap的区别

**底层数据结构：**

- jdk7之前concurrenthashmap底层采用分段数组+链表实现，jdk8后采用的是数组+链表/红黑树

- HashTable采用数组+链表，数组是主体，链表是解决hash冲突存在的 。

**实现线程安全的方式：**

- jdk8前，concurrenthashmap采用分段锁，对整个数组进行了分段分割，每一把锁只锁容器里的一部分数据，多线程访问不同数据段里的数据就不会存在锁竞争，提高并发访问。jdk8后，直接采用数组+链表/红黑树，并发控制采用CAS和synchronized操作，更加提高了速度。

- Hashtable：所有的方法都加了悲观锁来控制。当一个线程访问同步方法，此时另一个方法也来访问时，就会陷入阻塞或者轮询的状态。

#### 说一下HashMap和Hashtable、ConcurrentMap的区别 （不熟⭐️⭐️）

- HashMap线程不安全，效率高一点，可以存储null的key和value，null的key只能有一个，null的value可以有多个。默认初始容量为16，每次扩充变为原来2倍。创建时如果给定了初始容量，则扩充为2的幂次方大小。底层数据结构为数组+链表，插入元素后如果链表长度大于阈值（默认为8），先判断数组长度是否小于64，如果小于，则扩充数组，反之将链表转化为红黑树，以减少搜索时间。

- HashTable线程安全，效率低一点，其内部方法基本都经过synchronized修饰，不可以有null的key和value。默认初始容量为11，每次扩容变为原来的2n+1。创建时给定了初始容量，会直接用给定的大小。底层数据结构为数组+链表。它基本被淘汰了，要保证线程安全可以用ConcurrentHashMap。
- ConcurrentHashMap是Java中的一个线程安全的哈希表实现，它可以在多线程环境下并发地进行读写操作，而不需要像传统的HashTable那样在读写时加锁。ConcurrentHashMap的实现原理主要基于分段锁和CAS操作。它将整个哈希表分成了多Segment（段），每个Segment都类似于一个小的HashMap，它拥有自己的数组和一个独立的锁。在ConcurrentHashMap中，读操作不需要锁，可以直接对Segment进行读取，而写操作则只需要锁定对应的Segment，而不是整个哈希表，这样可以大大提高并发性能。

### **4、Set**

#### Set集合有什么特点？如何实现key无重复的？

- set集合特点：Set集合中的元素是唯一的，不会出现重复的元素。

- set实现原理：Set集合通过内部的数据结构（如哈希表、红黑树等）来实现key的无重复。当向Set集合中插入元素时，会先根据元素的hashCode值来确定元素的存储位置，然后再通过equals方法来判断是否已经存在相同的元素，如果存在则不会再次插入，保证了元素的唯一性

#### 有序的Set是什么？记录插入顺序的集合是什么？

有序的 Set 是TreeSet和LinkedHashSet。TreeSet是基于红黑树实现，保证元素的自然顺序。LinkedHashSet是基于双重链表和哈希表的结合来实现元素的有序存储，保证元素添加的顺序

记录插入顺序的集合通常指的是LinkedHashSet，它不仅保证元素的唯一性，还可以保持元素的插入顺序。当需要在Set集合中记录元素的插入顺序时，可以选择使用LinkedHashSet来实现。

-----

## 三、Java并发

### 1、多线程

> Java的线程安全体现在三个方面：

- 原子性： 提供互斥访问。同一时刻只能有一个线程对数据进行操作，在Java中使用atomic和synchronized这两个关键字来确保原子性。

- 可见性： 一个线程对主内存的修改可以及时的被其他线程看到。使用synchronized和volatile这两个关键字确保可见性。

- 有序性：一个线程观察其他线程中的指令执行顺序，使用happends-before原则来确保有序性。

  > 保证数据的一致性有哪些方案？

- 事务管理： 使数据库事务保证一组数据库操作要么全部成功，要么全部失败。通过ACID实现（原子性、一致性、隔离性、持久性）
- 锁机制： 使用锁来实现对共享资源的互斥访问，如synchronized, ReentrantLock控制并发访问。
- 版本控制： 通过乐观锁在更新数据时记录数据的版本信息。

#### 线程的创建方式

1.继承Thread类
	重写其run()方法，run()方法中定义了线程执行的具体任务。创建该类的实例后，通过调用start()方法启动线程。

```
class MyThread extends Thread {
    @Override
    public void run() {
        // 线程执行的代码
    }
}

public static void main(String[] args) {
    MyThread t = new MyThread();
    t.start();
}
```


2.实现Runnable接口

如果已经继承了其他类就不能继承Thread类，则可以实现java.lang.Runnale接口。也需要重写run方法。创建Thread对象后调用其start()方法启动线程。

```
class MyRunnable implements Runnable {
    @Override
    public void run() {
        // 线程执行的代码
    }
}

public static void main(String[] args) {
    Thread t = new Thread(new MyRunnable());
    t.start();
}
```


采用实现Runnable接口方式：

- 优点：线程类只是实现了Runable接口，还可以继承其他的类。在这种方式下，可以多个线程共享同一个目标对象，所以非常适合多个相同线程来处理同一份资源的情况，从而可以将CPU代码和数据分开，形成清晰的模型，较好地体现了面向对象的思想。

- 缺点：编程稍微复杂，如果需要访问当前线程，必须使用Thread.currentThread()方法。

3.实现Callable接口与FutureTask
	java.util.concurrent.Callable接口类似于Runnable，但Callable的call()方法可以有返回值并且可以抛出异常。要执行Callable任务，需将它包装进一个FutureTask，因为Thread类的构造器只接受Runnable参数，而FutureTask实现了Runnable接口。

````
class MyCallable implements Callable<Integer> {
    @Override
    public Integer call() throws Exception {
        // 线程执行的代码，这里返回一个整型结果
        return 1;
    }
}

public static void main(String[] args) {
    MyCallable task = new MyCallable();
    FutureTask<Integer> futureTask = new FutureTask<>(task);
    Thread t = new Thread(futureTask);
    t.start();

```
try {
    Integer result = futureTask.get();  // 获取线程执行结果
    System.out.println("Result: " + result);
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
}
```

}
````


采用实现Callable接口方式：

- 缺点：编程稍微复杂，如果需要访问当前线程，必须调用Thread.currentThread()方法。

- 优点：线程只是实现Runnable或实现Callable接口，还可以继承其他类。这种方式下，多个线程可以共享一个target对象，非常适合多线程处理同一份资源的情形。

#### 使用线程池（Executor框架）

从Java 5开始引入的java.util.concurrent.ExecutorService和相关类提供了线程池的支持，这是一种更高效的线程管理方式，避免了频繁创建和销毁线程的开销。可以通过Executors类的静态方法创建不同类型的线程池。

```
class Task implements Runnable {
    @Override
    public void run() {
        // 线程执行的代码
    }
}

public static void main(String[] args) {
    ExecutorService executor = Executors.newFixedThreadPool(10);  // 创建固定大小的线程池
    for (int i = 0; i < 100; i++) {
        executor.submit(new Task());  // 提交任务到线程池执行
    }
    executor.shutdown();  // 关闭线程池
}
```


采用线程池方式：

- 缺点：程池增加了程序的复杂度，特别是当涉及线程池参数调整和故障排查时。错误的配置可能导致死锁、资源耗尽等问题，这些问题的诊断和修复可能较为复杂。

- 优点：线程池可以重用预先创建的线程，避免了线程创建和销毁的开销，显著提高了程序的性能。对于需要快速响应的并发请求，线程池可以迅速提供线程来处理任务，减少等待时间。并且，线程池能够有效控制运行的线程数量，防止因创建过多线程导致的系统资源耗尽（如内存溢出）。通过合理配置线程池大小，可以最大化CPU利用率和系统吞吐量。

#### 如何启动线程

```
//创建两个线程，用start启动线程
MyThread myThread1 = new MyThread();  
MyThread myThread2 = new MyThread();  
myThread1.start();  
myThread2.start(); 
```

#### 如何停止一个线程的运行?

主要有这些方法：

- 异常法停止：线程调用interrupt()方法后，在线程的run方法中判断当前对象的interrupted()状态，如果是中断状态则抛出异常，达到中断线程的效果。

- 在沉睡中停止：先将线程sleep，然后调用interrupt标记中断状态，interrupt会将阻塞状态的线程中断。会抛出中断异常，达到停止线程的效果
- stop()暴力停止：线程调用stop()方法会被暴力停止，方法已弃用，该方法会有不好的后果：强制让线程停止有可能使一些请理性的工作得不到完成。
- 使用return停止线程：调用interrupt标记为中断状态后，在run方法中判断当前线程状态，如果为中断状态则return，能达到停止线程的效果。

#### 调用interrupt是如何让线程抛出异常的？

每个线程都有一个布尔属性表示中断状态。中断状态为false。当一个线程背其他线程调用Thread.interrupt()方法中断时，会根据实际情况做出反应。

- 如果该线程中执行低级别的可中断方法（如Thread.sleep, Thread.join, Object.wait）则会解除阻塞并抛出InterruptException异常。

- 否则Thread.interrupt()仅设置线程的中断状态，在该被中断的线程中稍后可通过轮询终端状态来决定是否要停止当前正在执行的任务。

#### Java线程的状态有哪些？

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bd232cac1f074b398f2f9d6f0f7ccb92.png)

Java中有6 种线程状态。可以调用线程的Thread.getState()方法获取当前线程的状态。

---------

| 线程状态      | 解释                                                     |
| ------------- | -------------------------------------------------------- |
| NEW           | 尚未启动的线程状态，即线程创建，还未调用start方法        |
| RUNNABLE      | 就绪状态（调用start，等待调度）+正在运行                 |
| BLOCKED       | 等待监视器锁时，陷入阻塞状态                             |
| WAITING       | 等待状态的线程正在等待另一线程执行特定的操作（如notify） |
| TIMED_WAITING | 具有指定等待时间的等待状态                               |
| TERMINATED    | 线程完成执行，终止状态                                   |

#### blocked和waiting有啥区别

- 触发条件： 线程进入BLOCKED状态通常是视图获取一个对象的锁(monitor lock)，但该锁已经被另一个线程持有。这通常发生在尝试进入synchronized块或方法时，如果锁已被专用，则线程将阻塞直到锁可用。线程进入WAITING状态是因为它正在等待另一个线程执行某些操作，如调用Object.wait()方法，Thread.join()或LockSupport.park()。在 这种状态下，线程不会消耗CPU资源。并且不会参与锁的竞争。

- 唤醒机制： 当一个线程被阻塞等待锁时， 一旦锁被释放，线程将有机会重新获取锁。如果锁此时未被其他线程获取，则线程可以从BLOCKED状态转为RUNNABLE线程。线程中WAITING状态需要被显式唤醒。例如，如果线程调用了Object.wait()，则它必须等待另一个线程调用同一对象上的Object.notifg()或Object.notifyAll()方法才能被唤醒。

#### wait 状态下的线程如何进行恢复到 running 状态?（不懂⭐️⭐️）

- 等待的线程被其他线程对象唤醒，notifty() 和 notifyAll()。

- 如果线程没有获取到锁则会直接进入Waiting状态。这种本质就是执行了LockSupport.park()方法进入了waiting状态，则解锁的时候会执行LockSupport.unpark(Thread)，和上面的park方法对应，给出许可证，解除等待状态。

#### notify选择的是哪个线程？

notify在源码的注释中说唤醒的方式的任意的。但具体取决于JVM。
JVM有很多实现，比较流行的是hotspot。hotspot对notify()的实现并不是随机唤醒，而是“先进先出”的顺序。

---------

### 2、并发安全

#### 如何保证并发安全？

- synchronized关键字: 可以使用synchronized关键字同步代码块或方法，确保同一时刻只有一个线程可以访问这些代码。对象锁是通过synchronized关键字锁定对象的监视器（monitor）来实现的。


```
public synchronized void someMethod() { /* ... */ }

public void anotherMethod() {
    synchronized (someObject) {
        /* ... */
    }
}
```

- volatile关键字： volatile关键字作用于变量，确保所有线程看到的是该变量的最新值，而不是可能存储在本地寄存器的副本


```
public volatile int sharedVariable;
```

- Lock接口和ReentrantLock类： java.util.concurrent.locks.Lock接口提供了比synchronized更强大的锁定机制，ReentrantLock是一个实现该接口的例子，提供恶劣更灵活的锁管理和更高的性能。

```
private final ReentrantLock lock = new ReentrantLock();

public void someMethod() {
    lock.lock();
    try {
        /* ... */
    } finally {
        lock.unlock();
    }
}
```



- 原子类： Java并发库（java.util.concurrent.atomic）提供了原子类，如AtomicInteger, AtomicLong等。这些类提供了原子操作，可以用于更新基本类型的变量而无需额外的同步。

```
AtomicInteger counter = new AtomicInteger(0);
int newValue = counter.incrementAndGet();
```

- 线程局部变量： ThreadLocal类可以为每个线程提供独立的变量副本，这样每个县城都拥有自己的变量，消除了竞争条件。

```
ThreadLocal<Integer> threadLocalVar = new ThreadLocal<>();

threadLocalVar.set(10);
int value = threadLocalVar.get();
```

- 
  并发集合： 使用java.util.concurrent包中的线程安全集合，如ConcurrentHashMap, ConcurrentLinkedQueue, 这些集合内部已经实现了线程安全的逻辑。
- JUC工具类： 使用java.util.concurrent包中的一些工具类可以用于控制线程间的同步和协作。如Semaphore和CyclicBarrier等。


#### Java中有哪些常用的锁，在什么场景下使用？

Java中的锁是用于管理多线程并发访问共享资源的关键机制。锁可以保证在任意给定时间内只有一个线程可以访问特定的资源，从而避免数据竞争和不一致性。

- 内置锁synchronized： Java中的synchronized关键字是内置锁机制的基础，可以用于方法或代码块。当一个线程进入synchronized代码块或方法时，它会获取关联对象的锁；当线程离开该代码块或方法时，锁会被释放。如果其他线程尝试获取同一个对象的锁，它们将被阻塞，直到锁被释放。其中，syncronized加锁时有无锁、偏向锁、轻量级锁和重量级锁几个级别。偏向锁用于当一个线程进入同步块时，如果没有任何其他线程竞争，就会使用偏向锁，以减少锁的开销。轻量级锁使用线程栈上的数据结构，避免了操作系统级别的锁。重量级锁则涉及操作系统级的互斥锁。

- ReentrantLock： java.util.concurrent.locks.ReentrantLock是一个显式的锁类，提供了比synchronized更高级的方法，如可中断的锁等待，定时锁等待，公平锁选项。ReentrantLock使用lock()和unlock()来获取和释放锁。其中，公平锁按照线程请求顺序来分配锁，保证了锁分配公平性，但可能增加锁的等待时间。非公平锁不保证锁分配的顺序，可以减少锁的竞争，提高性能，但可能导致某些线程的饥饿。
- 读写锁（ReadWriteLock） 允许多个读者同时访问共享资源，但只允许一个写入者。读写锁通常用于读写远多于写入的情况，以提高并发性。
- 乐观锁和悲观锁： 悲观锁（Pessimistic Locking） 通常指在访问数据前就锁定资源，假设最坏的情况，即数据很可能被其他线程修改。synchronized和ReentrantLock都是悲观锁的例子。乐观锁（Optimistic Locking） 通常不锁定资源，而是在更新数据时检查数据是否已被其他线程修改。乐观锁常使用版本号或时间戳来实现。
- 自旋锁： 自旋锁是一种锁机制，线程在等待锁时会持续循环检查锁是否可用，而不是放弃CPU并阻塞。通常可以使用CAS来实现。这在锁等待时间很短的情况下可以提高性能，但过度自旋会浪费CPU资源。

#### 在实践中是怎么使用锁的？

1.synchronized
synchronized关键字可以用于方法或代码块，它是Java中最早的锁实现，使用起来非常简单。

```
// 示例：synchronized方法
public class Counter {
    private int count = 0;

public synchronized void increment() {
    count++;
}

public synchronized int getCount() {
    return count;
}

}

// 示例：synchronized代码块
public class Counter {
    private Object lock = new Object();
    private int count = 0;

public void increment() {
    synchronized (lock) {
        count++;
    }
}

}
```


2.使用Lock接口
Lock接口提供了比synchronized更灵活的锁操作，包括尝试锁、可中断锁、定时锁等。ReentrantLock是Lock接口的一个实现。

```
示例：使用ReentrantLock

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Counter {
    private Lock lock = new ReentrantLock();
    private int count = 0;

public void increment() {
    lock.lock();
    try {
        count++;
    } finally {
        lock.unlock();
    }
}
}
```

3.使用ReadWriteLock
ReadWriteLock接口提供了一种读写锁的实现，允许多个读操作同时进行，但写操作是独占的。

```
示例：使用ReadWriteLock

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class Cache {
    private ReadWriteLock lock = new ReentrantReadWriteLock();
    private Lock readLock = lock.readLock();
    private Lock writeLock = lock.writeLock();
    private Object data;


public Object readData() {
    readLock.lock();	// 这里的读锁是确保没有写的。不是读互斥。
    try {
        return data;
    } finally {
        readLock.unlock();
    }
}

public void writeData(Object newData) {
    writeLock.lock();
    try {
        data = newData;
    } finally {
        writeLock.unlock();
    }
}


}
```

> synchronized和reentrantlock及其应用场景？（不懂⭐️⭐️⭐️）

#### synchronized 工作原理（不懂⭐️⭐️⭐️）

synchronized是Java提供的原子性内置锁，这种内置的并且使用者看不到的锁也被称为**监视器锁**，

使用synchronized之后，会在编译之后在同步的代码块前后加上monitorenter和monitorexit字节码指令，他依赖操作系统底层互斥锁实现。他的作用主要就是实现原子性操作和解决共享变量的内存可见性问题。

执行monitorenter指令时会尝试获取对象锁，如果对象没有被锁定或者已经获得了锁，锁的计数器+1。此时其他竞争锁的线程则会进入等待队列中。执行monitorexit指令时则会把计数器-1，当计数器值为0时，则锁释放，处于等待队列中的线程再继续竞争锁。

synchronized是排它锁，当一个线程获得锁之后，其他线程必须等待该线程释放锁后才能获得锁，而且由于Java中的线程和操作系统原生线程是一一对应的，线程被阻塞或者唤醒时时会从用户态切换到内核态，这种转换非常消耗性能。

从内存语义来说，加锁的过程会清除工作内存中的共享变量，再从主内存读取，而释放锁的过程则是将工作内存中的共享变量写回主内存。

实际上大部分时候我认为说到monitorenter就行了，但是为了更清楚的描述，还是再具体一点。

如果再深入到源码来说，synchronized实际上有两个队列waitSet和entryList。

1. 当多个线程进入同步代码块时，首先进入entryList

2. 有一个线程获取到monitor锁后，就赋值给当前线程，并且计数器+1
3. 如果线程调用wait方法，将释放锁，当前线程置为null，计数器-1，同时进入waitSet等待被唤醒，调用notify或者notifyAll之后又会进入entryList竞争锁
4. 如果线程执行完毕，同样释放锁，计数器-1，当前线程置为null<img src="https://i-blog.csdnimg.cn/direct/9e6e9d8923ef4a578236ff0fb8e600e1.png" alt="在这里插入图片描述" style="zoom:50%;" />

#### reentrantlock工作原理（不懂⭐️⭐️⭐️）

ReentrantLock的底层实现主要依赖于AbstractQueuedSynchronizer(AQS)这个抽象类。AQS是一个提供了基本同步机制的框架，包括了队列、状态值。

ReentrantLock在AQS的基础上通过内部类Sync来实现具体的锁的操作。不同的Sync子类实现了公平锁和非公平锁的不同逻辑：

- 可中断性： ReentrantLock实现了可中断性，即线程在等待锁的过程中，可以被其他线程中断而提前结束等待。在底层，ReentrantLock使用了与LockSupport.park()和LockSupport.unpark()相关的机制来实现可中断性。
- 设置超时时间： ReentrantLock支持在尝试获取锁时设置超时时间，即等待一定时间后如果还未获得锁，则放弃锁的获取。这是通过内部的tryAcquireNanos方法实现的。
- 公平锁和非公平锁： 直接创建ReentrantLock对象时，默认情况下是非公平锁。公平锁是按照线程等待的顺序来获取锁的，而非公平锁允许多个线程在同一时刻竞争锁，不考虑他们申请锁的顺序。公平锁和已通过在创建ReentrantLock时传入true来设置。如：

```
ReentrantLock fairLock = new ReentrantLock(true);
```

- 多个条件变量： ReentrantLock支持多个条件变量，每个条件变量可以与一个ReentrantLock关联。这使得线程可以更灵活地等待和唤醒操作，而不是仅仅基于对象监视器的wait()和notify()。多个条件变量的实现依赖于Condition接口，如：

```
ReentrantLock lock = new ReentrantLock();
Condition condition = lock.newCondition();
// 使用下面方法进行等待和唤醒
condition.await();
condition.signal();
```

- 可重入性： ReentrantLock支持可重入性，即同一个线程可以多次获得同一把锁，而不会造成死锁。这是通过内部的holdCount计数来实现的。当一个线程多次获取锁时，holdCount递增，释放锁时递减，只有当holdCount为零时，其他线程才有机会获取锁。

#### synchronized和ReentrantLock应用场景的区别

synchronized：

- 简单同步需求： 当你需要对代码块或方法进行简单的同步控制时，synchronized使用简单，不需要额外的资源管理，因为锁会在方法退出或代码块执行完毕后自动释放。

- 代码块同步： 如果你想对特定代码段进行同步，而不是整个方法，可以用synchronized。这个可以更精细控制同步的范围，减少锁的持有时间，提高并发性能。
- 内置锁的使用： synchroniezd关键字使用对象的内置锁（也称监视器锁），这在需要使用对象锁对象的情况下很有用，尤其是在对象状态与锁保护的代码紧密相关时。

synchronized有以下两种常用用法：

实例方法锁：当一个方法被声明为synchronized时，这个方法会获取实例对象的内置锁。因此，当一个线程调用synchronized实例方法时，其他线程必须等待，直到海鲜城释放对象锁后，才能进入其他使用相同对象锁的synchronized方法。

```
public class Example {
	// 同步整个方法
    public synchronized void method1() {
        // 这个方法是同步的，使用当前对象实例的锁
    }
	// 同步代码块
    public void method2() {
        synchronized(this) { // 使用当前对象实例的锁
            // 这里的代码块是同步的
        }
    }
}
```


代码块锁：在方法内可以使用synchronized块，指定一个对象作为锁。典型的用法是

```
synchronized(this)，表示当前实例对象的锁，也可以使用其他对象作为锁。
public void method() {
    Object lock = new Object(); // 创建一个对象作为锁
    synchronized (lock) {
        // 仅在持有 lock 对象的锁时才能执行的代码
    }
}
```

#### synchronized和reentrantlock区别？

synchronized和ReentrantLock都是java中提供的可重入锁：

- 用法不同： synchronized可以用来修饰普通方法、静态方法和代码块，而ReentrantLock只能用在代码块上。

- 获取锁和释放锁的方式： synchronized会自动加锁和释放锁，当进入synchronized修饰的代码块后自动加锁，离开后会自动释放锁。ReentrantLock需要手动加锁和释放锁。

- 锁类型不同： synchronized属于非公平锁，而ReentrantLock既可以是公平锁，也可以是非公平锁。

- 响应中断不同： ReentrantLock可以响应中断，解决死锁问题，而synchronized不能响应中断。

- 底层实现不同： synchronized是JVM层面通过监视器实现的，而ReentrantLock是基于AQS实现的。

  #### 怎么理解可重入锁？

可重入锁是指同一个线程在获取了锁之后，可以再次重复获取该锁而不会造成死锁或其他问题。当一个线程持有锁时，如果再次尝试获取该锁，就会成功获取而不会被阻塞。

ReentrantLock实现可重入锁的机制是基于线程持有锁的计数器。

- 当一个线程第一次获取锁时，计数器会加1，表示该线程持有了锁。在此之后，如果同一个线程再次获取锁，计数器会再次加1。每次线程成功获取锁时，都会将计数器加1。

- 当线程释放锁时，计数器会相应地减1。只有当计数器减到0时，锁才会完全释放，其他线程才有机会获取锁。
- 这种计数器的设计使得同一个线程可以多次获取同一个锁，而不会造成死锁或其他问题。每次获取锁时，计数器加1；每次释放锁时，计数器减1。只有当计数器减到0时，锁才会完全释放。

ReentrantLock通过这种计数器的方式，实现了可重入锁的机制。它允许同一个线程多次获取同一个锁，并且能够正确地处理锁的获取和释放，避免了死锁和其他并发问题。

#### synchronized 支持重入吗？如何实现的?

synchronized是基于原子性的内部锁机制，是可重入的，因此在一个线程调用synchronized方法的同时在其方法体内部调用该对象另一个synchronized方法，也就是说一个线程得到对象锁后再次去请求该对象锁，是允许的。

synchronized底层是利用计算机系统mutex Lock实现的。**每一个可重入锁都会关联一个线程ID和一个锁状态status**

当一个线程请求方法时，会去检查锁状态：

- 如果锁状态status是0，代表该锁没有被占用，使用CAS操作获取锁，将线程ID替换为自己的线程ID。

- 如果锁状态不是0，表示有线程在访问该方法。如果线程ID是自己的线程ID
- 如果锁是可重入锁，将status+1.然后获取到锁，进而执行相应方法。
- 如果是非重入锁， 则进入阻塞队列等待。
- 此时如果线程ID不是自己的线程ID，则无法访问？（没有⭐️⭐️）

在释放锁时：

- 如果是可重入锁，每一次退出方法，就会将status - 1.直到status值为0，最终释放该锁。

- 如果是非可重入锁，线程退出方法，直接就会释放该锁。

synchronized锁升级的情况讲一下：(不懂⭐️⭐️)

这些锁的状态会根据线程竞争的情况进行升级，以平衡性能和资源消耗。

升级过程是：无锁 -> 偏向锁 -> 轻量级锁 -> 重量级锁

- 无锁：是没有开启偏向锁的状态。在JDK1.6后偏向锁默认开启，但有一个偏向延迟，需要在JVM启动后多少秒后才能开启。这个可以通过JVM参数进行设置，同时是否开启偏向锁也可以通过JVM参数设置。

- 偏向锁：如果还没有线程拿到锁，这个锁状态叫做匿名偏向。当一个线程拿到偏向锁的时候，下次想要竞争锁只需拿线程ID跟MarkWord中存储的线程ID进行比较。如果线程ID相同责直接获取锁（相当于锁偏向这个进程），不需要进行CAS操作和将线程挂起的操作。
- 轻量级锁： 这个状态下线程主要通过CAS实现。将对象的MarkWord存储到线程的虚拟机栈上，然后通过CAS将对象的MarkWord的内容设置为指向Displaced Mark Word的指针。如果如果设置成功则获取锁。在线程出临界区的时候，也需要使用CAS，如果使用CAS替换成功则同步成功，如果失败表示有其他线程在获取锁，那么就需要在释放锁之后将被挂起的线程唤醒。
- 重量级锁： 当有两个以上的线程获取锁的时候轻量级锁就会升级为重量级锁，因为CAS如果没有成功的话始终都在自旋，进行while循环操作，这是非常消耗CPU的，但是在升级为重量级锁之后，线程会被操作系统调度然后挂起，这可以节约CPU资源。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b1a3c77dda1d40f28dc7e943dcdea5d0.png)

线程 A 进入 synchronized 开始抢锁，JVM会判断当前是否是偏向锁的状态，如果是则根据Mark Word中存储的线程ID来判断当前线程A是否是持有偏向锁的线程。如果是则忽略check，线程A直接执行临界区内的代码。

但如果Mark Word中的线程不是线程A，就会通过自选尝试获取锁。如果获取到了，就将Mark Word中的线程ID改为自己的。如果失败了就会撤销偏向锁，膨胀为轻量级锁。

后续的竞争线程都会通过自旋来尝试获取锁。如果自旋成功则锁的状态仍然是轻量级锁。如果竞争失败则锁会膨胀为重量级锁，后续等待竞争的线程都会被阻塞。

#### JVM对synchronized的优化？

- 锁膨胀： synchronized 从无锁升级到偏向锁，再到轻量级锁，最后到重量级锁的过程，它叫做锁膨胀也叫做锁升级。JDK 1.6 之前，synchronized 是重量级锁，也就是说 synchronized 在释放和获取锁时都会从用户态转换成内核态，而转换的效率是比较低的。但有了锁膨胀机制之后，synchronized 的状态就多了无锁、偏向锁以及轻量级锁了，这时候在进行并发操作时，大部分的场景都不需要用户态到内核态的转换了，这样就大幅的提升了 synchronized 的性能。

- **锁消除：**指的是在某些情况下，JVM 虚拟机如果检测不到某段代码被共享和竞争的可能性，就会将这段代码所属的同步锁消除掉，从而到底提高程序性能的目的。
- 锁粗化： 将多个连续的加锁、解锁操作连接在一起，扩展成一个范围更大的锁。
- 自适应自旋锁： 指通过自身循环，尝试获取锁的一种方式，优点在于它避免一些线程的挂起和恢复操作，因为挂起线程和恢复线程都需要从用户态转入内核态，这个过程是比较慢的，所以通过自旋的方式可以一定程度上避免线程挂起和恢复所造成的性能开销。

#### 介绍一下AQS（不懂⭐️⭐️）

AQS全称为AbstractQueuedSynchronizer，是Java中的一个抽象类。 AQS是一个用于构建锁、同步器、协作工具类的工具类（框架）。

AQS核心思想是，如果被请求的共享资源空闲，那么就将当前请求资源的线程设置为有效的工作线程，将共享资源设置为锁定状态；如果共享资源被占用，就需要一定的阻塞等待唤醒机制来保证锁分配。这个机制主要用的是CLH队列的变体实现的，将暂时获取不到锁的线程加入到队列中。

CLH：Craig、Landin and Hagersten队列，是单向链表，AQS中的队列是CLH变体的虚拟双向队列（FIFO），AQS是通过将每条请求共享资源的线程封装成一个节点来实现锁的分配。

原理图：![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b05106f70013482293be79cc7d408ddd.png)

**AQS使用一个Volatile的int类型的成员变量来表示同步状态，通过内置的FIFO队列来完成资源获取的排队工作，通过CAS完成对State值的修改。**

AQS主要完成的任务：

- 同步状态（如计数器）的原子性管理

- 线程的阻塞和解除阻塞
- 队列的管理

#### AQS原理（不懂⭐️⭐️）

AQS最核心的三大部分：

- 状态：state

- 控制线程抢锁和配合的FIFO队列（双向链表）
- 期望协作工具类去实现的获取/释放等重要方法（重写）

**状态state**

- 这里state的具体含义，会根据具体实现类的不同而不同：比如在Semapore里，他表示剩余许可证的数量；在CountDownLatch里，它表示还需要倒数的数量；在ReentrantLock中，state用来表示“锁”的占有情况，包括可重入计数，当state的值为0的时候，标识该Lock不被任何线程所占有。

- state是volatile修饰的，并被并发修改，所以修改state的方法都需要保证线程安全，比如getState、setState以及compareAndSetState操作来读取和更新这个状态。这些方法都依赖于unsafe类。

**FIFO队列**

- 这个队列用来存放“等待的线程，AQS就是“排队管理器”，当多个线程争用同一把锁时，必须有排队机制将那些没能拿到锁的线程串在一起。当锁释放时，锁管理器就会挑选一个合适的线程来占有这个刚刚释放的锁。

- AQS会维护一个等待的线程队列，把线程都放到这个队列里，这个队列是双向链表形式。

**实现获取/释放等方法**

- 这里的获取和释放方法，是利用AQS的协作工具类里最重要的方法，是由协作类自己去实现的，并且含义各不相同；

- 获取方法：获取操作会以来state变量，经常会阻塞（比如获取不到锁的时候）。在Semaphore中，获取就是acquire方法，作用是获取一个许可证； 而在CountDownLatch里面，获取就是await方法，作用是等待，直到倒数结束；

- 释放方法：在Semaphore中，释放就是release方法，作用是释放一个许可证； 在CountDownLatch里面，获取就是countDown方法，作用是将倒数的数减一；

  需要每个实现类重写tryAcquire和tryRelease等方法。

#### Threadlocal作用，原理，具体里面存的key value是啥，会有什么问题，如何解决?

ThreadLocal 是Java中用于解决线程安全问题的一种机制，它允许创建线程局部变量，即每个线程都有自己独立的变量副本，从而避免了线程的资源共享和同步问题。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/830b2245c41e493aa5498981c57c6179.png)

从内存结构图，我们可以看到：

- Thread类中，有个ThreadLocal.ThreadLocalMap的成员变量

- ThreadLocalMap内部维护了Entry数组，每个Entry代表一个完整的对象，key是ThreadLocal本身，value是ThreadLocal的泛型对象值。 （泛型对象值是什么？⭐️）

#### ThreadLocal的作用

- 线程安全： ThreadLocal为每个线程提供了独立的变量副本，这意味着线程之间不会互相影响，可以安全地在多线程环境中使用这些变量而不必担心数据

- 降低耦合度： 在同一个线程内的多个函数或组件之间，使用ThreadLocal可以减少参数的传递，降低代码之间的耦合度，使代码更加清晰和模块化。
- 性能优势： 由于ThreadLocal避免了线程间的同步开销，所以在大量线程并发执行时，相比传统的锁机制，它可以提供更好的性能。

#### ThreadLocal的原理

ThreadLocal的实现依赖于Thread类中的一个ThreadLocalMap字段，这是一个存储ThreadLocal变量本身和对应值的映射。每个线程都有自己的ThreadLocalMap实例，用于存储线程所持有的所有的ThreadLocal变量的值。

当你创建一个ThreadLocal变量时，他实际上就是一个ThreadLocal对象的实例。每个ThreadLocal对象都可以存储任意类型的值，这个值对每个线程来说都是独立的。

- 当调用ThreadLocal的get()方法时，ThreadLocal会检查当前线程的ThreadLocalMap中是否有与之关联的值。

- 如果有，返回该值；

- 如果没有，会调用initialValue()方法（如果重写了的话）来初始化该值，然后将其放入ThreadLocalMap中并返回。

- 当调用set()方法时，ThreadLocal会将给定的值与当前线程关联起来，即在当前线程的ThreadLocalMap中存储一个键值对，键是ThreadLocal对象自身，值是传入的值。

- 当调用remove()方法时，会从当前线程的ThreadLocalMap中移除与该ThreadLocal对象关联的条目。


#### 可能存在的问题

当一个线程结束时，其ThreadLocalMap也会随之销毁，但是ThreadLocal对象本身不会立即被垃圾回收，直到没有其他引用指向他为止。

因此，在使用ThreadLocal时需要注意，如果不显式调用 remove() 方法，或者线程结束时未正确清理ThreadLocal变量，可能会导致内存泄漏。**因为ThreadLocalMap会持续持有ThreadLocal变量的引用，即使这些变量不在被其他地方引用。**

因此，实际应用中需要在使用完ThreadLocal变量后调用remove()方法释放资源

#### Java中想实现一个乐观锁，都有哪些方式？

- CAS（Compare and Swap）操作： CAS 是乐观锁的基础。Java 提供了 java.util.concurrent.atomic 包，包含各种原子变量类（如 AtomicInteger、AtomicLong），这些类使用 CAS 操作实现了线程安全的原子操作，可以用来实现乐观锁。

- 版本号控制： 增加一个版本号字段记录数据更新时候的版本，每次更新时递增版本号。在更新数据时，同时比较版本号，若当前版本号和更新前获取的版本号一致，则更新成功，否则失败。
- 时间戳： 使用时间戳记录数据的更新时间，在更新数据时，在比较时间戳。如果当前时间戳大于数据的时间戳，则说明数据已经被其他线程更新，更新失败。

#### CAS 有什么缺点？

**ABA问题**： ABA的问题指的是在CAS更新的过程中，当读取到的值是A，然后准备赋值的时候仍然是A，但是实际上有可能A的值被改成了B，然后又被改回了A，这个CAS更新的漏洞就叫做ABA。只是ABA的问题大部分场景下都不影响并发的最终效果。Java中有AtomicStampedReference来解决这个问题，他加入了预期标志和更新后标志两个字段，更新时不光检查值，还要检查当前的标志是否等于预期标志，全部相等的话才会更新。
**循环时间长开销大**： 自旋CAS的方式如果长时间不成功，会给CPU带来很大的开销。
**只能保证一个共享变量的原子操作**： 只对一个共享变量操作可以保证原子性，但是多个则不行，多个可以通过AtomicReference来处理或者使用锁synchronized实现。

#### 为什么不能所有的锁都用CAS？

CAS操作是基于循环重试的机制。如果CAS操作一直未能成功，线程会一直自旋重试，占用CPU资源。在高并发情况下，大量线程自旋会导致CPU资源浪费。

#### voliatle关键字有什么作用？

volatite作用有 2 个：

- 保证变量对所有线程的可见性。 当一个变量被声明为volatile时，它会保证对这个变量的写操作会立即刷新到主存中，而对这个变量的读操作会直接从主存中读取，从而确保了多线程环境下对该变量访问的可见性。这意味着一个线程修改了volatile变量的值，其他线程能够立刻看到这个修改，不会受到各自线程工作内存的影响。

- 禁止指令重排序优化。 volatile关键字在Java中主要通过内存屏障来禁止特定类型的指令重排序。（不懂⭐️⭐️）

​	1）写-写（Write-Write）屏障：在对volatile变量执行写操作之前，会插入一个写屏障。这确保了在该变量写操作之前的所有普通写操作都已完成，防止了这些写操作被移到volatile写操作之后。

​	2）读-写（Read-Write）屏障：在对volatile变量执行读操作之后，会插入一个读屏障。它确保了对volatile变量的读操作之后的所有普通读操作都不会被提前到volatile读之前执行，保证了读取到的数据是最新的。

​	3）写-读（Write-Read）屏障：这是最重要的一个屏障，它发生在volatile写之后和volatile读之前。这个屏障确保了volatile写操作之前的所有内存操作（包括写操作）都不会被重排序到volatile读之后，同时也确保了volatile读操作之后的所有内存操作（包括读操作）都不会被重排序到volatile写之前。

#### 指令重排序的原理是什么？

在执行程序时，为了提高性能，处理器和编译器常常会对指令进行重排序，但是重排序要满足下面2个条件才能进行：

- 在单线程环境下不能改变程序运行的结果

- 存在数据依赖关系的不允许重排序。

所以重排序不会对单线程有影响，只会破坏多线程的执行语义。
我们看这个例子，A和C之间存在数据依赖关系，同时B和C之间也存在数据依赖关系。因此在最终执行的指令序列中，C不能被重排序到A和B的前面，如果C排到A和B的前面，那么程序的结果将会被改变。但A和B之间没有数据依赖关系，编译器和处理器可以重排序A和B之间的执行顺序。

![img](https://i-blog.csdnimg.cn/direct/dcd5987a821b4e4582e19cf471daddeb.png)

#### volatile可以保证线程安全吗？

volatile关键字**可以保证可见性，但不能保证原子性，因此不能完全保证线程安全。volatile**关键字用于修饰变量，当一个线程修改了volatile修饰的变量的值，其他线程能够立即看到最新的值，从而避免了线程之间的数据不一致。
但是，volatile并不能解决多线程并发下的复合操作问题。比如i++这种操作不是原子操作。如果多个线程同时对i进行自增操作，volatile不能保证线程安全。对于复合操作，需要使用synchronized关键字或者Lock来保证原子性和线程安全。

#### volatile和sychronized比较？

**Synchronized解决了多线程访问共享资源时可能出现的竞争条件和数据不一致问题，保证了线程安全性。**
**Volatile解决了变量在多线程环境下的可见性和有序性问题，确保了变量的修改对其他线程是可见的。**

**Synchronized: Synchronized是一种排他性的同步机制，保证了多个线程访问共享资源时的互斥性，即同一时刻只允许一个线程访问共享资源。通过对代码块或方法添加Synchronized关键字来实现同步。**
**Volatile: Volatile是一种轻量级的同步机制，用来保证变量的可见性和禁止指令重排序。当一个变量被声明为Volatile时，线程在读取该变量时会直接从内存中读取，而不会使用缓存，同时对该变量的写操作会立即刷回主内存，而不是缓存在本地内存中。**

#### 非公平锁吞吐量为什么比公平锁大？

- 公平锁执行流程： 获取锁时，先将线程自己添加到等待队列的队尾并休眠，当某线程用完锁之后，会去唤醒等待队列中队首的线程尝试去获取锁，锁的使用顺序也就是队列中的先后顺序，在整个过程中，线程会从运行状态切换到休眠状态，再从休眠状态恢复成运行状态，但线程每次休眠和恢复都需要从用户态转换成内核态，而这个状态的转换是比较慢的，所以公平锁的执行速度会比较慢。（线程休眠和运行状态转换会在内核态和用户态转换）

- 非公平锁执行流程： 当线程获取锁时，会先通过 CAS 尝试获取锁，如果获取成功就直接拥有锁，如果获取锁失败才会进入等待队列，等待下次尝试获取锁。这样做的好处是，获取锁不用遵循先到先得的规则，从而避免了线程休眠和恢复的操作，这样就加速了程序的执行效率。

  #### **上面说的通过CAS尝试获取锁，是什么意思？**

在并发编程中，“CAS”代表“比较并交换”（Compare-And-Swap），这是一种用于实现线程安全的原子操作。CAS 操作涉及三个基本元素：内存位置的值
𝑉、预期原值 𝐴 和新值 𝐵。在执行操作时，CAS 会自动比较内存位置的当前值与预期原值 𝐴是否相等：如果内存位置的当前值 𝑉等于预期原值 𝐴，那么处理器会将这个位置的值更新为新值 𝐵，并返回一个指示成功的响应。如果内存位置的当前值 𝑉 不等于预期原值 𝐴，则不会执行任何更新，而是返回一个指示失败的响应。

在锁的获取过程中，线程通过使用 CAS 操作来尝试更新锁的状态。例如，如果锁是用一个标志位表示，那么线程会尝试将这个标志位从“未锁定”状态（如 0）更新为“锁定”状态（如 1）。这个尝试是通过执行一个 CAS 操作来完成的：

如果 CAS 成功（即锁的状态确实是从未锁定到锁定），这意味着线程成功获取了锁，可以继续执行其临界区的代码。
如果 CAS 失败（即锁的状态已经是锁定），则说明另一个线程已经持有锁，当前线程通常会被放入一个等待队列，并在稍后重新尝试。

使用 CAS 获取锁的优势在于减少了线程上下文切换的开销。因为在锁的竞争不是特别激烈的情况下，线程可能在第一次尝试时就成功获取到锁，无需切换到内核模式、进行线程挂起与恢复等繁琐操作。这样可以显著提高并发性能，特别是在高性能计算或实时系统中。CAS 也是很多现代锁机制和无锁编程技术的基础。

#### Synchronized不属于公平锁，ReentrantLock是公平锁。

---------

### 3、线程池

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/12e5164f6ce340ba83210ba15b3ded28.png)

线程池分为核心线程池、线程池的最大容量、等待任务的队列。提交一个任务，如果核心线程没有满，就创建一个线程。如果满了就加入等待队列。如果等待队列满了就会增加线程，如果达到最大线程数量就会执行一些丢弃策略。

#### 线程池的参数有哪些？

线程池的构造函数有7个参数：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e42cb1280c6d42c784342c4899c6345a.png)

- 
  corePoolSize： 线程池核心线程数量。默认情况下，线程池中的线程数量 <= corePoolSize，则即使这些线程处于空闲状态，那也不会被销毁。

- maximumPoolSize： 线程池中最大可容纳的线程数量。当一个新任务交给线程池时，如果此时线程池中有空闲的线程，就会直接执行。如果没有空闲线程且当前线程池的线程数量<corePoolSize，就会创建一个新线程，从阻塞队列头部取出一个任务来执行，并将新任务加入到阻塞队列末尾。如果当前线程池中线程数量=maximumPoolSize，就不会创建新的线程，就回去执行拒绝策略。
- keepAliveTime： 当线程池中线程的数量> corePoolSize，并且某个线程的空闲时间超过了keepAliveTime，那么这个线程就会被销毁。
- unit： 就是keepAliveTime时间的单位。
- workQueue： 工作队列。当没有空闲的线程执行新任务时，该任务就会被放入工作队列中，等待执行。
- threadFactory： 线程工厂。可以用于给线程取名字。
- handler： 拒绝策略。当一个新任务交给线程池，如果此时线程池中有空闲的线程，就会直接执行，如果没有空闲的线程，就会将该任务加入阻塞队列。如果阻塞队列满了，就会创建一个新线程，从阻塞队列头部取出一个任务来执行，并将新任务加入阻塞队列末尾。如果当前线程池中线程的数量=maximumPoolSize，就不会创建新线程，就会去执行拒绝策略。

#### 线程池工作队列满了有哪些拒接策略？

当线程池的任务队列满了之后，线程池会执行指定的拒绝策略来应对，常用的四种拒绝策略包括：CallerRunsPolicy、AbortPolicy、DiscardPolicy、DiscardOldestPolicy，此外，还可以通过实现RejectedExecutionHandler接口来自定义拒绝策略。

- CallerRunsPolicy，使用线程池的调用者所在的线程去执行被拒绝的任务，除非线程池被停止或者线程池的任务队列已有空缺。

- AbortPolicy，直接抛出一个任务被线程池拒绝的异常。
- DiscardPolicy，不做任何处理，静默拒绝提交的任务。
- DiscardOldestPolicy，抛弃最老的任务，然后执行该任务。

- 自定义拒绝策略，通过实现接口可以自定义任务拒绝策略。

  #### 有线程池参数设置的经验吗？

- CPU密集型：corePoolSize = CPU核数 + 1

- IO密集型：corePoolSize = CPU核数 * 2

#### 线程池种类有哪些？

- ScheduledThreadPool： 可以设置定期的执行任务，它支持定时或周期性执行任务，如每隔10s执行一次任务，通过该实现类设置定期执行任务的策略。

- FixedThreadPool： 它的核心线程数和最大线程数一样，因此可以将其看做固定线程数的线程池。他的特点是线程池中的线程数除了初始阶段需要从0开始增加外，之后的线程数量就是固定的，就算任务数超过线程数，线程池也不会再多创建更多线程来处理任务，而会把超出线程处理能力外的任务放到任务队列中等待。就算任务队列满了到了本该继续增加线程数的时候，由于其最大线程数和核心线程数是一样的，所以也无法再增加新的线程了。
- CachedThreadPool： 可以称作可缓存线程池。它的特点在于线程数几乎可以无限增加（实际最大到Integer.MAX_VALUE，即231-1），而且当线程闲置时还可以对线程进行回收。也就是该线程池的线程数量不是固定不变的，当然它也有一个用于存储提交任务的队列，但这个队列是 SynchronousQueue，队列的容量为0，实际不存储任何任务，它只负责对任务进行中转和传递，所以效率比较高。
- SingleThreaedExecutor： 它会使用唯一的线程去执行任务，原理和FixedThreadPool是一样的，只不过这里线程只有1个，如果线程在执行任务的过程中发生异常，线程池也会重新创建一个线程来执行后续的任务。这种线程池由于只有一个线程，所以非常适合用于所有任务都需要按被提交的顺序依次执行的场景，而前几种线程池不一定能够保障任务的执行顺序等于被提交的顺序，因为它们是多线程并行执行的。
- SingleThreadScheduledExecutor： 它实际和 ScheduledThreadPool 线程池非常相似，它只是 ScheduledThreadPool 的一个特例，内部只有一个线程。

#### 线程池一般是怎么用的？

禁止使用new FixedThreadPool()和new CachedThreadPool，因为可能因为资源耗尽导致OOM。应该手动new ThreadPoolExecutor创建线程池。

#### 线程池中shutdown ()，shutdownNow()这两个方法有什么作用？

- shutdown使用后会置状态为SHUTDOWN，正在执行的任务会继续执行下去，没有被执行的则中断。此时不能往线程池中添加新任务，否则会抛出RejectedExecutionException异常。
- shutdownNow为STOP，他试图终止线程的方法是通过调用Thread.interrupt()方法来实现，但是这个方法的作用有限，如果线程中没有sleep, wait, condition, 定时锁等应用，interrupt()方法是无法中断当前的线程的。所以，shutdownNow并不代表线程池就一定会立即退出，他可能必须要等到所有正在执行的任务都执行完成了才能退出。

------------

## 四、Spring面试篇

### 1、Spring

说一下你对 Spring 的理解

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/dfc4ffd02fd544eb8543782a8367e939.png)


Spring框架核心特征包括：

- IoC容器： Spring通过控制反转实现了 对象的创建和对象间依赖关系管理。开发者只需要定义好Bean及其依赖关系，Spring容器负责创建和组装这些对象。

- AOP： 面向切面编程，允许开发者定义横切关注点，如事务管理、安全控制等，独立于业务逻辑的代码。通过AOP，可以将这些关注点模块化，提高代码的可维护性和可重用性。
- 事务管理： Spring提供了一致的事务管理接口，支持声明式和编程式事务。开发者可以轻松进行事务管理，而无需关心具体的事务API。添加@Transactional注解
- MVC框架： Spring MVC是一个基于Servlet API构建的Web框架，采用 模型-视图-控制器（MVC）架构。它支持灵活的URL到页面控制器的映射，以及多种视图技术。

#### Spring IoC和AOP 介绍一下

Spring IoC和AOP 区别：

- IoC： 即控制反转的意思，它是一种创建和获取对象的技术思想，依赖注入(DI)是实现这种技术的一种方式。传统开发过程中，我们需要通过new关键字来创建对象。使用IoC思想开发方式的话，我们不通过new关键字创建对象，而是通过IoC容器来帮我们实例化对象。 通过IoC的方式，可以大大降低对象之间的耦合度。

- AOP： 是面向切面编程，能够将那些与业务无关，却为业务模块所共同调用的逻辑封装起来，以减少系统的重复代码，降低模块间的耦合度。Spring AOP 就是基于动态代理的，如果要代理的对象，实现了某个接口，那么 Spring AOP 会使用 JDK Proxy，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候 Spring AOP 会使用 Cglib 生成一个被代理对象的子类来作为代理。

在 Spring 框架中，IOC 和 AOP 结合使用，可以更好地实现代码的模块化和分层管理。例如：

- 通过 IOC 容器管理对象的依赖关系，然后通过 AOP 将横切关注点统一切入到需要的业务逻辑中。

- 使用 IOC 容器管理 Service 层和 DAO 层的依赖关系，然后通过 AOP 在 Service 层实现事务管理、日志记录等横切功能，使得业务逻辑更加清晰和可维护。

#### Spring的aop介绍一下（不懂⭐️⭐️⭐️）

Spring AOP是Spring框架中的一个重要模块，用于实现面向切面编程。

在AOP中最小的单元是切面。一个“切面”可以包含很多种类型和对象，对它们进行模块化管理，如事务管理。

在面向切面编程的思想里，将功能分为两种：

- 核心业务： 登录、注册、增、删、改、查都叫核心业务

- 周边功能： 日志、事务管理这些为次要的周边业务。

在面向切面编程中，核心业务功能和周边功能分别独立进行开发，两者是不耦合的，然后把切面功能和核心业务功能“编织”在一起，这就叫AOP

AOP能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（如事务管理、日志管理、权限控制等） 封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

在AOP中有以下几个概念：

- AspectJ： 切面。只是概念没有具体接口或类。是Join point，Advice，Pointcut的统称

- Join point： 连接点，指程序执行过程中的一个点，例如 方法调用、异常处理。在Spring AOP中，仅支持方法级别的连接点。
- Advice： 通知，即定义的一个切面中的横切逻辑，有around、before、after三种类型。在很多的AOP实现框架中，Advice通常作为一个拦截器，也可以包含多个拦截器作为一条链路，围绕着Joint point进行处理。
- Pointcut： 切点。用于匹配连接点，一个AspectJ中包含哪些连接点Joint point需要由Pointcut进行筛选。
- Introduction： 引介，让一个切面可以声明被通知的对象，实现他们没有真正实现的额外的接口。例如，可以让一个代理对象代理两个目标类。❓
- Weaving： 织入。有了连接点、切点、通知以及切面，如何将其应用到程序中？ 通过织入。在切点的引导下，将通知逻辑插入到目标方法上，使得我们的通知逻辑在方法调用时得以执行。
- AOP proxy： AOP代理，指在AOP实现框架中实现切面协议的对象。在Spring AOP中有两种代理，分别是JDK动态代理和CGLIB动态代理。
- Target object： 目标对象，就是被代理的对象。

Spring AOP是基于JDK动态代理和cglib提升的，两种代理方式都属于运行时的一种方式，所以没有编译时的处理，因此spring是通过java实现的。

#### Spring IOC 是通过什么机制来实现的？

- 反射： Spring IOC容器利用Java的反射机制动态地加载类、创建对象实例以及调用对象方法。反射允许在运行时检查类，方法，属性等信息，从而实现灵活的对象实例化和管理。

- 依赖注入： IOC的核心概念是依赖注入，即容器负责管理应用程序组件之间的依赖关系。Spring通过构造函数注入、属性注入和方法注入，将组件之间的依赖关系描述在配置文件中或使用注解。
- 设计模式-工厂模式 Spring IOC容器通常采用工厂模式来管理对象的创建和生命周期。容器作为工厂负责实例化Bean并管理他们的生命周期，将Bean的实例化过程交给容器来管理。
- 容器实现： Spring IOC容器是实现IOC的核心，通常使用BeanFactory或ApplicationContext来管理Bean。BeanFactory是IOC容器的基本形式，提供基本的IOC功能。ApplicationContext是BeanFactory的扩展，并提供更多企业级功能。

#### Spring AOP实现机制

Spring AOP的实现依赖于动态代理技术。动态代理是在运行时动态生成代理对象，而不是在编译时。它允许开发者在运行时指定要代理的接口和行为，从而实现在不修改源码情况下增强方法的功能。

Spring AOP支持两种动态代理：

- 基于JDK的动态代理： 使用java.lang.reflect.Proxy类和java.lang.reflect.InvocationHandler接口实现。这种方式需要代理的类实现一个或多个接口。

- 基于CGLIB的动态代理： 当被代理的类没有实现接口时，Spring会使用CGLIB库生成一个被代理的子类作为代理。CGLIB（Code Generation Library）是一个第三方代码生成库，通过继承方式实现代理。

#### 怎么理解SpringIoc？

IOC：Inversion Of Control，即控制反转，是一种设计思想。在传统的 Java SE 程序设计中，我们直接在对象内部通过 new 的方式来创建对象，是程序主动创建依赖对象；

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ef0b5d37cdc8497bafc4c3aab659e806.png)

而在Spring程序设计中，IOC 是有专门的容器去控制对象。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b9a024c8e4154b748d8f0d48ab3303d5.png)

所谓控制就是对象的创建、初始化、销毁。

- 创建对象：原来是 new 一个，现在是由 Spring 容器创建。
- 初始化对象：原来是对象自己通过构造器或者 setter 方法给依赖的对象赋值，现在是由 Spring 容器自动注入。
- 销毁对象：原来是直接给对象赋值 null 或做一些销毁操作，现在是 Spring 容器管理生命周期负责销毁对象。

总结：IOC 解决了繁琐的对象生命周期的操作，解耦了我们的代码。所谓反转：其实是反转的控制权，前面提到是由 Spring 来控制对象的生命周期，那么对象的控制就完全脱离了我们的控制，控制权交给了 Spring 。这个反转是指：我们由对象的控制者变成了 IOC 的被动控制者。

#### 依赖倒置，依赖注入，控制反转分别是什么？

- 控制反转：“控制”指的是对程序执行流程的控制，而“反转”指的在没有使用框架之前，程序员自己控制整个程序的执行。在使用框架之后，整个程序的执行流程通过框架来控制。流程的控制权从程序员“反转”给了框架。

- 依赖注入：依赖注入和控制反转恰恰相反，它是一种具体的编码技巧。我们不通过new的方式在类内部创建依赖类的对象，而是将依赖的类对象在外部创建好后，通过构造函数、函数参数等方式传递（或注入）给类来使用。
- 依赖倒置：这条原则和控制反转有点类似，主要用于知道框架层面的设计。高层模块不依赖低层模块，他们共同依赖同一个抽象。抽象不要依赖具体实现细节，具体实现细节依赖抽象。

#### 如果让你设计一个SpringIoc，你觉得会从哪些方面考虑这个设计？（记不住⭐️⭐️⭐️）

- Bean的生命周期管理：需要设计Bean的创建、初始化、销毁等生命周期管理机制，可以考虑使用工厂模式和单例模式来实现。

- 依赖注入：需要实现依赖注入的功能，包括属性注入、构造函数注入、方法注入等，可以考虑使用反射机制和XML配置文件来实现。
- Bean的作用域：需要支持多种Bean作用域，比如单例、原型、会话、请求等，可以考虑使用Map来存储不同作用域的Bean实例。
- AOP功能的支持：需要支持AOP功能，可以考虑使用动态代理机制和切面编程来实现。
- 异常处理：需要考虑异常处理机制，包括Bean创建异常、依赖注入异常等，可以考虑使用try-catch机制来处理异常。
- 配置文件加载：需要支持从不同的配置文件中加载Bean的相关信息，可以考虑使用XML、注解或者Java配置类来实现。

#### SpringAOP主要想解决什么问题

它的目的是对于面向对象思维的一种补充，而不是像引入命令式、函数式编程思维让他顺应另一种开发场景。在我个人的理解下AOP更像是一种对于不支持多继承的弥补，除开对象的主要特征（我更喜欢叫“强共性”）被抽象为了一条继承链路，对于一些“弱共性”，AOP可以统一对他们进行抽象和集中处理。

举一个简单的例子，打印日志。需要打印日志可能是许多对象的一个共性，这在企业级开发中十分常见，但是日志的打印并不反应这个对象的主要共性。而日志的打印又是一个具体的内容，它并不抽象，所以它的工作也不可以用接口来完成。而如果利用继承，打印日志的工作又横跨继承树下面的多个同级子节点，强行侵入到继承树内进行归纳会干扰这些强共性的区分。

这时候，我们就需要AOP了。AOP首先在一个Aspect（切面）里定义了一些Advice（增强），其中包含具体实现的代码，同时整理了切入点，切入点的粒度是方法。最后，我们将这些Advice织入到对象的方法上，形成了最后执行方法时面对的完整方法。![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f0508d3a233d42d388f81ca7e4a7d3ec.png)

#### SpringAOP的原理了解吗

Spring AOP 的实现依赖于**动态代理技术**。动态代理是在运行时动态生成代理对象，而不是在编译时。它允许开发者在运行时指定要代理的接口和行为，从而实现在不修改源码的情况下增强方法的功能。

Spring AOP支持两种动态代理：

- 基于JDK的动态代理： 使用java.lang.reflect.Proxy类和java.lang.reflect.InvocationHandler接口实现。这种方式需要代理的类实现一个或多个接口。
- 基于CGLIB的动态代理： 当被代理的类没有实现接口时，Spring会使用CGLIB库生成一个被代理类的子类作为代理。CGLIB（Code Generation Library）是一个第三方代码生成库，通过继承方式实现代理。

#### 动态代理是什么？

Java的动态代理是一种在运行时动态创建代理对象的机制，主要用于在不修改原始类的情况下对方法调用进行拦截和增强。

Java动态代理主要分为两种类型：

- 基于接口的代理（JDK动态代理）： 这种类型的代理要求代理对象必须至少实现一个接口。Java动态代理会创建一个实现了相同接口的代理类，然后在运行时动态生成该代理类的实例。这种代理的实现核心是java.lang.reflect.Proxy和java.lang.reflect.InvocationHandler接口。每一个动态代理类都必须实现InvocationHandler接口，并且每个代理类的实例都关联到一个handler。当通过代理对象调用一个方法时，这个方法的调用会被转发由Invocationhandler接口invoke()方法来调用。

- 基于类的代理（CGLIB动态代理）： CGLIB（Code Generation Library）是一个强大的高性能代码生成库，它可以在运行时动态生成一个目标类的子类。CGLIB代理不需要目标类实现接口，而是通过继承的方式创建代理类。因此如果目标对象没有实现任何接口，可以使用CGLIB来创建动态代理。

#### 动态代理和静态代理的区别：

代理是一种常见的设计模式。目的是：为其他对象提供一个代理，以控制对某个对象的访问，将两个类的关系解耦。代理类和委托类都要实现相同的接口，因为代理真正调用的是委托类的方法。

区别：

- 静态代理：由程序员创建或者由特定工具创建，在代码编译时就确定了被代理的类是一个静态代理。静态代理通常只代理一个类。

- 动态代理：在代码运行期间，运用反射机制动态创建生成。动态代理，代理的是一个接口下的多个实现类。

#### AOP实现有哪些注解？

常见的注解包括：

- @Aspect：用于定义切面，标注着切面类上。

- @Pointcut：定义切点，标注着方法上，用于指定连接点。
- @Before：在方法执行前执行通知。
- @After：在方法执行后执行通知。
- @Around：在方法执行前后都执行通知。
- @AfterReturning：在方法执行后返回结果后执行通知。
- @AfterThrowing：在方法抛出异常后执行通知。
- @Advice：通用的通知类型：可以替代@Before，@After。

#### 什么是反射，有哪些使用场景？

反射具有一下特性：

- 运行时类信息访问： 反射机制允许程序在运行时获取类的完整结构信息，包括类名、包名、父类、实现的接口、构造函数、方法和字段等。

- 动态对象创建： 可以使用反射API动态地创建对象实例，即使在编译时不知道具体的类名。这是通过Class类的newInstance()方法或Constructor对象的newInstance()方法实现的。
- 动态方法调用： 可以在运行时动态地调用对象的方法，包括私有方法。这通过Method类的invoke()方法实现，允许你传入对象实例和参数值来执行方法。
- 访问和修改字段值。 反射还允许程序在运行时访问和修改对象的字段值，即使是私有的。这是通过Field类的get()和set()方法完成的。![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a45078fbb6c04eff87b6a437dd2a44e3.png)

#### **Spring框架的依赖注入（DI）和控制反转**

Spring使用反射来实现其核心特征：依赖注入
在Spring中，开发者可以通过XML配置文件或者基于注解的方式声明组件之间的依赖关系。当应用程序启动时，Spring容器会扫描这些配置或注解，然后利用反射来实例化Bean（即Java对象），并根据配置自动装配他们的依赖。

例如，当一个Service类依赖另一个DAO类时，开发者可以在Service类中使用@Autowired注解，而无需自己编写创建DAO实例的代码。Spring容器在运行时解析这个注解，通过反射找到对应的DAO类，实例化它，并将其注入到Service类中。这样不仅降低了组件之间的耦合度，也极大增强了代码的可维护性和可测试性。

#### **动态代理的实现：**

在需要对现有类的方法调用进行拦截、记录日志、权限控制或是事务管理，反射结合动态代理技术被广泛应用。

一个典型的例子是Spring AOP（面向切面编程）。Spring AOP允许开发者定义切面（Aspect），这些切面可以横切关注点（如日志记录，事务管理），并将其插入到业务逻辑中，而不需要修改业务逻辑的代码。

例如，为了给所有的服务层方法添加日志记录功能，可以定义一个切面，在这个切面中，Spring会使用JDK动态代理或CGLIB（如果目标类没有实现接口）来创建目标类的代理对象。这个代理对象在调用任何方法前或后，都会执行切面中定义的代码逻辑（如记录日志），**而这一切都是在运行时通过反射来动态构建和执行的，无需硬编码到每个方法调用中。**

这两个例子展示了反射机制如何在实际工程中促进松耦合、高内聚的设计，以及如何提供动态、灵活的编程能力，特别是在框架层面和解决跨切面问题时

#### spring是如何解决循环依赖的？（不完全懂⭐️⭐️⭐️）

循环依赖指的是两个类中的属性相互依赖对方：例如A类中有B属性，B类中有A属性，从而形成了一个依赖闭环。如下图：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e9e73e9640904968b099e2879de23473.png)

循环依赖问题在Spring中主要有三种情况：

- 第一种：通过构造函数进行依赖注入时产生的循环依赖问题。

- 第二种：通过setter方法进行依赖注入且是在多例（原型）模式下产生的循环依赖问题。
- 第三种：通过setter方法进行依赖注入且是在单例模式下产生的循环依赖问题。

只有「第三种方式」的循环依赖问题被Spring解决了，其他两种方式在遇到循环依赖问题时，Spring都会产生异常。

Spring解决单例模式下的setter循环依赖问题的主要方式是通过三级缓存解决循环依赖。三级缓存指的是Spring在创建Bean的过程中，通过三级缓存来缓存正在创建的Bean，以及已经创建完成的Bean实例。具体步骤如下：

- 实例化Bean： Spring在实例化Bean时，会先创建一个空的Bean对象，并将其放入一级缓存中。

- 属性赋值： Spring开始对Bean进行属性赋值，如果发现循环依赖，会将当前Bean对象提前暴露给后续需要依赖的Bean（通过提前暴露的方式解决循环依赖）
- 初始化Bean： 完成属性赋值后，Spring将Bean进行初始化，并将其放入二级缓存中。
- 注入依赖： Spring继续对Bean进行依赖注入，如果发现循环依赖，会从二级缓存中获取已经完成初始化的Bean实例。

通过三级缓存机制，Spring能够在处理循环依赖时，确保及时暴露正在创建的Bean对象，并能够正确地注入已经初始化的Bean实例，从而解决循环依赖问题，保证应用程序的正常运行。

#### Spring三级缓存的数据结构是什么？

都是Map类型的缓存。比如Map {k:name, v:bean}。

- 一级缓存（Singleton Objects）： 这是一个Map类型的缓存，存储的是已经完全初始化好的Bean，即完全准备好可以使用的bean实例。键是bean的名称，值是bean的实例。这个缓存在DefaultSingletonBeanRegistry类中的singletonObjects属性中。

- 二级缓存（Early Singleton Objects）： 这同样是一个Map类型的缓存，存储的是早期的bean引用，即已经实例化但未完全初始化的bean。这些bean已经被实例化，但是可能还没有进行属性注入等操作。这个缓存在DefaultSingletonBeanRegistry类中的earlySingletonObjects属性中。
- 三级缓存（Singleton Factories）： 这也是一个Map类型的缓存，存储的是ObjectFactory对象。这些对象可以生成早期的bean引用。当一个bean’正在创建过程中，如果它被其他bean依赖，那么这个正在创建的bean’就会通过这个ObjectFactory来创建一个早期引用，从而解决循环依赖问题。 这个缓存在DefaultSingletonBeanRegistry类中的singletonFactories属性中。

#### Spring框架中有那个到了哪些设计模式？

- 工厂设计模式： Spring使用工厂模式通过BeanFactory、ApplicationContext创建Bean对象。

- 代理设计模式： Spring AOP功能的实现。
- 单例设计模式： Spring中的Bean默认都是单例的。
- 模板方法模式： Spring中jdbcTemplate、hibernate template等以Template结尾的对数据库操作的类，他们就使用到了模板模式。
- 包装器设计模式： 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- 观察者模式： Spring时间驱动模型就是观察者模式很经典的一个应用。
- 适配器模式： Spring AOP的增强或通知（Advice）使用到了适配器模式、Spring MVC中也是用到了适配器模式适配Controller。

#### spring 常用注解有什么？

@**Autowired** 注解
@Autowired：主要用于自动装配bean。当Spring容器中存在与要注入的属性类型匹配的bean时，它会自动将bean注入到属性中。就像我们new对象一样。

@**Component**
用于标记一个类作为Spring的bean。当一个类被@Component注解标记时，Spring会将其实例化为一个Bean，并将其添加到Spring容器中。

@**Configuration**
@Configuration注解用于标记一个类作为Spring的配置类。配置类可以包含@Bean注解的方法，用于定义和配置bean，作为全局配置。示例如下：

```
@Configuration
public class MyConfiguration {

    @Bean
    public MyBean myBean() {
        return new MyBean();
    }

}
```

@**Bean**
@Bean注解用于标记一个方法作为Spring的bean工厂方法。当一个方法被@Bean注解标记时，Spring会将该方法的返回值作为一个bean，并将其添加到Spring容器中。 如果自定义配置，经常会用到这个注解。

```
@Configuration
public class MyConfiguration {
    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}
```


@**Service**
@Service注解用于标记一个类作为服务层的组件。它是@Component注解的特例，用于标记服务层的bean，一般标记在业务service的实现类！！（ServiceImpl！！）

```
@Service
public class MyServiceImpl {

}
```


@**Repository**
@Repository用于标记一个类作为数据访问层的组件。它也是@Component注解的特例，用于标记数据访问层的bean。（其实就是@Mapper，只不过@Mapper属于是mybatis的注解。）

@**Controller**
@Controller注解用于标记一个类作为控制层的组件。它也是@Component注解的特例，用于标记控制层的bean。这是MVC结构的另一个部分，加在控制层

#### Spring的事务什么情况下会失效？（又要背啊啊啊啊⭐️⭐️⭐️）

Spring Boot通过Spring框架的事务管理模块来支持事务操作。事务管理在Spring Boot中通常是通过@Transactional注解来实现的。事务失效的情况：

- 未捕获异常： 如果事务发生了未捕获异常，且异常未被处理或传播到事务边界之外，那事务会失效，所有的数据库操作都会回滚。

- 非受检异常（RuntimeException或其子类）： 如果出现了非受检异常（RuntimeException或其子类），事务会回滚。
- 事务传播属性设置不当： 如果多个事务之间存在事务嵌套，且事务传播属性配置不正确，可能导致事务是下。特别是在方法内部调用有@Transactional注解的方法时要特别注意。
- 多数据源的事务管理： 如果在使用多数据源时，事务管理没有正确配置或存在多个@Transcational注解时，可能会导致事务失效。
- 跨方法调用事务问题： 一个@Transactional调用一个没有@Transactional注解的方法，那外层事务也可能会失效。
- 事务在非公开方法中失效： 如果@Transactional注解标注着私有方法上或非public方法上，事务也会失效。

#### Spring的事务，使用this调用是否生效？

不能生效。

因为Spring事务是通过代理对象来控制的，只有通过代理对象 的方法调用才会引用事务管理的相关规则。当使用this直接调用时，是绕过了Spring的代理机制，因此不会应用事务设置。

#### Bean的生命周期说一下？（记不住一点⭐️⭐️）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7865f834c92c4b258495a19f6515b71b.png)

1. Spring启动，查找并加载需要被Spring管理的bean，进行Bean的实例化

2. Bean实例化后对将Bean的引入和值注入到Bean的属性中
3. 如果Bean实现了BeanNameAware接口的话，Spring将Bean的Id传递给setBeanName()方法
4. 如果Bean实现了BeanFactoryAware接口的话，Spring将调用setBeanFactory()方法，将BeanFactory容器实例传入
5. 如果Bean实现了ApplicationContextAware接口的话，Spring将调用Bean的setApplicationContext()方法，将bean所在应用上下文引用传入进来。
6. 如果Bean实现了BeanPostProcessor接口，Spring就将调用他们的postProcessBeforeInitialization()方法。
7. 如果Bean 实现了InitializingBean接口，Spring将调用他们的afterPropertiesSet()方法。类似的，如果bean使用init-method声明了初始化方法，该方法也会被调用
8. 如果Bean 实现了BeanPostProcessor接口，Spring就将调用他们的postProcessAfterInitialization()方法。
9. 此时，Bean已经准备就绪，可以被应用程序使用了。他们将一直驻留在应用上下文中，直到应用上下文被销毁。
10. 如果bean实现了DisposableBean接口，Spring将调用它的destory()接口方法，同样，如果bean使用了destory-method 声明销毁方法，该方法也会被调用。

#### Bean是否单例？

Spring中的Bean默认都是单例的。

即，每个Bean的实例只会被创建一次，并且会被存储在Spring容器的缓存中，以便在后续的请求中重复使用。这种单例模式可以提高应用程序的性能和内存效率。

但是Spring也支持将Bean设置为多例，即每次请求都会创建一个新的Bean实例。要将Bean设置为多例模式，可以在Bean定义中通过设置scope属性为prototype来实现。

需要注意的是，虽然默认是Bean单例模式，但在一些情况下，使用多例模式是更合适的。例如在创建状态不可变的Bean或有状态Bean时。此外，需要注意的是，如果Bean单例是有状态的，那么在使用时需要考虑线程安全性问题。

#### Bean的单例和多例，生命周期是否一样？

不一样。Spring的生命周期完全由IoC容器控制，Spring只帮我们管理单例模式Bean的完整生命周期，对于prototype的Bean，Spring在创建好交给使用者后，就不会在管理后续的生命周期。

#### Spring bean的作用域有哪些？

Spring框架中的Bean作用域（Scope）定义了Bean的生命周期和可见性。不同作用域影响着Spring容器如何管理这些Bean的实例，包括它们如何被创建、小一号以及是否可以被多个用户共享。

Spring支持几种不同的作用域，以满足不同的应用场景需求：

- Singleton（单例）： 在整个应用程序中只存在一个Bean实例。是默认作用域，Spring容器只会创建一个Bean实例，并在容器的整个生命周期中共享该实例。

- Prototype（原型）： 每次请求时都会创建一个新的Bean实例。手机用于状态非常瞬时的Bean。
- Request（请求）： 每个HTTP请求都会创建一个新的Bean实例。仅在Spring Web应用程序中有效，每个HTTP请求都会创建一个新的Bean实例，适用于Web引用中需求局部性的Bean。
- Session（会话）： Session范围内只会创建一个Bean实例，该bean实例在用户会话范围内共享，仅在Spring Web应用程序中有效，适用于与用户会话相关的Bean。
- Application： 当起ServletContext中只存在一个Bean实例，仅在Spring Web应用程序中有效，该Bean实例在整个ServletContxt范围内共享，适用于应用程序范围内共享的 Bean。
- WebSocket（Web套接字）： 在 WebSocket 范围内只存在一个 Bean 实例。仅在支持 WebSocket 的应用程序中有效，该 Bean 实例在 WebSocket 会话范围内共享，适用于 WebSocket 会话范围内共享的 Bean。
- Custom scopes（自定义作用域）： Spring 允许开发者定义自定义的作用域，通过实现 Scope 接口来创建新的 Bean 作用域。

在Spring配置文件中，可以通过标签的scope属性来指定Bean的作用域。例如：

```
<bean id="myBean" class="com.example.MyBeanClass" scope="singleton"/>
```


在Spring Boot或基于Java的配置中，可以通过@Scope注解来指定Bean的作用域。例如：

```
@Bean  
@Scope("prototype")  
public MyBeanClass myBean() {  
    return new MyBeanClass();  
}
```



#### Spring容器里存的是什么？

Spring容器中存储的主要是Bean对象。

Bean是Spring框架中的基本组件， 用于表示应用程序中的各种对象。当应用程序启动时，Spring容器会根据配置文件或注解的方式创建和管理这些Bean对象。Spring容器会负责创建、初始化、注入依赖、销毁Bean对象。

> 在Spring中，在bean加载/销毁前后，如果想实现某些逻辑，可以怎么做？（太多了吧😵‍💫⭐️⭐️）

在Spring框架中，如果希望在bean加载（即实例化、属性初值、初始化等过程完成后）或销毁前后执行某些逻辑，你可以使用Spring的生命周期回调接口或注解。这些接口和注解允许你定义在Bean生命周期的关键点执行的代码：

- 使用init-method和destroy-method

  在XML配置中，你可以通过init-method和destroy-method属性来指定Bean初始化后和销毁前需要调用的方法

```
<bean id="myBean" class="com.example.MyBeanClass"  
      init-method="init" destroy-method="destroy"/>
```


然后，在你的Bean类中实现这些方法：

```
public class MyBeanClass {  

    public void init() {  
        // 初始化逻辑  
    }  
      
    public void destroy() {  
        // 销毁逻辑  
    }  

}
```

实现InitializingBean和DisposableBean接口
你的Bean类可以实现org.springframework.beans.factory.InitializingBean和org.springframework.beans.factory.DisposableBean接口，并分别实现afterPropertiesSet和destroy方法。

```
import org.springframework.beans.factory.DisposableBean;  
import org.springframework.beans.factory.InitializingBean;  

public class MyBeanClass implements InitializingBean, DisposableBean {  

    @Override  
    public void afterPropertiesSet() throws Exception {  
        // 初始化逻辑  
    }  
      
    @Override  
    public void destroy() throws Exception {  
        // 销毁逻辑  
    }  

}
```

使用@PostConstruct和@PreDestroy注解

```
import javax.annotation.PostConstruct;  
import javax.annotation.PreDestroy;  

public class MyBeanClass {  

    @PostConstruct  
    public void init() {  
        // 初始化逻辑  
    }  
      
    @PreDestroy  
    public void destroy() {  
        // 销毁逻辑  
    }  

}
```


使用@Bean注解的initMethod和destroyMethod属性

```
@Configuration  
public class AppConfig {  

    @Bean(initMethod = "init", destroyMethod = "destroy")  
    public MyBeanClass myBean() {  
        return new MyBeanClass();  
    }  

}
```



> Bean注入和xml注入最终得到了相同的效果，它们在底层是怎样做的？（感觉挺重要⭐️⭐️）

#### **XML注入：**

使用XML文件进行Bean注入时，Spring在启动时会读取XML配置文件，以下是其底层步骤：

- Bean定义解析： Spring容器通过XmlBeanDefinitionReader类解析XML配置文件，读取器中<bean>标签以获取Bean的定义信息

- 注册Bean定义： 解析后的Bean信息被注册到BeanDefinitionRegistry（如DefaultListableBeanFactory）中，包括Bean的类、作用域、依赖关系、初始化和销毁方法等

- 实例化和依赖注入： 当应用程序请求某个Bean时，Spring容器会根据已经注册的Bean定义：

  首先，使用反射机制创建该Bean的实例。

  然后，根据Bean定义中的配置，通过setter方法、构造函数或方法注入所需的依赖Bean。

#### **注解注入：**

使用注解进行Bean注入时，Spring的处理过程如下：

- 类路径扫描： 当 Spring 容器启动时，它首先会进行类路径扫描，查找带有特定注解（如 @Component、@Service、@Repository 和 @Controller）的类。

- 注册 Bean 定义： 找到的类会被注册到 BeanDefinitionRegistry 中，Spring 容器将为其生成 Bean 定义信息。这通常通过 AnnotatedBeanDefinitionReader 类来实现。
- 依赖注入： 与 XML 注入类似，Spring 在实例化 Bean 时，也会检查字段上是否有 @Autowired、@Inject 或 @Resource 注解。如果有，Spring 会根据注解的信息进行依赖注入。

尽管使用的方式不同，但 XML 注入和注解注入在底层的实现机制是相似的，主要体现在以下几个方面：

- BeanDefinition： 无论是 XML 还是注解，最终都会生成 BeanDefinition 对象，并存储在同一个 BeanDefinitionRegistry 中。

- 后处理器：

  Spring提供了多个Bean后处理器（如AutowiredAnnotationBeanPostProcessor），用于处理注解（如@Autowired）的依赖注入

  对于 XML，Spring 也有相应的后处理器来处理 XML 配置的依赖注入。

- 依赖查找： 在依赖注入时，Spring容器会通过 ApplicationContext 中的 BeanFactory 方法来查找和注入依赖，无论是通过 XML 还是注解，都会调用类似的查找方法。

#### Spring给我们提供了很多扩展点，这些有了解吗？（记不住啊⭐️⭐️⭐️）

Spring框架提供了许多扩展点，使得开发者可以根据需求定制和扩展Spring的功能。以下是一些常用的扩展点：

- BeanFactoryPostProcessor：允许在Spring容器实例化bean之前修改bean的定义。常用于修改bean属性或改变bean的作用域。

- BeanPostProcessor：可以在bean实例化、配置以及初始化之后对其进行额外处理。常用于代理bean、修改bean属性等。

- PropertySource：用于定义不同的属性源，如文件、数据库等，以便在Spring应用中使用。ImportSelector和ImportBeanDefinitionRegistrar：用于根据条件动态注册bean定义，实现配置类的模块化。

- Spring MVC中的HandlerInterceptor：用于拦截处理请求，可以在请求处理前、处理中和处理后执行特定逻辑。

- Spring MVC中的ControllerAdvice：用于全局处理控制器的异常、数据绑定和数据校验。

- Spring Boot的自动配置：通过创建自定义的自动配置类，可以实现对框架和第三方库的自动配置。

  自定义注解：创建自定义注解，用于实现特定功能或约定，如权限控制、日志记录等。

#### MVC分层介绍一下

MVC全名是Model View Controller，是模型（model）-视图（view）-控制器（controller）的缩写，一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。

- 视图（view）：为用户提供使用界面，与用户进行直接交互。

- 模型（model）：代表一个存取数据的对象或Java POJO（Plain Old Java Object，简单java对象）。他也可以带有逻辑，主要用于承载数据，并对用户提交请求进行计算的模块。 模型分为两类，一类称为数据承载Bean，一类称为业务处理Bean。数据承载Bean是指实体类（如User类），专门为用户承载业务数据的，而业务处理Bean是指Service或Dao对象，专门用于处理用户请求提交。
- 控制器（Controller）： 用于将用户请求转发给相应的Model进行处理，并给局Model的计算结果向用户提供相应响应。它使视图和模型分离。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9fd5bfb2a0fa4297bb1c16231e31a3ed.png)

流程步骤：

1. 用户通过view页面向服务端提出请求，可以是表单请求、超链接请求、AJAX请求等
2. 服务端Controller控制器接收到请求后对请求进行解析，找到相应的Model，对用户请求进行处理，是Model处理。
3. 将处理结果再交给Controller（控制器其实只是起到了承上启下的作用）；
4. 根据处理结果找到要作为向客户端发回的响应View页面，页面经渲染后发送给客户端

#### 了解SpringMVC的处理流程吗？

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/83dfb8dc67384666a53f215e1bb11a43.png)

Spring MVC的工作流程如下：

1. 用户发送请求到前端控制器DispatcherServlet

2. DispatcherServlet收到请求调用处理器映射起HandlerMapping。
3. 处理器映射器根据请求url找到具体的处理器，生成处理器执行链HandlerExecutionChain（包括处理器对象和处理器拦截器）一并返回给DispatcherServlet。
4. DispatcherServlet根据处理器Handler获取处理器适配器HandlerAdapter执行HandlerAdapter帮助处理一系列操作，如：参数封装、数据格式转换，数据验证等操作。
5. 执行处理器Handler（Controller，也叫页面控制器）。
6. Handler执行完成返回ModelAndView
7. HandlerAdapter将Handler执行结果ModelAndView返回到DispatcherServlet
8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器
9. ViewReslover解析后返回具体View
10. DispatcherServlet对View进行渲染视图（即 将模型数据model填充到视图中）。
11. DispatcherServlet响应用户

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9aa7238663234e3cb686db7f56c51156.png)

#### Handlermapping 和 handleradapter有了解吗？

HandlerMapping：

- 作用：负责将请求映射到处理器（Controller）
- 功能：根据请求的URL、请求参数等信息，找到处理请求的Controller。
- 类型：Spring提供了多种HandlerMapping实现，如BeanNameUrlHandlerMapping、RequestMappingHandlerMapping等。
- 工作流程：根据请求信息确定要请求的处理器（Controller）。HandlerMapping可以根据URL、请求参数等规则确定对应的处理器（处理器就是Controller）。

HandlerAdapter：

- 作用：HandlerAdapter负责调用处理器Controller来处理请求。

- 功能：处理器Controller可能有不同的接口类型（Controller接口、HttpRequestHandler接口等），HandlerAdapter根据处理器的类型来选择合适的方法来调用处理器。
- 工作流程：根据处理器的接口类型，选择相应的HandlerAdapter来调用处理器。

工作流程：

1. 当客户端发送请求时，HandlerMapping根据请求信息找到对应的处理器(Controller)。

2. HandlerAdapter根据处理器的类型选择合适的方法来调用处理器。
3. 处理器执行相应的业务逻辑，生成ModelAndView。
4. HandlerAdapter将处理器的执行结果包装成ModelAndView。
5. 视图解析器根据ModelAndView找到对应的视图进行渲染。
6. 将渲染后的视图返回给客户端。

HandlerMapping和HandlerAdapter协同工作，通过将请求映射到处理器，并调用处理器来处理请求，实现了请求处理的流程。它们的灵活性使得在Spring MVC中可以支持多种处理器和处理方式，提高了框架的扩展性和适应性。

---------

### 2、SpringBoot

#### SpringBoot比Spring好在哪里

- Spring Boot 提供了自动化配置，大大简化了项目的配置过程。通过约定优于配置的原则，很多常用的配置可以自动完成，开发者可以专注于业务逻辑的实现。
- Spring Boot 提供了快速的项目启动器，通过引入不同的 Starter，可以快速集成常用的框架和库（如数据库、消息队列、Web 开发等），极大地提高了开发效率。
- Spring Boot 默认集成了多种内嵌服务器（如Tomcat、Jetty、Undertow），无需额外配置，即可将应用打包成可执行的 JAR 文件，方便部署和运行。

#### SpringBoot用到哪些设计模式？（这个要背吗⭐️⭐️⭐️）

- 代理模式：Spring 的 AOP 通过动态代理实现方法级别的切面增强，有静态和动态两种代理方式，采用动态代理方式。

- 策略模式：Spring AOP 支持 JDK 和 Cglib 两种动态代理实现方式，通过策略接口和不同策略类，运行时动态选择，其创建一般通过工厂方法实现。
- 装饰器模式：Spring 用 TransactionAwareCacheDecorator 解决缓存与数据库事务问题增加对事务的支持。
- 单例模式：Spring Bean 默认是单例模式，通过单例注册表（如 HashMap）实现。
- 简单工厂模式：Spring 中的 BeanFactory 是简单工厂模式的体现，通过工厂类方法获取 Bean 实例。
- 工厂方法模式：Spring中的 FactoryBean 体现工厂方法模式，为不同产品提供不同工厂。
- 观察者模式：Spring 观察者模式包含 Event 事件、Listener 监听者、Publisher 发送者，通过定义事件、监听器和发送者实现，观察者注册在 ApplicationContext 中，消息发送由 ApplicationEventMulticaster 完成。
- 模板模式：Spring Bean 的创建过程涉及模板模式，体现扩展性，类似 Callback 回调实现方式。
- 适配器模式：Spring MVC 中针对不同方式定义的 Controller，利用适配器模式统一函数定义，定义了统一接口 HandlerAdapter 及对应适配器类。

#### 怎么理解SpringBoot中的约定大于配置

理解 Spring Boot 中的“约定大于配置”原则，可以从以下几个方面来解释：

- 自动化配置： Spring Boot 提供了大量的自动化配置，通过分析项目的依赖和环境，自动配置应用程序的行为。开发者无需显式地配置每个细节，大部分常用的配置都已经预设好了。例如，Spring Boot 可以根据项目中引入的数据库依赖自动配置数据源。

- 默认配置： Spring Boot 在没有明确配置的情况下，会使用合理的默认值来初始化应用程序。这种默认行为使得开发者可以专注于核心业务逻辑，而无需关心每个细节的配置。
- 约定优于配置： Spring Boot 遵循了约定优于配置的设计哲学，即通过约定好的方式来提供默认行为，减少开发者需要做出的决策。例如，约定了项目结构、Bean 命名规范等，使得开发者可以更快地上手并保持团队间的一致性。

Spring Boot 的“约定大于配置”原则是一种设计理念，通过减少配置和提供合理的默认值，使得开发者可以更快速地构建和部署应用程序，同时降低了入门门槛和维护成本。
Spring Boot通过「自动化配置」和「起步依赖」实现了约定大于配置的特性。

- 自动化配置：Spring Boot根据项目的依赖和环境自动配置应用程序，无需手动配置大量的XML或Java配置文件。例如，如果项目引入了Spring Web MVC依赖，Spring Boot会自动配置一个基本的Web应用程序上下文。

- 起步依赖：Spring Boot提供了一系列起步依赖，这些依赖包含了常用的框架和功能，可以帮助开发者快速搭建项目。

> SpringBoot自动装配原理是什么？

#### **什么是自动装配**？

SpringBoot 的自动装配原理是基于Spring Framework的条件化配置和@EnableAutoConfiguration注解实现的。这种机制允许开发者在项目中引入相关的依赖，SpringBoot 将根据这些依赖自动配置应用程序的上下文和功能。

SpringBoot 定义了一套接口规范，这套规范规定：SpringBoot 在启动时会扫描外部引用 jar 包中的META-INF/spring.factories文件，将文件中配置的类型信息加载到 Spring 容器（此处涉及到 JVM 类加载机制与 Spring 的容器知识），并执行类中定义的各种操作。对于外部 jar 来说，只需要按照 SpringBoot 定义的标准，就能将自己的功能装置进 SpringBoot。

通俗来讲，自动装配就是通过注解或一些简单的配置就可以在SpringBoot的帮助下开启和配置各种功能，比如数据库访问、Web开发。

#### 说几个启动器（Starter）？（记不住啊⭐️⭐️）

- spring-boot-starter-web：这是最常用的起步依赖之一，它包含了Spring MVC和Tomcat嵌入式服务器，用于快速构建Web应用程序。

- spring-boot-starter-security：提供了Spring Security的基本配置，帮助开发者快速实现应用的安全性，包括认证和授权功能。
- mybatis-spring-boot-starter：这个Starter是由MyBatis团队提供的，用于简化在Spring Boot应用中集成MyBatis的过程。它自动配置了MyBatis的相关组件，包括SqlSessionFactory、MapperScannerConfigurer等，使得开发者能够快速地开始使用MyBatis进行数据库操作。
- spring-boot-starter-data-jpa 或 spring-boot-starter-jdbc：如果使用的是Java Persistence API (JPA)进行数据库操作，那么应该使用spring-boot-starter-data-jpa。这个Starter包含了Hibernate等JPA实现以及数据库连接池等必要的库，可以让你轻松地与MySQL数据库进行交互。你需要在application.properties或application.yml中配置MySQL的连接信息。如果倾向于直接使用JDBC而不通过JPA，那么可以使用spring-boot-starter-jdbc，它提供了基本的JDBC支持。
- spring-boot-starter-data-redis：用于集成Redis缓存和数据存储服务。这个Starter包含了与Redis交互所需的客户端（默认是Jedis客户端，也可以配置为Lettuce客户端），以及Spring Data Redis的支持，使得在Spring Boot应用中使用Redis变得非常便捷。同样地，需要在配置文件中设置Redis服务器的连接详情。
- spring-boot-starter-test：包含了单元测试和集成测试所需的库，如JUnit, Spring Test, AssertJ等，便于进行测试驱动开发(TDD)。

#### springboot怎么开启事务？

在 Spring Boot 中开启事务非常简单，只需在服务层的方法上添加 @Transactional 注解即可。

```
public class UserServiceImpl implements UserService {

@Autowired
private UserRepository userRepository;

@Override
@Transactional
public void saveUser(User user) {
    userRepository.save(user);
}

}

```

--------------
### 3、Mybatis

#### 还记得JDBC连接数据库的步骤吗？

1. 加载数据库驱动程序： 在使用JDBC连接数据库之前，需要加载相应的数据库驱动程序。可以通过Class.forBane("com.mysql.jdbc.Driver")来加载MySQL数据库的驱动程序。不同数据库的驱动类名会有所不同
2. 建立数据库连接： 使用DriverManager类的getConnection(url, username, password)方法来连接数据库，其中url是数据库的连接字符串（包括数据库类型、主机、端口等），username是数据库用户名，password是密码。
3. 创建 Statement 对象： 通过 Connection 对象的 createStatement() 方法创建一个 Statement 对象，用于执行 SQL 查询或更新操作。
4. 执行SQL查询或更新操作： 使用Statement对象的executeQuery(Sql)方法来执行Select查询操作，或者使用executeUpdate(sql)方法来执行INSERT、UPDATE或DELETE操作。
5. 处理查询结果： 如果是Selector查询操作，通过ResultSet对象来处理查询结果。可以使用ResultSet的next()方法遍历查询结果集，然后通过getXXX()方法获取各个字段的值。
6. 关闭连接： 在完成数据库操作后，需要逐级关闭数据库连接相关对象，即先关闭ResultSet，再关闭Statement，最后管理Connection。

以下是一个简单的示例代码：

```
import java.sql.*;

public class Main {
    public static void main(String[] args) {
        try {
            // 加载数据库驱动程序
            Class.forName("com.mysql.cj.jdbc.Driver");

        // 建立数据库连接
        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydatabase", "username", "password");

        // 创建 Statement 对象
        Statement statement = connection.createStatement();

        // 执行 SQL 查询
        ResultSet resultSet = statement.executeQuery("SELECT * FROM mytable");

        // 处理查询结果
        while (resultSet.next()) {
          // 处理每一行数据
        }

        // 关闭资源
        resultSet.close();
        statement.close();
        connection.close();
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (SQLException e) {
        e.printStackTrace();
    }
}

}

```


请注意，在实际应用中，需要进行异常处理以确保资源的正确释放，以及使用 try-with-resources 来简化代码和确保资源的及时关闭。

#### 如果项目中要用到原生的mybatis去查询，该怎样写？

1. 配置MyBatis： 在项目中配置MyBatis的数据源、SQL映射文件等。

2. 创建实体类： 创建用于映射数据库表的实体类。
3. 编写SQL映射文件： 创建XML文件，定义SQL语句和映射关系。
4. 编写DAO接口： 创建DAO接口，定义数据库操作的方法。
5. 编写具体的SQL查询语句： 在DAO接口中定义查询方法，并在XML文件中编写对应的SQL语句。
6. 调用查询方法： 在服务层或控制层调用DAO接口中的方法进行查询。

具体来说：

1. 配置MyBatis： 在配置文件中配置数据源、MyBatis的Mapper文件位置等信息。

2. 创建实体类： 创建与数据库表对应的实体类，字段名和类型需与数据库表保持一致。

```
public class User {
    private Long id;
    private String username;
    private String email;
    // Getters and setters
}
```

  3.编写SQL映射文件： 在resources目录下创建XML文件，定义SQL语句和映射关系。

```
<!-- userMapper.xml -->
<mapper namespace="com.example.dao.UserMapper">
    <select id="selectUserById" resultType="com.example.model.User">
        SELECT * FROM users WHERE id = #{id}

    </select>
</mapper>
```

  4.编写DAO接口： 创建DAO接口，定义查询方法。

```
public interface UserMapper {
    User selectUserById(Long id);
}
```

  5.编写具体的SQL查询语句： 在XML文件中编写对应的SQL语句。
  6.调用查询方法： 在服务层或控制层中调用DAO接口中的方法进行查询。

```
// 在Service层中调用
User user = userMapper.selectUserById(1);
```

#### Mybatis里的 # 和 $ 的区别？

- Mybatis在处理#{}时，会创建预编译的SQL语句，将SQL中得到#{}替换为 ？号，在执行SQL时会为预编译SQL中的占位符 ？ 赋值，调用PreparedStatement的set方法来赋值，预编译的SQL语句执行效率高，且可以防止SQL注入，提供更高的安全性，适合传递参数值。

- Mybatis在处理${}时，只是创建普通的SQL语句，然后在执行SQL语句时Mybatis将参数直接拼入到SQL中，不能防止SQL注入，因为是直接拼接的，如果参数未经过验证过滤，可能会导致安全问题。

#### MybatisPlus和Mybatis的区别？

MybatisPlus是一个基于Mybatis的增强工具库，旨在简化开发并提高效率。以下是MybatisPlus和Mubatis的区别：

- CRUD操作： MybatisPlus通过继承BaseMapper接口，提供了一系列内置的快捷方法，使得CRUD操作更加简单，无需编写重复的SQL语句。

- 代码生成器： MybatisPlus提供了代码生成器功能，可以根据数据库表结构自动生成实体类、Mapper接口以及XML映射文件，减少了手动编写的工作量。
- 通用方法封装： MybatisPlus封装了许多常用的方法，如条件构造器、排序、分页查询等，简化了开发过程，提高了开发效率。
- 分页插件： MybatisPlus内置了分页插件，支持各种数据库的分页查询，开发者可以轻松实现分页功能，而在传统的MyBatis中，需要开发者自己手动实现分页逻辑。
- 多租户支持： MybatisPlus提供了多租户的支持，可以轻松实现多租户数据隔离的功能。
- 注解支持： MybatisPlus引入了更多的注解支持，使得开发者可以通过注解来配置实体与数据库表之间的映射关系，减少了XML配置文件的编写。

#### MyBatis运用了哪些常见的设计模式？（记不住⭐️⭐️）

- 建造者模式（Builder），如：SqlSessionFactoryBuilder、XMLConfigBuilder、XMLMapperBuilder、XMLStatementBuilder、CacheBuilder等；

- 工厂模式，如：SqlSessionFactory、ObjectFactory、MapperProxyFactory；
- 单例模式，例如ErrorContext和LogFactory；
- 代理模式，Mybatis实现的核心，比如MapperProxy、ConnectionLogger，用的jdk的动态代理；还有executor.loader包使用了cglib或者javassist达到延迟加载的效果；
- 组合模式，例如SqlNode和各个子类ChooseSqlNode等；
- 模板方法模式，例如BaseExecutor和SimpleExecutor，还有BaseTypeHandler和所有的子类例如IntegerTypeHandler；
- 适配器模式，例如Log的Mybatis接口和它对jdbc、log4j等各种日志框架的适配实现；
- 装饰者模式，例如Cache包中的cache.decorators子包中等各个装饰者的实现；
- 迭代器模式，例如迭代器模式PropertyTokenizer；

------

### 4、SpringCloud

#### 了解SpringCloud吗，说一下他和SpringBoot的区别

Spring Boot是用于构建单个Spring应用的框架，而Spring Cloud则是用于构建分布式系统中的微服务架构的工具，Spring Cloud提供了服务注册与发现、负载均衡、断路器、网关等功能。

两者可以结合使用，通过Spring Boot构建微服务应用，然后用Spring Cloud来实现微服务架构中的各种功能。

#### 用过哪些微服务组件？

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/035d2c1c4e6d44eaa266bf477ba8cd68.png)

微服务常用的组件：

- 注册中心： 注册中心是微服务架构最核心的组件。它起到的作用是对新节点的注册与状态维护，解决了「如何发现新节点以及检查各节点的运行状态的问题」 。微服务节点在启动时会将自己的服务名称、IP、端口等信息在注册中心登记，注册中心会定时检查该节点的运行状态。注册中心通常会采用心跳机制最大程度保证已登记过的服务节点都是可用的。

- 负载均衡： 负载均衡解决了「如何发现服务及负载均衡如何实现的问题」，通常微服务在互相调用时，并不是直接通过IP、端口进行访问调用。而是先通过服务名在注册中心查询该服务拥有哪些节点，注册中心将该服务可用节点列表返回给服务调用者，这个过程叫服务发现，因服务高可用的要求，服务调用者会接收到多个节点，必须要从中进行选择。因此服务调用者一端必须内置负载均衡器，通过负载均衡策略选择合适的节点发起实质性的通信请求。
- 服务通信： 服务通信组件解决了「服务间如何进行消息通信的问题」，服务间通信采用轻量级协议，通常是HTTP RESTful风格。但因为RESTful风格过于灵活，必须加以约束，通常应用时对其封装。例如在SpringCloud中就提供了Feign和RestTemplate两种技术屏蔽底层的实现细节，所有开发者都是基于封装后统一的SDK进行开发，有利于团队间的相互合作。
- 配置中心： 配置中心主要解决了「如何集中管理各节点配置文件的问题」，在微服务架构下，所有的微服务节点都包含自己的各种配置文件，如jdbc配置、自定义配置、环境配置、运行参数配置等。要知道有的微服务可能可能有几十个节点，如果将这些配置文件分散存储在节点上，发生配置更改就需要逐个节点调整，将给运维人员带来巨大的压力。配置中心便由此而生，通过部署配置中心服务器，将各节点配置文件从服务中剥离，集中转存到配置中心。一般配置中心都有UI界面，方便实现大规模集群配置调整。
- 集中式日志管理： 集中式日志主要是解决了「如何收集各节点日志并统一管理的问题」。微服务架构默认将应用日志分别保存在部署节点上，当需要对日志数据和操作数据进行数据分析和数据统计时，必须收集所有节点的日志数据。那么怎么高效收集所有节点的日志数据呢？业内常见的方案有ELK、EFK。通过搭建独立的日志收集系统，定时抓取各节点增量日志形成有效的统计报表，为统计和分析提供数据支撑。
- 分布式链路追踪： 分布式链路追踪解决了「如何直观的了解各节点间的调用链路的问题」。系统中一个复杂的业务流程，可能会出现连续调用多个微服务，我们需要了解完整的业务逻辑涉及的每个微服务的运行状态，通过可视化链路图展现，可以帮助开发人员快速分析系统瓶颈及出错的服务。
- 服务保护： 服务保护主要是解决了「如何对系统进行链路保护，避免服务雪崩的问题」。在业务运行时，微服务间互相调用支撑，如果某个微服务出现高延迟导致线程池满载，或是业务处理失败。这里就需要引入服务保护组件来实现高延迟服务的快速降级，避免系统崩溃。

#### SpringCloud Alibaba实现的微服务架构：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4f5924d16de24acd8ae8b5257f89344e.png)

- SpringCloud Alibaba中使用Alibaba Nacos组件实现注册中心，Nacos提供了一组简单易用的特性集，可快速实现动态服务发现、服务配置、服务元数据及流量管理。
- SpringCloud Alibaba 使用Nacos服务端均衡实现负载均衡，与Ribbon在调用端负载不同，Nacos是在服务发现的同时利用负载均衡返回服务节点数据。
- SpringCloud Alibaba 使用Netflix Feign和Alibaba Dubbo组件来实现服务通行，前者与SpringCloud采用了相同的方案，后者则是对自家的RPC 框架Dubbo也给予支持，为服务间通信提供另一种选择。
- SpringCloud Alibaba 在API服务网关组件中，使用与SpringCloud相同的组件，即：SpringCloud Gateway。
- SpringCloud Alibaba在配置中心组件中使用Nacos内置配置中心，Nacos内置的配置中心，可将配置信息存储保存在指定数据库中
- SpringCloud Alibaba在原有的ELK方案外，还可以使用阿里云日志服务（LOG）实现日志集中式管理。
- SpringCloud Alibaba在分布式链路组件中采用与SpringCloud相同的方案，即：Sleuth/Zipkin Server。
- SpringCloud Alibaba使用Alibaba Sentinel实现系统保护，Sentinel不仅功能更强大，实现系统保护比Hystrix更优雅，而且还拥有更好的UI界面。

#### 负载均衡有哪些算法？

简单轮询：将请求按顺序分发给后端服务器上，不关心服务器当前的状态，比如后端服务器的性能、当前的负载。
加权轮询：根据服务器自身的性能给服务器设置不同的权重，将请求按顺序和权重分发给后端服务器，可以让性能高的机器处理更多的请求
简单随机：将请求随机分发给后端服务器上，请求越多，各个服务器接收到的请求越平均
加权随机：根据服务器自身的性能给服务器设置不同的权重，将请求按各个服务器的权重随机分发给后端服务器
一致性哈希：根据请求的客户端 ip、或请求参数通过哈希算法得到一个数值，利用该数值取模映射出对应的后端服务器，这样能保证同一个客户端或相同参数的请求每次都使用同一台服务器
最小活跃数：统计每台服务器上当前正在处理的请求数，也就是请求活跃数，将请求分发给活跃数最少的后台服务器

#### 如何实现一直均衡给一个用户？

可以通过「一致性哈希算法」来实现，根据请求的客户端 ip、或请求参数通过哈希算法得到一个数值，利用该数值取模映射出对应的后端服务器，这样能保证同一个客户端或相同参数的请求每次都使用同一台服务器。

#### 介绍一下服务熔断

服务熔断是应对微服务雪崩效应的一种链路保护机制，类似股市、保险丝。

比如说，微服务之间的数据交互是通过远程调用来完成的。服务A调用服务，服务B调用服务c，某一时间链路上对服务C的调用响应时间过长或者服务C不可用，随着时间的增长，对服务C的调用也越来越多，然后服务C崩溃了，但是链路调用还在，对服务B的调用也在持续增多，然后服务B崩溃，随之A也崩溃，导致雪崩效应。

服务熔断是应对雪崩效应的一种微服务链路保护机制。例如在高压电路中，如果某个地方的电压过高，熔断器就会熔断，对电路进行保护。同样，在微服务架构中，熔断机制也是起着类似的作用。当调用链路的某个微服务不可用或者响应时间太长时，会进行服务熔断，不再有该节点微服务的调用，快速返回错误的响应信息。当检测到该节点微服务调用响应正常后，恢复调用链路。

所以，服务熔断的作用类似于我们家用的保险丝，当某服务出现不可用或响应超时的情况时，为了防止整个系统出现雪崩，暂时停止对该服务的调用。

在Spring Cloud框架里，熔断机制通过Hystrix实现。Hystrix会监控微服务间调用的状况，当失败的调用到一定阈值，缺省是5秒内20次调用失败，就会启动熔断机制。

在Spring Cloud框架里，熔断机制通过Hystrix实现。Hystrix会监控微服务间调用的状况，当失败的调用到一定阈值，缺省是5秒内20次调用失败，就会启动熔断机制。

#### 介绍一下服务降级

服务降级一般是指在服务器压力剧增的时候，根据实际业务使用情况以及流量，对一些服务和页面有策略的不处理或者用一种简单的方式进行处理，从而释放服务器资源的资源以保证核心业务的正常高效运行。

服务器的资源是有限的，而请求是无限的。在用户使用即并发高峰期，会影响整体服务的性能，严重的话会导致宕机，以至于某些重要服务不可用。故高峰期为了保证核心功能服务的可用性，就需要对某些服务降级处理。可以理解为舍小保大

服务降级是从整个系统的负荷情况出发和考虑的，对某些负荷会比较高的情况，为了预防某些功能（业务场景）出现负荷过载或者响应慢的情况，在其内部暂时舍弃对一些非核心的接口和数据的请求，而直接返回一个提前准备好的fallback（退路）错误处理信息。这样，虽然提供的是一个有损的服务，但却保证了整个系统的稳定性和可用性。

-------------



## MySQL

### SQL基础

#### 数据库三大范式是什么？

具体看这里

```
第一范式（1NF）：要求数据库表的每一列都是不可分割的原子数据项。
第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于候选码（在1NF基础上消除非主属性对主码的部分函数依赖）
第三范式（3NF）：在2NF基础上，任何非主属性 (opens new window)不依赖于其它非主属性（在2NF基础上消除传递依赖）第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。
```

-----

#### MySQL 怎么连表查询？

数据库有一下几种链表查询类型：

1. 内连接（INNER JOIN）

2. 左外连接（LEFT JOIN）
3. 右外连接（RIGHT JOIN）
4. 全外连接（FULL JOIN）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/556b059632684d748f80250501af6ea5.png)

1. 内连接（INNER JOIN）
  内连接返回两个表中有匹配关系的行。
  语法：

  ```java
  select a.*, b.* from tablea a
     inner join tableb b
     on a.id = b.id
  
  或
  
  select a.*, b.* from tablea a
  join tableb b
  on a.id = b.id
  ```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/fcf5091eecea4e3c9b1d50870bd413bd.png)

2. 左外连接（LEFT JOIN）

```
select a.*, b.* from tablea a
left join tableb b
on a.id = b.id
或
select a.*, b.* from tablea a
left outer join tableb b
on a.id = b.id
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/00878b5b69294409a7447c0bc95ebcbc.png)

3. 右外连接（RIGHT JOIN）

```
select a.id aid,a.age,b.id bid,b.name from tablea a
right join tableb b
on a.id = b.id
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/07ebec8fea19425a83a59f53607272cf.png)

4. 全连接（FULL JOIN）
  全外连接返回两个表中所有行，包括非匹配行，在MySQL中，FULL JOIN 需要使用 UNION 来实现，因为 MySQL 不直接支持 FULL JOIN。

```
select a.id aid,a.age,b.id bid,b.name from tablea a
left join tableb b
on a.id = b.id
union
select a.id aid,a.age,b.id bid,b.name from tablea a
right join tableb b
on a.id = b.id
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c0112e2c8b4d43a695d8ce1af6861ae0.png)

-----------

#### MySQL如何避免重复插入数据？

方式一：使用UNIQUE约束
在表的相关列上添加UNIQUE约束，确保每个值在该列中唯一。例如：

```
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(255) UNIQUE,
    name VARCHAR(255)
);
```


方法二：使用INSERT … IN DUPLICATE KEY UPDATE
这种语句允许在插入记录时处理重复键的情况。如果插入的记录与现有记录冲突，可以选择更新现有记录：

```
INSERT INTO users (email, name) 
VALUES ('example@example.com', 'John Doe')
ON DUPLICATE KEY UPDATE name = VALUES(name);
```


方式三：使用INSERT IGNORE：
该语句会在插入记录时忽略那些因重复键而导致的插入错误：

```
INSERT IGNORE INTO users (email, name) 
VALUES ('example@example.com', 'John Doe');
```


如果email已经存在，这条插入语句将被忽略而不会返回错误。

选择哪种方法取决于具体的需求：

- 如果需要保证全局唯一性，使用UNIQUE约束是最佳做法

- 如果需要插入和更新结合，可以使用ON DUPLICATE KEY UPDATE。
- 对于快速忽略重复插入，INSERT IGNORE是合适选择。

---------------

#### CHAR 和 VARCHAR有什么区别？

- CHAR是固定长度的字符串类型，定义时需要指定固定长度，存储时会在末尾补足空格。CHAR适合存储长度固定的数据，如固定长度的代码、状态等。存储空间固定对于短字符串效率较高。

- VARCHAR是可变长度的字符串类型，定义时需要指定最大长度，实际存储时根据实际长度占用存储空间。VARCHAR适合存储可变长度的数据，如用户输入的文本、备注等，节约存储空间。

  -----------

#### Text数据类型可以无限大吗？

MySQL 3种text类型的最大长度如下：

```
TEXT：65535bytes ~ 64kb
MEDIUMTEXT：16777215 bytes ~ 16Mb
LONGTEXT：4294967295 bytes ~ 4Gb
```

-----

#### 说一下外键约束

外键约束的作用是维护表与表之间的关系，确保数据的完整性和一致性。让我们举一个简单的例子：

假设你有两个表，一个是学生表，另一个是课程表，这两个表之间有一个关系，即一个学生可以选修多门课程，而一门课程也可以被多个学生选修。在这种情况下，我们可以在学生表中定义一个指向课程表的外键，如下所示：

```
CREATE TABLE students (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  course_id INT,
  FOREIGN KEY (course_id) REFERENCES courses(id)
);
```

这里student表中的course_id字段是一个外键，它指向courses表中的id字段。这个外键约束确保了每个学生所选的课程在courses表中都存在，从而维护了数据的完整性和一致性。

如果没有定义外键约束，那么就有可能出现学生选了不存在的课程或者删除了一个课程而忘记从学生表中删除选修该课程的学生的情况，这会破坏数据的完整性和一致性。因此，使用外键约束可以帮助我们避免这些问题。

----------------

#### ？？？？？MySQL的关键字in和exist

`IN` 和 `EXISTS` 是两个常用的子查询操作符。但它们在功能、性能和使用场景上有各自的特点和区别。

**IN：** 

----------

#### 存储引擎

执行一条SQL请求的过程是什么？

下图是执行一条SQL查询语句 的流程，也可以从途中看出MySQL内部架构里的各个功能模块。

连接器：建立连接、管理连接，校验用户身份；
查询缓存：查询语句如果命中查询缓存，则直接返回，否则继续执行往下执行。MySQL 8.0已经删除该模块
解析器：通过解析器对SQL查询语句进行语法分析、词法分析，然后构建语法树，方便后续模块读取表名、字段、语句类型；
执行SQL：执行SQL共有 3 个阶段：
预处理阶段：检查表或字段是否存在；将select * 中的*符号扩展为表上的所有列。
优化阶段：基于查询成本的考虑，选择查询成本最小的执行计划；
执行阶段：根据执行计划执行SQL查询语句，从存储引擎读取记录，返回给客户端
讲一讲mysql的引擎吧，你有什么了解？

InnoDB：InnoDB是MySQL的默认存储引擎，具有ACID事务支持、行级锁、外键约束等特性。它适用于高并发的读写操作，支持较好的数据完整性和并发控制。
MyISAM：MyISAM是MySQL的另一种存储引擎，具有较低的存储空间和内存消耗，适用于大量读操作的场景。然而，MyISAM不支持事务、行级锁、外键约束，因此在并发写入和数据完整性方面有一定的限制。
Memory：Memory引擎将数据存储在内存中，适用于对性能要求较高的读操作。但在服务器重启或崩溃时数据会都是。它不支持事务、行级锁、外键约束
MySQL为什么InnoDB是默认引擎？

事务： InnoDB支持事务，MyISAM不支持事务，这是MySQL将默认存储引擎从MyISAM改为InnoDB的重要原因之一。
索引结构： InnoDB是聚簇索引，MyISAM是非聚簇索引。聚簇索引的文件存放在主键索引的叶子节点上，因此InnoDB必须要有主键。通过主键索引效率很高。但是辅助索引需要两次查询，先查询到主键，然后再通过主键查询到数据。因此，主键不应该过大，因为主键过大，其他索引也都会很大。而MyISAM是非聚簇索引，数据文件是分离的。索引保存的是数据文件的指针。主键索引和辅助索引是独立的。
锁粒度： InnoDB最小的锁粒度是行锁，MyISAM最小锁粒度是表锁。一个更新语句会锁住整张表，导致其他查询和更新都会被阻塞，因此并发访问受限。
count的效率： InnoDB不保存表的具体行数，执行selec t count(*) from table时需要全表扫描。而MyISAM用一个变量保存了整个表的行数，执行上述语句只需要读出变量即可，速度很快。
数据管理里，数据文件大体分成哪几种数据文件？

------

### 索引

#### 索引是什么？有什么好处？

索引类似于书籍的目录，可以减少扫描的数据量，提高查询效率。

如果查询的时候，没有用到索引就会全表扫描，这时候查询的时间复杂度是O（n）
如果用到了索引，那么查询的时候，可以基于二分查找算法，通过索引快速定位到目标数据，mysql的索引的数据结构一般是b+树，其搜索时间复杂度为O（logdN），其中d表示节点允许的最大子节点个数为d个。

--------

#### 讲讲索引的分类是什么？

- 按「数据结构」分类：B+ tree索引、Hash索引、Full-text索引

- 按「物理存储」分类：聚簇索引（主键索引）、二级索引（辅助索引）
- 按「字段特性」分类：主键索引、唯一索引、普通索引、前缀索引。
- 按「字段个数」索引：单列索引、联合索引。

按数据结构分类：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/500c6cfc2e534918bd4a5a03d14f5ea9.png)

InnoDB 是在 MySQL 5.5 之后成为默认的 MySQL 存储引擎，B+Tree 索引类型也是 MySQL 存储引擎采用最多的索引类型。

在创建表时，InnoDB 存储引擎会根据不同的场景选择不同的列作为索引：

- 如果有主键，默认会使用主键作为聚簇索引的索引键（key）；

- 如果没有主键，就选择第一个不包含NULL值的唯一列作为聚簇索引的索引键（key）
- 在上面两个都没有的情况下，InnoDB将自动生成一个隐式自增id列作为聚簇索引的索引键（key）；

其他索引都属于辅助索引，也被称为二级索引或非聚簇索引。创建的主键索引和二级索引默认使用的是B + Tree索引

按物理存储分类：
从物理存储的角度来看，索引分为聚簇索引（主键索引）、二级索引（辅助索引）。
这两个的区别：

- 主键索引的B+ 树的叶子节点存放的是实际数据，所有完整的用户记录都存放在主键索引的B+树的叶子节点里

- 二级索引的B+树的叶子节点存放的是主键值，而不是实际数据。

所以，在查询时使用了二级索引，如果查询的数据能在二级索引里查询得到，那么就不需要回表，这个过程就是覆盖索引。如果查询的数据不在二级索引里，就会先检索二级索引，找到对应的叶子节点，获取到主键值后，再检索主键索引，就能查询到数据了。这个过程就是回表。

按字段特性分类：

从字段特性的角度来看，索引分为：主键索引、唯一索引、普通索引、前缀索引。

1、主键索引
主键索引就是建立在主键字段上的索引，通常在创建表的时候一起创建，一张表最多只有一个主键索引，索引列的值不允许有空值。（使用primary key）
创建主键索引的方式如下：

```
CREATE TABLE table_name  (
  ....
  PRIMARY KEY (index_column_1) USING BTREE
);
```

2、唯一索引
唯一索引建立在UNIQUE字段上的索引，一张表可以有多个唯一索引，索引列的值必须唯一，但是允许有空值。
在创建表时，创建唯一索引的方式如下：

```
CREATE TABLE table_name  (
  ....
  UNIQUE KEY(index_column_1,index_column_2,...) 
);
```

3、普通索引
普通索引就是建立在普通字段上的索引，既不要求字段为主键，也不要求字段为 UNIQUE。
在创建表时，创建普通索引的方式如下：

```
CREATE TABLE table_name  (
  ....
  INDEX(index_column_1,index_column_2,...) 
);
```

建表后，如果要创建普通索引，可以使用这面这条命令：

```
CREATE INDEX index_name
ON table_name(index_column_1,index_column_2,...);
```

3、前缀索引
前缀索引是指对字符类型字段的前几个字符建立的索引，而不是在整个字段上建立的索引，前缀索引可以建立在字段类型为 char、 varchar、binary、varbinary 的列上。
使用前缀索引的目的是为了减少索引占用的存储空间，提升查询效率。

在创建表时，创建前缀索引的方式如下：

```
CREATE TABLE table_name(
    column_list,
    INDEX(column_name(length))
);
```

建表后，如果要创建前缀索引，可以使用这面这条命令：

```
CREATE INDEX index_name
ON table_name(column_name(length));
```

按字段个数分类：
从字段个数的角度来看，索引分为单列索引、联合索引（复合索引）

- 建立在单列上的索引被称为单列索引，比如主键索引。

- 建立在多列上的索引被称为联合索引；

通过将多个字段组合成一个索引，该索引就被称为联合索引。

比如，将商品表中的pruduct_no和name字段组合成联合索引（product_no, name)，创建联合索引的方式如下：

```
CREATE INDEX index_product_no_name ON product(product_no, name);
```

联合索引(product_no, name) 的 B+Tree 示意图如下

![![[Pasted image 20241118165229.png]]](https://i-blog.csdnimg.cn/direct/7dffd877773a4c44a763cfb8f8c06ebc.png)


也就是说，联合索引查询的 B+Tree 是先按 product_no 进行排序，然后再 product_no 相同的情况再按 name 字段排序。

因此，使用联合索引时，存在最左匹配原则，也就是默认按照最左优先的方式进行索引的匹配。在使用联合索引进行查询的时候，如果不遵循「最左匹配原则」，联合索引会失效。这样就无法利用索引快速查询的特性了。

比如，创建了一个(a, b, c)联合索引，如果查询条件是一下几种，就可以匹配上联合索引：

- where a = 1;

- where a = 1 and b = 2 and c =3;
- where a = 1 and b = 2;

需要注意的是，由于有查询优化器，所以a字段在where子句的顺序并不重要。
但是，如果查询条件是以下这几种，因为不符合「最左匹配原则」，所以就无法匹配上联合索引，联合索引就会失效：

- where b = 2;

- where c = 3;
- where b = 2 and c = 3;

上面这些查询条件之所以会失效，是因为(a, b, c)联合索引，是先按a排序，在a相同的情况下再按b排序，在b相同的情况下再按c排序。所以，b和c是全局无需，局部嫌贵的有序的，这样在没有遵循最左匹配原则的情况下，是无法利用到索引的。
联合索引有一些特殊情况，并不是查询过程使用了联合索引查询，就代表联合索引中的所有字段都用到了联合索引进行索引查询。 也就是可能存在部分字段用到了联合索引的B+树，部分字段没有用到联合索引的B+树的情况。

这种特殊情况就发生在范围查询。联合索引的最左匹配原则会一直向右匹配直到遇到「范围查询」就会停止匹配。也就是范围查询的字段可以用到联合索引，但是在范围查询字段后面的字段无法用到联合索引。

#### MySQL聚簇索引和非聚簇索引的区别是什么？

![![[../../../assets/Pasted image 20241118191649.png]]](https://i-blog.csdnimg.cn/direct/f81e36d899f440e9af93a0970420e441.png)

数据存储： 在聚簇索引中，数据行按照索引键值的顺序存储，也就是说，聚簇索引的叶子节点包含了实际的数据行。这意味着索引结构本身就是数据的物理存储结构。菲聚簇索引的叶子节点不包含完整的数据行，而是包含执行数据行的指针或主键值。数据行本身存储在聚簇索引中。
索引与数据关系： 当通过聚簇索引查找数据时，可以直接从所以那种获取数据行而不需要额外的步骤去查找数据所在的位置。单通过非聚簇索引查找数据时，首先在非聚簇索引中找到对应的主键值，然后通过这个主键值回溯到聚簇索引中查找实际的数据行，这一过程叫做回表。
唯一性： 聚簇索引通常是基于主键构建的，因此每个表只能有一个聚簇索引，因为数据只能有一种物理排序方式。一个表可以有多个非聚簇索引，因为他们不直接影响数据的物理存储位置。
效率： 对于范围查询和排序查询，聚簇索引通常更有效率。因为他避免了额外的寻址开销。非聚簇索引在使用覆盖索引查找时效率更高，因为他不需要读取完整的数据行，但是需要进行回表的操作，使用非聚簇索引效率比较低，因为需要进行额外的回表操作。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/25d2a8992abb49b8ac7d64fbd02b8c77.png)

#### 如果聚簇索引的数据更新，它的存储要不要变化？

- 如果更新的数据是非索引数据，即普通的用户记录，那么存储结构是不会发生变化的。
- 如果更新的数据是索引数据，那存储结构是有变化的，因为要维护b+树的有序性。

#### MySQL主键是聚簇索引吗？

在MySQL的InnoDB存储引擎中，主键是以聚簇索引的形式存储的。

InnoDB将数据存储在B+树的结构中，其中主键索引的B+树就是所谓的聚簇索引。这意味着表中的数据行在物理上是按照主键的顺序排序的，聚簇索引的叶节点包含了实际的数据行。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/256f49bc2f804e9fa5c9a72b78b84dc5.png)

InnoDB在创建聚簇索引时，会根据不同的场景选择不同的列作为索引：

- 如果有主键，默认会使用主键作为聚簇索引的索引键；
- 如果没有主键，就选择第一个不包含 NULL 值的唯一列作为聚簇索引的索引键；
- 在上面两个都没有的情况下，InnoDB 将自动生成一个隐式自增 id 列作为聚簇索引的索引键；

一张表只能有一个聚簇索引，为了实现非主键字段的快速索引，就引出了二级索引（非聚簇索引/辅助索引），它也是利用了B+树的数据结构，但是二级索引的叶子节点存放的是主键值，不是实际数据。

#### 什么字段适合做主键？

- 字段具有唯一性，且不能为空。

- 字段最好是有递增的趋势，如果字段的值是随机无序的，可能会引发页分裂的问题，造成性能影响。

- 不建议用业务数据做主键，例如会员卡号、订单号、学生号之类的。因为我们无法预测未来会不会因为业务需要，而出现业务字段重复或复用的情况。

- 通常情况下会用自增字段来做主键，对于单机系统来说是没问题的，但是如果有多台服务器，各自都可以录入数据，那就不一定使用了。因为如果每台机器各自产生的数据需要合并，就可能会出现主键重复的问题，这时候就要考虑分布式id的方案了。

  ---------------

#### 性别字段能加索引吗？为什么？

不建议针对性别字段加索引。

这实际上与索引创建规则之一区分度有关，性别字段，假设有100w数据，50w男以及50w女，区别度几乎等于0.

区分度的计算方式为：select count(DISTINCT sex)/count(*) from sys_user

实际上对于性别字段不适合创建索引，是因为select *操作，还得进行50w次回表操作，根据主键从聚簇索引中找到其他字段，这一部分开销从上面的测试来说还是比较大的，所以从性能角度来看不建议性别字段加索引，加上索引并不是索引失败，而是回表操作使其很慢。

既然走索引的查询的成本比全表扫描高，优化器就会选择全表扫描的方向进行查询，这时候建立的性能字段索引就没有起到加快查询的作用，反而还因为创建了索引占用空间。

---------

#### 表中十个字段，你主键是用自增ID还是UUID，为什么？

用的是自增Id。

因为uuid相对于 顺序的自增id来说是毫无规律可言的， 新行的值不一定要比之前的主键的值要大，所以innoDB无法做到总是把新行插入到索引的最后，而是需要为新行重新寻找合适的位置从而来分配新的空间。

这个过程需要做很多额外的操作，数据的毫无顺序会导致数据分布散乱，将会导致以下的问题：

- 写入的目标也很可能已经刷新到磁盘上并且从缓存上移除，或者还没有被加载到缓存中，InnoDB在插入之前不得不先找到病从磁盘读取目标页到内存中，这将导致大量的随机IO。

- 因为写入的顺序是乱序的，InnoDB不得不频繁的做页分裂操作，以便为新的行分配存储空间。页分裂导致移动大量的数据，影响性能。
- 由于频繁的页分裂，页会变得稀疏并被不规则的填满，最终会导致数据会有碎片。

结论：使用InnoDB应该尽可能按主键的自增顺序插入，并且尽可能使用单调增加的聚簇键的值来插入新行。

-------------

#### 什么自增ID更快一些，UUID不快吗，它在B+树里存储的是有序的吗？

自增的主键的值是顺序的，所以InnoDB把每一条记录都存储在一条记录的后面，所以自增id更快的原因：

- 下一条记录就会写入新的页中，一旦数据按照这种顺序的方式被加载，主键页就会近乎于顺序的记录填满，提升了页面的最大填充率，不会有页的浪费。

- 新插入的行一定会在原有的最大数据行下一行，mysql定位和寻址的很快，不会因为计算新航的位置而做出额外的消耗。
- 减少了页分裂和碎片的产生。

但是UUID不是递增的，MySQL中索引的数据结构是B+树，这种数据结构的特点是索引树上的节点的数据是有序的，而如果使用UUID作为主键，那么每次插入数据时，因为无法保证每次产生的UUID有序，所以就会出现新的UUID需要插入到索引树的中间去，这样可能会频繁地导致页分裂，使性能下降。

而且，UUID太占用内存。每个UUID由36个字符组成，在字符串进行比较时，需要从前往后比较，字符串越长，性能越差。另外字符串越长，占用的内存越大，由于页的大小是固定的，这样一个页上能存放的关键字数量就会减少，这样最终就会导致索引树的高度越大，在索引搜索的时候，发生的磁盘IO次数越多，性能越差。

------------------

#### MySQL中的索引是怎么实现的？

MySQL InnoDB引擎是用了B+树作为索引的数据结构。

B+树是一种多叉树，叶子节点才存放数据，非叶子节点只存放索引，而且每个节点里的数据是按照主键顺序存放的。每一层父节点的索引值都会出现在下层子节点的索引值中，因此在叶子节点中，包含了所有的索引值信息，并且每一个叶子节点都有两个指针，分别指向下一个叶子节点和上一个叶子节点，形成一个双向链表。

主键索引的B+树如图所示：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5173292f266c41feabd6cfc009861174.png)

比如，我们执行了下面这条查询语句：

```
select * from product where id= 5;
```

这条语句使用了主键索引查询 id 号为 5 的商品。查询过程是这样的，B+Tree 会自顶向下逐层进行查找：

- 将 5 与根节点的索引数据 (1，10，20) 比较，5 在 1 和 10 之间，所以根据 B+Tree的搜索逻辑，找到第二层的索引数据 (1，4，7)；

- 在第二层的索引数据 (1，4，7)中进行查找，因为 5 在 4 和 7 之间，所以找到第三层的索引数据（4，5，6）；
- 在叶子节点的索引数据（4，5，6）中进行查找，然后我们找到了索引值为 5 的行数据。

数据库的索引和数据都是存储在硬盘的，我们可以把读取一个节点当作一次磁盘 I/O 操作。那么上面的整个查询过程一共经历了 3 个节点，也就是进行了 3 次 I/O 操作。
B+Tree 存储千万级的数据只需要 3-4 层高度就可以满足，这意味着从千万级的表查询目标数据最多需要 3-4 次磁盘 I/O，所以B+Tree 相比于 B 树和二叉树来说，最大的优势在于查询效率很高，因为即使在数据量很大的情况，查询一个数据的磁盘 I/O 依然维持在 3-4次。

----------

#### 查询数据时，到了B+树的叶子节点，之后的查找数据是如何做？（不懂⭐️⭐️）

数据页中的记录按照「主键」顺序组成的单向链表。 单项链表的特点就是插入、删除非常方便，但是检索效率不高，最差的情况下需要遍历链表上的所有节点才能完成检索。

因此，数据页中有一个页目录，起到记录的索引作用，就像我们的书那样，针对书中内容的每个章节设立了一个目录，想看某个章节时，可以查看目录，快速找到对应的章节的页，而数据页中的页目录就是为了能够快速找到记录。

-----------

#### 那InnoDB是如何给记录创建页目录的呢？（不懂⭐️⭐️）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/762c32cac449471bbfbfca06b00da191.png)

页目录创建的过程如下：

1. 将所有的记录划分成几个组，这些记录包含最小记录和最大记录，但不包括标记为“已删除”的记录；

2. 每个记录组的最后一条记录就是组内最大的那条记录，并且最后一条记录的头信息中会存储该组一共有多少条记录，作为n_owned字段（上图中粉红色字段）
3. 页目录用来存储每组最后一条记录的地址偏移量，这些地址偏移量会按照先后顺序存储起来，每组的地址偏移量也被称之为槽（slot），每个槽相当于指针指向了不同组的最后一个记录。

从图中可以看到，页目录就是由多个槽组成的，槽相当于分组记录的索引。

-------

#### B+树的特性是什么？

- 所有叶子节点都在同一层： 这确保了所有数据项的检索都具有相同的I/O延迟，提高了搜索效率。每个叶子节点都包含指向相邻叶子节点的指针，形成一个链表。由于叶子节点之间的链接，B+树非常适合进行范围查询和排序扫描。可以沿着叶子节点的链表顺序访问数据，而无需进行多次随机访问。

- 非叶子节点存储键值： 非叶子节点仅存储键值和指向子节点的指针，不包含数据记录。这些键值 用于指导搜索路径，帮助快速定位到正确的叶子节点。并且， 由于非叶子节点只存放键值，当数据量比较大时，相对于B树，B+树的层高更少，查找效率也就更高。

- 叶子节点存储数据记录： 与B树不同，B+树的叶子节点存放实际的数据记录或指向数据记录的指针。这意味着每次搜索都会到达叶子节点，才能找到所需数据。

- 自平衡： B+树在插入、删除和更新操作后会自动重新平衡，确保树的高度相对稳定，从而保持良好的搜索性能。每个节点最多可以有M个子节点，最少可以有ceil(M/2)个子节点（除根节点外）。M为树的阶数。

- ------

说说B+树和B树的区别

- 在B+树中，数据都存储到叶子节点上，而分支节点只存储索引信息；B树的非叶子节点即存储索引也存储部分数据

- B+树的叶子节点使用链表相连，便于范围查找和顺序访问，B树的叶子节点没有链表连接。

- B+树的查找性能更稳定，每次查找时都需要查找到叶子节点；而B树的查找可能会在分支节点找到数据，性能相对不稳定。

- ----

#### B+树的好处是什么？

B树和B+树都是通过多叉树的方式，会将树的高度变矮，所以这两个数据结构非常适合检索存于磁盘中的数据。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e6fcebcd3b774d538f2ddaccd1ea5e19.png)

但是MySQL默认存储引擎InnoDB采用的是B+作为索引的数据结构，原因如下：

- B+树的分支节点不存放实际的记录数据，仅存放索引，因此数据量相同的情况下，相比存储即存索引又存记录的B树，B+树的分支节点可以存放更多的索引，因此B+树可以比B树更「矮胖」，查询底层节点的磁盘I/O次数会更少。

- B+树有大量的冗余节点（所有的分支节点都是冗余索引），这些冗余索引会让B+树在插入、删除的效率都更高，比如删除根节点的时候，不会像B树那样发生复杂的树的变化。

- B+树叶子节点之间用链表连接起来，有利于范围查询，而B树要实现范围查询只能通过树的遍历来完成范围查询，这会设计多个节点的磁盘I/O操作，范围查询效率不如B+树。

  ----

  #### B+树的叶子节点链表是单向的还是双向的？

双向的，为了实现倒序遍历或者排序。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/444c8c419a0c4df18b790aa2f3253f64.png)

- 
  B+ 树的叶子节点之间是用「双向链表」进行连接，这样的好处是既能向右遍历，也能向左遍历。

- B+ 树的节点内容是数据页，数据页里存放了用户的记录以及各种信息，每个数据页默认大小是 16 KB。

InnoDB根据索引类型不同，分为聚集和二级索引。他们的区别在于，聚集索引的叶子节点存放的是实际数据，所有完整的用户记录都存放在聚集索引的叶子节点上，而二级索引的叶子节点存放的是主键值，而不是实际数据。

因为表的数据都是存放着聚集索引的叶子节点里，所以InnoDB存储引擎一定会为表创建一个聚集索引，且由于数据在物理上只会保存一份，所以聚簇索引只能有一个，而二级索引可以创建多个。

-----

#### MySQL为什么用B+树结构，和其他结构比的优点？

- B+Tree vs B Tree： B+Tree 只在叶子节点存储数据，而 B 树 的非叶子节点也要存储数据，所以 B+Tree 的单个节点的数据量更小，在相同的磁盘 I/O 次数下，就能查询更多的节点。另外，B+Tree 叶子节点采用的是双链表连接，适合 MySQL 中常见的基于范围的顺序查找，而 B 树无法做到这一点。

- B+Tree vs 二叉树： 对于有 N 个叶子节点的 B+Tree，其搜索复杂度为O(logdN)，其中 d 表示节点允许的最大子节点个数为 d 个。在实际的应用当中， d 值是大于100的，这样就保证了，即使数据达到千万级别时，B+Tree 的高度依然维持在 3~4 层左右，也就是说一次数据查询操作只需要做 3~4 次的磁盘 I/O 操作就能查询到目标数据。而二叉树的每个父节点的儿子节点个数只能是 2 个，意味着其搜索复杂度为 O(logN)，这已经比 B+Tree 高出不少，因此二叉树检索到目标数据所经历的磁盘 I/O 次数要更多。

- B+Tree vs Hash： Hash 在做等值查询的时候效率贼快，搜索复杂度为 O(1)。但是 Hash 表不适合做范围查询，它更适合做等值的查询，这也是 B+Tree 索引要比 Hash 表索引有着更广泛的适用场景的原因

  -----

#### 为什么 MysSQL 不用 跳表？

B+树的高度在3层时存储的数据可能已经到千万级别，但是对于跳表而言同样去维护千万的数据量造成的跳表层数过高而导致的磁盘io次数增多，也就是使用B+树在存储同样的数据下磁盘io次数更少。

#### 联合索引的实现原理？

将多个字段组合成一个索引，该索引就被称为联合索引。

比如，将商品表中的product_no和name字段组合成联合索引(product_no, name)，创建联合索引的方式如下：

```
CREATE INDEX index_product_no_name ON product(product_no, name);
```

联合索引（）的B+树示意图如下：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e0c886573f4d48e998b821f1d50f4225.png)

可以看到，联合索引的分支节点用两个字段的值作为B+树的key值。当在联合索引查询数据时，先按product_no字段比较，在product_no相同的情况下再按name字段比较。

也就是说，联合索引查询的B+树是先按照product_no进行排序，在product_no相同的情况下再按照name字段排序。

因此使用联合索引时，存在「最左匹配原则」，也就是按照最左优先的方式进行索引的匹配。在使用联合索引进行查询时，如果不遵循「最左匹配原则」，联合索引就会失效，这样就无法利用索引快速查询的特性了。

比如，如果创建了一个（a，b，c）联合索引，条件是以下几种就可以匹配联合索引：

- where a=1；

- where a=1 and b=2 and c=3；
- where a=1 and b=2；

需要注意的是，因为有查询优化器，所以a字段在where语句的顺序并不重要。
但是如果查询条件是以下几种，因为不符合最左匹配原则就无法匹配上联合索引，联合索引就会失效：

- where b=2；

- where c=3；
- where b=2 and c=3；

上面查询条件失效的原因是因为（a，b，c）是联合索引，是先按a排序，在a相同的情况下再按b排序，在b相同的情况下再按c排序。所以b和c是全局无序，局部相对有序的。这样在没有遵循最左匹配原则的情况下，是无法利用到索引的。
我这里举联合索引（a，b）的例子，该联合索引的 B+ Tree 如下：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ba38d7af032a475a855bfe20f8598ff3.png)

可以看到，a 是全局有序的（1, 2, 2, 3, 4, 5, 6, 7 ,8），而 b 是全局是无序的（12，7，8，2，3，8，10，5，2）。因此，直接执行where b = 2这种查询条件没有办法利用联合索引的，利用索引的前提是索引里的 key 是有序的。
只有在 a 相同的情况才，b 才是有序的，比如 a 等于 2 的时候，b 的值为（7，8），这时就是有序的，这个有序状态是局部的，因此，执行where a = 2 and b = 7是 a 和 b 字段能用到联合索引的，也就是联合索引生效了。

--------

#### 创建联合索引时需要注意什么？

建立联合索引时的字段顺序，对索引效率也有很大影响。越靠前的字段被用于索引过滤的概率越高，实际开发中建立联合索引时，要把区分度大的字段排在前面，这样区分度大的字段越有可能被更多的SQL使用到。

区分度就是某个字段column不同值的个数 除以 表的总行数，计算公式如下：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a105f8e9c52947fd9fa9cec36620adee.png)

比如，性别的区分度就很小，不适合做索引后排在联合索引列的靠前的位置，而UUID这类字段就比较适合做索引后排在联合索引列的靠前位置。

因为如果索引的区分度很小，假设字段的值分布均匀，name无论搜索哪个值都可能得到一半的数据。这种情况还不如不要索引。因为 MySQL 还有一个查询优化器，查询优化器发现某个值出现在表的数据行中的百分比（惯用的百分比界线是"30%"）很高的时候，它一般会忽略索引，进行全表扫描。

--------

#### 联合索引ABC，现在有个执行语句是A = XXX and C < XXX，索引怎么走

根据最左匹配原则，A可以走联合索引，C不会走联合索引，但是C可以走索引下推

#### 联合索引(a,b,c) ，查询条件 where b > xxx and a = x 会生效吗

索引会生效，a和b字段都能利用联合索引，符合联合索引最左匹配原则。

#### 联合索引 (a, b，c)，where条件是 a=2 and c = 1，能用到联合索引吗？

会用到联合索引，但是只有a才能走索引，c无法走，因为不符合「最左匹配原则」。虽然c无法走索引，但是c字段在5.6版本后，会有索引下推的优化，能减少回表查询的次数。

---

#### 索引失效有哪些情况？

6钟情况会发生索引失效：

- 当我们使用左或左右模糊匹配时，即like %xx 或 like %xx%这两种方式都会造成索引失效；

- 当我们在查询条件中对索引列使用函数，就会导致索引失效。

- 当我们在查询条件中对索引列进行表达式计算，也会导致索引失效。

- MySQL在遇到字符串和数组比较时，会自动把字符串转为数字，然后再进行比较。如果字符串是索引列而条件语句中输入参数是数字的话，那索引列就会发生隐式类型转换，由于隐式类型转换是通过CAST函数实现的，等同于对索引列使用了函数，所以也会导致索引失效。

- 联合索引要能正确使用需要遵从最左匹配原则，也就是按照最左优先的方式进行索引的匹配，否则就会导致索引失效。

- 在WHERE子句中，如果在OR前的条件是索引列，而在OR后的条件列不是索引列，那么索引会失效。

  ----

#### 什么情况下会回表查询

从物理存储的角度来看，索引分为聚簇索引（主键索引）、二级索引（辅助索引）。

它们的主要区别如下：

- 主键索引的B+树的叶子节点存放的是实际数据，所有完整的用户记录都存放在主键索引的B+树的叶子节点里。

- 二级索引的B+树的叶子节点存放的是主键值，而不是实际数据。

所以，在查询时使用了二级索引，如果查询的数据能在二级索引里查询得到，则不需要回表，这个过程就是覆盖索引

如果查询的数据不在二级索引里，就会先检索二级索引，找到对应的叶子节点，获取到主键值后，然后再检索主键索引，就能查询到数据了。这个过程就叫回表

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4d318243a8e249a783406ab687be446e.png)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6702c777796e422bb9cc3e7d014db418.png)

--------

#### 什么是覆盖索引：

覆盖索引指的是一个索引包含了查询所需的所有列，因此不需要访问表中的数据行就能完成查询。

换句话说，查询所需的所有数据都能从索引中直接获取，而不需要进行回表查询。覆盖索引能够显著提高查询性能，因为减少了访问数据页的次数，从而减少了I/O操作。

假设有一张表 employees，表结构如下：

```
CREATE TABLE employees (
  id INT PRIMARY KEY,
  name VARCHAR(100),
  age INT,
  department VARCHAR(100),
  salary DECIMAL(10, 2)
);

CREATE INDEX idx_name_age_department ON employees(name, age, department);
```

如果我们有以下查询：

```
SELECT name, age, department FROM employees WHERE name = 'John';
```

在这种情况下，idx_name_age_department 是一个覆盖索引，因为它包含了查询所需的所有列：name、age 和 department。（即这个索引包含了数据库中的字段，根据索引就可以直接获取结果，而不需要再去查看表中的实际数据行）。查询可以完全在索引层完成，而不需要访问表中的数据行。

---------

#### 如果一个列即是单列索引，又是联合索引，单独查他的话先走哪个索引？

mysql优化器会分析每个索引的查询成本，然后选择成本最低的方案来执行sql。

如果单列索引是a，联合索引是（a, b），那么针对下面的查询：

```
select a, b from table where a = ? and b =?
```

优化器会选择联合索引，因为查询成本更低， 查询也不需要回表了，直接索引覆盖了。

-----

#### 索引字段是不是越多越好？

不是，建的越多会占用越多的空间，而且在写入频繁的场景下，对于B+树的维护所付出的性能消耗也会越大。

#### 如果一个字段的status是0或1，适合建立索引吗？

不适合，区分度低的字段不适合建立索引。

--------

#### 索引的优缺点？

索引最大的好处是提高查询速度，但是索引也是有缺点的，如：

- 需要占用物理空间，数量越大，占用空间越大。

- 创建索引和维护索引要耗费时间，这种时间随着数据量的增大而增大；

- 会降低表的增删改的效率，因此每次增删改索引，B+树为了维护索引有序性，都需要进行动态维护。

  所以，索引不是万能钥匙，它也是根据场景来使用的。

-----

#### 怎么决定建立哪些索引？

#### 什么时候适用索引？

- 字段有唯一性限制的，比如商品编码；
- 经常用于WHERE查询条件的字段，这样能够提高整个表的查询速度，如果查询条件不是一个字段，可以建立联合索引。
- 经常用于GROUP BY和ORDER BY的字段，这样在查询的时候就不需要再去做一次排序了。因为我们都已经知道了建立索引之后在B+树中的记录都是排好序的，不需要再去做一次排序了。![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/3f7325af2a9a48f4abee1456599e70c2.png)



----------

#### 什么时候不需要创建索引？

- WHERE，GROUP BY，ORDER BY里用不到的字段。索引的价值是快速定位，如果起不到定位的字段通常是不需要创建索引的，因为索引是会占据物理空间的。

- 字段中存在大量重复数据，不需要创建索引。比如性别字段，只有男女。如果数据库表中，男女的记录分布均匀，那么无论搜索哪个值都可能得到一半的数据。在这些情况下，还不如不要索引。因为MySQL还有一个查询优化器，查询优化器发现某个值出现在表的数据行中的百分比很高的时候，他一般会忽略索引，进行全表扫描。
- 表数据太少，不需要创建索引。
- 经常更新的字段不用创建索引，比如不要对电商项目的用户余额建立索引，因为索引字段频繁修改。

**索引优化详细讲讲**

常见优化索引的方法：

- 前缀索引优化：使用前缀索引是为了减少索引字段的大小，可以增加一个索引页中存储的索引值，有效提高索引的查询速度。在一些大字符串的字段作为索引时，使用前缀索引可以帮助我们减少索引项的大小。

- 覆盖索引优化：覆盖索引是指SQL中query的所有字段，在索引B+树的叶子节点上都能找得到那些索引，从二级索引中查询得到记录，而不需要通过聚簇索引查询获得，可以避免回表的操作。
- 主键索引最好是自增的：

​	1、如果我们使用自增主键，那么每次插入的新数据就会按照顺序添加到当前索引节点的位置，不需要移动已有的数据，当页面写满时，会自动开辟一个新页面。因为每次插入一条新纪录，都是追加操作，不需要移动已有的数据。当页面写满时，就会自动开辟一个新页面。因为每次插入一条新记录，都是追加操作，不需要重新移动数据。 因此这种插入数据的方法效率非常高。

​	2、如果我们使用非自增主键，由于每次插入主键的索引值都是随机的，因此每次插入新的数据时，就可能会插入到现有数据页中间的某个位置，这将不得不移动其他数据来满足新数据的插入，甚至需要从一个页面复制数据到另一个页面，我们通常将这种情况称为页分裂。页分裂还有可能会造成大量的内存碎片，导致索引结构不紧凑，从而影响查询效率。

- 防止索引失效：

1. 当我们使用左或左右模糊匹配的时候，即like %xx或者like %xx%这两种方式都会造成索引失效。

2. 当我们在查询条件中对索引列做了计算、函数、类型转换操作，这些情况下都会造成索引失效；

3. 联合索引要能正确使用需要遵循最左匹配原则，也就是按照最左优先的方式进行索引的匹配，否则就会导致索引失效。

4. 在WHERE语句汇总，如果在OR前面的条件是索引列而OR后面的条件列不是索引列，那么索引会失效。

   -------

#### 了解过前缀索引吗

使用前缀索引是为了见效索引字段的大小，可以增加一个索引页中存储的索引值，有效提高索引的查询速度。在一些大字符串的字段作为索引时，使用前缀索引可以帮助我们减小索引项的大小。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e9151f40f6904387996950b4eea2bb74.png)

### 事务

#### 事务的特性是什么，如何实现的？

- 原子性（Atomicity）： 一个事务中的所有操作，要么全部完成，要么全部不完成，不会阶数在中间某个环节，而且事务在执行过程中发生错误，会被回滚到事务开始时的状态，就像这个事务没有被执行过一样。

- 一致性（Consistency）： 是指事务操作前和操作后，数据满足完整性约束，数据库保持一致性状态。比如，用户 A 和用户 B 在银行分别有 800 元和 600 元，总共 1400 元，用户 A 给用户 B 转账 200 元，分为两个步骤，从 A 的账户扣除 200 元和对 B 的账户增加 200 元。一致性就是要求上述步骤操作后，最后的结果是用户 A 还有 600 元，用户 B 有 800 元，总共 1400 元，而不会出现用户 A 扣除了 200 元，但用户 B 未增加的情况（该情况，用户 A 和 B 均为 600 元，总共 1200 元）。
- 隔离性（Isolation）： 数据库运行多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致，因为多个事务同事使用相同的数据时，不会相互干扰，每个事务都要一个完整的数据空间，对其他并发事务是隔离的。也就是手，消费者购买商品这个事务，是不会影响其他消费者购买的。
- 持久性（Durability）： 事务处理结束后，对数据的修改就是永久的，即使系统故障也不会丢失。

#### MySQL InnoDB引擎通过什么技术来保证事务的四个特性呢？

- 持久性是通过redo log（重做日志）来保证的 。

- 原子性是通过undo log（回滚日志）来保证的
- 隔离性是通过MVCC（多版本并发控制）或锁机制来保证的
- 一致性是通过持久性+原子性+隔离性来保证的。

#### mysql可能出现什么和并发相关问题？

MySQL 服务端是允许多个客户端连接的，这意味着 MySQL 会出现同时处理多个事务的情况。

那么在同时处理多个事务的时候，就可能出现脏读（dirty read）、不可重复读（non-repeatable read）、幻读（phantom read）的问题。

```
脏读：
如果在一个事务「读到」了另一个「未提交事务修改过的数据」，就意味着发生了「脏读」现象。

不可重复读：
在一个事务内 多次读取同一个数据，如果出现前后两次读到的数据不一样的情况，就意味着发生「不可重复读」现象

幻读：
在一个事务内多次查询某个符合查询条件「记录数量」，如果出现前后查询到的记录数量不一致，则出现了幻读。
```

#### 哪些场景不适合脏读，举例说明？

脏读是指一个事务在读取到另一个事务未提交的数据时发生。脏读可能会导致不一样的数据被读取

- 银行系统： 如果一个账户的余额正在被调整但未提交，另一个事务读取到了这个临时的余额，就会导致客户看不到正确的余额。

- 库存管理系统： 如果一个商品的数量正在被更新但尚未提交，另一个事务读取了这个临时的数量，就会导致库存管理错误。
- 在线订单系统： 如果一个订单正在被修改但未提交，另一个事务读取了这个临时的订单状态，可能导致订单状态显示错误，客户收到不准确的信息。

#### mysql是怎么解决并发问题的？

- 锁机制：MySQL提供了多种锁机制来保证数据的一致性，包括行级锁、表级锁，页级锁等。通过锁机制，可以在读写操作时对数据进行加锁，确保同事只有一个操作能够访问或修改数据。

- 事务隔离级别：MySQL提供了多种事务隔离级别，包括读未提交、读已提交、可重复度和串行花。通过设置合适的事务隔离级别，可以在多个事务并发执行时，控制事务之间的隔离程度，以避免数据不一致的问题。

- MVCC（多版本并发控制）：MySQL使用MVCC来管理并发访问。它通过在数据库中保存不同版本的数据来实现不同事务之间的隔离。在读取数据时，MySQL会根据事务的隔离级别来选择合适的数据版本，从而保证数据的一致性。

  -------

#### 事务的隔离级别有哪些？

- 读未提交（read uncommited）： 一个事务还没提交时，它做的变更就能被其他事务看到。

- 读已提交（read commited）： 指一个事务提交后，它做的变更才能被其他事务看到。

- 可重复读（repeatable read）: 指一个事务执行过程中看到的数据，一直与这个事务启动时看到的数据是一致的，MySQL InnoDB引擎的默认隔离级别是可重复度。

- 串行化（serializable）： 会对记录加上读写锁，在多个事务对这条记录进行读写操作时，如果发生了读写冲突的时候，后访问的事务必须等前一个事务执行完成，才能继续执行；

- 在「读未提交」隔离级别下，可能发生脏读、不可重复读和幻读现象；

- 在「读已提交」隔离级别下，可能发生不可重复读和幻读，不会发生脏读

- 在「可重复读」隔离级别下，可能发生幻读，但不会发生脏读和不可重复读

- 在「串行化」隔离级别下，脏读、不可重复读、幻读都不会发生。


举例：
有一张账户余额表，里面有一条账户余额为 100 万的记录。然后有两个并发的事务，事务 A 只负责查询余额，事务 B 则会将我的余额改成 200 万，下面是按照时间顺序执行两个事务的行为：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2553841686164e2784e62f9cbf96dae5.png)

- 在「读未提交」隔离级别下，事务 B 修改余额后，虽然没有提交事务，但是此时的余额已经可以被事务 A 看见了，于是事务 A 中余额 V1 查询的值是 200 万，余额 V2、V3 自然也是 200 万了；

- 在「读提交」隔离级别下，事务 B 修改余额后，因为没有提交事务，所以事务 A 中余额 V1 的值还是 100 万，等事务 B 提交完后，最新的余额数据才能被事务 A 看见，因此额 V2、V3 都是 200 万；

- 在「可重复读」隔离级别下，事务 A 只能看见启动事务时的数据，所以余额 V1、余额 V2 的值都是 100 万，当事务 A 提交事务后，就能看见最新的余额数据了，所以余额 V3 的值是 200 万；

- 在「串行化」隔离级别下，事务 B 在执行将余额 100 万修改为 200 万时，由于此前事务 A 执行了读操作，这样就发生了读写冲突，于是就会被锁住，直到事务 A 提交后，事务 B 才可以继续执行，所以从 A 的角度看，余额 V1、V2 的值是 100 万，余额 V3 的值是 200万。

  -------

#### 这四种隔离级别具体是如何实现的？

- 对于「读未提交」，因为可以直接读取到未提交事务修改的数据，所以直接读取最新的数据。
- 对于「读已提交」和「可重复读」，他们是通过Read View实现的，他们的区别在于创建Read View的实际不同：「读已提交」是在「每个语句执行前」都会重新生成一个Read View，而「可重复读」是「启动事务时」生成一个Read View，然后整个事务期间都在用这个Read View。
- 对于「串行化」隔离级别事务来说，通过加读写锁的方式来避免并行访问；

------

#### MySQL默认隔离级别是什么？

可重复读隔离级别

#### 可重复度隔离级别下，A事务提交的数据，在B事务能看见吗？

可重复读隔离级别是由MVCC（多版本并发控制）实现的，实现的方式是开始事务后（执行begin语句后），在执行第一个查询语句后，会创建一个Read View，后续的查询语句利用这个Read View，通过这个Read View就可以在undo log版本链找到事务开始时的数据，所以事务过程中每次查询的数据都是一样的，即使中途有其他事务插入了新纪录，是查询不出来这个数据的。

#### 举例说明可重复读下的幻读问题

可重复读隔离级别下虽然很大程度上解决了幻读，但是还是没有能完全解决幻读。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1ef85e24d5254212aafff731eda3f0e2.png)

事务A执行查询id=5的记录，此时表中是没有该记录的，所以查询不出来。

然后事务 B 插入一条 id = 5 的记录，并且提交了事务。

此时事务A更新id=5这条记录，是的，事务A虽然看不到id=5这条记录，但是他去更新了这条记录。这场景确实太他妈违和了，然后在此查询id=5的记录，事务A就能看到事务B插入的记录了。幻读就是发生在这种违和的场景中。

整个幻读的时序图如下：

在可重复读隔离级别下，事务A第一次执行普通的select语句时生成一个ReadView，之后事务B向表中新插入了一条id=5的记录并提交。接着，事务A对id=5这条记录进行了更新，在这个时刻，这条新纪录trx_id隐藏列的值就变成了事务A的事务id（trx_id表示最近一次修改此记录的事务 ID，会被更新为事务 A 的事务 ID），之后事务A再使用普通select语句去查询就可以看到这条记录了，就发生了幻读。

因为这种特殊现象的存在，所以我们认为MySQL InnoDB中的MVCC并不能完全避免幻读现象。

----

#### Mysql 设置了可重复 读隔离级后，怎么保证不发生幻读？

尽量在开启事务后，马上执行select … for update这类锁定读（重点是锁定读）的语句，因为它会对记录加next-key lock，从而避免其他事务插入一条新纪录，就避免了幻读的问题。
select... for update 是锁定读
读的总结：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/61df1805d34a4b9c8ba58948afe738a0.png)

---------

#### 串行化隔离级别是通过什么实现的？

是通过行级锁实现的，序列化（串行化）隔离级别下，普通的select查询是会对记录加S型的next-key锁，其他事务就没办法对这些已经加锁的记录来增删改了，从而避免了脏读、不可重复读和幻读现象。

----------

#### 介绍MVCC实现原理（好多 记不住⭐️⭐️）

MVCC允许多个事务同事读取同一行数据，而不会彼此阻塞。每个事务看到的数据版本是该事务开始时的数据版本。这意味着，如果其他事务在此期间修改了数据，正在运行的事务仍然看到的是它开始时的数据状态，从而实现了非阻塞读操作。

对于「读已提交」和「可重复度」隔离级别的事务来说，他们是通过Read View来实现的，他们的区别在于创建Read View的时机不同。大家可以把Read View理解成一个数据快照，就像相机拍照那样，定格某一刻的风景。

- 「读已提交」是在「每个select语句执行前」都会重新生成一个Read View；

- 「可重复读」是在执行第一条select时，生成一个Read View，然后整个事务期间都在用这个Read View。

Read View 有四个重要的字段：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7da40da346f543be81d2ff7e05d7ec5a.png)

- 
  m_ids：指的是在创建Read View时，当前数据库中「活跃事务」的事务id列表。 注意是一个列表。“活跃事务”指的是，启动了但还没提交的事务。

- min_trx_id：指的是在创建Read View时，当前数据库中「活跃事务」中事务id最小的事务，也就是m_ids的最小值。
- max_trx_id：这个并不是m_ids的最大值，而是创建Read View时当前数据库中应该给下一个事务的id值，也就是全局事务中最大的事务id值+1；
- creator_trx_id：指的是创建该Read View的事务的事务id

对于使用InnoDB存储引擎的数据库表，它的聚簇索引记录中都包含下面两个隐藏列：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0af7306255c144d5b6089140d63655c2.png)

- 
  trx_id：当一个事务对某条聚簇索引记录进行改动时，就会把该事务的事务id记录着trx_id隐藏列里；

- roll_pointer：每次对某条局促索引进行改动是，都会把旧版本的记录写入到undo日志中，然后这个隐藏列是个指针，指向每一个旧版本记录，于是就可以通过他找到修改前的记录。

在创建Read View后， 我们可以将记录中的trx_id划分为三种情况：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2bb651bff49e4a3e9d18dda3d8db34be.png)

一个事务去访问记录的时候，除了自己的更新记录总是可见之外，还有这几种情况：

- 如果记录的trx_id值小于Read View中的min_trx_id值，表示这个版本的记录是在创建Read View 前 已经提交的事务生成的，所以该版本的记录对当前事务可见。

- 如果记录的trx_id值大于等于Read View中的max_trx_id值，表示这个版本的记录是在创建Read View后才启动的事务生成的，所以该版本的记录对当前事务不可见。
- 如果记录的trx_id值在Read View的min_trx_id和max_trx_id之间，需要判断trx_id是否在m_ids列表中：
  1. 如果改记录的trx_id值在 m_ids列表中，表示生成该版本记录的活跃事务依然活跃着（还没提交事务），所以该版本的记录对当前事务不可见。
  2. 如果记录的trx_id 不在 m_ids列表中，表示生成该版本记录的活跃事务已经被提交，所以该版本的记录对当前事务可见。

这种通过「版本链」来控制并发食物访问同一个记录时的行为就叫MVCC（多版本并发控制）。

----

#### 一条update是不是原子性的？为什么？

是原子性的。主要通过锁+undolog日志保证原子性的。

- 执行 update时，会加行级别锁，保证了一个事务更新一条记录的时候，不会被其他事务干扰。

- 事务执行过程中，会生成undolog，如果事务执行失败，就可以通过undolog日志进行回滚。

--------

#### 滥用事务，或者一个事务里有特别多sql的弊端？（不懂⭐️）

事务的资源在事务提交之后才会释放，比如存储资源、锁。

- 如果一个事务特别多sql，那么会带来这些问题：

- 如果一个事务特别多sql，锁定的数据太多，容易造成大量的死锁和锁超时。
  回滚记录会占用大量存储空间，事务回滚时间长。在MySQL中，实际上每条记录在更新的时候都会同时记录一条回滚操作。记录上的最新值，通过回滚个操作，都可以得到钱一个状态的值。sql越多，所需要保存的回滚数据就越多。
  如果执行时间长，容易造成主从延迟，主库上必须等事务执行完成才会写入binlog，再传给备库。所以，如果一个主库上的语句执行10分钟，那这个事务很可能就会导致从库延迟10分钟。

  --------

  ### 锁

  #### 讲一下mysql中有哪些锁？

在MySQL中，根据加锁的范围，可以分为全局锁、表级锁和行锁 这三类

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ceb308b630584aec9d4fd02ead67d067.png)

- 
  全局锁： 通过flush tables with read lock语句会将整个数据库设为只读状态，这时其他线程执行增删改或表结构修改都会阻塞。全局锁主要应用于做全库逻辑备份，这样在备份数据库期间，不会因为数据或表结构的更新，而出现备份文件和原来不一样的情况。

- 表级锁： MySQL中表级锁有这几种：

  1. 表锁：通过lock tables语句可以对表加表锁，表锁除了会限制别的线程的读写外，也会限制本线程后面的读写操作。
  2. 元数据锁：当我们对数据库表进行操作时，会自动给这个表加上MDL，对一张表进行CRUD操作时，加的是MDL读锁；对一张表做结构变更操作的时候，加的是MDL写锁；MDL是为了保证当用户对表执行CRUD操作时，防止其他线程对这个表结构做变更。
  3. 意向锁：当执行插入、更新、删除操作，需要先对表加上「意向独占锁」，然后对该记录加独占锁。意向锁的目的是为了快速判断表里是否有记录被加锁。

- 行级锁： InnoDB引擎是支持行级锁的，而MyISAM引擎不支持行级锁。
- 记录锁：锁住的是一条记录。记录锁是有S锁和X锁之分的，满足读写互斥，写写互斥。

- 间隙锁：只存在于可重复读个理解别，目的是为了解决可重复读隔离级别下的幻读现象。

- Next-Key Lock称为临键锁，是Record Lock + Gap Lock的组合，锁定一个范围，并且锁定记录本身。

---------

#### 数据库的表锁和行锁有什么作用？

表锁的作用：

- 整体控制：表锁可以用来控制整个表的并发访问，当一个事务获取了表锁时，其他事务无法对该表进行任何读写操作，从而确保数据的完整性和一致性。

- 粒度大：表锁的粒度比较大，在锁定表的情况下，可能会影响到整个表的其他操作，可能会引起锁竞争和性能问题。
- 适用于大批量操作：表锁适合于需要大批量操作表中数据的场景，例如表的重建、大量数据的加载等。

行锁的作用：

- 细粒度控制：行锁可以精确控制对表中某行数据的访问，使得其他事务可以同时访问表中的其他行数据，在并发量大的系统中能够提高并发性能。

- 减少锁冲突：行锁不会像表锁那样造成整个表的锁冲突，减少了锁竞争的可能性，提高了并发访问的效率。
- 适用于频繁单行操作：行锁适合于需要频繁对表中单独行进行操作的场景，例如订单系统中的订单修改、删除等操作。

-------

#### MySQL两个线程的update语句同时处理一条数据，会不会有阻塞？

如果是两个事务同时更新了id=1，比如update … where id=1，那么是会阻塞的。因为InnoDB存储引擎实现了行级锁。

当A事务对id=1这行记录进行更新时，会对主键id=1的记录加X类型的记录锁，这样第二事务对id=1进行更新时，发现已经有记录锁了，就会陷入阻塞状态。

----

#### 两条update语句处理一张表的不同的主键范围的记录，一个<10，一个>15，会不会遇到阻塞？底层是为什么的？（不懂⭐️）

不会，因为锁住的范围不一样，不会形成冲突。

第一条 update sql 的话（ id<10），锁住的范围是（-♾️，10）
第二条 update sql 的话（id >15），锁住的范围是（15，+♾️）

----------

#### 如果2个范围不是主键或索引？还会阻塞吗？（不懂⭐️）

如果两个范围查询的字段不是索引的话，那么代表update没有用到索引，这时候触发了全表扫描，全部索引都会加行级锁，这时候第二条update执行的时候，就会阻塞了。

因为如果update没有用到索引，在扫描过程中会对索引加锁，所以全表扫描的场景下，所有记录都会被加锁，也就是这条update语句产生了4个记录锁和5个间隙锁，相当于锁住了全表。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/55393e807f044d9faea171de57771659.png)

-----

### 日志

#### 日志文件是分成了哪几种？

- redo log 重做日志，是InnoDB存储引擎层生成的日志，实现了事务中的持久性，主要用于掉电等故障恢复。

- undo log 回滚日志，是InnoDB存储引擎层生成的日志，实现了事务中的原子性，主要用于事务回滚和MVCC。

- bin log 二进制日志，是Server层生成的日志，主要用于数据备份和主从复制；

- relay log 中继日志，用于主从复制场景下，slave(从服务器)通过io线程拷贝master(主服务器)的bin log后本地生成的日志

- 慢查询日志，用于记录执行时间过长的sql，需要设置阈值后手动开启

  ----

#### 讲一下bin log（记不住⭐️）

binlog 文件是记录了所有数据库表结构变更和表数据修改的日志，不会记录查询类的操作，比如 SELECT 和 SHOW 操作。

MySQL 在完成一条更新操作后，Server 层还会生成一条 binlog，等之后事务提交的时候，会将该事物执行过程中产生的所有 binlog 统一写 入 binlog 文件，binlog 是 MySQL 的 Server 层实现的日志，所有存储引擎都可以使用。

binlog 是追加写，写满一个文件，就创建一个新的文件继续写，不会覆盖以前的日志，保存的是全量的日志，用于备份恢复、主从复制；

binlog 有 3 种格式类型，分别是 STATEMENT（默认格式）、ROW、 MIXED，区别如下：

- STATEMENT：每一条修改数据的 SQL 都会被记录到 binlog 中（相当于记录了逻辑操作，所以针对这种格式， binlog 可以称为逻辑日志），主从复制中 slave 端再根据 SQL 语句重现。但 STATEMENT 有动态函数的问题，比如你用了 uuid 或者 now 这些函数，你在主库上执行的结果并不是你在从库执行的结果，这种随时在变的函数会导致复制的数据不一致；

- ROW：记录行数据最终被修改成什么样了（这种格式的日志，就不能称为逻辑日志了），不会出现 STATEMENT 下动态函数的问题。但 ROW 的缺点是每行数据的变化结果都会被记录，比如执行批量 update 语句，更新多少行数据就会产生多少条记录，使 binlog 文件过大，而在 STATEMENT 格式下只会记录一个 update 语句而已；
- MIXED：包含了 STATEMENT 和 ROW 模式，它会根据不同的情况自动使用 ROW 模式和 STATEMENT 模式；

----------

#### UndoLog日志的作用是什么？

undo log是一种用于撤销回退的日志，它保证了事务的ACID特性中的原子性（Atomicity）
在事务没提交之前，MySQL会先记录更新前的数据到undo log日志文件里。当事务回滚时，可以利用undo log来进行回滚![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f4d295915890470d98a8acfb67a2c518.png)



每当InnoDB引擎对一条记录进行操作（修改、删除、新增）时，要把回滚时需要的信息都记录到undo log里，比如：

- 在插入 一条记录时，要把这条记录的主键值记下来，这样之后回滚时只需要把这个主键值对应的记录删除 即可。

- 在删除 一条记录时，要把这条记录中的内容都记下来，这样之后回滚时再由这些内容组成的记录插入 到表中即可。
- 在更新 一条记录时，要把被更新的列的旧值记下来，这样之后回滚时再把这些列更新为旧值 即可。

在发生回滚时，就读取 undo log 里的数据，然后做原先相反操作。如delete一条记录时，undo log就会把记录中的内容都记下来，然后执行回滚操作的时候，就读取undo log里的数据，然后进行insert 操作。

------

#### 有了undo log为什么还需要redo log呢？

Buffer Pool 是提高了读写效率没错，但是问题来了，Buffer Pool 是基于内存的，而内存总是不可靠，万一断电重启，还没来得及落盘的脏页数据就会丢失。

为了防止断电导致数据丢失的问题，当有一条记录需要更新的时候，InnoDB 引擎就会先更新内存（同时标记为脏页），然后将本次对这个页的修改以 redo log 的形式记录下来，这个时候更新就算完成了。

后续，InnoDB 引擎会在适当的时候，由后台线程将缓存在 Buffer Pool 的脏页刷新到磁盘里，这就是 WAL （Write-Ahead Logging）技术。

WAL 技术指的是， MySQL 的写操作并不是立刻写到磁盘上，而是先写日志，然后在合适的时间再写到磁盘上。

过程如下图：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/cd1f63f3dfd942caa8ef0a6031b7252d.png)

redo log 是物理日志，记录了某个数据页做了什么修改，比如对 XXX 表空间中的 YYY 数据页 ZZZ 偏移量的地方做了AAA 更新，每当执行一个事务就会产生这样的一条或者多条物理日志。

在事务提交时，只要先将 redo log 持久化到磁盘即可，可以不需要等到将缓存在 Buffer Pool 里的脏页数据持久化到磁盘。

当系统崩溃时，虽然脏页数据没有持久化，但是 redo log 已经持久化，接着 MySQL 重启后，可以根据 redo log 的内容，将所有数据恢复到最新的状态。

redo log 和 undo log 这两种日志是属于 InnoDB 存储引擎的日志，它们的区别在于：

- redo log 记录了此次事务「完成后」的数据状态，记录的是更新之后的值；

- undo log 记录了此次事务「开始前」的数据状态，记录的是更新之前的值；

事务提交之前发生了崩溃，重启后会通过 undo log 回滚事务，事务提交之后发生了崩溃，重启后会通过 redo log 恢复事务，如下图：![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0a3a55f9e8cc4c819071cf35d73c2d6f.png)



所以有了 redo log，再通过 WAL 技术，InnoDB 就可以保证即使数据库发生异常重启，之前已提交的记录都不会丢失，这个能力称为 crash-safe（崩溃恢复）。可以看出来， redo log 保证了事务四大特性中的持久性。

写入 redo log 的方式使用了追加操作， 所以磁盘操作是顺序写，而写入数据需要先找到写入位置，然后才写到磁盘，所以磁盘操作是随机写。

磁盘的「顺序写 」比「随机写」 高效的多，因此 redo log 写入磁盘的开销更小。

针对「顺序写」为什么比「随机写」更快这个问题，可以比喻为你有一个本子，按照顺序一页一页写肯定比写一个字都要找到对应页写快得多。

可以说这是 WAL 技术的另外一个优点：MySQL 的写操作从磁盘的「随机写」变成了「顺序写」，提升语句的执行性能。这是因为 MySQL 的写操作并不是立刻更新到磁盘上，而是先记录在日志上，然后在合适的时间再更新到磁盘上 。

至此， 针对为什么需要 redo log 这个问题我们有两个答案：

- 实现事务的持久性，让MySQL有crash-safe的能力，能够保证MySQL在任何时间段突然崩溃，重启后之前已提交的记录都不会丢失；

- 将写操作从「随机写」变成了「顺序写」，提升MySQL写入磁盘的能力。

  -----

#### redo log怎么保证持久性？（对上面问题的总结更像是）

Redo log是MySQL中用于保证持久性的重要机制之一。它通过以下方式来保证持久性：

- Write-ahead logging（WAL）：在事务提交之前，将事务所做的修改操作记录到redo log中，然后再将数据写入磁盘。这样即使在数据写入磁盘之前发生了宕机，系统可以通过redo log中的记录来恢复数据。

- Redo log的顺序写入：redo log采用追加写入的方式，将redo日志记录追加到文件末尾，而不是随机写入。这样可以减少磁盘的随机I/O操作，提高写入性能。
- Checkpoint机制：MySQL会定期将内存中的数据刷新到磁盘，同时将最新的LSN（Log Sequence Number）记录到磁盘中，这个LSN可以确保redo log中的操作是按顺序执行的。在恢复数据时，系统会根据LSN来确定从哪个位置开始应用redo log。

-----

#### 能不能只用bin log 不用redo log？

不行。bin log是server层的日志，没办法记录哪些赃页还没有刷盘。redo log是存储引擎层的日志，可以记录哪些赃页还没有刷盘，这样崩溃恢复的时候，就能恢复那些还没有被刷盘的赃页数据。

-------

#### bin log 两阶段提交 过程是怎么样的？（没看懂⭐️⭐️）

事务提交之后，redo log和bin log都要持久化到磁盘。但是这两个是独立的逻辑，可能出现半成功的状态，这样就造成两份日志之间的逻辑不一致。

在MySQL的InnoDB存储引擎中，开启bin log的情况下，MySQL会同时维护bin log与InnoDB的redo log，为了保证这两个日志的一致性，MySQL使用了内部XA事务（也有外部XA事务，但和这里不相关），内部XA事务由bin log作为协调者，存储引擎是参与者。

当客户端执行commit语句或在自动提交的情况下，MySQL内部开启一个XA事务，分两阶段来完成XA事务的提交，如下图：![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4cd90d2911ee430abe0e60ca5a1c219a.png)

从图中可看出，事务的提交过程有两个阶段，就是将 redo log 的写入拆成了两个步骤：prepare 和 commit，中间再穿插写入binlog，具体如下：

- prepare阶段： 将 XID（内部 XA 事务的 ID） 写入到 redo log，同时将 redo log 对应的事务状态设置为 prepare，然后将 redo log 持久化到磁盘（innodb_flush_log_at_trx_commit = 1 的作用）；

- commit 阶段： 把 XID 写入到 binlog，然后将 binlog 持久化到磁盘（sync_binlog = 1 的作用），接着调用引擎的提交事务接口，将 redo log 状态设置为 commit，此时该状态并不需要持久化到磁盘，只需要 write 到文件系统的 page cache 中就够了，因为只要 binlog 写磁盘成功，就算 redo log 的状态还是 prepare 也没有关系，一样会被认为事务已经执行成功；

我们来看看在两阶段提交的不同时刻，MySQL 异常重启会出现什么现象？下图中有时刻 A 和时刻 B 都有可能发生崩溃：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2b5f0939f6bd4f02bcf344e629050985.png)

不管是时刻 A（redo log 已经写入磁盘， binlog 还没写入磁盘），还是时刻 B （redo log 和 binlog 都已经写入磁盘，还没写入 commit 标识）崩溃，此时的 redo log 都处于 prepare 状态。

在MySQL重启后，会按顺序扫描redo log文件，碰到处于prepare状态的redo log，就拿着redo log中的XID去binlog查看是否存在此XID：

- 如果 binlog 中没有当前内部 XA 事务的 XID，说明 redolog 完成刷盘，但是 binlog 还没有刷盘，则回滚事务。 对应时刻 A 崩溃恢复的情况。

- 如果 binlog 中有当前内部 XA 事务的 XID，说明 redolog 和 binlog 都已经完成了刷盘，则提交事务。 对应时刻 B 崩溃恢复的情况。

可以看到，对于处于prepare阶段的redo log，既可以提交事务，也可以回滚事务，这取决于是否能在 binlog 中查找到与 redo log 相同的 XID， 如果有就提交事务，如果没有就回滚事务。这样就可以保证 redo log 和 binlog 这两份日志的一致性了。

所以说，两阶段提交是以 binlog 写成功为事务提交成功的标识，因为 binlog 写成功了，就意味着能在 binlog 中查找到与 redo log 相同的 XID。

----

#### update语句的具体执行过程是怎么样的？（记不住）

具体更新一条记录UPDATE t_user SET name = 'xiaolin' WHERE id = 1; 流程如下：

- 执行器负责具体执行，会调用存储引擎的接口，通过主键索引树搜索获取 id = 1 这一行记录：

- ​	如果 id=1 这一行所在的数据页本来就在 buffer pool 中，就直接返回给执行器更新；
- ​	如果记录不在 buffer pool，将数据页从磁盘读入到 buffer pool，返回记录给执行器。
- 执行器得到聚簇索引记录后，会看一下更新前的记录和更新后的记录是否一样：
- ​	如果一样的话就不进行后续更新流程；
- ​	如果不一样的话就把更新前的记录和更新后的记录都当作参数传给 InnoDB 层，让 InnoDB 真正的执行更新记录的操作；
- 开启事务， InnoDB 层更新记录前，首先要记录相应的 undo log，因为这是更新操作，需要把被更新的列的旧值记下来，也就是要生成一条 undo log，undo log 会写入 Buffer Pool 中的 Undo 页面，不过在内存修改该 Undo 页面后，需要记录对应的 redo log。
- InnoDB 层开始更新记录，会先更新内存（同时标记为脏页），然后将记录写到 redo log 里面，这个时候更新就算完成了。为了减少磁盘I/O，不会立即将脏页写入磁盘，后续由后台线程选择一个合适的时机将脏页写入到磁盘。这就是 WAL 技术，MySQL 的写操作并不是立刻写到磁盘上，而是先写 redo 日志，然后在合适的时间再将修改的行数据写到磁盘上。
- 至此，一条记录更新完了。
- 在一条更新语句执行完成后，然后开始记录该语句对应的 binlog，此时记录的 binlog 会被保存到 binlog cache，并没有刷新到硬盘上的 binlog 文件，在事务提交时才会统一将该事务运行过程中的所有 binlog 刷新到硬盘。
- 事务提交（为了方便说明，这里不说组提交的过程，只说两阶段提交）：
- ​	prepare 阶段：将 redo log 对应的事务状态设置为 prepare，然后将 redo log 刷新到硬盘；
- ​	commit 阶段：将 binlog 刷新到磁盘，接着调用引擎的提交事务接口，将 redo log 状态设置为 commit（将事务设置为 commit 状态后，刷入到磁盘 redo log 文件）；
- 至此，一条更新语句执行完成。

---

#### 性能调优

#### mysql的explain有什么作用？

explain是查看sql的执行计划，主要用于分析sql语句的执行过程，比如有没有走索引，有没有外部排序，有没有索引覆盖等等。

如下图，就是一个没有使用索引，并且是一个全表扫描的查询语句。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ad705ff8b7c04ecabc33fcb211f7d619.png)

对于执行计划，参数有：

- possible_keys 字段表示可能用到的索引；

- key 字段表示实际用的索引，如果这一项为 NULL，说明没有使用索引；
- key_len 表示索引的长度；
- rows 表示扫描的数据行数。
- type 表示数据扫描类型，我们需要重点看这个。

type字段就是描述了找到所需数据时使用的扫描方式是什么，常见扫描类型的执行效率从低到高的顺序为：

- All（全表扫描）all是最坏的情况。因为是全表扫描

- index（全索引扫描）：index 和 all 差不多，只不过 index 对索引表进行全扫描，这样做的好处是不再需要对数据进行排序，但是开销依然很大。所以，要尽量避免全表扫描和全索引扫描。
- range（索引范围扫描）：range 表示采用了索引范围扫描，一般在 where 子句中使用 < 、>、in、between 等关键词，只检索给定范围的行，属于范围查找。从这一级别开始，索引的作用会越来越明显，因此我们需要尽量让 SQL 查询可以使用到 range 这一级别及以上的 type 访问方式。
- ref（非唯一索引扫描）：ref 类型表示采用了非唯一索引，或者是唯一索引的非唯一性前缀，返回数据返回可能是多条。因为虽然使用了索引，但该索引列的值并不唯一，有重复。这样即使使用索引快速查找到了第一条数据，仍然不能停止，要进行目标值附近的小范围扫描。但它的好处是它并不需要扫全表，因为索引是有序的，即便有重复值，也是在一个非常小的范围内扫描。（不懂⭐️）
- eq_ref（唯一索引扫描）：eq_ref 类型是使用主键或唯一索引时产生的访问方式，通常使用在多表联查中。比如，对两张表进行联查，关联条件是两张表的 user_id 相等，且 user_id 是唯一索引，那么使用 EXPLAIN 进行执行计划查看的时候，type 就会显示 eq_ref。
- const（结果只有一条的主键或唯一索引扫描）：const 类型表示使用了主键或者唯一索引与常量值进行比较，比如 select name from product where id=1。需要说明的是 const 类型和 eq_ref 都使用了主键或唯一索引，不过这两个类型有所区别，const 是与常量进行比较，查询效率会更快，而 eq_ref 通常用于多表联查中。

------

#### extra 显示的结果，这里说几个重要的参考指标：（不懂⭐️⭐️）

- Using filesort ：当你在查询中使用了 GROUP BY 或 ORDER BY，但数据库无法利用已有的索引来完成排序时，就会采用一种称为“文件排序”的方法。这种方法通常涉及读取数据到临时区域进行排序，效率较低。

- Using temporary：使了用临时表保存中间结果，MySQL 在对查询结果排序时使用临时表，常见于排序 order by 和分组查询 group by。效率低，要避免这种问题的出现。
- Using index：所需数据只需在索引即可全部获得，不须要再到表中取数据，也就是使用了覆盖索引，避免了回表操作，效率不错。

-------

#### 给你张表，发现查询速度很慢，你有那些解决方案

- 分析查询语句： 使用EXPLAIN命令分析SQL执行计划，找出慢查询的原因，比如是否使用了全表扫描，是否存在索引未被利用的情况等，并根据相应情况对索引进行适当修改。

- 创建或优化索引： 根据查询条件创建合适的索引，特别是经常用于WHERE子句的字段、Orderby 排序的字段、Join 连表查询的字典、 group by的字段，并且如果查询中经常涉及多个字段，考虑创建联合索引，使用联合索引要符合最左匹配原则，不然会索引失效
- 避免索引失效： 比如不要用左模糊匹配、函数计算、表达式计算等等。
- 查询优化：避免使用SELECT *，只查询真正需要的列；使用覆盖索引，即索引包含所有查询的字段；联表查询最好要以小表驱动大表，并且被驱动表的字段要有索引，当然最好通过冗余字段的设计，避免联表查询。
- 分页优化： 针对 limit n,y 深分页的查询优化，可以把Limit查询转换成某个位置的查询：select * from tb_sku where id>20000 limit 10，该方案适用于主键自增的表，
- 优化数据库表： 如果单表的数据超过了千万级别，考虑是否需要将大表拆分为小表，减轻单个表的查询压力。也可以将字段多的表分解成多个表，有些字段使用频率高，有些低，数据量大时，会由于使用频率低的存在而变慢，可以考虑分开。
- **使用缓存技术：**引入缓存层，如Redis，存储热点数据和频繁查询的结果，但是要考虑缓存一致性的问题，对于读请求会选择旁路缓存策略，对于写请求会选择先更新 db，再删除缓存的策略。

-----

#### 架构

#### MySQL主从复制了解吗

MySQL的主从复制依赖于bin log，也就是记录MySQL上的所有变化并以二进制形式保存在磁盘上。复制过程就是将bin log中的数据从主库传输到从库上。

这个过程一般是异步 的，也就是主库上执行事务操作的线程不会等待复制bin log的线程同步完成。

MySQL 集群的主从复制过程梳理成 3 个阶段：

- 写入 Binlog： 主库写 binlog 日志，提交事务，并更新本地存储数据。

- 同步 Binlog： 把 binlog 复制到所有从库上，每个从库把 binlog 写到暂存日志中。
- 回放 Binlog： 回放 binlog，并更新存储引擎中的数据。

具体详细过程如下：

- MySQL 主库在收到客户端提交事务的请求之后，会先写入 binlog，再提交事务，更新存储引擎中的数据，事务提交完成后，返回给客户端“操作成功”的响应。

- 从库会创建一个专门的 I/O 线程，连接主库的 log dump 线程，来接收主库的 binlog 日志，再把 binlog 信息写入 relay log 的中继日志里，再返回给主库“复制成功”的响应。
- 从库会创建一个用于回放 binlog 的线程，去读 relay log 中继日志，然后回放 binlog 更新存储引擎中的数据，最终实现主从的数据一致性。

在完成主从复制之后，你就可以在写数据时只写主库，在读数据时只读从库，这样即使写请求会锁表或者锁记录，也不会影响读请求的执行。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4297e175075c4153953f370699a77292.png)

------

#### 主从延迟都有什么处理方法？

强制走主库方案： 对于大事务或资源密集型操作，直接在主库上执行，避免从库的额外延迟。

#### 分表和分库是什么？有什么区别？

分库是一种水平扩展数据库的技术，将数据根据一定规则划分到多个独立的数据库中。每个数据库只负责存储部分数据，实现了数据的拆分和分布式存储。分库主要是为了解决并发连接过多，单机 mysql扛不住的问题。
分表指的是将单个数据库中的表拆分成多个表，每个表只负责存储一部分数据。这种数据的垂直划分能够提高查询效率，减轻单个表的压力。分表主要是为了解决单表数据量太大，导致查询性能下降的问题。






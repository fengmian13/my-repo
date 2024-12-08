# Object



### Object 类的常见方法有哪些?

Object 类是一个特殊的类，是所有类的父类，主要提供了以下 11 个方法：

```
/**
 * native 方法，用于返回当前运行时对象的 Class 对象，使用了 final 关键字修饰，故不允许子类重写。
 */
public final native Class<?> getClass()
/**
 * native 方法，用于返回对象的哈希码，主要使用在哈希表中，比如 JDK 中的HashMap。
 */
public native int hashCode()
/**
 * 用于比较 2 个对象的内存地址是否相等，String 类对该方法进行了重写以用于比较字符串的值是否相等。
 */
public boolean equals(Object obj)
/**
 * native 方法，用于创建并返回当前对象的一份拷贝。
 */
protected native Object clone() throws CloneNotSupportedException
/**
 * 返回类的名字实例的哈希码的 16 进制的字符串。建议 Object 所有的子类都重写这个方法。
 */
public String toString()
/**
 * native 方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
 */
public final native void notify()
/**
 * native 方法，并且不能重写。跟 notify 一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。
 */
public final native void notifyAll()
/**
 * native方法，并且不能重写。暂停线程的执行。注意：sleep 方法没有释放锁，而 wait 方法释放了锁 ，timeout 是等待时间。
 */
public final native void wait(long timeout) throws InterruptedException
/**
 * 多了 nanos 参数，这个参数表示额外时间（以纳秒为单位，范围是 0-999999）。 所以超时的时间还需要加上 nanos 纳秒。。
 */
public final void wait(long timeout, int nanos) throws InterruptedException
/**
 * 跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
 */
public final void wait() throws InterruptedException
/**
 * 实例被垃圾回收器回收的时候触发的操作
 */
protected void finalize() throws Throwable { }

```

### [hashCode() 有什么用？](https://javaguide.cn/java/basis/java-basic-questions-02.html#hashcode-有什么用)

`hashCode()` 的作用是获取哈希码（`int` 整数），也称为散列码。这个哈希码的作用是确定该对象在哈希表中的索引位置。

### [为什么重写 equals() 时必须重写 hashCode() 方法？](#为什么重写-equals-时必须重写-hashcode-方法)

因为两个相等的对象的 `hashCode` 值必须是相等。也就是说如果 `equals` 方法判断两个对象是相等的，那这两个对象的 `hashCode` 值也要相等。

如果重写 `equals()` 时没有重写 `hashCode()` 方法的话就可能会导致 `equals` 方法判断是相等的两个对象，`hashCode` 值却不相等。

**思考**：重写 `equals()` 时没有重写 `hashCode()` 方法的话，使用 `HashMap` 可能会出现什么问题。

**总结**：

- `equals` 方法判断两个对象是相等的，那这两个对象的 `hashCode` 值也要相等。

- 两个对象有相同的 `hashCode` 值，他们也不一定是相等的（哈希碰撞）。

  

  ### String、StringBuffer、StringBuilder 的区别?

1. **不可变性**：   - `String` 类型是不可变的，这意味着一旦创建了 `String` 对象，就不能更改它的内容。对 `String` 的任何修改都会生成一个新的 `String` 对象。

2. **线程安全**：   - `StringBuffer` 是线程安全的，这意味着它的方法是同步的，可以在多线程环境中使用，而不会出现数据不一致的问题。   - `StringBuilder` 不是线程安全的，它的性能通常比 `StringBuffer` 好，因为它不需要进行线程同步。 

3. **性能**：   - 由于 `StringBuffer` 是线程安全的，所以在执行字符串操作时会有额外的同步开销，这可能会影响性能。   - `StringBuilder` 由于不是线程安全的，所以在单线程环境下性能更好。 

   **使用场景**：   - 当你需要在多线程环境中操作字符串时，应该使用 `StringBuffer`。   - 当你在单线程环境中构建或修改字符串时，应该使用 `StringBuilder`。   - 当你需要一个不可变的字符串时，应该使用 `String`。 

   **方法**：   - `String` 提供了丰富的方法来操作字符串，

   例如 `substring()`、`indexOf()`、`replace()` 等，这些方法通常返回一个新的 `String` 对象。   - `StringBuffer` 和 `StringBuilder` 也提供了类似的字符串操作方法，例如 `append()`、`insert()`、`delete()` 等，但这些方法会直接修改当前对象，而不是返回一个新的对象。

   **示例**： 

   ```
   String str = "Hello";
   str = str + " World"; // 创建了一个新的 String 对象
   
   StringBuffer sb = new StringBuffer("Hello");
   sb.append(" World"); // 直接修改了 StringBuffer 对象
   
   StringBuilder sbd = new StringBuilder("Hello");
   sbd.append(" World"); // 直接修改了 StringBuilder 对象
   ```

   

    在Java 9及以后的版本中，`StringBuffer` 和 `StringBuilder` 已经被更新为不可变的，以提高安全性和性能。这意味着它们的实例在创建后不能被修改，这改变了它们的行为，使得它们更像 `String` 类型。但是，由于向后兼容性的原因，`StringBuffer` 和 `StringBuilder` 的现有实现仍然保持可变。

   ### [String 为什么是不可变的?](https://javaguide.cn/java/basis/java-basic-questions-02.html#string-为什么是不可变的)

   `String` 类中使用 `final` 关键字修饰字符数组来保存字符串，所以`String` 对象是不可变的。

   ```
   public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
       private final char value[];
     //...
   }
   
   ```

   

​	`String` 真正不可变有下面几点原因：

1. 保存字符串的数组被 `final` 修饰且为私有的，并且`String` 类没有提供/暴露修改这个字符串的方法。
2. `String` 类被 `final` 修饰导致其不能被继承，进而避免了子类破坏 `String` 不可变。

### [字符串常量池的作用了解吗？](https://javaguide.cn/java/basis/java-basic-questions-02.html#字符串常量池的作用了解吗)

**字符串常量池** 是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。

```
// 在堆中创建字符串对象”ab“
// 将字符串对象”ab“的引用保存在字符串常量池中
String aa = "ab";
// 直接返回字符串常量池中字符串对象”ab“的引用
String bb = "ab";
System.out.println(aa==bb);// true

```

### String s1 = new String("abc");这句话创建了几个字符串对象？

会创建 1 或 2 个字符串对象。

1、如果字符串常量池中不存在字符串对象“abc”的引用，那么它会在堆上创建两个字符串对象，其中一个字符串对象的引用会被保存在字符串常量池中。

2、如果字符串常量池中已存在字符串对象“abc”的引用，则只会在堆中创建 1 个字符串对象“abc”。

### [String#intern 方法有什么作用?](#string-intern-方法有什么作用)

`String.intern()` 是一个 native（本地）方法，其作用是将指定的字符串对象的引用保存在字符串常量池中，可以简单分为两种情况：

- 如果字符串常量池中保存了对应的字符串对象的引用，就直接返回该引用。
- 如果字符串常量池中没有保存了对应的字符串对象的引用，那就在常量池中创建一个指向该字符串对象的引用并返回。

### String 类型的变量和常量做“+”运算时发生了什么?

在Java中，当使用 `+` 运算符对 `String` 类型的变量或常量进行连接操作时，实际上发生了以下几个步骤：

1. **创建新的 `String` 对象**：由于 `String` 类型是不可变的，每次使用 `+` 运算符连接字符串时，都会创建一个新的 `String` 对象。这意味着原始的 `String` 对象不会被修改。
2. **字符串拼接**：`+` 运算符将两个或多个 `String` 对象的内容拼接在一起，生成一个新的 `String` 对象。
3. **临时变量**：在拼接过程中，可能会使用临时变量来存储中间结果，特别是当涉及到多个字符串连接时。
4. **自动类型转换**：如果 `+` 运算符的一边是 `String` 类型，另一边是其他数据类型（如 `int`、`double` 等），Java 会自动将非 `String` 类型的变量转换为 `String` 类型，然后再进行连接。
5. **表达式求值**：`+` 运算符在连接字符串时，会从左到右依次求值。这意味着在连接操作中的变量或常量会按照它们在表达式中出现的顺序被连接。
6. **结果赋值**：如果 `+` 运算符的结果被赋值给一个 `String` 类型的变量，那么新创建的 `String` 对象的引用将被赋给该变量。

例如，考虑以下代码：

```java
String a = "Hello";
String b = "World";
String result = a + " " + b; // "Hello World"
```

在这个例子中，首先将 `a` 的内容与一个空格连接，然后这个结果再与 `b` 的内容连接，最终创建了一个新的 `String` 对象 `"Hello World"`，并将这个新对象的引用赋给 `result` 变量。

由于字符串连接操作可能会涉及到多个中间 `String` 对象的创建，因此在循环或频繁执行字符串连接的场合，使用 `StringBuilder` 或 `StringBuffer` 可以提高性能，因为它们可以在内部数组上进行修改，而不需要每次操作都创建新的 `String` 对象。
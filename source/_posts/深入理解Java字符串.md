---
title: 深入理解Java字符串
date: 2021-05-07 08:21:36
tags: Java虚拟机
categories: Java
---
## 深入理解字符串常量池🥇

### Java内存区域🏆

String有两种赋值方式，第一种是通过“**字面量**”赋值。

```java
String str = "Hello";
```

 第二种是通过**new**关键字创建新对象。

```java
String str = new String("Hello");
```

 要弄清楚这两种方式的区别，首先要知道他们在内存中的存储位置。

[![g3zlRK.png](https://z3.ax1x.com/2021/05/07/g3zlRK.png)](https://imgtu.com/i/g3zlRK)

我们平时所说的内存就是图中的**运行时数据区（Runtime Data Area）**，其中与字符串的创建有关的是**方法区（Method Area）**、**堆区（Heap Area）**和**栈区（Stack Area）**。

1. **方法区**：存储类信息、常量、静态变量。全局共享。
2. **堆区**：存放对象和数组。全局共享。
3. **栈区**：基本数据类型、对象的引用都存放在这。线程私有。

[![g3ztZd.png](https://z3.ax1x.com/2021/05/07/g3ztZd.png)](https://imgtu.com/i/g3ztZd)

每当一个方法被执行时就会在栈区中创建一个**栈帧（Stack Frame）**，基本数据类型和对象引用就存在栈帧中**局部变量表（Local Variables）**。

当一个类被加载之后，类信息就存储在非堆的方法区中。在方法区中，有一块叫做**运行时常量池（Runtime Constant Pool）**,它是每个类私有的，每个class文件中的“常量池”被加载器加载之后就映射存放在这，后面会说到这一点。

和String最相关的是**字符串池（String Pool）**，其位置在方法区上面的**驻留字符串（Interned Strings）的位置**，之前一直把它和运行时常量池搞混，其实是两个完全不同的存储区域，字符串常量池是全局共享的。字符串调用String.intern()方法后，其引用就存放在String Pool中。

### 两种创建方式在内存中的区别🏆

了解了这些概念，下面来说说究竟两种字符串创建方式有何区别。

下面的Test类，在main方法里以“字面量”赋值的方式给字符串str赋值为“Hello”。

```java
public class Test {
    public static void main(String[] args) {
        
        String str = "Hello";
        
    } 
}
```

Test.java文件编译后得到.class文件，里面包含了类的信息，其中有一块叫做**常量池（Constant Pool）**的区域，.class常量池和内存中的常量池并不是一个东西。

.class文件常量池主要存储的就包括**字面量**，字面量包括类中定义的常量，由于String是不可变的，所以字符串“Hello”就存放在这。

[![g3z0Rf.png](https://z3.ax1x.com/2021/05/07/g3z0Rf.png)](https://imgtu.com/i/g3z0Rf)

当程序用到Test类时，Test.class被解析到内存中的方法区。.class文件中的常量池信息会被加载到**运行时常量池**，但String不是。

例子中“Hello”会在堆区中创建一个对象，同时会在**字符串池（String Pool）**存放一个**它的引用**，如下图所示。

[![g3zrQS.png](https://z3.ax1x.com/2021/05/07/g3zrQS.png)](https://imgtu.com/i/g3zrQS)

**此时只是Test类刚刚被加载，主函数中的str并没有被创建，而“Hello”对象已经创建在于堆中。**

当主线程开始创建str变量时，虚拟机会去字符串池中找是否有equals(“Hello”)的String，如果相等就把在字符串池中“Hello”的**引用复制给str**。如果找不到相等的字符串，就会在堆中新建一个对象，同时把引用驻留在字符串池，再把引用赋给str。

[![g3zyLQ.png](https://z3.ax1x.com/2021/05/07/g3zyLQ.png)](https://imgtu.com/i/g3zyLQ)

当用字面量赋值的方法创建字符串时，无论创建多少次，只要字符串的值相同，它们所指向的都是堆中的同一个对象。

```java
public class Test {
    public static void main(String[] args) {
        
        String str1 = "Hello";
        String str2 = “Hello”;
        String str3 = “Hello”;
        
    } 
}
```

[![g3zcZj.png](https://z3.ax1x.com/2021/05/07/g3zcZj.png)](https://imgtu.com/i/g3zcZj)

当利用**new**关键字去创建字符串时，前面加载的过程是一样的，只是在运行时无论字符串池中有没有与当前值相等的对象引用，都会在堆中新开辟一块内存，创建一个对象。

```java
public class Test {
    public static void main(String[] args) {
        
        String str1 = "Hello";
        String str2 = “Hello”;
        String str3 = new String("Hello");
        
    } 
}
```

[![g3z2on.png](https://z3.ax1x.com/2021/05/07/g3z2on.png)](https://imgtu.com/i/g3z2on)

### 解释不同的例子🏆

```java
String s1 = "Hello";
String s2 = "Hello";
String s3 = "Hel" + "lo";
String s4 = "Hel" + new String("lo");
String s5 = new String("Hello");
String s6 = s5.intern();
String s7 = "H";
String s8 = "ello";
String s9 = s7 + s8;
          
System.out.println(s1 == s2);  // true
System.out.println(s1 == s3);  // true
System.out.println(s1 == s4);  // false
System.out.println(s1 == s9);  // false
System.out.println(s4 == s5);  // false
System.out.println(s1 == s6);  // true
```

**s1**在创建对象的同时，在字符串池中也创建了其对象的引用。

由于**s2**也是利用字面量创建，所以会先去字符串池中寻找是否有相等的字符串，显然**s1**已经帮他创建好了，它可以直接使用其引用。那么s1和s2所指向的都是同一个地址，所以**s1==s2**。

**s3**是一个字符串拼接操作，参与拼接的部分都是字面量，编译器会进行优化，在编译时s3就变成“Hello”了，所以**s1==s3**。

**s4**虽然也是拼接，但“lo”是通过**new**关键字创建的，在编译期无法知道它的地址，所以不能像s3一样优化。所以必须要等到运行时才能确定，必然新对象的地址和前面的不同。

同理，**s9**由**两个变量拼接**，**编译期也不知道他们的具体位置**，不会做出优化。

**s5**是new出来的，在堆中的地址肯定和s4不同。

**s6**利用intern()方法得到了s5在**字符串池的引用**，并不是s5本身的地址。由于它们在字符串池的引用都指向同一个“Hello”对象，自然**s1==s6**。

 总结一下：

- **字面量创建字符串会先在字符串池中找，看是否有相等的对象，没有的话就在堆中创建，把地址驻留在字符串池；有的话则直接用池中的引用，避免重复创建对象。**
- **new关键字创建时，前面的操作和字面量创建一样，只不过最后在运行时会创建一个新对象，变量所引用的都是这个新对象的地址。**

 由于不同版本的JDK内存会有些变化，JDK1.6字符串常量池在永久代，1.7移到了堆中，1.8用元空间代替了永久代。但是基本对上面的结论没有影响，思想是一样的。

### intern()方法🏆

```java
/**
     * Returns a canonical representation for the string object.
     * <p>
     * A pool of strings, initially empty, is maintained privately by the
     * class {@code String}.
     * <p>
     * When the intern method is invoked, if the pool already contains a
     * string equal to this {@code String} object as determined by
     * the {@link #equals(Object)} method, then the string from the pool is
     * returned. Otherwise, this {@code String} object is added to the
     * pool and a reference to this {@code String} object is returned.
     * <p>
     * It follows that for any two strings {@code s} and {@code t},
     * {@code s.intern() == t.intern()} is {@code true}
     * if and only if {@code s.equals(t)} is {@code true}.
     * <p>
     * All literal strings and string-valued constant expressions are
     * interned. String literals are defined in section 3.10.5 of the
     * <cite>The Java™ Language Specification</cite>.
     *
     * @return  a string that has the same contents as this string, but is
     *          guaranteed to be from a pool of unique strings.
     */
    public native String intern();
```

这个方法是一个本地方法，注释中描述得很清楚：“如果常量池中存在当前字符串，就会直接返回当前字符串；如果常量池中没有此字符串，会将此字符串放入常量池中后，再返回”。

由于上面提到JDK1.6之后，字符串常量池在内存中的位置发生了变化，所以intern()方法在不同版本的JDK中也有所差别。

来看下面的代码

```java
public static void main(String[] args) {
    String s = new String("1");
    s.intern();
    String s2 = "1";
    System.out.println(s == s2);

    String s3 = new String("1") + new String("1");
    s3.intern();
    String s4 = "11";
    System.out.println(s3 == s4);
}
```

在JDK1.6中的运行结果是**false false**，而在1.7中结果是**false true**。

将intern()语句下移一行。

```java
public static void main(String[] args) {
    String s = new String("1");
    String s2 = "1";
    s.intern();
    System.out.println(s == s2);

    String s3 = new String("1") + new String("1");
    String s4 = "11";
    s3.intern();
    System.out.println(s3 == s4);
}
```

在JDK1.6中的运行结果是**false false**，在1.7中结果也是**false false**。

[![g3zhWV.jpg](https://z3.ax1x.com/2021/05/07/g3zhWV.jpg)](https://imgtu.com/i/g3zhWV)

图中绿色线条代表string对象的内容指向，黑色线条代表地址指向。

JDK1.6中的intern()方法只是返回常量池中的引用，上面说过，常量池中的引用所指向的对象和new出来的对象并不是一个，所以他们的地址自然不相同。

接着来说一下**JDK1.7**：

[![g3z4zT.jpg](https://z3.ax1x.com/2021/05/07/g3z4zT.jpg)](https://imgtu.com/i/g3z4zT)

再贴一下代码

```java
public static void main(String[] args) {
    String s = new String("1");
    s.intern();
    String s2 = "1";
    System.out.println(s == s2);

    String s3 = new String("1") + new String("1");
    s3.intern();
    String s4 = "11";
    System.out.println(s3 == s4);
}
```

创建s3生成了两个最终对象（**不考虑两个new String("1")，常量池中也没有“11”**），一个是**s3**，另一个是池中的**“1”**。如果在1.6中，s3调用intern()方法，则先在常量池中寻找是否有等于“11”的对象，本例中自然是没有，然后会在堆中创建一个“11”的对象，并在常量池中存储它的引用并返回。然而在1.7中调用intern()，如果这个字符串在常量池中是**第一次出现**，则不会重新创建对象，直接返回它在堆中的引用。在本例中，s4和s3指向的都是在堆中的那个对象，所以s3和s4的地址相等。

由于**s**是new出来的，所以会在常量池和堆中创建两个不同的对象，s.intern()后，发现“1”并不是第一次出现在常量池了，所以接下来就和之前没有区别了。

[![g3zoyF.jpg](https://z3.ax1x.com/2021/05/07/g3zoyF.jpg)](https://imgtu.com/i/g3zoyF)

```java
public static void main(String[] args) {
    String s = new String("1");
    String s2 = "1";
    s.intern();
    System.out.println(s == s2);

    String s3 = new String("1") + new String("1");
    String s4 = "11";
    s3.intern();
    System.out.println(s3 == s4);
}
```

将intern()语句下移一行后，执行顺序发生改变，执行到intern()时，字符串常量池已经存在“1”和“11”了，并不是第一次出现，字面量对象依然指向的是常量池，所以字面量创建的对象和new的对象地址一定是不同的。

### Reference🏆

[java用这样的方式生成字符串：String str = "Hello"，到底有没有在堆中创建对象？](https://www.zhihu.com/question/29884421/answer/113785601)

[触摸java常量池](https://www.cnblogs.com/iyangyuan/p/4631696.html)

[深入解析String#intern](https://tech.meituan.com/2014/03/06/in-depth-understanding-string-intern.html)

[Strings, Literally](https://javaranch.com/journal/200409/ScjpTipLine-StringsLiterally.html)

[理解Java字符串常量池与intern()方法](https://www.cnblogs.com/justcooooode/p/7603381.html)
---
title: Java字符串总结
date: 2021-05-03 21:19:19
tags: Java
categories: Java
---
## Java字符串总结**🦠**

### 1.字符串的用法总览（常用API）**🌴**

String类在java.lang包中，java使用String类创建一个字符串变量，字符串变量属于对象。java把String类声明的final类，不能有子类。String类对象创建后不能修改，由0或多个字符组成，包含在一对双引号之间。

```java
 java.lang.String  
  
 char charAt (int index)     返回index所指定的字符  
 String concat(String str)   将两字符串连接  
 boolean endsWith(String str)    测试字符串是否以str结尾  
 boolean equals(Object obj)  比较两对象  
 char[] getBytes     将字符串转换成字符数组返回  
 char[] getBytes(String str)     将指定的字符串转成制服数组返回  
 boolean startsWith(String str)  测试字符串是否以str开始  
 int length()    返回字符串的长度  
 String replace(char old ,char new)  将old用new替代  
 char[] toCharArray  将字符串转换成字符数组  
 String toLowerCase()    将字符串内的字符改写成小写  
 String toUpperCase()    将字符串内的字符改写成大写  
 String valueOf(Boolean b)   将布尔方法b的内容用字符串表示  
 String valueOf(char ch)     将字符ch的内容用字符串表示  
 String valueOf(int index)   将数字index的内容用字符串表示  
 String valueOf(long l)  将长整数字l的内容用字符串表示  
 String substring(int1,int2)     取出字符串内第int1位置到int2的字符串  
```

### 2.构造方法**🌴**

```java
 //直接初始化
String str = "abc";
//使用带参构造方法初始化
char data[] = {'a', 'b', 'c'};
String str1 = new String("abc");
String str2 = new String(str);
String str3 = new String(data);
System.out.println(str);
System.out.println(str1);
System.out.println(str2);
System.out.println(str3);
```

### 3.求字符串长度和某一位置字符**🌴**

```java
String str = new String("abcdef");
int length = str.length();
char ch = str.charAt(4);//index 从0 开始，取第五个字符
System.out.println(length);
System.out.println(ch);
```

### 4.提取子串**🌴**

用String的substring方法可以提取子串，该方法有两个参数形式：

`public String substring(int beginIndex)`

该方法从beginIndex位置起，从当前字符串中取出剩余的字符作为一个新的字符串返回。

`public String substring(int beginIndex, int endIndex)` 

该方法从beginIndex位置起，从当前字符串中取出到endIndex-1位置的字符作为一个新的字符串返回。

```java
 String str1 = new String("abcdef");
 String str2 = str1.substring(2);//index 从0 开始
 String str3 = str1.substring(2,5);
 System.out.println(str2);//str2 = "cdef"
 System.out.println(str3);//str3 = "cde"
```

### 5.字符串比较**🌴**

`public int compareTo(String anotherString)`

该方法是对字符串内容按字典顺序进行大小比较，通过返回的整数值指明当前字符串与参数字符串的大小关系。若当前对象比参数大则返回正整数，反之返回负整数，相等返回0

`public int compareToIgnoreCase(String anotherString)`

与compareTo方法相似，但忽略大小写

`public boolean equals(Object anotherObject)`

比较当前字符串和参数字符串，在两个字符串相等的时候返回true，否则返回false

`public boolean equalsIgnoreCase`(String anotherString)

与equals方法相似，但忽略大小写

```java
String str1 = new String("abc");
String str2 = new String("ABC");
int a = str1.compareTo(str2);//a>0
int b = str1.compareToIgnoreCase(str2);//b=0
boolean c = str1.equals(str2);//c=false
boolean d = str1.equalsIgnoreCase(str2);//d=true
System.out.println(a);
System.out.println(b);
System.out.println(c);
System.out.println(d);
```

### 6.字符串拼接**🌴**

`public String concat(String str)`

将参数中的字符串str连接到当前字符串的后面，效果等价于"+"

```java
String str = "aa".concat("bb").concat("cc");
//相当于String str = "aa"+"bb"+"cc";
System.out.println(str);
```

### 7.字符串查找**🌴**

`public int indexOf(int ch/String str)`

用于查找当前字符串中字符或子串，返回字符或子串在当前字符串中从左边起首次出现的位置，若没有出现则返回-1。

`public int indexOf(int ch/String str, int fromIndex)`

该方法与第一种类似，区别在于该方法从fromIndex位置向后查找。

`public int lastIndexOf(int ch/String str)`

该方法与第一种类似，区别在于该方法从字符串的末尾位置向前查找。

 `public int lastIndexOf(int ch/String str, int fromIndex)`

该方法与第二种方法类似，区别于该方法从fromIndex位置向前查找

```java
String str = "I really miss you !";
int a = str.indexOf('a');//a = 4
int b = str.indexOf("really");//b = 2
int c = str.indexOf("gg",2);//c = -1
int d = str.lastIndexOf('s');//d = 12
int e = str.lastIndexOf('s',11);//e = 11
//注意所有索引位置都是从前向后计算的，lastIndexOf只是方向是从后向前
System.out.println(a);
System.out.println(b);
System.out.println(c);
System.out.println(d);
System.out.println(e);
```

### 8.字符串替换**🌴**

`public String replace(char oldChar, char newChar)`

用字符newChar替换当前字符串中所有的oldChar字符，并返回一个新的字符串。

`public String replaceFirst(String regex, String replacement)`

该方法用字符replacement的内容替换当前字符串中遇到的第一个和字符串regex相匹配的子串，应将新的字符串返回。

`public String replaceAll(String regex, String replacement)`

该方法用字符replacement的内容替换当前字符串中遇到的所有和字符串regex相匹配的子串，应将新的字符串返回。

```java
String str = "asdzxcasd";
String str1 = str.replace('a','g');//str1 = "gsdzxcgsd"
String str2 = str.replace("asd","fgh");//str2 = "fghzxcfgh"
String str3 = str.replaceFirst("asd","fgh");//str3 = "fghzxcasd"
String str4 = str.replaceAll("asd","fgh");//str4 = "fghzxcfgh"
System.out.println(str1);
System.out.println(str2);
System.out.println(str3);
System.out.println(str4);
```

### 9.大小写转换**🌴**

`public String toLowerCase()`

返回将当前字符串中所有字符转换成小写后的新串

 `public String toUpperCase()`

返回将当前字符串中所有字符转换成大写后的新串

```java
String str = new String("abCD");
String str1 = str.toLowerCase();//str1 = "abcd"
String str2 = str.toUpperCase();//str2 = "ABCD"
```

### 10.其他方法**🌴**

`String trim()`

截去字符串两端的空格，但对于中间的空格不处理

```java
String str = " a bc ";
String str1 = str.trim();
int a = str.length();//a = 6
int b = str1.length();//b = 4
```

`boolean statWith(String prefix)`或`boolean endWith(String suffix)`

用来比较当前字符串的起始字符或子字符串prefix和终止字符或子字符串suffix是否和当前字符串相同，重载方法中同时还可以指定比较的开始位置offset

```java
String str = "abcdef";
boolean a = str.statWith("ab");//a = true
boolean b = str.endWith("ef");//b = true
```

`contains(String str)`

判断参数s是否被包含在字符串中，并返回一个布尔类型的值

```java
String str = "abcdef";
str.contains("ab");//true
str.contains("gh");//false
```

`String[] split(String str)`

将str作为分隔符进行字符串分解，分解后的字字符串在字符串数组中返回

```java
String str = "abc def ghi";
String[] str1 = str.split(" ");//str1[0] = "abc";str1[1] = "def";str1[2] = "ghi";
```

### 11.类型转换**🌴**

- 字符串转基本类型

  `public static byte parseByte(String s)`
  `public static short parseShort(String s)`
  `public static short parseInt(String s)`
  `public static long parseLong(String s)`
  `public static float parseFloat(String s)`
  `public static double parseDouble(String s)`

- 基本类型转字符串

  `public static String valueOf(char data[])`
  `public static String valueOf(char data[], int offset, int count)`
  `public static String valueOf(boolean b)`
  `public static String valueOf(char c)`
  `public static String valueOf(int i)`
  `public static String valueOf(long l)`
  `public static String valueOf(float f)`
  `public static String valueOf(double d)`

### 12.String类源码**🌴**

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];

    /** Cache the hash code for the string */
    private int hash; // Default to 0

    /** use serialVersionUID from JDK 1.0.2 for interoperability */
    private static final long serialVersionUID = -6849794470754667710L;

    /**
     * Class String is special cased within the Serialization Stream Protocol.
     *
     * A String instance is written into an ObjectOutputStream according to
     * <a href="{@docRoot}/../platform/serialization/spec/output.html">
     * Object Serialization Specification, Section 6.2, "Stream Elements"</a>
     */
    private static final ObjectStreamField[] serialPersistentFields =
        new ObjectStreamField[0];

    /**
     * Initializes a newly created {@code String} object so that it represents
     * an empty character sequence.  Note that use of this constructor is
     * unnecessary since Strings are immutable.
     */
    public String() {
        this.value = "".value;
    }

    /**
     * Initializes a newly created {@code String} object so that it represents
     * the same sequence of characters as the argument; in other words, the
     * newly created string is a copy of the argument string. Unless an
     * explicit copy of {@code original} is needed, use of this constructor is
     * unnecessary since Strings are immutable.
     *
     * @param  original
     *         A {@code String}
     */
    public String(String original) {
        this.value = original.value;
        this.hash = original.hash;
    }
    
```

1. String类是final类，也即意味着String类不能被继承，并且它的成员方法都默认为final方法。在Java中，被final修饰的类是不允许被继承的，并且该类中的成员方法都默认为final方法。
2. String类其实是通过char数组来保存字符串的。
3. **String对象一旦被创建就是固定不变的了，对String对象的任何改变都不影响到原对象，相关的任何change操作都会生成新的对象**
4. 关于字符串常量池介绍详见：**🌹**[字符串常量池](http://jevinzhao.com/2021/05/07/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3Java%E5%AD%97%E7%AC%A6%E4%B8%B2/)


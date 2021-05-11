---
title: Jevin学Java-基础程序设计结构
date: 2021-04-28 21:44:07
tags: Java
categories: Java
---
## 基础程序设计结构**🧶**

### 数据类型**🪐**

四类八种

#### 整型

表示没有小数部分的数值，可以是负数。一共有4种，具体如下：

[![giWiWT.png](https://z3.ax1x.com/2021/04/28/giWiWT.png)](https://imgtu.com/i/giWiWT)

`int`：最常用

`short /byte`：特定的应用场景，如：底层文件处理

`long`：写法为数值带有**后缀L或者l**（40000000L）

**Notion**：Java中的整型都是**有符号**的

#### 浮点型

表示带有小数部分的数值，有两种，具体如下：

[![giW3lD.png](https://z3.ax1x.com/2021/04/28/giW3lD.png)](https://imgtu.com/i/giW3lD)

`double`：最常用，表示数值精度是float的两倍，因为float的精度很难满足需求、

`float`：应用场景为，需要单精度的库，或者存储大量数据。写法为数值**带一个F或者f**（3.14F），没有后缀F的浮点数默认为double类型。

**三种特殊的浮点数**（常量）

- 正无穷  Double.POSITIVE_INFINITY
- 负无穷  Double.NEGATIVEJNFINITY
- NaN（不是一个数字）Double.NaN

**Notion**：

```Java
if (x = Double.NaN) // is never true
if (Double.isNaN(x)) // check whether x is "not a number"
```

在涉及金融计算中，使用`**BigDecimal**`处理。

#### char型

表示单个字符，强烈建议不要在程序中使用char 类型， 除非确实需要处理UTF-16 代码单元，最好
将字符串作为抽象数据类型处理。

#### boolean型

表示判定逻辑条件，有两个值：false和true

### 变量**🪐**

顾名思义，变动的量。

**变量的声明**：数据类型     变量名          （int    num）

> 必须用赋值语句对变量进行显式初始化

**变量的初始化**：num=12

两者可以同时做： int  num=12

### 常量**🪐**

顾名思义，不变的量，在Java中用关键字`final`修饰,习惯上常量名采用全大写，某个常量可以在一个类中的多个方法中使用，通常将这些常量称为**类常量**，常见写法：

```Java
public static final double CM_PER_INCH = 2.54;
```

### 运算符**🪐**

算术运算符+、-、*、/ 表示加、减、乘、除运算，％ 表示整数的求余操作（有时称为取模)，当参与/ 运算的两个操作数都是整数时， 表示**整数除法**；否则， 表示**浮点除法**。

#### 数值类型的合法转换

[![giWYmd.png](https://z3.ax1x.com/2021/04/28/giWYmd.png)](https://imgtu.com/i/giWYmd)

实线表示无信息丢失的转换（小转大），虚线表示有可能会有精度丢失的转换

#### 强制类型转换

```java
double x = 9.997;
int nx = (int) x;
```

#### 赋值与运算符结合

```Java
x+=4;
//等价于
x=x+4;
```

一般地， 要把运算符放在= 号左边， 如*= 或％=

#### 自增自减运算符

i++：使用变量原来的值进行操作，操作完后，再加1

++i：先加1，再进行操作

i--：使用变量原来的值进行操作，操作完后，再减1

--i：先减1，再进行操作

#### 关系运算符

比如：**==**，**！=**，**<**，**>**，**<=**，**>=**，得到的结果为Boolean类型

三元运算符：`condition ? expression1 : expression2`

逻辑运算符：`expression1 && expression2`,`expression1 ||expression2`

#### 位运算符

`& ("and")`, `| ("or")`,    `^("XOr")` ,    `~ ("not")`
有`>>`和`<<`运算符将位模式左移或右移

#### 运算符的优先级

如果不使用圆括号， 就按照给出的运算符优先级次序进行计算。同一个级别的运算符按照从左到右的次序进行计算（除了表中给出的右结合运算符外），最高优先级的运算符在的表的最上面，最低优先级的在表的底部

[![giWdtP.png](https://z3.ax1x.com/2021/04/28/giWdtP.png)](https://imgtu.com/i/giWdtP)

### 字符串**🪐**

Java 字符串就是Unicode 字符序列，Java 没有内置的字符串类型， 而是在标准Java 类库中提供了
一个预定义类， 很自然地叫做String。

字符串这部分准备深挖一下，详见：**🍂**[Java字符串总结](http://jevinzhao.com/2021/05/03/Java%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%80%BB%E7%BB%93/)

### 输入输出**🪐**

```Java
//输入
 Scanner in = new Scanner(Systera.in);
// get first input
System.out.print("What is your name? ");
String name = in.nextLine() ;
//get second input
System.out.print("How old are you? ")；
int age = in.nextInt();
//display output on console
System.out.println("Hello, " + name + Next year, you'll be " + (age + 1));

```

```java
//格式化输出
System.out.printf("%8.2f",x);
```

每一个以％ 字符开始的格式说明符都用相应的参数替换。格式说明符尾部的转换符将指示被格式化的数值类型：f 表示浮点数，s 表示字符串，d 表示十进制整数。

### 控制流程**🪐**

#### 块作用域

块（即复合语句）是指由一对大括号括起来的若干条简单的Java 语句。块确定了变量的作用域。

```java
public static void main(String口args)
{
	int n;
    {
        int k;
	} // k is only defined up to here
}
//erro
public static void main(String口args)
{
	int n;
	{
		int k;
		int n; // Error can't redefine n in inner block
	}
}
```

#### 条件语句

```Java
if (yourSales >= target)
{
performance = "Satisfactory";
bonus = 100 + 0.01 * (yourSales - target) ;
}
else
{
performance = "Unsatisfactory";
bonus = 0;
}
```

####  循环语句

```Java
while (balance < goal)
{
balance += payment;
double interest = balance * interestRate / 100;
balance += interest;
years++;
}
System.out.println(years + " years.") ;
```

```java
do
{
balance += payment;
double interest = balance * interestRate / 100;
balance += interest;
year++;
// print current balance
// ask if ready to retire and get input
}
while (input .equals("N") )；
```

#### for语句

for 循环语句只不过是while 循环的一种简化形式。

```java
for (i = 1; i <= 10; i ++){

}
```

#### switch语句

在处理多个选项时， 使用if/else 结构显得有些笨拙。Java 有一个与C/C++ 完全一样的switch 语句。

```java
Scanner in = new Scanner(System.in);
System.out.printC'Select an option (1, 2, 3, 4) M);
int choice = in.nextlnt();
switch (choice)
{
case 1:
break;
case 2:
break;
case 3:
break;
case 4:
break;
default:
// bad input
break;
}
```

#### 中断控制流程语句

`break`:break语句有三种用法，第一种是用于终止switch语句中的语句序列，第二种是用于退出循环，然而第三种是用作goto语句的“文明”形式！通过break语句，可以中断一个或多个代码块。而且这些代码块不必是某个循环或switch语句的一部分，他们可以是任何代码块。Java 还提供了一种带标签的break 语句， 用于跳出多重嵌套的循环语句。

```java
label:
{
if (condition) break label ;// exits block
}
…
// jumps here when the break statement executes
```

`continue`:continue语句将控制转移到最内层循环的首部,将continue 语句用于for 循环中， 就可以跳到for 循环的“ 更新” 部分。

```java
Scanner in = new Scanner(System.in) ;
while (sum < goal )
{
System.out .print ("Enter a number: ")；
n = in.nextlntO；
if (n < 0) continue;
sum += n; // not executed if n < 0
}
```

### 数组**🪐**

#### 声明/初始化方式

```java
//声明
int[] a;//使用较多
int a[];
//赋值
a=new int[100]//长度为100的数组
a={1,23,45,}//
```

`NOTE`:一旦创建了数组， 就不能再改变它的大小。

#### 增强for循环

**for (variable : collection) statement**

`collection` 这一集合表达式必须是一个数组或者是一个实现了Iterable 接口的类对象

#### 数组拷贝

在Java 中， 允许将一个数组变量拷贝给另一个数组变量。这时， 两个变量将引用同一个数组。

```java
int[] luckyNumbers = smallPrimes;
luckyNumbers[5] = 12; // now smallPrimes[5] is also 12
```

[![giWD1S.png](https://z3.ax1x.com/2021/04/28/giWD1S.png)](https://imgtu.com/i/giWD1S)

#### 数组排序

要想对数值型数组进行排序， 可以使用Arrays 类中的sort 方法：

```java
int[] a = new int[10000];
Arrays.sort(a)
```

#### 多维数组

Java 实际上没有多维数组， 只有一维数组。多维数组被解释为“ 数组的数组。”

[![giWc0s.png](https://z3.ax1x.com/2021/04/28/giWc0s.png)](https://imgtu.com/i/giWc0s)
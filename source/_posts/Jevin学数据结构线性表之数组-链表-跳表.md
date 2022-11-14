---
title: 'Jevin学数据结构线性表之数组,链表,跳表'
date: 2021-04-15 21:11:43
tags: 数据结构
categories: 数据结构与算法
mathjax: true # 加入这个声明，这篇文章就会开启mathjax渲染
---
## 数组，链表，跳表🌈

## Array🔥

数组（Array）是一种**线性表**数据结构。它用一组**连续的内存空间**，来存储一组具有**相同类型的数据**。

### 常见写法

Java，C++  ：int a[100]

Python          : list=[]

JavaScript    : let   x=[1,2,3]

### 底层实现

在申请创建数组时，计算机在内存中开辟一段连续地址，每一个地址可以通过内存管理器进行访问。如图所示

[![cRnmdK.png](https://z3.ax1x.com/2021/04/15/cRnmdK.png)](https://imgtu.com/i/cRnmdK)

当计算机需要随机访问数组中的某个元素时，它会首先通过下面的寻址公式，计算出该元素存储的内存地址🤣

```java
a[i]_address = base_address + i * data_type_size
```

其中 data_type_size 表示数组中每个元素的大小。我们举的这个例子里，数组中存储的是 int 类型数据，所以 data_type_size 就为 4 个字节。

数组支持随机访问，根据下标随机访问的时间复杂度为 O(1)。

内存管理器访问数组中任意一个地址的时间复杂度都是O(1)。因此，数组查询数据的速度非常快。

### 数组增加元素

如图所示，假设存在一个数组，里面的元素为ABCEFG，现在要插入元素D

[![cRnEs1.png](https://z3.ax1x.com/2021/04/15/cRnEs1.png)](https://imgtu.com/i/cRnEs1)

1. 把EFG往下挪一个位置
2. 插入D

时间复杂度分析：最好的情况插入到最后一个O(1)，最坏的情况插入到第一个，时间复杂度为O(n)，平均下来时间复杂度为O(n)。

如果要将某个数组插入到第 k 个位置，为了避免大规模的数据搬移，我们还有一个简单的办法就是，直接将第 k 位的数据搬移到数组元素的最后，把新的元素直接放入第 k 个位置。

[![4iOjxA.md.jpg](https://z3.ax1x.com/2021/09/13/4iOjxA.md.jpg)](https://imgtu.com/i/4iOjxA)

假设数组 a[10] 中存储了如下 5 个元素：a，b，c，d，e。我们现在需要将元素 x 插入到第 3 个位置。我们只需要将 c 放入到 a[5]，将 a[2] 赋值为 x 即可。最后，数组中的元素如下： a，b，x，d，e，c。利用这种处理技巧，在特定场景下，在第 k 个位置插入一个元素的时间复杂度就会降为 O(1)。

### 数组删除元素

以上过程的逆过程

1. 拿出D
2. 下面的元素向上挪一个位置

在某些特殊场景下，我们并不一定非得追求数组中数据的连续性。如果我们将多次删除操作集中在一起执行，删除的效率会提高很多。

数组 a[10] 中存储了 8 个元素：a，b，c，d，e，f，g，h。现在，我们要依次删除 a，b，c 三个元素。

[![4iX5WQ.md.jpg](https://z3.ax1x.com/2021/09/13/4iX5WQ.md.jpg)](https://imgtu.com/i/4iX5WQ)

为了避免 d，e，f，g，h 这几个数据会被搬移三次，我们可以先记录下已经删除的数据。每次的删除操作并不是真正地搬移数据，只是记录数据已经被删除。当数组没有更多空间存储数据时，我们再触发执行一次真正的删除操作，这样就大大减少了删除操作导致的数据搬移（**JVM 标记清除垃圾回收算法的核心思想**）

### 数组元素访问

根据索引直接访问对应的内存即可，时间复杂度为O(1)

### 数组与容器的取舍

ArrayList 最大的优势就是**可以将很多数组操作的细节封装起来**。比如前面提到的数组插入、删除数据时需要搬移其他数据等。另外，它还有一个优势，就是**支持动态扩容**

1. Java ArrayList 无法存储基本类型，比如 int、long，需要封装为 Integer、Long 类，而 Autoboxing、Unboxing 则有一定的性能消耗，所以如果特别关注性能，或者希望使用基本类型，就可以选用数组。
2. 如果数据大小事先已知，并且对数据的操作非常简单，用不到 ArrayList 提供的大部分方法，也可以直接使用数组。
3. 当要表示多维数组时，用数组往往会更加直观。比如 Object[][] array；而用容器的话则需要这样定义：ArrayList<ArrayList >  array。

对于业务开发，直接使用容器就足够了，省时省力。毕竟损耗一丢丢性能，完全不会影响到系统整体的性能。但如果你是做一些非常底层的开发，比如开发网络框架，性能的优化需要做到极致，这个时候数组就会优于容器，成为首选。

### Java源码引用（JDK8）

添加到数组的最后

```Java
    /**
     * Appends the specified element to the end of this list.
     *
     * @param e element to be appended to this list
     * @return <tt>true</tt> (as specified by {@link Collection#add})
     */
    public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
```

1. 检查数组容量是否够用
2. 插入最后一个元素

添加到数组的指定位置

```Java
    /**
     * Inserts the specified element at the specified position in this
     * list. Shifts the element currently at that position (if any) and
     * any subsequent elements to the right (adds one to their indices).
     *
     * @param index index at which the specified element is to be inserted
     * @param element element to be inserted
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public void add(int index, E element) {
        rangeCheckForAdd(index);

        ensureCapacityInternal(size + 1);  // Increments modCount!!
        System.arraycopy(elementData, index, elementData, index + 1,
                         size - index);
        elementData[index] = element;
        size++;
    }
```

1. 边界检查，判断插入位置是否在数组的索引内

2. 检查数组容量

3. 将插入位置后的元素向后移动一个位置

   ```java
      System.arraycopy(elementData, index, elementData, index + 1,
                            size - index);
       public static native void arraycopy(Object src,  int  srcPos,
                                           Object dest, int destPos,
                                           int length);
   ```

   该方法进行数组元素的复制，该方法在系统包下，涉及到底层的系统调用。在进行元素插入的时候会进行非常多的复制，造成时间复杂度较高，效率较低。

4. 将元素插入到指定位置

## Linked List🔥

数组需要一块**连续的内存空间**来存储，对内存的要求比较高。如果我们申请一个 100MB 大小的数组，当内存中没有连续的、足够大的存储空间时，即便内存的剩余总可用空间大于 100MB，仍然会申请失败。

而链表恰恰相反，它并不需要一块连续的内存空间，它通过“指针”将一组**零散的内存块**串联起来使用，所以如果我们申请的是 100MB 大小的链表，根本不会有问题。

把内存块称为链表的“**结点**”。为了将所有的结点串起来，每个链表的结点除了存储数据之外，还需要记录链上的下一个结点的地址。如图所示，我们把这个记录下个结点地址的指针叫作**后继指针 next**

### 链表结构

每一个元素（实际上是一个class，也叫node）有value和next两个成员变量，next指向下一个元素，串在一起就形成了**单向链表**；

[![cRnVqx.png](https://z3.ax1x.com/2021/04/15/cRnVqx.png)](https://imgtu.com/i/cRnVqx)

当下一个node指回上一个node时，会形成**双向链表**；

[![cRnnIO.png](https://z3.ax1x.com/2021/04/15/cRnnIO.png)](https://imgtu.com/i/cRnnIO)

双向链表需要额外的两个空间来存储后继结点和前驱结点的地址。所以，如果存储同样多的数据，双向链表要比单链表占用更多的内存空间。虽然两个指针比较浪费存储空间，但可以支持双向遍历，这样也带来了双向链表操作的灵活性。

[![4iXqe0.md.jpg](https://z3.ax1x.com/2021/09/13/4iXqe0.md.jpg)](https://imgtu.com/i/4iXqe0)

当尾指针指回头指针时，形成**循环链表**。跟单链表唯一的区别就在尾结点。我们知道，单链表的尾结点指针指向空地址，表示这就是最后的结点了。而循环链表的尾结点指针是指向链表的头结点。**循环链表**的优点是从链尾到链头比较方便。当要处理的数据具有环型结构特点时，就特别适合采用循环链表。比如著名的[约瑟夫问题](https://zh.wikipedia.org/wiki/约瑟夫斯问题)。

### 链表增加结点

1. 前继节点指针指向新节点
2. 新节点next指针指向之前的下一个节点

[![cRneZ6.png](https://z3.ax1x.com/2021/04/15/cRneZ6.png)](https://imgtu.com/i/cRneZ6)

时间复杂度分析：操作2次，但是是常数次，时间复杂度为O(1)

### 链表删除结点

1. 打掉前驱节点next，移到后继节点
2. 打掉目标节点的next

[![cRnAMR.png](https://z3.ax1x.com/2021/04/15/cRnAMR.png)](https://imgtu.com/i/cRnAMR)

不涉及整个链表的群移操作，亦不涉及元素的复制，时间复杂度为O(1)

### 链表元素的访问

访问链表中任意一个位置，由于没有索引的概念，必须从头结点开始一个一个的往后挪，直到移动到你访问的位置。平均下来的时间复杂度为O(n/2)，也就是O(n)

### 工程应用

LRU Cache

缓存的大小有限，当缓存被用满时，哪些数据应该被清理出去，哪些数据应该被保留？这就需要缓存淘汰策略来决定。常见的策略有三种：先进先出策略 FIFO（First In，First Out）、最少使用策略 LFU（Least Frequently Used）、最近最少使用策略 LRU（Least Recently Used）

**基于链表实现 LRU 缓存淘汰算法**

我们维护一个有序单链表，越靠近链表尾部的结点是越早之前访问的。当有一个新的数据被访问时，我们从链表头开始顺序遍历链表。

1. 如果此数据之前已经被缓存在链表中了，我们遍历得到这个数据对应的结点，并将其从原来的位置删除，然后再插入到链表的头部。

2. 如果此数据没有在缓存链表中，又可以分为两种情况：

- 如果此时缓存未满，则将此结点直接插入到链表的头部；
- 如果此时缓存已满，则链表尾结点删除，将新的数据结点插入链表的头部

因为不管缓存有没有满，我们都需要遍历一遍链表，所以这种基于链表的实现思路，缓存访问的时间复杂度为 O(n)

详见：

[LRU缓存算法](https://www.jianshu.com/p/b1ab4a170c3c)

### Java源码引用（JDK8）

成员变量以及构造方法

```Java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
{
    transient int size = 0;

    /**
     * Pointer to first node.
     * Invariant: (first == null && last == null) ||
     *            (first.prev == null && first.item != null)
     */
    transient Node<E> first;

    /**
     * Pointer to last node.
     * Invariant: (first == null && last == null) ||
     *            (last.next == null && last.item != null)
     */
    transient Node<E> last;

    /**
     * Constructs an empty list.
     */
    public LinkedList() {
    }
```

成员变量中定义了：元素个数，双向链表的头指针，双向链表的尾部指针；此外，链表中有一个内部类Node，其定义如下：

```Java
    private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
```

这个class的属性有三个：item表示当前数据元素，next表示下一个元素的引用，prev表示前一个元素的引用。`LinkedList`正是利用这个双向链表的数据结构实现了内部的元素存储，并通过维护一个头指针和尾指针，进行元素的增加和删除。

### 写链表代码技巧

1. **理解指针或引用的含义**

   对于指针(Java可以理解为引用)的理解：**将某个变量赋值给指针，实际上就是将这个变量的地址赋值给指针，或者反过来说，指针中存储了这个变量的内存地址，指向了这个变量，通过指针就能找到这个变量**

2. **警惕指针丢失和内存泄漏**

   **插入结点时，一定要注意操作的顺序**，要先将结点 x 的 next 指针指向结点 b，再把结点 a 的 next 指针指向结点 x，这样才不会丢失指针，导致内存泄漏。

   **删除链表结点时，也一定要记得手动释放内存空间**，否则，也会出现内存泄漏的问题

3. **利用哨兵简化实现难度**

   **针对链表的插入、删除操作，需要对插入第一个结点和删除最后一个结点的情况进行特殊处理**，

   哨兵，解决的是国家之间的边界问题。同理，这里说的哨兵也是解决“边界问题”的，不直接参与业务逻辑。引入哨兵结点，在任何时候，不管链表是不是空，head 指针都会一直指向这个哨兵结点。我们也把这种有哨兵结点的链表叫**带头链表**。相反，没有哨兵结点的链表就叫作**不带头链表**。

   因为哨兵结点一直存在，所以插入第一个结点和插入其他结点，删除最后一个结点和删除其他结点，都可以统一为相同的代码实现逻辑了。

4. **重点留意边界条件处理**

   - 如果链表为空时，代码是否能正常工作？
   - 如果链表只包含一个结点时，代码是否能正常工作？
   - 如果链表只包含两个结点时，代码是否能正常工作？
   - 代码逻辑在处理头结点和尾结点的时候，是否能正常工作

5. **举例画图，辅助思考**

6. **多写多练，没有捷径**

## Skip List🔥

### 跳表原理

目的是优化链表的访问效率，核心思想是**升维**，in other words，**空间换时间**

访问链表的首尾元素是最快的，那么我们可以将链表进行2分处理，即在中间位置上增加一个指针，同时添加一级索引，如果我们要访问中间位置的节点，我们可以直接从一级索引过去，不用从首节点，一步步移过去。

[![cRnKiD.png](https://z3.ax1x.com/2021/04/15/cRnKiD.png)](https://imgtu.com/i/cRnKiD)

同理，可以增加二级索引

[![cRnMJe.png](https://z3.ax1x.com/2021/04/15/cRnMJe.png)](https://imgtu.com/i/cRnMJe)

五级索引

[![cRnQRH.png](https://z3.ax1x.com/2021/04/15/cRnQRH.png)](https://imgtu.com/i/cRnQRH)

假设原始链表有n个节点且为2的整数幂，建有k级索引，k级索引有2个节点，则

| 索引级数k | k级索引节点数 |
| :-------: | :-----------: |
|     0     |       n       |
|     1     |      n/2      |
|     2     |      n/4      |
|     h     |     n/2^h     |
|     k     |       2       |

则
$$
n/2^k=2
$$
可以推出
$$
k=log_2{n}-1
$$
每层索引需要遍历的节点个数为3，那么一共要走3k个节点，其时间复杂度为
$$
O(3*log_2{n}-3)
$$
去掉常数为：
$$
O(log_2{n})
$$
更加详细的介绍详见：

[一文彻底搞懂跳表的各种时间复杂度、适用场景以及实现原理](https://juejin.cn/post/6844903955831619597)

### 跳表工程应用

Redis

详见：

[跳跃表](https://redisbook.readthedocs.io/en/latest/internal-datastruct/skiplist.html)
[为啥 redis 使用跳表(skiplist)](:https://www.zhihu.com/question/20202931)

## 数组与链表的对比🔥

|          | 数组                                                         | 链表                                                         |
| -------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 插入删除 | O（n）                                                       | O（1）                                                       |
| 随机访问 | O（1）                                                       | O（n）                                                       |
| 优点     | 数组简单易用，在实现上使用的是连续的内存空间，可以借助 CPU 的缓存机制，预读数组中的数据，所以访问效率更高。而链表在内存中并不是连续存储，所以对 CPU 缓存不友好，没办法有效预读 | 链表本身没有大小的限制，天然地支持动态扩容                   |
| 缺点     | 大小固定，一经声明就要占用整块连续内存空间。如果声明的数组过大，系统可能没有足够的连续内存空间分配给它，导致“内存不足（out of memory）”。如果声明的数组过小，则可能出现不够用的情况。这时只能再申请一个更大的内存空间，把原数组拷贝进去，非常费时 | 链表中的每个结点都需要消耗额外的存储空间去存储一份指向下一个结点的指针，所以内存消耗会翻倍。而且，对链表进行频繁的插入、删除操作，还会导致频繁的内存申请和释放，容易造成内存碎片，如果是 Java 语言，就有可能会导致频繁的 GC（Garbage Collection，垃圾回收） |












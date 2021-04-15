---
title: 'Jevin学算法与数据结构-数组,链表,跳表'
date: 2021-04-15 21:11:43
tags: Java
categories: 数据结构与算法
mathjax: true # 加入这个声明，这篇文章就会开启mathjax渲染
---
## 数组，链表，跳表🌈

## Array🔥

### 常见写法

Java，C++  ：int a[100]

Python          : list=[]

JavaScript    : let   x=[1,2,3]

### 底层实现

在申请创建数组时，计算机在内存中开辟一段连续地址，每一个地址可以通过内存管理器进行访问。如图所示

[![cRnmdK.png](https://z3.ax1x.com/2021/04/15/cRnmdK.png)](https://imgtu.com/i/cRnmdK)

内存管理器访问数组中任意一个地址的时间复杂度都是O(1)。因此，数组查询数据的速度非常快。

### 数组增加元素

如图所示，假设存在一个数组，里面的元素为ABCEFG，现在要插入元素D

[![cRnEs1.png](https://z3.ax1x.com/2021/04/15/cRnEs1.png)](https://imgtu.com/i/cRnEs1)

1. 把EFG往下挪一个位置
2. 插入D

时间复杂度分析：最好的情况插入到最后一个O(1)，最坏的情况插入到第一个，时间复杂度为O(n)，平均下来时间复杂度为O(n/2)。

### 数组删除元素

以上过程的逆过程

1. 拿出D
2. 下面的元素向上挪一个位置

### 数组元素访问

根据索引直接访问对应的内存即可，时间复杂度为O(1)

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

### 链表结构

每一个元素（实际上是一个class，也叫node）有value和next两个成员变量，next指向下一个元素，串在一起就形成了**单向链表**；

[![cRnVqx.png](https://z3.ax1x.com/2021/04/15/cRnVqx.png)](https://imgtu.com/i/cRnVqx)

当下一个node指回上一个node时，会形成**双向链表**；

[![cRnnIO.png](https://z3.ax1x.com/2021/04/15/cRnnIO.png)](https://imgtu.com/i/cRnnIO)

当尾指针指回头指针时，形成**循环链表**。

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

详见：

[LRU缓存算法]: https://www.jianshu.com/p/b1ab4a170c3c

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

[一文彻底搞懂跳表的各种时间复杂度、适用场景以及实现原理]: https://juejin.cn/post/6844903955831619597

### 跳表工程应用

Redis

详见：

[跳跃表]: https://redisbook.readthedocs.io/en/latest/internal-datastruct/skiplist.html
[为啥 redis 使用跳表(skiplist)]: https://www.zhihu.com/question/20202931
















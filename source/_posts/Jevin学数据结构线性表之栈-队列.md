---
title: Jevin学数据结构线性表之栈-队列
date: 2021-09-22 22:42:04
tags: 数据结构
categories: 数据结构与算法
---
## 栈，队列🌈

### 栈

**栈是一种“操作受限”的线性表**，只允许在一端插入和删除数据，一种先进后出的容器结构，如图所示：

[![4aePVH.png](https://z3.ax1x.com/2021/09/22/4aePVH.png)](https://imgtu.com/i/4aePVH)

**当某个数据集合只涉及在一端插入和删除数据，并且满足后进先出、先进后出的特性，我们就应该首选“栈”这种数据结构**

添加，删除操作时间复杂度皆为O(1)

查询操作为O(n),因为无序，所以要查找任何一个元素就需要遍历整个数据结构

#### 实现一个栈

栈既可以用数组来实现，也可以用链表来实现。用数组实现的栈，我们叫作**顺序栈**，用链表实现的栈，我们叫作**链式栈**

**顺序栈的Java代码实现**

```java
// 基于数组实现的顺序栈
public class ArrayStack {
  private String[] items;  // 数组
  private int count;       // 栈中元素个数
  private int n;           // 栈的大小
 
  // 初始化数组，申请一个大小为 n 的数组空间
  public ArrayStack(int n) {
    this.items = new String[n];
    this.n = n;
    this.count = 0;
  }
 
  // 入栈操作
  public boolean push(String item) {
    // 数组空间不够了，直接返回 false，入栈失败。
    if (count == n) return false;
    // 将 item 放到下标为 count 的位置，并且 count 加一
    items[count] = item;
    ++count;
    return true;
  }
  
  // 出栈操作
  public String pop() {
    // 栈为空，则直接返回 null
    if (count == 0) return null;
    // 返回下标为 count-1 的数组元素，并且栈中元素个数 count 减一
    String tmp = items[count-1];
    --count;
    return tmp;
  }
}
```

复杂度分析：

我们存储数据只需要一个大小为 n 的数组就够了。在入栈和出栈过程中，只需要一两个临时变量存储空间，所以空间复杂度是 O(1)，这里存储数据需要一个大小为 n 的数组，并不是说空间复杂度就是 O(n)。因为，这 n 个空间是必须的，无法省掉。所以我们说空间复杂度的时候，是指除了原本的数据存储空间外，算法运行还需要额外的存储空间。

不管是顺序栈还是链式栈，入栈、出栈只涉及栈顶个别数据的操作，所以时间复杂度都是 O(1)

**链式栈的Java代码实现**

```java
//TO-DO
```

#### 支持动态扩容的顺序栈

如果要实现一个支持动态扩容的栈，我们只需要底层依赖一个支持动态扩容的数组就可以了。当栈满了之后，我们就申请一个更大的数组，将原来的数据搬移到新数组中。

对于出栈操作来说，我们不会涉及内存的重新申请和数据的搬移，所以出栈的时间复杂度仍然是 O(1)。但是，对于入栈操作来说，情况就不一样了。当栈中有空闲空间时，入栈操作的时间复杂度为 O(1)。但当空间不够时，就需要重新申请内存和数据搬移，所以时间复杂度就变成了 O(n)。

如果当前栈大小为 K，并且已满，当再有新的数据要入栈时，就需要重新申请 2 倍大小的内存，并且做 K 个数据的搬移操作，然后再入栈。但是，接下来的 K-1 次入栈操作，我们都不需要再重新申请内存和搬移数据，所以这 K-1 次入栈操作都只需要一个 simple-push 操作就可以完成。

这 K 次入栈操作，总共涉及了 K 个数据的搬移，以及 K 次 simple-push 操作。将 K 个数据搬移均摊到 K 次入栈操作，那每个入栈操作只需要一个数据搬移和一个 simple-push 操作。以此类推，入栈操作的均摊时间复杂度就为 O(1)。

[![4aeprD.png](https://z3.ax1x.com/2021/09/22/4aeprD.png)](https://imgtu.com/i/4aeprD)

#### 栈的应用

1. 函数调用
2. 表达式求值
3. 括号匹配

### 队列

队列这个概念非常好理解。你可以把它想象成排队买票，先来的先买，后来的人只能站末尾，不允许插队。**先进者先出，这就是典型的“队列”**

一种先进先出的容器结构，如图所示：

[![4aZxxK.png](https://z3.ax1x.com/2021/09/22/4aZxxK.png)](https://imgtu.com/i/4aZxxK)

添加，删除操作时间复杂度皆为O(1)

查询操作为O(n),因为无序，所以要查找任何一个元素就需要遍历整个数据结构

#### 实现一个队列

跟栈一样，队列可以用数组来实现，也可以用链表来实现。用数组实现的栈叫作顺序栈，用链表实现的栈叫作链式栈。同样，用数组实现的队列叫作**顺序队列**，用链表实现的队列叫作**链式队列**。

**顺序队列的Java代码实现**

```java
// 用数组实现的队列
public class ArrayQueue {
  // 数组：items，数组大小：n
  private String[] items;
  private int n = 0;
  // head 表示队头下标，tail 表示队尾下标
  private int head = 0;
  private int tail = 0;
 
  // 申请一个大小为 capacity 的数组
  public ArrayQueue(int capacity) {
    items = new String[capacity];
    n = capacity;
  }
 
  // 入队
  public boolean enqueue(String item) {
    // 如果 tail == n 表示队列已经满了
    if (tail == n) return false;
    items[tail] = item;
    ++tail;
    return true;
  }
 
  // 出队
  public String dequeue() {
    // 如果 head == tail 表示队列为空
    if (head == tail) return null;
    // 为了让其他语言的同学看的更加明确，把 -- 操作放到单独一行来写了
    String ret = items[head];
    ++head;
    return ret;
  }
}
```

队列需要两个指针：一个是 head 指针，指向队头；一个是 tail 指针，指向队尾。

如图，当 a、b、c、d 依次入队之后，队列中的 head 指针指向下标为 0 的位置，tail 指针指向下标为 4 的位置。

[![4ae9qe.jpg](https://z3.ax1x.com/2021/09/22/4ae9qe.jpg)](https://imgtu.com/i/4ae9qe)

当我们调用两次出队操作之后，队列中 head 指针指向下标为 2 的位置，tail 指针仍然指向下标为 4 的位置。

[![4aeiad.jpg](https://z3.ax1x.com/2021/09/22/4aeiad.jpg)](https://imgtu.com/i/4aeiad)

随着不停地进行入队、出队操作，head 和 tail 都会持续往后移动。当 tail 移动到最右边，即使数组中还有空闲空间，也无法继续往队列中添加数据了，这时我们就需要进行数据搬移，有两种方式，一是每次进行出队操作都搬移整个队列中的数据，这样出队操作的时间复杂度就会从原来的 O(1) 变为 O(n)；二是我们在**出队时可以不用搬移数据**。如果没有空闲空间了，我们只需要在入队时，再**集中触发**一次数据的搬移操作。很显然使用方式二更好。

稍加改造一下入队函数 enqueue() 的实现，就可以轻松解决刚才的问题了。下面是具体的代码：

```java
   // 入队操作，将 item 放入队尾
  public boolean enqueue(String item) {
    // tail == n 表示队列末尾没有空间了
    if (tail == n) {
      // tail ==n && head==0，表示整个队列都占满了
      if (head == 0) return false;
      // 数据搬移
      for (int i = head; i < tail; ++i) {
        items[i-head] = items[i];
      }
      // 搬移完之后重新更新 head 和 tail
      tail -= head;
      head = 0;
    }
    
    items[tail] = item;
    ++tail;
    return true;
  }
```

当队列的 tail 指针移动到数组的最右边后，如果有新的数据入队，我们可以将 head 到 tail 之间的数据，整体搬移到数组中 0 到 tail-head 的位置，出队操作的时间复杂度仍然是 O(1)，但入队操作的时间复杂度还是 O(1)

[![4aeEGt.jpg](https://z3.ax1x.com/2021/09/22/4aeEGt.jpg)](https://imgtu.com/i/4aeEGt)

**链式队列的Java代码实现**

```java
//TO-DO
```

基于链表的实现，我们同样需要两个指针：head 指针和 tail 指针。它们分别指向链表的第一个结点和最后一个结点，入队时，tail->next= new_node, tail = tail->next；出队时，head = head->next

[![4aZv26.jpg](https://z3.ax1x.com/2021/09/22/4aZv26.jpg)](https://imgtu.com/i/4aZv26)

#### 循环队列

用数组来实现队列的时候，在 tail==n 时，会有数据搬移操作，这样入队操作性能就会受到影响，使用循环队列可以解决这个问题。

[![4aeFIA.jpg](https://z3.ax1x.com/2021/09/22/4aeFIA.jpg)](https://imgtu.com/i/4aeFIA)

图中这个队列的大小为 8，当前 head=4，tail=7。当有一个新的元素 a 入队时，我们放入下标为 7 的位置。但这个时候，我们并不把 tail 更新为 8，而是将其在环中后移一位，到下标为 0 的位置。当再有一个元素 b 入队时，我们将 b 放入下标为 0 的位置，然后 tail 加 1 更新为 1。所以，在 a，b 依次入队之后，循环队列中的元素就变成了下面的样子：

[![4aeAPI.jpg](https://z3.ax1x.com/2021/09/22/4aeAPI.jpg)](https://imgtu.com/i/4aeAPI)

循环队列为空的判断条件仍然是 head == tail，图中画的队满的情况，tail=3，head=4，n=8，所以总结一下规律就是：(3+1)%8=4。多画几张队满的图，你就会发现，当队满时，**(tail+1)%n=head**，当队列满时，图中的 tail 指向的位置实际上是没有存储数据的。所以，循环队列会浪费一个数组的存储空间。

[![4aeEGt.jpg](https://z3.ax1x.com/2021/09/22/4aeEGt.jpg)](https://imgtu.com/i/4aeEGt)

循环队列的Java代码实现

```java
public class CircularQueue {
  // 数组：items，数组大小：n
  private String[] items;
  private int n = 0;
  // head 表示队头下标，tail 表示队尾下标
  private int head = 0;
  private int tail = 0;
 
  // 申请一个大小为 capacity 的数组
  public CircularQueue(int capacity) {
    items = new String[capacity];
    n = capacity;
  }
 
  // 入队
  public boolean enqueue(String item) {
    // 队列满了
    if ((tail + 1) % n == head) return false;
    items[tail] = item;
    tail = (tail + 1) % n;
    return true;
  }
 
  // 出队
  public String dequeue() {
    // 如果 head == tail 表示队列为空
    if (head == tail) return null;
    String ret = items[head];
    head = (head + 1) % n;
    return ret;
  }
}
```

#### 阻塞队列

**阻塞队列**其实就是在队列基础上增加了阻塞操作。简单来说，就是在队列为空的时候，从队头取数据会被阻塞。因为此时还没有数据可取，直到队列中有了数据才能返回；如果队列已经满了，那么插入数据的操作就会被阻塞，直到队列中有空闲位置后再插入数据，然后再返回。

[![4aeZxf.jpg](https://z3.ax1x.com/2021/09/22/4aeZxf.jpg)](https://imgtu.com/i/4aeZxf)

基于阻塞队列实现的“生产者 - 消费者模型”，可以有效地协调生产和消费的速度。当“生产者”生产数据的速度过快，“消费者”来不及消费时，存储数据的队列很快就会满了。这个时候，生产者就阻塞等待，直到“消费者”消费了数据，“生产者”才会被唤醒继续“生产”。

#### 并发队列

在阻塞队列中，我们可以多配置几个“消费者”，来应对一个“生产者”，来提高数据的处理效率。在多线程情况下，会有多个线程同时操作队列此时会带来一个线程安全的问题。

[![4aemM8.jpg](https://z3.ax1x.com/2021/09/22/4aemM8.jpg)](https://imgtu.com/i/4aemM8)

线程安全的队列我们叫作**并发队列**。最简单直接的实现方式是直接在 enqueue()、dequeue() 方法上加锁，但是锁粒度大并发度会比较低，同一时刻仅允许一个存或者取操作。实际上，基于数组的循环队列，利用 CAS 原子操作，可以实现非常高效的并发队列。这也是循环队列比链式队列应用更加广泛的原因。

#### 工程应用

##### 线程池请求排队

线程池没有空闲线程时，新的任务请求线程资源时，线程池该如何处理？有两种处理策略。第一种是非阻塞的处理方式，直接拒绝任务请求；另一种是阻塞的处理方式，将请求排队，等到有空闲线程时，取出排队的请求继续处理。

我们希望公平地处理每个排队的请求，先进者先服务，所以队列这种数据结构很适合来存储排队请求。**基于链表的实现方式**，可以实现一个**支持无限排队的无界队列**（unbounded queue），但是可能会导致过多的请求排队等待，请求处理的响应时间过长。所以，针**对响应时间比较敏感**的系统，基于链表实现的无限排队的线程池是不合适的。

**基于数组实现的有界队列**（bounded queue），队列的大小有限，所以线程池中排队的请求超过队列大小时，接下来的请求就会被拒绝，这种方式对响应时间敏感的系统来说，就相对更加合理。不过，设置一个合理的队列大小，也是非常有讲究的。队列太大导致等待的请求太多，队列太小会导致无法充分利用系统资源、发挥最大性能。

**实际上，对于大部分资源有限的场景，当没有空闲资源时，基本上都可以通过“队列”这种数据结构来实现请求排队**

无锁并发队列的实现

##### 双端队列

栈和队列的结合体

[![4aeSKO.png](https://z3.ax1x.com/2021/09/22/4aeSKO.png)](https://imgtu.com/i/4aeSKO)

可以在队列的首尾进行添加删除元素

添加，删除操作时间复杂度皆为O(1)

查询操作为O(n),因为无序，所以要查找任何一个元素就需要遍历整个数据结构

在Java中Deque为一个接口，Interface Deque<E>，所有的实现类为[ArrayDeque](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayDeque.html), [ConcurrentLinkedDeque](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentLinkedDeque.html), [LinkedBlockingDeque](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/LinkedBlockingDeque.html), [LinkedList](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html)

##### 优先队列

**Priority Queue**

1.插入操作：O(1)

2.取出操作：O(logN)，按照元素的优先级取出（使用简单queue为O(N)）

3.底层实现较为多样和复杂：heap，bst，treap




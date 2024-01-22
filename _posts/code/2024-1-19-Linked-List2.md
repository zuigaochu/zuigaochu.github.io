---
layout:     post
title:      "数据结构——链表"
date:       2024-1-19
author:     "Mtz"
catalog: true
tags:
    - 数据结构与算法
    - 链表
---

# 概述

*  **定义**

> 链表是一种物理结构上非连续、非顺序的存储结构，数据元素的逻辑顺序是通过链表中的指针链接次序实现的。

<br/>

*  **特点**

动态分配内存、非连续存储、节点、插入和删除操作高效、不需要预分配空间、随机访问困难。

<br/>

*  **内存**

![list9](https://telegraph-image-a8w.pages.dev/file/b18a82843604e805918e5.png)

分析：

1. 从图中可以看出，链式结构在逻辑上是连续的，但在物理上不一定连续。
2. 现实中的结点一般都是从堆上申请出来的。
3. 从堆上申请的空间，是按照一定的策略来分配的，两次申请的空间可能连续，也可能不连续。

**堆内存（Heap Memory）：** 用于存储动态分配的对象。所有通过 `new` 创建的对象，包括类的实例对象，都被存储在堆内存中。堆内存的生命周期通常取决于对象的引用是否存在，当没有任何引用指向对象时，对象就成为不可访问的垃圾，最终由垃圾回收器回收。

**栈内存（Stack Memory）：** 用于存储局部变量和方法调用的信息。栈内存的生命周期与方法调用的开始和结束相关，当方法执行结束时，栈帧中的局部变量会被销毁。基本数据类型和对象的引用可以存储在栈内存中，但实际对象的内容仍然存储在堆内存中。

<br/>

*  **分类**

可以分类为

* 单向链表，每个元素只知道其下一个元素是谁

![list](https://telegraph-image-a8w.pages.dev/file/192cae5159d04a542a676.png)

* 双向链表，每个元素直到其上一个元素和下一个元素

![list2](https://telegraph-image-a8w.pages.dev/file/d2af87c05d83719e13544.png)

* 循环链表，通常的链表尾节点tail指向的都是null,而循环链表的tail指向的是头节点head.

![list3](https://telegraph-image-a8w.pages.dev/file/b36563e207b5f79e260c8.png)

<br/>

* **Java中引用链表实现的数据结构**

1. LinkedList (双向链表)
2. LinkedHashMap (链表的哈希表)
3. LinkedHashSet(链表的哈希集合)
4. Queue 接口的实现类 （ Queue<String> queue = new LinkedList<>();）

<br/>

# 代码实现

## 单向链表

```java
package com.ma.bianli;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.NoSuchElementException;

public class SinglyLinkedListSentinel implements Iterable<Integer>  {

    private Node head; // 头部节点

    private static class Node { // 节点类
        int value;
        Node next;

        public Node(int value, Node next) {
            this.value = value;
            this.next = next;
        }
    }
    private Node findLast() {
        if (this.head == null) {
            return null;
        }
        Node curr;
        for (curr = this.head; curr != null && curr.next != null; ) {
            curr = curr.next;
        }
        return curr;
    }

    public void addLast(int value) {
        Node last = findLast();
        if (last == null) {
            // 链表为空，创建新节点作为头节点
            this.head = new Node(value, null);
        } else {
            // 链表不为空，将新节点追加到最后
            last.next = new Node(value, null);
        }
    }

    private Node findNode(int index) {
        if (index == 0) {
            return this.head;
        }
        int i = 0;
        for (Node curr = this.head; curr != null; curr = curr.next, i++) {
            if (index == i) {
                return curr;
            }
        }
        return null;
    }

    public void insert(int index, int value) {
        Node prev = findNode(index - 1);
        if (prev != null) {
            prev.next = new Node(value, prev.next);
        } else {
            throw illegalIndex(index);
        }
    }

    public void remove(int index) {
        if (index == 0) {
            this.head = this.head.next;
        } else {
            Node prev = findNode(index - 1);
            Node curr;
            if (prev != null && (curr = prev.next) != null) {
                prev.next = curr.next;
            } else {
                throw illegalIndex(index);
            }
        }
    }


    public void addFirst(int value) {
        if (this.head == null) {
            this.head = new Node(value, null);
        } else {
            this.head = new Node(value, this.head);
        }
    }


    private IllegalArgumentException illegalIndex(int index) {
        return new IllegalArgumentException(String.format("index [%d] 不合法%n", index));
    }



    public Iterator<Integer> iterator() {
        return new NodeIterator();
    }

    private class NodeIterator implements Iterator<Integer> {
        Node curr = head;
        Node prev = null; // 用于支持 remove 操作

        public boolean hasNext() {
            return curr != null;
        }

        public Integer next() {
            if (!hasNext()) {
                throw new NoSuchElementException();
            }
            int value = curr.value;
            prev = curr;
            curr = curr.next;
            return value;
        }

        public void remove() {
            if (prev == null) {
                throw new IllegalStateException("不能在调用 next() 之前调用 remove()");
            }
            prev.next = curr; // 删除当前元素
        }
    }

 // 测试
    public static void main(String[] args) {

        SinglyLinkedListSentinel list = new SinglyLinkedListSentinel();
        list.addLast(1);
        list.addLast(3);
        Node last = list.findLast();
        System.out.println(last);
        list.insert(1, 2); // 在位置 1 插入元素 2

        for (int value : list) {
            System.out.print(value + " "); // 1 2 3
        }

    }
}
```



###  addFirst添加新结点

**代码示例**

```java
    public void addFirst(int value) {
        if (this.head == null) {
            this.head = new Node(value, null);
            System.out.println("头节点:"+head.value);
            System.out.println("头节点 next:"+head.next);
            System.out.println("头节点 head:"+this.head);
        } else {
            this.head = new Node(value, this.head);
            System.out.println("头节点:"+head.value);
            System.out.println("头节点 next:"+head.next.value);
            System.out.println("头节点 head:"+this.head);
        }
    }
```

```java
头节点:1
头节点 next:null
头节点 head:com.ma.linkedList.SinglyLinkedListSentinel$Node@2f0e140b
头节点:2
头节点 next:1
头节点 head:com.ma.linkedList.SinglyLinkedListSentinel$Node@7440e464
头节点:3
头节点 next:2
头节点 head:com.ma.linkedList.SinglyLinkedListSentinel$Node@49476842
头节点:4
头节点 next:3
头节点 head:com.ma.linkedList.SinglyLinkedListSentinel$Node@78308db1
4 3 2 1 
```



* 第一次添加值的情况

```java
head = new Node(value,null);
```

<img src="https://telegraph-image-a8w.pages.dev/file/d8e64f3bf5bc8deb72b9b.png" alt="list10" style="zoom:50%;" />

分析： 第一次添加值，其实就是新结点，赋值给头结点的过程， 也就是head = Node.

* 第二次添加值的情况

```java
head = new Node(value,head);
```

<img src="https://telegraph-image-a8w.pages.dev/file/e81dd91650af4501891e1.png" alt="list11" style="zoom:50%;" />

分析：把上一个的对象Node作为next ，值变换，组成一个新的结点Node，指向head，那么现在这个新的结点Next实际上就是第一次添加的结点，他们一模一样，内存的地址也一样。

观察得出此结论。

```java
头节点:1
头节点 next:null
头节点 head:com.ma.linkedList.SinglyLinkedListSentinel$Node@2f0e140b
头节点:2
头节点 next:1
头节点 head:com.ma.linkedList.SinglyLinkedListSentinel$Node@7440e464
```

**指针内存拓展**

> 硬件里头有一种特殊的寄存器，可以用来寻址，内存的管理是有分页的，一块一块的，你要知道数据存在哪里，可以把这个地址的信息放到寄存器,那么下一次你要的时候，找到这个寄存器就能马上找到内存中的具体位置





## 双向链表

```java
package com.ma.linkedList;

import java.util.Iterator;

public class DoublyLinkedListSentinel implements Iterable<Integer> {

    private final Node head;
    private final Node tail;

    static class Node {
        Node prev;
        int value;
        Node next;

        public Node(Node prev, int value, Node next) {
            this.prev = prev;
            this.value = value;
            this.next = next;
        }
    }

    public DoublyLinkedListSentinel() {
        head = new Node(null, 666, null);
        tail = new Node(null, 888, null);
        head.next = tail;
        tail.prev = head;
    }

    private Node findNode(int index) {
        int i = -1;
        for (Node p = head; p != tail; p = p.next, i++) {
            if (i == index) {
                return p;
            }
        }
        return null;
    }

    public void addFirst(int value) {
        insert(0, value);
    }

    public void removeFirst() {
        remove(0);
    }

    public void addLast(int value) {
        Node prev = tail.prev;
        Node added = new Node(prev, value, tail);
        prev.next = added;
        tail.prev = added;
    }

    public void removeLast() {
        Node removed = tail.prev;
        if (removed == head) {
            throw illegalIndex(0);
        }
        Node prev = removed.prev;
        prev.next = tail;
        tail.prev = prev;
    }

    public void insert(int index, int value) {
        Node prev = findNode(index - 1);
        if (prev == null) {
            throw illegalIndex(index);
        }
        Node next = prev.next;
        Node inserted = new Node(prev, value, next);
        prev.next = inserted;
        next.prev = inserted;
    }

    public void remove(int index) {
        Node prev = findNode(index - 1);
        if (prev == null) {
            throw illegalIndex(index);
        }
        Node removed = prev.next;
        if (removed == tail) {
            throw illegalIndex(index);
        }
        Node next = removed.next;
        prev.next = next;
        next.prev = prev;
    }

    private IllegalArgumentException illegalIndex(int index) {
        return new IllegalArgumentException(
                String.format("index [%d] 不合法%n", index));
    }

    @Override
    public Iterator<Integer> iterator() {
        return new Iterator<Integer>() {
            Node p = head.next;

            @Override
            public boolean hasNext() {
                return p != tail;
            }

            @Override
            public Integer next() {
                int value = p.value;
                p = p.next;
                return value;
            }
        };
    }


    public static void main(String[] args) {
        DoublyLinkedListSentinel list = new DoublyLinkedListSentinel();
        list.addLast(1);
        list.addLast(2);

        list.addFirst(0); // 在头部添加元素 0
        list.removeFirst(); // 删除头部元素

        list.addLast(3); // 在尾部添加元素 3
        list.removeLast(); // 删除尾部元素

        for (int value : list) {
            System.out.print(value + " "); // 1 2
        }

    }
}
```

### 链表初始化

* 构造方法

```java
    public DoublyLinkedListSentinel() {
        head = new Node(null, 666, null);
        tail = new Node(null, 888, null);
        head.next = tail;
        tail.prev = head;
    }t
```



<img src="https://telegraph-image-a8w.pages.dev/file/cccd417323d8a798c0fad.png" alt="list15" style="zoom:50%;" />

分析：head的next存储tail对象，tail的prev存储head对象。



###  findNode 根据索引找结点

```java
    private Node findNode(int index) {
        int i = -1;
        for (Node p = head; p != tail; p = p.next, i++) {
            if (i == index) {
                return p;
            }
        }
        return null;
    }
```

分析：循环中的p是一个指向链表结点的引用，实际上存储的是结点的地址（在java中是对象引用），不断更新p的值，指向链表中的下一个结点，从而遍历整个链表。

###　inser插入结点

```java
    public void insert(int index, int value) {
        Node prev = findNode(index - 1);
        if (prev == null) {
            throw illegalIndex(index);
        }
        Node next = prev.next;
        Node inserted = new Node(prev, value, next);
        prev.next = inserted;
        next.prev = inserted;
    }

```

<img src="https://telegraph-image-a8w.pages.dev/file/e1e26fb42f7ec3ebd88b1.png" alt="list16" style="zoom:50%;" />

**分析：**

1. 索引-1，找到上一个结点，设为A。
2. A的结点.next，找到下一个结点，设为B
3. 新结点指向A的next结点。
4. 新结点指向B的prev结点。



### remove删除结点

```java
    public void remove(int index) {
        Node prev = findNode(index - 1);
        if (prev == null) {
            throw illegalIndex(index);
        }
        Node removed = prev.next;
        if (removed == tail) {
            throw illegalIndex(index);
        }
        Node next = removed.next;
        prev.next = next;
        next.prev = prev;
    }
```

**分析：**

1. 找到上一个结点，设为A.
2. A的next 结点，设为B，也就是要删除的结点
3. B的next结点，设为C.
4. C结点指向A的next结点。
5. A结点指向C的prev结点。



## 环形链表（带哨兵）

![list4](https://telegraph-image-a8w.pages.dev/file/a66283fd02972835b5578.png)

![list8](https://telegraph-image-a8w.pages.dev/file/8e6e99544696b7402cbe2.png)

![list6](https://telegraph-image-a8w.pages.dev/file/ce0155b7531103cb056ac.png)

![list7](https://telegraph-image-a8w.pages.dev/file/5efa9df637f7a4ecf15f8.png)



```java
package com.ma.linkedList;

import java.util.Iterator;

public class DoublyLinkedListSentinel implements Iterable<Integer> {

    @Override
    public Iterator<Integer> iterator() {
        return new Iterator<Integer>() {
            Node p = sentinel.next;

            @Override
            public boolean hasNext() {
                return p != sentinel;
            }

            @Override
            public Integer next() {
                int value = p.value;
                p = p.next;
                return value;
            }
        };
    }

    static class Node {
        Node prev;
        int value;
        Node next;

        public Node(Node prev, int value, Node next) {
            this.prev = prev;
            this.value = value;
            this.next = next;
        }
    }

    private final Node sentinel = new Node(null, -1, null); // 哨兵

    public DoublyLinkedListSentinel() {
        sentinel.next = sentinel;
        sentinel.prev = sentinel;
    }

    /**
     * 添加到第一个
     *
     * @param value 待添加值
     */
    public void addFirst(int value) {
        Node next = sentinel.next;
        Node prev = sentinel;
        Node added = new Node(prev, value, next);
        prev.next = added;
        next.prev = added;
    }

    /**
     * 添加到最后一个
     *
     * @param value 待添加值
     */
    public void addLast(int value) {
        Node prev = sentinel.prev;
        Node next = sentinel;
        Node added = new Node(prev, value, next);
        prev.next = added;
        next.prev = added;
    }

    /**
     * 删除第一个
     */
    public void removeFirst() {
        Node removed = sentinel.next;
        if (removed == sentinel) {
            throw new IllegalArgumentException("非法");
        }
        Node a = sentinel;
        Node b = removed.next;
        a.next = b;
        b.prev = a;
    }

    /**
     * 删除最后一个
     */
    public void removeLast() {
        Node removed = sentinel.prev;
        if (removed == sentinel) {
            throw new IllegalArgumentException("非法");
        }
        Node a = removed.prev;
        Node b = sentinel;
        a.next = b;
        b.prev = a;
    }

    /**
     * 根据值删除节点
     * <p>假定 value 在链表中作为 key, 有唯一性</p>
     *
     * @param value 待删除值
     */
    public void removeByValue(int value) {
        Node removed = findNodeByValue(value);
        if (removed != null) {
            Node prev = removed.prev;
            Node next = removed.next;
            prev.next = next;
            next.prev = prev;
        }
    }

    private Node findNodeByValue(int value) {
        Node p = sentinel.next;
        while (p != sentinel) {
            if (p.value == value) {
                return p;
            }
            p = p.next;
        }
        return null;
    }

    public static void main(String[] args) {

        DoublyLinkedListSentinel list = new DoublyLinkedListSentinel();
        // 添加元素
        list.addFirst(1);
        list.addFirst(2);
        list.addFirst(3);


        printLinkedList(list);
    }

    private static void printLinkedList(DoublyLinkedListSentinel list) {
        for (int value : list) {
            System.out.print(value + " ");
        }
        System.out.println();
    }
}
```

###  环形链表初始化

<img src="https://telegraph-image-a8w.pages.dev/file/0d316577b83e09ac3b066.png" alt="list20" style="zoom:50%;" />

### 环形链表addFirst方法

```java
    public void addFirst(int value) {
        Node next = sentinel.next;
        Node prev = sentinel;
        Node added = new Node(prev, value, next);
        prev.next = added;
        next.prev = added;
    }
```

<img src="https://telegraph-image-a8w.pages.dev/file/53e22a7ec5be871768640.png" alt="list30" style="zoom:50%;" />

分析：

1. `Node next = sentinel.next;`: 获取链表中当前第一个节点的引用，即哨兵节点后的第一个节点。
2. `Node prev = sentinel;`: 获取哨兵节点的引用作为新节点的 `prev`，因为新节点将成为链表的第一个节点。
3. `Node added = new Node(prev, value, next);`: 创建一个新的节点 `added`，该节点的 `prev` 指向哨兵节点，`value` 设置为传入的参数 `value`，`next` 指向原来的第一个节点。
4. `prev.next = added;`: 将哨兵节点的 `next` 指向新节点 `added`，使新节点成为链表中的第一个节点。
5. `next.prev = added;`: 将原来第一个节点的 `prev` 指向新节点 `added`，建立新节点与原来第一个节点的双向连接。

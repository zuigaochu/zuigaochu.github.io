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

**定义**

> 在计算机科学中，链表是数据元素的线性集合，其每个元素都指向下一个元素，元素存储上并不连续

可以分类为

* 单向链表，每个元素只知道其下一个元素是谁

![list](https://telegraph-image-a8w.pages.dev/file/192cae5159d04a542a676.png)

* 双向链表，每个元素直到其上一个元素和下一个元素

![list2](https://telegraph-image-a8w.pages.dev/file/d2af87c05d83719e13544.png)

* 循环链表，通常的链表尾节点tail指向的都是null,而循环链表的tail指向的是头节点head.

![list3](https://telegraph-image-a8w.pages.dev/file/b36563e207b5f79e260c8.png)

链表内还有一种特殊的节点称为哨兵（Sentinel）节点，也叫做哑元（ Dummy）节点，它不存储数据，通常用作头尾，用来简化边界判断，如下图所示

![list4](https://gitee.com/writebird/cloudmage/raw/master/img/202401191016804.png)



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
        ArrayList<Object> objects = new ArrayList<>();

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



## 双向链表（带哨兵）

```java
package com.ma.bianli;

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
        head.next = tail;·········
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



## 环形链表（带哨兵）

![list4](https://telegraph-image-a8w.pages.dev/file/a66283fd02972835b5578.png)

![list8](https://telegraph-image-a8w.pages.dev/file/8e6e99544696b7402cbe2.png)

![list6](https://telegraph-image-a8w.pages.dev/file/ce0155b7531103cb056ac.png)

![list7](https://telegraph-image-a8w.pages.dev/file/5efa9df637f7a4ecf15f8.png)



```Java
public class DoublyLinkedListSentinel implements Iterable<Integer> {

    @Override
    public Iterator<Integer> iterator() {
        return new Iterator<>() {
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

}
```


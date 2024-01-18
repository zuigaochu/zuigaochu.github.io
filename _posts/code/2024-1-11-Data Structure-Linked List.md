---
layout:     post
title:      "链表——LRU缓存"
date:       2024-1-11
author:     "Mtz"
catalog: true
tags:
    - 数据结构与算法
    - 链表
---

# [LRU 缓存](https://leetcode.cn/problems/lru-cache/)

> LRU缓存，限制容量，常用的数据存起来，没用过的数据如果超出容量则顶出去删除掉，新增的数据进来。应用场景：历史记录



## 算法实现

```java
package com.ma.bianli;

import java.util.HashMap;
import java.util.Map;

public class LRUCache {
    private static class Node {
        int key, value;
        Node prev, next;

        Node(int k, int v) {
            key = k;
            value = v;
        }
    }

    private final int capacity;
    private final Node dummy = new Node(0, 0); // 哨兵节点
    private final Map<Integer, Node> keyToNode = new HashMap<>();

    public LRUCache(int capacity) {
        this.capacity = capacity;
        dummy.prev = dummy;
        dummy.next = dummy;
    }

    public int get(int key) {
        Node node = getNode(key);
        return node != null ? node.value : -1;
    }

    public void put(int key, int value) {
        Node node = getNode(key);
        if (node != null) { // 有这本书
            node.value = value; // 更新 value
            return;
        }
        node = new Node(key, value); // 新书
        keyToNode.put(key, node);
        pushFront(node); // 放在最上面
        if (keyToNode.size() > capacity) { // 书太多了
            Node backNode = dummy.prev;
            keyToNode.remove(backNode.key);
            remove(backNode); // 去掉最后一本书
        }
    }

    private Node getNode(int key) {
        if (!keyToNode.containsKey(key)) { // 没有这本书
            return null;
        }
        Node node = keyToNode.get(key); // 有这本书
        remove(node); // 把这本书抽出来
        pushFront(node); // 放在最上面
        return node;
    }

    // 删除一个节点（抽出一本书）
    private void remove(Node x) {
        x.prev.next = x.next;
        x.next.prev = x.prev;
    }

    // 在链表头添加一个节点（把一本书放在最上面）
    private void pushFront(Node x) {
        x.prev = dummy;
        x.next = dummy.next;
        x.prev.next = x;
        x.next.prev = x;
    }
}

```



* LRU缓存的使用

```java
    public static void main(String[] args) {
        LRUCache lruCache = new LRUCache(3);
        lruCache.put(5, 5);
        lruCache.put(1, 1);
        lruCache.put(2, 2);

        lruCache.get(1);
        lruCache.put(3, 3);
        System.out.println(lruCache.get(5)); // 返回-1
    }
```

解释：容量为3， 放进去 5，1，2  ， 使用了1，删除掉了5 ，添加了3， 现存的数据为 1，2，3  ， 所以获取5的结果是-1找不到。 



# 图解

![链表](https://ts1.cn.mm.bing.net/th/id/R-C.8689c31dff2149b3d9742e26b9a06ed3?rik=UORGFzX1y4huUA&riu=http%3a%2f%2fimages.cnitblog.com%2fi%2f497634%2f201403%2f241342164043381.jpg&ehk=zZnKvuo1pt7ltsbXH4yxeTihYUQgzHwUmYUJm1V5IPU%3d&risl=&pid=ImgRaw&r=0)



## 算法讲解

* Node类的定义

1. 在LRUCache类内部定义了一个静态内部类Node
2. 表示双向链表中的节点。每个节点有一个键值对(key, value)，
3. 指向前一个节点（prev）和后一个节点（next）的引用。

```java
    private static class Node {
        int key, value;
        Node prev, next;

        Node(int k, int v) {
            key = k;
            value = v;
        }
    }

```

<br/>

* LRUCache 初始化

1. capacity表示LRU缓存的最大容量，
2. dummy是哨兵节点
3. keyToNode是用于存储键和对应节点的HashMap。
4. 声明了LRUCache的成员变量
5. LRUCache类的构造函数，初始化LRU缓存的容量和哨兵节点。

```java
private final int capacity;
private final Node dummy = new Node(0, 0); // 哨兵节点
private final Map<Integer, Node> keyToNode = new HashMap<>();

    public LRUCache(int capacity) {
        this.capacity = capacity;
        dummy.prev = dummy;
        dummy.next = dummy;
    }
```
<br/>

* 删除节点

1. `x.prev.next = x.next;`: 这一行将x节点的前一个节点的next指向x节点的下一个节点，跳过了x节点。这样就在链表中去除了x节点。
2. `x.next.prev = x.prev;`: 这一行将x节点的下一个节点的prev指向x节点的前一个节点，同样也跳过了x节点。这样，x节点就从双向链表中完全抽出，不再与链表相连接。

这两行代码的效果是将节点x从链表中移除，实现了双向链表中删除节点的操作。这在LRUCache中很关键，因为当缓存满了需要淘汰最久未使用的节点时，就需要删除链表尾部的节点。

```java
// 删除一个节点（抽出一本书）
private void remove(Node x) {
    x.prev.next = x.next;
    x.next.prev = x.prev;
}
```

<br/>

* 添加节点

1. `x.prev = dummy;`: 将节点x的prev指向哨兵节点dummy，表示x的前一个节点是dummy。
2. `x.next = dummy.next;`: 将节点x的next指向dummy的下一个节点，表示x的下一个节点是原链表头部的节点。
3. `x.prev.next = x;`: 将dummy节点的next指向x，这样dummy节点的下一个节点就变成了x。也就是说，x节点成为了新的链表头部。
4. `x.next.prev = x;`: 将原链表头部节点的prev指向x，这样原链表头部节点的前一个节点就变成了x。

这样一来，通过这四行代码，节点x就被成功插入到了双向链表的头部，成为了新的链表头部。这是LRU缓存算法的核心操作之一，确保最近访问的节点总是位于链表头部

```java
// 在链表头添加一个节点（把一本书放在最上面）
private void pushFront(Node x) {
    x.prev = dummy;
    x.next = dummy.next;
    x.prev.next = x;
    x.next.prev = x;
}
```










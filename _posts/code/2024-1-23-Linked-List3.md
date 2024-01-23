---
layout:     post
title:      "反转链表"
date:       2024-1-23
author:     "Mtz"
tags:
    - 递归
---

# [反转链表](https://leetcode.cn/problems/reverse-linked-list/)

**图解**

![likst1](https://telegraph-image-a8w.pages.dev/file/4d2d54455567f039fe35c.png)

* 递归

  ```java
    public ListNode reverseList(ListNode head) {
          if (head == null || head.next == null) return head;
  
          ListNode newhead = reverseList(head.next);
          head.next.next = head;
          head.next = null;
          return newhead;
      }
  ```

* 迭代

```java
   public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;

        ListNode newHead = null;
        while (head != null) {
            ListNode tmp = head.next;
            head.next = newHead;
            newHead = head;
            head = tmp;
        }
        return newHead;
    }
```


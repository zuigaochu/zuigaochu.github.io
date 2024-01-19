---
layout:     post
title:      "数组——重复元素的值"
date:       2024-1-19
author:     "Mtz"
catalog: true
tags:
    - 数据结构与算法
    - 数组
---

* Map实现

```java
package com.ma.Array;

import java.util.HashMap;
import java.util.Map;

public class ArrayDuplicateCounter {

    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 2, 5, 1, 6, 2, 4};

        Map<Integer, Integer> counterMap = countDuplicates(array);

        // 打印每个元素及其重复次数
        for (Map.Entry<Integer, Integer> entry : counterMap.entrySet()) {
            System.out.println("元素 " + entry.getKey() + " 重复次数: " + entry.getValue());
        }
    }

    public static Map<Integer, Integer> countDuplicates(int[] array) {
        Map<Integer, Integer> counterMap = new HashMap<>();

        // 遍历数组
        for (int num : array) {
            // 将元素添加到Map中，如果已存在则增加计数
            counterMap.put(num, counterMap.getOrDefault(num, 0) + 1);
        }

        return counterMap;
    }
}

```

* 数组实现

```java
import java.util.Arrays;

public class ArrayDuplicateCounter {

    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 2, 5, 1, 6, 2, 4};

        int[] duplicateCount = countDuplicates(array);

        // 打印每个元素及其重复次数
        for (int i = 0; i < duplicateCount.length; i++) {
            if (duplicateCount[i] > 0) {
                System.out.println("元素 " + i + " 重复次数: " + duplicateCount[i]);
            }
        }
    }

    public static int[] countDuplicates(int[] array) {
        int maxElement = Arrays.stream(array).max().orElse(0);
        int[] duplicateCount = new int[maxElement + 1];

        // 遍历数组
        for (int num : array) {
            // 增加元素对应的计数
            duplicateCount[num]++;
        }

        return duplicateCount;
    }
}

```


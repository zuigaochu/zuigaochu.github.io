---
layout:     post
title:      "List & Map 遍历"
date:       2024-1-4
author:     "Mtz"
catalog: true
tags:
    - Java
    - 集合
---

# List集合

## 准备条件

```java
ArrayList<String> list = new ArrayList<>();
list.add("1");
list.add("2");
list.add("3");
list.add("4");
```

## 遍历方法

###### Iteator遍历器

```java
Iterator<String> ite = list.iterator(); 
while(ite.hasNext())     
{
    String s = ite.next();
    System.out.println(s);
}
```

说明：

1. list.iterator()  ，创建一个迭代器。
2. hasNext( )方法，检测迭代指针后方是否还有元素
3. next( )方法，获取迭代地址后方值

###### 列表迭代器

```java
ListIterator<String> listite = list.listIterator();
while(listite.hasNext())   //正向遍历
{
    String s = listite.next();
    System.out.println(s);
}

while(listite.hasPrevious())  //反向遍历
{
    String s = listite.previous();
    System.out.println(s);
}
```

说明：

列表迭代器ListIterator功能更加强大：允许延任意方向遍历列表，在迭代器件也允许修改列表，并获取列表中迭代器的当前位置。



###### 普通for循环

```java
for (int i = 0; i < list.size(); i++) {
    System.out.println(list.get(i));    //get(i):返回第i号位置的元素
}
```

###### 增强for循环

```java
for (String s : list) {
    System.out.println(s);
}
```

###### 使用 Stream API 遍历 ArrayList

```java
list.stream().forEach(element -> {
    // 进行操作
    System.out.println(element);
});
```



# Map集合遍历

## 准备条件

```java
Map<Integer, Integer> map = new HashMap<Integer, Integer>();
map.put(1, 10);
map.put(2, 20);
map.put(3, 30);
map.put(4, 40);
```

## 遍历方法

###### 迭代器 EntrySet

```java
 Iterator<Map.Entry<Integer, String>> iterator = map.entrySet().iterator();
        while (iterator.hasNext()) {
            Map.Entry<Integer, String> entry = iterator.next();
            System.out.print(entry.getKey());
            System.out.print(entry.getValue());
        }
```

###### 迭代器KeySet

```java
  Iterator<Integer> iterator = map.keySet().iterator();
        while (iterator.hasNext()) {
            Integer key = iterator.next();
            System.out.print(key);
            System.out.print(map.get(key));
        }
```

###### ForEach EntrySet

```java
for (Map.Entry<Integer, String> entry : map.entrySet()) {
            System.out.print(entry.getKey());
            System.out.print(entry.getValue());
        }
```

###### ForEach KeySet

```java
  for (Integer key : map.keySet()) {
            System.out.print(key);
            System.out.print(map.get(key));
        }
```



###### 通过Java8 Lambda表达式遍历

```java
map.forEach((k, v) -> System.out.println("key: " + k + " value:" + v));

// 并行处理
   map.entrySet().parallelStream().forEach((entry) -> {
            System.out.print(entry.getKey() + ": " + entry.getValue() + " ");
        });
```


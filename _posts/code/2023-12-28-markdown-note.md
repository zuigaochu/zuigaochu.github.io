---
layout:     post
title:      "markdown语法快速入门"
date:       2023-12-28
author:     "Mtz"
catalog: true
tags:
    - markdown
---

# markdown快速入门

| 名称       | 语法                                | Typora快捷键     |
| ---------- | ----------------------------------- | ---------------- |
| 标题       | # 标题                              | ctrl + 1         |
| 粗体       | **  粗体   **                       | ctrl + B         |
| 高亮       | == 黄色高亮 ==                      |                  |
| 图片链接   | ![] ()                              | ctrl + k         |
| 删除线     |                                     | alt + shift + 5  |
| 引用       | '>'+ 空格                           | ctrl + shift + q |
| 代码块     | '     ```       '                   | ctrl + shift + k |
| 分割线     | '       ---        '                |                  |
| 斜线       | * *                                 | ctrl + i         |
| 下划线     |                                     | ctrl + u         |
| 有序列表   | 1.  + 空格                          |                  |
| 无序列表   | *  + 空格                           |                  |
| 插入表格   |                                     | ctrl  + t        |
| 插入公式快 |                                     | ctrl + shift + M |
| 上标       | a< sup>10 </sup>                    |                  |
| 下标       | a< sub>10 </sub>                    |                  |
| 目录       | [ TOC ]                             |                  |
| 任务列表   | - [x] 完成项目<br/>- [ ] 未完成项目 |                  |
| 内部链接   | [ ] ()                              |                  |

## front matter语法

```text
layout:     post
title:      "markdown语法快速入门"
subtitle:   " \"Hello World, Hello Blog\""
date:       2023-12-28
author:     "Mtz"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Meta
```

分析：

1. **`layout`：** 指定文章的布局方式。在很多静态网站生成器中，你可以定义不同的布局来呈现不同类型的页面。在这个例子中，`post` 布局可能用于表示这是一篇博客文章。
2. **`title`：** 文章的标题。
3. **`subtitle`：** 文章的副标题。在这个例子中，副标题被用双引号包裹，因为它包含空格。
4. **`date`：** 文章的发布日期和时间。
5. **`author`：** 文章的作者。
6. **`header-img`：** 文章的头图或封面图片的链接。在这里，通过 `header-img` 指定了一张网络上的图片。
7. **`catalog`：** 一个布尔值，可能表示是否将这篇文章包含在网站的目录或索引中。在这里，设为 `true` 可能表示这篇文章应该包含在目录中。
8. **`tags`：** 一个包含文章标签的数组。标签用于标识文章的主题或关键词。在这个例子中，文章被标记为 "Meta"。

[语法参考1](https://zhuanlan.zhihu.com/p/138627806)

[语法参考2](https://blog.csdn.net/kt1776133839/article/details/123213530)

[数学公式](https://blog.csdn.net/kt1776133839/article/details/123213530)

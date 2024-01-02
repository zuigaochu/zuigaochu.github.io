---
layout:     post
title:      "markdown中的html语法"
date:       2024-1-2
author:     "Mtz"
catalog: true
tags:
    - markdown
---



# 字体语法

这是一段<span style="color: red;">文本</span>

```text
这是一段<span style="color: red;">文本</span>
```

```text
颜色：yellow  , purple
```

# 音乐

代码实现

```text
<audio controls>
  <source src="路径" type="audio/mp3">
</audio>
```

效果展示（网站链接）

<audio controls>
  <source src="https://freetyst.nf.migu.cn/public/product10th/productB36/2021/11/0316/2009%E5%B9%B412%E6%9C%8808%E6%97%A5%E6%B5%B7%E8%9D%B6%E5%94%B1%E7%89%87/%E6%A0%87%E6%B8%85%E9%AB%98%E6%B8%85/MP3_320_16_Stero/60058622727160700.mp3?channelid=02&msisdn=80fe7012-7ddf-42f3-b986-486a4c08df96&Tim=1703390484346&Key=724bb3795ea5dc99" type="audio/mp3">
  music
</audio>

代码实现

```text
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1939760259&auto=1&height=66"></iframe>
```

效果实现(网易云源)

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1939760259&auto=1&height=66"></iframe>

效果实现（github）

<audio controls>
  <source src="music/最伟大的作品-周杰伦.mp3" type="audio/mp3">
</audio>

# 视频

代码实现

```text
<video width="320" height="240" controls>
  <source src="video/2024-01-02 15-57-11.mp4" type="video/mp4">
cat
</video>
```

效果展示（github）

<video width="320" height="240" controls>
  <source src="video/2024-01-02 15-57-11.mp4" type="video/mp4">
cat
</video>


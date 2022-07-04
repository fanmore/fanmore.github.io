---
title: HTML常用知识
categories: Ⅱ前端
sort: 1
tags:
  - 前端
  - HTML
date: 2022-06-30 13:09:56
updated: 2022-06-30 13:10:00
---

# 头部icon

```html
<link rel="shortcut icon" href="图片url">
```

# http转https

网页中的链接需要都转为https请求，并且数据是动态的情况处理方式：

```html
<!--http转https-->
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">
```

<!-- more -->

# a标签新窗口打开

|值|说明|
|-|-|
|_blank|新窗口打开|
|_self|相同窗口，默认|
|_top|在整个窗口中打开|
|framename|指定框架中打开|

# textarea禁止拖拽

```
<textarea style="resize:none;"></textarea>
```

# 页面文字禁止选中

```html
<div onselectstart="return false">

</div>
```

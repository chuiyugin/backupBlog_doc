---
title: C++库函数积累应用
tags:
  - 库函数
categories:
  - C++
date: 2025-5-13 15:00:00
excerpt: Some useful C++ function!
---
# C++库函数积累应用
## 字符串 (string) 相关
+ 调整字符串数组长度的函数 `string::resize()`：
	+ 将字符串大小调整为 `n` 个字符的长度。
	+ 如果 `n` 小于当前字符串长度，则当前值将缩短为其第一个 `n` 个字符，删除超过 `n` 个字符的字符。
	+ 如果 `n` 大于当前字符串长度，则通过在末尾插入所需数量的字符来扩展当前内容，以达到 `n` 的大小。如果指定了 `char c`，则新元素初始化为 `char c` 的副本，否则，它们是值初始化字符（空字符）。

![string::resize()](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250513151409.png)


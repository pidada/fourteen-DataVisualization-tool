---
title: D3.js库-3-深入理解update、enter、exit
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-05-12 00:56:47
password:
summary:
tags:
  - update
  - enter
  - exit
categories:
  - datavisual
  - D3.js
---

### 三者作用



`Update、Enter、Exit`是`D3.js`中十分重要且关键的3个概念。它们三主要处理的是`数据集个数`和`选择集个数`之间的匹配问题。

<!--MORE-->

### 图解三者关系

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1geozic9rodj311e0ku11b.jpg)

上图的解释：

1. 绿色：如果给定的数据`data`和节点`Nodes`中的数据相等，则进行`update`操作
2. 蓝色：如果数组中个数多余节点中的元素个数，进行`update`和`enter`操作
3. 橙色：如果给定的数据中个数不足，则`update`和`exit`操作

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1geozp9y33tj30yk0hugn5.jpg)



### 代码解释

#### update

1. 给定的数组中的个数和`DOM`中的个数相等，则进行`update`操作，变成了红色，更新数据。
2. **没有进行enter()方法**中变成绿色的操作



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1geoytolkanj31200l4jt4.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1geoyummj3xj311p0m9gpq.jpg)

#### update和enter

1. 给定的元素个数是`2`，多余`DOM`的元素个数

2. 同时执行`update+enter`两个操作

   - 红色：`update`

   - 绿色：`enter`

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1geoytqqtsrj312o0l2abw.jpg)



#### update、exit

1. 给定的数组中元素个数小于DOM中的个数（2个）
2. 同时实行update+exit操作
   - 红色：update
   - 蓝色：exit

$\color{red}{exit部分通常执行的是remove操作，直接删除掉}$

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1geoywo7nbwj31280mfac2.jpg)

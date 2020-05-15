---
title: D3.js库-4-选择、插入和删除元素
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-05-15 07:16:07
password:
summary:
tags:
  - insert
  - append
  - remove
categories:
  - datavisual
  - D3.js
---


### D3.js库-4-选择、删除、插入元素

本文中介绍的是如何在D3.js库中**选择、插入和删除元素**

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gesebssbagj31qd0u0n2y.jpg)

<!--MORE-->

### 选择元素

在之前的文章[D3.js库-2-选择元素和绑定数据](https://www.jianshu.com/p/83afe172866b)中，有介绍过D3.js中的两种选择数据的方法，本部分为重复内容，温故而知新：

- d3.select()：选择所有指定元素的第一个
- d3.selectAll()：选择指定元素的全部

两个函数的返回集都称之为选择集，下面是常见的用法：

```javascript
const body = d3.select("body");  //选择文档中的body元素
const p1 = body.select("p");      //选择body中的第一个p元素
const p = body.selectAll("p");    //选择body中的所有p元素
const svg = body.select("svg");   //选择body中的svg元素
const rects = svg.selectAll("rect");  //选择svg中所有的rect元素
```

现在假设某个`<body>`标签中有4个`<p>`标签：

```javascript
<p>dog</p>
<p>cat</p>
<p>pig</p>
<p>rat</p>
```

- 选择第一个元素：

```javascript
const p = d3.select("body")   // 先选择body标签
						.select("p")   // 选择第一个p标签
						.style("color","red");   // 为选定的p标签设置红色
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gesf18sfycj31ae0igwjp.jpg)

注意：上面使用的链式语法

- 选择全部元素：

```javascript
const p = d3.select("body")   // 先选择body标签
						.selectAll("p")   // 选择第一个p标签
						.style("color","blue");   // 为选定的p标签设置蓝色
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gesf2v0iopj31be0pajyg.jpg)

- 在指定元素的属性之后，通过指定元素的属性来进行选择：
  - 访问class属性的元素加**点.**
  - 访问id属性的元素加井**号#**

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gesfb20tedj319z0u0tih.jpg)

### 删除元素

D3.js中的删除元素是通过**remove()**来实现的：

![image-20200515000953244](https://tva1.sinaimg.cn/large/007S8ZIlgy1gesfimlvcdj31880hkafd.jpg)





### 插入元素

D3.js中涉及到两种插入函数

- append()：在选择集尾部插入元素

- insert()：在指定选择集前面插入元素

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gesg2pkh48j321m0tiqbx.jpg)

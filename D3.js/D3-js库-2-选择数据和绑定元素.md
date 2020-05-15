---
title: D3.js库-2-选择数据和绑定元素
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-05-11 10:39:38
password:
summary:
tags:
  - D3
  - 选择集
categories:
  - datavisual
  - D3.js
---

### D3.js库-2-选择元素和绑定数据

选择元素和绑定数据可以说是后续进行`D3`库操作的基础，所以需要掌握其基本操作

- 选择集
  - select()
  - selectAll()
- 绑定元素
  - datum()：单个元素
  - data()：多个元素

<!--MORE-->

### 如何选择元素

在D3中，有两种方式来选择元素：

- d3.select()：选择所有指定元素的**第一个**
- d3.selectAll()：选择指定元素的**全部**

两个函数的返回集都称之为**选择集**，下面是常见的用法：

```javascript
const body = d3.select("body");  //选择文档中的body元素
const p1 = body.select("p");      //选择body中的第一个p元素
const p = body.selectAll("p");    //选择body中的所有p元素
const svg = body.select("svg");   //选择body中的svg元素
const rects = svg.selectAll("rect");  //选择svg中所有的svg元素
```

**选择集和绑定数据通常是一起使用的**



### 如何绑定数据

`D3.js`能够将数据绑定到`DOM`上面，**也就是绑定到文档上**。

例如：如果网页中有一个数字2和元素X，D3.js库就可以将它们绑定在一起。绑定数据的两个函数为：

- data()：将一个数组绑定到选择集上，采用的是一一对应的关系，$\color{red}{常用函数}$
- datum()：将一个元素绑定到所有选择集上，$\color{red}{用的少}$

#### datum使用

##### 示例

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1geogfcnffcj30yn0kpju2.jpg)


运行的结果是

- 第0个元素is an animal

- 第1个元素is an animal

- 第2个元素is an animal

  

##### 代码解释

1. datum方法将str字符串绑定在3个p选择集上
2. 通过无名函数funtion(d,i)，访问到绑定的元素：
   1. d代表数据，也就是和某个元素绑定的数据
   2. i代表索引，从0开始

#### data使用

##### 示例

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1geobbp88o4j30xo0jmtaq.jpg)

##### 代码解释

1. 多个元素绑定使用data方法
2. 无名函数的使用

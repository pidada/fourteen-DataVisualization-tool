---
title: D3.js库-1-入门篇
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-05-04 10:27:24
password:
summary:
tags:
  - visualization
  - D3.js
categories:
  - 可视化
  - D3.js
---


从今天开始可视化库$\color{red}{D3.js}$的第一章-入门篇咯😊

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1geg7h89kgkj31200gq40d.jpg)

<!--MORE-->

### 什么是D3.js

`D3`指的是`Data-Driven Documents`，`js`即`Javascript`，是后缀名。先看看官网上对`D3.js`库的定义：

> D3.js is a JavaScript library for manipulating documents based on data.D3 helps you bring data to life using HTML, SVG, and CSS.
>
> D3’s emphasis on web standards gives you the full capabilities of modern browsers without tying yourself to a proprietary framework, combining powerful visualization components and a data-driven approach to DOM manipulation.

翻译成中文大致意思为：

**D3.js 是基于数据驱动文档工作方式的一款JavaScript函数库，主要用于网页作图、生成互动图形，是最流行的可视化库之一。D3使你有能力借助HTML，SVG和CSS来生动地可视化各种数据****

**D3不需要你使用某个特定的框架，它的重点在于对现代主流浏览器的兼容，同时结合了强大的可视化组件，以数据驱动的方式去操作DOM**

通过上述的表达，总结**D3.js**库的几大特点：

1. 一款基于JavaScript的函数库
2. 借助HTML、SVG和CSS等实现可视化
3. 组件强大，通过数据驱动的方式来操作DOM

### 安装

1. 下载最新的版本V5.16.0。解压后，在HTML文件中包含相关的`js`文件即可

[D3.js]https://github.com/d3/d3/releases/download/v5.16.0/d3.zip

2. 通过采用<script>标签实现，在页面的代码中插入如下代码

```html
<script src="http://d3js.org/d3.v5.min.js"></script>
```

注意：现在已经是V5版本。V5和V3的很多语法还是有区别的，后期所有的文档都是基于V5.

### 预备知识

如果想通过`D3`来实现数据的可视化，需要的预备知识：

- HTML：超文本标记语言，用于设定网页的内容
- CSS：层叠样式表，用于设定网页的样式
- JavaScript：流行的前端语言，用于设定网页的行为
- DOM：文档对象模型，用于修改文档的内容和结果
- SVG：可缩放矢量图形，用于绘制可视化的图形

以上知识点没有必要掌握的非常精通，建议到[W3school](https://www.w3school.com.cn/)快速入门，了解基本概念，再看几个案例`demo`，以后遇到不懂的地方可以进行查看。

### 编程环境

D3.js是在网页上的可视化制图，常用的网页制作工具：

1. IDE的选择：VS code、Sublime Text、Notepad++等，推荐使用VS code

2. 浏览器：D3支持的主流浏览器不包括IE8及以前的版本。D3测试了Firefox、Chrome、Safari、Opera和IE9。D3的大部分组件可以在旧的浏览器运行。

   Chrome是最好的选择。强大的调试功能会让你事半功倍！推荐浏览使用chrome的另一个好处是查找资料更多更全面。

3. 服务器软件：Apache、Tomcat等，这个是非必须的。但是有些函数需要放置于服务器目录下，才能正常运行，比如关于导入json文件的函数



### 学习网站

以下是几个学习网页制作和D3的网站：

1. #### W3school

[W3school](https://www.w3school.com.cn/)，非常全面的网站建设课程，从基础的 HTML 到 CSS，乃至进阶的 XML、SQL、JS、PHP 等

2. #### HTML+CSS快速入门

[初识HTML(5)+CSS(3)-2020升级版](https://www.imooc.com/learn/9)

3. #### SVG

可缩放矢量图形，即[SVG](https://developer.mozilla.org/zh-CN/SVG)，是W3C XML的分支语言之一，用于标记可缩放的矢量图形

[SVG-菜鸟课程](https://www.runoob.com/svg/svg-tutorial.html)

[SVG|MDN](https://developer.mozilla.org/zh-CN/docs/Web/SVG/Tutorial)

[SVG图像入门教程|阮一峰老师](https://www.ruanyifeng.com/blog/2018/08/svg.html)

4. #### D3.js

**第一个推荐的网站肯定是[D3官网](https://d3js.org/)**，包含很多的示例和API文档，都是根据最新的版本发布的。还有几个不错的网站👇

[D3.js的V5版本入门教程](https://blog.csdn.net/qq_34414916/category_7608878.html)

[慕课网-使用D3制作图表](https://www.imooc.com/learn/103)

[数据可视化编程-使用D3.js](https://www.bilibili.com/video/BV1HK411L72d?from=search&seid=15321788357883060603)

[Data Visualization with D3.js - Full Tutorial Course](https://www.youtube.com/watch?v=_8V5o2UHG0E&t=11927s)，油管上的一个实例演示课程，需要科学上网才能观看。



### 第一个D3.js的程序

![image-20200504100247944](https://tva1.sinaimg.cn/large/007S8ZIlgy1geg6u5n5nvj31os0u0ajw.jpg)

代码解释：

1. 在`body`标签中放入两个p标签，没有写入内容
2. 定义变量p，通过链式调用获取到全部的p元素，即`selectAll()`方法
3. 通过`text()`方法来写入内容，进行输出



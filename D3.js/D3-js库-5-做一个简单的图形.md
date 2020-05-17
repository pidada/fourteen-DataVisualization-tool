---
title: D3.js库-5-做一个简单的图形
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-05-15 11:36:33
password:
summary:
tags:
  - rect
categories:
  - datavisual
  - D3.js
---

### D3.js库-5-做一个简单的图形

本文中介绍利用一组简单的数据制作一个条形图，先看效果：

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gesxnd1kzuj308v04h741.jpg)

<!--MORE-->

### 画布

在`HTML`中使用的画布有两种：`SVG`和`Canvas`，在`D3`中使用的是`SVG`。

#### SVG

**SVG，指可缩放矢量图形（Scalable Vector Graphics），是用于描述二维矢量图形的一种图形格式，是由万维网联盟制定的开放标准**。 SVG 使用 XML 格式来定义图形。SVG的几个特点

- SVG绘制的是矢量图，对图像进行放大后不会失真

- 基于XML，可以为每个元素添加JS事件的处理

- 每个图形是对象，更改对象的属性，图形也会改变

#### Canvas

**Canvas 是通过 JavaScript 来绘制 2D 图形，是 HTML 5 中新增的元素**，Canvas的几个特点

1. 绘制的是位图，放大后图形会失真

2. 不支持JS事件处理器

3. 能够以`.png`或者`.jpg`格式进行保存图像

#### 添加画布

有了画布才能在其上面作图。使用D3在body元素中**添加svg画布**的代码如下：

$\color{red}{此段代码常用，须记住}$

```javascript
// D3中定义画布svg，设置宽高
        const width = 300;
        const height = 300;

        const svg = d3.select("body")  // 选择body元素
                    .append("svg")   // 添加svg
                    .attr("width", width)  // 设置SVG的宽高
                    .attr("height", height)
```

### 绘制矩形

#### rect

在SVG中，矩形的元素标签是rect。**圆形的元素标签是circle**

```html
<svg>
  <rect></rect>
  <rect></rect>
  <rect></rect>
</svg>
```

rect的四个属性：

- x：矩形左上角x坐标

- y：矩形左上角的y坐标

- width：宽度

- height：高度

需要注意的：在SVG中，**x轴的正方向是水平向右，y轴的正方向是垂直向下的**

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gesxxeea08j312a0mxacm.jpg)

#### 代码解释

当我们定义了数组和画布之后，需要添加和数组长度相同的矩形元素

```javascript
svg.selectAll("rect")  // 绘制矩形使用rect标签
            .data(dataset)   // 绑定数据dataset
            .enter()     // 指定数据集中的enter部分
            .append("rect")   // 添加足够的元素。有数据但是没有图形元素的时候，使用append()进行追加
```

定义完每个矩形元素之后，使用无名函数对其进行属性的赋值，主要是上面👆提到的4个属性。在使用`attr`属性的时候，颜色对应的`fill`。

```javascript
.attr("x", 50)  // 定义左上角的坐标x
.attr("y", function(d,i){  // 定义左上角的坐标y：d是作用的数据，i是索引号
  return i * rectHeight;
})
.attr("width", function(d){   // 设置宽度
  return d;
})
.attr("height", rectHeight-2)  // 设置长度
.attr("fill", "blue");   // 设置属性颜色
```

### 完整代码

下面附上完整的HTML代码，保存之后通过浏览器打开即可看到效果。

```javascript
<!DOCTYPE html>
<html>
<head>
    <title>D3.js tutorial</title>
    <script src="https://d3js.org/d3.v5.min.js"></script>
</head>
<body>

    <script>

        // 定义数据：表示矩形的宽度
         const dataset = [80, 160, 20, 100, 300]
        // 定义画布的边距
        const merge = {top:60, bottom:60, left:60,right:60}


        // D3中定义画布svg，设置宽高
        const width = 300;
        const height = 300;

        var svg = d3.select("body")  // 选择body元素
                    .append("svg")   // 添加svg
                    .attr("width", width)  // 设置SVG的宽高
                    .attr("height", height)

        // 绘图：矩形的4个属性：x/y/width/height
        // 定义每个矩形占据的像素高度
        const rectHeight = 25;

        svg.selectAll("rect")  // 绘制矩形使用rect标签
            .data(dataset)   // 绑定数据dataset
            .enter()     // 指定数据集中的enter部分
            .append("rect")   // 添加足够的元素。有数据但是没有图形元素的时候，使用append()进行追加
                .attr("x", 50)  // 定义左上角的坐标x
                .attr("y", function(d,i){  // 定义左上角的坐标y：d是作用的数据，i是索引号
                    return i * rectHeight;
                })
                .attr("width", function(d){   // 设置宽度
                    return d;
                })
                .attr("height", rectHeight-2)  // 设置长度
                .attr("fill", "blue");   // 设置属性颜色
    </script>
</body>
</html>
```



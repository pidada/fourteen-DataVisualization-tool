---
title: D3.js库-7-坐标轴的使用
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-05-18 18:57:49
password:
summary:
tags:
  - 坐标轴
categories:
  - datavisual
  - D3.js
---


### D3.js库-7-添加坐标轴

### 坐标轴

坐标轴是可视化图表中经常出现的一种图形，由一些刻度和线列段组成。D3中是没有现成的坐标轴，SVG中因而没有现成的图形元素，需要通过D3提供的其他组件来手动添加。下图是添加了坐标轴之后的效果图。



![](https://i.loli.net/2020/05/18/SakHU9FAvPxWG3M.png)

![坐标轴+刻度尺](https://i.loli.net/2020/05/18/rtXwy2dZEu3n1x7.png)

<!--MORE-->

### 坐标轴构成

在SVG画布的预定义元素中，有6种基本的图形：

- 矩形
- 圆形
- 椭圆
- 线段
- 折线
- 多边形

还有一种特殊的元素就是：**路径path**

几乎画布中的所有图形都是由以上7种元素构成的。上述元素中没有坐标轴，采用类似的定义方式：将axis看做是一个标签，x1、x2等看做是它的一个个属性

```html
<axis x1="" x2="">...</axis>
```

### 分组元素g

由于没有上述的元素，需要其他的元素来组合构成类似的形式：

```html
<g>
    <!--第一个刻度-->
<g>
    <line></line>   <!--第一个刻度的直线-->
    <text></text>   <!--第一个刻度的文字-->
</g>
<g>
    <line></line>   <!--第二个刻度的直线-->
    <text></text>   <!--第二个刻度的文字-->
</g>
...
    <!--坐标轴的轴线-->
    <path></path>
</g>
```

分组元素是SVG画布中的元素，意思就是group，是将其他元素进行组合和分组存放。D3中提供了一个组件能够自动添加：`d3.svg.axis()`

每个分组g看做是一个刻度值和线段组成的group。

### 定义一个坐标轴

定义一个坐标轴需要使用上一篇文章中使用的比例尺。它们二者经常是一起使用的。

```javascript
// 定义数据
const dateset = [80,160,20,100,300]
// 定义一个线性比例尺
const scaleLinear = d3.scaleLinear()
						.domain([0, d3.max(dataset)])  // 映射区间
						.range([0,250]);

const axis = d3.axisBottom(scaleLinear)  // 定义向下的坐标轴
				.ticks(7);  // 坐标轴上的刻度数

g.append("g")  // 追加足够多的g元素
	.attr("transform","translate(" + 30 + (dataset.length * rectHeight) + ")")  // 设置位置信息
	.call(axis)   // 定义的比例尺本身就是函数，需要进行回调
```

### 柱状图加上坐标轴

下面是完整的代码

```javascript
<!DOCTYPE html>
<html>

  <head>
    <title>添加坐标轴</title>
    <meta charset="utf-8">
    <script type="text/javascript" src="http://d3js.org/d3.v5.min.js">
	</script>
  </head>

  <body>
    <svg width="960" height="600"></svg>
    <script>

        // 定义画布的边框和给定数据
        var marge = {top:60, bottom:60, left:60, right:60};
        var dataset = [80, 160, 20, 100, 300];

        // 定义一个线性比例尺
        var scaleLinear = d3.scaleLinear()
                            .domain([0,d3.max(dataset)])
                            .range([0,300]);

        // 定义每个分组元素
        var svg = d3.select("svg");
        var g = svg.append("g")
                    .attr("transform", "translate(" + marge.top + "," + marge.left + ")");

        // 定义每个矩形的像素
        var rectHeight = 25;


        // 每个g元素的属性进行设置
        g.selectAll("rect")   // 选择所有的矩形元素并绑定数据
            .data(dataset)
            .enter()
            .append("rect")   // 追加元素
            .attr("x", 20)
            .attr("y", function(d, i){
                return i * rectHeight;
            })
            .attr("width",function(d){
                return scaleLinear(d);
            })
            .attr("height", rectHeight - 5)
            .attr("fill", "blue");

        // 为坐标轴定义一个线性比例尺
        var xScale = d3.scaleLinear()
                        .domain([0,d3.max(dataset)])  // 定义范围
                        .range([0, 300]);

        // 定义一个坐标轴
        var xAxis = d3.axisBottom(xScale)   // 向下的坐标轴
                        .ticks(7);  // 刻度数为7

        g.append("g")
            .attr("transform","translate(" + 20 + "," +(dataset.length * rectHeight) + ")")  // 设置位置信息
            .call(xAxis);   // 将定义的坐标轴进行回调
    </script>
  </body>
</html>
```



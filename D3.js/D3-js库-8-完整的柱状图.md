---
title: D3.js库-8-完整的柱状图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-05-22 00:55:09
password:
summary:
tags:
  - bar
  - xScale
  - d3
categories:
  - datavisual
  - D3.js
---

### 制作一个完整的柱状图

一个完整的柱状图应该是包含坐标轴、文字、矩形和标题等。在本篇文章中将从**数据定义、定义画布和边框、坐标轴和比例尺的定义、矩形元素的属性设置、字体的大小**等各个方面进行讲解。

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf0js664aoj32is0u0n3m.jpg)



<!--MORE-->

### 定义数据集

首先，定义我们将要用于描绘的数据集合。每个数据由name和value组成

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf0hi3egwej30t60geq6i.jpg)



### 定义画布SVG

画布的定义需要从svg元素中提取出来`d3.select("#mainsvg")`

然后再定义其宽和高，注意两种定义的方法：一种是利用+号将字符串转成数值型，一种是直接赋值

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf0hn71uahj313i0ge42q.jpg)

### 定义margin

定义margin的时候需要指定4个属性：top、bottom、left、right。

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf0jl6u06qj31de0feaeo.jpg)

### 定义两种比例尺

**横轴是线性比例尺；纵轴是离散型的比例尺**。注意两种比例尺的映射范围

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf0jmtor8aj31ai0jewjq.jpg)

### 定义分组元素g

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf0itwx1b3j317o0fedj4.jpg)

### 定义两个坐标轴

坐标轴定义的时候需要将比例尺传进来。一个是向左，一个向下

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf0ixh4ubsj318m0ie0xg.jpg)



### 设置矩形元素的属性

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf0jpewr0yj30tm0iewij.jpg)



### 改变字体和设置标题

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf0j1gsds5j319u0jejw1.jpg)

### 源码

```javascript
<!DOCTYPE html>
<html>
  <head>
    <title>BarChart</title>
    <script type="text/javascript" src="http://d3js.org/d3.v5.min.js">
	</script>


  </head>

  <body>
    <svg width="1600" height="800" id="mainsvg" class="svgs"></svg>
    <script>
      // 1. 定义数据
      const data = [{name:"zhangsan",value:6}
                    ,{name:"lisi",value:17}
                    ,{name:"haozheng",value:27}
                    ,{name:"pidada",value:34}
                    ,{name:"zhouzheng",value:10}
                    ,{name:"peter",value:22}
                  ]

      // 2. 定义svg及其宽高
      const svg = d3.select("#mainsvg");  // 取出svg的id号
      const width = +svg.attr("width");  // + 表示将字符串转成数值
      const height = 800;  // 比较两种定义方法

      // 3. 定义margin：4个属性
      // SVG指的是整个作画的空间，定义了margin之后，将作画空间控制在svg-margin之内
      const margin = {top:60, right:30, left:100, bottom:60};
      const innerWidth = width - margin.left - margin.right;  // 整个画布减去margin的左右两边
      const innerHeight = height - margin.top - margin.bottom;  // 整个画布的高减去mrgin的上下

      // 4. 定义两种比例尺：线性和离散型
      const xScale = d3.scaleLinear()
                        .domain([0,d3.max(data, d => d.value)])  // x轴上的取值是value
                        .range([0,innerWidth]);

      const yScale = d3.scaleBand()
                        .domain(data.map(d => d.name))   // y轴上的取值是名字name
                        .range([0,innerHeight])
                        .padding(0.1);  // 指定每个矩形的间隔

      // 5. 定义分组元素g
      const g = svg.append("g")
                    .attr("id","maingroup")
                    .attr("transform",`translate(${margin.left},${margin.top})`);

      // 6. 定义两个坐标轴，并且利用元素g进行回调
      const yAxis = d3.axisLeft(yScale)
                        .tickSize(-innerWidth);
      g.append("g").call(yAxis);   // 通过分组元素g进行回调

      const xAxis = d3.axisBottom(xScale);
      g.append("g").call(xAxis).attr("transform",`translate(0,${innerHeight})`);

      // 7. 每个矩形元素进行属性设置
      data.forEach(d => {
            g.append("rect")
              .attr("width",xScale(d.value))
              .attr("height",yScale.bandwidth())
              .attr("fill","green")
              .attr("opacity",0.8)  // 透明度
              .attr("y",yScale(d.name));
      })

      // 8. 改变y轴上的字体大小
      d3.selectAll(".tick text").attr("font-size","1.5em");

      // 9. 标题设置
      g.append("text").text("bar-Chart")
              .attr("font-size","1.5em")
              .attr("transform",`translate(${innerWidth / 2}, 0)`)  // 字体最左边居中
              .attr("text-anchor","middle");  // 字体居中
    </script>
  </body>
</html>
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf0jo5mrtgj30u01esnmy.jpg)

### 效果图

![image-20200522005415359](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf0k4ywkzhj31nl0u0q54.jpg)

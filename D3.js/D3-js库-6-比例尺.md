---
title: D3.js库-6-比例尺
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-05-16 00:42:50
password:
summary:
tags:
  - 比例尺
categories:
  - datavisual
  - D3.js
---

### D3.js库-6-比例尺的使用

比例尺在D3中是一个非常实用的工具，可以这样理解比例尺：$\color{red}{一种一一映射}$的关系，从`domain`映射到`range`。因为在建立比例尺的过程中会经常使用到两个函数：**domain()和range()**。本文中介绍两种常用的比例尺

- 线性比例尺`scaleLinear`

- 序数比例尺`scaleOrdinal`

<!--MORE-->

### 线性比例尺scaleLinear

在线性比例尺中，`domain`和`range`都是连续变化的。**关系类似于线性函数**



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1getkgyarfsj31zo0py13v.jpg)



代码解释：

```javascript
var scaleLinear = d3.scaleLinear()
                    .domain([min,max])
                    .range([0,300]);
```

表示将数据从[0.9,5]映射到了[0,300]之间，**定义的变量scaleLinear好比是一个函数**，能够直接传入参数进行计算

注意在D3中如何进行**换行操作**



### 序数比例尺scaleOrdinal()

`domain`和`range`都是**离散化**的，可以说都是数组的形式，**不是连续的**

同样的，在定义了比例尺之后，可以当做函数来使用，传入参数



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1getkkyn1loj32440kwztl.jpg)

```javascript
<script>
      // 离散化比例尺
      var index = [0,1,2,3,4];
    	var color = ["red","blue","yellow","black","green"];
    	var scaleOrdinal = d3.scaleOrdinal()
    		.domain(index)   //将index中的值和color中的颜色一一对应
    		.range(color);

    	document.write("scaleOrdinal(1)输出："+scaleOrdinal(1));
    	d3.select("body").append("br");//换行
    	document.write("scaleOrdinal(2)输出："+scaleOrdinal(2));
    	d3.select("body").append("br");
    	document.write("scaleOrdinal(4)输出："+scaleOrdinal(4));
</script>
```



### 比例尺作图

#### 利用线性比例尺来做柱状图



```javascript
<body>
    <svg width="960" height="600"></svg>
    <script>
        // 定义画布边距、数组
    	var marge = {top:60,bottom:60,left:60,right:60}
    	var dataset = [ 2.5 , 2.1 , 1.7 , 1.3 , 0.9 ];
    	//定义一个线性比例尺
    	var scaleLinear = d3.scaleLinear()
    		.domain([0,d3.max(dataset)])   //指定domain和range
    		.range([0,300]);

    	var svg = d3.select("svg");   // 选择元素
    	var g = svg.append("g")   // 指定分组
    		.attr("transform", "translate("+marge.top+","+marge.left+")");

    	var rectHeight = 30;

        g.selectAll("rect")
            .data(dataset)
            .enter()
            .append("rect")
                .attr("x", 20)
                .attr("y",function(d,i){
                    return i * rectHeight;
                })
                .attr("width",function(d){
                    return scaleLinear(d);   // 使用比例尺
                })
                .attr("height", rectHeight-5)
                .attr("fill","red")
    </script>
</body>
```
![](https://tva1.sinaimg.cn/large/007S8ZIlgy1geu1y90szoj30k20asdft.jpg)

#### 利用比例尺加上刻度线来作图

```javascript
<body>
    <script>
        // 定义画布大小和数组
        var dataArray = [5, 40, 50, 60];
        var width = 500;
        var height = 500;

        //定义线性比例尺
        var widthScale = d3.scaleLinear()  // D3 v4之后不再使用该写法，请用d3.scaleLinear()
                            .domain([0,60])
                            .range([0,width]);
        // 定义颜色的渐变比例尺
        var color = d3.scaleLinear()
                            .domain([0,60])
                            .range(['red', 'blue']);

        var axis = d3.axisBottom(widthScale)  // 定义一个axis，方向朝下，10个刻度
                            .ticks(10);

        // 定义画布（代码记住）
        var svg = d3.select('body')
                    .append('svg')  // 追加一个svg元素
                        .attr('width', width)
                        .attr('height', height)
                    .append('g')
                        .attr('transform', 'translate(20,0)')
                        .call(axis);   // 最终调用axis刻度


        var bars = svg.selectAll('rect')   // 选择svg中的全部矩形
                    .data(dataArray)   // 绑定数组
                    .enter()  // 指定选择集的enter部分
                    .append('rect')  // 添加足够数量的矩形元素
                        .attr('x', 20)   // 距离原点的距离，默认是0
                        .attr('width',function(d) {return widthScale(d);})
                        .attr('height',50)
                        .attr('fill',function(d) { return color(d); })  // 填充颜色改变
                        .attr('y',function(d,i){ return i * 100 });  // d表示被绑定的数据，i是索引号

    svg.append('g')
            .attr("transform", "translate(0,400)")
            .call(axis);

    </script>
</body>
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1getlwm8qi8j30xm0ouwf6.jpg)

---
title: plotly-express-8-plotly实现散点图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-06-28 11:41:12
password:
summary:
tags:
  - px
  - 可视化
categories:
  - datavisual
  - px
---

本文中介绍的是利用`plotly_express`绘制散点图，使用的是`scatter()`方法。

> With `px.scatter`, each data point is represented as a marker point, whose location is given by the `x` and `y` columns.

- 通过`plotly_express`库来实现
- 通过`plotly.graph_objects`实现

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7uqfwvx1j30h60domyu.jpg)

<!--MORE-->

### 1 基于px的散点图

#### 1.1 模拟数据

直接将数据传进来

```python
import plotly_express as px
import pandas as pd
import numpy as np

px.scatter(x=[1,2,6,7,9,8,3,4,5],y=[2,14,12,24,36,8,25,7,18])
```



![image-20200628114827306](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7uz0dotij31em0pgdgz.jpg)

#### 1.2 内置数据gapminder

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7uzwkeg0j31hy0me42y.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7v0b8croj31gy0qe0vl.jpg)

#### 1.3 内置数据iris

```python
df = px.data.iris()
fig = px.scatter(
  df,
  x="sepal_width",
  y="sepal_length", # 绘图的数据及xy轴
  color="species",  # 点的颜色
  size='petal_length',   # 点的大小
  hover_data=['petal_width']  # 悬停显示的数据
)

fig.show()
```

![image-20200628115132933](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7v27dsm1j31fu0u0gvd.jpg)

#### 1.4 连续型的点图line-scatter

连续型的点图，比如：三角函数的图形、线性图形等

```python
x = np.linspace(0,10,100)   # 0-10的100个数
y = np.sin(x)
px.line(x=x,y=y,labels={"x":"t","y":"sin(t)"})
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7v3b7b3gj31m40u0n1f.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7v42qep4j31n30u0421.jpg)





### 2 基于go.Scatter的散点图

#### 2.1 demo

- `go.Figure`确定画布
- `go.Scatter`画图，传入需要的数据

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7v4qzn7xj31a80u0782.jpg)

```python
t = np.linspace(0, 10, 50)
y = np.sin(t)
fig = go.Figure(data=go.Scatter(x=t, y=y, mode="markers"))
fig.show()
```

![image-20200628134731146](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7yewurp6j31d40lsdgy.jpg)

#### 2.2 子图制作

在一个画布`figure`中画多个图

- `go.figure`确定画布
- `go.add_trace()`：将不同的图形画在一个画布上
- `fig.show()`：显示图形

```python
np.random.seed(2)

N = 100
random_x = np.linspace(0, 1, N)

random_y0 = np.random.randn(N) + 8
random_y1 = np.random.randn(N)
random_y2 = np.random.randn(N) - 8
random_y3 = np.random.randn(N) - 4

fig = go.Figure()

#  add traces
fig.add_trace(go.Scatter(x=random_x,y=random_y0,
                         mode="markers",name="markers"))
fig.add_trace(go.Scatter(x=random_x, y=random_y1,
                         mode='lines+markers',name='lines+markers'))
fig.add_trace(go.Scatter(x=random_x, y=random_y2,
                         mode='lines',name='lines'))
fig.add_trace(go.Scatter(x=random_x, y=random_y3,
                         mode='markers',name='markers'))
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7yfw9hzsj31fq0octdj.jpg)

#### 2.3 冒泡散点-bubble scatter

冒泡散点图：随着坐标轴数值的变化，点的大小随着变化

```python
fig = go.Figure(go.Scatter(
  x=np.linspace(0,50,10),
  y=np.random.randint(0,50,10),
  mode="markers",
  marker=dict(size=np.random.randint(0,50,10),  # 通过字典的形式来实现
              color=np.random.randint(50,100,10))
                          ))
fig.show()
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7yh04o2ij31d00mmjso.jpg)

```python
t = np.linspace(0, 10, 100)

fig = go.Figure()

fig.add_trace(go.Scatter(
    x=t, y=np.sin(t),
    name='sin',
    mode='markers',
    marker_color='rgba(20, 180, 60, .8)'
))

fig.add_trace(go.Scatter(
    x=t, y=np.cos(t),
    name='cos',
    mode='markers',
    marker_color='rgba(25, 182, 193, .9)'
))

# fig.update_traces(mode='markers', marker_line_width=2, marker_size=10)
fig.update_layout(title='Styled Scatter',   # 标题
                  yaxis_zeroline=False, xaxis_zeroline=False)
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7yhydiuoj31fq0p4mzw.jpg)

````python
t = np.linspace(0, 10, 100)

fig = go.Figure()

fig.add_trace(go.Scatter(
    x=t, y=np.sin(t),
    name='sin',
    mode='markers',
    marker_color='rgba(20, 180, 60, .8)'
))

fig.add_trace(go.Scatter(
    x=t, y=np.cos(t),
    name='cos',
    mode='lines',
    marker_color='rgba(25, 182, 193, .9)'
))

# Set options common to all traces with fig.update_traces
# 设置整个散点图的大小和间隔
fig.update_traces(mode='markers', marker_line_width=2, marker_size=8)
fig.update_layout(title='Styled Scatter',
                  yaxis_zeroline=True, xaxis_zeroline=False)


fig.show()
````

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7ynvt5svj31f40ooq63.jpg)

#### 2.4 数据悬停Data Labels on Hover

在使用go.Scatter的时候，如何实现悬停时候数据的显示

```python
df = px.data.iris()
fig = go.Figure(data = go.Scatter(   # Figure类中的第一个属性是data
    x=df["sepal_length"],  # xy坐标轴的数据
    y=df["sepal_width"],
    mode="markers",  # 点的表示
    marker_color=df["species_id"],  # 点的颜色，px中是color
    text=df["species"]))  # 悬停的显示，px中是hove_data
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7ykty4rxj31cg0mgjt4.jpg)

#### 2.5 Scatter with a Color Dimension

指的是在图形右边实现颜色的不断变化

````python
x = np.linspace(0,10,500)
y = np.random.randint(0,100,500)

fig = go.Figure(data=go.Scatter(
    x=x,
    y=y,
    mode="markers",
    marker=dict(   # marker是字典的形式
        size=20,
        color=np.random.randint(0,100,500),  # 指定颜色区间
        colorscale="Viridis",   # 选择哪种颜色
        showscale=True   # 右边的颜色尺度尺是否显示
    )
))

fig.show()
````

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7yno8zo4j31gk0p8n6x.jpg)

默认的颜色

```python
x = np.linspace(0,10,500)
y = np.random.randint(0,100,500)

fig = go.Figure(data=go.Scatter(
    x=x,
    y=y,
    mode="markers",
    marker=dict(   # marker是字典的形式
        size=20,
        color=np.random.randint(0,100,500),
        showscale=True
    )
))

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg7ymy8kvfj31ew0migv8.jpg)

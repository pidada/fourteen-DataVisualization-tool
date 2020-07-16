---
title: plotly-express-17-plotly绘图技巧之图例与标题(二)
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-07-09 20:06:46
password:
summary:
tags:
  - 可视化
  - px
categories:
  - datavisual
  - px
---


### Plotly-express-17-图例legend和标题设置

本文中介绍的是Plotly中对于图形图例设置的技巧，主要包含：

- 整体基本设置
- 修改图例名称
- 隐藏图例入口（第一个图例）
- 图例位置显示
- 自定义优美图例
- 图例散点大小设置
- 组图例设置
- 标题设置



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkub1wptxj30u0140qv5.jpg)

<!--MORE-->

### 参考

https://plotly.com/python/figure-labels/

https://plotly.com/python/legend/

https://plotly.com/python/reference/#layout

### 整体设置

```python
fig = go.Figure()

fig.add_trace(go.Scatter(
    x=[0, 1, 2, 3, 4, 5, 6, 7, 8],
    y=[0, 1, 2, 3, 4, 5, 6, 7, 8],
    name="Name of Trace 1"       # 第一个图例名称
))

fig.add_trace(go.Scatter(
    x=[0, 1, 2, 3, 4, 5, 6, 7, 8],
    y=[1, 0, 3, 2, 5, 4, 7, 6, 8],
    name="Name of Trace 2",  # 第2个图例名称
  	visible='legendonly'  #  将第2图例变成灰色，点击可见图形

))

fig.update_layout(
    title="Plot Title",     # 主标题
    xaxis_title="x Axis Title",  # 2个坐标轴的标题
    yaxis_title="y Axis Title",
    font=dict(
        family="Courier New, monospace",
        size=18,
        color="#7f7f7f"
    )
)
fig.update_layout(showlegend=False,   # 隐藏图例，默认是True
                  legend_title_text='Trend'   # 修改图例的名称
                 )
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkufte2zjj31q00s2dj3.jpg)

### 图例

#### 隐藏图例入口

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkuk8e7e0j31jt0u0ae7.jpg)

#### 修改图例名称

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkun0ubpaj31mn0u0gp9.jpg)



#### 图例显示位置

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkunkvt8pj31gy0u0dju.jpg)

图例作为legend，位置在左上角

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkuorywgsj31f60u0tc8.jpg)

#### 自定义图例

```python
import plotly.graph_objects as go

fig = go.Figure()

fig.add_trace(go.Scatter(x=[1, 2, 3, 4, 5],y=[1, 2, 3, 4, 5],))
fig.add_trace(go.Scatter(x=[1, 2, 3, 4, 5],y=[5, 4, 3, 2, 1],))

fig.update_layout(
    legend=dict(x=0,y=1,  # 图例的位置：将坐标轴看做是单位1
                traceorder="normal",
                font=dict(
                  family="sans-serif",
            			size=12,
            			color="black"),
                bgcolor="LightSteelBlue",  # 背景颜色，边框颜色和宽度
        				bordercolor="Black",
        				borderwidth=2
               )
)

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkut9lzypj31j80lumze.jpg)

#### 散点大小

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkyp1py9rj319q0u0jvw.jpg)

#### Grouped Legend

```python
import plotly.graph_objects as go

fig = go.Figure()

fig.add_trace(go.Scatter(
    x=[1, 2, 3],
    y=[2, 1, 3],
    legendgroup="group",  # this can be any string, not just "group"
    name="first legend group",  # 名称
    mode="markers",   # 散点类型：markers，lines
    marker=dict(color="Crimson", size=10)   # mode的设置
))

fig.add_trace(go.Scatter(
    x=[1, 2, 3],
    y=[2, 2, 2],
    legendgroup="group",
    name="first legend group - average",
    mode="lines",
    line=dict(color="Crimson")
))

fig.add_trace(go.Scatter(
    x=[1, 2, 3],
    y=[4, 9, 2],
    legendgroup="group2",
    name="second legend group",
    mode="markers",
    marker=dict(color="MediumPurple", size=10)
))

fig.add_trace(go.Scatter(
    x=[1, 2, 3],
    y=[5, 5, 5],
    legendgroup="group2",
    name="second legend group - average",
    mode="lines",
    line=dict(color="MediumPurple")
))

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkyrvlhbyj31nq0naabv.jpg)

### 标题设置-Align Plot Title

```python
import plotly.graph_objects as go

fig = go.Figure(go.Scatter(
    y=[3, 1, 4],
    x=["Mon", "Tue", "Wed"]))

fig.update_layout(
    title={
        'text': "Plot Title",   # 标题名称
        'y':0.9,  # 位置，坐标轴的长度看做1
        'x':0.5,
        'xanchor': 'center',   # 相对位置
        'yanchor': 'top'})

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkyz0ya0jj31ka0skwgl.jpg)

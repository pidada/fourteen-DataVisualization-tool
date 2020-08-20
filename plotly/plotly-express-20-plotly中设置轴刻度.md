---
title: plotly-express-20-plotly中设置轴刻度
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-07-29 00:37:08
password:
summary:
tags:
  - px
  - 可视化
categories:
  - datavisual
  - px
---





在某些业务需求中，我们并不希望坐标轴上的刻度是连续型的，而是具有一些我们指定的间距，这个时候需要我们指定轴刻度。本文中介绍的是如何在`plotly`实现轴刻度的设置。

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh60127he6j31hc0u0anh.jpg)

<!--MORE-->

### 改变起始值

改变坐标轴的起始值，有时候不需要从0开始

```python
import plotly.graph_objects as go

fig = go.Figure(go.Scatter(
    x = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
    y = [28.8, 28.5, 37, 56.8, 69.7, 79.7, 78.5, 77.8, 74.1, 62.6, 45.3, 39.9]
))

fig.update_layout(
    xaxis = dict(
        tickmode = 'linear',
        tick0 = 0.5,   # 起始点
        dtick = 0.75  # 间距
    )
)

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh75duqukaj31740u0aei.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh75gh52ndj314a0u0gpt.jpg)

### 自定义刻度

改变坐标轴上的默认刻度值，用自定义的刻度。通过数组的形式来实现

```python
import plotly.graph_objects as go

go.Figure(go.Scatter(
    x = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
    y = [28.8, 28.5, 37, 56.8, 69.7, 79.7, 78.5, 77.8, 74.1, 62.6, 45.3, 39.9]
))

fig.update_layout(
    xaxis = dict(
        tickmode = 'array',
        tickvals = [1, 3, 10, 7, 9, 12],  # 10表示的是第10个数据
        ticktext = ['One', 'Three', 'Five', 'Seven', 'Nine', 'Eleven']
    )
)

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh75jbtg44j31aa0u00wk.jpg)

### 改变轴刻度属性

```python
import plotly.graph_objects as go

go.Figure(go.Scatter(
    x = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
    y = [28.8, 28.5, 37, 56.8, 69.7, 79.7, 78.5, 77.8, 74.1, 62.6, 45.3, 39.9]
))

fig.update_layout(yaxis_tickformat = '%')
fig.show()
```

![image-20200729003130606](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh75m7ve9hj31bn0u0n12.jpg)

### 坐标轴使用刻度线

```python
import plotly.graph_objects as go

fig = go.Figure(go.Bar(
    x = ["apples", "oranges", "pears"],
    y = [1, 2, 3]
))

fig.update_xaxes(
    showgrid=True,
    ticks="outside",
    tickson="boundaries",
    ticklen=20
)

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh75plnk2dj31a00n2t9s.jpg)

改变标签位置

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh75qf1a94j31as0u0tax.jpg)

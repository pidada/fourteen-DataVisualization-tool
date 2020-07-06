---
title: plotly-express-9-plotly实现线型图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-06-29 10:14:25
password:
summary:
tags:
  - px
  - 可视化
  - dash
categories:
  - datavisual
  - px
---


### plotly-express-9-plotly绘制线型图Line

本文中介绍的是利用`plotly`绘制线型图，使用的是`line()`和`go.Line()`方法

> With `px.line`, each data point is represented as a vertex (which location is given by the `x` and `y` columns) of a **polyline mark** in 2D space.

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8xofga7qj309c064gln.jpg)

<!--MORE-->

### 导入库

```python
import pandas as pd
import numpy as np

import  plotly_express as px
import plotly.graph_objects as go   # 导入go模块

import dash
import dash_core_components as dcc  # dash的组件
import dash_html_components as html
```

### 使用px实现

在`plotly_express`中是通过`px.line`方法来实现的

#### Simple Line Plot with plotly.express

```python
data = px.data.gapminder()
df = data.query("country=='Canada'")

fig = px.line(df, x="year",y="lifeExp",title="Life expectancy in Canada")
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8xoijawjj30ps0d80tb.jpg)

#### dash中实现

在dash中实现的一个通用方法

```python
fig = go.Figure() # or any Plotly Express function e.g. px.bar(...)
# fig.add_trace( ... )
# fig.update_layout( ... )

import dash
import dash_core_components as dcc
import dash_html_components as html

app = dash.Dash()
app.layout = html.Div([
    dcc.Graph(figure=fig)
])

app.run_server()
```

####  Line Plot with column encoding color

```python
df = px.data.gapminder().query("continent=='Oceania'")
fig = px.line(df, x="year", y="lifeExp", color='country')
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8xomoudvj30qr0dcq3r.jpg)

### 使用go.Line实现

#### 官网demo

```python
np.random.seed(1)

N = 100
random_x = np.linspace(0, 1, N)
random_y0 = np.random.randn(N) + 5
random_y1 = np.random.randn(N)
random_y2 = np.random.randn(N) - 5

# Create traces
fig = go.Figure()
fig.add_trace(go.Scatter(x=random_x, y=random_y0,
                    mode='lines',
                    name='lines'))
fig.add_trace(go.Scatter(x=random_x, y=random_y1,
                    mode='lines+markers',
                    name='lines+markers'))   # 同时使用线型和点
fig.add_trace(go.Scatter(x=random_x, y=random_y2,
                    mode='markers', name='markers'))

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8xp1eu0tj30qd0cgdh6.jpg)

###  Style Line Plots

如何给线性图设置风格

```python
# Add data
month = ['January', 'February', 'March', 'April', 'May', 'June', 'July',
         'August', 'September', 'October', 'November', 'December']

high_2007 = [36.5, 26.6, 43.6, 52.3, 71.5, 81.4, 80.5, 82.2, 76.0, 67.3, 46.1, 35.0]
low_2007 = [23.6, 14.0, 27.0, 36.8, 47.6, 57.7, 58.9, 61.2, 53.3, 48.5, 31.0, 23.6]
high_2014 = [28.8, 28.5, 37.0, 56.8, 69.7, 79.7, 78.5, 77.8, 74.1, 62.6, 45.3, 39.9]
low_2014 = [12.7, 14.3, 18.6, 35.5, 49.9, 58.0, 60.0, 58.6, 51.7, 45.2, 32.2, 29.1]

fig = go.Figure()
# Create and style traces
fig.add_trace(go.Scatter(x=month, y=high_2014, name='High 2014',
                         line=dict(color='firebrick', width=4)))  # 通过line参数设置
fig.add_trace(go.Scatter(x=month, y=low_2014, name = 'Low 2014',
                         line=dict(color='royalblue', width=4)))

fig.add_trace(go.Scatter(x=month, y=high_2007, name='High 2007',
                         line=dict(color='firebrick', width=4,
                              dash='dashdot') # dash options include 'dash', 'dot', and 'dashdot'
))
fig.add_trace(go.Scatter(x=month, y=low_2007, name='Low 2007',
                         line = dict(color='royalblue', width=4, dash='dot')))

# Edit the layout
fig.update_layout(title='Average High and Low Temperatures in New York',   # 左上角的标题设置
                   xaxis_title='Month',  # 横纵坐标的名称
                   yaxis_title='Temperature (degrees F)')

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8xp7nu26j30s80ikwgl.jpg)

### Connect Data Gaps

如果数据中存在缺失值或者说断点，如何处理？

```python
x = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]

fig = go.Figure()

fig.add_trace(go.Scatter(
    x=x,
    y=[10, 20, None, 15, 10, 5, 15, None, 20, 10, 10, 15, 25, 20, 10],
    name = '<b>No</b> Gaps', # Style name/legend entry with html tags：使用HTML标签给图例或者名字进行格式设置
    connectgaps=True # override default to connect the gaps  断点连接起来
))
fig.add_trace(go.Scatter(
    x=x,
    y=[5, 15, None, 10, 5, 0, 10, None, 15, 5, 5, 10, 20, 15, 5],
    name='Gaps',
))

fig.update_layout(title="How to deal with the missing data with plot")   # 图例

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8xpc2i1qj30qk0cjwfj.jpg)

### Label Lines with Annotations

如何给图中的某些点添加注释`annotions`

```python
fig = go.Figure()

fig.add_trace(go.Scatter(
    x=[0, 1, 2, 3, 4, 5, 6, 7, 8],
    y=[0, 1, 3, 2, 4, 3, 4, 6, 5],
    name = "Line-1"  # 将右上角的标签进行修改
))

fig.add_trace(go.Scatter(
    x=[0, 1, 2, 3, 4, 5, 6, 7, 8],
    y=[0, 4, 5, 1, 2, 2, 3, 4, 2],
    name = "Line-2"
))

fig.add_annotation(
            x=2,  # 给（2.5） 给特殊点添加注解
            y=5,
            text="max number")

fig.add_annotation(
            x=4,
            y=4,
            text="median number")

fig.update_annotations(dict(
            xref="x",
            yref="y",
            showarrow=True,
            arrowhead=7,
            ax=0,
            ay=-40
))

fig.update_layout(showlegend=True)  # 显示图例

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8xphtneuj30r90c9aay.jpg)

### Filled Lines

通过一个例子学习如何画出带有填充区域的线型图

```python
x = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
x_rev = x[::-1]  # 翻转的数据

# Line 1
y1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
y1_upper = [2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
y1_lower = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
y1_lower = y1_lower[::-1]

# Line 2
y2 = [5, 2.5, 5, 7.5, 5, 2.5, 7.5, 4.5, 5.5, 5]
y2_upper = [5.5, 3, 5.5, 8, 6, 3, 8, 5, 6, 5.5]
y2_lower = [4.5, 2, 4.4, 7, 4, 2, 7, 4, 5, 4.75]
y2_lower = y2_lower[::-1]

fig = go.Figure()

# 第一条线的填充区域
fig.add_trace(go.Scatter(
    x = x + x_rev,
    y = y1_upper + y1_lower,
    fill = 'toself',  # 显示填充
    fillcolor = 'rgba(0,100,80,0.2)',  # 填充区域颜色
    line_color = 'rgba(255,255,255,0)',  # 边界颜色
    showlegend = False,
    name = 'Fair',
))
fig.add_trace(go.Scatter(
    x = x + x_rev,
    y = y2_upper + y2_lower,
    fill = 'toself',
    fillcolor = 'rgba(231,107,243,0.2)',
    line_color = 'rgba(255,255,255,0)',
    name = 'Premium',
    showlegend = False,
))

# 画两条线
fig.add_trace(go.Scatter(
    x=x, y=y1,
    line_color='rgb(0,100,80)',
    name='Fair',
))

fig.add_trace(go.Scatter(
    x=x, y=y2,
    line_color='rgb(231,107,243)',
    name='Premium',
))

# fig.update_traces(mode='lines')

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg8xpltknrj30rf0cmgmp.jpg)

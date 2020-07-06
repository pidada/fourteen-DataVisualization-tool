---
title: plotly-express-10-plotly实现饼图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-06-29 12:32:38
password:
summary:
tags:
  - px
  - dash
  - 可视化
categories:
  - datavisual
  - px
---


 本文中介绍的是如何利用px.pie和go.Pie来绘制饼图，以及在饼图中的相关参数设置。

> A pie chart is a circular statistical chart, which is divided into sectors to illustrate numerical proportion.

- 基于px.pie
- 基于go.pie

<!--MORE-->

### 导入库

```python
import pandas as pd
import numpy as np

import plotly_express as px
import plotly.graph_objects as go
 
import dash
import dash_core_components as dcc
import dash_html_components as html
```



### 基于px.pie实现

In `px.pie`, data visualized by the sectors of the pie is set in `values`. The sector labels are set in `names`.

```python
df = px.data.gapminder().query("year == 2007").query("continent == 'Europe'")
df.loc[df["pop"] < 2.e6, "country"] = "Other countries"  # 将满足条件的全部改为Other countries
fig = px.pie(df, values="pop", names="country", title="Population of Eurpean contient")
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91wsurqtj30tr0fvdip.jpg)





### 基于go.pie实现

#### basic 

In `go.Pie`, data visualized by the sectors of the pie is set in `values`. The sector labels are set in `labels`. The sector colors are set in `marker.colors`.

- labels
- values

```python
labels = ['Oxygen','Hydrogen','Carbon_Dioxide','Nitrogen']
values = [4500, 2500, 1053, 500]

fig = go.Figure(data=go.Pie(labels=labels,values=values))   
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91wrrtnvj30l60c3jrq.jpg)

#### update_traces

```python
# 颜色选项
colors = ['gold', 'mediumturquoise', 'darkorange', 'lightgreen']

# 绘图
fig = go.Figure(data=go.Pie(labels=['Oxygen','Hydrogen','Carbon_Dioxide','Nitrogen'],
                             values=[4500,2500,1053,500]))

# 更新饼图配置
fig.update_traces(hoverinfo='label+percent',  # 悬停信息
                  textinfo='value', # 饼图中显示的信息
                  textfont_size=20,
                  marker=dict(colors=colors, line=dict(color='#000000', width=2)))
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91woxgyaj30su0j83zy.jpg)

#### 文本隐藏

```python
df = px.data.gapminder().query("continent == 'Asia'")
fig = px.pie(df, values='pop', names='country')

fig.update_traces(textposition='inside') # 文字信息在里面
fig.update_layout(uniformtext_minsize=15,  # 文本信息的最小值
                  uniformtext_mode='hide'  # 小于最小值则被隐藏
                 )
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91wmolfvj30la0d3q47.jpg)

不进行隐藏的效果对比：

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91wkp98ij30tj0gpaby.jpg)







### 设置饼图颜色

#### 通过序列形式

通过`color_discrete_sequence=px.colors.sequential.Bluyl`来实现

```python
df = px.data.tips()
#  设置饼图的颜色：px.colors.sequential.Bluyl
fig = px.pie(df, values="tip", names="day",color_discrete_sequence=px.colors.sequential.Bluyl)  
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91wihyh6j30nz0d374n.jpg)

#### 通过字典形式

```python
# 通过字典的形式上色：这个颜色好看呀
fig = px.pie(df, values="tip",names="day",color="day",
            color_discrete_map={'Thur':'lightcyan',
                                 'Fri':'cyan',
                                 'Sat':'royalblue',
                                 'Sun':'darkblue'})
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91wgkeoxj30k00bower.jpg)

### 布局和属性设置

```python
df = px.data.gapminder().query("year == 2007").query("continent == 'Americas'")   # 居然是Americas！！！！

fig = px.pie(df, values='pop', names='country',
             title='Population of American continent',
             hover_data=['lifeExp'], labels={'lifeExp':'life expectancy'})

fig.update_traces(textposition='inside', textinfo='percent+label')  # 将文本的信息放在里面
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91we95vdj30s60fp763.jpg)

修改参数之后的效果对比：

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91wbedw9j30r20gadgz.jpg)

### 文本排列

文本信息如何在扇区中进行合理地排列，3种方式：

- horizontal
- radial
- tangential

![NWqdM9.png](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91w8oerij30tw0hit9n.jpg)

![NWquvj.png](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91w5dpz6j30tz0ghjsc.jpg)

![NWqDVx.png](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91w3vsmej30up0hnmy7.jpg)



### Donut & Pulling sectors 

```python
labels = ['Oxygen','Hydrogen','Carbon_Dioxide','Nitrogen']
values = [4500, 2500, 1053, 500]

# Use `hole` to create a donut-like pie chart
fig = go.Figure(data=[go.Pie(labels=labels, values=values, hole=.3)])   # 通过hole参数实现中心的环
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91w1mc9mj30nf0cm74n.jpg)



```python
labels = ['Oxygen','Hydrogen','Carbon_Dioxide','Nitrogen']
values = [4500, 2500, 1053, 500]

# pull is given as a fraction of the pie radius
fig = go.Figure(data=[go.Pie(labels=labels, values=values, pull=[0, 0, 0.2, 0])])  # 通过pull参数
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91vybjlaj30je0aq0sz.jpg)

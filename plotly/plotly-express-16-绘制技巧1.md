---
title: plotly-express-16-绘制技巧(一)
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-07-09 16:34:45
password:
summary:
tags:
  - 可视化
  - px
categories:
  - datavisual
  - px
---

### Plotly-express-16-绘制技巧（一）

本文中介绍的是利用Plotly绘图小技巧：

- 图片的保存：jupyter notebook下的保存和指定路径下的保存
- 柱状图的颜色改变（避免同样的颜色过于单调）
- 双坐标轴图形的绘制
- 子图制作

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggjt5bu6cjj30u0140hdu.jpg)

<!--MORE-->

### 保存路径

#### 改变路径

需要导入的库是os模块

- .plot方法可以在指定位置生成
- .iplot方法在notebook中生成

```python
import plotly as py
import plotly.graph_objects as go
from plotly.graph_objects import Scatter,Bar
py.offline.init_notebook_mode()   # notebook里面直接生成图片
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggksu043kgj315c0n6wii.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggksubdt7wj31ce0nidgz.jpg)

#### 改变颜色

```python
import random
import plotly as py
import plotly.graph_objects as go
from plotly.graph_objects import Scatter,Bar
py.offline.init_notebook_mode()   # notebook里面直接生成图片


# 生成颜色的函数
def random_color_generator(number_of_colors):
    color = ["#"+''.join([random.choice('0123456789ABCDEF') for j in range(6)])
                 for i in range(number_of_colors)]
    return color


# add data
trace0 = Scatter(x=[1,2,3,4],
                 y=[2,4,6,8])

# 改变每个柱状图的颜色
trace1 = Bar(x=[2,3,4,5],
             y=[10,12,16,20],
             marker=dict(color=random_color_generator(4)), # 使用随机函数生成4个颜色 ，改变柱状图的颜色
             opacity=0.7
            )

data = [trace0,trace1]


# add layout
layout = go.Layout(title="basic figure by using plotly",
                  xaxis=dict(title="x轴数据"),
                  yaxis=dict(title="y轴数据"),
                  legend=dict(x=1,y=0.5,font=dict(size=20, color="black")))

# add figure
fig = go.Figure(data=data, layout=layout)

# generate figure
fig.show(render="svg")
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggksvl15faj317k08475s.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggksw6cseoj31di0pqwgb.jpg)

### 绘制双坐标轴

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggksyffuk0j312g0tgae4.jpg)

```python
# df.sort_values("产量",ascending=False)：将产量进行降序排列，图形更直观
# 排序函数：sort_values("col",ascending=True)  根据某个字段，默认是升序

trace1 = go.Bar(
    x=df.sort_values("产量",ascending=False)["地区"].values.tolist(),
    y=df.sort_values("产量",ascending=False)["产量"].values.tolist(),
    marker=dict(color=random_color_generator(6)),
    # 指定显示的文本，类似y值
    text=df.sort_values("产量",ascending=False)["产量"].values.tolist(),
    textposition="outside",  # 文本的位置
    opacity=0.5,
    name="地区订单数量"
)

trace2 = go.Scatter(
    x=df["地区"].values.tolist(),
    y=df["财政收入"].values.tolist(),
    mode="markers",
    marker=dict(color=random_color_generator(6)),
    opacity=0.5,
    name="地区财政收入",
    yaxis="y2"   # 这是第二条y轴
)

data = [trace1,trace2]  # 添加图形的轨迹数据

layout = go.Layout(title="不同地区的订单数量",
                   xaxis=dict(title="地区"),   # 共用x轴
                   yaxis=dict(title="地区订单数量"),  # 第一条y轴的名字
                   yaxis2=dict(title="地区财政收入",overlaying="y", side="right"),   # 第二条y轴的名字，堆叠位置（与y相同），位置在右边
                   legend=dict(x=0.8,y=0.9,font=dict(size=12,color="red"))  # 图例的位置（图形看做一个单位长度），大小和字体颜色
                  )

fig = go.Figure(data=data,layout=layout)

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggksz5t9xej31fg0p6wh1.jpg)

### 子图制作

介绍利用plotly中的tools工具如何制作子图

```python
from plotly import tools   # 导入工具

fig = tools.make_subplots(rows=2,cols=1)  # 2*1的子图

# ----------------
trace1 = go.Bar(
    x=df.sort_values("产量",ascending=False)["地区"].values.tolist(),
    y=df.sort_values("产量",ascending=False)["产量"].values.tolist(),
    marker=dict(color=random_color_generator(6)),
    # 指定显示的文本，类似y值
    text=df.sort_values("产量",ascending=False)["产量"].values.tolist(),
    textposition="outside",  # 文本的位置
    opacity=0.5,
    name="地区订单数量"
)

sz = np.random.rand(100) * 30

trace2 = go.Scatter(
    x=df["地区"].values.tolist(),
    y=df["财政收入"].values.tolist(),
    mode="markers",
    marker=dict(color=random_color_generator(6),
		        size=sz,
				opacity=0.5,
				colorscale="Viridis"
		),
    name="地区财政收入",
    yaxis="y2"   # 这是第二条y轴
)

# -----------------------
# 添加数据轨迹
fig.append_trace(trace1,1,1)
fig.append_trace(trace2,2,1)

# 修改fig的布局
fig["layout"].update(height=600,width=800,title="子图制作")
fig.show()
```
![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggktkullw4j316y0rq40q.jpg)

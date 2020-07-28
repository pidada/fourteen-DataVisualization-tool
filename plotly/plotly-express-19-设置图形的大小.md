---
title: plotly-express-19-设置图形的大小
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-07-28 00:08:52
password:
summary:
tags:
  - px
  - 可视化
categories:
  - datavisual
  - px
---

### Plotly-express-19-plotly中设置图形大小

本文中介绍的是如何在`plotly`中通过两种方法来设置图形的大小

- `px`实现
- `go.Figure`实现

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh5z4ya15zj30u0140x6p.jpg)



<!--MORE-->

### px实现

#### 数据

数据使用的是px中自带的tips数据集

```python
import plotly.express as px

df = px.data.tips()
df
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh5z3bgbjyj30r20ja76j.jpg)

#### 绘图

```python
fig = px.scatter(df, x="total_bill", y="tip", facet_col="sex",facet_row="smoker",  # 横纵坐标的参考属性
                 width=1000, height=600)

fig.update_layout(
    margin=dict(l=20, r=20, t=20, b=20),  # 上下左右的边距大小
    paper_bgcolor="dodgerblue",  # 颜色
)

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh5z504v8sj30rs0go0u9.jpg)

#### 颜色

记录了全部颜色

```python
liceblue, antiquewhite, aqua, aquamarine, azure,
            beige, bisque, black, blanchedalmond, blue,
            blueviolet, brown, burlywood, cadetblue,
            chartreuse, chocolate, coral, cornflowerblue,
            cornsilk, crimson, cyan, darkblue, darkcyan,
            darkgoldenrod, darkgray, darkgrey, darkgreen,
            darkkhaki, darkmagenta, darkolivegreen, darkorange,
            darkorchid, darkred, darksalmon, darkseagreen,
            darkslateblue, darkslategray, darkslategrey,
            darkturquoise, darkviolet, deeppink, deepskyblue,
            dimgray, dimgrey, dodgerblue, firebrick,
            floralwhite, forestgreen, fuchsia, gainsboro,
            ghostwhite, gold, goldenrod, gray, grey, green,
            greenyellow, honeydew, hotpink, indianred, indigo,
            ivory, khaki, lavender, lavenderblush, lawngreen,
            lemonchiffon, lightblue, lightcoral, lightcyan,
            lightgoldenrodyellow, lightgray, lightgrey,
            lightgreen, lightpink, lightsalmon, lightseagreen,
            lightskyblue, lightslategray, lightslategrey,
            lightsteelblue, lightyellow, lime, limegreen,
            linen, magenta, maroon, mediumaquamarine,
            mediumblue, mediumorchid, mediumpurple,
            mediumseagreen, mediumslateblue, mediumspringgreen,
            mediumturquoise, mediumvioletred, midnightblue,
            mintcream, mistyrose, moccasin, navajowhite, navy,
            oldlace, olive, olivedrab, orange, orangered,
            orchid, palegoldenrod, palegreen, paleturquoise,
            palevioletred, papayawhip, peachpuff, peru, pink,
            plum, powderblue, purple, red, rosybrown,
            royalblue, rebeccapurple, saddlebrown, salmon,
            sandybrown, seagreen, seashell, sienna, silver,
            skyblue, slateblue, slategray, slategrey, snow,
            springgreen, steelblue, tan, teal, thistle, tomato,
            turquoise, violet, wheat, white, whitesmoke,
            yellow, yellowgreen
```

### go.Figure实现

#### 数据

```python
import plotly.graph_objects as go

fig = go.Figure()

fig.add_trace(go.Scatter(
    x=[0, 1, 2, 3, 4, 5, 6, 7, 8],
    y=[0, 1, 2, 3, 4, 5, 6, 7, 8]
))

fig.update_layout(
    autosize=False,
    width=800,
    height=800,
    margin=dict(
        l=50,
        r=50,
        b=100,
        t=100,
        pad=9
    ),
    paper_bgcolor="mediumaquamarine",
)

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh5z89nkdej30m80m8t9o.jpg)

第二个案例

```python
import plotly.graph_objects as go


fig = go.Figure()

fig.add_trace(go.Bar(
    x=["Apples", "Oranges", "Watermelon", "Pears"],
    y=[3, 2, 1, 4]
))

fig.update_layout(
    autosize=False,
    width=1000,
    height=800,
    yaxis=dict(
        title_text="Y-axis Title",
        ticktext=["Very long label", "long label", "3", "label"],
        tickvals=[1, 2, 3, 4],
        tickmode="array",
        titlefont=dict(size=50),
    )
)

fig.update_yaxes(automargin=True)  # Y-axis Title自动移到左边合适的位置

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh5za0reccj31860u040g.jpg)

如果设置成`False`的话，效果为：

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh5zekamyuj316x0u0abs.jpg)

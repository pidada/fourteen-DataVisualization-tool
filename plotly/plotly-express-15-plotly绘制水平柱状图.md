---
title: plotly-express-15-plotly绘制水平柱状图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-07-09 10:05:33
password:
summary:
tags:
  - 可视化
  - px
categories:
  - datavisual
  - px
---

### Plotly-express-15-plotly实现水平柱状图（h）

本文中介绍的是如何在plotly中实现水平方向的柱状图：

- px.bar(oritention="h")
- go.Bar(oritention="h)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkub1wptxj30u0140qv5.jpg)

<!--MORE-->

### Horizontal Bar Chart with Plotly Express

#### demo



```python
import plotly.express as px
df = px.data.tips()
fig = px.bar(df, x="total_bill", y="day", orientation='h')
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkgghloeej31l60niabe.jpg)

#### Configure horizontal bar chart

In this example a column is used to color the bars, and we add the information from other columns to the hover data.

```python
import plotly.express as px
df = px.data.tips()
fig = px.bar(df, x="total_bill", y="sex", color='day', orientation='h',
             hover_data=["tip", "size"],   # 悬停数据
             height=400,  # 高度
             title='Restaurant bills')  # 标题
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkghy13r8j31om0gu75x.jpg)

### Horizontal Bar Chart with go.Bar

#### demo

```python
import plotly.graph_objects as go

fig = go.Figure(go.Bar(
            x=[20, 14, 23],
            y=['giraffes', 'orangutans', 'monkeys'],
            orientation='h'))

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkgiwwxq7j31o20o8wfs.jpg)





#### Colored Horizontal Bar Chart

```python
import plotly.graph_objects as go

fig = go.Figure()
fig.add_trace(go.Bar(
    y=['giraffes', 'orangutans', 'monkeys'],
    x=[20, 14, 23],
    name='SF Zoo',
    orientation='h',
    marker=dict(
        color='rgba(246, 78, 139, 0.6)',
        line=dict(color='rgba(246, 78, 139, 1.0)', width=3)
    )
))
fig.add_trace(go.Bar(
    y=['giraffes', 'orangutans', 'monkeys'],
    x=[12, 18, 29],
    name='LA Zoo',
    orientation='h',
    marker=dict(
        color='rgba(58, 71, 80, 0.6)',
        line=dict(color='rgba(58, 71, 80, 1.0)', width=3)
    )
))

fig.update_layout(barmode='stack')
fig.show()
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkgl7np4bj311l0u0tdz.jpg)

![image-20200709092402483](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkgm3r0afj31ou0mujsn.jpg)

### Color Palette for Bar Chart

```python
import plotly.graph_objects as go

# 最上面的属性，添加了Give up
top_labels = ['Strongly<br>agree', 'Agree', 'Neutral', 'Disagree',
              'Strongly<br>disagree', 'Give<br>up']

# 每行数据中每个框的颜色
colors = ['rgba(38, 24, 74, 0.8)', 'rgba(71, 58, 131, 0.8)',
          'rgba(122, 120, 168, 0.8)', 'rgba(164, 163, 204, 0.85)',
          'rgba(190, 192, 213, 1)','rgba(224, 210, 240, 10)']

# xy的数据
x_data = [[16, 30, 21, 16, 12, 5],
          [24, 21, 19, 10, 15, 11],
          [7, 26, 23, 11, 13, 20],
          [29, 10, 15, 18, 14, 14],
         ]

# 左侧的属性
y_data = ['The course was effectively<br>organized',
          'The course developed my<br>abilities and skills ' +
          'for<br>the subject', 'The course developed ' +
          'my<br>ability to think critically about<br>the subject',
          'I would recommend this<br>course to a friend']

fig = go.Figure()

#
for i in range(0, len(x_data[0])):
    for xd, yd in zip(x_data, y_data):
        fig.add_trace(go.Bar(   # 每个方框都是一个trace
            x=[xd[i]],
            y=[yd],
            orientation='h',
            marker=dict(
                color=colors[i],
                line=dict(color='rgb(248, 248, 249)', width=1)
            )
        ))

fig.update_layout(
    xaxis=dict(
        showgrid=False,
        showline=False,
        showticklabels=False,
        zeroline=False,
        domain=[0.15, 1]
    ),
    yaxis=dict(
        showgrid=False,
        showline=False,
        showticklabels=False,
        zeroline=False,
    ),
    barmode='stack',
    paper_bgcolor='rgb(248, 248, 255)',  # 整个画布和图片的背景色
    plot_bgcolor='rgb(248, 248, 255)',
    margin=dict(l=120, r=10, t=140, b=80),
    showlegend=False,
)

annotations = []   # 添加注解

for yd, xd in zip(y_data, x_data):

    # labeling the y-axis
    annotations.append(dict(xref='paper', yref='y',
                            x=0.14, y=yd,
                            xanchor='right',
                            text=str(yd),
                            font=dict(family='Arial', size=14,
                                      color='rgb(67, 67, 67)'),
                            showarrow=False, align='right'))

    # labeling the first percentage of each bar (x_axis)
    annotations.append(dict(xref='x', yref='y',
                            x=xd[0] / 2, y=yd,
                            text=str(xd[0]) + '%',
                            font=dict(family='Arial', size=14,
                                      color='rgb(248, 248, 255)'),
                            showarrow=False))

    # labeling the first Likert scale (on the top)
    if yd == y_data[-1]:
        annotations.append(dict(xref='x', yref='paper',
                                x=xd[0] / 2, y=1.1,
                                text=top_labels[0],
                                font=dict(family='Arial', size=14,
                                          color='rgb(67, 67, 67)'),
                                showarrow=False))

    space = xd[0]
    for i in range(1, len(xd)):
            # labeling the rest of percentages for each bar (x_axis)
            annotations.append(dict(xref='x', yref='y',
                                    x=space + (xd[i]/2), y=yd,
                                    text=str(xd[i]) + '%',
                                    font=dict(family='Arial', size=14,
                                              color='rgb(248, 248, 255)'),
                                    showarrow=False))

            # labeling the Likert scale
            if yd == y_data[-1]:
                annotations.append(dict(xref='x', yref='paper',
                                        x=space + (xd[i]/2), y=1.1,
                                        text=top_labels[i],
                                        font=dict(family='Arial', size=14,
                                                  color='rgb(67, 67, 67)'),
                                        showarrow=False))
            space += xd[i]

fig.update_layout(annotations=annotations)   # 注解

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggkhsclyi2j31k80kwn12.jpg)

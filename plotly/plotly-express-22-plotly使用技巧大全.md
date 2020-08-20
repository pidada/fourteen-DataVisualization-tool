---
title: plotly-express-22-plotly使用技巧大全
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-08-16 17:24:18
password:
summary:
tags:
  - px
  - 可视化
categories:
  - datavisual
  - px
---


本文中将前段时间写的`plotly-express`可视化库的相关技巧进行整理，**方便后续快速实现调用**

- 先整理之前写的亮点
- 后面肯定会补充内容

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsq8985uuj31400u0aex.jpg)



<!--MORE-->

### 常见绘图参数

常见图形的[绘图参数]([http://renpeter.cn/2020/06/04/plotly-express-4-%E5%B8%B8%E8%A7%81%E7%BB%98%E5%9B%BE%E5%8F%82%E6%95%B0.html](http://renpeter.cn/2020/06/04/plotly-express-4-常见绘图参数.html))

### 修改x/y轴名称(px)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsqf1v299j31m40u0n14.jpg)

### 修改x/y轴名称(go)

```python
# Edit the layout
fig.update_layout(title='Average High and Low Temperatures in New York',   # 左上角的标题设置
                   xaxis_title='Month',  # 横纵坐标的名称
                   yaxis_title='Temperature (degrees F)')
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsqlukluxj30s80ikju3.jpg)

### 修改X/Y轴名称（figure实现）

```python
# 布局设置
layout = go.Layout(
    title = 'Prime genre',   # 整个图的标题
    margin = dict(
        l = 100   # 左边距离
    ),
    xaxis = dict(
        title = 'Type of app'   # 2个轴的标题
    ),
    yaxis = dict(
        title = 'Count of app'
    ),
    width = 900,
    height = 500
)

fig = go.Figure(data=data, layout=layout)
fig.update_traces(textposition="outside")
```

### 坐标轴刻度

[坐标轴]([http://renpeter.cn/2020/07/29/plotly-express-20-plotly%E4%B8%AD%E8%AE%BE%E7%BD%AE%E8%BD%B4%E5%88%BB%E5%BA%A6.html](http://renpeter.cn/2020/07/29/plotly-express-20-plotly中设置轴刻度.html))的起始点和间距问题

### 多子图绘制-1

```python
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

### 多子图绘制-2

[子图]([http://renpeter.cn/2020/07/04/plotly-express-12-plotly%E5%AE%9E%E7%8E%B0%E5%A4%9A%E5%AD%90%E5%9B%BE.html](http://renpeter.cn/2020/07/04/plotly-express-12-plotly实现多子图.html))的绘制知识点很多，主要包含：

- 每个子图的名称
- 指定几行几列
- 子图属性设置
  - 第一个子图的起始位置
  - 每个子图的标题
  - 子图之间的间隔设置
  - 如何共享x轴
  - 每个子图中的文本信息设置及位置显示
  - 子图右边的图例名称
  - 子图的位置通过row/col实现
  - 单独设置xy轴的名称
- 共享轴
- 自定义子图位置（在哪行哪列）
- 子图类型

```python
fig = make_subplots(
    rows=2, cols=2, shared_yaxes=True，  # 共享y轴
    specs=[[{"type": "xy"}, {"type": "polar"}],   # 子图类型
           [{"type": "domain"}, {"type": "scene"}]],
)
```



### 冒泡点大小

```python
fig = go.Figure(go.Scatter(
  x=np.linspace(0,50,10),
  y=np.random.randint(0,50,10),
  mode="markers",
  marker=dict(size=np.random.randint(0,50,10),  # 通过字典的形式来实现
              color=np.random.randint(50,100,10))
                          ))
```

### 冒泡点颜色

```python
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
```

### 添加注释（某个特殊点）

```python
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
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsqn9ud4xj30r90c93zk.jpg)

### 柱状图-信息显示在外面

```python
df = px.data.gapminder().query("continent == 'Europe' and year == 2007 and pop > 2.e6")
fig = px.bar(df, x="country", y="pop", text="pop")

fig.update_traces(texttemplate="%{text:.2s}",  # 数字保留2位有效数字
                  textposition="outside")  # 数字显示在外面
fig.update_layout(uniformtext_minsize=10, uniformtext_mode="hide")

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsraehc30j30qf0ci401.jpg)

### 柱状图-改变柱子颜色

结合颜色随机生成方法

```python
# 生成颜色的函数
def random_color_generator(number_of_colors):
    color = ["#"+''.join([random.choice('0123456789ABCDEF') for j in range(6)])
                 for i in range(number_of_colors)]
    return color
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsrrmm9brj317k08475m.jpg)

### 绘制双坐标

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



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsrusmo3vj31fg0p6whc.jpg)









### 柱状图-坐标轴排序

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsrc7oftoj30t20i3gn4.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsrclby14j30sh0hhq4a.jpg)

### 饼图-颜色（自定义）

#### go实现

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg91woxgyaj30su0j83zy.jpg)

#### px实现

##### 字典形式

```python
fig = px.pie(df, values="tip",names="day",color="day",
            color_discrete_map={'Thur':'lightcyan',
                                 'Fri':'cyan',
                                 'Sat':'royalblue',
                                 'Sun':'darkblue'})
```

##### 通过序列形式

通过`color_discrete_sequence=px.colors.sequential.Bluyl`来实现

```python
df = px.data.tips()
#  设置饼图的颜色：px.colors.sequential.Bluyl
fig = px.pie(df, values="tip", names="day",color_discrete_sequence=px.colors.sequential.Bluyl)
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsqtexld0j30nz0d33yu.jpg)



### 饼图-文本隐藏

```python
df = px.data.gapminder().query("continent == 'Asia'")
fig = px.pie(df, values='pop', names='country')

fig.update_traces(textposition='inside') # 文字信息在里面
fig.update_layout(uniformtext_minsize=15,  # 文本信息的最小值
                  uniformtext_mode='hide'  # 小于最小值则被隐藏
                 )
fig.show()

```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsqrkse7uj30la0d3wfp.jpg)

### 饼图-布局和属性设置

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsqwxqk95j30s60fpwge.jpg)

### 饼图-文本位置（3种）

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsqxivs1uj30tw0hign5.jpg)

### 百分比实现

将各个类别的数量变成百分比

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggilz9nhsvj31b40q4q6f.jpg)

### 颜色随机生成（优秀）

这个方法很巧妙，能够用在任何绘制的图形中，只要有多个颜色出现：只需要在color参数中调用函数即可实现

```python
# 颜色的随机生成：#123456  # 加上6位数字构成
def random_color_generator(number_of_colors):
    color = ["#"+''.join([random.choice('0123456789ABCDEF') for j in range(6)])
                 for i in range(number_of_colors)]
    return color

trace = go.Bar(
    x = cnt_srs.index,
    y = cnt_srs.values,
    text = text,
    marker = dict(
        color = random_color_generator(100),
        line = dict(color='rgb(8, 48, 107)',
                    width = 1.5)
    ),
    opacity = 0.7
)
```

### 坐标轴倾斜

将坐标轴的文字信息倾斜显示

```python
# 图表标题
fig.update_layout(title_text='January 2013 Sales Report',xaxis_tickangle=-45) # + x轴标签旋转45度
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghsr8vyf71j30r90f4wfc.jpg)



### Plotly实现表格

如何使用Plotly实现[表格]([http://renpeter.cn/2020/07/06/plotly-express-13-plotly%E7%94%9F%E6%88%90%E8%A1%A8%E6%A0%BC.html](http://renpeter.cn/2020/07/06/plotly-express-13-plotly生成表格.html))

### jupyter中保存图片

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggksu043kgj315c0n6wii.jpg)

### kaleido保存图片

```python
from kaleido.scopes.plotly import PlotlyScope
import plotly.graph_objects as go
scope = PlotlyScope()

fig = go.Figure(data=[go.Scatter(y=[1, 3, 2])])
with open("figure.png", "wb") as f:   # 在本地目录下面就会生成文件
    f.write(scope.transform(fig, format="png"))
```

### 图例设置

对于[图例设置]([http://renpeter.cn/2020/07/09/plotly-express-17-plotly%E7%BB%98%E5%9B%BE%E6%8A%80%E5%B7%A7%E4%B9%8B%E5%9B%BE%E4%BE%8B%E4%B8%8E%E6%A0%87%E9%A2%98.html](http://renpeter.cn/2020/07/09/plotly-express-17-plotly绘图技巧之图例与标题.html))的技巧，主要包含：

- 整体基本设置
- 修改图例名称
- 隐藏图例入口（第一个图例）
- 图例位置显示
- 自定义优美图例
- 图例散点大小设置
- 组图例设置
- 标题设置

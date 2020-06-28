---
title: plotly-express-7-Dash利用滑动条实现数据选择
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-06-26 20:37:19
password:
summary:
tags:
  - 可视化
  - pandas
  - px
categories:
  - datavisual
  - px
---


本文中介绍的是`Dash`如何利用滑动条实现数据选择，同时自动更新可视化的图形

- 利用`Slider`实现
- 利用`RangeSlider`实现
- 利用`px`库实现`RangeSlider`

<!--MORE-->

### 利用Slider实现

**Slider的特点是：一端是固定的，只能够移动一个端点**

#### demo

官网上的`demo`

```python
import dash
import dash_html_components as html
import dash_core_components as dcc

external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

app = dash.Dash(__name__, external_stylesheets=external_stylesheets)
app.layout = html.Div([
    dcc.Slider(
        id='my-slider',
        min=0,
        max=20,
        step=0.5,
        value=10,
    ),
    html.Div(id='slider-output-container')
])


@app.callback(
    dash.dependencies.Output('slider-output-container', 'children'),
    [dash.dependencies.Input('my-slider', 'value')])
def update_output(value):
    return 'You have selected "{}"'.format(value)


if __name__ == '__main__':
    app.run_server(debug=True)
```



#### 实例



```python
import plotly_express as px
import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output

import pandas as pd

df = px.data.gapminder()  # 使用px内置的数据

external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']
app = dash.Dash(__name__, external_stylesheets=external_stylesheets)

app.layout = html.Div([
    html.H1("Select Data with Slider", style={"text-align":"center","color":"blue"}),
    dcc.Graph(id="graph-with-slider"),
    dcc.Slider(
        id = "year-slider",
        min = df["year"].min(), max = df["year"].max(),value = df["year"].min(),  # 范围和初始值
        marks = {str(year):str(year) for year in df["year"].unique()},  # 滑动条下每个年份数字，改成字符型数据
        step = None  # 步长
    )
])

@app.callback(
    Output("graph-with-slider","figure"),  # 输出与输入，和上面的名称一一对应
    [Input("year-slider","value")]
)

def update_figure(selected_year):  # 生成画图需要的数据
    filtered_df = df[df.year == selected_year]  # 将指定年份的数据选择出来
    traces = []
    for i in filtered_df.continent.unique():   # continent 属于哪个洲
        df_by_continent = filtered_df[filtered_df["continent"] == i]  # 将已过滤年份的数据指定大洲再选择
        traces.append(dict(
            x = df_by_continent["gdpPercap"],  # 横纵坐标
            y = df_by_continent["lifeExp"],
            text = df_by_continent["country"],  # 显示国家
            mode = "markers",  # 使用圆点
            opacity = 0.7,  # 透明度设置
            marker = {  # 上面marker的属性设置：大小和外围颜色
                "size":15,  # 圆点大小
                "line":{"width":0.7, "color":"white"}
            },
            name = i   # 左上角的图例信息，将trace改成实际的名字
        ))
    return {   # 返回的内容
        "data":traces,  # 数据必须是列表形式，列表中的元素是字典的键值对
        "layout":dict(
            xaxis = {"type":"log",  # x轴的数据以对数log的形式展现
                     "title":"GDP for Per Captial",
                    "range": [2.3, 4.8]},  # 范围
            yaxis = {"title":"Life Expectancy", "range":[20,90]},
            margin={'l': 40, 'b': 40, 't': 10, 'r': 10},   # 定义图片的上下左右
            legend = {"x":0,"y":1},
            hovermode = "closest",  # 只显示最近点的数据
            transition = {"duration":500},
        )
    }

if __name__ == '__main__':
    app.run_server()
```

#### 参数解释

**下面是各个组件中重要参数的详细解释，辅助理解**

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg5yyrmynmj31690n445v.jpg)





### 利用RangeSlider实现

**RangeSlider的特点是：两个端点都是可以移动的**

#### demo

这里是官网上的demo

```python
import dash
import dash_html_components as html
import dash_core_components as dcc

external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

app = dash.Dash(__name__, external_stylesheets=external_stylesheets)
app.layout = html.Div([
    dcc.RangeSlider(
        id='my-range-slider',
        min=0,
        max=20,
        step=0.5,
        value=[5,10]
    ),
    html.Div(id='output-container-range-slider')
])


@app.callback(
    Output('output-container-range-slider', 'children'),
    [Input('my-range-slider', 'value')])

def update_output(value):
#     return value[0] + value[1]
    return 'You have selected "{}"'.format(value)

if __name__ == '__main__':
    app.run_server()
```



#### 实例

**通过RangeSlider得到的数据是列表形式**，可以通过索引取出每个数据

```python
import dash
import dash_html_components as html
import dash_core_components as dcc

external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']
app = dash.Dash(__name__, external_stylesheets=external_stylesheets)

app.layout = html.Div([
    html.H1("Select Data with RangeSlider", style={"text-align":"center","color":"blue"}),
    dcc.Graph(id="graph-with-rangeslider"),
    dcc.RangeSlider(
        id = "year-rangeslider",
        min = df["year"].min(),
        max = df["year"].max(),
        value = [1952, 1957],  # 范围和初始值
        marks = {str(year):str(year) for year in df["year"].unique()},  # 滑动条下每个年份数字，改成字符型数据
        step = None  # 步长
    ),
#     html.Div(id='output-container-range-slider')
])


@app.callback(
    Output("graph-with-rangeslider","figure"),  # 输出与输入，和上面的名称一一对应
#     Output('output-container-range-slider', 'children'),
    [Input("year-rangeslider","value")]
)


def update_figure(selected_year):  # 生成画图需要的数据

    # 通过索引将指定年份的数据选择出来，再通过concat函数进行合并，得到完整的、用于处理的数据
    filtered_df0 = df[df.year == selected_year[0]]
    filtered_df1 = df[df.year == selected_year[1]]
    filtered_df = pd.concat([filtered_df0,filtered_df1])
#     return px.scatter(filtered_df,x="gdpPercap",y="lifeExp",log_x=True,hover_data=["continent","country"],color="country")

    traces = []

    for i in filtered_df.continent.unique():   # continent 属于哪个洲
        df_by_continent = filtered_df[filtered_df["continent"] == i]  # 将已过滤年份的数据指定大洲再选择
        traces.append(dict(
            x = df_by_continent["gdpPercap"],  # 横纵坐标
            y = df_by_continent["lifeExp"],
            text = df_by_continent["country"],  # 显示国家
            mode = "markers",  # 使用圆点
            opacity = 0.7,  # 透明度设置
            marker = {  # 上面marker的属性设置：大小和外围颜色
                "size":15,  # 圆点大小
                "line":{"width":0.7, "color":"white"}
            },
            name = i   # 左上角的图例信息，将trace改成实际的名字
        ))

    return {   # 返回的内容
        "data":traces,  # 数据必须是列表形式，列表中的元素是字典的键值对
        "layout":dict(
            xaxis = {"type":"log",  # x轴的数据以对数log的形式展现
                     "title":"GDP for Per Captial",
                    "range": [2.3, 4.8]},  # 范围
            yaxis = {"title":"Life Expectancy", "range":[20,90]},
            margin={'l': 40, 'b': 40, 't': 10, 'r': 10},   # 定义图片的上下左右
            legend = {"x":0,"y":1},
            hovermode = "closest",  # 只显示最近点的数据
            transition = {"duration":500},
        )
    }

if __name__ == '__main__':
    app.run_server()
```

效果图如下：

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg5yy8dg57j31h20enjup.jpg)





### 利用px库实现RangeSlider



```python
import dash
import dash_html_components as html
import dash_core_components as dcc

external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']
app = dash.Dash(__name__, external_stylesheets=external_stylesheets)

app.layout = html.Div([
    html.H1("Select Data with RangeSlider", style={"text-align":"center","color":"blue"}),
    dcc.Graph(id="graph-with-rangeslider"),
    dcc.RangeSlider(
        id = "year-rangeslider",
        min = df["year"].min(),
        max = df["year"].max(),
        value = [1952, 1957],  # 范围和初始值
        marks = {str(year):str(year) for year in df["year"].unique()},  # 滑动条下每个年份数字，改成字符型数据
        step = None  # 步长
    ),
#     html.Div(id='output-container-range-slider')
])


@app.callback(
    Output("graph-with-rangeslider","figure"),  # 输出与输入，和上面的名称一一对应
#     Output('output-container-range-slider', 'children'),
    [Input("year-rangeslider","value")]
)


def update_figure(selected_year):  # 生成画图需要的数据

    filtered_df0 = df[df.year == selected_year[0]]  # 将指定年份的数据选择出来
    filtered_df1 = df[df.year == selected_year[1]]
    filtered_df = pd.concat([filtered_df0,filtered_df1])
    return px.scatter(filtered_df,x="gdpPercap",y="lifeExp",log_x=True,hover_data=["continent","country"],color="continent")  # 使用对数，显示多个数据hover_data属性
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg5yx65tnuj31h20flq5j.jpg)















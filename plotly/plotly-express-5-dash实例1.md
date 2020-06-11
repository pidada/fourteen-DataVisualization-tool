---
title: plotly-express-5-dash实例1
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-06-05 14:08:03
password:
summary:
tags:
  - dash
  - 空行
categories:
  - datavisual
  - px
---

本文中介绍的是如何利用dash制作**单个图形+下拉菜单**，主要实现的功能：

- 一级标题文本的居中
- 空行实现
- 下拉菜单的多个参数设置
- 将透视表变成DF数据框

<!--MORE-->

### 导入库和包

```python
import pandas as pd
import plotly_express as px
import plotly.graph_objects as go

import dash
import dash_core_components as dcc
import dash_html_components as html
from dash.dependencies import Input, Output
```



### 显示

```python
# 显示所有列
pd.set_option('display.max_columns', None)

#显示所有行
# pd.set_option('display.max_rows', None)

#设置value的显示长度为100，默认为50
# pd.set_option('max_colwidth',100)
```

### 数据

数据是网上下载的，导入之后具体的形式为：

![image-20200605140236469](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfhdljf98bj31a80nsn1q.jpg)

透视求平均后的结果：

![image-20200605140315689](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfhdm5lbgsj31jc0qetci.jpg)

**将透视表中的数据变成数据框的形式，很巧妙的做法：**

![image-20200605140401669](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfhdmy3plbj31100oigpl.jpg)

### 绘图

```python
app = dash.Dash(__name__)

app.layout = html.Div([

    # 一级标题居中显示
    html.H1("application with Dash", style={"text-align": "center"}),

    # 下拉菜单
    dcc.Dropdown(id="slct_year",
                options=[  # 下拉菜单的全部选项值
                    {"label":'2015',"value":2015},
                    {"label":'2016',"value":2016},
                    {"label":'2017',"value":2017},
                    {"label":'2018',"value":2018}],
                multi=False,  # 不允许多选
                value=2015,  # 默认的初始值
                style={"width":"50%"}
                ),

    html.Div(id='output_container',children=[]),
    html.Br(),   # 空行的实现

    dcc.Graph(id='my_bee_dash',figure={})
])

# 回调函数
@app.callback(  # 输出和输入
    [Output('output_container','children'),Output('my_bee_dash','figure')],
    [Input('slct_year','value')]
)

# 根据选择的year更新图
def update_graph(option_slctd):

    container = "the year chosen by user was:{}".format(option_slctd)

    dff = df.copy()
    dff = dff[dff["Year"] == option_slctd]
    dff = dff[dff["Affected by"] == "Varroa_mites"]

    # px作图
    fig = px.choropleth(
        data_frame = dff,
        locationmode = "USA-states",
        locations = "state_code",
        scope = "usa",
        color = "Pct of Colonies Impacted",
        hover_data = ['State','Pct of Colonies Impacted'],
        color_continuous_scale = px.colors.sequential.YlOrRd,
        labels = {'Pct of Colonies Impacted': '% of Bee Colonies'},
        template='plotly_dark' # 3种风格之一
    )

    return container, fig  # 返回的是容器和图型figure

if __name__ == "__main__":
    app.run_server()
```

### 结果

下拉菜单中选择不同的年份，图形会随着变化；默认是2015年的数据

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfhdotbtp3j31210lljx8.jpg)



下面是2017的图形

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfhdpwa3i3j327a0u0dle.jpg)



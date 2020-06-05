---
title: plotly-express-2-布局Layout
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-05-28 14:51:12
password:
summary:
tags:
  - layout
  - html
  - dash
categories:
  - datavisual
  - px
---


本文主要是介绍了在Dash中如何使用布局Layout。Layout的主要作用是对dash中各个应用的外观进行描述，其包含两个重要部分：

- dash_html_components
- dash_dcc_components

<!--MORE-->

### 什么是dash

> Dash is a productive Python framework for building web applications.
>
> Written on top of Flask, Plotly.js, and React.js, Dash is ideal for building data visualization apps with highly custom user interfaces in pure Python. It's particularly suited for anyone who works with data in Python.

1. Dash是用于构建Web应用程序的高效Python框架
2. 基于Flask、plotly.js和react.js
3. 适合高度自定义且使用纯Python的用户使用

### Dash-布局Layout

> Dash apps are composed of two parts. The first part is the "`layout`" of the app and it describes what the application looks like. The second part describes the interactivity of the application and will be covered in the [next chapter](https://dash.plotly.com/basic-callbacks).

一个Dash应用由两个部分组成：

- layout：布局描述外观
- callback：回调函数描述交互性

> Dash provides Python classes for all of the visual components of the application. We maintain a set of components in the `dash_core_components` and the `dash_html_components` library but you can also [build your own](https://github.com/plotly/dash-component-boilerplate) with JavaScript and React.js.

Dash给应用的各种组件提供了Python的多种类，组件包含：

- dash_core_components，组件
- dash_html_components库
- 自建组件：JS或者React.JS



### 官网demo

#### demo代码

```python
# -*- coding: utf-8 -*-
import dash
import dash_core_components as dcc   # 作图
import dash_html_components as html  # html元素

external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']
app = dash.Dash(__name__, external_stylesheets=external_stylesheets)

app.layout = html.Div(children=[
    html.H1(children='Hello Dash'),  # 大标题
    html.Div(children='''
        Dash: A web application framework for Python.
    '''),  # 容器内的文本

    dcc.Graph(  # 作图
        id='example-graph',  # 图形名称
        # figure里面的元素是字典键值对形式；data作为第一个键，里面的多个元素仍然是键值对形式
        figure={'data': [  # 列表里面嵌套字典形式
            {'x': [1, 2, 3], 'y': [4, 1, 2], 'type': 'bar', 'name': 'SF'},   # 指定数据、作图形状和名称
            {'x': [1, 2, 3], 'y': [2, 4, 5], 'type': 'bar', 'name': u'Montréal'},],
                'layout': {'title': 'Dash Data Visualization'}   # 最里面布局的名称，使用字典形式{}
               }
    )
])

if __name__ == '__main__':
    app.run_server(debug=True)
```

运行之后便可看到如下的动态交互式图形，注意对比上面的各个属性及名称在图中的位置

#### demo图形

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf85zqy5czj30lw0fsgm9.jpg)

#### 特点

1. The `layout` is composed of a tree of "components" like `html.Div` and `dcc.Graph`.

布局是由两种元素组成的树状结构

2. The `dash_html_components` library has a component for every HTML tag. The `html.H1(children='Hello Dash')` component generates a `Hello Dash` HTML element in your application.

dast_html_components是作用于html标签

3. Not all components are pure HTML. The `dash_core_components` describe higher-level components that are interactive and are generated with JavaScript, HTML, and CSS through the React.js library.

dash_core_components是对高级元素的可视化进行描述

4. Each component is described entirely through keyword attributes. Dash is *declarative*: you will primarily describe your application through these attributes.

每个组件通过关键字属性来进行描述，需要事先声明属性

5. The `children` property is special. By convention, it's always the first attribute which means that you can omit it: `html.H1(children='Hello Dash')` is the same as `html.H1('Hello Dash')`. Also, it can contain a string, a number, a single component, or a list of components.

`children`属性比较特殊，通常是第一个，而且是可以忽略的。

### HTML标签

#### 改变各种HTML标签

改变HTML标签的属性

```python
import dash
import dash_core_components as dcc
import dash_html_components as html

external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']
app = dash.Dash(__name__, external_stylesheets=external_stylesheets)

colors = {
    "background": "#111111",
    "text": "#7FDBFF"
}

app.layout = html.Div(style = {"background":colors["background"]},
                     children = [
                         html.H1(children = "Hello dash",
                                 style = {"textAlign": "center","color": colors["text"]}
                                ),
                         html.Div(children = "Dash: a web application framework for python",
                                 style = {"textAlign": "center","color": colors["text"]}
                                 ),
                         dcc.Graph(id = "example-graph-2",
                                  figure = {'data':[{'x': [1, 2, 3], 'y': [4, 1, 2], 'type': 'bar', 'name': 'SF'},
                                                     {'x': [1, 2, 3], 'y': [2, 4, 5], 'type': 'bar', 'name': u'Montréal'},],
                                            "layout":{"plot_bgcolor":colors["background"],
                                                      "paper_bgcolor":colors["background"],
                                                      "font":{"color": colors["text"]}
                                                     }
                                            }
                                  )
                     ])

if __name__ == '__main__':
    app.run_server()
```

运行之后看到的可视化图形

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf8601kil5j316u0f1t9l.jpg)

#### 代码解释

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf85zjwn5hj30rm0nw0t4.jpg)

1. style属性是分号分割的字符串形式，使用**字典**的形式
2. style中的属性是驼峰式的，比如：`text-align`变成`textAlign`
3. HTML中的class属性在dash中是`className`
4. children属性是通过关键字指定的，通常是第一个并且可以忽略
5. figure属性data里面包裹的数据是**列表中包裹的字典键值对形式**：[{k1:v1},{k2:v2}]
6. layout属性中的就是**字典里面包裹**的各个键值对：{k1:v1,k2:v2,....}

最后两点的特点见如下例子：

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf85zhree7j30qg0ibjrg.jpg)

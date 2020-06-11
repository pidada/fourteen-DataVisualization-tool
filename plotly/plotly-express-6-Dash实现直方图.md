---
title: plotly-express-6-Dash实现直方图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-06-08 13:48:53
password:
summary:
tags:
  - 直方图
  - px
  - figure
categories:
  - datavisual
  - px
---

如何利用plotly-express结合Dash实现直方图，最终的效果图
![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfku0snp85j31h50fkabb.jpg)

<!--MORE-->

### 数据

数据是自行模拟的，姓名作为行索引，科目当做属性字段

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({"math":[88,97,83,84],
                  "chinese":[81,72,95,87],
                  "english":[90,85,78,96],
                  "physics":[73,84,83,90]},
                 index=["xiaoming","zhangsan","lisi","zhoujun"])
df
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfku0iigdoj30bt04y747.jpg)

### 坐标系

将每个人姓名作为x轴，每个科目的成绩作为y轴作图

**重点：作图的时候传进来的数据，必须是列表形式**

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfku0jqawmj30bw03rglh.jpg)

### Dash作图

```python
import dash
import dash_core_components as dcc
import dash_html_components as html

external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']
app = dash.Dash(__name__, external_stylesheets=external_stylesheets)

colors = {
    "background": "#252191",
    "text": "#7FDBFF"
}

app.layout = html.Div(style = {"background":colors["background"]},
                     children = [
                         html.H1(children = "Hello dash",
                                 style = {"textAlign": "center","color": colors["text"]}
                                ),        
                         html.Div(children = "Bar with dash",
                                 style = {"textAlign": "center","color": colors["text"]}
                                 ),
                         dcc.Graph(id = "example-graph-2",
                                  figure = {'data':[{'x': df.columns.tolist(), 'y': df.loc["xiaoming"].tolist(), 'type': 'bar', 'name': 'xiaoming'},
                                               {'x': df.columns.tolist(), 'y': df.loc["zhoujun"].tolist(), 'type': 'bar', 'name': u'zhoujun'},
                                               {'x': df.columns.tolist(), 'y': df.loc["lisi"].tolist(), 'type': 'bar', 'name': u'lisi'},
                                               {'x': df.columns.tolist(), 'y': df.loc["zhangsan"].tolist(), 'type': 'bar', 'name': u'zhangsan'},
                                                    
                                                   ],
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


1. 指定属性colors，后面可以直接调用
2. html组件中的第一个属性都是children，可以省略不写
3. 作图的时候，figure包含**data和layout**两个属性
   1. data的取值是一个列表形式，里面的真实数据是字典
   2. layout的取值是{}包裹起来的键值对

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfku0o33imj30u00uk7cd.jpg)



### 结果

图形是动态交互式的

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfku0snp85j31h50fkabb.jpg)

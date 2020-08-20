---
title: plotly-express-21-theme-and-FigureWidget
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-08-01 20:40:50
password:
summary:
tags:
  - 可视化
categories:
  - datavisual
  - px
---


### plotly-express-21-theme and Figurewidget

本文中介绍了`Plotly`中的多种图形主题的效果，自己一般用的都是默认的主题；`FigureWidget`是一个比较好的功能，可以往空白图中依次添加图形，图形也是叠加出现。

如果是`Figure`的话，是先把数据处理号，再添加进入后一次性生成全部的图形，`FigureWidget`是逐个加入。

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=584155&auto=1&height=66"></iframe>

<!--MORE-->

### 不同的主题

#### 主题

主要的主题包含下面几种：

```python
import plotly.io as pio
pio.templates


Templates configuration
-----------------------
    Default template: 'plotly'
    Available templates:
        ['ggplot2', 'seaborn', 'simple_white', 'plotly',
         'plotly_white', 'plotly_dark', 'presentation', 'xgridoff',
         'ygridoff', 'gridon', 'none']
```

#### 实例

```python
import plotly.express as px

df = px.data.gapminder()
df_2007 = df.query("year==2007")

for template in ["plotly", "plotly_white", "plotly_dark", "ggplot2", "seaborn", "simple_white", "none"]:
    fig = px.scatter(df_2007,
                     x="gdpPercap", y="lifeExp", size="pop", color="continent",
                     log_x=True, size_max=60,
                     template=template, title="Gapminder 2007: '%s' theme" % template)
    fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlly1ghamykhpgyj31nm0ritdn.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlly1ghamz9uy4vj30jg0ciabp.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlly1ghan00arczj31lw0piwip.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlly1ghan0btsnbj31ny0pejvp.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlly1ghan0sjv2kj31na0q477y.jpg)







### FigureWidget

#### 原理

`FigureWidget`是最近发现的Plotly的功能，其功能可以概括为：conf生成的空白图开始，添加给定数据，再生成需要的图形

- 生成一个空白图
- 添加数据，生成指定的图形
- 修改数据，更新图形

#### demo

##### 生成空白的图形

![image-20200801202440132](https://tva1.sinaimg.cn/large/007S8ZIlly1ghbkylpdi0j31i20nwta5.jpg)

##### 添加折线图

![](https://tva1.sinaimg.cn/large/007S8ZIlly1ghbkzx7sswj31im0skjur.jpg)

##### 添加柱状图

![](https://tva1.sinaimg.cn/large/007S8ZIlly1ghbl0z85awj31j80u077g.jpg)

##### 添加标题

![image-20200801202817382](https://tva1.sinaimg.cn/large/007S8ZIlly1ghbl2clbq8j31hm0sadj5.jpg)

##### 更新数据

![](https://tva1.sinaimg.cn/large/007S8ZIlly1ghbl77r6c1j31hy0twwiz.jpg)

![image-20200801203341078](https://tva1.sinaimg.cn/large/007S8ZIlly1ghbl7zd18rj31i00u0aez.jpg)

![image-20200801203559673](https://tva1.sinaimg.cn/large/007S8ZIlly1ghbladvcboj313u0seaeo.jpg)

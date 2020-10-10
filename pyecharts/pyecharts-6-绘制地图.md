---
title: pyecharts-6-绘制地图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-10-10 15:12:30
password:
summary:
tags:
  - pyecharts
  - 可视化
  - pandas
categories:
  - datavisual
  - pyecharts
---

### Pyecharts-6-绘制地理图

本文中介绍的是如何利用`pyecharts`绘制地理图形 ，学习的资料主要是来自[官网](https://gallery.pyecharts.org/#/Map/README)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjk9ag5s58j31400u07wi.jpg)

<!--MORE-->



### 导入库

```python
from pyecharts import options as opts
from pyecharts.charts import Map, Geo, Map3D
from pyecharts.faker import Faker
from pyecharts.commons.utils import JsCode
from pyecharts.globals import ThemeType, ChartType

import pandas as pd
import numpy as np
```

### 绘制基本图形-全国

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjk8s9s585j31d20g6dip.jpg)

效果图（实际是动态的）

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjk8t12uw6j315a0pgwju.jpg)

### 广东省数据

当绘制具体某个省份的地图时候，需要在地级市后面加上一个$\color{red}{市}$，否则不能出图：

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjk8uy5dznj31cg0gun02.jpg)

效果图为：

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjk8vnf3xej31600s2dl6.jpg)

如果某个地级市名字中少了市字，则它的数据则不会显示：

```python
# 少了“市”字的地区则不显示

guangdong = ['汕头', '汕尾', '揭阳市', '阳江', '肇庆', '广州', '惠州市']

c = (
    Map()
    .add("商家A", [list(z) for z in zip(guangdong, Faker.values())], "广东")
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Map-广东地图"), visualmap_opts=opts.VisualMapOpts()
    )
)

c.render_notebook()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjk8xurbv2j315m0s4n2t.jpg)

全部加上市字，则会显示：

```python
# 全部加上市字

guangdong = ['深圳市', '汕尾市', '揭阳市', '东莞市', '肇庆市', '广州市', '惠州市']

c = (
    Map()
    .add("商家A", [list(z) for z in zip(guangdong, Faker.values())], "广东")
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Map-广东地图"), visualmap_opts=opts.VisualMapOpts()
    )
)

c.render_notebook()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjk8zgkne5j313s0s0dlt.jpg)

### 数值分段

对实际数据进行分段显示，并且用不同的颜色进行标注

```python
c = (
    Map()
    .add("商家A", [list(z) for z in zip(Faker.provinces, Faker.values())], "china")
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Map地图-基本示例"),
        visualmap_opts=opts.VisualMapOpts(max_=200, is_piecewise=True)  # 分段通过 is_piecewise=True 实现
    )
)

c.render_notebook()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjk93pupu9j31du0sgqav.jpg)

不分段的效果：`is_piecewise=False`

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjk94bp30nj319i0pggrw.jpg)

### 标签是否显示

不显示标签：

```python
c = (
    Map()
    .add("商家A", [list(z) for z in zip(Faker.provinces, Faker.values())], "china")
    .set_series_opts(label_opts=opts.LabelOpts(is_show=False))
    .set_global_opts(title_opts=opts.TitleOpts(title="Map-不显示Label"))
)

c.render_notebook()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjk95rvh99j31ak0o8aeq.jpg)

显示标签的效果

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjk96ap7iyj31b80qsdm6.jpg)

### 全国行政区域图

```python
c = (
    Map3D()
    .add_schema(
        itemstyle_opts=opts.ItemStyleOpts(
            color="rgb(5,101,123)",
            opacity=1,
            border_width=0.8,
            border_color="rgb(62,215,213)",
        ),
        map3d_label=opts.Map3DLabelOpts(
            is_show=True,
            text_style=opts.TextStyleOpts(
                color="#fff", font_size=16, background_color="rgba(0,0,0,0)"
            ),
        ),
        emphasis_label_opts=opts.LabelOpts(is_show=True),
        light_opts=opts.Map3DLightOpts(
            main_color="#fff",
            main_intensity=1.2,
            is_main_shadow=False,
            main_alpha=55,
            main_beta=10,
            ambient_intensity=0.3,
        ),
    )
    .add(series_name="", data_pair="", maptype=ChartType.MAP3D)
    .set_global_opts(
        title_opts=opts.TitleOpts(title="全国行政区划地图-Base"),
        visualmap_opts=opts.VisualMapOpts(is_show=False),
        tooltip_opts=opts.TooltipOpts(is_show=True),
    )
)

c.render_notebook()
```

3D效果图如下：

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjk97l1zhfj317o0mw13s.jpg)

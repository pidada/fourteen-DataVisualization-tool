---
title: pyecharts-3-绘制K线图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-09-02 00:24:59
password:
summary:
tags:
  - 可视化
  - px
categories:
  - datavisual
  - pyecharts
---

### pyecharts-3-绘制K线图

本文中记录一次利用`pyecharts`绘制`K`线图。最近从朋友那边获取到一组关于`stock`的数据，于是抽空画了一下K线图，熟悉`pyecharts`中K线图的画法

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1giblio19gsj31400u0dkt.jpg)

<!--MORE-->



### 什么是K线

引用一段来自维基百科的解释：

> **K线**（Candlestick chart）又称“阴阳烛”、“蜡烛线”，是反映[价格](https://zh.wikipedia.org/wiki/价格)走势的一种图线，其特色在于一个线段内记录了多项讯息，相当易读易懂且实用有效，广泛用于[股票](https://zh.wikipedia.org/wiki/股票)、[期货](https://zh.wikipedia.org/wiki/期貨)、[贵金属](https://zh.wikipedia.org/wiki/貴金屬)、[数字货币](https://zh.wikipedia.org/wiki/数字货币)等行情的[技术分析](https://zh.wikipedia.org/wiki/技术分析)，称为**K线分析**。
>
> 据传K线为[日本](https://zh.wikipedia.org/wiki/日本)[江户时代](https://zh.wikipedia.org/wiki/江户时代)的[白米](https://zh.wikipedia.org/wiki/白米)商人[本间宗久](https://zh.wikipedia.org/wiki/本間宗久)所发明，用来记录每日的米市行情，研析[期货](https://zh.wikipedia.org/wiki/期貨)市场。日语中K线称为“蜡烛足（日语：ローソク足）”。
>
> 蜡烛线的英文是candlestick chart，日文[片假名](https://zh.wikipedia.org/wiki/片假名)为キャンドル スティック（**K**YANDORU SUTIKKU），故中文也称之为**K**线。

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1giblhsnxwoj30go0f2wf5.jpg)

**自己的理解就是根据每个股每天的：开盘价、最低价、最高价和收盘价绘制的一种走势图线，从中找出个股的规律**



### pyecharts格式

官网上数据的格式：

- 每天的数据在一个列表
- 全部的数据组成一个新的大列表

```mysql
from pyecharts import options as opts
from pyecharts.charts import Kline

data = [
    [2320.26, 2320.26, 2287.3, 2362.94],
    [2300, 2291.3, 2288.26, 2308.38],
    [2295.35, 2346.5, 2295.35, 2345.92],
    [2347.22, 2358.98, 2337.35, 2363.8],
    [2360.75, 2382.48, 2347.89, 2383.76],
]

c = (
    Kline()
    .add_xaxis(["2017/7/{}".format(i + 1) for i in range(31)])
    .add_yaxis("kline", data)
    .set_global_opts(
        xaxis_opts=opts.AxisOpts(is_scale=True),
        yaxis_opts=opts.AxisOpts(
            is_scale=True,
            splitarea_opts=opts.SplitAreaOpts(
                is_show=True, areastyle_opts=opts.AreaStyleOpts(opacity=1)
            ),
        ),
        datazoom_opts=[opts.DataZoomOpts(type_="inside")],
        title_opts=opts.TitleOpts(title="Kline-DataZoom-inside"),
    )
    .render("kline_datazoom_inside.html")
)
点击复制 (Click to Copy)错误 (Error)复制 (Copy)

```



### 导入库

```mysql
import pymysql   # 连接数据库
import pandas as pd
import numpy as np
from pyecharts.charts import Kline,Line,Bar,Grid   -- 绘图
from datetime import datetime   #  处理时间
```

### 获取数据

![image-20200902000319583](https://tva1.sinaimg.cn/large/007S8ZIlgy1giblhnx5bgj30us0o0jtw.jpg)

### 原始数据

```mysql
data = []

for i in cur.fetchall():
    data.append(i)

df_stock = pd.DataFrame(data,columns=['ts_code','trade_date','open','close','low','high'])
df_stock
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1giblkd57tej30v80bc0u4.jpg)

### 时间转化

在pyecharts中绘制K线图的时候，时间格式使用的是`年-月-日`的格式，所以需要先对上面的数据进行处理。

上面的数据只是字符串类型：

- 使用to_datetime()方法转化成时间类型的数据，format参数指定我们想要的格式
- 通过匿名函数将上一步的时间数据再转成最终的时间格式数据

```python
df_stock['trade_date'] = pd.to_datetime(df_stock['trade_date'], format ='%Y-%m-%d')

df_stock['trade_date'] = df_stock['trade_date'].apply(lambda x: x.strftime('%Y-%m-%d'))
```

### 划分代码和证券交易所

将股票代码和证券交易所的代号分开

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1giblq4jcf7j30r60as0u2.jpg)

使用**匿名函数和分割函数**进行处理，取出相应的数据，增加`code`和`exchange`两个字段

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1giblrvg3ljj31620dsgog.jpg)

### 个股数据量

查看每个股在规定时间范围内的数据量

```mysql
stock_list = df_stock['code'].value_counts().reset_index().rename(columns={'index':'code','code':'number'})
stock_list
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1giblzdd20jj30h40lc0u8.jpg)

### 000001-demo

以深证的000001股票为例绘制K线图，下图为数据量：

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gibm18ofesj310e0nqgpc.jpg)

指定在某个时间段内绘制：

```mysql
# 使用&，不要用and

df_kline = kline[(kline['trade_date'] >= '2020-01-10') & (kline['trade_date'] <= '2020-02-04')]
df_kline
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gibm2n2kvyj31060kedja.jpg)

### 生成列表类型数据

将`open、close、low、high`的4个数据放在一个列表，代表一天的完整数据，再将每天的数据组成新的大列表。

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gibm7yltcxj31ik0eatc5.jpg)

### 绘图

将上面得到的完整数据导入官网的代码示例中：

```python
c = (
    Kline()
    .add_xaxis(df_kline['trade_date'].tolist())
    .add_yaxis("kline", data_list)
    .set_global_opts(
        xaxis_opts=opts.AxisOpts(is_scale=True),
        yaxis_opts=opts.AxisOpts(
            is_scale=True,
            splitarea_opts=opts.SplitAreaOpts(
                is_show=True, areastyle_opts=opts.AreaStyleOpts(opacity=1)
            ),
        ),
        title_opts=opts.TitleOpts(title="K线走势图"),
    )
)

c.render_notebook()
```

结果如下图：

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gibm9l0342j31jk0qumyq.jpg)

---
title: plotly-express-11-plotly实现柱状图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-06-29 17:02:52
password:
summary:
tags:
  - px
  - pandas
  - 可视化
categories:
  - datavisual
  - px
---



本文中介绍的是如何在`plotly`中绘制柱状图`Bar`

- 基于`px.bar`
- 基于`go.Bar`

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99kmj65uj30ak086aa8.jpg)

<!--MORE-->

### 导入库

```python
import pandas as pd
import numpy as np

import plotly_express as px
import plotly.graph_objects as go

import dash
import dash_core_components as dcc
import dash_html_components as html
```



### 基于px.bar实现

With `px.bar`, each row of the DataFrame is represented as a rectangular mark.

#### basic

```python
data_canada = px.data.gapminder().query("country == 'Canada'")
fig = px.bar(data_canada, x="year", y="pop")
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99kskg13j30qd0eo74q.jpg)

#### 自定义customize

```python
fig = px.bar(data_canada, x="year",y="pop",
             color="lifeExp",height=400,
             hover_data=["lifeExp","gdpPercap"],
             labels={"pop":"population of Canada"}  # 将y轴的名称pop改掉
            )
fig.show()
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99kwv7fdj30sw0dujs9.jpg)

#### 堆叠柱状图

When several rows share the same value of `x` (here Female or Male), the rectangles are stacked on top of one another by default.

通过堆叠的方式展示

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99mqtkhjj30tf0ftjtw.jpg)

通过分组的方式展示

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99l49hy9j30t60cr758.jpg)

#### facetted subplots

```python
fig = px.bar(df, x="sex",y="total_bill",
             color="smoker",barmode="group",
             facet_col="day",   #  in the horizontal direction 水平方向
             facet_row="time",  #  in the vertical direction  竖直方向
             category_orders={"day":["Thur","Fri","Sat","Sun"],  # 2个方向的具体分类情况
                             "time":["Lunch","Dinner"]})
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99l7hrd7j30rv0e0aaq.jpg)



### 基于go.Bar实现

#### basic

```python
animals = ["cat","dog","pig","chicken","monkeys"]
values = [20,50,40,60,30]

fig = go.Figure(data=(go.Bar(x=animals,y=values)))
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99lb98koj30og0b5gls.jpg)



#### grouped bar chart

```python
animals = ["cat","dog","pig","chicken","monkeys"]
values_1 = [20,50,40,60,40]
values_2 = [40,70,20,50,20]

fig = go.Figure(data=[
    go.Bar(x=animals,y=values_1,name="shenzhen Zoo"),  #  多组数据用列表的形式
    go.Bar(x=animals,y=values_2,name="guangzhou Zoo"),
])

# change the bar mode：更新柱状图的mode
fig.update_layout(barmode="group")   # 分组的形式！！！

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99lfhqmsj30th0kcq3y.jpg)



#### stack bar chart

```python
animals = ["cat","dog","pig","chicken","monkeys"]
values_1 = [20,50,40,60,40]
values_2 = [40,70,20,50,20]

fig = go.Figure(data=[
    go.Bar(x=animals,y=values_1,name="shenzhen Zoo"),  #  多组数据用列表的形式
    go.Bar(x=animals,y=values_2,name="guangzhou Zoo"),
])

# change the bar mode：更新柱状图的mode
fig.update_layout(barmode="stack")   # 堆叠图的形式

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99lkct34j30qg0c474k.jpg)



### Bar Chart with Hover Text

- 悬停文字信息的显示
- x轴标签的旋转
- y轴上文本信息的显示和位置显示（auto/outside/inside）

```python
x = ['Product A', 'Product B', 'Product C']
y = [20, 14, 23]

# Use the hovertext kw argument for hover text
fig = go.Figure(data=[go.Bar(x=x, y=y,
                             text=y,textposition="auto",  # 矩形框中显示的文本text和文本的位置textposition
                             hovertext=['27% market share', '24% market share', '19% market share']) # 悬停显示文本
                     ])

# update_traces
fig.update_traces(marker_color='rgb(1,202,225)',
                  marker_line_color='rgb(108,10,0)',  # 边框线条颜色
                  marker_line_width=1.5,   # 矩形边框的线条粗细
                  opacity=0.8
                 )

# 图表标题
fig.update_layout(title_text='January 2013 Sales Report',xaxis_tickangle=-45) # 标题 + x轴标签旋转45度
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99loux2jj30r90f4jrw.jpg)



### Controlling text fontsize with uniformtext

```python
df = px.data.gapminder().query("continent == 'Europe' and year == 2007 and pop > 2.e6")
fig = px.bar(df, x="country", y="pop", text="pop")

fig.update_traces(texttemplate="%{text:.2s}",  # 数字保留2位有效数字
                  textposition="outside")  # 数字显示在外面
fig.update_layout(uniformtext_minsize=10, uniformtext_mode="hide")

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99na5v6tj30qf0cimyp.jpg)



### Customizing Individual Bar Widths

如何自定义矩形框的宽度

```python
fig = go.Figure(data=[go.Bar(
    x=[1, 2, 3, 5.5, 10],
    y=[10, 8, 6, 4, 2],
    width=[0.8, 0.8, 0.8, 3.5, 4] # 指定每个矩形框的宽度
)])

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99lxt0nzj30po0ca3yo.jpg)



### Bar Chart with Relative Barmode

With "relative" barmode, the bars are stacked on top of one another, with negative values below the axis, positive values above.

通过barmode参数的relative来设置

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99m0k42zj30t90igmxy.jpg)

### Bar Chart with Sorted or Ordered Categories

Set `categoryorder` to `"category ascending"` or `"category descending"` for the alphanumerical order of the category names or `"total ascending"` or `"total descending"` for numerical order of values. [categoryorder](https://plotly.com/python/reference/#layout-xaxis-categoryorder) for more information.

Note that sorting the bars by a particular trace isn't possible right now - it's only possible to sort by the total values. Of course, you can always sort your data *before* plotting it if you need more customization.

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99m67hv6j30t20i3aar.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg99mca1s6j30sh0hhmxt.jpg)



### 参数

- **data_frame** (*DataFrame* *or* *array-like* *or* [*dict*](https://docs.python.org/3/library/stdtypes.html#dict)) – This argument needs to be passed for column names (and not keyword names) to be used. Array-like and dict are tranformed internally to a pandas DataFrame. Optional: if missing, a DataFrame gets constructed under the hood using the other arguments.
- **x** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*int*](https://docs.python.org/3/library/functions.html#int) *or* *Series* *or* *array-like*) – Either a name of a column in `data_frame`, or a pandas Series or array_like object. Values from this column or array_like are used to position marks along the x axis in cartesian coordinates. Either `x` or `y` can optionally be a list of column references or array_likes, in which case the data will be treated as if it were ‘wide’ rather than ‘long’.
- **y** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*int*](https://docs.python.org/3/library/functions.html#int) *or* *Series* *or* *array-like*) – Either a name of a column in `data_frame`, or a pandas Series or array_like object. Values from this column or array_like are used to position marks along the y axis in cartesian coordinates. Either `x` or `y` can optionally be a list of column references or array_likes, in which case the data will be treated as if it were ‘wide’ rather than ‘long’.
- **color** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*int*](https://docs.python.org/3/library/functions.html#int) *or* *Series* *or* *array-like*) – Either a name of a column in `data_frame`, or a pandas Series or array_like object. Values from this column or array_like are used to assign color to marks.
- **facet_row** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*int*](https://docs.python.org/3/library/functions.html#int) *or* *Series* *or* *array-like*) – Either a name of a column in `data_frame`, or a pandas Series or array_like object. Values from this column or array_like are used to assign marks to facetted subplots in the vertical direction.
- **facet_col** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*int*](https://docs.python.org/3/library/functions.html#int) *or* *Series* *or* *array-like*) – Either a name of a column in `data_frame`, or a pandas Series or array_like object. Values from this column or array_like are used to assign marks to facetted subplots in the horizontal direction.
- **facet_col_wrap** ([*int*](https://docs.python.org/3/library/functions.html#int)) – Maximum number of facet columns. Wraps the column variable at this width, so that the column facets span multiple rows. Ignored if 0, and forced to 0 if `facet_row` or a `marginal` is set.
- **hover_name** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*int*](https://docs.python.org/3/library/functions.html#int) *or* *Series* *or* *array-like*) – Either a name of a column in `data_frame`, or a pandas Series or array_like object. Values from this column or array_like appear in bold in the hover tooltip.
- **hover_data** (*list of str* *or* [*int*](https://docs.python.org/3/library/functions.html#int)*, or* *Series* *or* *array-like**, or* [*dict*](https://docs.python.org/3/library/stdtypes.html#dict)) – Either a list of names of columns in `data_frame`, or pandas Series, or array_like objects or a dict with column names as keys, with values True (for default formatting) False (in order to remove this column from hover information), or a formatting string, for example ‘:.3f’ or ‘[|](https://plotly.com/python-api-reference/generated/plotly.express.bar.html#id1)%a’ or list-like data to appear in the hover tooltip or tuples with a bool or formatting string as first element, and list-like data to appear in hover as second element Values from these columns appear as extra data in the hover tooltip.
- **custom_data** (*list of str* *or* [*int*](https://docs.python.org/3/library/functions.html#int)*, or* *Series* *or* *array-like*) – Either names of columns in `data_frame`, or pandas Series, or array_like objects Values from these columns are extra data, to be used in widgets or Dash callbacks for example. This data is not user-visible but is included in events emitted by the figure (lasso selection etc.)
- **text** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*int*](https://docs.python.org/3/library/functions.html#int) *or* *Series* *or* *array-like*) – Either a name of a column in `data_frame`, or a pandas Series or array_like object. Values from this column or array_like appear in the figure as text labels.
- **error_x** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*int*](https://docs.python.org/3/library/functions.html#int) *or* *Series* *or* *array-like*) – Either a name of a column in `data_frame`, or a pandas Series or array_like object. Values from this column or array_like are used to size x-axis error bars. If `error_x_minus` is `None`, error bars will be symmetrical, otherwise `error_x` is used for the positive direction only.
- **error_x_minus** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*int*](https://docs.python.org/3/library/functions.html#int) *or* *Series* *or* *array-like*) – Either a name of a column in `data_frame`, or a pandas Series or array_like object. Values from this column or array_like are used to size x-axis error bars in the negative direction. Ignored if `error_x` is `None`.
- **error_y** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*int*](https://docs.python.org/3/library/functions.html#int) *or* *Series* *or* *array-like*) – Either a name of a column in `data_frame`, or a pandas Series or array_like object. Values from this column or array_like are used to size y-axis error bars. If `error_y_minus` is `None`, error bars will be symmetrical, otherwise `error_y` is used for the positive direction only.
- **error_y_minus** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*int*](https://docs.python.org/3/library/functions.html#int) *or* *Series* *or* *array-like*) – Either a name of a column in `data_frame`, or a pandas Series or array_like object. Values from this column or array_like are used to size y-axis error bars in the negative direction. Ignored if `error_y` is `None`.
- **animation_frame** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*int*](https://docs.python.org/3/library/functions.html#int) *or* *Series* *or* *array-like*) – Either a name of a column in `data_frame`, or a pandas Series or array_like object. Values from this column or array_like are used to assign marks to animation frames.
- **animation_group** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*int*](https://docs.python.org/3/library/functions.html#int) *or* *Series* *or* *array-like*) – Either a name of a column in `data_frame`, or a pandas Series or array_like object. Values from this column or array_like are used to provide object-constancy across animation frames: rows with matching [`](https://plotly.com/python-api-reference/generated/plotly.express.bar.html#id3)animation_group`s will be treated as if they describe the same object in each frame.
- **category_orders** (dict with str keys and list of str values (default `{}`)) – By default, in Python 3.6+, the order of categorical values in axes, legends and facets depends on the order in which these values are first encountered in `data_frame` (and no order is guaranteed by default in Python below 3.6). This parameter is used to force a specific ordering of values per column. The keys of this dict should correspond to column names, and the values should be lists of strings corresponding to the specific display order desired.
- **labels** (dict with str keys and str values (default `{}`)) – By default, column names are used in the figure for axis titles, legend entries and hovers. This parameter allows this to be overridden. The keys of this dict should correspond to column names, and the values should correspond to the desired label to be displayed.
- **color_discrete_sequence** (*list of str*) – Strings should define valid CSS-colors. When `color` is set and the values in the corresponding column are not numeric, values in that column are assigned colors by cycling through `color_discrete_sequence` in the order described in `category_orders`, unless the value of `color` is a key in `color_discrete_map`. Various useful color sequences are available in the `plotly.express.colors` submodules, specifically `plotly.express.colors.qualitative`.
- **color_discrete_map** (dict with str keys and str values (default `{}`)) – String values should define valid CSS-colors Used to override `color_discrete_sequence` to assign a specific colors to marks corresponding with specific values. Keys in `color_discrete_map` should be values in the column denoted by `color`. Alternatively, if the values of `color` are valid colors, the string `'identity'` may be passed to cause them to be used directly.
- **color_continuous_scale** (*list of str*) – Strings should define valid CSS-colors This list is used to build a continuous color scale when the column denoted by `color` contains numeric data. Various useful color scales are available in the `plotly.express.colors` submodules, specifically `plotly.express.colors.sequential`, `plotly.express.colors.diverging` and `plotly.express.colors.cyclical`.
- **range_color** (*list of two numbers*) – If provided, overrides auto-scaling on the continuous color scale.
- **color_continuous_midpoint** (number (default `None`)) – If set, computes the bounds of the continuous color scale to have the desired midpoint. Setting this value is recommended when using `plotly.express.colors.diverging` color scales as the inputs to `color_continuous_scale`.
- **opacity** ([*float*](https://docs.python.org/3/library/functions.html#float)) – Value between 0 and 1. Sets the opacity for markers.
- **orientation** (str, one of `'h'` for horizontal or `'v'` for vertical.) – (default `'v'` if `x` and `y` are provided and both continous or both categorical, otherwise `'v'`(`‘h’`) if `x`(`y`) is categorical and `y`(`x`) is continuous, otherwise `'v'`(`‘h’`) if only `x`(`y`) is provided)
- **barmode** (str (default `'relative'`)) – One of `'group'`, `'overlay'` or `'relative'` In `'relative'` mode, bars are stacked above zero for positive values and below zero for negative values. In `'overlay'` mode, bars are drawn on top of one another. In `'group'` mode, bars are placed beside each other.
- **log_x** (boolean (default `False`)) – If `True`, the x-axis is log-scaled in cartesian coordinates.
- **log_y** (boolean (default `False`)) – If `True`, the y-axis is log-scaled in cartesian coordinates.
- **range_x** (*list of two numbers*) – If provided, overrides auto-scaling on the x-axis in cartesian coordinates.
- **range_y** (*list of two numbers*) – If provided, overrides auto-scaling on the y-axis in cartesian coordinates.
- **title** ([*str*](https://docs.python.org/3/library/stdtypes.html#str)) – The figure title.
- **template** ([*str*](https://docs.python.org/3/library/stdtypes.html#str) *or* [*dict*](https://docs.python.org/3/library/stdtypes.html#dict) *or* *plotly.graph_objects.layout.Template instance*) – The figure template name (must be a key in plotly.io.templates) or definition.
- **width** (int (default `None`)) – The figure width in pixels.
- **height** (int (default `None`)) – The figure height in pixels.

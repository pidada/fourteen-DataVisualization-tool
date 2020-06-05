---
title: plotly_express-4-常见绘图参数
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-06-04 23:32:20
password:
summary:
tags:
  - 参数
categories:
  - datavisual
  - px
---



本文中介绍了几种常见的利用`plotly_express`作图方法的参数

- scatter
- scatter_geo
- line
- line_polar
- area
- bar
- bar_polar
- violin
- histogram
- pie
- choropleth
- density_heatmap

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfgoea9ye6j304r01ujrc.jpg)

<!--MORE-->

### scatter-散点图

In a scatter plot, each row of `data_frame` is represented by a symbol mark in 2D space.

```python
px.scatter(data_frame=None, x=None, y=None, color=None, symbol=None, size=None, hover_name=None, hover_data=None, custom_data=None, text=None, facet_row=None, facet_col=None, facet_col_wrap=0, error_x=None, error_x_minus=None, error_y=None, error_y_minus=None, animation_frame=None, animation_group=None, category_orders={}, labels={}, orientation=None, color_discrete_sequence=None, color_discrete_map={}, color_continuous_scale=None, range_color=None, color_continuous_midpoint=None, symbol_sequence=None, symbol_map={}, opacity=None, size_max=None, marginal_x=None, marginal_y=None, trendline=None, trendline_color_override=None, log_x=False, log_y=False, range_x=None, range_y=None, render_mode='auto', title=None, template=None, width=None, height=None)
```

### scatter_geo-基于地图的散点图

> In a geographic scatter plot, each row of `data_frame` is represented by a symbol mark on a map.

```python
px.scatter_geo(data_frame=None, lat=None, lon=None, locations=None, locationmode=None, color=None, text=None, hover_name=None, hover_data=None, custom_data=None, size=None, animation_frame=None, animation_group=None, category_orders={}, labels={}, color_discrete_sequence=None, color_discrete_map={}, color_continuous_scale=None, range_color=None, color_continuous_midpoint=None, opacity=None, size_max=None, projection=None, scope=None, center=None, title=None, template=None, width=None, height=None)
```

### line-2维线形图

In a 2D line plot, each row of `data_frame` is represented as vertex of a polyline mark in 2D space.

```python
px.line(data_frame=None, x=None, y=None, line_group=None, color=None, line_dash=None, hover_name=None, hover_data=None, custom_data=None, text=None, facet_row=None, facet_col=None, facet_col_wrap=0, error_x=None, error_x_minus=None, error_y=None, error_y_minus=None, animation_frame=None, animation_group=None, category_orders={}, labels={}, orientation=None, color_discrete_sequence=None, color_discrete_map={}, line_dash_sequence=None, line_dash_map={}, log_x=False, log_y=False, range_x=None, range_y=None, line_shape=None, render_mode='auto', title=None, template=None, width=None, height=None)
```

### line_polar-线性极坐标图

In a polar line plot, each row of data_frame is represented as vertex of a polyline mark in polar coordinates.

```python
px.line_polar(data_frame=None, r=None, theta=None, color=None, line_dash=None, hover_name=None, hover_data=None, custom_data=None, line_group=None, text=None, animation_frame=None, animation_group=None, category_orders={}, labels={}, color_discrete_sequence=None, color_discrete_map={}, line_dash_sequence=None, line_dash_map={}, direction='clockwise', start_angle=90, line_close=False, line_shape=None, render_mode='auto', range_r=None, range_theta=None, log_r=False, title=None, template=None, width=None, height=None)
```

### area-面积图

In a stacked area plot, each row of data_frame is represented as vertex of a polyline mark in 2D space. The area between successive polylines is filled.

在堆叠的面积图形中，每行的DF数据代表多边形的最高点。

```python
px.area(data_frame=None, x=None, y=None, line_group=None, color=None, hover_name=None, hover_data=None, custom_data=None, text=None, facet_row=None, facet_col=None, facet_col_wrap=0, animation_frame=None, animation_group=None, category_orders={}, labels={}, color_discrete_sequence=None, color_discrete_map={}, orientation=None, groupnorm=None, log_x=False, log_y=False, range_x=None, range_y=None, line_shape=None, title=None, template=None, width=None, height=None)
```

### bar-柱状图

In a bar plot, each row of data_frame is represented as a rectangular mark.

在柱状图中，每行的DF数据代表一个矩形

```python
px.bar(data_frame=None, x=None, y=None, color=None, facet_row=None, facet_col=None, facet_col_wrap=0, hover_name=None, hover_data=None, custom_data=None, text=None, error_x=None, error_x_minus=None, error_y=None, error_y_minus=None, animation_frame=None, animation_group=None, category_orders={}, labels={}, color_discrete_sequence=None, color_discrete_map={}, color_continuous_scale=None, range_color=None, color_continuous_midpoint=None, opacity=None, orientation=None, barmode='relative', log_x=False, log_y=False, range_x=None, range_y=None, title=None, template=None, width=None, height=None)
```

### bar_polar-柱状极坐标图

In a polar bar plot, each row of data_frame is represented as a wedge（楔形） mark in polar coordinates.

```python
px.bar_polar(data_frame=None, r=None, theta=None, color=None, hover_name=None, hover_data=None, custom_data=None, animation_frame=None, animation_group=None, category_orders={}, labels={}, color_discrete_sequence=None, color_discrete_map={}, color_continuous_scale=None, range_color=None, color_continuous_midpoint=None, barnorm=None, barmode='relative', direction='clockwise', start_angle=90, range_r=None, range_theta=None, log_r=False, title=None, template=None, width=None, height=None)¶
```

### violin-小提琴图

In a violin plot, rows of data_frame are grouped together into a curved（弯曲的） mark to visualize their distribution（分布）

```python
px.violin(data_frame=None, x=None, y=None, color=None, facet_row=None, facet_col=None, facet_col_wrap=0, hover_name=None, hover_data=None, custom_data=None, animation_frame=None, animation_group=None, category_orders={}, labels={}, color_discrete_sequence=None, color_discrete_map={}, orientation=None, violinmode=None, log_x=False, log_y=False, range_x=None, range_y=None, points=None, box=False, title=None, template=None, width=None, height=None)
```

### histogram-矩形图

In a histogram, rows of data_frame are grouped together into a rectangular mark to visualize the 1D distribution of an aggregate function histfunc (e.g. the count or sum) of the value y (or x if orientation is 'h').

```python
px.histogram(data_frame=None, x=None, y=None, color=None, facet_row=None, facet_col=None, facet_col_wrap=0, hover_name=None, hover_data=None, animation_frame=None, animation_group=None, category_orders={}, labels={}, color_discrete_sequence=None, color_discrete_map={}, marginal=None, opacity=None, orientation=None, barmode='relative', barnorm=None, histnorm=None, log_x=False, log_y=False, range_x=None, range_y=None, histfunc=None, cumulative=None, nbins=None, title=None, template=None, width=None, height=None)
```

### pie

In a pie plot, each row of data_frame is represented as a sector of a pie

```python
plotly.express.pie(data_frame=None, names=None, values=None, color=None, color_discrete_sequence=None, color_discrete_map={}, hover_name=None, hover_data=None, custom_data=None, labels={}, title=None, template=None, width=None, height=None, opacity=None, hole=None)
```



### choropleth-等值线图

In a choropleth map, each row of data_frame is represented by a colored region mark on a map.

通常在地图中使用

```python
px.choropleth(data_frame=None, lat=None, lon=None, locations=None, locationmode=None, geojson=None, featureidkey=None, color=None, hover_name=None, hover_data=None, custom_data=None, animation_frame=None, animation_group=None, category_orders={}, labels={}, color_discrete_sequence=None, color_discrete_map={}, color_continuous_scale=None, range_color=None, color_continuous_midpoint=None, projection=None, scope=None, center=None, title=None, template=None, width=None, height=None)
```



### density_heatmap-密度热图

In a density heatmap, rows of data_frame are grouped together into colored rectangular tiles to visualize the 2D distribution of an aggregate function histfunc (e.g. the count or sum) of the value z.

```python
plotly.express.density_heatmap(data_frame=None, x=None, y=None, z=None, facet_row=None, facet_col=None, facet_col_wrap=0, hover_name=None, hover_data=None, animation_frame=None, animation_group=None, category_orders={}, labels={}, orientation=None, color_continuous_scale=None, range_color=None, color_continuous_midpoint=None, marginal_x=None, marginal_y=None, opacity=None, log_x=False, log_y=False, range_x=None, range_y=None, histfunc=None, histnorm=None, nbinsx=None, nbinsy=None, title=None, template=None, width=None, height=None)
```

### 参数解释

参数解释以第一个散点图为例：

- data_frame：目标数据，类型为dataframe；
- x ：指定列名。列中的值用于笛卡尔坐标中沿 X 轴的定位标记。图表类型为水平柱状图时，这些值用作参数histfunc的入参；
- y ：指定列名。列中的值用于笛卡尔坐标中沿 Y 轴的定位标记。图表类型为垂直柱状图时，这些值用作参数histfunc的入参；
- color：指定列名。为列中的不同值，(由px)自动匹配不同的标记颜色；**若列为数值数据时，还会自动生成连续色标**；
- symbol：指定列名。为列中的不同值，设置不同的标记形状；
- size：指定列名。为列中的不同值，设置不同的标记大小；
- $\color{red}{hover_name}$：指定列名。将列中的值，加粗显示在悬停提示内容的正上方；
- hover_data：指定列名组成的列表。所有列的值，显示在悬停提示内容中，位于x/y值的下方。指定的列与x/y重复时仅显示1条数据；
- text：指定列名。列中的值，在图的标记中显示为文本标签，同时也显示在悬停提示内容中；
- facet_row：指定列名。根据列中不同的(N个)值，在垂直方向上显示N个子图，并在子图右侧，垂直方向上，进行文本标注；
- facet_col：指定列名。根据列中不同的(N个)值，在水平方向上显示N个子图，并在子图上方，水平方向上，进行文本标注；
- error_x：指定列名。显示误差线，列中的值用于调整 X 轴误差线的大小。如果参数error_x_minus == None，则悬停提示内容中，显示对称的误差值；否则显示正向的误差值。该列通常是基于元数据加工的结果，目的是统计元数据指标的误差值，一般会用元数据除以100的整数倍。
- error_x_minus：指定列名。列中的值用于在负方向调整 X 轴误差线的大小，如果参数error_x==None，则直接忽略该参数；
- error_y：指定列名。显示误差线，列中的值用于调整 Y 轴误差线的大小。如果参数error_y_minus == None，则悬停提示内容中，显示对称的误差值；否则显示正向的误差值。该列通常是基于元数据加工的结果，目的是统计元数据指标的误差值，一般会用元数据除以100的整数倍。
- error_y_minus：指定列名。列中的值用于在负方向调整 Y 轴误差线的大小，如果参数error_y==None，则直接忽略该参数；
- animation_frame：指定列名。列中的值用于为动画帧指定标记，即设置滑动条；
- animation_group：指定列名。列中的值用于提供跨动画帧的联动匹配；
- category_orders：带有字符串键和字符串列表值的字典，默认为{}，此参数用于强制每列的特定值排序，dict键是列名，dict值是指定的排列顺序的字符串列表。默认情况下，在Python 3.6+中，轴，图例和构面中的分类值的顺序取决于在data_frame中首次出现的顺序，而在3.6以下的Python中，默认不保证顺序，该参数即为解决此类问题而设计；
- labels：带字符串键和字符串值的dict，默认为{}。此参数用于修改图表中显示的列名称。默认情况下，图表中使用列名称作为轴标题、图例条目、悬停提示等，此参数可以进行修改，dict的键是列名，dict值是修改的新名称；
- color_discrete_sequence：有效的CSS颜色字符串列表，取自plotly_express的color子模块。当参数color指定的列不是数值数据时，该参数为color列指定颜色序列，若category_orders参数不为None，则按category_orders中设定的顺序循环执行color_discrete_sequence，除非color列的值在参数color_discrete_map入参的dict键中；
- color_discrete_map：带字符串键和有效CSS颜色字符串值的dict，默认为{}。当参数color指定的列不是数值数据时，该参数用于将特定颜色分配给，与特定值对应的标记，color_discrete_map中的键为color表示的列值。其优先级高，会覆盖color_discrete_sequence参数中的设置；
- color_continuous_scale：有效的CSS颜色字符串列表，取自plotly_express的color子模块。当参数color指定的列是数值数据时，为连续色标，设置指定的颜色序列。实际上，color指定列时，px会自动匹配颜色：1）若指定列是数值数据，通过参数color_continuous_scale可以设定具体的颜色序列；2）若指定列是非数值数据时，通过参数color_discrete_sequence可以设定具体的颜色序列(循环匹配)；通过参数color_discrete_map可以为列中不同值，指定具体的颜色；
- range_color：2个数字元素组成的列表，参数用于设定连续色标上的自动缩放，即边界的大小值；
- color_continuous_midpoint：数字，默认为无。如果设置，则计算连续色标的边界以具有所需的中点。 若使用plotly_express.colors.diverging色标作为color_continuous_scale的如参时，建议设置此值；
- symbol_sequence：定义plotly.js符号的字符串列表。参数用于为列中的值分配符号，除非symbol的值是symbol_map中的键。分配符号的顺序：按按category_orders中设置的顺序循环执行；
- symbol_map：带字符串键和定义plotly.js符号的字符串值的dict，默认值{}。该参数用于将特定符号分配给，与特定值对应的标记，symbol_map中的键为symbol表示的列值。其优先级高，会覆盖symbol_sequence参数中的设置；
- opacity：数字，介于0和1之间，设置标记的不透明度；
- size_max：整数，默认为20。使用size参数时，设置最大标记的大小；
- marginal_x：字符串，取值：rug(细条)、box(箱图)、violin(小提琴图)、histogram(直方图)。该参数用于在主图上方，绘制一个水平子图，以便对x分布，进行可视化；
- marginal_y：字符串，取值：rug(细条)、box(箱图)、violin(小提琴图)、histogram(直方图)。该参数用于在主图右侧，绘制一个垂直子图，以便对y分布，进行可视化；
- trendline：字符串，取值：ols、lowess、None。取值为ols时，将为每个离散颜色/符号组，绘制一个普通最小二乘回归线；取值为lowess时，则将为每个离散颜色/符号组，绘制局部加权散点图平滑线；
- trendline_color_override：字符串，有效的CSS颜色。如果设置了参数trendline趋势线，则将以此颜色绘制所有趋势线；
- log_x：布尔值，默认为False。如果为True，则 X 轴在笛卡尔坐标系中进行对数缩放；
- log_y：布尔值，默认为False。如果为True，则 Y 轴在笛卡尔坐标系中进行对数缩放；
- range_x：2个数字元素组成的列表，用于设定笛卡尔坐标中 X 轴上的自动缩放，即边界的大小值；
- range_y：2个数字元素组成的列表，用于设定笛卡尔坐标中 Y 轴上的自动缩放，即边界的大小值；
- render_mode：字符串，取值：auto(默认)、svg、webgl。用于控制绘制标记的浏览器API，svg适用于少于1000的数据，并允许完全矢量化输出；webgl可以接收1000点以上的数据；auto使用启发式方法来选择模式；
- title：字符串，设置图表的标题；
- template：字符串或Plotly.py模板对象，设置图表的背景颜色。有三个内置的 Plotly 主题： plotly， plotly_white 和 plotly_dark；
- width：整数，默认无，设置图表的宽度（以像素为单位）；
- height：整数，默认600，设置图表的高度（以像素为单位）；

**其他作图方法的作图参数类似**

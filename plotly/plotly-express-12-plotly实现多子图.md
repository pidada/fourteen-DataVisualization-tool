---
title: plotly-express-12-plotly实现多子图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-07-04 09:01:09
password:
summary:
tags:
  - px
  - 可视化
categories:
  - datavisual
  - px
---

### Plotly-express-12-实现多子图subplots

在很多的实际业务需求中，需要将多个图形集中放置一个`figure`中，而不是单独显示，在这种情况下我们需要使用子图的概念。本文中讲解如何在plotly中使用`plotly.graph_objects`绘制各种形式的子图

> Figures with subplots are created using the `make_subplots` function from the `plotly.subplots` module.

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggftcn8pfzj30ja0hawgn.jpg)

<!--MORE-->

### 参数详解

```python
make_subplots(rows=1, cols=1, shared_xaxes=False, shared_yaxes=False, start_cell='top-left', print_grid=False, horizontal_spacing=None, vertical_spacing=None, subplot_titles=None, column_widths=None, row_heights=None, specs=None, insets=None, column_titles=None, row_titles=None, x_title=None, y_title=None, **kwargs)

Return an instance of plotly.graph_objs.Figure with predefined subplots configured in 'layout'.
```



```python
Parameters
    ----------
    rows: int (default 1)
        Number of rows in the subplot grid. Must be greater than zero.

    cols: int (default 1)
        Number of columns in the subplot grid. Must be greater than zero.

    shared_xaxes: boolean or str (default False)
        Assign shared (linked) x-axes for 2D cartesian subplots

          - True or 'columns': Share axes among subplots in the same column
          - 'rows': Share axes among subplots in the same row
          - 'all': Share axes across all subplots in the grid.

    shared_yaxes: boolean or str (default False)
        Assign shared (linked) y-axes for 2D cartesian subplots

          - 'columns': Share axes among subplots in the same column
          - True or 'rows': Share axes among subplots in the same row
          - 'all': Share axes across all subplots in the grid.

    start_cell: 'bottom-left' or 'top-left' (default 'top-left')
        Choose the starting cell in the subplot grid used to set the
        domains_grid of the subplots.

          - 'top-left': Subplots are numbered with (1, 1) in the top
                        left corner
          - 'bottom-left': Subplots are numbererd with (1, 1) in the bottom
                           left corner

    print_grid: boolean (default True):
        If True, prints a string representation of the plot grid.  Grid may
        also be printed using the `Figure.print_grid()` method on the
        resulting figure.

    horizontal_spacing: float (default 0.2 / cols)
        Space between subplot columns in normalized plot coordinates. Must be
        a float between 0 and 1.

        Applies to all columns (use 'specs' subplot-dependents spacing)

    vertical_spacing: float (default 0.3 / rows)
        Space between subplot rows in normalized plot coordinates. Must be
        a float between 0 and 1.

        Applies to all rows (use 'specs' subplot-dependents spacing)

    subplot_titles: list of str or None (default None)
        Title of each subplot as a list in row-major ordering.

        Empty strings ("") can be included in the list if no subplot title
        is desired in that space so that the titles are properly indexed.

    specs: list of lists of dict or None (default None)
        Per subplot specifications of subplot type, row/column spanning, and
        spacing.

        ex1: specs=[[{}, {}], [{'colspan': 2}, None]]

        ex2: specs=[[{'rowspan': 2}, {}], [None, {}]]

        - Indices of the outer list correspond to subplot grid rows
          starting from the top, if start_cell='top-left',
          or bottom, if start_cell='bottom-left'.
          The number of rows in 'specs' must be equal to 'rows'.

        - Indices of the inner lists correspond to subplot grid columns
          starting from the left. The number of columns in 'specs'
          must be equal to 'cols'.

        - Each item in the 'specs' list corresponds to one subplot
          in a subplot grid. (N.B. The subplot grid has exactly 'rows'
          times 'cols' cells.)

        - Use None for a blank a subplot cell (or to move past a col/row span).

        - Note that specs[0][0] has the specs of the 'start_cell' subplot.

        - Each item in 'specs' is a dictionary.
            The available keys are:
            * type (string, default 'xy'): Subplot type. One of
                - 'xy': 2D Cartesian subplot type for scatter, bar, etc.
                - 'scene': 3D Cartesian subplot for scatter3d, cone, etc.
                - 'polar': Polar subplot for scatterpolar, barpolar, etc.
                - 'ternary': Ternary subplot for scatterternary
                - 'mapbox': Mapbox subplot for scattermapbox
                - 'domain': Subplot type for traces that are individually
                            positioned. pie, parcoords, parcats, etc.
                - trace type: A trace type which will be used to determine
                              the appropriate subplot type for that trace

            * secondary_y (bool, default False): If True, create a secondary
                y-axis positioned on the right side of the subplot. Only valid
                if type='xy'.
            * colspan (int, default 1): number of subplot columns
                for this subplot to span.
            * rowspan (int, default 1): number of subplot rows
                for this subplot to span.
            * l (float, default 0.0): padding left of cell
            * r (float, default 0.0): padding right of cell
            * t (float, default 0.0): padding right of cell
            * b (float, default 0.0): padding bottom of cell

        - Note: Use 'horizontal_spacing' and 'vertical_spacing' to adjust
          the spacing in between the subplots.

    insets: list of dict or None (default None):
        Inset specifications.  Insets are subplots that overlay grid subplots

        - Each item in 'insets' is a dictionary.
            The available keys are:

            * cell (tuple, default=(1,1)): (row, col) index of the
                subplot cell to overlay inset axes onto.
            * type (string, default 'xy'): Subplot type
            * l (float, default=0.0): padding left of inset
                  in fraction of cell width
            * w (float or 'to_end', default='to_end') inset width
                  in fraction of cell width ('to_end': to cell right edge)
            * b (float, default=0.0): padding bottom of inset
                  in fraction of cell height
            * h (float or 'to_end', default='to_end') inset height
                  in fraction of cell height ('to_end': to cell top edge)

    column_widths: list of numbers or None (default None)
        list of length `cols` of the relative widths of each column of suplots.
        Values are normalized internally and used to distribute overall width
        of the figure (excluding padding) among the columns.

        For backward compatibility, may also be specified using the
        `column_width` keyword argument.

    row_heights: list of numbers or None (default None)
        list of length `rows` of the relative heights of each row of subplots.
        If start_cell='top-left' then row heights are applied top to bottom.
        Otherwise, if start_cell='bottom-left' then row heights are applied
        bottom to top.

        For backward compatibility, may also be specified using the
        `row_width` kwarg. If specified as `row_width`, then the width values
        are applied from bottom to top regardless of the value of start_cell.
        This matches the legacy behavior of the `row_width` argument.

    column_titles: list of str or None (default None)
        list of length `cols` of titles to place above the top subplot in
        each column.

    row_titles: list of str or None (default None)
        list of length `rows` of titles to place on the right side of each
        row of subplots. If start_cell='top-left' then row titles are
        applied top to bottom. Otherwise, if start_cell='bottom-left' then
        row titles are applied bottom to top.

    x_title: str or None (default None)
        Title to place below the bottom row of subplots,
        centered horizontally

    y_title: str or None (default None)
        Title to place to the left of the left column of subplots,
       centered vertically
```

### 使用模块

```python
from plotly.subplots import make_subplots
import plotly.graph_objects as go
```

### 一行多列

```python
fig = make_subplots(rows=1, cols=2,
                   subplot_titles=("Plot 1", "Plot 2"))

fig.add_trace(go.Scatter(x=[1, 2, 3], y=[4, 5, 2],
                         name="figure-one"),
              row=1, col=1)
fig.add_trace(go.Scatter(x=[20, 30, 40], y=[20, 40, 30],
                         name="figure-two"),
              row=1, col=2)

fig.update_layout(height=400, width=900, title_text="Side By Side Subplots")
fig.show()
```

![image-20200705085820419](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggfte6ge6aj31js0u0grt.jpg)

### 一列多行

```python
fig = make_subplots(rows=3, cols=1)   # 其实就是3*1的方式

fig.append_trace(go.Scatter(
    x=[3, 4, 5],
    y=[1000, 1100, 1200],
), row=1, col=1)

fig.append_trace(go.Bar(
    x=[2, 3, 4],
    y=[100, 110, 120],
), row=2, col=1)

fig.append_trace(go.Scatter(
    x=[0, 1, 2],
    y=[10, 11, 12]
), row=3, col=1)


fig.update_layout(height=600, width=600, title_text="Stacked Subplots")
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggftgzujvoj30wx0u0gnm.jpg)

### 多行多列

```python
fig = make_subplots(rows=2, cols=2,   # 2*2
                    start_cell="top-left",  # 子图开始的位置，只有2个选项：bottom-left或者top-left
                   subplot_titles=("折线图1","柱状图1","折线图2","柱状图2"),   # 每个子图的标题
                   column_widths=[0.4, 0.6])   # 每个子图占比

fig.add_trace(go.Scatter(x=[1, 2, 3],
                         y=[4, 5, 6],
                         mode="markers+text",
                         text=["Text A", "Text B", "Text C"],   # 散点图中的文本设置和显示
                         textposition="top center",
                         name="line1"),
              row=1, col=1)  # 指定位置

fig.add_trace(go.Bar(x=[20, 30, 40],
                     y=[50, 60, 70],
                     text=["Text A", "Text B", "Text C"],  #  柱状图中文本的设置和显示
                     textposition="inside",
                     name="bar1"),
              row=1, col=2)   # 红色柱状图

fig.add_trace(go.Bar(x=[300, 400, 500],
                     y=[600, 700, 800],
                     name="bar2"),
              row=2, col=1)

fig.add_trace(go.Scatter(x=[4000, 5000, 6000],
                         y=[7000, 8000, 9000],
                         name="line2"),
              row=2, col=2)

fig.update_layout(width=900,height=900,  # 改变整个figure的大小
                 title_text="Mutiple subplots")
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggftjkm93dj31090u0ad2.jpg)

### 子图属性设置

- 第一个子图的起始位置
- 每个子图的标题
- 子图之间的间隔设置
- 如何共享x轴
- 每个子图中的文本信息设置及位置显示
- 子图右边的图例名称
- 子图的位置通过row/col实现
- 单独设置xy轴的名称

```python
fig = make_subplots(rows=2, cols=2,   # 2*2
                    start_cell="top-left",  # 子图开始的位置，只有2个选项：bottom-left或者top-left
                   subplot_titles=("折线图1","柱状图1","折线图2","柱状图2"),   # 每个子图的标题
                   column_widths=[0.4, 0.6],  # 每个子图占比
#                    shared_xaxes=True,   # 共享x轴
#                    shared_yaxes=True,   # 共享y轴
#                    vertical_spacing=0.02)  # 每个子图的间隔
                   )

fig.add_trace(go.Scatter(x=[1, 2, 3],
                         y=[4, 5, 6],
                         mode="markers+text",
                         text=["Text A", "Text B", "Text C"],   # 散点图中的文本设置和显示
                         textposition="top center",
                         name="line1"),
              row=1, col=1)  # 指定位置

fig.add_trace(go.Bar(x=[20, 30, 40],
                     y=[50, 60, 70],
                     text=["Text A", "Text B", "Text C"],  #  柱状图中文本的设置和显示
                     textposition="inside",
                     name="bar1"),
              row=1, col=2)   # 红色柱状图

fig.add_trace(go.Bar(x=[300, 400, 500],
                     y=[600, 700, 800],
                     name="bar2"),
              row=2, col=1)

fig.add_trace(go.Scatter(x=[4000, 5000, 6000],
                         y=[7000, 8000, 9000],
                         name="line2"),
              row=2, col=2)

# Update xaxis properties
# 设置每个子图自己的x轴
fig.update_xaxes(title_text="x_1", row=1, col=1)
fig.update_xaxes(title_text="x_2", row=1, col=2)
fig.update_xaxes(title_text="x_3", showgrid=False, row=2, col=1)
fig.update_xaxes(title_text="x_4", row=2, col=2)

# 设置每个子图自己的y轴
# Update yaxis properties
fig.update_yaxes(title_text="yaxis_1", row=1, col=1)
fig.update_yaxes(title_text="yaxis_2", row=1, col=2)
fig.update_yaxes(title_text="yaxis_3", showgrid=False, row=2, col=1)
fig.update_yaxes(title_text="yaxis_4", row=2, col=2)


# 设置整个图的大小和标题
fig.update_layout(width=1000,height=1200,  # 改变整个figure的大小
                 title_text="Mutiple subplots")
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggftp1jymqj30w70u0q5p.jpg)

###  共享颜色轴

选择颜色

```python
['aggrnyl', 'agsunset', 'algae', 'amp', 'armyrose', 'balance',
             'blackbody', 'bluered', 'blues', 'blugrn', 'bluyl', 'brbg',
             'brwnyl', 'bugn', 'bupu', 'burg', 'burgyl', 'cividis', 'curl',
             'darkmint', 'deep', 'delta', 'dense', 'earth', 'edge', 'electric',
             'emrld', 'fall', 'geyser', 'gnbu', 'gray', 'greens', 'greys',
             'haline', 'hot', 'hsv', 'ice', 'icefire', 'inferno', 'jet',
             'magenta', 'magma', 'matter', 'mint', 'mrybm', 'mygbm', 'oranges',
             'orrd', 'oryel', 'peach', 'phase', 'picnic', 'pinkyl', 'piyg',
             'plasma', 'plotly3', 'portland', 'prgn', 'pubu', 'pubugn', 'puor',
             'purd', 'purp', 'purples', 'purpor', 'rainbow', 'rdbu', 'rdgy',
             'rdpu', 'rdylbu', 'rdylgn', 'redor', 'reds', 'solar', 'spectral',
             'speed', 'sunset', 'sunsetdark', 'teal', 'tealgrn', 'tealrose',
             'tempo', 'temps', 'thermal', 'tropic', 'turbid', 'twilight',
             'viridis', 'ylgn', 'ylgnbu', 'ylorbr', 'ylorrd']
```

```python
fig = make_subplots(rows=1, cols=2, shared_yaxes=True)

fig.add_trace(go.Bar(x=[1, 2, 3], y=[4, 5, 6],
                    marker=dict(color=[4, 5, 6],
                                coloraxis="coloraxis")),
              1, 1)

fig.add_trace(go.Scatter(x=[1, 2, 3], y=[2, 3, 5],
                    marker=dict(color=[2, 3, 5],
                                coloraxis="coloraxis")),
              1, 2)

fig.update_layout(coloraxis=dict(colorscale='magma'), showlegend=False)
fig.show()
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggftt3f2p6j31bm0l6abl.jpg)

```python
fig = make_subplots(rows=1, cols=2, shared_yaxes=True)

fig.add_trace(go.Bar(x=[1, 2, 3], y=[4, 5, 6],
                    marker=dict(color=[4, 5, 6], coloraxis="coloraxis")),
              1, 1)

fig.add_trace(go.Bar(x=[1, 2, 3], y=[2, 3, 5],
                    marker=dict(color=[2, 3, 5], coloraxis="coloraxis")),
              1, 2)

fig.update_layout(coloraxis=dict(colorscale='tealgrn'),   # 变化点
                  showlegend=False)
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggftu9pzj7j31cw0mowfx.jpg)

### 自定义子图位置（占几行几列）

写法说明：

- {}：表示该位置出现一个子图
- {"rowspan":2} 表示该位置的子图占据2行
- None：表示该位置上没有子图

```python
fig = make_subplots(
    rows=5, cols=2,
    specs=[[{}, {"rowspan": 2}],
           [{}, None],
           [{"rowspan": 2, "colspan": 2}, None],
           [None, None],
           [{}, {}]],
#     print_grid=True
)

fig.add_trace(go.Scatter(x=[1, 2], y=[1, 2], name="(1,1)"), row=1, col=1)  # 数据，名字，位置
fig.add_trace(go.Scatter(x=[1, 2], y=[1, 2], name="(1,2)"), row=1, col=2)
fig.add_trace(go.Bar(x=[1, 2, 3], y=[2, 4, 6], name="(2,1)"), row=2, col=1)
fig.add_trace(go.Scatter(x=[1, 2], y=[1, 2], name="(3,1)"), row=3, col=1)
fig.add_trace(go.Scatter(x=[1, 2], y=[1, 2], name="(5,1)"), row=5, col=1)
fig.add_trace(go.Bar(x=[1, 2], y=[1, 2], name="(5,2)"), row=5, col=2)

fig.update_layout(height=600, width=600, title_text="specs examples")
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggftvkn6scj30xs0u0jtm.jpg)

###  子图类型

当不同的子图放在一起的时候，需要指定子图的类型，常见的类型有：

> By default, the make_subplots function assumes that the traces that will be added to all subplots are 2-dimensional cartesian traces (e.g. scatter, bar, histogram, violin, etc.). Traces with other subplot types (e.g. scatterpolar, scattergeo, parcoords, etc.) are supporteed by specifying the type subplot option in the specs argument to make_subplots.

Here are the possible values for the type option:

- "xy": 2D Cartesian subplot type for scatter, bar, etc. This is the default if no type is specified.
- "scene": 3D Cartesian subplot for scatter3d, cone, etc.
- "polar": Polar subplot for scatterpolar, barpolar, etc.
- "ternary": Ternary subplot for scatterternary.
- "mapbox": Mapbox subplot for scattermapbox.
- "domain": Subplot type for traces that are individually positioned. pie, parcoords, parcats, etc.

```python
fig = make_subplots(
    rows=2, cols=2,
    specs=[[{"type": "xy"}, {"type": "polar"}],   # 每行每列在规定位置指明类型
           [{"type": "domain"}, {"type": "scene"}]],
)

fig.add_trace(go.Bar(y=[2, 3, 1]),
              row=1, col=1)

fig.add_trace(go.Barpolar(theta=[0, 45, 90], r=[2, 3, 1]),
              row=1, col=2)

fig.add_trace(go.Pie(values=[2, 3, 1]),
              row=2, col=1)

fig.add_trace(go.Scatter3d(x=[2, 3, 1], y=[0, 0, 0],
                           z=[0.5, 1, 2], mode="lines"),
              row=2, col=2)

fig.update_layout(height=700, showlegend=False)

fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggftzxupxbj318i0u0jvv.jpg)



又是收获满满的一篇💪

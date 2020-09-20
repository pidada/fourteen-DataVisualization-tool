---
title: pyecharts-2-全局配置项设置
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-08-22 12:04:24
password:
summary:
tags:
  - pyecharts
  - 可视化
categories:
  - datavisual
  - pyecharts
---

### pyecharts-2-全局配置项设置

本文中介绍的如何在pyecharts中配置全局组件，在后续的作图中会用到这些全局配置项。

> **组件配置的正确合理，能够让整个图形看上去很美观和清晰，同时表达效果更好**



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghxqs1w9euj31400u0aex.jpg)



<!--MORE-->

这里是[参考资料]([https://pyecharts.org/#/zh-cn/global_options?id=titleopts%ef%bc%9a%e6%a0%87%e9%a2%98%e9%85%8d%e7%bd%ae%e9%a1%b9](https://pyecharts.org/#/zh-cn/global_options?id=titleopts：标题配置项))，自己主要是进行了提炼。官网上的资料很全面，但是有些设置平时可能用的并不是很频繁，所以我做了删减。感谢官方学习资料😄

### 全局配置

全局配置项可通过 `set_global_options` 方法设置

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghxqtzvz0hj30wh0iwq5y.jpg)

**常用到的几个配置项**

- [InitOpts：初始化配置项](https://pyecharts.org/#/zh-cn/global_options?id=initopts：初始化配置项)
- [ToolBoxFeatureSaveAsImagesOpts：工具箱保存图片配置项](https://pyecharts.org/#/zh-cn/global_options?id=toolboxfeaturesaveasimagesopts：工具箱保存图片配置项)
- [ToolBoxFeatureDataViewOpts：工具箱数据视图工具](https://pyecharts.org/#/zh-cn/global_options?id=toolboxfeaturedataviewopts：工具箱数据视图工具)
- [ToolBoxFeatureDataZoomOpts：工具箱区域缩放配置项](https://pyecharts.org/#/zh-cn/global_options?id=toolboxfeaturedatazoomopts：工具箱区域缩放配置项)
- [ToolboxOpts：工具箱配置项](https://pyecharts.org/#/zh-cn/global_options?id=toolboxopts：工具箱配置项)
- [TitleOpts：标题配置项](https://pyecharts.org/#/zh-cn/global_options?id=titleopts：标题配置项)
- [DataZoomOpts：区域缩放配置项](https://pyecharts.org/#/zh-cn/global_options?id=datazoomopts：区域缩放配置项)
- [LegendOpts：图例配置项](https://pyecharts.org/#/zh-cn/global_options?id=legendopts：图例配置项)
- [VisualMapOpts：视觉映射配置项](https://pyecharts.org/#/zh-cn/global_options?id=visualmapopts：视觉映射配置项)
- [TooltipOpts：提示框配置项](https://pyecharts.org/#/zh-cn/global_options?id=tooltipopts：提示框配置项)
- [AxisLineOpts: 坐标轴轴线配置项](https://pyecharts.org/#/zh-cn/global_options?id=axislineopts-坐标轴轴线配置项)
- [AxisTickOpts: 坐标轴刻度配置项](https://pyecharts.org/#/zh-cn/global_options?id=axistickopts-坐标轴刻度配置项)



其中$\color{red}{初始化配置、标题配置项、图例配置项和坐标轴}$相关配置项是最常用的，**需要重点掌握**

在之后的实例中会经常使用这些配置项

### InitOpts-初始化配置项

```python
class InitOpts(
    # 图表画布宽度，css 长度单位。
    width: str = "900px",
    # 图表画布高度，css 长度单位。
    height: str = "500px",
    # 图表 ID，图表唯一标识，用于在多图表时区分。
    chart_id: Optional[str] = None,
    # 渲染风格 "canvas", "svg"
    renderer: str = RenderType.CANVAS,
    # 网页标题
    page_title: str = "Awesome-pyecharts",
    # 图表主题
    theme: str = "white",
    # 图表背景颜色
    bg_color: Optional[str] = None,
    # 远程 js host，如不设置默认为 https://assets.pyecharts.org/assets/"
    js_host: str = "",

    # 画图动画初始化配置，参考 `global_options.AnimationOpts`
    animation_opts: Union[AnimationOpts, dict] = AnimationOpts(),
)
```



### ToolBoxFeatureSaveAsImagesOpts-工具箱保存图片配置项

```python
class ToolBoxFeatureSaveAsImageOpts(
    # 保存的图片格式。支持 'png' 和 'jpeg'。
    type_: str = "png",
    # 保存的文件名称，默认使用 title.text 作为名称。
    name: Optional[str] = None,
    # 保存的图片背景色，默认使用 backgroundColor，如果backgroundColor不存在的话会取白色。
    background_color: str = "auto",

    # 如果图表使用了 echarts.connect 对多个图表进行联动，则在导出图片时会导出这些联动的图表。该配置项决定了图表与图表之间间隙处的填充色。
    connected_background_color: str = "#fff",
    # 保存为图片时忽略的组件列表，默认忽略工具栏。
    exclude_components: Optional[Sequence[str]] = None,

    # 是否显示该工具。
    is_show: bool = True,

    # 提示语
    title: str = "保存为图片",

    # 保存图片的分辨率比例，默认跟容器相同大小，如果需要保存更高分辨率的，可以设置为大于 1 的值，例如 2。
    pixel_ratio: Numeric = 1,
):
```



### ToolBoxFeatureDataViewOpts-工具箱数据视图工具

```python
class ToolBoxFeatureDataViewOpts(
    # 是否显示该工具。
    is_show: bool = True,
    # 提示语
    title: str = "还原",
    # 可以通过 'image://url' 设置为图片，其中 URL 为图片的链接，或者 dataURI。
    icon: Optional[JSFunc] = None
    # 是否不可编辑（只读）。默认为 False
    is_read_only: bool = False,
    # 数据视图上有三个话术，默认是['数据视图', '关闭', '刷新']。
    lang: Optional[Sequence[str]] = None,
    # 数据视图浮层背景色。
    background_color: str = "#fff",
    # 数据视图浮层文本输入区背景色。
    text_area_color: str = "#fff",
    # 数据视图浮层文本输入区边框颜色。
    text_area_border_color: str = "#333",
    # 文本颜色
    text_color: str = "#000",
    # 按钮颜色
    button_color: str = "#c23531",
    # 按钮文本颜色
    button_text_color: str = "#fff",
):
```



### ToolBoxFeatureDataZoomOpts-工具箱区域缩放配置项

```python
class ToolBoxFeatureDataZoomOpts(
    # 是否显示该工具。
    is_show: bool = True,
    # 提示语
    zoom_title: str = "区域缩放",
    # 提示语
    back_title: str = "区域缩放还原",

    # 指定哪些 xAxis 被控制。如果缺省则控制所有的 x 轴。
    # 如果设置为 false 则不控制任何x轴。如果设置成 3 则控制 axisIndex 为 3 的 x 轴。
    # 如果设置为 [0, 3] 则控制 axisIndex 为 0 和 3 的 x 轴。
    xaxis_index: Union[Numeric, Sequence, bool] = None,
  	# 同上
    yaxis_index: Union[Numeric, Sequence, bool] = None,

    # dataZoom 的运行原理是通过数据过滤以及在内部设置轴的显示窗口来达到数据窗口缩放的效果。
    # 'filter'：当前数据窗口外的数据，被过滤掉。即会影响其他轴的数据范围。
    #  每个数据项，只要有一个维度在数据窗口外，整个数据项就会被过滤掉。
    # 'weakFilter'：当前数据窗口外的数据，被过滤掉。即会影响其他轴的数据范围。
    #  每个数据项，只有当全部维度都在数据窗口同侧外部，整个数据项才会被过滤掉。
    # 'empty'：当前数据窗口外的数据，被设置为空。即不会影响其他轴的数据范围。
    # 'none': 不过滤数据，只改变数轴范围。
    filter_mode: str = "filter",
```



### ToolboxOpts-工具箱配置项

```python
class ToolboxOpts(
    # 是否显示工具栏组件
    is_show: bool = True,

    # icon 的布局朝向:'horizontal', 'vertical'
    orient: str = "horizontal",

    # 工具栏组件离容器左侧的距离。
    # 可以是像 20 这样的具体像素值，可以是像 '20%' 这样相对于容器高宽的百分比，
    # 也可以是 'left', 'center', 'right'。
    # 如果 left 的值为'left', 'center', 'right'，组件会根据相应的位置自动对齐
    pos_left: str = "80%",
    pos_right: Optional[str] = None,

    # 工具栏组件离容器上侧的距离。
    # 也可以是 'top', 'middle', 'bottom'。
    pos_top: Optional[str] = None,
    pos_bottom: Optional[str] = None
)
```



### TitleOpts-标题配置项

**这个应用的非常频繁**

> 标题的设置是很重要的

```python
class TitleOpts(
    # 主标题文本，支持使用 \n 换行。
    title: Optional[str] = None,

    # 主标题跳转 URL 链接
    title_link: Optional[str] = None,

    # 主标题跳转链接方式,默认值是: blank
    # 'self' 当前窗口打开; 'blank' 新窗口打开
    title_target: Optional[str] = None,

    # 副标题文本和副标题跳转 URL 链接
    subtitle: Optional[str] = None,
    subtitle_link: Optional[str] = None,
    # 'self' 当前窗口打开; 'blank' 新窗口打开
    subtitle_target: Optional[str] = None,

    # 'left', 'center', 'right'。
    pos_left: Optional[str] = None,
    pos_right: Optional[str] = None,

    # 'top', 'middle', 'bottom'。
    pos_top: Optional[str] = None,
    pos_bottom: Optional[str] = None,

    # 标题内边距，单位px，默认各方向内边距为5，接受数组分别设定上右下左边距。
    # // 设置内边距为 5
    # padding: 5
    # // 设置上下的内边距为 5，左右的内边距为 10
    # padding: [5, 10]
    # // 分别设置四个方向的内边距
    # padding: [
    #     5,  // 上
    #     10, // 右
    #     5,  // 下
    #     10, // 左
    # ]
    padding: Union[Sequence, Numeric] = 5,

    # 主副标题之间的间距。
    item_gap: Numeric = 10,

    # 主标题字体样式配置项，参考 `series_options.TextStyleOpts`
    title_textstyle_opts: Union[TextStyleOpts, dict, None] = None,

    # 副标题字体样式配置项，参考 `series_options.TextStyleOpts`
    subtitle_textstyle_opts: Union[TextStyleOpts, dict, None] = None,
```

### DataZoomOpts：区域缩放配置项

```python
class DataZoomOpts(
    # 是否显示 组件。如果设置为 false，不会显示，但是数据过滤的功能还存在。
    is_show: bool = True,

    # 可选 "slider", "inside"
    type_: str = "slider",

    # 拖动时，是否实时更新系列的视图。如果设置为 false，则只在拖拽结束的时候更新。
    is_realtime: bool = True,

    # 数据窗口范围的起始百分比。范围是：0 ~ 100。表示 0% ~ 100%。
    range_start: Union[Numeric, None] = 20,
    range_end: Union[Numeric, None] = 80,

    # 数据窗口范围的起始数值。如果设置了 start 则 startValue 失效。
    start_value: Union[int, str, None] = None,
    end_value: Union[int, str, None] = None,

    # 布局方式是横还是竖。不仅是布局方式，对于直角坐标系而言，也决定了，缺省情况控制横向数轴还是纵向数轴
    # 可选值为：'horizontal', 'vertical'
    orient: str = "horizontal",

    # 设置 dataZoom-inside 组件控制的 x 轴（即 xAxis，是直角坐标系中的概念，参见 grid）。
    # 不指定时，当 dataZoom-inside.orient 为 'horizontal'时，默认控制和 dataZoom 平行的第一个 xAxis
    # 如果是 number 表示控制一个轴，如果是 Array 表示控制多个轴。
    xaxis_index: Union[int, Sequence[int], None] = None,

    # 设置 dataZoom-inside 组件控制的 y 轴（即 yAxis，是直角坐标系中的概念，参见 grid）。
    # 不指定时，当 dataZoom-inside.orient 为 'horizontal'时，默认控制和 dataZoom 平行的第一个 yAxis
    # 如果是 number 表示控制一个轴，如果是 Array 表示控制多个轴。
    yaxis_index: Union[int, Sequence[int], None] = None,

    # 是否锁定选择区域（或叫做数据窗口）的大小。
    # 如果设置为 true 则锁定选择区域的大小，也就是说，只能平移，不能缩放。
    is_zoom_lock: bool = False,

    # dataZoom-slider 组件离容器左侧的距离。
    # left 的值可以是像 20 这样的具体像素值，可以是像 '20%' 这样相对于容器高宽的百分比，
    # 也可以是 'left', 'center', 'right'。
    # 如果 left 的值为 'left', 'center', 'right'，组件会根据相应的位置自动对齐。
    pos_left: Optional[str] = None,
    pos_right: Optional[str] = None,

  	# dataZoom-slider 组件离容器上侧的距离。
    # top 的值可以是像 20 这样的具体像素值，可以是像 '20%' 这样相对于容器高宽的百分比，
    # 也可以是 'top', 'middle', 'bottom'。
    # 如果 top 的值为 'top', 'middle', 'bottom'，组件会根据相应的位置自动对齐。
    pos_top: Optional[str] = None,
    pos_bottom: Optional[str] = None,

    # dataZoom 的运行原理是通过数据过滤以及在内部设置轴的显示窗口来达到数据窗口缩放的效果。
    # 'filter'、'weakFilter'、'empty'、'none'
    filter_mode: str = "filter"
)
```



### LegendOpts：图例配置项

```python
class LegendOpts(
    # 图例的类型。可选值：
    # 'plain'：普通图例。缺省就是普通图例。
    # 'scroll'：可滚动翻页的图例。当图例数量较多时可以使用。
    type_: Optional[str] = None,

    # 图例选择的模式，控制是否可以通过点击图例改变系列的显示状态。默认开启图例选择，可以设成 false 关闭
    # 设成 'single' 或者 'multiple' 使用单选或者多选模式。
    selected_mode: Union[str, bool, None] = None,

    # 是否显示图例组件
    is_show: bool = True,

    # left 的值可以是像 20 这样的具体像素值，可以是像 '20%' 这样相对于容器高宽的百分比，
    # 也可以是 'left', 'center', 'right'
    pos_left: Union[str, Numeric, None] = None,
    pos_right: Union[str, Numeric, None] = None,

    # top 的值可以是像 20 这样的具体像素值，可以是像 '20%' 这样相对于容器高宽的百分比，
    # 也可以是 'top', 'middle', 'bottom'。
    pos_top: Union[str, Numeric, None] = None,
    pos_bottom: Union[str, Numeric, None] = None,

    # 布局朝向：'horizontal', 'vertical'
    orient: Optional[str] = None,

    # 图例标记和文本的对齐。默认自动（auto）；可选参数: `auto`, `left`, `right`
    align: Optional[str] = None,

    # 图例内边距，单位px，默认各方向内边距为5
    padding: int = 5,

    # 图例每项之间的间隔。横向布局时为水平间隔，纵向布局时为纵向间隔。
    # 默认间隔为 10
    item_gap: int = 10,

    # 图例标记的图形宽度。默认宽度为 25
    item_width: int = 25,
    # 图例标记的图形高度。默认高度为 14
    item_height: int = 14,

    # 图例关闭时的颜色。默认是 #ccc
    inactive_color: Optional[str] = None,

    # 图例组件字体样式，参考 `series_options.TextStyleOpts`
    textstyle_opts: Union[TextStyleOpts, dict, None] = None,

    # ECharts 提供的标记类型包括 'circle', 'rect', 'roundRect', 'triangle', 'diamond', 'pin', 'arrow', 'none'
    # 可以通过 'image://url' 设置为图片，其中 URL 为图片的链接，或者 dataURI。
    # 可以通过 'path://' 将图标设置为任意的矢量路径。
    legend_icon: Optional[str] = None,
)
```

### VisualMapOpts：视觉映射配置项

```python
class VisualMapOpts(
    # 是否显示视觉映射配置
    is_show: bool = True,

    # 映射过渡类型，可选，"color", "size"
    type_: str = "color",

    # 指定 visualMapPiecewise 组件的最值。
    min_: Union[int, float] = 0,
    max_: Union[int, float] = 100,

    # 两端的文本，如['High', 'Low']。
    range_text: Union[list, tuple] = None,

    range_color: Union[Sequence[str]] = None,
    range_size: Union[Sequence[int]] = None,

    # 透明度。
    range_opacity: Optional[Numeric] = None,

    # 水平（'horizontal'）或竖直（'vertical'）
    orient: str = "vertical",


    # left 的值可以是像 20 这样的具体像素值，可以是像 '20%' 这样相对于容器高宽的百分比，
    # 也可以是 'left', 'center', 'right'。
    # 如果 left 的值为'left', 'center', 'right'，组件会根据相应的位置自动对齐。
    pos_left: Optional[str] = None,
    pos_right: Optional[str] = None,


    # top 的值可以是像 20 这样的具体像素值，可以是像 '20%' 这样相对于容器高宽的百分比，
    # 也可以是 'top', 'middle', 'bottom'
    pos_top: Optional[str] = None,
    pos_bottom: Optional[str] = None,

    # 对于连续型数据，自动平均切分成几段。默认为5段。连续数据的范围需要 max 和 min 来指定
    split_number: int = 5,

    # 指定取哪个系列的数据，默认取所有系列。
    series_index: Union[Numeric, Sequence, None] = None,

    # 组件映射维度
    dimension: Optional[Numeric] = None,

    # 是否显示拖拽用的手柄（手柄能拖拽调整选中范围）。
    is_calculable: bool = True,

    # 是否为分段型
    is_piecewise: bool = False,

    # 是否反转 visualMap 组件
    is_inverse: bool = False,

    # 数据展示的小数精度。
    # 连续型数据平均分段，精度根据数据自动适应。
    # 连续型数据自定义分段或离散数据根据类别分段模式，精度默认为0（没有小数）。
    precision: Optional[int] = None,

    # 自定义的每一段的范围，以及每一段的文字，以及每一段的特别的样式。例如：
    # pieces: [
    #   {"min": 1500}, // 不指定 max，表示 max 为无限大（Infinity）。
    #   {"min": 900, "max": 1500},
    #   {"min": 310, "max": 1000},
    #   {"min": 200, "max": 300},
    #   {"min": 10, "max": 200, "label": '10 到 200（自定义label）'},
    #   {"value": 123, "label": '123（自定义特殊颜色）', "color": 'grey'}, //表示 value 等于 123 的情况
    #   {"max": 5}     // 不指定 min，表示 min 为无限大（-Infinity）。
    # ]
    pieces: Optional[Sequence] = None,

    # 定义 在选中范围外 的视觉元素。（用户可以和 visualMap 组件交互，用鼠标或触摸选择范围）
    #  可选的视觉元素有：
    #  symbol: 图元的图形类别。
    #  symbolSize: 图元的大小。
    #  color: 图元的颜色。
    #  colorAlpha: 图元的颜色的透明度。
    #  opacity: 图元以及其附属物（如文字标签）的透明度。
    #  colorLightness: 颜色的明暗度，参见 HSL。
    #  colorSaturation: 颜色的饱和度，参见 HSL。
    #  colorHue: 颜色的色调，参见 HSL。
    out_of_range: Optional[Sequence] = None,

    # 图形的宽度，即长条的宽度。
    item_width: int = 0,

    # 图形的高度，即长条的高度。
    item_height: int = 0,

    # visualMap 组件的背景色。
    background_color: Optional[str] = None,

    # visualMap 组件的边框颜色。
    border_color: Optional[str] = None,

    # visualMap 边框线宽，单位px。
    border_width: int = 0,

    # 文字样式配置项
    textstyle_opts: Union[TextStyleOpts, dict, None] = None,
)
```

### TooltipOpts：提示框配置项


 **formatter**参数很重要

```python
class TooltipOpts(
    is_show: bool = True,

    # 'item': 数据项图形触发，主要在散点图，饼图等无类目轴的图表中使用。
    # 'axis': 坐标轴触发，主要在柱状图，折线图等会使用类目轴的图表中使用。
    # 'none': 什么都不触发
    trigger: str = "item",

    # 'mousemove': 鼠标移动时触发。
    # 'click': 鼠标点击时触发。
    # 'mousemove|click': 同时鼠标移动和点击时触发。
    # 'none': 不在 'mousemove' 或 'click' 时触发，
    trigger_on: str = "mousemove|click",

    # 'line'：直线指示器
    # 'shadow'：阴影指示器
    # 'none'：无指示器
    # 'cross'：十字准星指示器。其实是种简写，表示启用两个正交的轴的 axisPointer。
    axis_pointer_type: str = "line",


    # 提示框浮层的位置，默认不设置时位置会跟随鼠标的位置。
    # 1、通过数组配置：
    # 绝对位置，相对于容器左侧 10px, 上侧 10 px ===> position: [10, 10]
    # 相对位置，放置在容器正中间 ===> position: ['50%', '50%']
    # 2、通过回调函数配置
    # 3、固定参数配置：'inside'，'top'，'left'，'right'，'bottom'
    position: Union[str, Sequence, JSFunc] = None,

    # 标签内容格式器，支持字符串模板和回调函数两种形式，字符串模板与回调函数返回的字符串均支持用 \n 换行。
    # 字符串模板 模板变量有：
    # {a}：系列名。
    # {b}：数据名。
    # {c}：数据值。
    # {@xxx}：数据中名为 'xxx' 的维度的值，如 {@product} 表示名为 'product'` 的维度的值。
    # {@[n]}：数据中维度 n 的值，如{@[3]}` 表示维度 3 的值，从 0 开始计数。
    # 示例：formatter: '{b}: {@score}'
    #
    # 回调函数，回调函数格式：
    # (params: Object|Array) => string
    # 参数 params 是 formatter 需要的单个数据集。格式如下：
    # {
    #    componentType: 'series',
    #    // 系列类型
    #    seriesType: string,
    #    // 系列在传入的 option.series 中的 index
    #    seriesIndex: number,
    #    // 系列名称
    #    seriesName: string,
    #    // 数据名，类目名
    #    name: string,
    #    // 数据在传入的 data 数组中的 index
    #    dataIndex: number,
    #    // 传入的原始数据项
    #    data: Object,
    #    // 传入的数据值
    #    value: number|Array,
    #    // 数据图形的颜色
    #    color: string,
    # }
    formatter: Optional[str] = None,

    # 提示框浮层的背景颜色。
    background_color: Optional[str] = None,

    # 提示框浮层的边框颜色。
    border_color: Optional[str] = None,

    # 提示框浮层的边框宽。
    border_width: Numeric = 0,

    # 文字样式配置项，参考 `series_options.TextStyleOpts`
    textstyle_opts: TextStyleOpts = TextStyleOpts(font_size=14),
)
```



### AxisLineOpts: 坐标轴轴线配置项



```python
class AxisLineOpts(
    # 是否显示坐标轴轴线。
    is_show: bool = True,

    # X 轴或者 Y 轴的轴线是否在另一个轴的 0 刻度上，只有在另一个轴为数值轴且包含 0 刻度时有效。
    is_on_zero: bool = True,

    # 当有双轴时，可以用这个属性手动指定，在哪个轴的 0 刻度上。
    on_zero_axis_index: int = 0,

    # 轴线两边的箭头。可以是字符串，表示两端使用同样的箭头；或者长度为 2 的字符串数组，分别表示两端的箭头。
    # 默认不显示箭头，即 'none'。
    # 两端都显示箭头可以设置为 'arrow'。
    # 只在末端显示箭头可以设置为 ['none', 'arrow']。
    symbol: Optional[str] = None,

    # 坐标轴线风格配置项，参考 `series_optionsLineStyleOpts`
    linestyle_opts: Union[LineStyleOpts, dict, None] = None,
)
```

### AxisTickOpts: 坐标轴刻度配置项

```python
class AxisTickOpts(
    # 是否显示坐标轴刻度。
    is_show: bool = True,

    # 类目轴中在 boundaryGap 为 true 的时候有效，可以保证刻度线和标签对齐。
    is_align_with_label: bool = False,

    # 坐标轴刻度是否朝内，默认朝外。
    is_inside: bool = False,

    # 坐标轴刻度的长度。
    length: Optional[Numeric] = None,

    # 坐标轴线风格配置项，参考 `series_optionsLineStyleOpts`
    linestyle_opts: Union[LineStyleOpts, dict, None] = None,
)
```

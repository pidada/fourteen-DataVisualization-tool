---
title: Highcharts-2-配置项
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2021-02-09 17:41:04
password:
summary:
tags:
  - 可视化
  - highcharts
categories:
  - datavisual
  - highcharts
---

### Highcharts-2-配置项介绍

本文介绍的是`Highcharts`中相关配置项，理解各个名词的基本含义。

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gngkhp1fgkj30rm05sdg5.jpg)

<!--MORE-->

参考信息来自官网，仅供学习使用：https://api.highcharts.com.cn/highcharts

### Highcharts基本组成

一个图标通常是由`图表区、标题、绘图区、坐标轴、图例/数据列`等不同部分组成的，如下图所示：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnhd2at2ybj30go0asdh0.jpg)

### 名词解释

- lang：语言文字对象，所有Highcharts文字相关的设置
- chart：图表区、图形区和通用图表配置选项
- colors：图表数据列颜色配置，是一个**颜色数组**
- credits: 版权信息，Highcharts在图表的**右下方放置**的版权信息及链
- drilldown：钻取，向下钻取数据，深入到其中的具体数据
- exporting：导出模块，导出功能配置，导出即将图表下载为图片或打印图表
- legend：**图例**，用不同形状、颜色、文字等 标示不同数据列，通过点击标示可以显示或隐藏该数据列
- loading：加载中，加载选项控制覆盖绘图区的加载屏的外观和文字
- navigation：导航，导出模块按钮和菜单配置选项组
- noData：没有数据，没有数据时显示的内容
- pane：分块，针对仪表图和雷达图专用的配置，主要设置弧度及背景色
- plotOptions：针对不同类型图表的配置
- series：数据列，图表上一个或多个数据系列，比如图表中的一条曲线，一个柱形
- title：标题，包括即标题和副标题，其中副标题为非必须的
- tooltip：数据点提示框，当鼠标滑过某点时，以框的形式提示改点的数据，比如该点的值，数据单位等
- Axis：坐标轴，包括x轴和y轴（x-axis，y-axis）。多个不同的数据列可共用同一个X轴或Y轴，当然，还可以有两个X轴或Y轴，分别显示在图表的上下或左右

### 配置选项

#### 全局配置

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnhdmes9m2j30dg05yjrr.jpg)

案例说明：

```python
Highcharts.setOptions({
  global: {  # 全局配置参数是针对所有Highcharts图表生效的配置，只能通过Highcharts.setOption函数来配置
    Date: object  # 用于高级时间处理的自定义时间类，例如 JDate 可以用于处理 Jalali 时间。
    VMLRadialGradientURL: String   # 用于在 VML 中渲染渐变效果的图片路径
    getTimezoneOffset: Function
    timezone: String
    timezoneOffset: Number
    useUTC: Boolean
  }

  lang: {  # 语言文字对象，全局设置，针对所有图标有效
    contextButtonTitle: String  # 导出按钮的标题
    decimalPoint: String  # 默认的小数点符号
    downloadJPEG: String  # 导出JPEG图片选项显示的文字
    downloadPDF: String   # 导出PDF选项显示的文字
    downloadPNG: String   # 导出PNG图片选项显示的文字
    downloadSVG: String   # 导出SVG图片选项显示的文字
    drillUpText: String   # 当图标下钻后会有一个返回按钮
    invalidDate: String   # 当时间值无效时显示的信息，默认是空字符串
    loading: String  # 当图标加载中状态时显示的文字
    months:Array<String>  # 月份数组，在日期格式化函数 Highcharts.dateFormat() 中月份格式字符 %B 会用到。 默认是：[ "January" , "February" , "March" , "April" , "May" , "June" , "July" , "August" , "September" , "October" , "November" , "December"]
    shortMonths: Array<String>   # 月份缩写数组，月份缩写数组，在时间格式化函数 Highcharts.dateFormat() 中缩写月份格式符 %b 中会用到。 默认是：[ "Jan" , "Feb" , "Mar" , "Apr" , "May" , "Jun" , "Jul" , "Aug" , "Sep" , "Oct" , "Nov" , "Dec"]
    weekdays: Array<String>  # 星期数组，默认是：["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
    shortWeekdays: Array<String>   # 星期的缩写，如果有指定该参数，那么 Highcharts 会默认使用 lang.weekdays 中对应的前三个字母。
    noData: String   # 没有数据显示的文字
    resetZoom: String  # 当图表缩放后重置缩放比例按钮的文字。 默认是：Reset zoom
    resetZoomTitle: String   # 重置缩放比例按钮的标题，默认是Reset zoom level 1:1
  }
})
```



#### 主配置

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnhdn4q2ebj30he0ra41u.jpg)

#### 函数与属性

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnhdo0njnxj30ew0bigmr.jpg)



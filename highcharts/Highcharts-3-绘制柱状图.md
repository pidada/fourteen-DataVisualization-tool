---
title: Highcharts-3-绘制柱状图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2021-02-10 12:39:19
password:
summary:
tags:
  - 可视化
  - 互联网
  - highcharts
categories:
  - datavisual
  - highcharts
---


### Highcharts-3-绘制柱状图

本文介绍的是如何利用`python-highcharts`绘制柱状图

- 水平/垂直柱状图
- 并行柱状图
- 堆叠柱状图

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gngkhp1fgkj30rm05sdg5.jpg)

<!--MORE-->

### 图形

首先我们直接看看最终的效果：

- 4个洲
- 5个年份

点击年份的时候会隐藏或者显示

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnhipenh0qj313m0u0whc.jpg)

隐藏其中一个年份：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnhisafj9jj312h0u0dim.jpg)



### 代码

#### 原始数据和代码

```python
from highcharts import Highchart   # 导入库
H = Highchart(width=750, height=600)  # 设置图形的大小

# 4组数据：代表的是4个年份
data1 = [107, 31, 235, 203, 24]  # 每个洲有一个数据
data2 = [133, 156, 947, 868, 106]
data3 = [373, 914, 854, 732, 34]
data4 = [652, 954, 1250, 740, 38]

# 进行配置
options = {
	'title': {  # 1、主标题
        'text': 'Stacked bar chart'
    },

    'subtitle': {   # 2、副标题：可以使用带有HTML标签的代码
        'text': 'Source: <a href="https://en.wikipedia.org/wiki/World_population">Wikipedia.org</a>'
    },

    'xAxis': {   # 3、横轴的5个分类
        'categories': ['Africa', 'America', 'Asia', 'Europe', 'Oceania'],
        'title': {
            'text': "5个洲"   # x轴的名称
        }
    },
    'yAxis': {
        'min': 0,  # 设置最小值
        'title': {
            'text': '人口数(百万)',  # y轴名称
            'align': 'high'
        },
        'labels': {
            'overflow': 'justify'
        }
    },
    'tooltip': {
        'valueSuffix': ' millions'
    },
    'legend': {  # 图例设置
        'layout': 'vertical',   # 垂直方向
        'align': 'right',  # 靠右显示
        'verticalAlign': 'top',   # 顶部
        'x': -40,
        'y': 80,
        'floating': True,
        'borderWidth': 2,    # 图例外围线条宽度
        'backgroundColor': "((Highcharts.theme && Highcharts.theme.legendBackgroundColor) || '#9ACFF0')", # 图例背景颜色
        'shadow': True
    },
    'credits': {  # 右下角的版权标签
        'enabled': True
    },
    'plotOptions': {
        'bar': {
            'dataLabels': {
                'enabled': True  # 显示数据（柱状图顶部的数据显示出来）
            }
        }
    }
}

H.set_dict_options(options)   # 添加配置

# 每个年份添加一组数据
H.add_data_set(data1, 'bar', 'Year 2000')
H.add_data_set(data2, 'bar', 'Year 2004')
H.add_data_set(data3, 'bar', 'Year 2008')
H.add_data_set(data4, 'bar', 'Year 2012')

# jupyter notebook中显示图形
H
```

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnhitkye1tj30u017qx1r.jpg)

#### 使用pandas模拟数据

使用`pandas`来模拟生成上面的数据

```python
# 使用DataFrame数据框

import pandas as pd

data = pd.DataFrame({
    "2000": [107, 31, 235, 203, 24],
    "2004": [133, 156, 947, 868, 106],
    "2008": [373, 914, 854, 732, 34],
    "2012": [652, 954, 1250, 740, 38]},

    index=['Africa', 'America', 'Asia', 'Europe', 'Oceania']
)

data
```

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gni8x2e4a4j30z00nsn0i.jpg)

生成需要的4个`data`数据和分类`categories`：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gni8yadnq9j30wa0ka76r.jpg)



#### 图形翻转

对上面的图形实现翻转效果，即显示为水平的柱状图，先看看最终的效果：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gniapkjq0hj317t0u0q61.jpg)

实现的方法只需要在上面的代码配置项中加上：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gniat6hzboj30oi084gmr.jpg)

完整代码如下：

```python
from highcharts import Highchart   # 导入库
H = Highchart(width=750, height=600)  # 设置图形的大小

# 4组数据，代表4个年份
# 每组5个数据代表的是5个洲
data1 = [107, 31, 235, 203, 24]
data2 = [133, 156, 947, 868, 106]
data3 = [373, 914, 854, 732, 34]
data4 = [652, 954, 1250, 740, 38]

# 进行配置
options = {
    'chart': {  # 加上chart配置变成水平柱状图
        'type': 'bar'
    },
	'title': {  # 1、主标题
        'text': 'Stacked bar chart'
    },

    'subtitle': {   # 2、副标题
        'text': 'Source: <a href="https://en.wikipedia.org/wiki/World_population">Wikipedia.org</a>'
    },

    'xAxis': {   # 3、横轴的5个分类
        'categories': ['Africa', 'America', 'Asia', 'Europe', 'Oceania'],
        'title': {
            'text': "5个洲"   # x轴的名称
        }
    },
    'yAxis': {
        'min': 0,  # 设置最小值
        'title': {
            'text': '人口数(百万)',  # y轴名称
            'align': 'high'
        },
        'labels': {
            'overflow': 'justify'
        }
    },
    'tooltip': {
        'valueSuffix': ' millions'
    },
    'legend': {  # 图例设置
        'layout': 'vertical',   # 垂直方向
        'align': 'right',  # 靠右显示
        'verticalAlign': 'top',   # 顶部
        'x': -40,
        'y': 80,
        'floating': True,
        'borderWidth': 2,    # 图例外围线条宽度
        'backgroundColor': "((Highcharts.theme && Highcharts.theme.legendBackgroundColor) || '#9ACFF0')",#图例背景颜色
        'shadow': True
    },
    'credits': {  # 右下角的版权标签
        'enabled': True
    },
    'plotOptions': {
        'bar': {
            'dataLabels': {
                'enabled': True  # 显示数据（柱状图顶部的数据显示出来）
            }
        }
    }
}

H.set_dict_options(options)   # 添加配置

# 每个年份添加一组数据
H.add_data_set(data1, 'bar', 'Year 2000')
H.add_data_set(data2, 'bar', 'Year 2004')
H.add_data_set(data3, 'bar', 'Year 2008')
H.add_data_set(data4, 'bar', 'Year 2012')

H
```

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gniatqjf3oj30u01qbnet.jpg)



### 并行柱状图

#### 效果图

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gni9vv6kzij30yq0lo0vk.jpg)

#### 原数据和代码

```python
from highcharts import Highchart
H = Highchart(width=550, height=400)

# 1、数值分类区间
categories = ['0-4', '5-9', '10-14', '15-19',
              '20-24', '25-29', '30-34', '35-39',
              '40-44','45-49', '50-54', '55-59',
              '60-64', '65-69','70-74', '75-79',
              '80-84', '85-89', '90-94','95-99', '100 + ']
# 2、配置项
# 在这种图形中横轴和纵轴需要调换
options = {
	'chart': {  #  指定图表类型：柱状图
        'type': 'bar'
    },
    'title': {  # 主标题
        'text': 'Population pyramid for Germany, midyear 2010'
    },
    'subtitle': {  # 副标题
        'text': 'Source: www.census.gov'
    },
    'xAxis': [{  # 左侧标签设置
        'categories': categories,
        'reversed': False,   # 分类区间是否翻转
        'labels': {
            'step': 1  # 标签区间的间隔
        }
    }, {   # 右侧标签设置
        'opposite': True,
        'reversed': False,
        'categories': categories,
        'linkedTo': 0,
        'labels': {
            'step': 1
        }
    }],
    'yAxis': {
        'title': {
            'text': None
        },
        'labels': {  # y轴标签
            'formatter': "function () {\
                                    return (Math.abs(this.value) / 1000000) + 'M';\
                                }"
        },
        'min': -4000000,
        'max': 4000000
    },

    'plotOptions': {
        'series': {
            'stacking': 'normal'
        }
    },

    'tooltip': {
        'formatter': "function () {\
                            return '<b>' + this.series.name + ', age ' + this.point.category + '</b><br/>' +\
                                'Population: ' + Highcharts.numberFormat(Math.abs(this.point.y), 0);\
                        }"
    },
}

# 设置男女的数值
data_male = [-1746181, -1884428, -2089758, -2222362,
             -2537431, -2507081, -2443179, -2664537,
             -3556505, -3680231, -3143062, -2721122,
             -2229181, -2227768,-2176300, -1329968,
             -836804, -354784, -90569, -28367, -3878]

data_female = [1656154, 1787564, 1981671, 2108575,
               2403438, 2366003, 2301402, 2519874,
               3360596, 3493473, 3050775, 2759560,
               2304444, 2426504, 2568938, 1785638,
               1447162, 1005011, 330870, 130632, 21208]

# 添加配置项
H.set_dict_options(options)

# 添加数据和指定图表类型bar
H.add_data_set(data_male, 'bar', 'Male')
H.add_data_set(data_female, 'bar', 'Female')

H
```

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gni9r97m90j30u01eakcj.jpg)

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gni9y65cg0j31cl0u00yv.jpg)

#### 适用场景

当两个组别之间存在多个数值区间的时候，适用用此种图表

### 堆叠柱状图-stack bar

下面的代码是根据官网的源码进行修改得到的最后实现代码

```python
from highcharts import Highchart   # 导入库
H = Highchart(width=800, height=600)  # 设置图形的大小

# 配置数据项：根据源码修改
data1 = [5, 3, 4, 7, 2]
data2 = [2, 2, 3, 2, 1]
data3 = [3, 4, 4, 2, 5]

options = {
    'chart': {
        'type': 'bar'  # 图表类型
    },
    'title': {  # 主标题
        'text': 'Stacked bar chart'
    },
    'xAxis': {
        'categories': ['Apples', 'Oranges', 'Pears', 'Grapes', 'Bananas']
    },
    'yAxis': {
        'min': 0,
        'title': {
            'text': 'Total fruit consumption'
        }
    },
    'legend': {
        'reversed': True
    },
    'plotOptions': {
        'series': {
            'stacking': 'normal'
        }
    }
}

# 增加部分
H.set_dict_options(options)   # 添加配置

H.add_data_set(data1,'bar','John')
H.add_data_set(data2,'bar','Jane')
H.add_data_set(data3,'bar','Joe')

H
```



![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnibhhxwc0j314t0u0dhk.jpg)

### 带有负值的柱状图-column with negative values

如何绘制带有负值的柱状图？

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnic1cfvolj30ou0ck408.jpg)

**在柱状图上方将数据显示出来的配置**：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnic0ta2n5j30t80780tt.jpg)

完整的代码如下所示：

```python
from highcharts import Highchart   # 导入库
H = Highchart(width=800, height=600)  # 设置图形的大小

# 配置数据项
data1 = [5, 3, -4, 7, 2]
data2 = [2, 2, 3, -2, 1]
data3 = [-3, 4, 4, 2, 5]

options = {
    'chart': {  # 图表类型不是bar，而是column
        'type': 'column'
    },
    'title': {  # 主标题
        'text': 'column with negative values'
    },
    'xAxis': {
        'categories': ['Apples', 'Oranges', 'Pears', 'Grapes', 'Bananas']
    },
    'yAxis': {
        'title': {
            'text': '水果数量',  # y轴名称
            'align': 'high'
        },
        'labels': {
            'overflow': 'justify'
        }
    },
    'legend': {
        'reversed': True
    },
    'credits': {  # 右下角的版权信息
        'enabled': False
    },
    'plotOptions': {   # 将每个数据在柱状图上方显示出来
        'bar': {
            'dataLabels': {
                'enabled': True  # 显示数据（柱状图顶部的数据显示出来）
            }
        }
    }
}

H.set_dict_options(options)   # 添加配置
H.add_data_set(data1,'bar','John')
H.add_data_set(data2,'bar','Jane')
H.add_data_set(data3,'bar','Joe')

H
```



最终呈现的效果图：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnibykvuhzj31470u00ug.jpg)



















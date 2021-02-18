---
title: Highcharts-4-堆叠柱状图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2021-02-18 12:42:10
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


### Highcharts-4-柱状图2

本文继续介绍`Highcharts`中柱状图的制作，主要讲解了3种柱状图的制作：

- 堆叠柱状图
- 分组堆叠柱状图
- 带有百分比堆叠柱状图

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gngkhp1fgkj30rm05sdg5.jpg)

<!--MORE-->

### 垂直堆叠柱状图

#### 效果图

先看下整体的效果图：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnl6djt9x0j314y0u0ac6.jpg)

#### 代码

将官网的源代码进行修改，使用`python-highcharts`来进行绘制。

- 设置options中需要将bar改成column
- add_set_data中需要将bar改成column

```python
from highcharts import Highchart   # 导入库
H = Highchart(width=800, height=600)  # 设置图形的大小

# 配置数据项
data1 = [5, 3, 4, 7, 2]
data2 = [2, 2, 3, 2, 1]
data3 = [3, 4, 4, 2, 5]

options = {
    'chart': {
        'type': 'column'   # bar改成column
    },
    'title': {
        'text': 'Stacked column chart'
    },
    'xAxis': {
        'categories': ['Apples', 'Oranges', 'Pears', 'Grapes', 'Bananas']
    },
    'yAxis': {
        'min': 0,
        'title': {
            'text': 'Total fruit consumption'
        },
        'stackLabels': {
            'enabled': True,
            'style': {
                'fontWeight': 'bold',
                'color': "(Highcharts.defaultOptions.title.style && \
                    Highcharts.defaultOptions.title.style.color) || 'gray'"
            }
        }
    },
    'legend': {
        'align': 'right',
        'x': -30,
        'verticalAlign': 'top',
        'y': 25,
        'floating': True,
        'backgroundColor':
            "Highcharts.defaultOptions.legend.backgroundColor || 'white'",
        'borderColor': '#CCC',
        'borderWidth': 1,
        'shadow': False
    },
    'tooltip': {
        'headerFormat': '<b>{point.x}</b><br/>',
        'pointFormat': '{series.name}: {point.y}<br/>Total: {point.stackTotal}'
    },
    # 在这里设置堆叠的信息
    'plotOptions': {   # 将每个数据在柱状图上方显示出来
        'column': {
            'stacking': 'normal',
            'dataLabels': {
                'enabled': True  # 显示数据（柱状图顶部的数据显示出来）
            }
        }
    }
}

H.set_dict_options(options)   # 添加配置

# 将之前的bar改成column即可
H.add_data_set(data1,'column','John')
H.add_data_set(data2,'column','Jane')
H.add_data_set(data3,'column','Joe')

H
```

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnl6g4hedij30u01f6wtn.jpg)

### 分组堆叠图-stack and group column

#### 效果图

先看下整体的效果：

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnrjhqp20dj318k0u0q5f.jpg)



有4个不同的人和5种不同的水果：用户之间用颜色区分，水果之间通过组别间隔开来

#### 代码

```python
# 导入库
from highcharts import Highchart
# 设置图形的大小
H = Highchart(width=750, height=600)

# 绘图数据
data1 = [5, 3, 3, 9, 2]
data2 = [7, 5, 4, 3, 5]
data3 = [2, 5, 6, 2, 8]
data4 = [3, 1, 8, 4, 3]

# 配置项
options = {
	'title': {    # 主标题
        'text': '基于性别的水果消费情况'
    },

    'xAxis': {  # x轴的分类数据
        'categories': ['Apples', 'Oranges', 'Pears', 'Grapes', 'Bananas']
    },

    'yAxis': {  # y轴设置
        'allowDecimals': False,
        'min': 0,
        'title': {
            'text': 'Number of fruits'
        }
    },

    'tooltip': {  # 设置显示数据情况
        'formatter': "function () {\
                        return '<b>' + this.x + '</b><br/>' +\
                            this.series.name + ': ' + this.y + '<br/>' +\
                            'Total: ' + this.point.stackTotal;\
                    }"
    },
    'plotOptions': {
        'column': {
            'stacking': 'normal',
#             'pointPadding': 0.2,
#             'borderWidth': 0,
#              'groupPadding': 0.1,  # x轴上每个组之间的距离
            'shadow': False,   # 是否显示柱状图的阴影

            'dataLabels': {  # 重点：将数据显示在柱子外面或里面
                'enabled': True,
                'inside': True
            }
        }
    }
}

# 添加配置项
H.set_dict_options(options)

# 添加数据项：stack参数设置
H.add_data_set(data1, 'column', 'John', stack='male' )
H.add_data_set(data2, 'column', 'Joe', stack='male')
H.add_data_set(data3, 'column', 'Jane', stack='female')
H.add_data_set(data4, 'column', 'Janet', stack='female')

H
```

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnrjk102uaj30u01f2tmp.jpg)

#### 重点配置项

**一个重点的配置项**：如何将数据显示在柱子外面或者里面，甚至是直接隐藏起来？

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnrjli3xztj30ti0ek76n.jpg)

### 带有百分比的柱状图-bar with percentage

#### 效果图

每个水果的整体柱子是一样的高度：100%；当鼠标放在

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnrjx14zmzj315g0u0gnv.jpg)





#### 代码

```python
from highcharts import Highchart   # 导入库
H = Highchart(width=800, height=600)  # 设置图形的大小

# 配置数据项
data1 = [5, 3, 4, 7, 2]
data2 = [2, 2, 3, 2, 1]
data3 = [3, 4, 4, 2, 5]

options = {
    'chart': {
        'type': 'column'  # 图表类型
    },
    'title': {  # 主标题
        'text': '带有百分比的柱状图'
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
    'tooltip': {
        'pointFormat': '<span style="color:{series.color}">{series.name}</span>: <b>{point.y}</b> ({point.percentage:.0f}%)<br/>',
        'shared': True
    },
    'legend': {
        'reversed': True
    },
    'plotOptions': {
        'series': {   # 将stacking参数设置成percent
            'stacking': 'percent'   # 多种取值：normal+percent
        }
    }
}

H.set_dict_options(options)   # 添加配置

H.add_data_set(data1,'bar','John')
H.add_data_set(data2,'bar','Jane')
H.add_data_set(data3,'bar','Joe')

H
```

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnrjya6opyj30ub0u0wq4.jpg)

#### 重要设置项

一个重要的设置项：``'stacking': 'percent'``

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnrkvitdlzj31cu0dsju1.jpg)



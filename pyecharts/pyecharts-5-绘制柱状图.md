---
title: pyecharts-5-绘制柱状图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-09-27 21:20:31
password:
summary:
tags:
  - 可视化
  - pyecharts
categories:
  - datavisual
  - pyecharts
---

### Pyecharts-5-绘制柱状图Bar

本文中介绍的是如何利用pyecharts中绘制各种bar柱状图。

主要是[参考官网](https://gallery.pyecharts.org/#/Bar/README)的各种例子进行学习和整理

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5gayx9boj31400u0wqw.jpg)

<!--MORE-->

### 导入各种库

```python
from pyecharts import options as opts
from pyecharts.charts import Bar
from pyecharts.faker import Faker
from pyecharts.commons.utils import JsCode
from pyecharts.globals import ThemeType

import pandas as pd
import numpy as np
```

### 数值+百分比


在柱状图中同时显示数值和百分比，能够对比不同的组之间的数据


```python
list2 = [
    {"value": 18, "percent": 18 / (18 + 3)},
    {"value": 23, "percent": 23 / (23 + 21)},
    {"value": 33, "percent": 33 / (33 + 5)}
]

list3 = [
    {"value": 3, "percent": 3 / (18 + 3)},
    {"value": 21, "percent": 21 / (23 + 21)},
    {"value": 5, "percent": 5 / (33 + 5)}
]

c = (
    Bar(init_opts=opts.InitOpts(theme=ThemeType.LIGHT)) # 使用的主题
    .add_xaxis([1, 2, 3])
    .add_yaxis("product1", list2, stack="stack1", category_gap="50%")   # 堆叠+间隔
    .add_yaxis("product2", list3, stack="stack1", category_gap="50%")
    .set_series_opts(
        label_opts=opts.LabelOpts(
            position="right",
            formatter=JsCode(  # 显示的形式
                "function(x){return Number(x.data.percent * 100).toFixed() + '%';}"
            ),
        )
    )
)

c.render_notebook()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5igl1bnvj31b20qamyh.jpg)




### x轴标题斜体

解决x轴标题很长的问题


```python
c = (
    Bar()
    .add_xaxis(
        [
            "名字很长的X轴标签1",   # x轴标签
            "名字很长的X轴标签2",
            "名字很长的X轴标签3"
        ]
    )
    .add_yaxis("商家A", [10, 20, 30])   # 加入两组y轴数据
    .add_yaxis("商家B", [20, 10, 40])
    .set_global_opts(
        xaxis_opts=opts.AxisOpts(axislabel_opts=opts.LabelOpts(rotate=-15)),  # 设置旋转角度
        title_opts=opts.TitleOpts(title="Bar-旋转X轴标签", subtitle="标题真的很长"),  # 主标题和副标题
    )
)

c.render_notebook()
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5ih1rybmj31ke0romz7.jpg)


### 堆叠柱状图-stack


```python
c = (
    Bar()
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A", Faker.values(), stack="stack1")   # 两组堆叠数据
    .add_yaxis("商家B", Faker.values(), stack="stack1")
    .add_yaxis("商家C", Faker.values())   # 不参与堆叠
    .set_series_opts(label_opts=opts.LabelOpts(is_show=False))
    .set_global_opts(title_opts=opts.TitleOpts(title="Bar-堆叠数据（部分）"))
#     .render("bar_stack1.html")  用于生成html格式文件
)

c.render_notebook()  # notebook在线显示
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5ii2ce3dj31dg0r2wg5.jpg)


### 通过字典设置主副标题


```python
c = (
    Bar({"theme": ThemeType.LIGHT})
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A", Faker.values())
    .add_yaxis("商家B", Faker.values())
    .set_global_opts(
        # 通过字典形式设置两个标题
        title_opts={"text": "主标题（字典设置）", "subtext": "副标题（字典配置）"}
    )
#     .render("bar_base_dict_config.html")
)

c.render_notebook()
```

![image-20200927210825506](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5ijp7nb3j31ip0u077w.jpg)




### 加入工具栏目

右上角的工具栏进行多种操作


```python
c = (
    Bar()
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A", Faker.values())
    .add_yaxis("商家B", Faker.values())
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Bar-显示 ToolBox"),
        toolbox_opts=opts.ToolboxOpts(),
        legend_opts=opts.LegendOpts(is_show=False),
    )
#     .render("bar_toolbox.html")
)

c.render_notebook()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5iknxf8nj31iu0r040o.jpg)


### 设置xy轴名称


```python
c = (
    Bar()
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A", Faker.values())
    .add_yaxis("商家B", Faker.values())
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Bar-XY设置新名字"),
        yaxis_opts=opts.AxisOpts(name="Y轴新名字"),
        xaxis_opts=opts.AxisOpts(name="X轴新名字"),
    )
#     .render("bar_xyaxis_name.html")
)

c.render_notebook()
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5ilxu9srj31fs0rsq55.jpg)




### 垂直缩放

按照数值大小进行滑动


```python
c = (
    Bar()
    .add_xaxis(Faker.days_attrs)
    .add_yaxis("商家A", Faker.days_values, color=Faker.rand_color())
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Bar-DataZoom（垂直活动条）"),
        datazoom_opts=opts.DataZoomOpts(orient="vertical"),
    )
#     .render("bar_datazoom_slider_vertical.html")
)

c.render_notebook()
```



![image-20200927211107338](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5imgzwbxj31h60r2go0.jpg)




### 水平缩放


```python
c = (
    Bar()
    .add_xaxis(Faker.days_attrs)
    .add_yaxis("商家A", Faker.days_values)
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Bar-DataZoom（slider-水平）"),
        datazoom_opts=opts.DataZoomOpts(),
    )
#     .render("bar_datazoom_slider.html")
)

c.render_notebook()
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5inhz6nsj31he0t40v1.jpg)






### 双边滑动（水平方向）


```python
c = (
    Bar()
    .add_xaxis(Faker.days_attrs)
    .add_yaxis("商家A", Faker.days_values, color=Faker.rand_color())
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Bar-DataZoom（水平方向双边滑动）"),
        datazoom_opts=[opts.DataZoomOpts(), opts.DataZoomOpts(type_="inside")],
    )
#     .render("bar_datazoom_both.html")
)

c.render_notebook()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5ipb8mc7j31fs0tcgnz.jpg)






### 绘制直方图


```python
c = (
    Bar()
    .add_xaxis(Faker.choose())
    # 将间隔设置成0就变成了直方图histogram
    .add_yaxis("商家A", Faker.values(), category_gap=0, color=Faker.rand_color())
    .set_global_opts(title_opts=opts.TitleOpts(title="Bar-直方图"))
#     .render("bar_histogram.html")
)

c.render_notebook()
```





![image-20200927211454708](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5iqezvyej31ay0q475s.jpg)






### 水平柱状图


```python
c = (
    Bar()
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A", Faker.values(), category_gap=0)
    .add_yaxis("商家B", Faker.values(), category_gap=0)
    .reversal_axis()   #  翻转坐标轴
    .set_series_opts(label_opts=opts.LabelOpts(position="right"))   # label的位置
    .set_global_opts(title_opts=opts.TitleOpts(title="水平柱状图(间隔为0)"))
#     .render("bar_reversal_axis.html")
)

c.render_notebook()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5ir0dmnsj31co0rqdhv.jpg)





```python
c = (
    Bar()
    .add_xaxis(Faker.choose())
    .add_yaxis("商家A", Faker.values())
    .add_yaxis("商家B", Faker.values())
    .reversal_axis()   #  翻转坐标轴
    .set_series_opts(label_opts=opts.LabelOpts(position="right"))   # label的位置
    .set_global_opts(title_opts=opts.TitleOpts(title="水平柱状图"))
#     .render("bar_reversal_axis.html")
)

c.render_notebook()
```





![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5irs13i5j31d60r0abz.jpg)








### 多个指标联动


```python
import pyecharts.options as opts
from pyecharts.charts import Timeline, Bar, Pie


total_data = {}
name_list = [
    "北京",
    "天津",
    "河北",
    "山西",
    "内蒙古",
    "辽宁",
    "吉林",
    "黑龙江",
    "上海",
    "江苏",
    "浙江",
    "安徽",
    "福建",
    "江西",
    "山东",
    "河南",
    "湖北",
    "湖南",
    "广东",
    "广西",
    "海南",
    "重庆",
    "四川",
    "贵州",
    "云南",
    "西藏",
    "陕西",
    "甘肃",
    "青海",
    "宁夏",
    "新疆",
]

data_gdp = {
    2011: [
        16251.93,
        11307.28,
        24515.76,
        11237.55,
        14359.88,
        22226.7,
        10568.83,
        12582,
        19195.69,
        49110.27,
        32318.85,
        15300.65,
        17560.18,
        11702.82,
        45361.85,
        26931.03,
        19632.26,
        19669.56,
        53210.28,
        11720.87,
        2522.66,
        10011.37,
        21026.68,
        5701.84,
        8893.12,
        605.83,
        12512.3,
        5020.37,
        1670.44,
        2102.21,
        6610.05,
    ],
    2010: [
        14113.58,
        9224.46,
        20394.26,
        9200.86,
        11672,
        18457.27,
        8667.58,
        10368.6,
        17165.98,
        41425.48,
        27722.31,
        12359.33,
        14737.12,
        9451.26,
        39169.92,
        23092.36,
        15967.61,
        16037.96,
        46013.06,
        9569.85,
        2064.5,
        7925.58,
        17185.48,
        4602.16,
        7224.18,
        507.46,
        10123.48,
        4120.75,
        1350.43,
        1689.65,
        5437.47,
    ],
    2009: [
        12153.03,
        7521.85,
        17235.48,
        7358.31,
        9740.25,
        15212.49,
        7278.75,
        8587,
        15046.45,
        34457.3,
        22990.35,
        10062.82,
        12236.53,
        7655.18,
        33896.65,
        19480.46,
        12961.1,
        13059.69,
        39482.56,
        7759.16,
        1654.21,
        6530.01,
        14151.28,
        3912.68,
        6169.75,
        441.36,
        8169.8,
        3387.56,
        1081.27,
        1353.31,
        4277.05,
    ],
    2008: [
        11115,
        6719.01,
        16011.97,
        7315.4,
        8496.2,
        13668.58,
        6426.1,
        8314.37,
        14069.87,
        30981.98,
        21462.69,
        8851.66,
        10823.01,
        6971.05,
        30933.28,
        18018.53,
        11328.92,
        11555,
        36796.71,
        7021,
        1503.06,
        5793.66,
        12601.23,
        3561.56,
        5692.12,
        394.85,
        7314.58,
        3166.82,
        1018.62,
        1203.92,
        4183.21,
    ],
    2007: [
        9846.81,
        5252.76,
        13607.32,
        6024.45,
        6423.18,
        11164.3,
        5284.69,
        7104,
        12494.01,
        26018.48,
        18753.73,
        7360.92,
        9248.53,
        5800.25,
        25776.91,
        15012.46,
        9333.4,
        9439.6,
        31777.01,
        5823.41,
        1254.17,
        4676.13,
        10562.39,
        2884.11,
        4772.52,
        341.43,
        5757.29,
        2703.98,
        797.35,
        919.11,
        3523.16,
    ],
    2006: [
        8117.78,
        4462.74,
        11467.6,
        4878.61,
        4944.25,
        9304.52,
        4275.12,
        6211.8,
        10572.24,
        21742.05,
        15718.47,
        6112.5,
        7583.85,
        4820.53,
        21900.19,
        12362.79,
        7617.47,
        7688.67,
        26587.76,
        4746.16,
        1065.67,
        3907.23,
        8690.24,
        2338.98,
        3988.14,
        290.76,
        4743.61,
        2277.35,
        648.5,
        725.9,
        3045.26,
    ]
}

data_pi = {
    2011: [
        136.27,
        159.72,
        2905.73,
        641.42,
        1306.3,
        1915.57,
        1277.44,
        1701.5,
        124.94,
        3064.78,
        1583.04,
        2015.31,
        1612.24,
        1391.07,
        3973.85,
        3512.24,
        2569.3,
        2768.03,
        2665.2,
        2047.23,
        659.23,
        844.52,
        2983.51,
        726.22,
        1411.01,
        74.47,
        1220.9,
        678.75,
        155.08,
        184.14,
        1139.03,
    ],
    2010: [
        124.36,
        145.58,
        2562.81,
        554.48,
        1095.28,
        1631.08,
        1050.15,
        1302.9,
        114.15,
        2540.1,
        1360.56,
        1729.02,
        1363.67,
        1206.98,
        3588.28,
        3258.09,
        2147,
        2325.5,
        2286.98,
        1675.06,
        539.83,
        685.38,
        2482.89,
        625.03,
        1108.38,
        68.72,
        988.45,
        599.28,
        134.92,
        159.29,
        1078.63,
    ],
    2009: [
        118.29,
        128.85,
        2207.34,
        477.59,
        929.6,
        1414.9,
        980.57,
        1154.33,
        113.82,
        2261.86,
        1163.08,
        1495.45,
        1182.74,
        1098.66,
        3226.64,
        2769.05,
        1795.9,
        1969.69,
        2010.27,
        1458.49,
        462.19,
        606.8,
        2240.61,
        550.27,
        1067.6,
        63.88,
        789.64,
        497.05,
        107.4,
        127.25,
        759.74,
    ],
    2008: [
        112.83,
        122.58,
        2034.59,
        313.58,
        907.95,
        1302.02,
        916.72,
        1088.94,
        111.8,
        2100.11,
        1095.96,
        1418.09,
        1158.17,
        1060.38,
        3002.65,
        2658.78,
        1780,
        1892.4,
        1973.05,
        1453.75,
        436.04,
        575.4,
        2216.15,
        539.19,
        1020.56,
        60.62,
        753.72,
        462.27,
        105.57,
        118.94,
        691.07,
    ],
    2007: [
        101.26,
        110.19,
        1804.72,
        311.97,
        762.1,
        1133.42,
        783.8,
        915.38,
        101.84,
        1816.31,
        986.02,
        1200.18,
        1002.11,
        905.77,
        2509.14,
        2217.66,
        1378,
        1626.48,
        1695.57,
        1241.35,
        361.07,
        482.39,
        2032,
        446.38,
        837.35,
        54.89,
        592.63,
        387.55,
        83.41,
        97.89,
        628.72,
    ],
    2006: [
        88.8,
        103.35,
        1461.81,
        276.77,
        634.94,
        939.43,
        672.76,
        750.14,
        93.81,
        1545.05,
        925.1,
        1011.03,
        865.98,
        786.14,
        2138.9,
        1916.74,
        1140.41,
        1272.2,
        1532.17,
        1032.47,
        323.48,
        386.38,
        1595.48,
        382.06,
        724.4,
        50.9,
        484.81,
        334,
        67.55,
        79.54,
        527.8,
    ]
}

data_si = {
    2011: [
        3752.48,
        5928.32,
        13126.86,
        6635.26,
        8037.69,
        12152.15,
        5611.48,
        5962.41,
        7927.89,
        25203.28,
        16555.58,
        8309.38,
        9069.2,
        6390.55,
        24017.11,
        15427.08,
        9815.94,
        9361.99,
        26447.38,
        5675.32,
        714.5,
        5543.04,
        11029.13,
        2194.33,
        3780.32,
        208.79,
        6935.59,
        2377.83,
        975.18,
        1056.15,
        3225.9,
    ],
    2010: [
        3388.38,
        4840.23,
        10707.68,
        5234,
        6367.69,
        9976.82,
        4506.31,
        5025.15,
        7218.32,
        21753.93,
        14297.93,
        6436.62,
        7522.83,
        5122.88,
        21238.49,
        13226.38,
        7767.24,
        7343.19,
        23014.53,
        4511.68,
        571,
        4359.12,
        8672.18,
        1800.06,
        3223.49,
        163.92,
        5446.1,
        1984.97,
        744.63,
        827.91,
        2592.15,
    ],
    2009: [
        2855.55,
        3987.84,
        8959.83,
        3993.8,
        5114,
        7906.34,
        3541.92,
        4060.72,
        6001.78,
        18566.37,
        11908.49,
        4905.22,
        6005.3,
        3919.45,
        18901.83,
        11010.5,
        6038.08,
        5687.19,
        19419.7,
        3381.54,
        443.43,
        3448.77,
        6711.87,
        1476.62,
        2582.53,
        136.63,
        4236.42,
        1527.24,
        575.33,
        662.32,
        1929.59,
    ],
    2008: [
        2626.41,
        3709.78,
        8701.34,
        4242.36,
        4376.19,
        7158.84,
        3097.12,
        4319.75,
        6085.84,
        16993.34,
        11567.42,
        4198.93,
        5318.44,
        3554.81,
        17571.98,
        10259.99,
        5082.07,
        5028.93,
        18502.2,
        3037.74,
        423.55,
        3057.78,
        5823.39,
        1370.03,
        2452.75,
        115.56,
        3861.12,
        1470.34,
        557.12,
        609.98,
        2070.76,
    ],
    2007: [
        2509.4,
        2892.53,
        7201.88,
        3454.49,
        3193.67,
        5544.14,
        2475.45,
        3695.58,
        5571.06,
        14471.26,
        10154.25,
        3370.96,
        4476.42,
        2975.53,
        14647.53,
        8282.83,
        4143.06,
        3977.72,
        16004.61,
        2425.29,
        364.26,
        2368.53,
        4648.79,
        1124.79,
        2038.39,
        98.48,
        2986.46,
        1279.32,
        419.03,
        455.04,
        1647.55,
    ],
    2006: [
        2191.43,
        2457.08,
        6110.43,
        2755.66,
        2374.96,
        4566.83,
        1915.29,
        3365.31,
        4969.95,
        12282.89,
        8511.51,
        2711.18,
        3695.04,
        2419.74,
        12574.03,
        6724.61,
        3365.08,
        3187.05,
        13469.77,
        1878.56,
        308.62,
        1871.65,
        3775.14,
        967.54,
        1705.83,
        80.1,
        2452.44,
        1043.19,
        331.91,
        351.58,
        1459.3,
    ]
}

data_ti = {
    2011: [
        12363.18,
        5219.24,
        8483.17,
        3960.87,
        5015.89,
        8158.98,
        3679.91,
        4918.09,
        11142.86,
        20842.21,
        14180.23,
        4975.96,
        6878.74,
        3921.2,
        17370.89,
        7991.72,
        7247.02,
        7539.54,
        24097.7,
        3998.33,
        1148.93,
        3623.81,
        7014.04,
        2781.29,
        3701.79,
        322.57,
        4355.81,
        1963.79,
        540.18,
        861.92,
        2245.12,
    ],
    2010: [
        10600.84,
        4238.65,
        7123.77,
        3412.38,
        4209.03,
        6849.37,
        3111.12,
        4040.55,
        9833.51,
        17131.45,
        12063.82,
        4193.69,
        5850.62,
        3121.4,
        14343.14,
        6607.89,
        6053.37,
        6369.27,
        20711.55,
        3383.11,
        953.67,
        2881.08,
        6030.41,
        2177.07,
        2892.31,
        274.82,
        3688.93,
        1536.5,
        470.88,
        702.45,
        1766.69,
    ],
    2009: [
        9179.19,
        3405.16,
        6068.31,
        2886.92,
        3696.65,
        5891.25,
        2756.26,
        3371.95,
        8930.85,
        13629.07,
        9918.78,
        3662.15,
        5048.49,
        2637.07,
        11768.18,
        5700.91,
        5127.12,
        5402.81,
        18052.59,
        2919.13,
        748.59,
        2474.44,
        5198.8,
        1885.79,
        2519.62,
        240.85,
        3143.74,
        1363.27,
        398.54,
        563.74,
        1587.72,
    ],
    2008: [
        8375.76,
        2886.65,
        5276.04,
        2759.46,
        3212.06,
        5207.72,
        2412.26,
        2905.68,
        7872.23,
        11888.53,
        8799.31,
        3234.64,
        4346.4,
        2355.86,
        10358.64,
        5099.76,
        4466.85,
        4633.67,
        16321.46,
        2529.51,
        643.47,
        2160.48,
        4561.69,
        1652.34,
        2218.81,
        218.67,
        2699.74,
        1234.21,
        355.93,
        475,
        1421.38,
    ],
    2007: [
        7236.15,
        2250.04,
        4600.72,
        2257.99,
        2467.41,
        4486.74,
        2025.44,
        2493.04,
        6821.11,
        9730.91,
        7613.46,
        2789.78,
        3770,
        1918.95,
        8620.24,
        4511.97,
        3812.34,
        3835.4,
        14076.83,
        2156.76,
        528.84,
        1825.21,
        3881.6,
        1312.94,
        1896.78,
        188.06,
        2178.2,
        1037.11,
        294.91,
        366.18,
        1246.89,
    ],
    2006: [
        5837.55,
        1902.31,
        3895.36,
        1846.18,
        1934.35,
        3798.26,
        1687.07,
        2096.35,
        5508.48,
        7914.11,
        6281.86,
        2390.29,
        3022.83,
        1614.65,
        7187.26,
        3721.44,
        3111.98,
        3229.42,
        11585.82,
        1835.12,
        433.57,
        1649.2,
        3319.62,
        989.38,
        1557.91,
        159.76,
        1806.36,
        900.16,
        249.04,
        294.78,
        1058.16,
    ]
}

data_estate = {
    2011: [
        12363.18,
        5219.24,
        8483.17,
        3960.87,
        5015.89,
        8158.98,
        3679.91,
        4918.09,
        11142.86,
        20842.21,
        14180.23,
        4975.96,
        6878.74,
        3921.2,
        17370.89,
        7991.72,
        7247.02,
        7539.54,
        24097.7,
        3998.33,
        1148.93,
        3623.81,
        7014.04,
        2781.29,
        3701.79,
        322.57,
        4355.81,
        1963.79,
        540.18,
        861.92,
        2245.12,
    ],
    2010: [
        10600.84,
        4238.65,
        7123.77,
        3412.38,
        4209.03,
        6849.37,
        3111.12,
        4040.55,
        9833.51,
        17131.45,
        12063.82,
        4193.69,
        5850.62,
        3121.4,
        14343.14,
        6607.89,
        6053.37,
        6369.27,
        20711.55,
        3383.11,
        953.67,
        2881.08,
        6030.41,
        2177.07,
        2892.31,
        274.82,
        3688.93,
        1536.5,
        470.88,
        702.45,
        1766.69,
    ],
    2009: [
        9179.19,
        3405.16,
        6068.31,
        2886.92,
        3696.65,
        5891.25,
        2756.26,
        3371.95,
        8930.85,
        13629.07,
        9918.78,
        3662.15,
        5048.49,
        2637.07,
        11768.18,
        5700.91,
        5127.12,
        5402.81,
        18052.59,
        2919.13,
        748.59,
        2474.44,
        5198.8,
        1885.79,
        2519.62,
        240.85,
        3143.74,
        1363.27,
        398.54,
        563.74,
        1587.72,
    ],
    2008: [
        8375.76,
        2886.65,
        5276.04,
        2759.46,
        3212.06,
        5207.72,
        2412.26,
        2905.68,
        7872.23,
        11888.53,
        8799.31,
        3234.64,
        4346.4,
        2355.86,
        10358.64,
        5099.76,
        4466.85,
        4633.67,
        16321.46,
        2529.51,
        643.47,
        2160.48,
        4561.69,
        1652.34,
        2218.81,
        218.67,
        2699.74,
        1234.21,
        355.93,
        475,
        1421.38,
    ],
    2007: [
        7236.15,
        2250.04,
        4600.72,
        2257.99,
        2467.41,
        4486.74,
        2025.44,
        2493.04,
        6821.11,
        9730.91,
        7613.46,
        2789.78,
        3770,
        1918.95,
        8620.24,
        4511.97,
        3812.34,
        3835.4,
        14076.83,
        2156.76,
        528.84,
        1825.21,
        3881.6,
        1312.94,
        1896.78,
        188.06,
        2178.2,
        1037.11,
        294.91,
        366.18,
        1246.89,
    ],
    2006: [
        5837.55,
        1902.31,
        3895.36,
        1846.18,
        1934.35,
        3798.26,
        1687.07,
        2096.35,
        5508.48,
        7914.11,
        6281.86,
        2390.29,
        3022.83,
        1614.65,
        7187.26,
        3721.44,
        3111.98,
        3229.42,
        11585.82,
        1835.12,
        433.57,
        1649.2,
        3319.62,
        989.38,
        1557.91,
        159.76,
        1806.36,
        900.16,
        249.04,
        294.78,
        1058.16,
    ]
}

data_financial = {
    2011: [
        12363.18,
        5219.24,
        8483.17,
        3960.87,
        5015.89,
        8158.98,
        3679.91,
        4918.09,
        11142.86,
        20842.21,
        14180.23,
        4975.96,
        6878.74,
        3921.2,
        17370.89,
        7991.72,
        7247.02,
        7539.54,
        24097.7,
        3998.33,
        1148.93,
        3623.81,
        7014.04,
        2781.29,
        3701.79,
        322.57,
        4355.81,
        1963.79,
        540.18,
        861.92,
        2245.12,
    ],
    2010: [
        10600.84,
        4238.65,
        7123.77,
        3412.38,
        4209.03,
        6849.37,
        3111.12,
        4040.55,
        9833.51,
        17131.45,
        12063.82,
        4193.69,
        5850.62,
        3121.4,
        14343.14,
        6607.89,
        6053.37,
        6369.27,
        20711.55,
        3383.11,
        953.67,
        2881.08,
        6030.41,
        2177.07,
        2892.31,
        274.82,
        3688.93,
        1536.5,
        470.88,
        702.45,
        1766.69,
    ],
    2009: [
        9179.19,
        3405.16,
        6068.31,
        2886.92,
        3696.65,
        5891.25,
        2756.26,
        3371.95,
        8930.85,
        13629.07,
        9918.78,
        3662.15,
        5048.49,
        2637.07,
        11768.18,
        5700.91,
        5127.12,
        5402.81,
        18052.59,
        2919.13,
        748.59,
        2474.44,
        5198.8,
        1885.79,
        2519.62,
        240.85,
        3143.74,
        1363.27,
        398.54,
        563.74,
        1587.72,
    ],
    2008: [
        8375.76,
        2886.65,
        5276.04,
        2759.46,
        3212.06,
        5207.72,
        2412.26,
        2905.68,
        7872.23,
        11888.53,
        8799.31,
        3234.64,
        4346.4,
        2355.86,
        10358.64,
        5099.76,
        4466.85,
        4633.67,
        16321.46,
        2529.51,
        643.47,
        2160.48,
        4561.69,
        1652.34,
        2218.81,
        218.67,
        2699.74,
        1234.21,
        355.93,
        475,
        1421.38,
    ],
    2007: [
        7236.15,
        2250.04,
        4600.72,
        2257.99,
        2467.41,
        4486.74,
        2025.44,
        2493.04,
        6821.11,
        9730.91,
        7613.46,
        2789.78,
        3770,
        1918.95,
        8620.24,
        4511.97,
        3812.34,
        3835.4,
        14076.83,
        2156.76,
        528.84,
        1825.21,
        3881.6,
        1312.94,
        1896.78,
        188.06,
        2178.2,
        1037.11,
        294.91,
        366.18,
        1246.89,
    ],
    2006: [
        5837.55,
        1902.31,
        3895.36,
        1846.18,
        1934.35,
        3798.26,
        1687.07,
        2096.35,
        5508.48,
        7914.11,
        6281.86,
        2390.29,
        3022.83,
        1614.65,
        7187.26,
        3721.44,
        3111.98,
        3229.42,
        11585.82,
        1835.12,
        433.57,
        1649.2,
        3319.62,
        989.38,
        1557.91,
        159.76,
        1806.36,
        900.16,
        249.04,
        294.78,
        1058.16,
    ]
}


def format_data(data: dict) -> dict:
    for year in range(2006, 2012):
        max_data, sum_data = 0, 0
        temp = data[year]
        max_data = max(temp)
        for i in range(len(temp)):
            sum_data += temp[i]
            data[year][i] = {"name": name_list[i], "value": temp[i]}
        data[str(year) + "max"] = int(max_data / 100) * 100
        data[str(year) + "sum"] = sum_data
    return data


# GDP
total_data["dataGDP"] = format_data(data=data_gdp)
# 第一产业
total_data["dataPI"] = format_data(data=data_pi)
# 第二产业
total_data["dataSI"] = format_data(data=data_si)
# 第三产业
total_data["dataTI"] = format_data(data=data_ti)
# 房地产
total_data["dataEstate"] = format_data(data=data_estate)
# 金融
total_data["dataFinancial"] = format_data(data=data_financial)


# 2006 - 2011 年的数据
def get_year_overlap_chart(year: int) -> Bar:
    bar = (
        Bar()
        .add_xaxis(xaxis_data=name_list)
        .add_yaxis(
            series_name="GDP",
            yaxis_data=total_data["dataGDP"][year],
            is_selected=False,
            label_opts=opts.LabelOpts(is_show=False),
        )
        .add_yaxis(
            series_name="金融",
            yaxis_data=total_data["dataFinancial"][year],
            is_selected=False,
            label_opts=opts.LabelOpts(is_show=False),
        )
        .add_yaxis(
            series_name="房地产",
            yaxis_data=total_data["dataEstate"][year],
            is_selected=False,
            label_opts=opts.LabelOpts(is_show=False),
        )
        .add_yaxis(
            series_name="第一产业",
            yaxis_data=total_data["dataPI"][year],
            label_opts=opts.LabelOpts(is_show=False),
        )
        .add_yaxis(
            series_name="第二产业",
            yaxis_data=total_data["dataSI"][year],
            label_opts=opts.LabelOpts(is_show=False),
        )
        .add_yaxis(
            series_name="第三产业",
            yaxis_data=total_data["dataTI"][year],
            label_opts=opts.LabelOpts(is_show=False),
        )
        .set_global_opts(
            title_opts=opts.TitleOpts(
                title="{}全国宏观经济指标".format(year), subtitle="数据来自国家统计局"
            ),
            tooltip_opts=opts.TooltipOpts(
                is_show=True, trigger="axis", axis_pointer_type="shadow"
            ),
        )
    )
    pie = (
        Pie()
        .add(
            series_name="GDP占比",
            data_pair=[
                ["第一产业", total_data["dataPI"]["{}sum".format(year)]],
                ["第二产业", total_data["dataSI"]["{}sum".format(year)]],
                ["第三产业", total_data["dataTI"]["{}sum".format(year)]],
            ],
            center=["75%", "25%"],
            radius="28%",
        )
        .set_series_opts(tooltip_opts=opts.TooltipOpts(is_show=True, trigger="item"))
    )
    return bar.overlap(pie)


# 生成时间轴的图
timeline = Timeline(init_opts=opts.InitOpts(width="1600px", height="800px"))

for y in range(2006, 2012):
    timeline.add(get_year_overlap_chart(year=y), time_point=str(y))

timeline.add_schema(is_auto_play=True, play_interval=1000)
# timeline.render("finance_indices_2002.html")
timeline.render_notebook()
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gj5iukk3o2j311s0u0jtf.jpg)



感谢官网的资料，个人整理主要是为了更好地使用😊



---
title: plotly_express-1-入门介绍
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-05-22 17:12:23
password:
summary:
tags:
  - plotly_express
  - plotly
  - datavisual
categories:
  - datavisual
  - px
---



Python的一个高级可视化库plotly_express是目前使用和见识过最棒的可视化库，通过这篇文章来入门这个可视化神器。

这篇文章可能不仅仅是入门😃
![](https://i.loli.net/2020/05/19/aTLeKNvYnD9AFhs.png)

<!--MORE-->

$\color{red}{Attention}$：下面的所有图在jupyter notebool均是动态可交互式图形

$\color{red}{Attention}$：下面的所有图在jupyter notebool均是动态可交互式图形

$\color{red}{Attention}$：下面的所有图在jupyter notebool均是动态可交互式图形



### 什么是plotly_express

首先，我们看看官网上对plotly_express的定义：

> Plotly Express is the easy-to-use, high-level interface to Plotly, which [operates on "tidy" data](https://plotly.com/python/px-arguments/) and produces [easy-to-style figures](https://plotly.com/python/styling-plotly-express/).
>
> Every Plotly Express function returns a `graph_objects.Figure` object whose `data` and `layout` has been pre-populated according to the provided arguments.

`Plotly Express` 是一个新的高级 `Python` 可视化库：它是 `Plotly.py` 的高级封装，它为复杂的图表提供了一个简单的语法，内置了大量实用、现代的绘图模板，**用户只需调用简单的API函数**，即可快速生成漂亮的互动图表。

### 安装与使用

安装非常简单，使用pip命令即可

使用的时候，导入import之后一般是用`px`别名

```python
pip install plotly_express   # 安装
import plotly_express as px  # 导入
```

### 内置数据

px中有几个内置数据，可以供用户使用

![](https://i.loli.net/2020/05/19/DhqdUgGKtYXx5nB.png)

比如使用其中著名的`iris`数据，见如下的使用方法：

![image.png](https://i.loli.net/2020/05/19/jTQ9KEcbR4GYNFp.png)

### 使用内置数据集Gapminder

#### 如何使用内置数据集

##### 选择数据

```python
gapminder = px.data.gapminder()
gapminder.head()
```

![](https://i.loli.net/2020/05/19/ibTdDxa7v3kClXQ.png)

##### 查看属性

![](https://i.loli.net/2020/05/19/Z2ThBileo9ntcMd.png)

#### line图

设置线条类型

```python
# line 图
fig = px.line(gapminder,x="year",y="lifeExp",color="continent",line_group="continent",
              hover_name="country",line_shape="spline",render_mode="svg")
fig.show()
```

![image.png](https://i.loli.net/2020/05/19/sRm5q4wCNraHbMJ.png)

#### 堆积区域图area

```python
# area 图
fig = px.area(gapminder,x="year",y="pop",color="continent",line_group="country")
fig.show()
```

![](https://i.loli.net/2020/05/19/wLVeMRZzICTmpfA.png)

#### 散点图

散点图是最简单的图形，有两个属性即可作图。下面的color属性表示颜色根据哪个属性

```python
gapminder2007 = gapminder.query("year==2007")
px.scatter(gapminder2007,x="gdpPercap",y="lifeExp")
```

**注意各个参数的含义**

![](https://i.loli.net/2020/05/19/sQVmeh2qlRE3L4B.png)

**改变每个点的大小-size**

```python
px.scatter(gapminder2007,x="gdpPercap",y="lifeExp"
           ,color="continent"  # 区分颜色
           ,size="pop"   # 区分圆的大小
           ,size_max=60)
```

![](https://i.loli.net/2020/05/19/jofK2TUEWSQiDpz.png)

#### 悬停参数-hover_name

```python
px.scatter(gapminder2007
           ,x="gdpPercap"
           ,y="lifeExp"
           ,color="continent"  # 区分颜色
           ,size="pop"   # 区分圆的大小
           ,size_max=60
           ,hover_name="country"
           ,log_x=True)
```

![](https://i.loli.net/2020/05/19/8LxUFqICZ2uENsJ.png)

#### 分块facet_col+滚动条animation_frame

```python
px.scatter(gapminder   # 绘图使用的数据
           ,x="gdpPercap" # 横纵坐标使用的数据
           ,y="lifeExp"
           ,color="continent"  # 区分颜色的属性
           ,size="pop"   # 区分圆的大小
           ,size_max=60  # 圆的最大值
           ,hover_name="country"  # 图中可视化最上面的名字
           ,animation_frame="year"  # 横轴滚动栏的属性year
           ,animation_group="country"
           ,facet_col="continent"   # 按照国家country属性进行分格显示
           ,log_x=True
           ,range_x=[100,100000]
           ,range_y=[25,90]
           ,labels=dict(pop="Populations",  # 属性名字的变化，更直观
                       gdpPercap="GDP per Capital",
                       lifeExp="Life Expectancy"))
```

![](https://i.loli.net/2020/05/19/iVDbMdrFSh1LZcv.png)



#### 地理图

##### choropleth-面分布图

```python
fig = px.choropleth(gapminder,locations="iso_alpha",color="lifeExp",
             hover_name="country",animation_frame="year",
             range_color=[20,80],projection="natural earth")
fig.show()
```

![image.png](https://i.loli.net/2020/05/19/EBRWqGtUYDKrd6w.png)

##### scatter_geo地图_点分布

```python
fig = px.scatter_geo(gapminder,locations="iso_alpha",
                     color="continent",hover_name="country",
                     size="pop",animation_frame="year",
                     projection="natural earth")

fig.show()
```

![image.png](https://i.loli.net/2020/05/19/YsCEo5xWc2Ig7OJ.png)

**去掉projection参数，看不同的效果**

```python
px.scatter_geo(gapminder,locations="iso_alpha",color="continent",hover_name="country",size="pop",animation_frame="year"
#,projection="natural earth"   # 去掉projection参数
)
```

![](https://i.loli.net/2020/05/19/KxBNuDUngl79Qvt.png)

##### line_geo-线型地理图

![image.png](https://i.loli.net/2020/05/19/khGU4VJFlcwNfOz.png)

### 使用内置数据集iris

查看文档并导入数据

```python
# 查看数据文档
print(px.data.iris.__doc__)

# 导入数据集
iris = px.data.iris()
iris

# 查看属性
iris["species"].value_counts()
```

![image.png](https://i.loli.net/2020/05/19/U3zcompB7vqdXxI.png)

#### 绘制散点图

```python
# 如何知道每个点的种类：指定颜色参数color="species"
px.scatter(iris,x="sepal_width",y="sepal_length",color="species")
```

![image.png](https://i.loli.net/2020/05/19/WYMn9RJIZUtFh3D.png)

#### 联合分布图（散点图+直方图）

上方增加直方图，右方增加细条图

```python
px.scatter(iris,x="sepal_width",y="sepal_length",color="species",
           marginal_x="histogram",marginal_y="rug"  # 主要是这两个参数决定
          )
```

![image.png](https://i.loli.net/2020/05/19/1UyAbGNjZBPJO2k.png)

#### violin-小提琴图

```python
px.scatter(iris,x="sepal_width",y="sepal_length",color="species"
           ,marginal_y="violin",marginal_x="box"    # 定义两个属性
           ,trendline="ols"  # 显示数据的变化趋势
          )
```

![image.png](https://i.loli.net/2020/05/19/7OPSWsDEarudB8o.png)

#### SPLOM-散点矩阵图

**这个图真的是非常棒，一条语句可以直接生成矩阵图**

![image.png](https://i.loli.net/2020/05/19/P4k93qJ2oESguX7.png)

#### 平行坐标图

```python
px.parallel_coordinates(iris,color="species_id",labels={"species_id":"Species",
                                                       "sepal_width":"Sepal Width",
                                                       "sepal_length":"Sepal Length",
                                                       "petal_length":"Petal Length",
                                                       "petal_width":"Petal Width"},
                       color_continuous_scale=px.colors.diverging.Tealrose,
                       color_continuous_midpoint=2)
```

![](https://i.loli.net/2020/05/19/O3Gau4NkDmwpU9d.png)

#### 箱体误差图

在上下分别加上误差值

```python
# 对当前值加上下两个误差值
iris["e"] = iris["sepal_width"] / 100
px.scatter(iris,x="sepal_width",y="sepal_length",color="species",error_x="e",error_y="e")
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gexzoko4n9j31760suwhn.jpg)

#### 等密度线图density_contour

```python
px.density_contour(iris,x="sepal_width",y="sepal_length",color="species")
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gexzpkv3prj315i0qcjy4.jpg)

等密度线图加上其他的图

```python
px.density_contour(iris,x="sepal_width",y="sepal_length",color="species",
                  marginal_x="rug",marginal_y="histogram"   # 在密度图的基础上，指定另外两种图形
                  )
```

![image-20200519193734781](https://tva1.sinaimg.cn/large/007S8ZIlgy1gexzqrdj6lj315e0rgdkx.jpg)

#### 密度热力图density_heatmap

```python
px.density_heatmap(iris,x="sepal_width",y="sepal_length",
                  marginal_y="rug",marginal_x="histogram"   # 在密度图的基础上，指定另外两种图形
                  )
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gexzuioctaj315s0qqjt0.jpg)

### 小费tips实例

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gexzv7ywl6j30rq0e8abk.jpg)

#### 散点图

根据性别的不同进行分类作图

```python
# 根据性别属性进行分类作图
fig = px.scatter(tips,x="total_bill",y="tip",color="size",render_mode="webgl",
                facet_col="sex",color_continuous_scale=px.colors.sequential.Viridis)
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gexzw052t6j313a0s2q6r.jpg)



#### 并行类别图

```python
fig = px.parallel_categories(tips,color="size",color_continuous_scale=px.colors.sequential.Inferno)
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gexzx0ntzjj315o0qawyw.jpg)

#### Bar-柱状图

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gexzxltyjqj319b0u0jto.jpg)

多重属性的柱状图

```python
fig = px.bar(tips,x="sex",y="total_bill",color="smoker",barmode="group"
      ,facet_row="time",facet_col="day",category_orders={"day": ["Thur","Fri","Sat","Sun"],"time":["Lunch", "Dinner"]})
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gexzzuz0vcj315k0u0q7k.jpg)

#### Histogram-直方图

```python
px.histogram(tips,x="sex",y="tip",histfunc="avg",color="smoker",barmode="group",
            facet_row="time",facet_col="day",category_orders={"day":["Thur","Fri","Sat","Sun"],
                                                             "time":["Lunch","Dinner"]})
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey012c5pfj315i0rwtag.jpg)

#### box-箱型图

```python
# notched=True显示连接处的锥形部分
px.box(tips,x="day",y="total_bill",color="smoker",notched=True)
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey02i8fxmj31850u00vx.jpg)

如果不加`notched`参数，则连接处则会是矩形，而不是锥形

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey0n78zdpj31500qq75j.jpg)

####  violin-小提琴图

```python
px.violin(tips,x="smoker",y="tip",color="sex",box=True   # box是显示内部的箱体
          ,points="all",hover_data=tips.columns  # 结果中显示全部数据
         )
```

![image-20200519201017897](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey0otef6jj316p0u0n2h.jpg)

### 3D图形绘制

使用的是election数据集

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey0pkxcs7j315s0emgny.jpg)

#### 散点3D图

```python
fig = px.scatter_3d(election,x="Joly",y="Coderre",z="Bergeron"  # 指定3个轴
                    ,color="winner",size="total",hover_name="district_id"  # 指定颜色种类、大小和显示名称
                    ,symbol="result"  # 右边的圆形和菱形
                    ,color_discrete_map={"Joly":"blue","Bergeron":"green","Coderre":"red"}   # 改变默认颜色
                   )
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey0tl2sxaj31l40u045z.jpg)

#### line-3D图

```python
px.line_3d(election,x="Joly",y="Coderre",z="Bergeron",color="winner",line_dash="winner")
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey0xjgge3j31a80ryq7a.jpg))

### 极坐标图-内置数据wind

使用的数据是wind

![image-20200519201924518](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey0ygkooqj30jq0dqwfb.jpg)

#### scatter-polar散点极坐标

```python
fig = px.scatter_polar(wind,r="frequency",theta="direction",color="strength",symbol="strength"
                      ,color_discrete_sequence=px.colors.sequential.Plasma_r)
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey0zeh0b5j31100qitda.jpg)

#### line_polar线性极坐标

```python
fig = px.line_polar(wind,r="frequency",theta="direction",color="strength",line_close=True
                      ,color_discrete_sequence=px.colors.sequential.Plasma_r)
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey10aopi8j311q0qkgrz.jpg)

#### bar_polar柱状极坐标

```python
fig = px.bar_polar(wind,r="frequency",theta="direction",color="strength",template="plotly_dark"
                  ,color_discrete_sequence=px.colors.sequential.Plasma_r)
fig.show()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey11rmj8jj30jg0cimy4.jpg)

### 颜色面板和序列

在px库中，px.colors模块中有很多可用的色标和序列：定性的、序列型的、离散的、循环等。

```python
px.colors.qualitative.swatches()
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey12sz8r3j312g0sk0ub.jpg)

```python
px.colors.sequential.swatches()  # 部分截图
```

![image-20200519202422882](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey13hrk3aj310o0go754.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey13wa6txj310k0h2t9g.jpg)

### 主题

主题允许用户控制图形范围的设置，包含**边距、字体、背景颜色、刻度定位**等。px内置的主题有3种：

- plotly
- plotly_white
- plotly_dark

```python
px.scatter(iris,x="sepal_width",y="sepal_length",color="species",marginal_x="box",
          marginal_y="histogram",height=600,trendline="ols",template="plotly")
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey15it2b1j30jg0ciwfg.jpg)

```python
px.scatter(iris,x="sepal_width",y="sepal_length",color="species",marginal_x="box",
          marginal_y="histogram",height=600,trendline="ols",template="plotly_white")
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey1b9rhelj30jg0ciab0.jpg)

```python
px.scatter(iris,x="sepal_width",y="sepal_length",color="species",marginal_x="box",
          marginal_y="histogram",height=600,trendline="ols",template="plotly_dark")
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gey1anf1nvj30jg0go3zs.jpg)

### 参数详解

以散点图为例，对绘制的参数进行解释

#### 定义

```python
def scatter(data_frame, x=None, y=None, color=None, symbol=None, size=None,
            hover_name=None, hover_data=None, text=None, facet_row=None,
            facet_col=None, error_x=None, error_x_minus=None, error_y=None,
            error_y_minus=None, animation_frame=None, animation_group=None,
            category_orders={}, labels={}, color_discrete_sequence=None,
            color_discrete_map={}, color_continuous_scale=None,
            range_color=None, color_continuous_midpoint=None,
            symbol_sequence=None, symbol_map={}, opacity=None,
            size_max=None, marginal_x=None, marginal_y=None, trendline=None,
            trendline_color_override=None, log_x=False, log_y=False,
            range_x=None, range_y=None, render_mode='auto', title=None,
            template=None, width=None, height=None):

    return
```

#### 参数解释

- data_frame：目标数据，类型为dataframe；
- x ：指定列名。列中的值用于笛卡尔坐标中沿 X 轴的定位标记。图表类型为水平柱状图时，这些值用作参数histfunc的入参；
- y ：指定列名。列中的值用于笛卡尔坐标中沿 Y 轴的定位标记。图表类型为垂直柱状图时，这些值用作参数histfunc的入参；
- color：指定列名。为列中的不同值，(由px)自动匹配不同的标记颜色；**若列为数值数据时，还会自动生成连续色标**；
- symbol：指定列名。为列中的不同值，设置不同的标记形状；
- size：指定列名。为列中的不同值，设置不同的标记大小；
- $\color{red}{hover\_name}$：指定列名。将列中的值，加粗显示在悬停提示内容的正上方；
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

### 参考资料

px真的是见过最好的可视化神器，特别是结合dash的在线功能，学习💪

[可视化神器plotly_express详解](https://www.jianshu.com/p/41735ecd3f75)

[API详解](https://plotly.com/python-api-reference/plotly.express.html)

[Plotly_express in python](https://plotly.com/python/plotly-express/)



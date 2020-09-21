---
title: pyecharts-4-绘制桑葚图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-09-05 23:57:54
password:
summary:
tags:
  - 可视化
  - pyecharts
categories:
  - datavisual
  - pyecharts
---


### pyecharts-4-绘制桑葚图

本文中介绍的是如何利用`Pyecharts`绘制桑葚图，包含：

- 什么是桑葚图
- 官网demo，理解数据含义
- 模拟数据及生成对应的数据
- 实际效果展示



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gig24iyisij30q60g60v0.jpg)

<!--MORE-->

### 桑葚图

桑基图是可视化图表的一种，一般用来表示数据流量。

> 桑基图（Sankey diagram），即桑基能量分流图，也叫桑基能量平衡图。它是一种特定类型的流程图，图中延伸的分支的宽度对应数据**流量**的大小，比较适用于用户流量等数据的可视化分析。因1898年Matthew Henry Phineas Riall Sankey绘制的“蒸汽机的能源效率图”而闻名，此后便以其名字命名为“桑基图”。

桑基图主要由**边、流量和支点**组成，其中边代表了流动的数据，流量代表了流动数据的具体数值，节点代表了不同分类。边的宽度与流量成比例地显示，边越宽，数值越大。

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gig2ctxa11j31920segny.jpg)



### 官网demo

下面的数据是官网提供demo，通过观察可以找到桑葚图数据的特点：

- `nodes`代表的是所有节点的名称
- `links`代表的是每两个节点的数据流向和具体数值，`soure`可以看做是父类的节点，`target`代表的是子类的节点，`value`代表的是具体数据
- **上面的两个数据要组成json格式**

```python
from pyecharts import options as opts
from pyecharts.charts import Sankey

nodes = [
    {"name": "category1"},
    {"name": "category2"},
    {"name": "category3"},
    {"name": "category4"},
    {"name": "category5"},
    {"name": "category6"},
]

links = [
    {"source": "category1", "target": "category2", "value": 10},   # 一组数据
    {"source": "category2", "target": "category3", "value": 15},
    {"source": "category3", "target": "category4", "value": 20},
    {"source": "category5", "target": "category6", "value": 25},   # 单独的数据
]
c = (
    Sankey()
    .add(
        "sankey",
        nodes,
        links,
        linestyle_opt=opts.LineStyleOpts(opacity=0.2, curve=0.5, color="source"),
        label_opts=opts.LabelOpts(position="right"),
    )
    .set_global_opts(title_opts=opts.TitleOpts(title="Sankey-基本示例"))
    .render("sankey_base.html")
)
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gig2j7n6d4j319k0rw75y.jpg)

### 模拟数据

#### 原始数据

下面的数据是**随意模拟**的，主要是了解如何生成绘制桑葚图需要的数据

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gig2105z4fj30sk0vytch.jpg)

#### 转成父类+子类数据

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gig2vetxsjj319m0ncafj.jpg)

- 先从总费这个父类用到服装等4个子类
- 再从服装（服装为例）这个类从自己的3个子类

最终得到右侧的`父类+子类+数据`这样的完整数据

### json数据

#### Nodes数据

```python
# 节点去重
nodes = list(set(list(set(data['父类'].tolist())) + list(set(data['子类'].tolist()))))

# 节点列表数据
nodes_list = []
for i in nodes:
    dic = {}
    dic["name"] = i
    nodes_list.append(dic)
```

#### ![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gig83mzv7mj30gm0jwtb2.jpg)



#### links数据

```python
links_list = []
for i in range(len(data)):
    dic = {}
    dic["source"] = data.iloc[i,0]
    dic["target"] = data.iloc[i,1]
    dic["value"] = data.iloc[i,2]
    links_list.append(dic)
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gig84ut79fj30vu0ji79o.jpg)

#### 最终完整数据

通过上面的数据处理得到了生成桑葚图的完整数据

```python
nodes_list = [
      {'name': '滴滴'},
      {'name': '长辈'},
      {'name': '公交'},
      {'name': '总费用'},
      {'name': '房租'},
      {'name': '衣服'},
      {'name': '饮料'},
      {'name': '地铁'},
      {'name': '服装'},
      {'name': '同学'},
      {'name': '袜子'},
      {'name': '聚餐'},
      {'name': '住宿'},
      {'name': '水电费'},
      {'name': '外卖'},
      {'name': '红包'},
      {'name': '餐饮'},
      {'name': '鞋子'},
      {'name': '交通'}
]

links_list = [
    {'source': '总费用', 'target': '服装', 'value': 560},
    {'source': '总费用', 'target': '餐饮', 'value': 1180},
    {'source': '总费用', 'target': '住宿', 'value': 1500},
    {'source': '总费用', 'target': '交通', 'value': 400},
    {'source': '总费用', 'target': '红包', 'value': 1500},
    {'source': '服装', 'target': '衣服', 'value': 150},
    {'source': '服装', 'target': '鞋子', 'value': 350},
    {'source': '服装', 'target': '袜子', 'value': 60},
    {'source': '餐饮', 'target': '外卖', 'value': 600},
    {'source': '餐饮', 'target': '饮料', 'value': 280},
    {'source': '餐饮', 'target': '聚餐', 'value': 300},
    {'source': '住宿', 'target': '房租', 'value': 1200},
    {'source': '住宿', 'target': '水电费', 'value': 300},
    {'source': '交通', 'target': '地铁', 'value': 200},
    {'source': '交通', 'target': '滴滴', 'value': 150},
    {'source': '交通', 'target': '公交', 'value': 50},
    {'source': '红包', 'target': '长辈', 'value': 1000},
    {'source': '红包', 'target': '同学', 'value': 500}
   ]
```

### 绘图

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gig81naiw7j30u019larq.jpg)

### 结果

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gig86bn7wpj31di0sg783.jpg)

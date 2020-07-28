---
title: plotly-express-18-plotly输出静态图
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-07-25 00:23:00
password:
summary:
tags:
  - 可视化
  - pandas
  - px
categories:
  - datavisual
  - px
---

### Plotly-express-18-plotly输出静态图



本文介绍的是如何在`Plotly`中输出静态图，尝试使用了两种方式：

- Kaleido
- Orca

输出的时候可以指定不同的格式：`png\jpeg\pdf`等

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh2c6nv0y7j30ql0qltbf.jpg)

<!--MORE-->

### Orca

Orca is a pipeline orchestration tool that allows you to define dynamic data sources and explicitly connect them to processing functions. Orca has many features for working with [Pandas](http://pandas.pydata.org/) data structures, but it can be used with anything.

Orca has explit goals of flexibility, transparency, lazy execution, and encouraging good practices. Those goals are achieved by:

- Flexibility
  - Users may write and run any Python

- Transparency
  - Dependencies between data and processing units are explicitly listed
  - Your code is a record of everything that happens
- Lazy execution
  - Orca only calls functions if they are explicitly needed
- Good practices
  - Encourage small, functional units
  - Encourage code re-use



#### 代码

```python
import plotly.graph_objects as go
import numpy as np
np.random.seed(1)

N = 100
x = np.random.rand(N)
y = np.random.rand(N)
colors = np.random.rand(N)
sz = np.random.rand(N) * 30

fig = go.Figure()
fig.add_trace(go.Scatter(
    x=x,
    y=y,
    mode="markers",
    marker=go.scatter.Marker(
        size=sz,
        color=colors,
        opacity=0.6,
        colorscale="Viridis")
))

fig.show()
```

#### 图片

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh2bolu1l5j30jg0ciq42.jpg)

#### 报错

但是当在使用Orca进行保存的时候出现了报错：

````python
import os

if not os.path.exists("images"):
    os.mkdir("images")   # 不存在则创建目录

fig.write_image("images/fig1.png")   # 保存在当前的images目录下
````

出现的报错：原因在于orca没有添加到环境变量中

```python
ValueError                                Traceback (most recent call last)
<ipython-input-3-81433d09fdb2> in <module>
----> 1 fig.write_image("images/fig1.png")

/Applications/downloads/anaconda/anaconda3/lib/python3.7/site-packages/plotly/basedatatypes.py in write_image(self, *args, **kwargs)
   2822         import plotly.io as pio
   2823
-> 2824         return pio.write_image(self, *args, **kwargs)
   2825
   2826     # Static helpers

/Applications/downloads/anaconda/anaconda3/lib/python3.7/site-packages/plotly/io/_orca.py in write_image(fig, file, format, scale, width, height, validate)
   1768     # Do this first so we don't create a file if image conversion fails
   1769     img_data = to_image(
-> 1770         fig, format=format, scale=scale, width=width, height=height, validate=validate
   1771     )
   1772

/Applications/downloads/anaconda/anaconda3/lib/python3.7/site-packages/plotly/io/_orca.py in to_image(fig, format, width, height, scale, validate)
   1533     # Make sure orca sever is running
   1534     # -------------------------------
-> 1535     ensure_server()
   1536
   1537     # Handle defaults

/Applications/downloads/anaconda/anaconda3/lib/python3.7/site-packages/plotly/io/_orca.py in ensure_server()
   1388         # Validate orca executable only if server_url is not provided
   1389         if status.state == "unvalidated":
-> 1390             validate_executable()
   1391         # Acquire lock to make sure that we keep the properties of orca_state
   1392         # consistent across threads

/Applications/downloads/anaconda/anaconda3/lib/python3.7/site-packages/plotly/io/_orca.py in validate_executable()
   1182 for more info on Xvfb
   1183 """
-> 1184         raise ValueError(err_msg)
   1185
   1186     if not help_result:

ValueError:
The orca executable is required in order to export figures as static images,
but the executable that was found at '/Users/piqianchao/.nvm/versions/node/v13.0.1/bin/orca'
does not seem to be a valid plotly orca executable. Please refer to the end of
this message for details on what went wrong.

If you haven't installed orca yet, you can do so using conda as follows:

    $ conda install -c plotly plotly-orca

Alternatively, see other installation methods in the orca project README at
https://github.com/plotly/orca

After installation is complete, no further configuration should be needed.

..........

    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1011:10)
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh24zlz86nj31hw0gwwi8.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh2512uqv9j31e60lqtdm.jpg)



![image-20200724163937248](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh25hztnf8j312i0c60w7.jpg)

#### 解决

1. 先检查Orca是否[安装](https://udst.github.io/orca/)

With the dependencies installed, install Orca with pip:

```shell
pip install orca
```

Orca may also be installed with conda:

```shell
conda install -c udst orca
```

Add the server option to include the optional server dependencies:

```shell
pip install orca[server]
```

2. 如果安装之后，将Orca添加到电脑的环境变量中即可，具体参考[Mac/Linux环境变量设置](https://zhuanlan.zhihu.com/p/25976099)
3. 其他依赖
   - Make sure [nodejs](https://nodejs.org/) is installed. (I use [Homebrew](http://brew.sh/) on my Mac.)
   - Install [gulp](http://gulpjs.com/): `npm install -g gulp`
   - Change directories to `orca/server/static`
   - Run `npm install` to install dependencies
   - Build the bundle: `gulp js-build`, or
   - Watch JS files to rebuild the bundle on changes: `gulp js-watch`



**Orca的使用和安装挺麻烦的，那么Kaleido来了**😃

### Kaleido

#### Intro

Kaleido is a cross-platform library for generating static images (e.g. png, svg, pdf, etc.) for web-based visualization libraries, with a particular focus on eliminating external dependencies.

The project's initial focus is on the export of plotly.js images from Python for use by plotly.py, but it is designed to be relatively straight-forward to extend to other web-based visualization libraries, and other programming languages.

The primary focus of Kaleido (at least initially) is to serve as a dependency of web-based visualization libraries like plotly.py. As such, the focus is on providing a programmatic-friendly, rather than user-friendly, API.

https://www.ctolib.com/plotly-Kaleido.html

#### install

安装直接`pip install kaleido`

Install the kaleido wheel.

```python
$ pip install kaleido
```

Install plotly as well

```python
$ pip install plotly
```

#### demo

```python
from kaleido.scopes.plotly import PlotlyScope
import plotly.graph_objects as go
scope = PlotlyScope()

fig = go.Figure(data=[go.Scatter(y=[1, 3, 2])])
with open("figure.png", "wb") as f:   # 在本地目录下面就会生成文件
    f.write(scope.transform(fig, format="png"))
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh2is2l7hsj30jg0dw74q.jpg)

相比较于Orca，Kaleido还是非常简洁的

https://github.com/plotly/Kaleido

---
title: plotly_express-3-Dash_callback
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-06-01 18:49:02
password:
summary:
tags:
  - callback
  - Dash
  - 回调函数
categories:
  - datavisual
  - px
---


### dash callbacks

The `dash_html_components` library provides classes for all of the HTML tags, and the keyword arguments describe the HTML attributes like `style`, `className`, and `id`.

The `dash_core_components` library generates higher-level components like controls and graphs.

- dash_html_components提供的是HTML标签的各种类以及用于描述HTML属性，比如style、className、id 等的各种关键参数
- dash_core_components提供的是能够用于控制和作图的高级组件

<!--MORE-->

### 第一个demo

#### 代码示例

在使用回调函数的时候需要引入一个模块：

```python
from dash.dependencies import Input, Output
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfczdrpq6vj30jp0hk0tj.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfczdmpjkgj30cl05qq2w.jpg)

#### 代码解释

1. The "inputs" and "outputs" of our application interface are described declaratively through the `app.callback` decorator.

应用的中input和output接口通过回调装饰器来实现

2. In Dash, the inputs and outputs of our application are simply the properties of a particular component. In this example, our input is the "`value`" property of the component that has the ID "`my-id`". Our output is the "`children`" property of the component with the ID "`my-div`".

inputs和outputs两个部分只是特殊组件的属性，它们同时拥有value和id两个属性值

3. Whenever an input property changes, the function that the callback decorator wraps will get called automatically. Dash provides the function with the new value of the input property as an input argument and Dash updates the property of the output component with whatever was returned by the function.

只要输入input的属性改变，回调函数构成的装饰器会自动改变输出值

4. The `component_id` and `component_property` keywords are optional (there are only two arguments for each of those objects). I have included them here for clarity but I will omit them from here on out for brevity and readability.

`component_id` and `component_property`这两个参数可以省略，直接写出它们相应的值即可

5. Notice how we don't set a value for the `children` property of the `my-div` component in the `layout`. When the Dash app starts, it automatically calls all of the callbacks with the initial values of the input components in order to populate the initial state of the output components. In this example, if you specified something like `html.Div(id='my-div', children='Hello world')`, it would get overwritten when the app starts.

上述例子中没有对children属性赋值。Dash应用程序启动时，它将自动使用输入组件的初始值调用所有回调，以填充输出组件的初始状态。如果将其设为其他值，原始值将会被覆盖。

#### 第二个demo

#### 代码

**另一个例子：通过dcc.Silder来更新dcc.Graph**，将滑动条和图形相结合的例子

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfczde5m1aj30u01hrk49.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfczd793haj30i50akaai.jpg)



#### 代码解释

1. the `"value"` property of the `Slider` is the input of the app and the output of the app is the `"figure"` property of the `Graph`.

指明了应用APP的输入slider和输出figure

2. Whenever the `value` of the `Slider` changes, Dash calls the callback function `update_figure` with the new value. The function filters the dataframe with this new value, constructs a `figure` object, and returns it to the Dash application.

当滑动条改变，即输入改变的时候，dash的回调函数也会同时更新，然后返回给dash应用

3. We load our dataframe at the start of the app: `df = pd.read_csv('...')`. This dataframe `df` is in the global state of the app and can be read inside the callback functions.

在应用APP开始的时候就导入了数据df；保证了数据df是全局性质，在任何地方都可以读取使用。

4. The callback does not modify the original data, it just creates copies of the dataframe by filtering through pandas filters. This is important: *your callbacks should never mutate variables outside of their scope*.

不要在回调函数内部改变原始数据，它仅仅是使用pandas来进行过滤数据，从而来使用其副本。

5. We are turning on transitions with `layout.transition` to give an idea of how the dataset evolves with time: transitions allow the chart to update from one state to the next smoothly, as if it were animated.

开启了trasnsition属性，能够看到数据集随时间的变化情况。



#### 可视化效果

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfczd2lrd9j30ji0cvgn6.jpg)

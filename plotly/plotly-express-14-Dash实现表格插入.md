---
title: plotly-express-14-Dash实现表格插入
top: false
cover: false
toc: true
mathjax: true
copyright: true
date: 2020-07-06 17:16:00
password:
summary:
tags:
  - 可视化
  - px
categories:
  - datavisual
  - px
---



### plotly-express-14-Dash实现表格

本文中介绍的是在Dash中如何实现表格，往表格中添加数据，使用的是`app.layout = dash_table.DataTable()`

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggjt5sinlyj31400u0u0y.jpg)


<!--MORE-->

### 参数

参数详解：https://dash.plotly.com/datatable/style

```python
DataTable(active_cell=undefined, columns=undefined, include_headers_on_copy_paste=undefined,
          locale_format=undefined, css=undefined, data=undefined, data_previous=undefined,
          data_timestamp=undefined, editable=undefined, end_cell=undefined,id=undefined,
          export_columns=undefined, export_format=undefined, export_headers=undefined,
          fill_width=undefined, hidden_columns=undefined, is_focused=undefined,
          merge_duplicate_headers=undefined, fixed_columns=undefined, fixed_rows=undefined,
          column_selectable=undefined, row_deletable=undefined, row_selectable=undefined,
          selected_cells=undefined, selected_rows=undefined, selected_columns=undefined,
          selected_row_ids=undefined, start_cell=undefined, style_as_list_view=undefined,
          page_action=undefined, page_current=undefined, page_count=undefined,
          page_size=undefined, dropdown=undefined, dropdown_conditional=undefined,
          dropdown_data=undefined, tooltip=undefined, tooltip_conditional=undefined,
          tooltip_data=undefined, tooltip_delay=undefined, tooltip_duration=undefined,
          filter_query=undefined, filter_action=undefined, sort_action=undefined,
          sort_mode=undefined, sort_by=undefined, sort_as_null=undefined,
          style_table=undefined, style_cell=undefined, style_data=undefined,
          style_filter=undefined, style_header=undefined, style_cell_conditional=undefined,
          style_data_conditional=undefined, style_filter_conditional=undefined,
          style_header_conditional=undefined, virtualization=undefined,
          derived_filter_query_structure=undefined, derived_viewport_data=undefined,
          derived_viewport_indices=undefined, derived_viewport_row_ids=undefined,
          derived_viewport_selected_columns=undefined, derived_viewport_selected_rows=undefined,
          derived_viewport_selected_row_ids=undefined, derived_virtual_data=undefined,
          derived_virtual_indices=undefined, derived_virtual_row_ids=undefined,
          derived_virtual_selected_rows=undefined, derived_virtual_selected_row_ids=undefined,
          loading_state=undefined, persistence=undefined, persisted_props=undefined, persistence_type=undefined, **kwargs)
```



### demo

#### 数据

```python
import dash
import dash_table
import pandas as pd
import numpy as np

df = pd.DataFrame({"number":np.arange(200000),
                   "chinese":np.random.randint(80,100,200000),
                   "math":np.random.randint(80,100,200000),
                   "english":np.random.randint(80,100,200000),
                   "gem":np.random.randint(80,100,200000)})
df
```

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gghc1wkkt9j30pu0kmjt3.jpg)

#### 转成字典形式

```python
data = df.to_dict("records")   # 转成字典形式
data[:20]
```



![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gghc2eufdfj31660m245k.jpg)

### 生成表格

#### Demo

```python
app = dash.Dash(__name__)

# 在layout中生成数据
app.layout = dash_table.DataTable(
    id='table',
    columns=[{"name": i, "id": i} for i in df.columns],   # 表的属性
    data=df.to_dict('records'),   # 将数据转成字典形式
    fixed_rows={'headers': True},  # 滚动的时候每个属性仍然可见
    page_size=50,   # 每页显示的数据量。当有滚动条后，需要滚动下拉，默认是250
    style_table={'height': '400px', 'overflowY': 'auto'},  # 时间滚动条和滚动页面的高度设置 defaults to 500
    style_header={
        'overflow': 'hidden',
        'textOverflow': 'ellipsis',
        'maxWidth': 50,
    },
    style_cell={
        'minWidth': 30, 'maxWidth': 60, 'width': 45,
        'textAlign': 'center'  # 文本居中显示
    }
)

if __name__ == '__main__':
    app.run_server()
```

#### 效果

右侧能够实现滚动；滚动的时候属性仍然是可见的。上面的代码中有各种参数的详细解释

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gghd6ta5bjj327o0o4go2.jpg)

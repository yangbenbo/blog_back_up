---
title: Pandas
date: 2024-06-30 07:32:58
categories:
- Program
tags:
- python
---


# 简介
Pandas[^1]是一个开源的数据分析和操作库, 它是Python中用于数据科学和数据分析的主要工具之一. Pandas提供了快速, 灵活和表达力强的数据结构, 旨在使数据清洗和分析工作变得更加简单易行

简要信息: 行索引(index), 列索引(column)

官方教程已经非常详细, 这里展示多级索引示例以及预处理文件保存
```
import pandas as pd
import numpy as np

# 定义多级列索引的名称
second_level_columns = {
    'cur_jnt': ['R1', 'R2'],
    'cmd_jnt': ['R3', 'R4'],
    't': []  # 'Timestamp' 只有一级索引
}

# 创建多级列索引
multi_columns = pd.MultiIndex.from_tuples(
    [('t',)] +
    [('cur_jnt', r1) for r1 in second_level_columns['cur_jnt']] +
    [('cmd_jnt', r2) for r2 in second_level_columns['cmd_jnt']]
)

# 创建一个空的DataFrame，行索引为整数范围
integer_row_index = range(5)
df = pd.DataFrame(index=integer_row_index, columns=multi_columns)

# 将新数据行添加到 DataFrame
df.loc[:, :] = np.random.random((5, 5))
df.loc[:, ("cur_jnt", "R1")] = np.random.random(5)
df.loc[:, "t"] = np.arange(5)

# 添加一行数据
cur_jnt = [1, 2, 3, 4, 5]
df.loc[len(df)] = cur_jnt

# 打印填充数据后的DataFrame查看
print("\nFilled DataFrame:")
print(df)
df["cur_jnt"].plot()
```

保存为`json`文件
```
df.to_json('df.json', orient='split')  # 使用 'split' 方式保存


# 从 JSON 文件读取数据
df_loaded = pd.read_json('df.json', orient='split')

# 恢复多级列索引
# 由于 'split' 方式保存，读取时列索引已经是多级索引形式
# 但是，你需要确保列的顺序与原始 DataFrame 相同
df_loaded.columns = pd.MultiIndex.from_tuples(df_loaded.columns.tolist())
```


[^1]: https://pandas.pydata.org/docs/user_guide/10min.html#
---
title: FastAPI-11：函数参数类型是list参数
date: 2023-11-15 18:50:53
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-11：函数参数类型是list参数

## 函数参数类型是列表，但不使用typing中的list，而使用list，会怎样？

### 使用typing中的List、Set、Tuple的栗子

```python
from typing import Optional, List, Tuple, Set
import uvicorn
from fastapi import FastAPI, Body
app = FastAPI()
@app.put('/items/{item_id}')
async def update_item(
    list_: List[int] = Body(...),
    tuple_: Tuple[int] = Body(...),
    set_: Set[int] = Body(...),
):
    results = {'list_': list_, 'tuple_': tuple_, 'set_': set_}
    return results
if __name__ == '__main__':
    uvicorn.run(app='ninth-9:app', host='0.0.0.0', port=8080, debug=True)
```

#### 假设里面的元素传了非int且无法自动转换成int：

- typing的List、Set、Tuple都会指定里面参数的数据类型；
- 而FastAPI会对声明了数据类型的数据进行数据校验，所以会针对序列里面的参数进行数据校验；
- 如果校验失败，会报一个友好的提示。

{% asset_img "FastAPI-11：函数参数类型是list参数-1.png" %}

### 使用list、set、tuple的栗子

用Python自带的list、set、tuple类，是无法指定序列里面参数的数据类型，所以FastAPI并不会针对里面的参数进行数据校验

```python
@app.put('/item/{item_id}')
async def update_item(
    list_: list = Body(...),
    tuple_: tuple = Body(...),
    set_: set = Body(...),
):
    results = {'list_': list_, 'tuple_': tuple_, 'set_': set_}
    return results
# 这样就变成了传啥类型的值都可以了
```

#### 示例：

{% asset_img "FastAPI-11：函数参数类型是list参数-2.png" %}

## 总结

要充分利用FastAPI的优势，强烈建议使用typing的List、Set、Tuple来表示列表、集合、元祖类型。
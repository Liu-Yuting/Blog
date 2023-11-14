---
title: FastAPI-10：详解Body
date: 2023-11-14 18:58:13
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-10：详解Body

## 前言

- 上一篇有讲到将参数类型指定为`Pydantic Model`，这样FastAPI会解析它为一个Request Body；
- 那单类型（int、float、str、bool...）参数可以成为Request Body的一部分吗？答案显然是肯定的；
- 通过Body函数即可完成，和Path、Query有异曲同工之妙。

## Body

- 主要作用：可以将但类型的参数成为Request Body的一部分，即从查询参数变成请求体参数；
- 和Query、Path提供的额外校验、元数据是基本一致的（多了个embed参数，最后详解）。

{% asset_img "FastAPI-10：详解Body-1.png" %}

### Body的简单栗子

```python
from typing import Optional
import uvicorn
from fastapi import Body, FastAPI
from pydantic import BaseModel
app = FastAPI()
class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None
class User(BaseModel):
    username: str
    full_name: Optional[str] = None
@app.put('/items/{item_id}')
async def update_item(
    item_id: int,
    item: Item,
    user: User,
    importance: int = Body(...)
):
    results = {'item_id': item_id, 'item': item, 'user': user, 'importance': importance}
    return results
if __name__ == '__main__':
    uvicorn.run(app='eighth-8:app', host='0.0.0.0', port=8080, debug=True)
```

#### 正确传参的请求结果：

{% asset_img "FastAPI-10：详解Body-2.png" %}

**传递的参数中多了importance参数**

#### 查看`Swagger API` 文档：

{% asset_img "FastAPI-10：详解Body-3.png" %}

### Query、Path、Body终极混用

```python
from fastapi import Path, Query
@app.put("/item_all/{item_id}")
async def update_item(
        *,
        item_id: int = Path(default=..., description="路径参数", gt=0, lt=10),
        address: str = Query(default=None, description="查询参数", max_length=10),
        item: Item,
        user: User,
        importance: int = Body(default=..., description="请求体", ge=1, le=5)
):
    results = {
        "item_id": item_id,
        "address": address,
        "item": item,
        "user": user,
        "importance": importance
    }
    return results
```

#### 正确传参的结果：

{% asset_img "FastAPI-10：详解Body-4.png" %}

#### 查看`Swagger API`文档：

{% asset_img "FastAPI-10：详解Body-5.png" %}

### Body设置的元数据会在JSON Schema中体现

{% asset_img "FastAPI-10：详解Body-6.png" %}

## Body()中的`embed`参数

### 为什么要讲这个`embed`参数

当函数只有一个参数指定了Pydantic Model且没有其他Body传参时，传参的时候请求体可以不指定参数名

```python
@app.put('/item/{item_id}')
async def update_item(
    item_id: int,
    item: Item,
):
    return {'item_id': item_id, 'item': Item}
# 默认并不需要指定item为字段名
# 如果想要指定item为请求体的字段名，就是通过embed参数达到目的了
@app.put("/items/{item_id}")
async def update_item(
        *,
        item_id: int,
        # 将 embed 设置为 True
        item: Item = Body(..., embed=True)
):
    results = {"item_id": item_id, "item": item}
    return results
```

#### 正确传参的请求结果：

{% asset_img "FastAPI-10：详解Body-7.png" %}

#### 不传item字段的请求结果：

{% asset_img "FastAPI-10：详解Body-8.png" %}

#### 查看 `Swagger API` 文档：

{% asset_img "FastAPI-10：详解Body-9.png" %}
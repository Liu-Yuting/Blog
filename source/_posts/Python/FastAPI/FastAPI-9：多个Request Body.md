---
title: FastAPI-9：多个Request Body
date: 2023-11-13 19:25:14
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-9：多个Request Body

## Path、Query、Request Body混合使用

```python
from fastapi import FastAPI, Path, Query
from typing import Optional
from pydantic import BaseModel
import uvicorn 
app = FastAPI()
class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None
@app.put('/items/{item_id}')
async def update_item(
    *,
    item_id: int = Path(default=..., description='item_id', gt=1, lt=20, example=2),
    name: Optional[str] = Query(default=None, description='查询参数', min_length=0, max_length=20, example='示例值'),
    item: Optional[Item] = None,
):
    results = {'item_id': item_id}
    if name:
        results.update({'name': name})
    if item:
        results.update({'item': item})
    return results
if __name__ == '__main__':
    uvicorn.run(app='', host='0.0.0.0', port=8080, reload=True)
# 除了路径参数item_id是必传的，查询参数name和请求体item都是可选非必传
```

#### 只传路径参数的请求结果：

{% asset_img "FastAPI-9：多个Request Body-1.png" %}

#### 路径参数、查询参数、请求体均传递的请求结果：

{% asset_img "FastAPI-9：多个Request Body-2.png" %}

#### 查看 `Swagger API` 文档：

{% asset_img "FastAPI-9：多个Request Body-3.png" %}

## 多个 Request Body

```python
# 自定义第2个模型类
class User(BaseModel):
    username: str
    full_name: Optional[str] = None
@app.put('/item/{item_id}')
async def update_item(
    item_id: int,
    item: Item,  # 指定第一个 Model 类型
    user: User,  # 指定第二个 Model 类型
):
    results = {
        'item_id': item_id,
        'item': item,
        'user': user
    }
    return results
```

- 这种情况下，FastAPI会注意到函数中有两个 `Request Body`，因为这 `item、name` 两个参数都指定了 `Pydantic` 模型；
- FastAPI将使用参数名作为 `Request Body` 中的键（字段名称）。

#### 正确传参的结果：

{% asset_img "FastAPI-9：多个Request Body-4.png" %}

#### 查看 `Swagger API` 文档：

{% asset_img "FastAPI-9：多个Request Body-5.png" %}
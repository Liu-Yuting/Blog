---
title: FastAPI-22：Pydantic Model结合Union、List的使用场景
date: 2024-01-29 09:37:14
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-22：Pydantic Model结合Union、List的使用场景

## 前言

有多个模型且请求、响应需要声明多个模型的时候，可以根据不同使用场景结合typing库里的Union、List来达到目的。

## Union

### 作用

联合类型，[详细教程](https://www.cnblogs.com/poloyy/p/15170066.html)。使用Union时，建议首先包含具体的类型，然后是不太具体的类型。

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time     : 2024/1/18 14:56 
# @Author   : 22759
# @Email    : lyt_sy@sina.com
# @Project  : FastApi-demo
# @File     : demo18
# @Software : PyCharm
from typing import Optional, Union, List, Dict

from fastapi import APIRouter
from pydantic import BaseModel, EmailStr

router = APIRouter()


class ItemBase(BaseModel):
    description: str
    type: str


class ItemCar(ItemBase):
    """给个默认值"""
    type: str = "car"


class ItemPlane(ItemBase):
    type: str = "plane"
    size: int


ITEM = {
    "item1": {
        "description": "All my friends drive a low rider",
        "type": "car"
    },
    "item2": {
        "description": "Music is my aeroplane, it's my aeroplane",
        "type": "plane",
        "size": 5,
    }
}


@router.get("/item/{item_id}", response_model=Union[ItemPlane, ItemCar])
async def read_item(item_id: str) -> Dict:
    return ITEM[item_id]
```

### item_id = "item1"

请求结果

{% asset_img "1.png" %}

### item_id = "item2"

请求结果

{% asset_img "2.png" %}

## List

```python
class Item(ItemBase):
    name: Optional[str]
    description: Optional[str]


NEW_ITEMS = [
    {'name': 'Foo', 'description': 'There comes my hero'},
    {'name': 'Red', 'description': "It's my aeroplane", 'size': 123},  # 多了个size字段
]


@router.post("/items/", response_model=List[Item])
async def read_item():
    return NEW_ITEMS
```

### 正确传参的响应结果

{% asset_img "3.png" %}

返回了一个数组，第二个值中的size不会返回。这是因为响应模型不包含size，所以最终返回的数据也不包含size。

### 内容不包含description

```python
NEW_ITEMS = [
    {"name": "Foo", "description": "There comes my hero"},
    {"name": "Red", "description": "It's my aeroplane", "size": 123},  # 多了个size字段
    {"name": "Red", "size": 123},  # 不包含description字段
]
```

### 请求结果

{% asset_img "4.png" %}

- 因为响应模型声明了 name、description 都是必传参数，假设不传就会报错
- 但又因为是响应数据有问题，代表应用程序（服务端）有问题，所以客户端发送请求就会报 500

---
title: FastAPI-26：Path Operation Configuration路径操作的配置
date: 2024-02-02 15:25:44
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-26：Path Operation Configuration路径操作的配置

## 前言

再一次声明，路径操作：

- @app.get()
- @app.post()
- @app.put()
- @app.delete()
- ...

而路径操作的配置，其实就是上述函数的参数。这些参数可以让SwaggerAPI文档中显示这些参数，友好显示相关信息。

## 全部配置

{% asset_img "1.png" %}

## 这一篇会讲的配置项

- tags
- summary
- description
- deprecated
- name

### 实际栗子

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time     : 2024/1/24 11:39 
# @Author   : 22759
# @Email    : lyt_sy@sina.com
# @Project  : FastApi-demo
# @File     : demo20
# @Software : PyCharm
from typing import Optional, Set

from pydantic import BaseModel
from fastapi import APIRouter, status

router = APIRouter()


class Item(BaseModel):
    name: str
    price: float
    tags: Set[str] = []
    tax: Optional[float] = None
    description: Optional[str] = None


@router.post("/few/", response_model=Item, tags=["items"], deprecated=True)
async def create_item(item: Item):
    return item


@router.get(
    "/many/",
    tags=["many"],
    status_code=status.HTTP_201_CREATED,
    summary="This is the API for the many",
    description="这是一个Many的API",
    response_description="响应描述"
)
async def read_items():
    return [{"name": "Foo", "price": 42}]
```

#### 查看Swagger API文档

{% asset_img "2.png" %}

{% asset_img "3.png" %}

### description第二种传参方式

```python
"""这种方法可以在字符串内写MarkDown，FastAPI可以识别到它"""
@router.get('/other_description/')
async def other_description():
    """This is the other description API"""
    return {"name": "Foo", "price": 42}
```

#### 查看Swagger API文档

{% asset_img "4.png" %}
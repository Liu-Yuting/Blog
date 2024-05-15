---
title: FastAPI-28：JSON Compatible Encoder利器之jsonable encoder
date: 2024-02-04 16:44:45
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-28：JSON Compatible Encoder利器之jsonable encoder

## jsonable_encoder

​	在实际应用场景中，可能需要将数据类型（如：Pydantic模型）转换为与JSON兼容的类型（如：字典、列表）；比如：需要将数据存储在数据库中；为此，FastAPI提供了一个jsonable_encoder()函数；jsonable_encoder实际上是FastAPI内部用来转换数据的，但它在许多其他场景中很有用。

## 实际栗子

**需求**

- 假设有一个接收兼容JSON数据的数据库fake_db；
- 例如，它不接收日期时间对象，因为这些对象与JSON不兼容；
- 因此，必须将日期时间对象转换为包含JSON格式数据的字符串；
- 同样，这个数据库不会接收Pydantic模型（具有属性的对象），只会接收dict；
- 使用jsonable_encoder将数据转换成dict。

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time     : 2024/2/6 16:04 
# @Author   : 22759
# @Email    : lyt_sy@sina.com
# @Project  : FastApi-demo
# @File     : demo22
# @Software : PyCharm
from typing import Optional
from datetime import datetime

from fastapi import APIRouter
from pydantic import BaseModel
from fastapi.encoders import jsonable_encoder

router = APIRouter()
FAKE_DB = dict()


class Item(BaseModel):
    age: Optional[int]
    title: Optional[str]
    timestamp: Optional[datetime]


@router.put('/jsonable/{fake_id}/')
async def jsonable(fake_id: str, item: Item):
    """jsonable_encoder工具使用"""
    """1、打印传入的数据和类型"""
    print(f'item is {item}')
    print(f'item type is {type(item)}')
    """2、调用jsonable_encoder方法将Pydantic Model转换为Dict"""
    json_item_data = jsonable_encoder(item)
    """3、模拟将数据落库操作"""
    FAKE_DB[fake_id] = json_item_data
    """4、打印转换后的数据和类型"""
    print(f'encoder_data is {json_item_data}')
    print(f'encoder_data type is {type(json_item_data)}')
    return json_item_data
```

- jsonable_encoder将Pydantic模型转换为dict，并将日期时间转换为str；
- 它将返回一个Python标准数据结构（比如：dict），其中的值和子值都可以和JSON兼容。

### 正确传参响应结果

{% asset_img "1.png" %}

### 终端输出信息

{% asset_img "2.png" %}

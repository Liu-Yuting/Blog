---
title: FastAPI-16：额外的数据类型
date: 2024-01-23 17:10:22
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-16：额外的数据类型

## 常见的数据类型

- int
- float
- str
- bool
- ...

FastAPI支持使用更复杂的数据类型，仍然可以得到FastAPI的支持：

- IDE智能提示；
- 请求数据的数据类型转换；
- 响应数据的数据类型转换；
- 数据验证；
- 自动注释和文档；

## 复杂的数据类型

### 1、UUID

- 常见的唯一标识符；
- str 类型；

### 2、datetime.datetime

- Python 的 datetime.datetime;
- str 类型；
- 栗子：2008-09-15T15:53:00+05:00；

### 3、datetime.date

- Python 的 datetime.date；
- str 类型；
- 栗子：2008-09-15；

### 4、datetime.time

- Python 的 datetime.time；
- str 类型；
- 栗子：15:53:00.003；

### 5、datetime.timedelta

- Python 的 datetime.timedelta；
- float 类型；
- 表示秒数；

### 6、frozenset

- set 类型；
- 在请求中，将读取一个列表，消除重复项并将其转换为一个集合；
- 在响应中，集合将被转换为列表；
- 会在 Schema 中加一个标识 uniqueItems，表示 set 里面的值是唯一的；

### 7、bytes

- Python 标准类型bytes；
- str 类型；
- 生成 Schema 会指定它为一个带有二进制格式的 str；

### 8、Decimal

- Python 标准类型十进制；
- float 类型；

### 重点

- FastAPI不只是有以上复杂的数据类型，更多的数据类型可以看[Pydantic Types](https://pydantic-docs.helpmanual.io/usage/types/)。
- 只要Pydantic有的，FastAPI都支持。

## 复杂数据类型的栗子

```python
from datetime import datetime, time, timedelta
from decimal import Decimal
from typing import Optional
from uuid import UUID
from fastapi import APIRouter, Body, Cookie
router = APIRouter()
@router.put("/items/{item_id}")
async def read_items(
        item_id: UUID,
        start_datetime: Optional[datetime] = Body(None),
        end_datetime: Optional[datetime] = Body(None),
        repeat_at: Optional[time] = Body(None),
        process_after: Optional[timedelta] = Body(None),
        address: Optional[frozenset] = Body(None),
        computer: Optional[bytes] = Body(None),
        age: Optional[Decimal] = Body(None),
):
    start_process = start_datetime + process_after
    duration = end_datetime - start_process
    return {
        "item_id": item_id,
        "start_datetime": start_datetime,
        "end_datetime": end_datetime,
        "repeat_at": repeat_at,
        "process_after": process_after,
        "start_process": start_process,
        "duration": duration,
        "address": address,
        "computer": computer,
        "age": age,
    }
```

### 正确传参的结果

{% asset_img "1.png" %}

### 错误传参的结果

{% asset_img "2.png" %}

### 查看Swagger API文档

{% asset_img "3.png" %}

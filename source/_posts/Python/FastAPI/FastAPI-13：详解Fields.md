---
title: FastAPI-13：详解Fields
date: 2023-11-17 14:57:34
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-13：详解Fields

## 针对Pydantic Model内部字段添加额外校验和元数据

## 前言

- 前面讲了Query、Path、Body，均可以对某个字段进行额外的校验和添加元数据；
- 这一篇来讲Fields，它针对 Pydantic Model 内部字段进行额外的校验和添加元数据。

## Fields

- 它是Pydantic提供的方法，并不是 FastAPI 提供的哦；
- 该方法返回了一个实例对象，是 Pydantic 中 FieldInfo 类的实例对象。

{% asset_img "FastAPI-13：详解Fields-1.png" %}

### 重点

FastAPI提供的Query、Path等其他公共Param类和Body类，都是Pydantic的FieldInfo类的子类

{% asset_img "FastAPI-13：详解Fields-2.png" %}

{% asset_img "FastAPI-13：详解Fields-3.png" %}

{% asset_img "FastAPI-13：详解Fields-4.png" %}

Query、Path继承Param、Param继承FieldInfo

{% asset_img "FastAPI-13：详解Fields-5.png" %}

Body直接继承FieldInfo

### 简单的栗子

```python
from typing import Optional
import uvicorn
from fastapi import FastAPI, Body
from pydantic import BaseModel, Field
app = FastAPI()
class Item(BaseModel):
    name: str
    description: Optional[str] = Field(
        default=None,
        title="标题",
        description="The description of the item",
        max_length=15,
    )
    price: float = Field(..., ge=0, description="需要大于0")
    tax: Optional[float] = None
@app.put("/items/{item_id}")
async def update_item(
        item_id: int,
        item: Item = Body(..., embed=True),
):
    results = {"item_id": item_id, "item": item}
    return results
if __name__ == '__main__':
    uvicorn.run(app='eleventh-11:app', host='0.0.0.0', port=8080, debug=True)
```

#### 正确传递参数的请求结果：

{% asset_img "FastAPI-13：详解Fields-6.png" %}

#### 查看 `Swagger API` 文档：

{% asset_img "FastAPI-13：详解Fields-7.png" %}

JSON Schema对加了 Fields 的字段会有详细的描述

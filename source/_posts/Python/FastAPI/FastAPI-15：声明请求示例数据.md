---
title: FastAPI-15：声明请求示例数据
date: 2024-01-22 17:07:43
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-15：声明请求示例数据

## 前言

- FastAPI可以给Pydantic Model或者路径函数声明需要接收的请求示例，而且可以显示在openAPI文档上。
- 有几种方式，接下来会详细介绍。

## Pydantic的json_schema_extra

可以使用`Config cass`和`json_schema_extra`为Pydantic Model声明一个示例值。

```python
from typing import Optional

from fastapi import APIRouter
from pydantic import BaseModel

router = APIRouter()


class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None

    # 内部类，固定写法
    class Config:
        json_schema_extra = {
            "example": {
                "name": "Foo",
                "description": "A very nice Item",
                "price": 35.4,
                "tax": 3.2,
            }
        }


@router.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```

### 查看Swagger API文档

{% asset_img "0.png" %}

{% asset_img "1.png" %}

注：无论是 Example Value 还是 Schema 都会显示声明的示例值。

## Field添加额外参数

使用Pydantic的Field()时，可以将任何其他任意参数添加到函数参数中，来生命JSON Schema的额外信息。

### Field的参数

{% asset_img "2.png" %}

默认Field是没有example参数的，而`**extra`就是关键字参数，标识可以添加关键字参数和常见的`**kwargs`是一个作用。

[关键字参数教程](https://www.cnblogs.com/poloyy/p/12526592.html) + [Field 教程](./FastAPI-13：详解Fields)

### 添加额外的参数：example参数

```python
class Item2(BaseModel):
    # 给每个字段加上了 example 参数
    name: str = Field(..., example="刘宇亭")
    description: Optional[str] = Field(None, example="描述")
    price: float = Field(..., example=1.11)
    tax: Optional[float] = Field(None, example=3.2)


@router.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```

#### 两个问题

- 一定要叫example吗？
  - 不一定，命名为其它的也可以；但是只有添加参数名为example的参数，SwaggerAPI上的Example Value才会显示这里传递的值（示例值）。
- 重点：
  - 因为这里的example是额外添加的，所以不进行数据验证；比如字段指定类型为str，example参数传递了数组也不回报错。

### 查看Swagger API文档

{% asset_img "3.png" %}

{% asset_img "4.png" %}

注：它是针对每个字段设置示例值，所以会显示在字段下面。

## OpenAPI中的example和examples参数

当使用FastAPI提供的

- Path()
- Query()
- Header()
- Cookie()
- Body()
- Form()
- File()

可以声明一个example或者examples参数，FastAPI自动将example或者examples的值添加到OpenAPI文档中。

{% asset_img "5.png" %}

### 总结

Pydantic没有直接支持example的参数，而FastAPI进行了扩展，直接支持添加example或者examples参数。

### 使用Body添加example参数

```python
@router.put("/Body/")
async def update_item(
        item: Item = Body(
            default=...,
            description="描述",
            # 添加一个 example 参数
            example={
                "name": "body name",
                "description": "body 描述",
                "price": 3.33,
                "tax": 5.55
            },
        ),
):
    results = {"item": item}
    return results
```

#### 查看Swagger API文档

{% asset_img "6.png" %}

{% asset_img "7.png" %}

### 使用Body添加examples参数

"""无法在新版本中重现，故此不继续说明"""

examples本身是一个dict，每个键标识一个具体的示例，而键对应的值也是一个dict；每个示例的dict可以包含：

- summary：简短描述。
- description：可以包含Markdown文本的长描述。
- value：显示的示例值。
- externalValue：替代值，指向示例的URL（不怎么用）。

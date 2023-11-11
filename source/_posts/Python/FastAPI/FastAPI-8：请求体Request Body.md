---
title: FastAPI-8：请求体Request Body
date: 2023-11-12 21:21:36
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-8：请求体Request Body

## 前言

- 接口传参方式之一：通过发送请求体（Request Body）来传递请求数据；
- 在FastAPI，提倡使用 `Pydantic` 模型来定义请求体；
- 这篇文章会详细讲不使用 `Pydantic` 和 使用 `Pydantic` 发送请求体的栗子。

### 注意

- 请求体并不是只有 `POST` 请求有，只不过POST更常见；
- 在 `PUT` 、`DELETE` 、`PATCH` 请求中都可以使用请求体；
- 其实，在 `GET` 请求中也可以使用请求体，不过仅适用于非常极端的情况下，**而且 `Swagger API` 并不会显示 `GET` 请求的请求体** 。

## 不使用Pydantic的栗子

```python
from fastapi import FastAPI
import uvicorn
app = FastAPI()
@app.post('/items')
async def read_item(item: dict):
    return {'item': item}
if __name__ == '__main__':
    uvicorn.run(app='sixth-6:app', host='0.0.0.0', port=8080, debug=True)
# 指定查询参数的类型为dice
```

#### 正确传参的结果：

{% asset_img "FastAPI-8：请求体Request Body-1.png" %}

### 重点

- 用 `postman` 发起请求的话，一定要选 `JSON` 格式哦；
- 因为接收的是 `dict` ，所以 `FastAPI` 会自动将 `JSON` 字符串转换为 `dict`；
- 这种场景下，虽然查询参数叫 `item` ，但请求体的字段名可以随意取，字段数量也可以任意一个。

## 用Dict代替dict的栗子

Dict 是 typing 模块提供的类，可以指定键值对的数据类型。

```python
from typing import Dict
@app.post("/Dict/")
# 键为str，值为float
async def create_index_weights(weights: Dict[str, float]):
    return weights
```

### 使用 `Dict` 相比直接使用 `dict` 的好处

声明为 `Dict[str, float]` ,FastAPI 会对每一个键值对都做数据校验，校验失败会有友好的错误提示。

#### 正确传参的请求结果：

{% asset_img "FastAPI-8：请求体Request Body-2.png" %}

#### 校验失败的请求结果：

{% asset_img "FastAPI-8：请求体Request Body-3.png" %}

## 使用 `Pydantic` 模型（建议使用）

### 实际栗子

```python
from typing import Optional
from pydantic import BaseModel
# 自定义一个Pydantic
class Item(BaseModel):
    name: strm
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None
# item 参数的类型指定为 Item 模型
@app.post('/items/')
async def create_item(item: Item):
    return item
```

### 参数指定为 `Pydantic` 模型后， FastAPI做了这几件事

1. 将请求体识别为 `JSON` 字符串；
2. 将字段值转换相应的类型（若需要）；
3. 验证数据，如果验证失败，会返回一个清洗的错误，准确支出错误数据的位置和信息；
4. `item` 会接收到完整的请求体数据，拥有所有属性及其类型，IDE也会给予对应的智能提示；
5. 给 `Pydantic` 模型自动的生成 `JSON Schema` ，这些 `Schema` 会成为生成 `OpenAPI Schema` 的一部分，并显示在接口文档上。

#### 正确传参的请求结果：

{% asset_img "FastAPI-8：请求体Request Body-4.png" %}

正常传参，所有属性按指定的类型进行传数据

### 字段类型自动转换

- `name: str` 传了bool类型的数据；
- `description: str` 传了float类型数据；
- `price: float` 传了int类型数据；
- `tax: float` 传了bool类型数据。

#### 请求结果：

{% asset_img "FastAPI-8：请求体Request Body-5.png" %}

FastAPi 会将传进来的值自动转换为指定类型的值

- 将 true 转成 str 类型，即 "True"
- 将 12.22 转成 str 类型，即 "12.22"
- 将 12 转成 float 类型，即 12.0
- 将 true 转成 float 类型，即 1.0

如果转换失败，则会报 `type_error` 错误（如图）

{% asset_img "FastAPI-8：请求体Request Body-6.png" %}

#### 查看 `Swagger API` 文档：

{% asset_img "FastAPI-8：请求体Request Body-7.png" %}

`model` 的 `JSON Schema` 会成为 `Swagger API` 文档的一部分

#### IDE 智能提示

因为知道name属性的类型是 str，所以IDE会智能提示str内置的方法

{% asset_img "FastAPI-8：请求体Request Body-8.png" %}

## Request Body + Path + Query Parameters 综合栗子

- 可以同时声明请求体、路径参数、查询参数；
- FastAPI可以识别出它们中的每一个，并从正确的位置获取到数据。

### 实际代码

```python
@app.post('/items/{item_id}')
async def create_item(
    # 路径参数
    item_id: int,
    # 请求体，模型类型
    item: Item,
    # 查询参数
    name: Optional[str] = None,
):
    result = {'item_id': item_id, **item.dice()}
    print(result)
    if name:
        # 如果查询参数 name 不为空，则替换掉 item 参数里面的 name 属性
        result.update({'name': name})
    return result
```

### FastAPI识别参数的逻辑

- 如果参数也在路径中声明，它将解释为路径参数【item_id】；
- 如果参数是单数类型（如int、float、str、bool等），它将被解释为查询参数【name】；
- 如果参数被声明为Pydantic模型的类型，它将被解释为请求体参数【item】。

#### 正确传参的请求结果：

{% asset_img "FastAPI-8：请求体Request Body-9.png" %}

#### Pycharm Console输出结果：

{% asset_img "FastAPI-8：请求体Request Body-10.png" %}

#### 查看 `Swagger API` 文档：

{% asset_img "FastAPI-8：请求体Request Body-11.png" %}

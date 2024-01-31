---
title: FastAPI-19：Response Model响应模型
date: 2024-01-26 09:34:49
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-19：Response Model响应模型

## 前言

- 前面文章写的这么多路径函数最终return的都是自定义结构的字典。
- FastAPI 提供了response_model参数，声明return响应体的模型。

### 什么是路径操作、路径函数

```python
# 路径操作
@app.post("/items/", response_model=Item)
# 路径函数
async def create_item(item: Item):
    ...
```

### 重点

response_model 是路径操作的参数，并不是路径函数的参数哦

- @app.get()
- @app.post()
- @app.put()
- @app.delete()

## 最简单的栗子

```python
from typing import Optional, List
from fastapi import APIRouter
from pydantic import BaseModel
router = APIRouter()
class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None
    tags: List[str] = []
@router.post("/response_model", response_model=Item)
async def response_model(item: Item):
    return item
# 上面代码栗子，请求模型和响应模型都是同一个 Pydantic Model
```

### FastAPI 通过 response_model 会做

- 将输出数据转换为Model中声明的类型。
- 验证数据。
- 在OpenAPI给Response添加JSON Schema和Example Value。
- 最重要：将输出数据限制为 model 的数据。

### 正确传参结果

{% asset_img "1.png" %}

### 查看Swagger

{% asset_img "2.png" %}

### 为什么 response_model 不是路径函数参数而是路径操作参数呢？

- 因为路径函数的返回值并不是固定的，可能是 dict、数据库对象，或其他模型。
- 但是使用响应模型可以对响应数据进行字段限制和序列化。

## 区分请求模型和响应模型的栗子

### 需求

- 假设一个注册功能
- 输入账号、密码、昵称、邮箱，注册成功后返回个人信息
- 正常情况下不应该返回密码，所以请求体和响应体肯定是不一样的

```python
from pydantic import BaseModel, EmailStr
class UserIn(BaseModel):
    username: str
    password: str
    email: EmailStr
    full_name: Optional[str] = None
class UserOut(BaseModel):
    username: str
    email: EmailStr
    full_name: Optional[str] = None
@router.post("/user/", response_model=UserOut)
async def create_user(user: UserIn):
    return user
```

- 即使请求数据包含了密码，但因为响应模型不包含 password，所以最终返回的响应数据也不会包含 password
- FastAPI 通过 Pydantic 过滤掉所有未在响应模型中声明的数据

### 正确传参请求结果

{% asset_img "3.png" %}

### 查看Swagger

{% asset_img "4.png" %}

## 看看路径参数有什么关于响应模型的参数

{% asset_img "5.png" %}

### response_model_exclude_unset

#### 作用

- 有时候数据会有默认值，比如数据库中设置了默认值，不想返回这些默认值怎么办？
- response_model_exclude_unset=True设置该参数后就不会返回默认值，只会返回实际设置的值，假设没设置值，则不返回该字段

#### 实际代码

```python
class Item(BaseModel):
    name: str
    price: float
    # 下面三个字段有默认值
    description: Optional[str] = None
    tax: float = 10.5
    tags: List[str] = []
items = {
    "foo": {"name": "Foo", "price": 50.2},
    "bar": {"name": "Bar", "description": "The bartenders", "price": 62, "tax": 20.2},
    "baz": {"name": "Baz", "description": None, "price": 50.2, "tax": 10.5, "tags": []},
}
@router.get("/items/{item_id}", response_model=Item, response_model_exclude_unset=True)
async def read_item(item_id: str):
    # 从上面 items 字典中，根据 item_id 取出对应的值并返回
    return items[item_id]
```

##### 1、item_id = foo

{% asset_img "6.png" %}

**不会返回有默认值的字段**

##### 2、item_id = bar

{% asset_img "7.png" %}

**只返回了设置值的字段**

##### 3、item_id = baz

{% asset_img "8.png" %}

**五个字段都有设置值，所以都包含在响应数据中了。即使description、tax、tags 设置的值和默认值是一样的，FastAPI 仍然能识别出它们是明确设置的值，所以会包含在响应数据中**

### response_model_include 和 response_model_exclude

#### 作用

- include：包含。
- exclude：排除。
- 其实就是响应模型只包含、排除这些属性。

#### 参数数据类型

- Optional：可选
- Union：联合类型

#### 官方建议

- 不推荐使用这两个参数，而推荐使用上面讲到的思想，通过多个类来满足请求模型、响应模型
- 因为在 OpenAPI 文档中可以看到 Model 完整的 JSON Schema

#### include的栗子

```python
@router.post("/include/", response_model=UserIn, response_model_include={"username", "email", "full_name"})
async def include(user: UserIn):
    return user
```

##### 正确传参的请求结果

{% asset_img "9.png" %}

##### 查看Swagger

{% asset_img "10.png" %}

**password仍然存在，这明显不是我们想要的最佳效果，所以还是推荐用多个类的思想**

#### exclude的栗子

```python
@router.post("/exclude/", response_model=UserIn, response_model_exclude={"password"})
async def exclude(user: UserIn):
    return user
```

##### 正确传参的请求结果

{% asset_img "11.png" %}

##### 查看Swagger

{% asset_img "12.png" %}

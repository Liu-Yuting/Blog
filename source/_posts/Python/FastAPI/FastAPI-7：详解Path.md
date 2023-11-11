---
title: FastAPI-7：详解Path
date: 2023-11-11 21:09:32
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-7：详解Path

## 前言

- 上一篇讲了可以为查询参数添加额外的校验和元数据，Query库；
- 这篇可以为路径查询添加额外的校验元数据，Path库。

## Path

可以为路径参数添加额外的校验和元数据，跟 `Query` 的参数是一毛一样的

{% asset_img "FastAPI-7：详解Path-1.png" %}

## 元数据

Path也可以添加元数据相关信息，这些信息将包含在生成的 `OpenAPI` 中，并由文档用户界面和外部工具使用

### 四种元数据参数

```python
# 别名
alias: Optional[str] = None
# 标题
title: Optional[str] = None
# 描述
description: Optional[str] = None
# 是否弃用
deprecated: Optional[bool] = None
```

### 实际代码

```python
from fastapi import FastAPI, Path
from typing import Optional
import uvicorn
app = FastAPI()
# 元数据
@app.get('/items/{item_id}')
async def read_items(
    	item_id: Optional[str] = Path(
            default=...,
            min_length=2,
            max_length=10,
            regex='^刘.*星$',
            title='Fuck',
            description='很长很长的描述',
            deprecated=True,
        )
):
    return {'item_id': item_id}
if __name__ == '__main__':
    uvicorn.run(app='fifth-5:app', host='0.0.0.0', port=8080, debug=True)
```

#### 校验成功的请求结果：

{% asset_img "FastAPI-7：详解Path-2.png" %}

#### 校验失败的请求结果：

{% asset_img "FastAPI-7：详解Path-3.png" %}

#### 查看 `Swagger API` 文档：

{% asset_img "FastAPI-7：详解Path-4.png" %}

### 重点

- 路径参数使用是必须的，必须是路径的一部分；
- 所以， `Path` 的 `default` 参数值必须设为 ...

### 元数据不应该使用 `alias` 

因为路径参数并不能通过 `参数名=value` 的形式来传参，所以没有办法通过 `alias = value` 的方式给别名传值，最终会报错。

```python
@app.get('/alias/{item_id}')
async def read_items(
    	item_id: Optional[str] = Path(default=..., alias='item_alias'),
):
    return {'item_id': item_id}
```

#### 请求结果：

{% asset_img "FastAPI-7：详解Path-5.png" %}

#### 不使用别名：

{% asset_img "FastAPI-7：详解Path-6.png" %}

#### 查看 `Swagger API` 文档，并运行：

{% asset_img "FastAPI-7：详解Path-7.png" %}

直接在 `Swagger API` 文档上尝试运行也会报错，所以路径参数不要使用别名参数哦！！！

### 函数参数排序问题

{% asset_img "FastAPI-7：详解Path-8.png" %}

Python会将 `item_id: Option[str] = Path(...)` 识别为默认参数，而 `name: str` 是位置参数，而位置参数不能在默认参数后面，所以报红了。

### 解决方案

```python
@app.get('/item/{item_id}')
async def read_items(
    	*,
    	item_id: int = Path(...),
    	name: str,
):
    return {'item_id': item_id, 'name': name}
# 将 * 作为第一个参数，那么 * 后面的所有参数都会当做关键字参数处理，即使它们没有设置默认值（像name）
```

#### 正常传参的结果：

{% asset_img "FastAPI-7：详解Path-9.png" %}

## 数字类型校验

`Query` 和 `Path` 都可以添加数字校验，`Query` 文章并没有讲解数字校验，所以这里重点讲一下！！！

### 数字校验参数

```python
# 大于
gt: Optional[float] = None
# 大于等于
ge: Optional[float] = None
# 小于
lt: Optional[float] = None
# 小于等于
le: Optional[float] = None
```

### 实际代码

```python
@app.get('/number/{item_id}')
async def read_items(
    	*,
    	item_id: Optional[int] = Path(..., title='The ID', gt=10, le=50),
    	name: str = None,
):
    return {'item_id': item_id, 'name': name}
```

#### 校验成功的请求结果：

{% asset_img "FastAPI-7：详解Path-10.png" %}

#### 校验失败的请求结果：

{% asset_img "FastAPI-7：详解Path-11.png" %}

## `Query` 和 `Path` 综合使用

```python
@app.get('/path_query/{item_id}')
async def read_items(
    	*,
        item_id: int = Path(..., description='path', ge=1, lt=5, example=1),
        name: str,
        age: float = Query(..., description='query', gt=0.0, le=10),
):
    return {'item_id': item_id, 'age': age, 'name': name}
```

#### 正确传参的请求结果：

{% asset_img "FastAPI-7：详解Path-12.png" %}

### 注意

数字校验也适用于 `float` 类型的值

#### 查看 `Swagger API` 文档：

{% asset_img "FastAPI-7：详解Path-13.png" %}

这里的 `item_id` 还加了个 `example` 参数，就是个示例值，所以在接口文档中会显示 `Example`

## 总结

- `Query` 、 `Path` 和后面会讲到的 `Form` 、 `Cookie` ... 等等，都是公共的 `Param` 类的子类，但实际开发中并不会直接使用 `Param` 类；
- 所有这些子类都共享相同的额外校验参数和元数据。

### Query 类

{% asset_img "FastAPI-7：详解Path-14.png" %}

### Path 类

{% asset_img "FastAPI-7：详解Path-15.png" %}

### Param 类

{% asset_img "FastAPI-7：详解Path-16.png" %}


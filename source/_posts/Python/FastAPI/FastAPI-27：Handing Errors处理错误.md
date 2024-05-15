---
title: FastAPI-27：Handing Errors处理错误
date: 2024-02-03 15:27:54
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-27：Handing Errors处理错误

## 前言

许多情况下，需要向用户返回一些特定的错误，比如：

- 客户端没有足够的权限进行该操作；
- 客户端无权访问该资源；
- 客户端尝试访问的项目不存在；
- ...。

### HTTPException

#### **介绍**

- 要将`带有错误的HTTP响应（状态码和响应信息）返回给客户端`，需要使用HTTPException；
- HTTPException是一个普通的exception，包含和API相关的附加数据；
- 因为是一个Python exception，应该raise它，而不是return它。

#### **查看一下HTTPException源码**

{% asset_img "1.png" %}

- status_code：响应状态码；
- detail：报错信息；
- headers：响应头。

## 简单的栗子

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time     : 2024/2/1 10:52 
# @Author   : 22759
# @Email    : lyt_sy@sina.com
# @Project  : FastApi-demo
# @File     : demo21
# @Software : PyCharm
from fastapi import APIRouter, HTTPException, status

router = APIRouter()
ITEMS = {'foo': 'The Foo Wrestlers'}


@router.get('/items/{item_id}')
async def read_items(item_id: str):
    if item_id not in ITEMS:
        raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail=f"item_id不存在")
    return {item_id: ITEMS[item_id]}
```

### 重点

可以传递任何可以转换为JSON字符串的值给detail参数，而不仅仅是str，可以是dict、list，它们由FastAPI自动处理并转换为JSON。

### item_id = foo的请求结果

{% asset_img "2.png" %}

### item_id = foos的请求结果

{% asset_img "3.png" %}

## 添加自定义Headers

```python
@router.get("/items-header/{item_id}")
async def read_item_header(item_id: str):
    if item_id not in ITEMS:
        raise HTTPException(
            status_code=status.HTTP_408_REQUEST_TIMEOUT,
            detail="Item not found",
            headers={"X-Error": "There goes my error"},
        )
    return {"item": ITEMS[item_id]}
```

### 找不到item_id的请求结果

{% asset_img "4.png" %}

## 自定义Exception Handlers

### 背景

- 假设有一个自定义异常UnicornException；
- 希望使用FastAPI全局处理批量异常；
- 可以使用`@app.exception_handler()`添加自定义异常处理程序。

```python
"""创建APP的时候, 设置自定义异常返回"""
class UnicornException(Exception):
    def __init__(self, name: str):
        self.name = name


@app.exception_handler(UnicornException)
async def custom_exception_handler(request: Request, exc: UnicornException):
    """自定义异常返回"""
    error = traceback.format_exc()
    return JSONResponse(
        status_code=status.HTTP_418_IM_A_TEAPOT,
        content={
            "msg": f"服务器内部错误：{exc.name}",
            "data": {"traceback": error}
        }
    )

"""demo21"""
@router.get("/unicorns/{name}/")
async def read_unicorn(name: str):
    if name == "unicorn":
        raise UnicornException(name=name)
    return {"unicorn_name": name}
```

### name = unicorn的请求结果

{% asset_img "5.png" %}

## 重写默认异常处理程序

- FastAPI 有一些默认的异常处理程序；
- 比如：当引发 HTTPException 并且请求包含无效数据时，异常处理程序负责返回默认的 JSON 响应；
- 可以使用自己的异常处理程序覆盖（重写）这些默认的异常处理程序。

### 重写HTTPException异常处理程序

```python
"""创建app的时候，重写HTTPException"""
from fastapi.responses import JSONResponse, PlainTextResponse
from fastapi.exceptions import RequestValidationError, ResponseValidationError
"""重写HTTPException异常处理程序"""
@app.exception_handler(HTTPException)
async def http_exception_handler(request: Request, exc: HTTPException):
    """将原来是JSONResponse，现在改成返回PlainTextResponse"""
    return PlainTextResponse(content=exc.detail, status_code=exc.status_code)
"""demo21"""
@router.get('/new-http-exception/{item_id}')
async def new_http_exception(item_id: int):
    if item_id == 1:
        raise HTTPException(status_code=status.HTTP_500_INTERNAL_SERVER_ERROR, detail="Nope! I don't like 1.")
    return {"item_id": item_id}
```

### item_id = 1的请求结果

{% asset_img "6.png" %}

## 重写请求验证异常的处理程序

当请求包含无效数据时，FastAPI会在内部引发RequestValidationError，它还包括一个默认的异常处理程序。

```python
"""创建app的时候，重写HTTPException"""
@app.exception_handler(RequestValidationError)
    async def validation_exception_handler(request: Request, exc: RequestValidationError):
        """重写ResponseValidationError异常处理程序"""
        return PlainTextResponse(content=str(exc), status_code=status.HTTP_400_BAD_REQUEST)
```

**怎样才会请求验证失败？**

item_id 声明为 int，传一个无法转成 int 的字符串就会抛出 RequestValidationError，比如 "str"。

### 在没有重写 RequestValidationError 异常处理程序前，请求验证失败的返回值

{% asset_img "7.png" %}

### 重写之后，请求验证失败的返回值

{% asset_img "8.png" %}

### 使用RequestValidationError的body属性

RequestValidationError 包含它收到的带有无效数据的正文，可以在开发应用程序时使用它来记录主体并调试它，将其返回给用户。

#### 看一眼 RequestValidationError 的源码

有一个body实例属性

{% asset_img "9.png" %}

### RequestValidationError vs ValidationError

- RequestValidationError是Pydantic的ValidationError的子类；
- 当使用了response_model，如果响应数据校验失败，就会抛出ValidationError；
- 客户端并不会直接收到ValidationError，而是会收到500，并报Internal Server Error服务器错误；这意味着就是服务端代码有问题；
- 正常来说，客户端看不到ValidationError是正确的，因为这可能会暴露安全漏洞。

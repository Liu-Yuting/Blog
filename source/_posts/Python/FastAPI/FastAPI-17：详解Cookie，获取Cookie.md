---
title: FastAPI-17：详解Cookie，获取Cookie
date: 2024-01-24 17:11:22
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-17：详解Cookie，获取Cookie

## FastAPI提供的Cookie

Cookie是Path和Query的“姐妹”类。它也继承自相同的通用类Param类。**注意**：从fastapi导入Query、Param、Cookie等时，这些实际上返回的是**特殊的函数**。

{% asset_img "1.png" %}

## 手动给浏览器设置Cookie

打开浏览器的控制台，输入：`document.cookie="name=test_cookie"`。

{% asset_img "2.png" %}

### 查看应用程序

{% asset_img "3.png" %}

## 读取Cookie

```python
@router.get("/items/")
async def read_items(
        name: Optional[str] = Cookie(None)
) -> dict:
    return {"name": name}
```

**重点**：函数参数的命名很重要，需要和应用程序中Cookie的名称对应上才能拿到Cookie。

### 浏览器访问该接口

因为上面手动在浏览器添加的Cookie，所以只能从浏览器测试该接口。

{% asset_img "4.png" %}

## 返回Set-Cookie

在一个正常的网站中，登录成功或者鉴权成功，服务器返回的响应会带着Set-Cookie，表示浏览器需要设置的一些Cookie。那么FastAPI是如何返回带有Set-Cookie的响应的呢？

```python
from fastapi.response import JSONResponse
@router.get("/cookie")
def cookie():
    content = {"message": "cookie"}
    response = JSONResponse(content=content, status_code=200)
    response.set_cookie(key='username', value='admin')
    return response
# 这里会用到FastAPI提供的响应模式，后面会详解，当前先做个了解方便演示
```

{% asset_img "5.png" %}

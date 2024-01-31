---
title: FastAPI-18：详解Header，获取Header
date: 2024-01-25 17:13:20
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-18：详解Header，获取Header

## FastAPI提供的Header

Header是Path、Query、Cookie的“姐妹”类。它也继承自相同的通用Param类。**注意**：从 fastapi 导入 Query、Path、Cookie、Header 等时，这些实际上是返回**特殊类的函数**。

{% asset_img "1.png" %}

有个参数`convert_underscores`，盲猜与转换下划线有关。

## 获取Header的栗子

```python
@router.get("/header")
async def get_header(accept_encoding: Optional[str] = Header(None)):
    return {"Accept-Encoding": accept_encoding}
```

### 浏览器访问接口

{% asset_img "2.png" %}

可以看到，获取的是 Request Header 里面的值。

### 思考：函数参数命名为 accept_encoding 为什么能识别到 Accept-Encoding？

- 首先，Accept-Encoding 这种变量名在 Python 是无效的。
- 因此，Header 默认情况下，会用下划线 _ 代替 - ，这就是 convert_underscores 参数的作用。
- 重点：HTTP Header 是不区分大小写的，所以写 accept_encoding 还是 Accept_Encoding 是一样效果的。

## 多个重名Header

假设一个Request Header里面有多个重名的Header，那可以用List[str]来声明参数类型。

```python
@router.get("/header-list")
async def read_items(x_token: Optional[List[str]] = Header(None)):
    return {"X-Token values": x_token}
# 假设Request Header有两个重名Header
X-Token: foo
X-Token: bar
# 访问接口/header-list得到的响应体会是
{
    'X-Token values': ["bar", "foo"]
}
```

## 设置Request Header

```python
@router.post("/header/")
async def set_header():
    content = {
        "name": "王德发",
        "age": 10
    }
    response = JSONResponse(content=content)
    token = {
        "x-token-name": "token",
        "x-token-value": "test_header"
    }
    # 设置 Header
    response.init_headers(token)
    return response
# 这里会用到FastAPI提供的响应模式，后面会详解，当前先做个了解方便演示
```

### 访问该接口

{% asset_img "3.png" %}

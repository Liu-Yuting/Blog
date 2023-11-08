---
title: FastAPI-4：路径参数Path Parameters
date: 2023-11-08 18:42:35
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-4：路径参数Path Parameters

## 什么是路径

- 假设一个 `url` 是：http://127.0.0.1:8080/items/abcd
- 那么路径 `path` 就是：/items/abcd

### 路径参数

就是将路径上的某一部分变成参数，可通过请求传递，然后 FastAPI 解析。

### 最简单的栗子

```python
import uvicorn
from fastapi import FastAPI
app = fastAPI()
# 路径参数 item_id
@app.get('/items/{item_id}')
async def read_item(item_id):
    return {'item_id': item_id}

if __name__ == '__main__':
    uvicorn.run(app='')
```

#### 请求结果：

{% asset_img "FastAPI-4：路径参数Path Parameters-1.png" %}

### 限定类型的路径参数

```python
# 指定类型的路径参数
@app.get('/items/{item_id}/article/{num}')
async def path_test(
    	item_id: str,
    	num: int
):
    return {"item_id": item_id, 'num': num}
# 多个路径参数，且有指定类型
```

#### 正确传参的请求结果：

{% asset_img "FastAPI-4：路径参数Path Parameters-2.png" %}

#### num不传int的结果：

{% asset_img "FastAPI-4：路径参数Path Parameters-3.png" %}

#### Swagger接口文档的显示效果

{% asset_img "FastAPI-4：路径参数Path Parameters-4.png" %}

### 路径函数顺序问题

```python
@app.get('/users/me')
async def read_user_me():
    return {'user_id': 'the current user'}

@app.get('/users/{user_id}')
async def read_user(user_id: str):
    return {'user_id': user_id}
# /users/{user_id} 路径是包含 /users/me 的
# 当想匹配到固定路径时，需要将固定路径函数放在路径参数函前面
```

#### 请求结果：

{% asset_img "FastAPI-4：路径参数Path Parameters-5.png" %}

### 将两个函数顺序调换过来

```python
@app.get('/users/{user_id}')
async def read_user(user_id: str):
    return {'user_id': user_id}

@app.get('/users/me')
async def read_user_me():
    return {'user_id': 'the current user'}
# 这样就无法匹配到固定路径 /users/me 函数了
```

#### 请求结果：

{% asset_img "FastAPI-4：路径参数Path Parameters-6.png" %}

## 数据转换器

### 前言

- 当你有一个路径是 `/files/{file_path}` ，但是不确定 `file_path` 到底会取什么值，并不是固定的长度，可能是 `/files/home/johndoe/myfile.txt` 也可能是 `/files/test/myfile.txt` ，那怎么办呢？
- 路径转换器来啦！

### 实际栗子

```python
@app.get('/files/{file_path:path}')
async def read_file(file_path: str):
    return {'file_path': file_path}
```

#### 请求结果：

{% asset_img "FastAPI-4：路径参数Path Parameters-7.png" %}

### 枚举类型的路径参数

```python
# 导入枚举类型
from enum import Enum
# 自定义枚举类
class ModelName(Enum):
    polo = 'polo'
    yy = 'yy'
    test = 'test'
    
@app.get('model/{model_name}')
# 类型限定为枚举类
async def get_model(model_name: ModelName):
    # 取枚举值方式一
    if model_name == ModelName.polo:
        return {'model_name': model_name, 'message': 'Oh!!!polo!!!'}
    # 取枚举方式二
    if model_name.value == 'yy':
        return {'model_name': model_name, 'message': 'God!!!yy!!!'}
    return {'model_name': model_name, 'message': '巴啦啦能量!!!'}
# 错误提示传的参数值并不是枚举类型中的值
```

#### 参数传枚举值请求结果：

polo:

{% asset_img "FastAPI-4：路径参数Path Parameters-8.png" %}

yy:

{% asset_img "FastAPI-4：路径参数Path Parameters-9.png" %}

test:

{% asset_img "FastAPI-4：路径参数Path Parameters-10.png" %}

#### 参数传入非枚举值的请求结果：

{% asset_img "FastAPI-4：路径参数Path Parameters-11.png" %}

## 重点：

### 路径参数可以不传吗？答案：不可以！路径参数是必传参数。

实际栗子：

{% asset_img "FastAPI-4：路径参数Path Parameters-12.png" %}

## 总结：

**路径参数是请求路径的一部分，如果不传，请求的是另一个路径，如果不存在就会404**



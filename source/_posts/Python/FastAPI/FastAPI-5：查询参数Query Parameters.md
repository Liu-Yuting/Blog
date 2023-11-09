---
title: FastAPI-5：查询参数Query Parameters
date: 2023-11-09 11:16:02
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-5：查询参数Query Parameters

## 什么是查询参数？

http://localhost:8080/get?name=xxx&age=18

http://localhost:8080/get?age=18&name=xxx

在url的 `?`  后面跟着的一组或多组键值对，就是查询参数

## FastAPI的查询参数

- 当声明了不属于路径参数以外的其他函数参数时，FastAPI会自动解析为查询参数；
- 和路径参数不同，查询参数可以是可选非必填的，也可以具有默认值。

### 路径参数 + 请求参数的栗子

```python
from fastapi import FastAPI
import uvicorn
app = FastAPI()
# 路径参数 + 请求参数
@app.get('/items/{item_id}')
async def read_item(
    item_id: str,
    name: str
):
    return {'item_id': item_id, 'name': name}
if __name__ == '__main__':
    uvicorn.run(app='***:app', host='0.0.0.0', port=8080, debug=True)
```

#### 正确传参的请求结果：

{% asset_img "FastAPI-5：查询参数Query Parameters-1.png" %}

### 必传参数 + 可选参数的栗子

```python
from typing import Optional
# 必传参数 + 可选参数
@app.get('/items')
async def read_item(
    	item_id: str,
    	name: Optional[str] = None
):
    return {'item_id': item_id, 'name': name}
# 如下：name没有传递参数取的值是None空
```

#### 不传 `name` 的请求结果：

{% asset_img "FastAPI-5：查询参数Query Parameters-2.png" %}

### 查询参数类型自动转换

```python
# 查询参数类型转换
@app.get('/items/{item_id}')
async def read_item(
    	item_id: str,
    	q: Optional[str] = None,
    	short: bool = False
):
    item = {'item_id': item_id}
    if q:
        item.update({'q': q})
    if not short:
        # 如果 short == False，则多加一个键description
        item.update({
            'description': 'This is an amazing item has a long description'
        })
    return item
```

#### `short` 是 `True` 的请求结果：

{% asset_img "FastAPI-5：查询参数Query Parameters-3.png" %}

#### `short` 是 `False` 的请求结果：

{% asset_img "FastAPI-5：查询参数Query Parameters-4.png" %}

### 指定枚举类型请求参数的栗子

```python
from enum import Enum
from typing import Optional, List
# 自定义枚举类型
class ModelName(Enum):
    boy = '男'
    girl = '女'
    unknown = '不知道'
@app.get('/item_enum')
async def read_item(
    	name: str,
    	sex: Optional[ModelName] = ModelName.unknown
):
    return {'name': name, 'sex': sex}
# 不传sex，会取sex的默认值：枚举类中的unknown的值
```

#### 参数传递枚举值的请求结果：

{% asset_img "FastAPI-5：查询参数Query Parameters-5.png" %}

#### 不传 `sex` 的请求结果：

{% asset_img "FastAPI-5：查询参数Query Parameters-6.png" %}

### 查询参数能用List[str]传参吗？

```python
# List[str]
@app.get('/list')
async def read_item(
	    address:List[str] = None
):
    return {'address': address}
```

#### 请求结果：

{% asset_img "FastAPI-5：查询参数Query Parameters-7.png" %}

- 即使参数值写成数组形式也不会传值成功，因为查询参数都是字符串；
- 所以 `['北京','上海','广州','深圳']` 其实是一个字符串str，并不是List[str]，那么怎样才能传数组呢？

### 分开多次传 `address` 可以吗？

{% asset_img "FastAPI-5：查询参数Query Parameters-8.png" %}

答案也是否定的

### 具体怎样做？

用Query库，下篇细说！！！

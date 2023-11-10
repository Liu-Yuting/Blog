---
title: FastAPI-6：Query库详解
date: 2023-11-10 17:43:36
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-6：Query库详解

### 可选参数

上篇讲过查询参数可以不是必传的，可以是可选参数

```python
from fastapi import FastAPI
from typing import Optional
import uvicorn 
app = FastAPI()
# 必传参数 + 可选参数
@app.get('/items')
async def read_item(
    	item_id: str,
    	name: Optional[str] = None
):
    return {'item_id': item_id, 'name': name}
if __name__ == '__main__':
    uvicorn.run(app='fourth-4:app', host='0.0.0.0', port=8080, debug=True)
# 可选其实也是一种校验
```

#### 请求结果：

{% asset_img "FastAPI-6：Query库详解-1.png" %}

## Query

为了对查询参数进行额外的校验，可以导入 `Query` 库

## Query支持多种校验

{% asset_img "FastAPI-6：Query库详解-2.png" %}

### 可选参数有默认值 + 长度最大为10

```python
# 需要先导入Query库
from fastapi import Query
@app.get('/items/')
async def read_items(
    	name: Optional[str] = Query(default=None, max_length=10)
):
    results = {'items': [{'item_id': 'Foo'}, {'item_id': 'Bar'}]}
    if name:
        results.update({'name': name})
    return results
```

#### 不传 `name` 的请求结果：

{% asset_img "FastAPI-6：Query库详解-3.png" %}

#### 传入 `name` 校验成功的请求结果：

{% asset_img "FastAPI-6：Query库详解-4.png" %}

#### `name` 长度大于10，校验失败的请求结果：

{% asset_img "FastAPI-6：Query库详解-5.png" %}

**友好的错误提示呀，直接说清楚哪个字段长度不满足了。。。**

```python
name: Optional[str] = Query(None)
# 等价于
name: Optional[str] = None
```

## Optional的作用

为了让IDE更好的支持智能提示

### 一个参数多个校验

```python
# 多条校验
@app.get('/items/twice')
async def read_items(
    	name: Optional[str] = Query(default='王德发', min_length=3, max_length=10)
):
    return {'name': name}
```

#### 校验成功的请求结果：

{% asset_img "FastAPI-6：Query库详解-6.png" %}

#### `name` 长度小于3，校验失败的请求结果：

{% asset_img "FastAPI-6：Query库详解-7.png" %}

### 添加正则表达式校验结果

```python
# 正则表达式
@app.get('/items/regular')
async def read_items(
    	name: Optional[str] = Query(default='王德发', min_length=3, max_length=10, regex='^刘.*星$')
):
    return {'name': name}
```

#### 校验成功的请求结果：

{% asset_img "FastAPI-6：Query库详解-8.png" %}

#### `name` 不满足正则，校验失败的请求结果：

{% asset_img "FastAPI-6：Query库详解-9.png" %}

### 必传参数 + 长度最小为3

#### 不使用Query时，查询参数怎么必传？

```python
# 不指定默认值就可以
name: str
```

#### 当使用Query时，查询参数怎么必传？

```python
# Query 默认值参数 default 是必传的，传了默认值不就变成可选参数了吗，那么怎么办呢？
# 必传参数
@app.get('/items/require')
async def read_items(
    	name: Optional[str] = Query(default=..., min_length=3)
):
    return {'name': name}
# 只需要将 `...` 赋值给default参数，FastAPI就会知道这个参数是必传的了
```

#### 校验成功的请求结果：

{% asset_img "FastAPI-6：Query库详解-10.png" %}

#### 没有传入 `name` 参数，校验失败的请求结果：

{% asset_img "FastAPI-6：Query库详解-11.png" %}

因为是必传参数，所以不传报错！！！

#### 查看 `Swagger API` 文档：

{% asset_img "FastAPI-6：Query库详解-12.png" %}

**大大的required标识，代表必传**

## List类型的查询参数

使用 `Query` 时，可以指定查询参数的类型为 List，即一个参数可以接收多个值

```python
from typing import List
# List[str]
@app.get('/list')
async def read_item(
    	address: Optional[List[str]] = Query(default=[], max_length=2)
):
    return {'address': address}
```

#### 没有传值的请求结果：

{% asset_img "FastAPI-6：Query库详解-13.png" %}

#### 正确传参的请求结果：

{% asset_img "FastAPI-6：Query库详解-1.png" %}

**设置了校验 `max_length=2` ，但是传了4个address也正常，证明这个 `max_length` 的校验对数组的长度并不生效**

#### 校验失败的请求结果：

{% asset_img "FastAPI-6：Query库详解-15.png" %}

**`max_length` 校验任然会对数组里面的字符串生效**

#### 查看 `Swagger API` 文档：

{% asset_img "FastAPI-6：Query库详解-16.png" %}

### List类型的查询参数有多个默认值

```python
@app.get('/list/default')
async def read_item(
    	address: Optional[List[str]] = Query(default=['北京', '上海'])
):
    return {'address': address}
```

#### 不传参数的结果：

{% asset_img "FastAPI-6：Query库详解-17.png" %}

## 元数据

Query可以添加元数据相关信息，这些信息将包含在生成的OpenAPI中，并由文档用户界面和外部工具使用

### 四种元数据参数

```python
# 别名
alias: Optional[str] = None
# 标题
title: Optional[str] = None
# 描述
description Optional[str] = None
# 是否弃用
deprecated: Optional[bool] = None
```

### 实际代码

```python
# 元数据
@app.get('/items/all')
async def read_items(
    	name: Optional[str] = Query(
        	default='王德发',
            min_length=3,
            max_length=10,
            regex='^刘.*星$',
            alias='name_alias_query',
            title='成功',
            description='长得帅',
            deprecated=True,
    	)
):
    return {'name': name}
```

#### 不使用 `alias` 进行传参的请求结果：

{% asset_img "FastAPI-6：Query库详解-18.png" %}

当做不存在的查询参数处理

#### 用 `alias` 进行传参的请求结果：

{% asset_img "FastAPI-6：Query库详解-19.png" %}

**定义了 `alias` ，记得要用 `alias` 进行传参**

#### 查看 `Swagger API` 文档：

{% asset_img "FastAPI-6：Query库详解-20.png" %}

- title字段并不会显示在这里，只会显示在JSON Schema中；
- 而JSON Schema只有请求参数方式为Request Body才会显示，这里是查询参数，所以并没有JSON Schema这一说，后面会介绍！！！

## 总结

限定于字符串的校验：

- min_length
- max_length
- regex

### Path

除了可以给查询参数添加额外的校验，也可以给路径参数添加额外的校验

Path的具体教程： https://www.cnblogs.com/poloyy/p/15308131.html 


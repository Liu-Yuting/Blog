---
title: FastAPI-23：详解Form，发送表单数据
date: 2024-01-30 09:59:53
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-23：详解Form，发送表单数据

## 前言

form-data：表单格式的请求数据其实也是挺常见的。FastAPI通过Form来声明参数需要接收表单数据。

## 安装python-multipart

```bash
# 要是用Form，需要先安装这个库
pip install python-multipart
```

## Form

Form继承自Body，所以可以定义和Body相同的元数据以及额外的验证。

{% asset_img "1.png" %}

## 简单的栗子

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time     : 2024/1/19 10:18 
# @Author   : 22759
# @Email    : lyt_sy@sina.com
# @Project  : FastApi-demo
# @File     : demo19
# @Software : PyCharm
from fastapi import APIRouter, Form

router = APIRouter()


@router.post('/form/')
async def form(username: str = Form(...), password: str = Form(...)):
    return {'username': username, 'password': password}
```

在 OAuth2 规范的一种使用方式（密码流）中，需要将用户名、密码作为表单字段发送，而不是 JSON【后面会详解 OAuth2】

### 重点

- 请求发送表单格式的数据，请求头通常包含`Content-Type: application/x-www-form-urlencoded`。
- 如果需要发送包含文件的表单数据，会变成`Content-Type: multipart/form-data`。

### 正确传参请求结果

{% asset_img "2.png" %}

### 请求头

{% asset_img "3.png" %}

### 查看Swagger

{% asset_img "4.png" %}

- 可以看到接口文档中，接口的Content-type默认也是`application/x-www-form-urlencoded`。
- **注意**：在Swagger上无法测试上传文件，因为Content-type无法切换到`multipart/form-data`，如果需要测试，要用FastAPI提供的File。
- [File详细教程](./FastAPI-24：详解File，上传文件)

---
title: FastAPI-21：多个模型的代码演进
date: 2024-01-28 09:36:02
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-21：多个模型的代码演进

## 前言

在一个完整的应用程序中，通常会有很多个相关模型，比如：

- 请求模型需要有password。
- 响应模型不能有password。
- 数据库模型可能需要一个哈希（hash）加密过的password。

## 多个模型的栗子

### 需求

1. 注册功能；
2. 请求输入密码；
3. 响应不需要密码；
4. 数据库存储加密后的密码；

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time     : 2024/1/18 13:37 
# @Author   : 22759
# @Email    : lyt_sy@sina.com
# @Project  : FastApi-demo
# @File     : demo17
# @Software : PyCharm
from typing import Optional

from fastapi import APIRouter, status
from pydantic import BaseModel, EmailStr

router = APIRouter()


class UserIn(BaseModel):
    """请求模型"""
    username: str
    password: str
    email: EmailStr
    full_name: Optional[str] = None


class UserOut(BaseModel):
    """响应模型"""
    username: str
    email: EmailStr
    full_name: Optional[str] = None


class UserInDB(BaseModel):
    """数据库模型"""
    username: str
    hashed_password: str
    email: EmailStr
    full_name: Optional[str] = None


def fake_password_hasher(password: str) -> str:
    """加密算法"""
    return "supersecret" + password


def fake_save_user(user: UserIn):
    """数据库存储"""
    """取出用户的密码进行加密"""
    hashed_password = fake_password_hasher(user.password)
    """转换为数据库类型"""
    user_db = UserInDB(**user.model_dump(), hashed_password=hashed_password)
    """返回数据"""
    return user_db


@router.post('/add-user/', response_model=UserOut, status_code=status.HTTP_201_CREATED)
def add_user(user: UserIn):
    """添加用户接口"""
    user_saved = fake_save_user(user)
    return user_saved

```

- `.model_dump()`替换`.dict()`使用。[Pydantic 入门篇](https://www.cnblogs.com/poloyy/p/15158713.html)
- `**user.model_dump()`：先将user转换为dict，然后再解包。[Python 解包教程](https://www.cnblogs.com/poloyy/p/15096333.html)

## 减少代码重复

### 核心思想

- 减少代码重复时FastAPI的核心思想之一。
- 因为代码重复增加了错误、安全问题、代码同步问题（当在一个地方更新而不在其它地方更新时）等的可能性。

### 上面代码存在的问题

- 三个模型都共享大量数据

### 利用Python继承的思想进行改进

1. 首先：声明一个`UserBase`模型，作为其它模型的基础；
2. 然后：创建该模型的子类来继承其属性（类型声明、验证等），所有数据类型转换、验证、文档等任然能正常使用；
3. 最后：不同模型之间的差异（使用明文密码、使用哈希密码、不使用密码）也很容易识别出来。

```python
"""优化后代码"""


class UserBase(BaseModel):
    """User基类"""
    username: str
    email: EmailStr
    full_name: Optional[str] = None


class NewUserIn(UserBase):
    """新用户请求模型"""
    password: str


class NewUserOut(UserBase):
    """新用户响应模型"""


class NewUserInDB(UserBase):
    """新用户数据库模型"""
    hashed_password: str


def new_fake_save_user(user: NewUserIn):
    hashed_password = fake_password_hasher(user.password)
    user_db = NewUserInDB(**user.model_dump(), hashed_password=hashed_password)
    return user_db


@router.post('/add-users/', response_model=NewUserOut, status_code=status.HTTP_201_CREATED)
def add_user(user: NewUserIn):
    user_db = new_fake_save_user(user)
    return user_db
```

### 正确传参的响应结果

{% asset_img "1.png" %}

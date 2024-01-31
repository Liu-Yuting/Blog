---
title: FastAPI-24：详解File，上传文件
date: 2024-01-31 10:00:26
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-24：详解File，上传文件

## 前言

可以使用FastAPI提供的File定义客户端要上传的文件。学习File前最好先学习[Form](./FastAPI-23：详解Form，发送表单数据)。

## 安装python-multipart

```shell
# 要是用File，需要先安装python-multipart
$ pip install python-multipart
```

## File

File是继承Form，所以可以定义和Form相同的元数据以及额外的验证。

{% asset_img "1.png" %}

## 上传单个文件的栗子

```python
from fastapi import APIRouter, Form, File, UploadFile

router = APIRouter()

@router.post('/file/')
async def file(files: bytes = File(...)):
    return {'files': len(files)}


@router.post('/uploadFile/')
async def upload_file(files: UploadFile = File(...)):
    result = {
        'filename': files.filename,
        'content-type': files.content_type,
        'read': await files.read(),
    }
    return result
```

- 因为UploadFile对象提供的方法都是async异步的，所以调用的时候都要加await比如`await file.read()`（后面会详解async/await）。
- 当使用异步方法时，FastAPI在线程池中运行文件方法并等待它们。

### 不加await调用async方法会报错

{% asset_img "2.png" %}

{% asset_img "3.png" %}

### file: bytes的请求结果

{% asset_img "4.png" %}

### file: UploadFile的请求结果

{% asset_img "5.png" %}

### 查看Swagger

{% asset_img "6.png" %}

## file: bytes和file: UploadFile

### bytes

- FileAPI将会读取文件，接收到内容就是文件字节。
- 会将整个内容存储在内存中，更适用于小文件。

### UploadFile

FastAPI 的 UploadFile 直接继承了 Starlette 的 UploadFile，但增加了一些必要的部分，使其与 Pydantic 和 FastAPI 的其他部分兼容。

### UploadFile相比bytes优势

- 存储在内存中的文件达到最大大小限制，超过此限制后，它将存储在磁盘中，可以很好地处理大文件，如图像、视频、大型二进制文件等，而不会消耗所有内存。
- 可以从上传的文件中获取元数据。
- 有一个类似文件的async异步接口。
- 它公开了一个Python SpooledTemporaryFile对象，可以将它传递给其他需要文件的库。

### UploadFile具有以下属性

- filename：str，上传的源文件名，例如：requirements.txt。
- content_type：str，包含Content-type（MIME type / media type），例如：image/jpeg。
- file：一个SpooledTemporaryFile（一个类似文件的对象）。这是实际的Python文件，可以将其直接传递给其他需要“类文件”对象的函数或库。

### UploadFile具有以下async异步方法

- write(data)：写入data（str或bytes）到文件。
- read(size)：读取文件的size（int）个字节/字符。
- seek(offset)：转到文件中的字节位置offset（int），如：`await myfile.seek(0)`将转到文件的开头。
- close()：关闭文件。

```python
@router.post('/files/')
async def files_list(files: List[bytes] = File(...)):
    return {'file_sizes': [len(f) for f in files]}


@router.post('/uploadFiles/')
async def upload_files(files: List[UploadFile] = File(...)):
    return {'filenames': [f.filename for f in files]}
```

### 正确传参的响应结果

{% asset_img "7.png" %}

{% asset_img "8.png" %}

### 查看Swagger

{% asset_img "9.png" %}

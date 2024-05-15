---
title: FastAPI-25：File、From混合使用
date: 2024-02-01 11:08:46
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-25：File、Form混合使用

## 路径函数混合使用Form、File

```python
@router.post('/file_form/')
async def file_form(
        files: bytes = File(...),
        file_b: UploadFile = File(...),
        token: str = Form(...)
):
    return {
        "file_size": len(files),
        "file_b_content_type": file_b.content_type,
        "token": token
    }
```

### 正确传参的结果

{% asset_img "1.png" %}

**注意**：发送表单数据的时候如果有文件，一定要用 **form-data** ！！！

### 查看Swagger文档

{% asset_img "2.png" %}

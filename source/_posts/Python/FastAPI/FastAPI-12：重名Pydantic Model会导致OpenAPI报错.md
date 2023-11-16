---
title: FastAPI-12：重名Pydantic Model会导致OpenAPI报错
date: 2023-11-16 16:07:43
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-12：重名Pydantic Model会导致OpenAPI报错

## 背景

在一个 Python 模块中，如果包含两个同名的 Pydantic Model，访问 /docs 会报错哦！！！

```python
from typing import Optional
import uvicorn
from fastapi import Body, FastAPI
from pydantic import BaseModel
app = FastAPI()
class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None
class Item(BaseModel):
    it: str
    address: str
if __name__ == '__main__':
    uvicorn.run(app='tenth-10:app', host='0.0.0.0', port=8080, debug=True)
```

#### IDE就提示了

{% asset_img "FastAPI-12：重名Pydantic Model会导致OpenAPI报错-1.png" %}

#### 启动uvicorn，并访问 /docs：

{% asset_img "FastAPI-12：重名Pydantic Model会导致OpenAPI报错-2.png" %}

**注意啦！一个 Python 模块中不要有重名的 Pydantic Model 哦！！ **

**注意啦！一个 Python 模块中不要有重名的 Pydantic Model 哦！！ **

**注意啦！一个 Python 模块中不要有重名的 Pydantic Model 哦！！ **
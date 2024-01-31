---
title: FastAPI-14：Pydantic Model
date: 2024-01-21 17:06:27
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-14:Pydantic Model

## 有类型参数的字段

Python有一种特定的方法来声明具有内部类型或类型参数的列表

其实前面都见过，就是

```python
List[str]
Set[str]
Tuple[str]
Dict[str]
```

- List、Set、Tuple、Dice都是从typing模块中导入的。
- typing常见类型提示，详细教程：https://www.cnblogs.com/poloyy/p/15150315.html

## 在Pydantic Model中使用typing提供的类型

```python
from typing import List, Optional, Set, Dict, Tuple
from pydantic import BaseModel
class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None
    tags: List[str] = []
    address: Set[str] = set()
    phone: Tuple[int] = ()
    ext: Dict[str, str] = {}
item = Item(name="刘星星", price=12.2)
print(dict(item))
# 程序输出结果
# >>> {'name': '刘星星', 'description': None, 'price': 12.2, 'tax': None, 'tags': [], 'address': set(), 'phone': (), 'ext': {}}
```

## Pydantic嵌套模型

```python
from typing import List
from pydantic import BaseModel
# 模型 1
class Foo(BaseModel):
    count: int
    size: float = None
# 模型 2
class Bar(BaseModel):
    apple: Optional[str] = 'x'
    banana: Optional[str] = 'y'
# 模型 3
class Spam(BaseModel):    
   # 字段类型是 Pydantic Model，这就是嵌套模型
    foo: Foo
    bars: List[Bar]
f = Foo(count=2)
b = Bar()
s = Spam(foo=f, bars=[b])
print(dict(s))
# 上述程序执行结果
# >>> {'foo': Foo(count=2, size=None), 'bars': [Bar(apple='x', banana='y')]}
```

## FastAPI中使用Pydantic嵌套模型

```python
# 模型一
class Image(BaseModel):
    url: str
    name: str


# 模型二
class Items(BaseModel):
    name: str
    price: float
    description: Optional[str] = None
    tags: Set[str] = set()
    # Image 模型组成的列表类型
    image: Optional[List[Image]] = None


@router.post("/items/{item_id}")
async def update_item(
        item_id: int,
        # 声明类型为：嵌套模型的 Item
        item: Items
):
    results = {"item_id": item_id, "item": item}
    return results
```

### 期望得到的请求体

```python
{
  "name": "string",
  "price": 0,
  "description": "string",
  "tags": [],
  "image": [
    {
      "url": "string",
      "name": "string"
    }
  ]
}
```

### 重点

tags 虽然声明为 Set()，但在接口层面并没有集合这个概念，所以还是传数组 [ ] 格式哦，并不是传 { } 哦。但是！集合的特性仍然会保留：**`去重`**

### FastAPI给嵌套模式提供的功能

和前面讲的没什么区别

- IDE 智能代码提示，甚至对于嵌套模型也支持
- 数据转换
- 数据验证
- OpenAPI 文档

### 正确传参结果

{% asset_img "0.png" %}

如上：第二个框中的内容将“标签1”去重了。

### 校验失败的请求结果

{% asset_img "1.png" %}

### 查看Swagger API文档

{% asset_img "2.png" %}

## 深层次嵌套模型

```python
# 模型一
class Image(BaseModel):
    url: str
    name: str


# 模型二
class Items(BaseModel):
    name: str
    price: float
    description: Optional[str] = None
    tags: Set[str] = set()
    # Image 模型组成的列表类型
    image: Optional[List[Image]] = None


# 模型三
class Offer(BaseModel):
    name: str
    description: Optional[str] = None
    items: List[Items]


@router.post("/offers/")
async def create_offer(offer: Offer):
    return offer
```

### 期望得到的请求体

```python
{
    "name": "string",
    "description": "string",
    "items": [
        {
            "name": "string",
            "price": 0,"tags": [],
            "images": [
                {
                    "url": "string",
                    "name": "string"
                }
            ]
        }
    ]
}
```

### 正确传参的请求结果

{% asset_img "3.png" %}

### IDE提供的智能提示

即使是三层嵌套模型，也可以拥有丝滑般的代码提示哦！！！

{% asset_img "4.png" %}

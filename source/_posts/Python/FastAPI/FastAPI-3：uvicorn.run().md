---
title: FastAPI-3：uvicorn.run()
date: 2023-11-07 18:53:27
---
# FastAPI-3：uvicorn.run()

## Uvicorn

- 基于 `uvloop` 和 `httptools` 构建的非常快速的 `ASGI` 服务器；
- 它不是一个 `Web` 框架，而是一个服务器；
- 例如，他不是一个提供路径路由的框架，这是 `FastAPI` 框架提供的东西；
- 它是 `Starlette` 和 `FastAPI` 的推荐使用的服务器。

### 总结

`uvicorn` 是运行 `FastAPI` 应用程序的主要 `Web` 服务器，`uvicorn` 和 `Gunicorn` 结合使用，拥有一个异步多进程服务器。

## 什么是ASGI、WSGI

https://www.cnblogs.com/poloyy/15291403.html

## 最简单的 FastAPI代码

```python
from fastapi import FastAPI
app = FastAPI()
@app.get('/')
async def first():
    return {'message': 'Hello World'}
```

## 启动 uvicorn 

进到 `.py` 文件所处目录下的命令运行

```python
uvicorn main:app --reload
```

![1](../../static/Python/FastAPI/FastAPI-2：快速入门-1.png)

能不能不用命令行方式运行呢，否则太不方便了？ --可以！

### 使用 `uvicorn.run()`

```python
from fastapi import FastAPI
app = FastAPI()
@app.get('/')
async def first():
    return {'message': 'Hello World'}
if __name__ == '__main__':
    uvicorn.run(app='main:app', host='0.0.0.0', post=8010, reload=True, debug=True)
# 这样就不用敲命令行啦；
# uvicorn 有什么命令行参数，run()方法就有什么参数。
```

### uvicorn常用参数

| 参数        | 作用                                   |
| ----------- | -------------------------------------- |
| app         | 运行的 .py 文件:FastAPI实例对象        |
| host        | 访问url，默认 127.0.0.1                |
| port        | 访问端口，默认8080                     |
| reload      | 热更新，有内容修改自动重启服务器       |
| debug       | 同 reload                              |
| reload_dirs | 设置需要 reload 的目录，List[str] 类型 |
| log_level   | 设置日志级别，默认 info                |


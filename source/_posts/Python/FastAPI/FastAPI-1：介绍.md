---
title: FastAPI-1：介绍
date: 2023-11-06 15:19:30
author: 刘宇亭
category:
    - Python
    - FasAPI
tag:
    - Python
    - FastAPI
---
# FastAPI-1：介绍

## 前言

为啥要学它呢，因为学 `Flask` 的时候发现有人更推荐它代替 `Flask` ，看了下介绍，感觉很强，而且也能拿来做平台，当然学起来！卷起来！

## 为什么要是用FastAPI？

- 日渐没落的是后端HTML渲染这种方式，比如 `Flask + Jinja2` 
- 前后端分离成为主流
- 异步框架

官方地址：https://fastapi.tiangolo.com/

## FastAPI是什么？

- `FastAPI` 是一个现代、快速（高性能）的web框架
- 用于基于标准 `Python` 类型提示是用 `Python 3.6+` 构建API

## FastAPI版本要求

Python3.6+

## FastAPI优点

官方说明：

- 类型检查、自动swagger UI、支持asyncio、强大的依赖注入系统；
- 围绕着框架本身的插件生态，比如pydantic、SQLAlchemy，成熟；
- 速度快：非常高的性能，与 `NodeJS` 和 `Go` 不相上下，多亏 `Starlette` 和 `Pydantic` ， FastAPI是最快的 `Python` 框架之一；
- 编码快：将开发特性所需的速度提高大约 200% 到 300%；
- 错误少：减少大约 40% 的人为（开发）错误；
- 直观：强大的编辑器支持，支持多场景开发，调试所花的时间更少；
- 简单：被设计为易于使用和学习，减少阅读文档的时间；
- 代码少：最小化重复，更少的错误；
- 健壮：代码可随时部署到生产环境，并自动提供交互文档；
- 标准：基于（并完全兼容）api的开放标准：OpenAPI（以前称为Swagger）和JSON模式

## Pydantic 在 FastAPI

- FastAPI是完全建立在Pydantic的基础上的；
- Pydantic是一个用来执行数据校验的Python库，具体教程可看：https://www.cnblogs.com/poloyy/tag/Pydantic/

## Type Hints 在FastAPI

- Type Hints 介绍：https://www.cnblogs.com/poloyy/p/15145380.html
- typing 模块：https://www.cnblogs.com/poloyy/p/15150315.html

### 使用FastAPI时用Type Hints声明参数可以获得

- 编辑器支持智能提示，错误检查；
- 类型检查，不对会报warning；

### FastAPI还会用类型提示来做

- **定义参数要求** ：声明对请求路径参数、查询参数、请求头、请求体、依赖等的要求；
- **转换数据** ：将来自请求的数据转换为需要的类型；
- **校验数据 **：对于每一个请求当数据校验失败时自动生成错误信息返回给客户端；
- **使用 OpenAPI 记录API** ：然后用于自动生成交互式文档的用户界面，参数会显示对应的类型注释。
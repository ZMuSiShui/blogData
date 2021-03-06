---
title: FastAPI(六)
date: 2021-03-29 14:32:41
tags:
  - FastAPI
  - Python
  - 学习笔记
categories:
  - Python
  - FastAPI
---

## 响应状态码

在以下任意的路径操作中使用 `status_code` 参数来声明用于响应的 HTTP 状态码：

- `@app.get()`
- `@app.post()`
- `@app.put()`
- `@app.delete()`

```python
from fastapi import FastAPI

app = FastAPI()


@app.post("/items/", status_code=201)
async def create_item(name: str):
    return {"name": name}
```

**注意**: `status_code` 是「装饰器」方法（`get`，`post` 等）的一个参数。它不属于路径操作函数。

`status_code` 参数接收一个表示 HTTP 状态码的数字。

`status_code` 也能够接收一个 `IntEnum` 类型，比如 Python 的 [`http.HTTPStatus`](https://docs.python.org/3/library/http.html#http.HTTPStatus)

## 常用 HTTP 状态码

- **`100`** 接受的请求正在处理

- **`200`** 请求正常处理完毕

  - `200` **：**表示从客户端发送给服务器的请求被正常处理并返回；
  - `201` **：**表示客户端发送给客户端的请求得到了成功处理，但在返回的响应报文中不含实体的主体部分（没有资源可以返回）；
  - `204` **：**表示客户端进行了范围请求，并且服务器成功执行了这部分的GET请求，响应报文中包含由Content-Range指定范围的实体内容。

- **`300`** 需要进行附加操作以完成请求

  - `301` **：**永久性重定向，表示请求的资源被分配了新的URL，之后应使用更改的URL；

  - `302` **：**临时性重定向，表示请求的资源被分配了新的URL，希望本次访问使用新的URL；

    301与302的区别：前者是永久移动，后者是临时移动（之后可能还会更改URL）

  - `303` **：**表示请求的资源被分配了新的URL，应使用GET方法定向获取请求的资源；

    302与303的区别：后者明确表示客户端应当采用GET方式获取资源

  - `304` **：**表示客户端发送附带条件（是指采用GET方法的请求报文中包含if-Match、If-Modified-Since、If-None-Match、If-Range、If-Unmodified-Since中任一首部）的请求时，服务器端允许访问资源，但是请求为满足条件的情况下返回改状态码；

  - `307` **：**临时重定向，与303有着相同的含义，307会遵照浏览器标准不会从POST变成GET；（不同浏览器可能会出现不同的情况）；

- **`400`** 客户端请求出错，服务器无法处理请求

  - `400` **：**表示请求报文中存在语法错误；
  - `401` **：**未经许可，需要通过HTTP认证；
  - `403` **：**服务器拒绝该次访问（访问权限出现问题）
  - `404` **：**表示服务器上无法找到请求的资源，除此之外，也可以在服务器拒绝请求但不想给拒绝原因时使用；

- **`500`** 服务器处理请求出错

  - `500` **：**表示服务器在执行请求时发生了错误，也有可能是web应用存在的bug或某些临时的错误时；
  - `503` **：**表示服务器暂时处于超负载或正在进行停机维护，无法处理请求；


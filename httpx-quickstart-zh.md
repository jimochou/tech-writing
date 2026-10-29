# httpx 快速上手（中文版）

本文用 10 分钟带你装上 httpx，并发出第一个 HTTP 请求。

## 关于本文

1. 本文档面向对 Python 有了解，但不熟悉 HTTP 的读者。

2. 本文档覆盖安装、第一个请求和常见问题。

3. 本文档不覆盖 Python 的安装教程。


## 开始之前

- 需要 Python 3.9 或更高版本
- 打开终端，运行 `python --version`
- 看到 `Python 3.9` 以上（例如 `Python 3.14.7`）就可以继续
- 低于 3.9 或提示找不到命令，见文末 [常见问题](#常见问题)

## 安装

在终端执行：

```powershell
PS > python -m pip install httpx
```

安装完成后，输出最后会出现类似这样一行：

```text
Successfully installed anyio-4.15.1 certifi-2026.10.23 h11-0.16.0
httpcore-1.0.9 httpx-0.28.1 idna-3.20 typing_extensions-4.16.0
```

依赖较多，中间会有若干 `Collecting` / `Downloading` 行；
版本号在不同时间可能不同，只要出现 `Successfully installed` 且包含 `httpx` 即为成功。

验证一下：

```powershell
PS > python -c "import httpx; print(httpx.__version__)"
0.28.1
```

httpx 版本号为 0.28.1，验证安装成功。

## 第一个请求

执行以下命令发送一个 GET 请求：

```powershell
PS > python -c "import httpx; r = httpx.get('https://example.com'); print(r)"
<Response [200 OK]>
```

`httpx.get()` 发送一个 GET 请求——HTTP 里最常用的方法，意思是"我要读取这个地址的内容"。

输出 `<Response [200 OK]>` 中，200 是 HTTP 状态码，表示请求成功。

## 术语表速查

- GET：HTTP 方法之一，用于读取资源。另有 POST（提交数据）、PUT（更新）、DELETE（删除）等方法。
- HTTP：浏览器与服务器之间请求和响应的通信规则。
- 状态码：服务器返回的三位数字，200 表示成功，404 表示找不到。

## 常见问题

### 低于 3.9 或提示找不到命令，说明本机没有可用的 Python

因本文档不覆盖 Python 安装，请到 [Python 下载页面](https://www.python.org/downloads/) 安装 3.9 以上版本。

装好后回到本文「开始之前」第 2 步确认。

### 报错 ModuleNotFoundError

```text
ModuleNotFoundError: No module named 'httpx'
```

原因：httpx 没有装进当前的 Python 环境，通常是跳过了 [安装](#安装) 一节。

解决：回到 [安装](#安装) 一节，按那里的命令重装一次并验证。
若重装后仍报错，确认 `python --version` 与你安装时用的是同一个解释器。

### 报错 httpx command line client could not run

```text
The httpx command line client could not run…
```

原因：`httpx` 命令需要额外依赖，基础安装不含命令行功能。

解决：`python -m pip install "httpx[cli]"`

## 下一步

- 完整功能示例：[httpx 官方 Quickstart](https://www.python-httpx.org/quickstart/)

- API 参考：[Developer interface](https://www.python-httpx.org/api/)

- 异步写法：[Async](https://www.python-httpx.org/async/)
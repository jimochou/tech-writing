# httpx 快速上手（中文版）

本文用 10 分钟带你装上 httpx，并发出第一个 HTTP 请求。

## 关于本文

1. 本文档面向对 Python 有了解，但不熟悉 HTTP 的读者。

2. 本文档覆盖安装、第一个请求、术语表、常见问题和验证情况。

3. 本文档不覆盖 Python 的安装教程。

4. 本文的命令都在 Windows PowerShell 中执行，代码块里只写命令本身，直接复制即可


## 开始之前

- 需要 Python 3.9 或更高版本（httpx 0.28 实际支持 3.8，但 3.8 已停止维护，本文按 3.9 起）
- 打开终端，运行 `python --version`
- 看到 `Python 3.9` 或更高（例如 `Python 3.14.7`）就可以继续
- 低于 3.9 或提示找不到命令，见文末 [常见问题](#常见问题)



## 安装

在终端执行：

```powershell
python -m pip install httpx
```

安装完成后，最后会看到类似这一行：

```text
Successfully installed … httpx-0.28.1 …
```

安装过程中可能出现若干 `Collecting` / `Downloading` 行，
也可能出现 Retrying 之类的警告——只要最后出现
Successfully installed 且包含 httpx，就说明成功。

如果最后一行是：

```text
Requirement already satisfied: httpx …
```

说明本机之前就已经装好，直接做下一步验证。

验证一下：

```powershell
python -c "import httpx; print(httpx.__version__)"
```

输出：

```text
0.28.1
```

输出形如 `0.28.x` 即为安装成功（以你实际看到的版本为准；本文写作时为 0.28.1）。

## 第一个请求

执行以下命令发送一个 GET 请求：

```powershell
python -c "import httpx; r = httpx.get('https://example.com'); print(r)"
```

输出：

```text
<Response [200 OK]>
```

`httpx.get()` 发送一个 GET 请求——HTTP 里最常用的方法，意思是“我要读取这个地址的内容”。

输出 `<Response [200 OK]>` 中，200 是 HTTP 状态码，表示请求成功。

如果这一步报错（例如 `httpx.ConnectError`、`httpx.ProxyError`），先确认这台机器能打开
[https://example.com](https://example.com)，浏览器也打不开就是网络问题，见 [常见问题](#常见问题)。

## 术语表速查

- GET：HTTP 方法之一，用于读取资源。另有 POST（提交数据）、PUT（更新）、DELETE（删除）等方法。
- HTTP：浏览器与服务器之间请求和响应的通信规则。
- 状态码：服务器返回的三位数字，200 表示成功，404 表示找不到。

## 常见问题

### 低于 3.9 或提示找不到命令

先运行 `python --version`，对照下面：

| 你看到的 | 说明 | 该做什么 |
|---|---|---|
| 弹出微软商店，或什么都不输出 | 应用执行别名占位符 | 设置 → 应用 → 高级应用设置 → 应用执行别名，关掉 python 的开关 |
| “无法将 python 项识别为 …” | 没装，或没加 PATH | 装 Python 时勾 **Add python.exe to PATH**；装完**关掉终端重开** |
| 版本低于 3.9 | 版本太旧 | 到 [Python 下载页面](https://www.python.org/downloads/) 安装 3.9 或更高版本，安装时务必勾选 “Add python.exe to PATH” |

### 安装卡住或出现 Retrying / ConnectionResetError

原因：从 pypi.org 下载时网络被中断，国内网络较常见。
pip 会自动重试，通常几分钟后仍能成功。

如果一直失败，临时换国内镜像：

```powershell
python -m pip install httpx -i https://pypi.tuna.tsinghua.edu.cn/simple
```

想让之后的 pip 命令都走镜像（例如本文后面装 `httpx[cli]` 时），可以写入配置：

```powershell
python -m pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

### 报错 ModuleNotFoundError

输出：

```text
ModuleNotFoundError: No module named 'httpx'
```

原因：httpx 没有装进当前的 Python 环境，通常是跳过了 [安装](#安装) 一节。

解决：回到 [安装](#安装) 一节，按那里的命令重装一次并验证。
若重装后仍报错，确认 `python --version` 与你安装时用的是同一个解释器。

### 报错 httpx command line client could not run

如果在终端直接输入 `httpx`（而不是用 `python -c`），会看到：

```text
The httpx command line client could not run…
```

原因：`httpx` 命令需要额外依赖，基础安装不含命令行功能。

解决：

```powershell
python -m pip install "httpx[cli]"
```

### 第一个请求报错 ConnectError / ProxyError

现象：`httpx.get('https://example.com')` 抛出 `httpx.ConnectError`、`httpx.ProxyError` 或 `httpx.ConnectTimeout`。

原因：请求根本没连上。常见于断网、公司代理拦截目标站点，或代理设置（系统代理、代理环境变量）指向了不可用的地址。

排查：

1. 用浏览器打开 [https://example.com](https://example.com)，打不开就是网络问题
2. 不需要代理时，清掉代理环境变量后重试：

    ```powershell
    Remove-Item Env:HTTPS_PROXY, Env:HTTP_PROXY -ErrorAction SilentlyContinue
    ```

3. 需要代理但地址变了，先设好再试：

    ```powershell
    $env:HTTPS_PROXY = "http://代理地址:端口"
    ```

## 下一步

- 完整功能示例：[httpx 官方 Quickstart](https://www.python-httpx.org/quickstart/)

- API 参考：[Developer interface](https://www.python-httpx.org/api/)

- 异步写法：[Async](https://www.python-httpx.org/async/)

## 验证情况

- 环境：Windows 11 24H2（26100.6584）、Python 3.14.7、httpx 0.28.1
- 验证日期：2026-11-01
- 已完成：本人在上述环境按本文档完整执行一遍；并在全新虚拟环境中验证了安装流程
- 未完成：尚无其他读者独立试用
- 已知限制：仅在 Windows + PowerShell 下验证；Python 3.8–3.13 未测试
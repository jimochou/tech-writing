PS C:\Users\lilli\Documents\tech-writing> git --version
git : 无法将“git”项识别为 cmdlet、函数、脚本文件或可运行程序的名称。请检查名称的拼写，如果包括路径，请确保路径正确，
然后再试一次。
所在位置 行:1 字符: 1
+ git --version
+ ~~~
    + CategoryInfo          : ObjectNotFound: (git:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException

PS C:\Users\lilli\Documents\tech-writing> Get-Command git -All
Get-Command : 无法将“git”项识别为 cmdlet、函数、脚本文件或可运行程序的名称。请检查名称的拼写，如果包括路径，请确保路
径正确，然后再试一次。
所在位置 行:1 字符: 1
+ Get-Command git -All
+ ~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (git:String) [Get-Command], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException,Microsoft.PowerShell.Commands.GetCommandCommand

PS C:\Users\lilli\Documents\tech-writing>

PS C:\Users\lilli\Documents\tech-writing> git --version
git version 2.56.0.windows.3
PS C:\Users\lilli\Documents\tech-writing> (Get-Command git).Source
C:\Program Files\Git\cmd\git.exe
PS C:\Users\lilli\Documents\tech-writing>

git config --global user.name "allen"
git config --global user.email "2667330167@qq.com"
git config --global init.defaultBranch main



PS C:\Users\lilli\Documents\tech-writing> git --version
git version 2.56.0.windows.3
PS C:\Users\lilli\Documents\tech-writing> (Get-Command git).Source
C:\Program Files\Git\cmd\git.exe
PS C:\Users\lilli\Documents\tech-writing> git config --global user.name "allen"
PS C:\Users\lilli\Documents\tech-writing> git config --global user.email "2667330167@qq.com"
PS C:\Users\lilli\Documents\tech-writing> git config --global init.defaultBranch main
PS C:\Users\lilli\Documents\tech-writing> git config --global --list
core.editor="C:\Users\lilli\AppData\Local\Programs\Microsoft VS Code\bin\code" --wait
user.name=allen
user.email=2667330167@qq.com
init.defaultbranch=main
PS C:\Users\lilli\Documents\tech-writing>



git config --global core.quotepath false 中文
PS C:\Users\lilli\Documents\tech-writing> git config --global user.email "l13991911659@gmail.com"
PS C:\Users\lilli\Documents\tech-writing> git commit --amend --no-edit --reset-author
[main 105116b] docs: 添加环境盘点与排查记录
 2 files changed, 194 insertions(+)
 create mode 100644 raw-log.md
 create mode 100644 环境盘点.md
PS C:\Users\lilli\Documents\tech-writing>

一个代理配置小脚本：$reg = Get-ItemProperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings'
$proxy = $reg.ProxyServer
if ($proxy) {
    git config --global http.proxy "http://$proxy"
    git config --global https.proxy "http://$proxy"
    Write-Host "Git 代理已设为 http://$proxy"
} else {
    Write-Host "未检测到系统代理，可能梯子没开"
}
换代理
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890

2026-10-26 看着自己的仓库，有点兴奋。

确认自己在哪：git rev-parse --show-toplevel


## 2026-10-29 httpx 安装流程复现
环境：Windows 11 24H2 / Python 3.14.7
目的：重新走一遍安装流程，作为 Quickstart 的素材

1. 装了什么

```powershell
PS C:\Users\lilli\Documents\tech-writing> python --version
Python 3.14.7
PS C:\Users\lilli\Documents\tech-writing> python -m pip show httpx
Name: httpx
Version: 0.28.1
Summary: The next generation HTTP client.
Home-page: https://github.com/encode/httpx
Author:
Author-email: Tom Christie <tom@tomchristie.com>
License: BSD-3-Clause
Location: C:\Users\lilli\AppData\Local\Programs\Python\Python314\Lib\site-packages
Requires: anyio, certifi, httpcore, idna
Required-by:
PS C:\Users\lilli\Documents\tech-writing>
```

2. 卸载并重装 httpx

```powershell

PS C:\Users\lilli\Documents\tech-writing> python -m pip uninstall httpx
Found existing installation: httpx 0.28.1
Uninstalling httpx-0.28.1:
  Would remove:
    c:\users\lilli\appdata\local\programs\python\python314\lib\site-packages\httpx-0.28.1.dist-info\*
    c:\users\lilli\appdata\local\programs\python\python314\lib\site-packages\httpx\*
    c:\users\lilli\appdata\local\programs\python\python314\scripts\httpx.exe
Proceed (Y/n)? y
  Successfully uninstalled httpx-0.28.1

PS C:\Users\lilli\Documents\tech-writing> python -m pip list | Select-String "httpcore|anyio|certifi"

anyio             4.15.1
certifi           2026.10.23
httpcore          1.0.9


PS C:\Users\lilli\Documents\tech-writing> python -c "import httpx"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import httpx
ModuleNotFoundError: No module named 'httpx'
PS C:\Users\lilli\Documents\tech-writing> python -m pip install httpx
Collecting httpx
  Using cached httpx-0.28.1-py3-none-any.whl.metadata (7.1 kB)
Requirement already satisfied: anyio in C:\Users\lilli\AppData\Local\Programs\Python\Python314\Lib\site-packages (from httpx) (4.15.1)
Requirement already satisfied: certifi in C:\Users\lilli\AppData\Local\Programs\Python\Python314\Lib\site-packages (from httpx) (2026.10.23)
Requirement already satisfied: httpcore==1.* in C:\Users\lilli\AppData\Local\Programs\Python\Python314\Lib\site-packages (from httpx) (1.0.9)
Requirement already satisfied: idna in C:\Users\lilli\AppData\Local\Programs\Python\Python314\Lib\site-packages (from httpx) (3.20)
Requirement already satisfied: h11>=0.16 in C:\Users\lilli\AppData\Local\Programs\Python\Python314\Lib\site-packages (from httpcore==1.*->httpx) (0.16.0)
Requirement already satisfied: typing_extensions>=4.16.0 in C:\Users\lilli\AppData\Local\Programs\Python\Python314\Lib\site-packages (from anyio->httpx) (4.16.0)
Using cached httpx-0.28.1-py3-none-any.whl (73 kB)
Installing collected packages: httpx
Successfully installed httpx-0.28.1

```
3. 验证

```powershell

PS C:\Users\lilli\Documents\tech-writing> python -c "import httpx; print(httpx.__version__)"
0.28.1
PS C:\Users\lilli\Documents\tech-writing> python -c "import httpx; r = httpx.get('https://example.com'); print(r)"
<Response [200 OK]>

```
PS C:\Users\lilli\Documents\tech-writing> httpx --version
The httpx command line client could not run because the required dependencies were not installed.
Make sure you've installed everything with: pip install 'httpx[cli]'

httpx.exe 随基础包安装，但命令行功能需要 [cli] 额外依赖；缺依赖时给出明确修复命令。



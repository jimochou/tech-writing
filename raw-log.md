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
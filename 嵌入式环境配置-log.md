# 嵌入式环境配置记录

## 基本信息
- 教材：《ARM Cortex-M4 嵌入式系统原理及应用》，第 3 章
- 开发板：STM32F407（主控芯片型号）
- 目标：能编译并下载一个点灯程序
- 系统：Windows 11 24H2

## 要装的软件
- [jre ] ___
- [ ] ___
- [ ] ___

## 过程记录

### JRE安装

[JRE安装](https://www.java.com/en/download/manual.jsp)
jre-8u511-windows-x64Java 环境：本机已有 JDK 19（PATH 优先）；另装 JRE 1.8.0_511
（C:\Program Files\Java\jre1.8.0_511）供 CubeMX 使用。
书上说：安装 JRE 8 后继续
我遇到：java -version 显示 19.0.2（本机已有 JDK 19，PATH 优先）
判断：JRE 8 装好了，位置 C:\Program Files\Java\jre1.8.0_511，
     但 PATH 里的 java 指向旧的那套
处理：暂不改 PATH，继续装 CubeMX，看它能否自己找到 JRE 8

环境提示：本机已有 Java 19.0.2（PATH 中优先），另装了 JRE 1.8.0_511 供 CubeMX 使用。
CubeMX 通常不用 PATH 里的 java——它会自己找已安装的 JRE，或者读 JAVA_HOME。

所以：先别动 PATH，继续装 CubeMX。

Get-ChildItem "C:\Program Files\Java", "C:\Program Files (x86)\Java" -ErrorAction SilentlyContinue |
  Select-Object -ExpandProperty FullName

  C:\Program Files\Java\jre1.8.0_511

### STM32CubeMX安装
[安装地址](https://www.st.com/en/development-tools/stm32cubemx.html#section-get-software-table)
需要注册st账号，注册按网站步骤，本文档不再赘述。
注册后下载第一个STM32CubeMX
1. 安装模式：Install for all users（管理员权限）
安装路径：D:\ST\STM32CubeMX
版本：6.18.1
2.
首次启动反复弹出 "The Updater is already in use"，
原因是 CubeMX 启动时自动检查更新，网络/代理导致超时卡住。
处理：关闭代理（或断网）后重启 CubeMX，或关闭启动时自动检查。

3.
| 设置项 | 默认值 | 改为 | 原因 |
|---|---|---|---|
| Repository Folder | C:/Users/<user>/STM32Cube/Repository | D:/STM32Cube/Repository | 固件包几 GB，避免占满 C 盘 |
| Check and Update Settings | Automatic（5 天） | Manual | 网络不佳时启动反复弹 Updater 窗口 |
| Data Auto-Refresh | Data-only at startup | No Auto-Refresh | 启动时联网拉数据，慢且易卡 |

STM32CubeF4 V1.28.3

### MDK-ARM下载

[地址](https://www.keil.com/demo/eval/arm.htm)
填写信息即可安装
### 固件包
[地址](https://www.keil.arm.com/packs/stm32f4xx_dfp-keil/overview/)
#### 下载STM32CubeMX安装时反复跳回登录页，或下载中途中断

现象：点击 Download 后，页面不断跳回登录页；或者下载到一半断开。

原因：ST 官网在国内访问不稳定，登录会话和下载链路容易中断。
这不是账号问题，也不是操作错误。

处理（按顺序试）：
1. 换用手机热点
2. 如果你有可用的网络代理，开启后再访问——登录态更容易保持
3. 换时段重试（夜间通常更快）
4. 已登录仍反复跳转时，退出重登，或换个浏览器

### 点 
1. java -version 返回 19.0.2（本机已有 JDK 19，PATH 优先）
2. 安装路径避 Program Files（空格），统一装 D 盘（C 盘只剩 43 GB）
3. ST 官网下载反复跳登录页 → 换网络/代理
4. CubeMX 首次启动反复弹 Updater 冲突
5. Help → Updater Settings 在 6.18 里叫 Connection & Updates
6. 固件包仓库默认在 C 盘 → 改到 D:\STM32Cube\Repository
7. Keil 安装时弹 ULINK 驱动提示 → 选安装
8. Keil 的 Pack 路径单独选 D:\ARM\Packs（卸载本体不丢包）

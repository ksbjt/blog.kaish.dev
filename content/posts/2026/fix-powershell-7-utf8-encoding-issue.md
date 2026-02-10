---
date: '2026-02-10T22:25:01+08:00'
draft: false
title: '修改 Powershell 7 默认编码为 UTF-8'
tags:
  - windows
  - ai coding
---

- 此教程是为了解决在使用 AI Coding 时候, 读取项目文件提示代码乱码问题.
- 以下内容只修改 <u>pwsh</u> 默认编码问题, Vscode,  Pycharm 等 IDE.
- 需要手动修改默认 <u>powershell</u> 为 **7.x** 版本,  否则默认还是 **5.x** 版本.

### 安装 Powershell 7 

1. 查看当前 Powershell 版本

``` powershell
$PSVersionTable
```

输出如下

``` powershell
PSVersion 7.5.4
```

#### 如果使用的是 <u>Powershell 5.x</u> 建议升级到 <u>Powershell 7.x</u>, 对 UTF-8 支持会更好.

2. 搜索最新版本的 PowerShell

``` powershell
winget search --id Microsoft.PowerShell
```

输出如下

``` powershell
Name               Id                           Version Source
---------------------------------------------------------------
PowerShell         Microsoft.PowerShell         7.5.4.0 winget
PowerShell Preview Microsoft.PowerShell.Preview 7.6.0.6 winget
```

3. 使用 `--id` 参数安装 PowerShell 或 PowerShell 预览版

``` powershell
winget install --id Microsoft.PowerShell --source winget
```

``` powershell
winget install --id Microsoft.PowerShell.Preview --source winget
```

4. 打开 <u>pwsh</u> 查看默认编码, 注意 `powershell` 指令默认打开为 **5.x** 版本, `pwsh` 打开为 **7.x** 版本

 ``` powershell
 chcp
 ```

如果输出不是为 <u>65001</u> 就不是 UTF-8 编码

```powershell
Active code page: 65001
```

#### 永久生效

1. 打开 <u>pwsh</u> 终端运行

```powershell
$PROFILE
```

输出如下, 如果不是以下路径就说明打开的不是 <u>Powershell 7</u>

``` powershell
C:\Users\你的用户名\Documents\PowerShell\Microsoft.PowerShell_profile.ps1
```

2. 然后编辑文件, 没有就创建添加以下内容

``` powershell
# ==============================
# Force UTF-8 everywhere
# ==============================

$utf8 = [System.Text.UTF8Encoding]::new($false)  # UTF-8 without BOM

[Console]::InputEncoding  = $utf8
[Console]::OutputEncoding = $utf8

# PowerShell cmdlets default encoding
$PSDefaultParameterValues['*:Encoding'] = 'utf8'

# Try to align Windows console code page
try {
    chcp 65001 | Out-Null
} catch {}
```

3. 然后重新打开 <u>pwsh</u> 检查默认编码

``` powershell
chcp
```

输出如下

```
Active code page: 65001
```

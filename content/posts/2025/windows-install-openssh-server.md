---
date: '2025-12-03T12:27:59+08:00'
draft: false
title: 'Windows 安装 SSH 服务端'
tags:
  - windows
---

### 安装

---

1. 下载 [OpenSSH](https://github.com/PowerShell/Win32-OpenSSH)

2. 以管理员身份打开 PowerShell 并进入 OpenSSH 目录

3. 允许 PowerShell 脚本执行

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
```

4. 运行安装脚本, 注册 SSH 服务端.

```powershell
.\install-sshd.ps1
```

5. 启动服务

```powershell
Start-Service sshd
```

6. 开机自启

```powershell
Set-Service sshd -StartupType Automatic
```

7. 查看服务

```powershell
Get-Service sshd
```

8. 放行端口

```powershell
New-NetFirewallRule -Name sshd -DisplayName "OpenSSH Port 22" `
 -Protocol TCP -Direction Inbound -LocalPort 22 -Action Allow
```

9. 测试, 必须要有密码, Windows 不允许空密码

```powershell
ssh Administrator@IP
```

### 卸载

---

1. 停止服务

```powershell
Stop-Service sshd
Stop-Service ssh-agent
```

2. 卸载服务

```powershell
.\uninstall-sshd.ps1
```

3. 删除 OpenSSH 文件夹
4. 检查

```powershell
Get-Service sshd
```

返回以下内容

```powershell
Cannot find any service with service name 'sshd'
```

5. 删除防火墙规则（可选）

```powershell
Remove-NetFirewallRule -Name sshd
```

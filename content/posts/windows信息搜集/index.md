---
title: "Windows信息搜集"
date: 2023-10-30T18:36:52+08:00
draft: true
tags: ["windows"]
categories: ["Note"]
description: "windows note"
lightgallery: true
---
Windows信息搜集

<!--more-->

## 本机信息

### 查看网络配置
```
ipconfig /all
```
### 查看系统信息
```
systeminfo
```
### 查看系统架构
```
echo %PROCESSOR_ARCHITECTURE%
```

### 查询已安装的软件及版本信息

```
wmic product get name,version
- `Caption`：获取软件的标题。
- `Version`：获取软件的版本号。
- `Vendor`：获取软件的发布者。
- `InstallDate`：获取软件的安装日期。
- `Description`：获取软件的描述信息。
- `IdentifyingNumber`：获取软件的唯一标识号。
- `InstallLocation`：获取软件的安装路径。
- `URLInfoAbout`：获取软件的官方网站地址。

powershell
Get-WmiObject -Class Win32_Product | Select-Object Name, Vendor, InstallDate
```


### 查看端口
```
netstat -ano
```


### 查看服务

```
wmic service list brief
wmic service get DisplayName,Name,State,StartMode,Displayname,pathname

Name：服务的名称。
DisplayName：服务的显示名称。
State：服务的状态，例如 "Running"（正在运行）或 "Stopped"（已停止）。
StartMode：服务的启动模式，例如 "Auto"（自动启动）或 "Manual"（手动启动）。
ProcessId：服务的进程 ID。
ExitCode：服务的退出代码。
PathName：服务的可执行文件路径。
ServiceType：服务的类型，例如 "Win32ShareProcess" 或 "Win32OwnProcess"。
```

### 查看进程
```
wmic process list brief

tasklist
```

### 查看启动项的程序信息
```
wmic startup get command,caption

Description：启动项的描述。
Location：启动项的位置。
User：启动项所属的用户。
SettingID：启动项的设置 ID。
Status：启动项的状态。
Caption：启动项的标题。
Command：启动项的命令。
UserSID：启动项所属用户的安全标识符。
```

### 查看计划任务详细信息
```
schtasks /query /v /fo list
```

### 查看主机的开机时间
```
net statistics workstation

systeminfo
```

### 查看补丁信息
```
wmic qfe get caption,description,hotfixid,installedon

systeminfo
```


### 查看用户
```
net user
```


### 查看本地管理员信息
```
net localgroup administrators
```

### 查看当前在线的用户
```
quser

qwinsta
```

### 查看本机共享列表
```
wmic share get name,path,status
net share
```

### 查询路由表及所有可用的ARP缓存表
```
route print
arp -a
```

### 查看防火墙配置
```
netsh advfirewall show allprofiles    显示防火墙详细信息
netsh advfirewall show allprofile state    显示防火墙状态信息
```

### 查看internet代理
```
reg query "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
```

### wmic查询注册表
```
https://3gstudent.github.io/%E6%B8%97%E9%80%8F%E5%9F%BA%E7%A1%80-WMIC%E7%9A%84%E4%BD%BF%E7%94%A8
(1)获得当前用户的远程桌面连接历史记录
枚举注册表键值HKCU:\Software\Microsoft\Terminal Server Client\Servers，命令如下：


wmic /namespace:"\\root\cimv2" path stdregprov call EnumKey ^&h80000001,"Software\Microsoft\Terminal Server Client\Servers"

defender 

wmic /namespace:"\\root\default" path StdRegProv call EnumValues ^&h80000002,"SOFTWARE\Microsoft\Windows Defender\Exclusions\Paths"

```

### powershell查询注册表
```
powershell -command "Get-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows Defender\Exclusions\Paths'"
```

---

## 域内信息

### 获取本机域内用户的详细信息
```
net user xxx /domain
```

### 判断是否存在域
```
查看dns信息是否为域名
systeminfo 
ipconfig /all
发现域名后，利用nslookup命令直接解析域名的ip，借此来判断dns服务器与域控是否在同一主机上，及是否为同一内网中

查看登录域
net config workstation
```

### 查询当前的登录域与用户信息 
```
net config workstation
```

### 判断主域
```
net time /domain

若是此命令在显示域处显示WORKGROUP，则不存在域，若是报错：发生系统错误5，则存在域，但该用户不是域用户

```

### 获取域密码信息
```
net accounts /domain
```

### 查看所有域成员计算机列表
```
net group "domain computers" /domain
```

### 查询域内所有用户组
```
net group /domain
```

### 查询域内所有计算机
```
net view /domain:域名
```

### 获取域信任信息
```
nltest /domain_trusts
```

### 查看所有域控机器名
```
nltest /DCLIST:域名
```

### 查看域控制器组
```
net group "Domain Controllers" /domain

```


### 查看所有域用户列表
```
shell net user /domain
```

### 获取域内用户的详细信息
```
wmic useraccount get /all
```

### 查询域管理员组用户
```
net group "domain admins" /domain
```

### 查询管理员用户组
```
net group "Enterprise Admins" /domain

```

### 收集所有活动域的会话列表
```
NetSess -h，使用的是NetSess.exe
```


### 获取域信任信息
```
nltest /domain_trusts
```
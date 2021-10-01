---
title: Synchronization of Windows Clocks
excerpt: Synchronization of Windows Clocks
date: 2020-03-14 19:01:21
tags:
  - Cloud
  - Windows
categories: [Operating system, Windows]
---

# Synchronization of Windows Clocks


Windows 可以使用 NTP 作为时间输入源，运行在虚拟机上时还可以使用 VMIC (Virtual Machine Integration Components) 时间输入源。
从 Windows 10 1809 或 Windows Server 2019 开始，可以使用 PTP 作为时间输入源。

- PTP: %SystemRoot%\System32\ptpprov.dll
- NTP: %SystemRoot%\System32\w32time.dll
- VMIC: %SystemRoot%\System32\vmictimeprovider.dll

```cmd
C:\>reg query HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\PtpClient
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider
```

## Initialization

当 Windows Time Service 不能正常工作时，可以重新注册这个服务。


```cmd
net stop w32time
W32tm /unregister
W32tm /register
net start w32time
```

## Configuration

- [Windows Time service tools and settings](https://docs.microsoft.com/en-us/windows-server/networking/windows-time-service/windows-time-service-tools-and-settings)

### NTP

常用的 Windows Time Service 的注册表配置项如下：
```cmd
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config /t REG_DWORD /v AnnounceFlags /d 10 /f
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config /t REG_DWORD /v EventLogFlags /d 3 /f
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config /t REG_DWORD /v MinPollInterval /d 4 /f
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config /t REG_DWORD /v MaxPollInterval /d 6 /f

reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters /t REG_SZ /v Type /d NTP /f
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters /t REG_SZ /v NtpServer /d "time.windows.com,0x9 de.pool.ntp.org,0x9 us.pool.ntp.org,0x9 jp.pool.ntp.org,0x9" /f

reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient /t REG_DWORD /v Enabled /d 1 /f
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient /t REG_EXPAND_SZ /v DllName /d "%SystemRoot%\System32\w32time.dll" /f
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient /t REG_EXPAND_SZ /v DllName /d "C:\WINDOWS\SYSTEM32\w32time.DLL" /f
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient /t REG_DWORD /v EventLogFlags /d 3 /f
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient /t REG_DWORD /v InputProvider /d 1 /f
```

也可以这样配置 NTP 服务器：

```cmd
w32tm /config /manualpeerlist:"time.windows.com,0x9 de.pool.ntp.org,0x9 us.pool.ntp.org,0x9 jp.pool.ntp.org,0x9" /syncfromflags:manual /update
net stop w32time && net start w32time
```

### PTP

```cmd
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\PtpClient /t REG_SZ /v PtpMasters /d "10.10.10.10 10.10.10.11" /f
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\PtpClient /t REG_DWORD /v Enabled /d 1 /f
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\PtpClient /t REG_DWORD /v InputProvider /d 1 /f
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\PtpClient /t REG_EXPAND_SZ /v DllName /d "%SystemRoot%\System32\ptpprov.dll" /f
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\PtpClient /t REG_DWORD /v DelayPollInterval /d 0x3e80 /f
reg add HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\PtpClient /t REG_DWORD /v AnnounceInterval /d 0x0fa0 /f
```

## Workaround of VM time out of sync

```PowerShell
Get-VMIntegrationService -VMName DockerDesktopVM -Name "Time Synchronization" | Disable-VMIntegrationService
Get-VMIntegrationService -VMName DockerDesktopVM -Name "Time Synchronization" | Enable-VMIntegrationService
```

## Tools

### Query

```cmd
w32tm /query /configuration
w32tm /query /status /verbose
w32tm /query /source
w32tm /query /peers
```

### Resynchronization

```cmd
w32tm /resync
```

### Offset measurement

```cmd
w32tm /stripchart /rdtsc /computer:0.pool.ntp.org /samples:5
w32tm /stripchart /rdtsc /computer:time.windows.com /samples:5
```

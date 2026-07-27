# Windows 全量卸载 SOLIDWORKS、CATIA 与 3DEXPERIENCE 实战记录

> **执行日期：** 2026-07-26  
> **适用范围：** SOLIDWORKS、SOLIDWORKS CAM、Visualize、Composer、eDrawings、CATIA、3DEXPERIENCE 以及 Dassault Systèmes 相关组件  
> **文档目的：** 记录一次“程序目录曾被手动删除、控制面板找不到卸载入口、Windows Installer 登记仍然存在”的完整修复与全量卸载过程。  
> **最终结果：** 安装登记、进程、服务、计划任务、厂商注册表、环境变量和已知残留目录均清理为 0。

---

## 目录

- [1. 背景与问题现象](#1-背景与问题现象)
- [2. Windows 软件卸载的底层结构](#2-windows-软件卸载的底层结构)
- [3. 本次处理原则](#3-本次处理原则)
- [4. 第一阶段：只读审计](#4-第一阶段只读审计)
- [5. 第二阶段：确认程序本体与卸载登记状态](#5-第二阶段确认程序本体与卸载登记状态)
- [6. 第三阶段：使用 Windows Installer 卸载主程序](#6-第三阶段使用-windows-installer-卸载主程序)
- [7. 第四阶段：批量卸载其余 SOLIDWORKS 组件](#7-第四阶段批量卸载其余-solidworks-组件)
- [8. 第五阶段：处理 CAM 的 1603 错误](#8-第五阶段处理-cam-的-1603-错误)
- [9. 第六阶段：卸载 Dassault 前置组件](#9-第六阶段卸载-dassault-前置组件)
- [10. 第七阶段：删除孤立服务](#10-第七阶段删除孤立服务)
- [11. 第八阶段：隔离并清理残留目录与注册表](#11-第八阶段隔离并清理残留目录与注册表)
- [12. 第九阶段：重启后的最终验证](#12-第九阶段重启后的最终验证)
- [13. 第十阶段：永久删除隔离目录](#13-第十阶段永久删除隔离目录)
- [14. 最终结果](#14-最终结果)
- [15. 可复用的软件卸载标准流程](#15-可复用的软件卸载标准流程)
- [16. 常见错误与经验总结](#16-常见错误与经验总结)
- [17. 附录：本次涉及的 ProductCode](#17-附录本次涉及的-productcode)
- [18. 附录：常见 MSI 返回码](#18-附录常见-msi-返回码)

---

# 1. 背景与问题现象

此前曾直接删除部分 SOLIDWORKS、CATIA 或 3DEXPERIENCE 文件夹，导致系统进入不一致状态：

- 控制面板“程序和功能”中看不到 SOLIDWORKS；
- 原安装目录已经不存在；
- Windows Installer 仍认为部分组件处于安装状态；
- 某些组件无法重新安装或正常卸载；
- 注册表中仍有卸载登记；
- Windows 服务仍指向已经不存在的可执行文件；
- AppData、ProgramData 和公共文档中仍有残留。

这种状态不能简单理解为“软件已经删完”。

Windows 中一套大型软件通常至少包含：

```text
程序文件
+ Windows Installer 登记
+ 卸载注册表
+ 服务
+ 计划任务
+ 环境变量
+ 共享组件
+ ProgramData 缓存
+ AppData 用户配置
+ 许可证组件
+ 用户工程文件
```

只删除 `Program Files` 或 D 盘安装目录，只是删除了其中一部分。

---

# 2. Windows 软件卸载的底层结构

## 2.1 卸载登记

Windows 通常从以下注册表位置读取传统桌面软件的卸载信息：

```text
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall
HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
```

重要字段包括：

```text
DisplayName
DisplayVersion
Publisher
InstallLocation
UninstallString
WindowsInstaller
SystemComponent
```

## 2.2 MSI ProductCode

对于使用 Windows Installer 的软件，常见卸载命令为：

```text
MsiExec.exe /X{GUID}
```

其中 `{GUID}` 是该产品版本的 `ProductCode`。

例如：

```text
{DB2C3F1B-3025-4743-AAA8-1B5E20047E34}
```

在本次案例中对应：

```text
SOLIDWORKS 2025 SP0
```

> [!WARNING]
> ProductCode 与产品、版本、语言和组件相关。不能从网上随便复制相似版本的 GUID，也不能把某个注册表键名一律当作 MSI ProductCode。

## 2.3 为什么控制面板里看不到软件

本次多个 SOLIDWORKS 组件具有：

```text
SystemComponent = 1
```

这表示该卸载登记会被隐藏，不在“程序和功能”中正常显示。

因此：

```text
控制面板里看不到
```

并不等于：

```text
Windows Installer 中没有登记
```

## 2.4 EstimatedSize 不等于真实占用

卸载登记中的 `EstimatedSize` 只是安装器写入的估算值。

当程序目录已经被手动删除时，注册表仍可能显示数 GB，但并不代表这些文件仍真实占用磁盘。

---

# 3. 本次处理原则

整个过程遵守以下顺序：

```text
先审计
→ 再正常卸载
→ 再用 ProductCode 卸载
→ 再修复损坏登记
→ 再处理服务
→ 再检查个人文件
→ 再隔离目录
→ 再备份并清理注册表
→ 重启验证
→ 最后永久删除
```

明确禁止：

```text
直接删除 C:\Windows\Installer
直接删除 C:\Windows\WinSxS
直接删除未知服务
看到 GUID 就随便执行 msiexec /x
先删注册表再尝试正常卸载
同时批量修改多个软件家族
未检查工程文件就删除公共文档目录
```

---

# 4. 第一阶段：只读审计

以管理员身份打开 Windows PowerShell。

## 4.1 扫描关键词

```powershell
$rx = '(?i)SOLIDWORKS|Dassault|CATIA|3DEXPERIENCE|3D\s*EXPERIENCE|ENOVIA|DELMIA|SIMULIA|DraftSight|eDrawings|Composer|Visualize|SolidNetWork|DS\s*License|DSLS'
```

## 4.2 扫描已安装程序登记

```powershell
$uninstallPaths = @(
    'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*',
    'HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*',
    'HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*'
)

$apps = Get-ItemProperty $uninstallPaths -ErrorAction SilentlyContinue |
Where-Object {
    "$($_.DisplayName) $($_.Publisher) $($_.InstallLocation)" -match $rx
} |
Select-Object `
    DisplayName,
    DisplayVersion,
    Publisher,
    EstimatedSize,
    InstallLocation,
    PSChildName,
    UninstallString,
    QuietUninstallString,
    SystemComponent,
    WindowsInstaller,
    PSPath |
Sort-Object DisplayName
```

## 4.3 扫描服务、进程与计划任务

```powershell
$services = Get-CimInstance Win32_Service |
Where-Object {
    "$($_.Name) $($_.DisplayName) $($_.PathName)" -match $rx
} |
Select-Object State, StartMode, Name, DisplayName, PathName

$processes = Get-CimInstance Win32_Process |
Where-Object {
    "$($_.Name) $($_.ExecutablePath) $($_.CommandLine)" -match $rx
} |
Select-Object ProcessId, Name, ExecutablePath, CommandLine

$tasks = @(
    foreach ($task in Get-ScheduledTask) {
        $actions = (
            $task.Actions |
            ForEach-Object {
                "$($_.Execute) $($_.Arguments)"
            }
        ) -join ' | '

        if ("$($task.TaskName) $($task.TaskPath) $actions" -match $rx) {
            [PSCustomObject]@{
                TaskPath = $task.TaskPath
                TaskName = $task.TaskName
                State    = $task.State
                Actions  = $actions
            }
        }
    }
)
```

## 4.4 本次审计发现

发现 12 项相关安装登记：

| 组件 | 状态 |
|---|---|
| SOLIDWORKS 2025 SP0 | 登记存在，安装目录不存在 |
| SOLIDWORKS CAM 2025 SP0 | 登记存在，安装目录不存在 |
| SOLIDWORKS Visualize 2025 SP0 | 登记存在，安装目录不存在 |
| SOLIDWORKS Composer Player 2025 SP0 | 登记存在 |
| SOLIDWORKS eDrawings 2025 SP0 | 登记存在 |
| SOLIDWORKS File Utilities 2025 SP0 | 登记存在 |
| SOLIDWORKS 2025 简体中文资源 | 登记存在 |
| CEF for SOLIDWORKS Applications | 登记存在 |
| Dassault Systèmes VBA 7.1 | 登记存在 |
| Dassault VC10 Prerequisites | 登记存在 |
| Dassault VC11 Prerequisites | 登记存在 |
| Dassault VC12 Prerequisites | 登记存在 |

还发现相关服务：

```text
DTSInterops
SolidWorks Flexnet Server
SolidWorks Licensing Service
SWVisualize2025.Queue.Server
```

---

# 5. 第二阶段：确认程序本体与卸载登记状态

## 5.1 检查安装目录

```powershell
$targets = @(
    'D:\SOLIDWORKS Corp',
    'D:\SOLIDWORKS Corp\SOLIDWORKS\SLDWORKS.exe',
    'D:\SOLIDWORKS Corp\SOLIDWORKS CAM',
    'D:\SOLIDWORKS Corp\SOLIDWORKS Visualize',
    'D:\SOLIDWORKS Corp\eDrawings',
    'C:\Program Files\SOLIDWORKS Corp',
    'C:\SolidWorks_Flexnet_Server',
    'C:\Program Files\Dassault Systemes'
)

$targets |
ForEach-Object {
    [PSCustomObject]@{
        Exists = Test-Path $_
        Path   = $_
    }
} |
Format-Table -AutoSize
```

本次结果表明 SOLIDWORKS 主程序文件已被删除，但安装登记仍存在。

## 5.2 查看隐藏卸载登记

```powershell
Get-ItemProperty $uninstallPaths -ErrorAction SilentlyContinue |
Where-Object {
    "$($_.DisplayName) $($_.Publisher)" -match
    'SOLIDWORKS|Dassault|CATIA|3DEXPERIENCE'
} |
Select-Object `
    DisplayName,
    DisplayVersion,
    SystemComponent,
    WindowsInstaller,
    InstallLocation,
    UninstallString,
    PSPath |
Sort-Object DisplayName |
Format-List
```

多个组件显示：

```text
SystemComponent : 1
WindowsInstaller : 1
```

这解释了为什么控制面板中看不到这些组件。

---

# 6. 第三阶段：使用 Windows Installer 卸载主程序

```powershell
$ErrorActionPreference = 'Stop'

$log = "$env:USERPROFILE\Desktop\SW2025_main_uninstall.log"

$msiArguments = "/x {DB2C3F1B-3025-4743-AAA8-1B5E20047E34} /passive /norestart /L*v `"$log`""

$process = Start-Process `
    -FilePath "$env:SystemRoot\System32\msiexec.exe" `
    -ArgumentList $msiArguments `
    -Wait `
    -PassThru

Write-Host "卸载返回码：$($process.ExitCode)"
Write-Host "日志位置：$log"
```

本次返回：

```text
0
```

说明 SOLIDWORKS 2025 SP0 已由 Windows Installer 正常移除。

## 一个实际踩坑：不要使用 `$args`

曾错误使用：

```powershell
$args = '/x ...'
```

`$args` 是 PowerShell 自动变量，不适合作为自定义参数变量，可能导致：

```text
Cannot validate argument on parameter 'ArgumentList'
```

正确做法是使用：

```powershell
$msiArguments
```

---

# 7. 第四阶段：批量卸载其余 SOLIDWORKS 组件

## 7.1 Visualize 与 CAM

```powershell
$products = @(
    [PSCustomObject]@{
        Name = 'SOLIDWORKS Visualize 2025 SP0'
        Code = '{5A836655-81D9-4702-92BC-0C2DD424BF41}'
    },
    [PSCustomObject]@{
        Name = 'SOLIDWORKS CAM 2025 SP0'
        Code = '{B56E1B6B-5ADA-4E85-B1C1-D8DE719A24AE}'
    }
)

foreach ($item in $products) {
    $logPath = "$env:USERPROFILE\Desktop\$($item.Name -replace ' ','_').log"
    $arguments = "/x $($item.Code) /passive /norestart /L*v `"$logPath`""

    $process = Start-Process `
        -FilePath "$env:SystemRoot\System32\msiexec.exe" `
        -ArgumentList $arguments `
        -Wait `
        -PassThru

    Write-Host "$($item.Name)：$($process.ExitCode)"
}
```

结果：

```text
SOLIDWORKS Visualize 2025 SP0    0
SOLIDWORKS CAM 2025 SP0          1603
```

## 7.2 绕过 CAM，继续卸载其他组件

```powershell
$remainingProducts = @(
    [PSCustomObject]@{
        Name = 'SOLIDWORKS Composer Player 2025 SP0'
        Code = '{D28B5B78-D35E-4463-B211-CFAE2A19327A}'
    },
    [PSCustomObject]@{
        Name = 'SOLIDWORKS eDrawings 2025 SP0'
        Code = '{7D7F080A-418C-4366-A783-EEB48737914F}'
    },
    [PSCustomObject]@{
        Name = 'SOLIDWORKS File Utilities 2025 SP0'
        Code = '{3D5985E3-CBCB-42E6-9B79-00CE9B183015}'
    },
    [PSCustomObject]@{
        Name = 'SOLIDWORKS 2025 Chinese Simplified Resources'
        Code = '{CA4C86CC-6E7D-4A70-A51B-0FDE437D989E}'
    },
    [PSCustomObject]@{
        Name = 'CEF for SOLIDWORKS Applications'
        Code = '{D2AD4CE7-115F-47BD-93D8-725570B53346}'
    }
)

foreach ($item in $remainingProducts) {
    $arguments = "/x $($item.Code) /passive /norestart"

    $process = Start-Process `
        -FilePath "$env:SystemRoot\System32\msiexec.exe" `
        -ArgumentList $arguments `
        -Wait `
        -PassThru

    Write-Host "$($item.Name)：$($process.ExitCode)"
}
```

本次全部返回 `0`。

---

# 8. 第五阶段：处理 CAM 的 1603 错误

## 8.1 查看 MSI 日志

```powershell
$camLog = "$env:USERPROFILE\Desktop\Dassault_Uninstall_Logs\SOLIDWORKS_CAM_2025_SP0.log"

Select-String `
    -Path $camLog `
    -Pattern 'Return value 3|Error 1603|failed|failure|cannot|not found' `
    -Context 20,10 |
Select-Object -Last 20 |
Format-List
```

日志中出现了序列号读取失败，但 `1603` 是通用严重错误，不能只根据单行日志确定唯一根因。

结合“安装目录不存在、MSI 登记仍存在、正常卸载返回 1603”，决定使用微软官方安装/卸载疑难解答处理损坏登记。

## 8.2 微软官方工具

微软官方说明页：

[Fix problems that block programs from being installed or removed](https://support.microsoft.com/en-us/windows/deployment/install-upgrade/fix-problems-that-block-programs-from-being-installed-or-removed)

工具直接下载：

[Microsoft Program Install and Uninstall troubleshooter](https://download.microsoft.com/download/7/E/9/7E9188C0-2511-4B01-8B4E-0A641EC2F600/MicrosoftProgram_Install_and_Uninstall.meta.diagcab)

工具文件名：

```text
MicrosoftProgram_Install_and_Uninstall.meta.diagcab
```

操作步骤：

```text
打开工具
→ 选择“正在卸载”
→ 选择 SOLIDWORKS CAM 2025 SP0
```

若列表中没有该软件：

```text
选择“未列出”
→ 输入准确的 ProductCode
```

本次输入：

```text
{B56E1B6B-5ADA-4E85-B1C1-D8DE719A24AE}
```

工具最终显示问题已修复。

> [!NOTE]
> 该工具主要修复 Windows Installer 登记和卸载状态，不保证自动删除所有服务、缓存、AppData 和用户文件。

---

# 9. 第六阶段：卸载 Dassault 前置组件

## 9.1 Dassault VBA 7.1

```powershell
$vbaInstaller = 'C:\ProgramData\Package Cache\{a6ae86d7-6ebc-4bda-8a47-4f265093612a}\DSVBA71Installer.exe'

if (Test-Path $vbaInstaller) {
    $process = Start-Process `
        -FilePath $vbaInstaller `
        -ArgumentList '/uninstall' `
        -Wait `
        -PassThru

    Write-Host "VBA 卸载返回码：$($process.ExitCode)"
}
```

本次返回 `0`。

## 9.2 Dassault VC12、VC11、VC10

> [!IMPORTANT]
> 这里卸载的是名称明确带有 `Dassault Systemes Software` 的组件，不是删除系统中所有 Microsoft Visual C++ Redistributable。

```powershell
$vcProducts = @(
    [PSCustomObject]@{
        Name = 'Dassault VC12 Prerequisites'
        Code = '{B9449F79-F230-4631-9C42-6B3CD08FFD5E}'
    },
    [PSCustomObject]@{
        Name = 'Dassault VC11 Prerequisites'
        Code = '{C857169D-3F1A-4530-99A0-CAE966CE267E}'
    },
    [PSCustomObject]@{
        Name = 'Dassault VC10 Prerequisites'
        Code = '{7C534131-6431-4ECB-9069-525CB5F75CC8}'
    }
)

foreach ($item in $vcProducts) {
    $arguments = "/x $($item.Code) /passive /norestart"

    $process = Start-Process `
        -FilePath "$env:SystemRoot\System32\msiexec.exe" `
        -ArgumentList $arguments `
        -Wait `
        -PassThru

    Write-Host "$($item.Name)：$($process.ExitCode)"
}
```

本次三项均返回 `0`。

## 9.3 验证安装登记

```powershell
$remaining = @(
    Get-ItemProperty $uninstallPaths -ErrorAction SilentlyContinue |
    Where-Object {
        "$($_.DisplayName) $($_.Publisher)" -match
        'SOLIDWORKS|Dassault|CATIA|3DEXPERIENCE'
    }
)

Write-Host "剩余安装登记数量：$($remaining.Count)"
```

结果为 `0`。

---

# 10. 第七阶段：删除孤立服务

重启后仍发现：

```text
SolidWorks Flexnet Server
SolidWorks Licensing Service
```

它们指向的可执行文件已经不存在。

## 10.1 备份服务注册表

```powershell
$backupDir = "$env:USERPROFILE\Desktop\Dassault_Service_Backup"
New-Item -ItemType Directory -Path $backupDir -Force | Out-Null

& reg.exe export `
    "HKLM\SYSTEM\CurrentControlSet\Services\SolidWorks Flexnet Server" `
    "$backupDir\SolidWorks_Flexnet_Server.reg" `
    /y

& reg.exe export `
    "HKLM\SYSTEM\CurrentControlSet\Services\SolidWorks Licensing Service" `
    "$backupDir\SolidWorks_Licensing_Service.reg" `
    /y
```

## 10.2 删除服务

```powershell
& sc.exe stop "SolidWorks Flexnet Server"
& sc.exe delete "SolidWorks Flexnet Server"

& sc.exe stop "SolidWorks Licensing Service"
& sc.exe delete "SolidWorks Licensing Service"
```

服务本来未启动时，`stop` 返回 `1062` 不影响删除。关键结果是：

```text
[SC] DeleteService SUCCESS
```

## 10.3 验证

```powershell
$remainingServices = @(
    Get-CimInstance Win32_Service -ErrorAction SilentlyContinue |
    Where-Object {
        $_.Name -in @(
            'SolidWorks Flexnet Server',
            'SolidWorks Licensing Service'
        )
    }
)

Write-Host "剩余目标服务数量：$($remainingServices.Count)"
```

结果为 `0`。

---

# 11. 第八阶段：隔离并清理残留目录与注册表

## 11.1 先检查工程文件

重点扩展名：

```text
.sldprt .sldasm .slddrw
.prtdot .asmdot .drwdot
.sldmat .sldlfp
.catpart .catproduct .catdrawing
.3dxml .step .stp .iges .igs
.x_t .x_b .dwg .dxf
```

扫描：

```powershell
$targetFolders = @(
    'C:\Program Files\Dassault Systemes',
    'C:\ProgramData\DassaultSystemes',
    "$env:LOCALAPPDATA\DassaultSystemes",
    "$env:LOCALAPPDATA\SOLIDWORKS",
    "$env:APPDATA\DassaultSystemes",
    'C:\Users\Public\Documents\SOLIDWORKS'
)

$projectPattern = '(?i)^\.(sldprt|sldasm|slddrw|prtdot|asmdot|drwdot|sldmat|sldlfp|catpart|catproduct|catdrawing|3dxml|step|stp|iges|igs|x_t|x_b|dwg|dxf)$'

$projectFiles = @(
    foreach ($folder in $targetFolders) {
        if (Test-Path $folder) {
            Get-ChildItem $folder -Recurse -Force -File -ErrorAction SilentlyContinue |
            Where-Object {
                $_.Extension -match $projectPattern
            }
        }
    }
)

Write-Host "发现可能需要保留的工程文件：$($projectFiles.Count)"
```

本次结果为 `0`。

## 11.2 备份厂商注册表

```powershell
$stamp = Get-Date -Format 'yyyyMMdd_HHmmss'
$backupDir = "$env:USERPROFILE\Desktop\Dassault_Final_Backup_$stamp"

New-Item -ItemType Directory -Path $backupDir -Force | Out-Null

& reg.exe export "HKLM\SOFTWARE\Dassault Systemes" `
    "$backupDir\HKLM_Dassault_Systemes.reg" /y

& reg.exe export "HKLM\SOFTWARE\WOW6432Node\Dassault Systemes" `
    "$backupDir\HKLM_WOW6432Node_Dassault_Systemes.reg" /y

& reg.exe export "HKLM\SOFTWARE\WOW6432Node\SOLIDWORKS" `
    "$backupDir\HKLM_WOW6432Node_SOLIDWORKS.reg" /y

& reg.exe export "HKCU\SOFTWARE\Dassault Systemes" `
    "$backupDir\HKCU_Dassault_Systemes.reg" /y
```

## 11.3 先改名隔离，不立即永久删除

```powershell
$renameResults = @()

foreach ($folder in $targetFolders) {
    if (-not (Test-Path $folder)) {
        continue
    }

    $parent = Split-Path $folder -Parent
    $leaf = Split-Path $folder -Leaf
    $newLeaf = "$leaf.ZJX_DASSAULT_REMOVED_$stamp"
    $newPath = Join-Path $parent $newLeaf

    Rename-Item `
        -LiteralPath $folder `
        -NewName $newLeaf `
        -ErrorAction Stop

    $renameResults += [PSCustomObject]@{
        OriginalPath   = $folder
        QuarantinePath = $newPath
    }
}
```

本次共隔离 6 个目录：

```text
C:\Program Files\Dassault Systemes
C:\ProgramData\DassaultSystemes
%LOCALAPPDATA%\DassaultSystemes
%LOCALAPPDATA%\SOLIDWORKS
%APPDATA%\DassaultSystemes
C:\Users\Public\Documents\SOLIDWORKS
```

## 11.4 删除已备份的厂商注册表根目录

```powershell
$registryRoots = @(
    'HKLM:\SOFTWARE\Dassault Systemes',
    'HKLM:\SOFTWARE\WOW6432Node\Dassault Systemes',
    'HKLM:\SOFTWARE\WOW6432Node\SOLIDWORKS',
    'HKCU:\SOFTWARE\Dassault Systemes'
)

foreach ($path in $registryRoots) {
    if (Test-Path $path) {
        Remove-Item `
            -LiteralPath $path `
            -Recurse `
            -Force `
            -ErrorAction Stop
    }
}
```

完成后正常重启。

---

# 12. 第九阶段：重启后的最终验证

最终必须同时满足：

```text
安装登记 = 0
相关进程 = 0
相关服务 = 0
相关计划任务 = 0
原始目录均不存在
厂商注册表均不存在
相关环境变量 = 0
```

验证核心：

```powershell
$ErrorActionPreference = 'SilentlyContinue'

$rx = '(?i)SOLIDWORKS|Dassault|CATIA|3DEXPERIENCE|3D\s*EXPERIENCE|SolidWorks Flexnet|DTSInterops|SWVisualize|DSLS|DS License'

$uninstallPaths = @(
    'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*',
    'HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*',
    'HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*'
)

$apps = @(
    Get-ItemProperty $uninstallPaths |
    Where-Object {
        "$($_.DisplayName) $($_.Publisher) $($_.InstallLocation)" -match $rx
    }
)

$processes = @(
    Get-CimInstance Win32_Process |
    Where-Object {
        "$($_.Name) $($_.ExecutablePath) $($_.CommandLine)" -match $rx
    }
)

$services = @(
    Get-CimInstance Win32_Service |
    Where-Object {
        "$($_.Name) $($_.DisplayName) $($_.PathName)" -match $rx
    }
)

$environmentMatches = @(
    foreach ($scope in @('Machine', 'User')) {
        $variables = [Environment]::GetEnvironmentVariables($scope)

        foreach ($entry in $variables.GetEnumerator()) {
            if ("$($entry.Key) $($entry.Value)" -match $rx) {
                [PSCustomObject]@{
                    Scope = $scope
                    Name  = $entry.Key
                    Value = $entry.Value
                }
            }
        }
    }
)

Write-Host "剩余相关安装登记：$($apps.Count)"
Write-Host "剩余相关进程：$($processes.Count)"
Write-Host "剩余相关服务：$($services.Count)"
Write-Host "相关环境变量：$($environmentMatches.Count)"
```

本次以上结果全部为 `0`；厂商注册表与原始目录全部为 `False`。

---

# 13. 第十阶段：永久删除隔离目录

经过重启验证后，永久删除隔离目录：

```powershell
$quarantineFolders = @(
    'C:\Program Files\Dassault Systemes.ZJX_DASSAULT_REMOVED_时间戳',
    'C:\ProgramData\DassaultSystemes.ZJX_DASSAULT_REMOVED_时间戳',
    "$env:LOCALAPPDATA\DassaultSystemes.ZJX_DASSAULT_REMOVED_时间戳",
    "$env:LOCALAPPDATA\SOLIDWORKS.ZJX_DASSAULT_REMOVED_时间戳",
    "$env:APPDATA\DassaultSystemes.ZJX_DASSAULT_REMOVED_时间戳",
    'C:\Users\Public\Documents\SOLIDWORKS.ZJX_DASSAULT_REMOVED_时间戳'
)

foreach ($folder in $quarantineFolders) {
    if (Test-Path -LiteralPath $folder) {
        Remove-Item `
            -LiteralPath $folder `
            -Recurse `
            -Force `
            -ErrorAction Stop
    }
}
```

删除后再次用 `Test-Path` 验证，本次 6 个隔离目录均不存在。

---

# 14. 最终结果

| 检查项目 | 结果 |
|---|---:|
| SOLIDWORKS/Dassault/CATIA/3DE 安装登记 | 0 |
| 相关进程 | 0 |
| 相关 Windows 服务 | 0 |
| 相关计划任务 | 0 |
| 相关环境变量 | 0 |
| 已知原始程序目录 | 全部不存在 |
| 厂商注册表根目录 | 全部不存在 |
| 隔离目录 | 全部永久删除 |
| 常见工程/模板文件误删 | 未发现 |

由此可判定：

> 从 Windows 的软件安装体系、MSI 状态、程序文件、服务、计划任务、厂商注册表、缓存目录和环境变量角度看，SOLIDWORKS、CATIA、3DEXPERIENCE 与 Dassault Systèmes 相关组件已经完整清除。

用户自己存放在其他位置的工程文件不属于软件残留，例如：

```text
.SLDPRT .SLDASM .SLDDRW
.CATPart .CATProduct .CATDrawing
.STEP .DWG
```

---

# 15. 可复用的软件卸载标准流程

以后处理任何“以前乱删过、现在无法卸载或重装”的 Windows 软件，可以使用以下流程：

```text
1. 查正常卸载入口
2. 查卸载登记
3. 判断 MSI 或厂商 EXE 安装体系
4. 使用准确 ProductCode 或官方卸载器
5. 记录返回码并生成日志
6. 必要时使用官方疑难解答修复损坏登记
7. 检查服务、计划任务、启动项和环境变量
8. 检查个人文件
9. 备份注册表
10. 隔离残留目录
11. 重启验证
12. 最后永久删除
```

查询卸载登记：

```powershell
$paths = @(
    'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*',
    'HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*',
    'HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*'
)

Get-ItemProperty $paths -ErrorAction SilentlyContinue |
Where-Object {
    $_.DisplayName -match '软件名称'
} |
Select-Object `
    DisplayName,
    DisplayVersion,
    WindowsInstaller,
    SystemComponent,
    InstallLocation,
    UninstallString,
    PSChildName |
Format-List
```

特殊安装体系应使用各自工具，例如：

```text
Autodesk ODIS
Adobe Creative Cloud
Visual Studio Installer
SQL Server Installation Center
Microsoft Store
驱动程序
Windows 可选功能
```

---

# 16. 常见错误与经验总结

## 16.1 直接删程序目录不是卸载

错误流程：

```text
删除 Program Files 或 D 盘软件目录
→ 删除部分注册表
→ 再尝试卸载或重装
```

可能造成 MSI 缓存丢失、安装器拒绝覆盖、服务残留以及 1603/1612 错误。

## 16.2 控制面板看不到不等于没有登记

重点检查：

```text
SystemComponent = 1
```

以及三个卸载注册表位置。

## 16.3 不要把所有 GUID 都当 ProductCode

只有明确属于 Windows Installer 产品的 GUID 才能用于：

```powershell
msiexec /x {GUID}
```

## 16.4 不要删除 Windows Installer 缓存

禁止手动清理：

```text
C:\Windows\Installer
C:\ProgramData\Package Cache
```

## 16.5 不要随意卸载共享运行库

不能因为某套软件安装过它们，就删除系统中的所有：

```text
Microsoft Visual C++ Redistributable
.NET Runtime
WebView2 Runtime
Windows SDK
FlexNet
```

## 16.6 PowerShell 中 `else` 必须与 `if` 一起提交

在交互式 PowerShell 中先执行完 `if`，再单独输入 `else`，会出现 `else is not recognized`。这只是语法提交方式问题，不代表前面的操作失败。

---

# 17. 附录：本次涉及的 ProductCode

> [!CAUTION]
> 以下代码只对应本次扫描到的 2025 SP0 组件。其他版本必须重新读取本机卸载登记，不能照抄。

| 产品 | ProductCode |
|---|---|
| SOLIDWORKS 2025 SP0 | `{DB2C3F1B-3025-4743-AAA8-1B5E20047E34}` |
| SOLIDWORKS Visualize 2025 SP0 | `{5A836655-81D9-4702-92BC-0C2DD424BF41}` |
| SOLIDWORKS CAM 2025 SP0 | `{B56E1B6B-5ADA-4E85-B1C1-D8DE719A24AE}` |
| SOLIDWORKS Composer Player 2025 SP0 | `{D28B5B78-D35E-4463-B211-CFAE2A19327A}` |
| SOLIDWORKS eDrawings 2025 SP0 | `{7D7F080A-418C-4366-A783-EEB48737914F}` |
| SOLIDWORKS File Utilities 2025 SP0 | `{3D5985E3-CBCB-42E6-9B79-00CE9B183015}` |
| SOLIDWORKS 2025 简体中文资源 | `{CA4C86CC-6E7D-4A70-A51B-0FDE437D989E}` |
| CEF for SOLIDWORKS Applications | `{D2AD4CE7-115F-47BD-93D8-725570B53346}` |
| Dassault VBA 7.1 Bundle | `{a6ae86d7-6ebc-4bda-8a47-4f265093612a}` |
| Dassault VC12 Prerequisites | `{B9449F79-F230-4631-9C42-6B3CD08FFD5E}` |
| Dassault VC11 Prerequisites | `{C857169D-3F1A-4530-99A0-CAE966CE267E}` |
| Dassault VC10 Prerequisites | `{7C534131-6431-4ECB-9069-525CB5F75CC8}` |

---

# 18. 附录：常见 MSI 返回码

| 返回码 | 含义 | 建议 |
|---:|---|---|
| `0` | 成功 | 继续验证登记和目录 |
| `3010` | 成功，但需要重启 | 完成本轮后重启 |
| `1605` | 产品未安装 | 检查是否只剩孤立注册表 |
| `1614` | 产品已经卸载 | 继续验证 |
| `1612` | 找不到安装源 | 检查 MSI 缓存或使用疑难解答 |
| `1603` | 通用严重错误 | 查看详细日志中的 `Return value 3` |
| `1618` | 另一安装/卸载正在运行 | 等待其他 MSI 操作结束 |

---

## 参考资料

- [Microsoft Support：Fix problems that block programs from being installed or removed](https://support.microsoft.com/en-us/windows/deployment/install-upgrade/fix-problems-that-block-programs-from-being-installed-or-removed)
- [Microsoft Program Install and Uninstall troubleshooter 直接下载](https://download.microsoft.com/download/7/E/9/7E9188C0-2511-4B01-8B4E-0A641EC2F600/MicrosoftProgram_Install_and_Uninstall.meta.diagcab)

---

## 免责声明

本文记录的是一次具体电脑上的实际处理过程。

执行任何卸载和注册表操作前，请至少做到：

```text
确认产品名称和版本
确认 ProductCode
备份重要工程文件
导出待删除注册表
记录服务和目录路径
逐项执行并验证返回码
完成后重启再复查
```

不要把本文中的 ProductCode 直接套用于其他版本或其他电脑。

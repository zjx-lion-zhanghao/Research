# Autodesk Windows 全量干净卸载实战记录

> 一次针对 AutoCAD、Inventor、Fusion、Autodesk Access、ODIS、Licensing Service、Identity Manager、Genuine Service 及相关共享组件的完整清理记录。
>
> 实测日期：2026-07-26  
> 适用系统：Windows 10 / Windows 11  
> 文档定位：**故障排查纪实 + 可复用检查框架**，不是通用“一键删除脚本”。

---

## 目录

- [1. 为什么需要全量干净卸载](#1-为什么需要全量干净卸载)
- [2. 重要警告](#2-重要警告)
- [3. 官方工具与参考资料](#3-官方工具与参考资料)
- [4. 本次故障背景](#4-本次故障背景)
- [5. 清理总原则](#5-清理总原则)
- [6. 建立备份和日志目录](#6-建立备份和日志目录)
- [7. 第一阶段：盘点全部 Autodesk 安装登记](#7-第一阶段盘点全部-autodesk-安装登记)
- [8. 第二阶段：优先使用正规卸载方式](#8-第二阶段优先使用正规卸载方式)
- [9. 第三阶段：处理主程序卸载记录损坏](#9-第三阶段处理主程序卸载记录损坏)
- [10. 第四阶段：卸载可正常工作的 MSI 附属组件](#10-第四阶段卸载可正常工作的-msi-附属组件)
- [11. 第五阶段：移除 Autodesk Access 与 ODIS](#11-第五阶段移除-autodesk-access-与-odis)
- [12. 第六阶段：修复并卸载 Desktop Licensing Service](#12-第六阶段修复并卸载-desktop-licensing-service)
- [13. 第七阶段：处理 Access 残留登记](#13-第七阶段处理-access-残留登记)
- [14. 第八阶段：卸载 Identity Manager](#14-第八阶段卸载-identity-manager)
- [15. 第九阶段：清理隐藏的 ACAD Private、REX 与 RSA 组件](#15-第九阶段清理隐藏的-acad-privaterex-与-rsa-组件)
- [16. 第十阶段：清理孤立的 AdskNLM 服务](#16-第十阶段清理孤立的-adsknlm-服务)
- [17. 第十一阶段：最后卸载 Genuine Service](#17-第十一阶段最后卸载-genuine-service)
- [18. 第十二阶段：删除标准残留目录](#18-第十二阶段删除标准残留目录)
- [19. 第十三阶段：处理被 Explorer 占用的 AcSignCore16.dll](#19-第十三阶段处理被-explorer-占用的-acsigncore16dll)
- [20. 第十四阶段：备份并删除 Autodesk 主注册表键](#20-第十四阶段备份并删除-autodesk-主注册表键)
- [21. 第十五阶段：删除 C:\Autodesk 安装缓存](#21-第十五阶段删除-cautodesk-安装缓存)
- [22. 最终全量审计](#22-最终全量审计)
- [23. 本次实机涉及的产品代码](#23-本次实机涉及的产品代码)
- [24. Windows Installer 常见退出码](#24-windows-installer-常见退出码)
- [25. 本次踩坑与经验](#25-本次踩坑与经验)
- [26. 绝对不要做的事](#26-绝对不要做的事)
- [27. 清理完成后的重新安装建议](#27-清理完成后的重新安装建议)
- [28. 最终结果](#28-最终结果)

---

## 1. 为什么需要全量干净卸载

普通卸载通常只删除主程序，但可能保留：

- Windows 卸载登记；
- ODIS 安装框架；
- Autodesk Access；
- Licensing Service；
- Identity Manager；
- Genuine Service；
- Network License Manager；
- MSI 附属组件；
- Program Files、ProgramData 和 AppData 中的缓存；
- Autodesk 主注册表键；
- FLEXnet 中的 Autodesk 许可文件；
- 指向已不存在程序的服务或启动项。

当这些状态彼此不一致时，常见现象包括：

- Windows 仍认为软件已经安装；
- 卸载入口存在，但卸载器无法启动；
- 安装器提示缺少 MSI；
- ODIS 提示准备安装失败；
- 新版本安装器拒绝覆盖旧版本；
- Licensing Service 无法卸载或缺少 `uninstall.dat`；
- “已安装的应用”中出现幽灵条目；
- 软件目录被删了，但注册表、服务和 Windows Installer 登记仍在。

Autodesk 官方将这种面向**全部 Autodesk 软件**的彻底移除称为 Clean Uninstall。只卸载单个产品时，不应直接照搬本文全部步骤。

---

## 2. 重要警告

> [!WARNING]
> 本文包含注册表备份与删除、服务删除、权限修复和恢复环境离线删除。执行前必须确认目标是**移除电脑上的全部 Autodesk 产品**。

### 不适合使用本文完整流程的情况

- 还需要保留任意 Autodesk 产品；
- 公司电脑由 IT 管理；
- 使用网络许可证但不清楚许可文件位置；
- 没有管理员权限；
- 没有备份重要模板、材料库、插件、项目和自定义配置；
- 不清楚 BitLocker 恢复密钥。

### 公开上传 GitHub 时的隐私处理

本文中的用户目录统一写成：

```text
C:\Users\<用户名>
```

不要上传：

- BitLocker 48 位恢复密钥；
- Autodesk 账号信息；
- 许可证文件；
- 序列号；
- 私有日志中的邮箱、用户名和组织信息。

---

## 3. 官方工具与参考资料

### 3.1 Microsoft“程序安装和卸载疑难解答”

这是本次使用的微软工具，可修复损坏的卸载注册表、安装状态和不完整卸载信息。

- 官方说明页面：  
  <https://support.microsoft.com/zh-CN/windows/deployment/install-upgrade/fix-problems-that-block-programs-from-being-installed-or-removed>

- 官方直接下载：  
  <https://download.microsoft.com/download/7/E/9/7E9188C0-2511-4B01-8B4E-0A641EC2F600/MicrosoftProgram_Install_and_Uninstall.meta.diagcab>

> 当程序没有出现在工具列表中时，微软工具可能要求提供 MSI ProductCode。不要把 ODIS 的包 ID、UPI2 ID 或任意 GUID 当作 MSI ProductCode 猜测输入。

### 3.2 Autodesk 官方资料

- Autodesk 全量干净卸载：  
  <https://www.autodesk.com/support/technical/article/caas/sfdcarticles/sfdcarticles/Clean-uninstall.html>

- 卸载 Autodesk 软件：  
  <https://www.autodesk.com/sg/support/download-install/individuals/manage/uninstall-autodesk-software>

- 卸载 Autodesk Desktop Licensing Service：  
  <https://www.autodesk.com/support/technical/article/caas/sfdcarticles/sfdcarticles/How-to-uninstall-Autodesk-Desktop-Licensing-Service.html>

- 下载并安装 Desktop Licensing Service：  
  <https://www.autodesk.com/support/technical/article/caas/sfdcarticles/sfdcarticles/How-to-download-and-install-Autodesk-Licensing-Service.html>

- `uninstall.dat` 丢失的官方处理：  
  <https://www.autodesk.com/support/technical/article/caas/sfdcarticles/sfdcarticles/The-uninstall-dat-file-cannot-be-found-and-is-required-to-uninstall-the-application-aborting-when-uninstalling-AdskLicensing-tool.html>

- 卸载 Autodesk Access：  
  <https://www.autodesk.com/support/technical/article/caas/sfdcarticles/sfdcarticles/How-do-I-remove-hide-or-uninstall-Autodesk-Access.html>

- 卸载 Network License Manager：  
  <https://help.autodesk.com/cloudhelp/ENU/Autodesk-NetworkAdmin/files/GUID-24E64A50-9760-4880-8B45-68B60C548F46.htm>

- ODIS 产品与 `msiexec /x` 的区别：  
  <https://www.autodesk.com/support/technical/article/caas/sfdcarticles/sfdcarticles/Msiexec-exe-X-command-to-uninstall-Autodesk-software-installed-via-the-New-Installation-Experience-ODIS.html>

---

## 4. 本次故障背景

本机曾安装或残留：

- AutoCAD 2025 / 2026；
- Inventor 2024 / 2026；
- Fusion；
- Autodesk Access；
- Autodesk App Manager；
- Autodesk Save to Web and Mobile；
- Autodesk Interoperability Engine Manager；
- Autodesk CER；
- Autodesk Identity Manager；
- Autodesk Desktop Licensing Service；
- Autodesk Genuine Service；
- REX Framework；
- RSA Engine；
- Autodesk Network License Manager；
- ACAD Private 2025 / 2026。

历史上曾直接删除部分文件和注册表，导致：

- Inventor 2026 的 MSI 安装源丢失；
- ODIS 元数据不完整；
- Access 卸载时报“准备安装时发生错误”；
- Licensing Service 的 `uninstall.dat` 丢失；
- 部分组件只能看到登记，找不到正常卸载入口；
- `AdskNLM` 服务存在，但对应卸载登记和程序目录不完整；
- `AcSignCore16.dll` 被 Explorer 加载，普通模式无法删除。

---

## 5. 清理总原则

本次采用以下顺序：

```text
只读盘点
→ 正规卸载主程序和 MSI 组件
→ 修复或移除 ODIS / Access
→ 修复并卸载 Licensing Service
→ 卸载 Identity Manager
→ 清理遗漏的 ACAD Private / REX / RSA
→ 清理孤立 NLM 服务
→ 最后卸载 Genuine Service
→ 删除标准残留目录
→ 备份并删除 Autodesk 主注册表键
→ 删除 C:\Autodesk 安装缓存
→ 最终全量审计
```

核心规则：

1. **先卸载，后删文件。**
2. **先备份，后删注册表。**
3. **一次只处理一个组件。**
4. **退出码只是证据之一，必须二次验证。**
5. **Genuine Service 最后处理。**
6. **不要删除整个 FLEXnet。**
7. **不要碰 `C:\Windows\Installer`。**
8. **ODIS 主产品不能简单把某个 ODIS GUID 当 MSI ProductCode。**

---

## 6. 建立备份和日志目录

管理员 PowerShell：

```powershell
New-Item -ItemType Directory `
    -Path 'D:\AutodeskCleanupBackup' `
    -Force | Out-Null

New-Item -ItemType Directory `
    -Path 'D:\AutodeskCleanupLogs' `
    -Force | Out-Null
```

本次使用：

```text
D:\AutodeskCleanupBackup
D:\AutodeskCleanupLogs
```

建议每个危险操作创建时间戳子目录：

```powershell
$stamp = Get-Date -Format 'yyyyMMdd-HHmmss'
$backupFolder = "D:\AutodeskCleanupBackup\Operation-$stamp"
New-Item -ItemType Directory -Path $backupFolder -Force | Out-Null
```

---

## 7. 第一阶段：盘点全部 Autodesk 安装登记

```powershell
$uninstallPaths = @(
    'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*',
    'HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*',
    'HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*'
)

Get-ItemProperty $uninstallPaths -ErrorAction SilentlyContinue |
Where-Object {
    $_.Publisher -match '^Autodesk' -or
    $_.DisplayName -match 'Autodesk|AutoCAD|Inventor|Fusion|Adsk'
} |
Select-Object DisplayName,
              DisplayVersion,
              Publisher,
              PSChildName,
              UninstallString,
              InstallLocation,
              PSPath |
Sort-Object DisplayName, DisplayVersion |
Format-List
```

### 为什么需要 `Format-List`

`Format-Table` 会截断 GUID 和路径。真正操作前必须看到完整：

- `PSChildName`；
- `UninstallString`；
- `InstallLocation`；
- `PSPath`。

### 本次第一次盘点后仅剩的核心组件

在清理中后期曾检测到：

```text
Autodesk Access
Autodesk Genuine Service
Autodesk Identity Manager
ACAD Private 25.0
ACAD Private 25.1
REX Framework
RSA Engine
```

这说明“主程序消失”不等于“Autodesk 已全部卸载”。

---

## 8. 第二阶段：优先使用正规卸载方式

### 8.1 Windows 设置或控制面板

首先尝试：

```text
Win + R
→ appwiz.cpl
→ 找到 Autodesk 产品
→ 卸载
```

### 8.2 Microsoft 疑难解答

正规卸载失败时：

```text
运行 MicrosoftProgram_Install_and_Uninstall.meta.diagcab
→ 正在卸载
→ 选择目标程序
→ 是，尝试卸载
```

本次工具能看到部分 Inventor 残留，但 Access 没有出现在列表中。

### 8.3 不要猜 ProductCode

当微软工具选择“未列出”时，会要求产品代码。只有确定它是 MSI 的 ProductCode 才能输入。

以下不能混为一谈：

- MSI ProductCode；
- ODIS 包 ID；
- UPI2；
- 注册表子键 GUID；
- UpgradeCode。

---

## 9. 第三阶段：处理主程序卸载记录损坏

### 9.1 Inventor 2026：MSI 源丢失

本次产品代码：

```text
{7F4DD591-3064-0001-0000-7107D70F3DB4}
```

执行：

```powershell
$p = Start-Process `
    -FilePath "$env:SystemRoot\System32\msiexec.exe" `
    -ArgumentList '/x {7F4DD591-3064-0001-0000-7107D70F3DB4} /L*v "D:\AutodeskCleanupLogs\Inventor2026-uninstall.log"' `
    -Wait `
    -PassThru

$p.ExitCode
```

返回：

```text
1612
```

含义：Windows Installer 找不到原始安装源 `Inventor.msi`。

微软疑难解答也无法修复后，才对已经确认名称的卸载登记、Installer Products 和 UpgradeCodes 做**备份后的定点删除**。

> [!CAUTION]
> 不要把这一步做成“搜索所有 Autodesk 字符串后批量删除”。必须按产品代码定位、核对 DisplayName、导出 `.reg`，再删除单个目标。

### 9.2 通用的安全注册表导出模式

```powershell
$target = 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\{目标GUID}'
$native = 'HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\{目标GUID}'
$backup = 'D:\AutodeskCleanupBackup\Target-Uninstall.reg'

$item = Get-ItemProperty $target -ErrorAction Stop
$item | Select-Object DisplayName, DisplayVersion, Publisher | Format-List

& reg.exe export $native $backup /y

if ($LASTEXITCODE -ne 0) {
    throw '备份失败，停止删除。'
}

Remove-Item $target -Recurse -Force
```

---

## 10. 第四阶段：卸载可正常工作的 MSI 附属组件

本次以下组件通过 `msiexec /x` 正常卸载：

| 组件 | ProductCode | 首次退出码 |
|---|---|---:|
| Autodesk Save to Web and Mobile | `{5BF7551A-09A6-4BDD-AE1E-048104FB478C}` | 0 |
| Autodesk App Manager | `{0FC454BE-3FA9-40A2-B5F1-C15B3CE740AA}` | 0 |
| Autodesk Interoperability Engine Manager | `{217D7134-F441-3B94-8AAB-63175C9228A9}` | 0 |
| Autodesk CER | `{C2895666-EC78-4E43-A590-A744CEDD756A}` | 0 |
| REX Inventor | `{B2285C37-5FC0-445A-A7D0-7F331481DD56}` | 0 |

通用命令：

```powershell
$p = Start-Process `
    -FilePath "$env:SystemRoot\System32\msiexec.exe" `
    -ArgumentList '/x {PRODUCT-CODE} /L*v "D:\AutodeskCleanupLogs\component-uninstall.log"' `
    -Wait `
    -PassThru

$p.ExitCode
```

### 重复执行为什么返回 1605

例如 Interoperability 和 RSA Engine 首次返回 `0`，重复执行返回：

```text
1605
```

这通常表示产品已经不在 Windows Installer 登记中。**成功后不要重复运行同一卸载命令。**

---

## 11. 第五阶段：移除 Autodesk Access 与 ODIS

### 11.1 停止 Access 服务

```powershell
Get-Service -ErrorAction SilentlyContinue |
Where-Object {
    $_.DisplayName -eq 'Autodesk Access Service Host'
} |
Stop-Service -Force -ErrorAction SilentlyContinue
```

验证：

```powershell
Get-Service -ErrorAction SilentlyContinue |
Where-Object {
    $_.DisplayName -eq 'Autodesk Access Service Host'
} |
Format-Table Name, DisplayName, Status, StartType -AutoSize
```

### 11.2 运行 RemoveODIS

```powershell
$removeOdis = 'C:\Program Files\Autodesk\AdODIS\V1\RemoveODIS.exe'

if (Test-Path $removeOdis) {
    $p = Start-Process `
        -FilePath $removeOdis `
        -Verb RunAs `
        -Wait `
        -PassThru

    Write-Host "RemoveODIS 退出码：$($p.ExitCode)"
}
```

验证：

```powershell
[PSCustomObject]@{
    RemoveODISExists = Test-Path 'C:\Program Files\Autodesk\AdODIS\V1\RemoveODIS.exe'
    InstallerExists  = Test-Path 'C:\Program Files\Autodesk\AdODIS\V1\Installer.exe'
    AdODISFolder     = Test-Path 'C:\Program Files\Autodesk\AdODIS\V1'
} | Format-List
```

本次结果：

```text
RemoveODISExists : False
InstallerExists  : False
AdODISFolder     : True
```

说明 ODIS 主程序已被移除，但留下空目录或零散文件。

### 11.3 PowerShell 的 `else` 语法坑

错误示例：

```powershell
if (Test-Path $path) {
    Write-Host '存在'
}

# 已经执行完上一段后，再单独输入：
else {
    Write-Host '不存在'
}
```

会得到：

```text
else : The term 'else' is not recognized...
```

原因：PowerShell 已经把前面的 `if` 当成完整语句执行。`else` 必须和 `if` 在同一次解析中提交。

更稳妥的交互式写法是避免 `else`：

```powershell
if (Test-Path $path) {
    Write-Host '存在'
}

if (-not (Test-Path $path)) {
    Write-Host '不存在'
}
```

---

## 12. 第六阶段：修复并卸载 Desktop Licensing Service

### 12.1 发现的问题

卸载程序存在：

```text
C:\Program Files (x86)\Common Files\Autodesk Shared\AdskLicensing\uninstall.exe
```

PowerShell 返回退出码 `0`，但图形界面实际报错：

```text
The uninstall.dat file cannot be found and is required to uninstall the application, aborting
```

这说明：**不能只信 `$p.ExitCode`。** 外层卸载器可能返回 0，但内部卸载失败。

### 12.2 按 Autodesk 官方方式修复

先停止许可相关服务，把损坏目录重命名：

```powershell
$root = 'C:\Program Files (x86)\Common Files\Autodesk Shared'
$licensingFolder = Join-Path $root 'AdskLicensing'
$backupFolder = Join-Path $root (
    'AdskLicensing.broken-' + (Get-Date -Format 'yyyyMMdd-HHmmss')
)

Get-Service -ErrorAction SilentlyContinue |
Where-Object {
    $_.Name -match 'AdskLicensing' -or
    $_.DisplayName -match 'Autodesk.*Licensing'
} |
Stop-Service -Force -ErrorAction SilentlyContinue

if (Test-Path $licensingFolder) {
    Rename-Item `
        -Path $licensingFolder `
        -NewName (Split-Path $backupFolder -Leaf)

    Write-Host "损坏目录已重命名为：$backupFolder"
}
```

然后从 Autodesk 官网下载并安装最新版 Desktop Licensing Service。

### 12.3 验证修复是否成功

```powershell
$licensingFolder =
    'C:\Program Files (x86)\Common Files\Autodesk Shared\AdskLicensing'

[PSCustomObject]@{
    FolderExists      = Test-Path $licensingFolder
    UninstallerExists = Test-Path "$licensingFolder\uninstall.exe"
    ServiceExeExists  = Test-Path "$licensingFolder\Current\AdskLicensingService\AdskLicensingService.exe"
    UninstallDatCount = @(
        Get-ChildItem `
            $licensingFolder `
            -Filter 'uninstall.dat' `
            -Recurse `
            -ErrorAction SilentlyContinue
    ).Count
} | Format-List
```

本次修复后：

```text
FolderExists      : True
UninstallerExists : True
ServiceExeExists  : True
UninstallDatCount : 1
```

### 12.4 正式卸载 Licensing Service

```powershell
$uninstaller =
    'C:\Program Files (x86)\Common Files\Autodesk Shared\AdskLicensing\uninstall.exe'

$p = Start-Process `
    -FilePath $uninstaller `
    -ArgumentList '--mode unattended' `
    -Verb RunAs `
    -Wait `
    -PassThru

Write-Host "Licensing Service 卸载退出码：$($p.ExitCode)"
Start-Sleep -Seconds 10
```

### 12.5 验证真实结果

```powershell
$licensingFolder =
    'C:\Program Files (x86)\Common Files\Autodesk Shared\AdskLicensing'

$remainingFiles = @(
    Get-ChildItem `
        -Path $licensingFolder `
        -Force `
        -Recurse `
        -ErrorAction SilentlyContinue
)

[PSCustomObject]@{
    FolderExists       = Test-Path $licensingFolder
    UninstallerExists  = Test-Path "$licensingFolder\uninstall.exe"
    ServiceExeExists   = Test-Path "$licensingFolder\Current\AdskLicensingService\AdskLicensingService.exe"
    UninstallDatCount  = @(
        Get-ChildItem `
            -Path $licensingFolder `
            -Filter 'uninstall.dat' `
            -Recurse `
            -ErrorAction SilentlyContinue
    ).Count
    RemainingItemCount = $remainingFiles.Count
} | Format-List
```

本次成功结果：

```text
FolderExists       : False
UninstallerExists  : False
ServiceExeExists   : False
UninstallDatCount  : 0
RemainingItemCount : 0
```

服务查询也无输出。

---

## 13. 第七阶段：处理 Access 残留登记

微软疑难解答列表中没有 Autodesk Access，但卸载登记仍存在：

```text
Autodesk Access 2.19.0.110
{A3158B3E-5F28-358A-BF1A-9532D8EBC811}
```

这个 GUID 是本次 Access 的卸载登记标识，不能盲目当作 MSI ProductCode。

在确认：

- ODIS 已移除；
- Access Service Host 已消失；
- Access 正常卸载入口不可用；

之后，执行名称校验、注册表导出和定点删除：

```powershell
$ErrorActionPreference = 'Stop'

$productId = '{A3158B3E-5F28-358A-BF1A-9532D8EBC811}'
$stamp = Get-Date -Format 'yyyyMMdd-HHmmss'
$backupFolder = "D:\AutodeskCleanupBackup\Access-$stamp"

New-Item -ItemType Directory -Path $backupFolder -Force | Out-Null

$targets = @(
    "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\$productId",
    "HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\$productId"
)

foreach ($target in $targets) {
    if (-not (Test-Path $target)) {
        Write-Host "不存在，跳过：$target"
        continue
    }

    $item = Get-ItemProperty $target
    Write-Host "检测到：$($item.DisplayName)"

    if ($item.DisplayName -ne 'Autodesk Access') {
        throw "名称不匹配，停止删除：$($item.DisplayName)"
    }

    $nativePath = $target -replace '^HKLM:\\', 'HKEY_LOCAL_MACHINE\'
    $backupFile = Join-Path $backupFolder 'Autodesk-Access-Uninstall.reg'

    & reg.exe export "$nativePath" "$backupFile" /y

    if ($LASTEXITCODE -ne 0) {
        throw "注册表备份失败，未执行删除：$nativePath"
    }

    Remove-Item $target -Recurse -Force
    Write-Host "已清除 Access 残留登记：$($item.DisplayName)"
}
```

验证：

```powershell
Get-ItemProperty $uninstallPaths -ErrorAction SilentlyContinue |
Where-Object {
    $_.DisplayName -eq 'Autodesk Access'
} |
Select-Object DisplayName, DisplayVersion, PSChildName |
Format-Table -AutoSize
```

无输出即表示 Access 卸载登记已清除。

---

## 14. 第八阶段：卸载 Identity Manager

检查：

```powershell
$identityUninstaller =
    'C:\Program Files\Autodesk\AdskIdentityManager\uninstall.exe'

[PSCustomObject]@{
    FolderExists      = Test-Path 'C:\Program Files\Autodesk\AdskIdentityManager'
    UninstallerExists = Test-Path $identityUninstaller
} | Format-List
```

运行：

```powershell
$p = Start-Process `
    -FilePath $identityUninstaller `
    -Verb RunAs `
    -Wait `
    -PassThru

Write-Host "Identity Manager 卸载退出码：$($p.ExitCode)"
```

本次结果：

```text
Identity Manager 卸载退出码：0
```

验证：

```powershell
Test-Path 'C:\Program Files\Autodesk\AdskIdentityManager'
```

结果：

```text
False
```

---

## 15. 第九阶段：清理隐藏的 ACAD Private、REX 与 RSA 组件

主程序和 Identity Manager 移除后，重新查询发现：

```text
ACAD Private 25.0.58.0
ACAD Private 25.1.60.0
REX Framework 24.0.0.5143
RSA Engine 24.0.0.10037
Autodesk Genuine Service
```

### 15.1 ACAD Private 2025

```powershell
$p = Start-Process `
    -FilePath "$env:SystemRoot\System32\msiexec.exe" `
    -ArgumentList '/x {28B89EEF-8101-0000-3102-CF3F3A09B77D} /L*v "D:\AutodeskCleanupLogs\ACAD-Private-2025-uninstall.log"' `
    -Wait `
    -PassThru

$p.ExitCode
```

结果：`0`。

### 15.2 ACAD Private 2026

```powershell
$p = Start-Process `
    -FilePath "$env:SystemRoot\System32\msiexec.exe" `
    -ArgumentList '/x {28B89EEF-9101-0000-3102-CF3F3A09B77D} /L*v "D:\AutodeskCleanupLogs\ACAD-Private-2026-uninstall.log"' `
    -Wait `
    -PassThru

$p.ExitCode
```

结果：`0`。

### 15.3 REX Framework

```powershell
$p = Start-Process `
    -FilePath "$env:SystemRoot\System32\msiexec.exe" `
    -ArgumentList '/x {D29C8D32-C8E0-42A8-AA21-71A4C17B6ACD} /L*v "D:\AutodeskCleanupLogs\REX-Framework-uninstall.log"' `
    -Wait `
    -PassThru

$p.ExitCode
```

结果：`0`。

### 15.4 RSA Engine

```powershell
$p = Start-Process `
    -FilePath "$env:SystemRoot\System32\msiexec.exe" `
    -ArgumentList '/x {E4BDBBF7-4F7D-44F9-825C-93610182835A} /L*v "D:\AutodeskCleanupLogs\RSA-Engine-uninstall.log"' `
    -Wait `
    -PassThru

$p.ExitCode
```

首次结果：`0`；重复执行结果：`1605`。

### 15.5 正则误匹配警告

曾使用：

```powershell
$_.DisplayName -match '...|REX|RSA'
```

结果把以下 Microsoft 组件也筛选出来：

```text
Universal CRT Extension SDK
Universal CRT Headers Libraries and Sources
Universal CRT Redistributable
Universal CRT Tools x64 / x86
Universal General MIDI DLS Extension SDK
```

原因：`RSA` 可以匹配 `Universal` 中的字符片段。

更安全的写法：

```powershell
$_.DisplayName -in @(
    'REX Framework',
    'RSA Engine'
)
```

不要因为关键词误匹配卸载 Microsoft Windows SDK 组件。

---

## 16. 第十阶段：清理孤立的 AdskNLM 服务

最终服务审计发现：

```text
Name        : AdskNLM
State       : Stopped
StartMode   : Auto
PathName    : "C:\Program Files (x86)\Common Files\Autodesk Shared\Network License Manager\lmgrd.exe"
```

但：

- 控制面板没有 Network License Manager 卸载项；
- 常见 NLM 目录不存在；
- 服务指向的程序文件夹也已缺失。

这属于孤立服务登记。

### 16.1 备份服务注册表

```powershell
$ErrorActionPreference = 'Stop'

$stamp = Get-Date -Format 'yyyyMMdd-HHmmss'
$backup = "D:\AutodeskCleanupBackup\AdskNLM-$stamp"
$serviceRegistry = 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\AdskNLM'

New-Item -ItemType Directory -Path $backup -Force | Out-Null

& reg.exe export `
    "$serviceRegistry" `
    "$backup\AdskNLM-Service.reg" `
    /y

if ($LASTEXITCODE -ne 0) {
    throw 'AdskNLM 服务注册表备份失败，已停止。'
}
```

### 16.2 删除孤立服务

```powershell
Stop-Service -Name 'AdskNLM' -Force -ErrorAction SilentlyContinue
Set-Service -Name 'AdskNLM' -StartupType Disabled -ErrorAction SilentlyContinue

sc.exe delete AdskNLM
```

正常输出：

```text
[SC] DeleteService SUCCESS
```

重启后验证：

```powershell
[PSCustomObject]@{
    ServiceExists =
        $null -ne (
            Get-Service -Name 'AdskNLM' -ErrorAction SilentlyContinue
        )

    ServiceRegistryExists =
        Test-Path 'HKLM:\SYSTEM\CurrentControlSet\Services\AdskNLM'

    NlmFolderExists =
        Test-Path 'C:\Program Files (x86)\Common Files\Autodesk Shared\Network License Manager'
} | Format-List
```

本次结果：

```text
ServiceExists         : False
ServiceRegistryExists : False
NlmFolderExists       : False
```

---

## 17. 第十一阶段：最后卸载 Genuine Service

Autodesk 官方全量卸载流程要求先卸载其他 Autodesk 软件，Genuine Service 留到最后。

本次 ProductCode：

```text
{D207E870-6397-417E-B7DD-720BFBE589A3}
```

执行：

```powershell
$p = Start-Process `
    -FilePath "$env:SystemRoot\System32\msiexec.exe" `
    -ArgumentList '/x {D207E870-6397-417E-B7DD-720BFBE589A3} /L*v "D:\AutodeskCleanupLogs\GenuineService-final-uninstall.log"' `
    -Wait `
    -PassThru

Write-Host "Genuine Service 卸载退出码：$($p.ExitCode)"
```

本次结果：

```text
Genuine Service 卸载退出码：0
```

验证安装登记：

```powershell
Get-ItemProperty $uninstallPaths -ErrorAction SilentlyContinue |
Where-Object {
    $_.DisplayName -eq 'Autodesk Genuine Service'
} |
Select-Object DisplayName, DisplayVersion, PSChildName |
Format-Table -AutoSize
```

验证服务：

```powershell
Get-CimInstance Win32_Service -ErrorAction SilentlyContinue |
Where-Object {
    $_.DisplayName -match 'Autodesk.*Genuine' -or
    $_.PathName -match 'Autodesk\\Genuine Service'
} |
Select-Object Name, DisplayName, State, StartMode, PathName |
Format-List
```

两者均无输出。

---

## 18. 第十二阶段：删除标准残留目录

标准目标：

```powershell
$targets = @(
    'C:\Program Files\Autodesk',
    'C:\Program Files\Common Files\Autodesk Shared',
    'C:\Program Files (x86)\Autodesk',
    'C:\Program Files (x86)\Common Files\Autodesk Shared',
    'C:\ProgramData\Autodesk',
    "$env:LOCALAPPDATA\Autodesk",
    "$env:APPDATA\Autodesk"
)
```

普通删除：

```powershell
$results = foreach ($folder in $targets) {
    $status = '本来就不存在'
    $errorMessage = ''

    if (Test-Path -LiteralPath $folder) {
        try {
            Remove-Item `
                -LiteralPath $folder `
                -Recurse `
                -Force `
                -ErrorAction Stop

            $status = '已删除'
        }
        catch {
            $status = '删除失败'
            $errorMessage = $_.Exception.Message
        }
    }

    [PSCustomObject]@{
        Path   = $folder
        Result = $status
        Error  = $errorMessage
    }
}

$results | Format-Table -Wrap -AutoSize
```

本次第一次结果：

```text
C:\Program Files\Autodesk                         权限拒绝
C:\Program Files\Common Files\Autodesk Shared   权限拒绝
%LOCALAPPDATA%\Autodesk                           cer.log 被占用
其他目录                                            已删除或原本不存在
```

### 18.1 只对 Autodesk 专属目录修复权限

```powershell
$systemFolders = @(
    'C:\Program Files\Autodesk',
    'C:\Program Files\Common Files\Autodesk Shared'
)

foreach ($folder in $systemFolders) {
    if (Test-Path -LiteralPath $folder) {
        & takeown.exe /F $folder /A /R /D Y

        & icacls.exe `
            $folder `
            /grant '*S-1-5-32-544:(OI)(CI)F' `
            /T `
            /C
    }
}
```

`*S-1-5-32-544` 是本机 Administrators 组 SID，避免中英文系统组名差异。

> [!WARNING]
> 只对明确的 Autodesk 目录操作。不要对整个 `C:\Program Files` 或整个系统盘递归接管权限。

---

## 19. 第十三阶段：处理被 Explorer 占用的 AcSignCore16.dll

最后只剩：

```text
C:\Program Files\Common Files\Autodesk Shared\AcSignCore16.dll
```

查询加载进程：

```powershell
tasklist.exe /m AcSignCore16.dll
```

结果：

```text
explorer.exe    AcSignCore16.dll
```

### 19.1 为什么结束 Explorer 后仍可能失败

Explorer 被结束后，Windows 可能自动重启它，并再次加载 DLL。即使 `takeown` 和 `icacls` 成功，也可能因为模块重新加载而继续报拒绝访问。

### 19.2 不成功的方法：MoveFileEx 重启删除

尝试通过 `MoveFileEx(..., MOVEFILE_DELAY_UNTIL_REBOOT)` 登记重启删除，返回：

```text
Windows 错误码：3
已登记 0 个项目
```

错误码 3 表示路径未找到；本次没有成功登记，所以不能把“已登记 0 个项目”误认为成功。

### 19.3 最终成功方法：Windows 恢复环境离线删除

由于系统盘启用了 BitLocker，先检查并保存恢复密钥：

```powershell
manage-bde -status C:
manage-bde -protectors -get C:
```

> 不要把 48 位恢复密钥上传到 GitHub、聊天或截图。

进入 Windows 恢复环境：

```powershell
shutdown.exe /r /o /t 0
```

蓝色页面：

```text
疑难解答
→ 高级选项
→ 命令提示符
```

确认 Windows 盘符：

```cmd
for %d in (C D E F) do @if exist %d:\Windows\System32\cmd.exe echo Windows=%d:
```

假设 Windows 在 `C:`：

```cmd
dir /a "C:\Program Files\Common Files\Autodesk Shared"
```

确认路径后离线删除：

```cmd
rmdir /s /q "C:\Program Files\Common Files\Autodesk Shared"
```

验证：

```cmd
if exist "C:\Program Files\Common Files\Autodesk Shared" (echo STILL_EXISTS) else (echo DELETED_SUCCESSFULLY)
```

进入正常 Windows 后：

```powershell
[PSCustomObject]@{
    SharedFolderExists =
        Test-Path 'C:\Program Files\Common Files\Autodesk Shared'

    AcSignCoreExists =
        Test-Path 'C:\Program Files\Common Files\Autodesk Shared\AcSignCore16.dll'
} | Format-List

tasklist.exe /m AcSignCore16.dll
```

本次最终结果：

```text
SharedFolderExists : False
AcSignCoreExists   : False
INFO: No tasks are running which match the specified criteria.
```

---

## 20. 第十四阶段：备份并删除 Autodesk 主注册表键

确认所有 Autodesk 产品、服务和主要目录都已移除后，再处理主键。

目标：

```text
HKLM\SOFTWARE\Autodesk
HKCU\SOFTWARE\Autodesk
HKLM\SOFTWARE\WOW6432Node\Autodesk
```

脚本：

```powershell
$ErrorActionPreference = 'Stop'

$stamp = Get-Date -Format 'yyyyMMdd-HHmmss'
$backupFolder =
    "D:\AutodeskCleanupBackup\FinalRegistry-$stamp"

New-Item `
    -ItemType Directory `
    -Path $backupFolder `
    -Force |
Out-Null

$targets = @(
    [PSCustomObject]@{
        PSPath = 'HKLM:\SOFTWARE\Autodesk'
        NativePath = 'HKEY_LOCAL_MACHINE\SOFTWARE\Autodesk'
        BackupName = 'HKLM-SOFTWARE-Autodesk.reg'
    },
    [PSCustomObject]@{
        PSPath = 'HKCU:\SOFTWARE\Autodesk'
        NativePath = 'HKEY_CURRENT_USER\SOFTWARE\Autodesk'
        BackupName = 'HKCU-SOFTWARE-Autodesk.reg'
    },
    [PSCustomObject]@{
        PSPath = 'HKLM:\SOFTWARE\WOW6432Node\Autodesk'
        NativePath = 'HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Autodesk'
        BackupName = 'HKLM-WOW6432Node-Autodesk.reg'
    }
)

$results = foreach ($target in $targets) {
    if (-not (Test-Path -LiteralPath $target.PSPath)) {
        [PSCustomObject]@{
            RegistryPath = $target.PSPath
            Backup       = '不需要'
            Result       = '本来就不存在'
        }

        continue
    }

    $backupFile = Join-Path $backupFolder $target.BackupName

    & reg.exe export `
        $target.NativePath `
        $backupFile `
        /y

    if ($LASTEXITCODE -ne 0) {
        throw "注册表备份失败，停止删除：$($target.NativePath)"
    }

    if (-not (Test-Path -LiteralPath $backupFile)) {
        throw "没有检测到备份文件，停止删除：$backupFile"
    }

    Remove-Item `
        -LiteralPath $target.PSPath `
        -Recurse `
        -Force `
        -ErrorAction Stop

    [PSCustomObject]@{
        RegistryPath = $target.PSPath
        Backup       = $backupFile
        Result       = '已备份并删除'
    }
}

$results | Format-Table -Wrap -AutoSize
Write-Host "`n注册表备份目录：$backupFolder"
```

验证：

```powershell
@(
    'HKLM:\SOFTWARE\Autodesk',
    'HKCU:\SOFTWARE\Autodesk',
    'HKLM:\SOFTWARE\WOW6432Node\Autodesk'
) |
ForEach-Object {
    [PSCustomObject]@{
        RegistryPath = $_
        Exists       = Test-Path -LiteralPath $_
    }
} |
Format-Table -AutoSize
```

本次结果全部为 `False`。

---

## 21. 第十五阶段：删除 C:\Autodesk 安装缓存

检查：

```powershell
$cacheFolder = 'C:\Autodesk'

Get-ChildItem `
    -LiteralPath $cacheFolder `
    -Force `
    -ErrorAction SilentlyContinue |
Select-Object Name, FullName, Mode, LastWriteTime |
Format-Table -Wrap -AutoSize
```

本次只剩：

```text
C:\Autodesk\IM
C:\Autodesk\WI
```

它们是安装文件仓库，没有 NLM 许可文件，也没有用户项目。

删除：

```powershell
Remove-Item `
    -LiteralPath 'C:\Autodesk' `
    -Recurse `
    -Force `
    -ErrorAction Stop
```

验证：

```powershell
[PSCustomObject]@{
    AutodeskCacheFolderExists =
        Test-Path -LiteralPath 'C:\Autodesk'

    IMExists =
        Test-Path -LiteralPath 'C:\Autodesk\IM'

    WIExists =
        Test-Path -LiteralPath 'C:\Autodesk\WI'
} | Format-List
```

本次结果全部为 `False`。

---

## 22. 最终全量审计

下面脚本只读取系统状态：

```powershell
Write-Host "`n=== 1. Autodesk 安装登记 ==="

$uninstallPaths = @(
    'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*',
    'HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*',
    'HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*'
)

Get-ItemProperty $uninstallPaths -ErrorAction SilentlyContinue |
Where-Object {
    $_.Publisher -match '^Autodesk' -or
    $_.DisplayName -match 'Autodesk|AutoCAD|Inventor|Fusion|Adsk'
} |
Select-Object DisplayName, DisplayVersion, Publisher, PSChildName |
Sort-Object DisplayName |
Format-Table -AutoSize


Write-Host "`n=== 2. Autodesk 服务 ==="

Get-CimInstance Win32_Service -ErrorAction SilentlyContinue |
Where-Object {
    $_.Name -match 'Autodesk|Adsk' -or
    $_.DisplayName -match 'Autodesk|Adsk' -or
    $_.PathName -match 'Autodesk|Adsk'
} |
Select-Object Name, DisplayName, State, StartMode, PathName |
Format-Table -Wrap -AutoSize


Write-Host "`n=== 3. Autodesk 运行进程 ==="

Get-CimInstance Win32_Process -ErrorAction SilentlyContinue |
Where-Object {
    $_.Name -match 'Autodesk|Adsk|Inventor|acad|Fusion' -or
    $_.ExecutablePath -match '\\Autodesk\\'
} |
Select-Object ProcessId, Name, ExecutablePath |
Format-Table -Wrap -AutoSize


Write-Host "`n=== 4. Autodesk 计划任务 ==="

Get-ScheduledTask -ErrorAction SilentlyContinue |
Where-Object {
    $_.TaskName -match 'Autodesk|Adsk' -or
    $_.TaskPath -match 'Autodesk|Adsk'
} |
Select-Object TaskName, TaskPath, State |
Format-Table -AutoSize


Write-Host "`n=== 5. 标准目录 ==="

$folders = @(
    'C:\Program Files\Autodesk',
    'C:\Program Files\Common Files\Autodesk Shared',
    'C:\Program Files (x86)\Autodesk',
    'C:\Program Files (x86)\Common Files\Autodesk Shared',
    'C:\ProgramData\Autodesk',
    "$env:LOCALAPPDATA\Autodesk",
    "$env:APPDATA\Autodesk",
    'C:\Autodesk'
)

$folders | ForEach-Object {
    [PSCustomObject]@{
        Path   = $_
        Exists = Test-Path -LiteralPath $_
    }
} | Format-Table -AutoSize


Write-Host "`n=== 6. Autodesk 主注册表键 ==="

@(
    'HKLM:\SOFTWARE\Autodesk',
    'HKCU:\SOFTWARE\Autodesk',
    'HKLM:\SOFTWARE\WOW6432Node\Autodesk'
) | ForEach-Object {
    [PSCustomObject]@{
        RegistryPath = $_
        Exists       = Test-Path -LiteralPath $_
    }
} | Format-Table -AutoSize


Write-Host "`n=== 7. FLEXnet Autodesk 文件 ==="

Get-ChildItem 'C:\ProgramData\FLEXnet' `
    -Force `
    -ErrorAction SilentlyContinue |
Where-Object {
    $_.Name -like 'adsk*'
} |
Select-Object Name, FullName |
Format-Table -AutoSize
```

### 合格标准

- 第 1～4 部分无输出；
- 第 5 部分全部 `False`；
- 第 6 部分全部 `False`；
- 第 7 部分无输出。

### 本次最终审计结果

```text
安装登记：无
服务：无
运行进程：无
计划任务：无
8 个标准目录：全部 False
3 个 Autodesk 主注册表键：全部 False
FLEXnet adsk* 文件：无
```

---

## 23. 本次实机涉及的产品代码

> [!IMPORTANT]
> 下表用于记录本次实机。不同版本、语言包和安装方式可能使用不同 GUID，不要直接复制到其他电脑执行。

| 项目 | 标识 | 类型 / 备注 |
|---|---|---|
| Inventor 2026 | `{7F4DD591-3064-0001-0000-7107D70F3DB4}` | MSI ProductCode，原始 MSI 丢失，返回 1612 |
| REX Inventor | `{B2285C37-5FC0-445A-A7D0-7F331481DD56}` | MSI，正常卸载 |
| AutoCAD 2025 语言/MSI 条目 | `{28B89EEF-8101-0804-2102-CF3F3A09B77D}` | 残留条目 |
| ACAD Private 2025 | `{28B89EEF-8101-0000-3102-CF3F3A09B77D}` | MSI，正常卸载 |
| ACAD Private 2026 | `{28B89EEF-9101-0000-3102-CF3F3A09B77D}` | MSI，正常卸载 |
| Save to Web and Mobile | `{5BF7551A-09A6-4BDD-AE1E-048104FB478C}` | MSI，正常卸载 |
| App Manager | `{0FC454BE-3FA9-40A2-B5F1-C15B3CE740AA}` | MSI，正常卸载 |
| Interoperability Engine Manager | `{217D7134-F441-3B94-8AAB-63175C9228A9}` | MSI，正常卸载 |
| CER | `{C2895666-EC78-4E43-A590-A744CEDD756A}` | MSI，正常卸载 |
| REX Framework | `{D29C8D32-C8E0-42A8-AA21-71A4C17B6ACD}` | MSI，正常卸载 |
| RSA Engine | `{E4BDBBF7-4F7D-44F9-825C-93610182835A}` | MSI，正常卸载 |
| Genuine Service | `{D207E870-6397-417E-B7DD-720BFBE589A3}` | MSI，最后卸载 |
| Autodesk Access | `{A3158B3E-5F28-358A-BF1A-9532D8EBC811}` | 本机卸载登记 / ODIS 标识，不应默认当作 MSI ProductCode |
| AutoCAD 2026 ODIS | `{9165B4AF-8BF4-37D6-882D-694B73603AED}` | ODIS 记录 |
| Inventor 2024 ODIS | `{6A358503-3AE7-35FF-BDC5-5B39F75B2E88}` | ODIS 记录 |

---

## 24. Windows Installer 常见退出码

| 退出码 | 含义 | 本次处理原则 |
|---:|---|---|
| 0 | 成功 | 继续验证卸载登记、目录和服务 |
| 3010 | 成功，需要重启 | 保存工作并重启 |
| 1603 | 严重错误 | 停止，检查 MSI 日志 |
| 1605 | 当前产品未安装 | 常见于成功后重复运行；检查登记是否已消失 |
| 1612 | 安装源不可用 | 原始 MSI 丢失；优先微软工具或修复安装源 |

查询命令日志：

```powershell
Select-String `
    -Path 'D:\AutodeskCleanupLogs\目标日志.log' `
    -Pattern 'Return value 3|error|failed' `
    -CaseSensitive:$false
```

---

## 25. 本次踩坑与经验

### 25.1 退出码 0 不一定等于真实成功

Licensing Service 的卸载器返回 0，但界面提示 `uninstall.dat` 缺失。必须检查：

- 文件夹是否消失；
- 服务是否消失；
- 卸载登记是否消失；
- GUI 是否弹出错误。

### 25.2 `Write-Host` 不能证明上一条命令成功

错误示例：

```powershell
Remove-Item $folder -ErrorAction Stop
Write-Host '目录已删除。'
```

如果用户在交互式 PowerShell 中继续单独执行第二行，它仍会打印“已删除”，即使前一条已经报错。

真正验证必须使用：

```powershell
Test-Path $folder
```

### 25.3 不要重复运行成功的卸载命令

首次 `0` 后重复执行常返回 `1605`。重复操作只会增加混乱。

### 25.4 `Format-Table` 会截断关键信息

涉及 GUID、路径和卸载命令时使用：

```powershell
Format-List
```

### 25.5 关键词正则可能误匹配系统组件

`RSA` 匹配到了 `Universal`。涉及卸载时应优先使用精确名称：

```powershell
$_.DisplayName -eq 'RSA Engine'
```

或：

```powershell
$_.DisplayName -in @('REX Framework', 'RSA Engine')
```

### 25.6 ODIS 主产品与 MSI 子组件不是同一种卸载路径

ODIS 主产品通常应由 ODIS Installer 触发卸载；只有部分 MSI 子组件可以使用 `msiexec /x`。

### 25.7 文件被占用时，权限修复不一定有效

`AcSignCore16.dll` 已经成功接管所有权并授予管理员完全控制，但仍因被 Explorer 加载而无法删除。最终使用恢复环境离线删除，而不是继续扩大权限范围。

### 25.8 恢复环境盘符可能变化

WinRE 中 Windows 不一定仍是 `C:`，删除前必须检查：

```cmd
for %d in (C D E F) do @if exist %d:\Windows\System32\cmd.exe echo Windows=%d:
```

---

## 26. 绝对不要做的事

```text
[禁止] 删除 C:\Windows\Installer
[禁止] 删除整个 HKEY_CLASSES_ROOT\Installer
[禁止] 删除整个 C:\Program Files
[禁止] 删除整个 C:\ProgramData
[禁止] 删除整个 C:\ProgramData\FLEXnet
[禁止] 因为名字包含 Universal 就卸载 Windows SDK 组件
[禁止] 把 ODIS GUID 当 MSI ProductCode 猜测执行
[禁止] 未备份就删除 Autodesk 主注册表键
[禁止] 未确认服务路径就执行 sc delete
[禁止] 把 BitLocker 恢复密钥上传到 GitHub
[禁止] 在还保留其他 Autodesk 产品时执行全量清理
[禁止] 使用第三方注册表清理器批量删除未知键
```

Microsoft Visual C++、.NET、WebView2、Windows SDK 等共享组件不要因为 Autodesk 已卸载就一起删除。

---

## 27. 清理完成后的重新安装建议

完成最终审计后，可以重新从 Autodesk 官网下载产品。

建议：

1. 先重启一次；
2. 只从 Autodesk Account 或 Autodesk 官网下载；
3. 不使用以前残缺的 MSI、ODIS 缓存或旧安装包；
4. 优先选择先完整下载再安装；
5. 第一次只安装一个主产品；
6. 安装期间不要清理 `%TEMP%`、`C:\Autodesk` 或 ODIS 目录；
7. 安装完成后测试：启动、登录、创建文件、保存、关闭、再次启动；
8. 确认稳定后再安装下一个产品。

重新安装时以下组件再次出现属于正常现象：

- Autodesk Access；
- ODIS；
- Autodesk Identity Manager；
- Autodesk Desktop Licensing Service；
- Autodesk Genuine Service；
- 产品共享组件。

备份和日志建议保留到新产品稳定运行一段时间后：

```text
D:\AutodeskCleanupBackup
D:\AutodeskCleanupLogs
```

不要导入备份的 `.reg`，除非明确需要回滚某一项操作。

---

## 28. 最终结果

本次最终验证：

```text
Autodesk 安装登记              无
Autodesk 服务                  无
Autodesk 运行进程              无
Autodesk 计划任务              无
C:\Program Files\Autodesk      False
Autodesk Shared                False
ProgramData\Autodesk           False
LocalAppData\Autodesk          False
RoamingAppData\Autodesk        False
C:\Autodesk                    False
HKLM\SOFTWARE\Autodesk        False
HKCU\SOFTWARE\Autodesk        False
WOW6432Node\Autodesk           False
FLEXnet 中 adsk* 文件          无
```

结论：

> 在“已安装产品、Windows Installer 登记、服务、进程、计划任务、标准文件目录、Autodesk 主注册表键和 Autodesk 许可缓存”的范围内，本机 Autodesk 全量干净卸载已经完成，可以重新从 Autodesk 官网安装产品。

---

## License / 转载说明

本文是一次真实故障排查记录。可以在注明来源和风险提示的前提下修改、转载或用于个人维护文档。

执行任何注册表、服务和离线删除操作前，请根据自己的系统状态重新核对路径、产品名称和 ProductCode。

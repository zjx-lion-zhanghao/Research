# Windows C 盘结构与安全清理指南

> 适用于 Windows 10 / Windows 11  
> 目标：理解 C 盘里“每一类东西是什么”，再决定能不能删，而不是看到大文件就手动删除。

---

## 目录

- [1. C 盘到底是什么](#1-c-盘到底是什么)
- [2. 为什么资源管理器看到的不是全部内容](#2-为什么资源管理器看到的不是全部内容)
- [3. C 盘根目录常见内容](#3-c-盘根目录常见内容)
- [4. Users 与 AppData 是什么](#4-users-与-appdata-是什么)
- [5. Program Files、ProgramData 和 Windows 的区别](#5-program-filesprogramdata-和-windows-的区别)
- [6. 隐藏系统文件分别有什么用](#6-隐藏系统文件分别有什么用)
- [7. 哪些内容可以删、条件删除、绝对不能删](#7-哪些内容可以删条件删除绝对不能删)
- [8. 为什么“直接删软件文件夹”会出问题](#8-为什么直接删软件文件夹会出问题)
- [9. 一套安全的 C 盘清理流程](#9-一套安全的-c-盘清理流程)
- [10. PowerShell 只读审计脚本](#10-powershell-只读审计脚本)
- [11. 常用缓存的正确清理方法](#11-常用缓存的正确清理方法)
- [12. 常见误区](#12-常见误区)
- [13. 长期维护建议](#13-长期维护建议)
- [14. 清理核对表](#14-清理核对表)
- [15. 参考资料](#15-参考资料)

---

# 1. C 盘到底是什么

很多人把 C 盘理解成“Windows 的一个大文件夹”，这是不准确的。

Windows 存储可以分成四层：

```text
物理硬盘 / SSD
└─ 分区
   └─ 文件系统（通常为 NTFS）
      └─ 根目录与文件夹
```

例如：

```text
一块 1TB NVMe SSD
├─ EFI 启动分区
├─ C: 系统分区
├─ D: 数据分区
└─ Recovery / OEM 恢复分区
```

因此：

- `C:` 和 `D:` 不一定是两块硬盘；
- 它们可能只是同一块 SSD 上的两个分区；
- 把文件从 C 盘移动到 D 盘，只是改变分区占用；
- 如果 C、D 在同一块 SSD 上，D 盘不能算作 C 盘数据的真正备份。

## 一个最容易混淆的例子

```text
C:\D
```

它只是 **C 盘根目录下名为 D 的普通文件夹**，并不是真正的：

```text
D:\
```

判断方法：

- `C:\D`：C 盘里的文件夹；
- `D:\`：D 分区的根目录。

这两个路径完全不同。

---

# 2. 为什么资源管理器看到的不是全部内容

资源管理器默认会隐藏一部分内容：

- 隐藏文件；
- 受保护的操作系统文件；
- 系统还原点；
- NTFS 文件系统元数据；
- 无盘符的 EFI、Recovery、OEM 分区；
- 因权限不足而无法进入的目录；
- Junction、符号链接和其他重解析点。

所以：

> “文件夹看起来是空的”并不等于里面真的什么都没有。

例如，一个目录可能只剩：

```text
desktop.ini
~$某个文档.docx
```

资源管理器默认隐藏它们，但 PowerShell 使用 `-Force` 后可以看到。

## 为什么 WizTree、资源管理器和 PowerShell 的统计可能不同

常见原因包括：

1. **隐藏或系统文件未显示**；
2. **权限不足导致扫描漏项**；
3. **Junction 被重复遍历或被跳过**；
4. **WinSxS 使用硬链接，简单相加可能重复统计**；
5. **稀疏文件、压缩文件、虚拟磁盘的逻辑大小和实际占用不同**；
6. **系统还原点和卷影副本位于 `System Volume Information` 中**。

因此，大文件分析工具适合定位方向，但不能把每个“大目录”直接等同于垃圾。

---

# 3. C 盘根目录常见内容

## 3.1 正常系统目录

| 路径 | 作用 | 处理原则 |
|---|---|---|
| `C:\Windows` | Windows 系统主体 | 禁止手动删除 |
| `C:\Program Files` | 64 位程序的默认安装目录 | 通过卸载器处理 |
| `C:\Program Files (x86)` | 32 位程序的默认安装目录 | 通过卸载器处理 |
| `C:\ProgramData` | 所有用户共享的软件数据、缓存和配置 | 禁止整体删除 |
| `C:\Users` | 用户账户、个人文件和 AppData | 只能整理自己的数据 |
| `C:\Recovery` | Windows 恢复环境相关内容 | 保留 |

## 3.2 可能出现的系统或安装残留

| 路径 | 可能来源 | 是否可删 |
|---|---|---|
| `C:\$SysReset` | “重置此电脑”日志或残留 | 重置早已完成后可核对删除 |
| `C:\Windows.old` | 上一个 Windows 版本 | 通过“存储/磁盘清理”删除 |
| `C:\$WINDOWS.~BT` | Windows 升级文件 | 通过系统清理工具处理 |
| `C:\$Windows.~WS` | Windows 安装准备文件 | 通过系统清理工具处理 |
| `C:\$WinREAgent` | 更新或恢复过程文件 | 更新完成前不要动 |
| `C:\Config.Msi` | MSI 安装/卸载回滚数据 | 安装结束并确认稳定后再判断 |
| 随机字符串目录 | 驱动、更新、安装器解压目录 | 先识别来源，不能仅凭名字删除 |

## 3.3 厂商目录

根目录可能出现：

```text
C:\NVIDIA
C:\AMD
C:\Intel
C:\ASUS
C:\Drivers
C:\eSupport
C:\SWSetup
```

其中有些只是驱动安装包解压缓存，有些可能是 OEM 恢复和驱动资源。

判断原则：

- `C:\NVIDIA`、`C:\AMD` 如果只是旧驱动安装包，通常可以核对后删除；
- `eSupport`、`Drivers`、`ASUS`、`System.sav` 等目录，在没有制作恢复介质和备份驱动前不要删除；
- `Program Files\NVIDIA Corporation` 不是普通安装缓存，不能手动删除。

---

# 4. Users 与 AppData 是什么

## 4.1 `C:\Users\用户名`

这是当前用户的个人主目录，常见内容包括：

```text
Desktop
Downloads
Documents
Pictures
Videos
Music
AppData
.ssh
.vscode
.gradle
.rustup
.cargo
.nuget
```

其中只有个人文件目录适合直接整理：

```text
Downloads
Documents
Pictures
Videos
Desktop
```

## 4.2 AppData 的三层结构

```text
C:\Users\用户名\AppData
├─ Local
├─ LocalLow
└─ Roaming
```

### Local

保存：

- 浏览器缓存；
- 软件本机缓存；
- 临时文件；
- 崩溃转储；
- 本地数据库；
- 大型模型或下载缓存。

### LocalLow

通常用于权限更低的程序或兼容组件。

### Roaming

保存：

- 用户配置；
- 软件设置；
- 扩展配置；
- 登录状态；
- 部分聊天和应用数据。

> `AppData` 不是“垃圾目录”。它混合了缓存、配置、数据库和用户状态，绝对不能整体删除。

## 4.3 用户目录中的隐藏开发环境

常见目录：

| 目录 | 用途 | 正确处理 |
|---|---|---|
| `.vscode` | VS Code 扩展和配置 | 不要直接整删 |
| `.ssh` | SSH 密钥和主机记录 | 重要数据，必须备份 |
| `.gradle` | Gradle 缓存和配置 | 可清缓存，保留配置 |
| `.nuget` | NuGet 本地包缓存 | 用官方命令清理 |
| `.rustup`、`.cargo` | Rust 工具链和缓存 | 不用 Rust 时用 `rustup self uninstall` |
| `.ollama` | Ollama 本地模型与配置 | 模型可能很大，先确认用途 |
| `.docker` | Docker 配置 | 真正的大数据可能位于 WSL/VHDX 中 |
| `.cache` | 多软件共用缓存区 | 必须拆解后处理 |

## 4.4 Junction：为什么有些文件夹显示 0 字节

用户目录中常见：

```text
Application Data
Local Settings
My Documents
Recent
SendTo
Templates
Cookies
```

它们通常不是普通文件夹，而是为了兼容旧程序保留的 **Junction（目录联接点）**。

它们本质上是另一个目录的别名。删除或强行遍历可能造成：

- 重复统计；
- 无限递归；
- 权限错误；
- 旧程序兼容性问题。

所以不要删除这些 0 字节“文件夹”。

---

# 5. Program Files、ProgramData 和 Windows 的区别

## 5.1 Program Files

用于安装程序本体，例如：

```text
C:\Program Files\Microsoft Visual Studio
C:\Program Files\Adobe
C:\Program Files\Autodesk
```

软件的路径可能被写入：

- 注册表；
- PATH；
- 文件关联；
- 服务；
- 计划任务；
- 快捷方式；
- 更新器；
- 卸载信息。

因此不能把软件目录直接剪切到 D 盘。

## 5.2 ProgramData

保存多个用户共享的数据，例如：

- 软件缓存；
- 安装缓存；
- 公共数据库；
- 授权信息；
- 服务配置；
- 更新文件。

`ProgramData` 不能整体删除。即使其中某个子目录很大，也必须先确认属于哪个软件。

## 5.3 Windows

`C:\Windows` 中包括：

```text
System32
WinSxS
Installer
servicing
SoftwareDistribution
DriverStore
Temp
Logs
```

处理原则：

- `System32`：绝对不能手动删除；
- `WinSxS`：使用 DISM 清理；
- `Installer`：保留，否则软件无法修复、更新或卸载；
- `DriverStore`：使用驱动管理工具处理；
- `SoftwareDistribution`：优先使用 Windows 存储和磁盘清理；
- `Temp`：可以通过系统工具清理可删除项。

---

# 6. 隐藏系统文件分别有什么用

| 路径 | 作用 | 处理建议 |
|---|---|---|
| `C:\pagefile.sys` | 页面文件、提交内存和崩溃转储支持 | 保持系统管理，禁止手删 |
| `C:\swapfile.sys` | Windows 管理的交换文件 | 禁止手删 |
| `C:\hiberfil.sys` | 休眠和快速启动 | 只有关闭休眠后才会自动移除 |
| `C:\DumpStack.log` | 启动与转储辅助日志 | 保留，通常极小 |
| `C:\$Recycle.Bin` | 每个分区的回收站 | 清空内容，不删目录本身 |
| `C:\System Volume Information` | 系统还原点、卷影副本和卷信息 | 通过系统保护管理 |
| `C:\Documents and Settings` | 指向 `C:\Users` 的兼容联接点 | 绝对不能删 |
| `C:\bootmgr`、`C:\Boot` | Windows 启动文件 | 绝对不能删 |

## 6.1 页面文件为什么不建议关闭

即使电脑有较大内存，页面文件仍可能用于：

- 扩展系统提交上限；
- 支持大型软件和峰值负载；
- 生成系统崩溃转储；
- 提高部分程序兼容性。

普通用户最稳妥的设置是：

```text
系统管理的大小
```

## 6.2 休眠文件可以删吗

不能直接删除 `hiberfil.sys`。

只有明确不需要休眠和快速启动时，才使用管理员终端：

```powershell
powercfg.exe /hibernate off
```

恢复：

```powershell
powercfg.exe /hibernate on
```

## 6.3 NTFS 元数据

NTFS 还维护很多不会被资源管理器正常展示的结构，例如：

```text
$Mft
$MftMirr
$LogFile
$Bitmap
$Boot
$BadClus
$Secure
$Extend
$UsnJrnl
```

这些不是垃圾文件，而是文件系统本身。绝对不能修改、清空或删除。

---

# 7. 哪些内容可以删、条件删除、绝对不能删

## 7.1 通常可以安全清理

- 回收站中确认无用的内容；
- `Downloads` 中已经安装完成的软件安装包；
- 确认重复的 ISO、ZIP、RAR；
- `AppData\Local\Temp` 中能正常删除的临时文件；
- `AppData\Local\CrashDumps` 中不再分析的崩溃转储；
- pip、NuGet、Gradle、Scoop 等工具的可重建缓存；
- 已确认无用的个人视频、模型、数据集和压缩包；
- 旧的桌面临时锁文件，例如 `~$文档.docx`；
- 已经结束很久的 `$SysReset` 日志目录。

## 7.2 满足条件后再删除

- `Windows.old`：确认不需要回退旧版本；
- `$WINDOWS.~BT`：升级完成且不需要回滚；
- `hiberfil.sys`：明确关闭休眠后；
- 旧系统还原点：完成清理、重启并测试稳定后；
- `OneDriveTemp`：同步完成并退出 OneDrive；
- 根目录随机字符串目录：确认属于废弃安装器；
- NVIDIA/AMD 根目录缓存：确认只是安装包解压目录；
- `.gradle\caches`、`.nuget`、pip 缓存：确定可以重新下载；
- `.ollama` 模型：确定不再使用对应本地模型；
- 聊天软件图片和视频：先确认是否有唯一资料。

## 7.3 绝对不要手动删除

```text
C:\Windows
C:\Windows\System32
C:\Windows\WinSxS
C:\Windows\Installer
C:\Windows\System32\DriverStore
C:\Program Files
C:\Program Files (x86)
C:\ProgramData
C:\Recovery
C:\Users 整体
AppData 整体
pagefile.sys
swapfile.sys
System Volume Information
Documents and Settings
NTUSER.DAT
EFI / MSR / Recovery 分区
NTFS 的 $Mft、$Bitmap、$LogFile 等元数据
```

---

# 8. 为什么“直接删软件文件夹”会出问题

软件安装不只是复制文件。

安装程序通常还会修改：

```text
Program Files 中的程序文件
ProgramData 中的共享数据
AppData 中的用户配置
注册表卸载登记
Windows 服务
计划任务
PATH 环境变量
开始菜单快捷方式
文件关联
授权服务
共享 DLL 与运行库
MSI / Package Cache
```

如果直接删除软件目录，可能造成：

- 软件文件消失，但 Windows 仍认为它已安装；
- 卸载器无法启动；
- 重新安装时提示“已安装”；
- MSI 缓存丢失；
- 服务和 PATH 指向不存在的文件；
- 开始菜单留下失效快捷方式；
- 同一套软件出现多个“幽灵登记”。

正确顺序：

```text
确认用户数据
→ 使用官方卸载器
→ 重启
→ 检查服务、启动项、PATH 和快捷方式
→ 最后处理明确失效的残留登记
```

---

# 9. 一套安全的 C 盘清理流程

## 第 0 阶段：先做准备

- 备份重要文档和项目；
- 备份 BitLocker 恢复密钥；
- 记录常用软件；
- 清理期间不要同时卸载多个大型软件家族；
- 不要对整个 C 盘执行 `takeown` 或 `icacls`。

## 第 1 阶段：看清空间结构

优先观察：

```text
C:\Users\当前用户
C:\Program Files
C:\Program Files (x86)
C:\ProgramData
C:\Windows
C:\System Volume Information
C:\$Recycle.Bin
C 盘根目录中的自建目录
```

## 第 2 阶段：个人文件

先处理：

- Downloads；
- Desktop；
- Videos；
- 重复 ISO；
- 安装包；
- 旧项目压缩包；
- 已归档课程资料；
- 不再使用的模型和数据集。

## 第 3 阶段：应用缓存

使用软件自己的功能或官方命令清理：

- 浏览器缓存；
- QQ、微信、飞书等聊天软件存储管理；
- pip、NuGet、Gradle、Scoop 缓存；
- CrashDumps 和 Temp；
- Adobe、IDE 和编译工具缓存。

## 第 4 阶段：卸载不用的软件

进入：

```text
设置 → 应用 → 已安装的应用 → 按大小排序
```

每次只处理一个软件：

1. 确认是否仍使用；
2. 备份项目和数据；
3. 正常卸载；
4. 重启；
5. 测试系统；
6. 再检查残留。

## 第 5 阶段：Windows 官方清理

使用：

```text
设置 → 系统 → 存储 → 临时文件
设置 → 系统 → 存储 → 清理建议
磁盘清理 → 清理系统文件
```

管理员终端可执行：

```powershell
Dism.exe /Online /Cleanup-Image /StartComponentCleanup
```

不要随意添加 `/ResetBase`，因为它会降低卸载旧更新的能力。

## 第 6 阶段：系统还原点

完成清理后：

1. 重启；
2. 测试驱动、软件和开发环境；
3. 稳定运行几天；
4. 再删除旧还原点；
5. 创建新的“系统治理完成”还原点。

---

# 10. PowerShell 只读审计脚本

以下命令只用于查看，不会删除文件。

## 10.1 查看 C 盘容量

```powershell
Get-CimInstance Win32_LogicalDisk -Filter "DeviceID='C:'" |
    Select-Object DeviceID,
        @{Name='TotalGB';Expression={[math]::Round($_.Size / 1GB, 2)}},
        @{Name='UsedGB';Expression={[math]::Round(($_.Size - $_.FreeSpace) / 1GB, 2)}},
        @{Name='FreeGB';Expression={[math]::Round($_.FreeSpace / 1GB, 2)}},
        @{Name='FreePercent';Expression={[math]::Round($_.FreeSpace / $_.Size * 100, 1)}}
```

## 10.2 显示 C 盘根目录中的隐藏和系统项目

```powershell
Get-ChildItem 'C:\' -Force |
    Select-Object Name, Attributes, LinkType, Target, LastWriteTime |
    Format-Table -AutoSize
```

## 10.3 确认桌面真实路径

```powershell
[PSCustomObject]@{
    EnvironmentDesktop = [Environment]::GetFolderPath('Desktop')
    UserShellFolders    = (
        Get-ItemProperty `
        'HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders'
    ).Desktop
    ShellFolders        = (
        Get-ItemProperty `
        'HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders'
    ).Desktop
} | Format-List
```

正常桌面路径通常为：

```text
C:\Users\用户名\Desktop
```

## 10.4 安全统计用户目录，跳过 Junction

```powershell
function Get-SafeFolderSize {
    param([Parameter(Mandatory)][string]$Path)

    $bytes = [int64]0
    $count = [int64]0
    $stack = New-Object 'System.Collections.Generic.Stack[string]'
    $stack.Push($Path)

    while ($stack.Count -gt 0) {
        $current = $stack.Pop()

        try {
            foreach ($file in [System.IO.Directory]::EnumerateFiles($current)) {
                try {
                    $info = [System.IO.FileInfo]::new($file)
                    $bytes += $info.Length
                    $count++
                }
                catch {}
            }

            foreach ($directory in [System.IO.Directory]::EnumerateDirectories($current)) {
                try {
                    $info = [System.IO.DirectoryInfo]::new($directory)

                    if (
                        ($info.Attributes -band
                        [System.IO.FileAttributes]::ReparsePoint) -eq 0
                    ) {
                        $stack.Push($directory)
                    }
                }
                catch {}
            }
        }
        catch {}
    }

    [PSCustomObject]@{
        Name   = Split-Path $Path -Leaf
        SizeGB = [math]::Round($bytes / 1GB, 3)
        Files  = $count
        Path   = $Path
    }
}

Get-ChildItem $env:USERPROFILE -Directory -Force |
    Where-Object {
        ($_.Attributes -band [System.IO.FileAttributes]::ReparsePoint) -eq 0
    } |
    ForEach-Object {
        Get-SafeFolderSize -Path $_.FullName
    } |
    Sort-Object SizeGB -Descending |
    Format-Table -AutoSize
```

## 10.5 扫描 AppData 一级目录

```powershell
$appDataRoots = @(
    "$env:LOCALAPPDATA",
    "$env:APPDATA",
    "$env:USERPROFILE\AppData\LocalLow"
)

$rows = foreach ($root in $appDataRoots) {
    if (Test-Path -LiteralPath $root) {
        Get-ChildItem -LiteralPath $root -Directory -Force |
            Where-Object {
                ($_.Attributes -band
                [System.IO.FileAttributes]::ReparsePoint) -eq 0
            } |
            ForEach-Object {
                Get-SafeFolderSize -Path $_.FullName
            }
    }
}

$rows |
    Sort-Object SizeGB -Descending |
    Select-Object -First 40 |
    Format-Table -AutoSize
```

## 10.6 查看页面文件、还原点和恢复环境

```powershell
Get-CimInstance Win32_PageFileUsage |
    Select-Object Name, AllocatedBaseSize, CurrentUsage, PeakUsage

vssadmin list shadowstorage

reagentc /info
```

## 10.7 查看全部分区，包括隐藏分区

```powershell
Get-Partition |
    Sort-Object DiskNumber, PartitionNumber |
    Select-Object DiskNumber, PartitionNumber, DriveLetter,
        Type,
        @{Name='SizeGB';Expression={[math]::Round($_.Size / 1GB, 3)}},
        IsSystem,
        IsBoot,
        IsHidden,
        IsReadOnly |
    Format-Table -AutoSize
```

---

# 11. 常用缓存的正确清理方法

## pip

```powershell
python -m pip cache info
python -m pip cache purge
```

不会删除已安装的 Python 包，只删除下载缓存。

## NuGet

```powershell
dotnet nuget locals all --clear
```

## Scoop

```powershell
scoop cleanup *
scoop cache rm *
```

## Gradle

关闭 Android Studio 和 Gradle 构建后，可清理：

```powershell
Remove-Item "$env:USERPROFILE\.gradle\caches" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$env:USERPROFILE\.gradle\daemon" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$env:USERPROFILE\.gradle\wrapper\dists" -Recurse -Force -ErrorAction SilentlyContinue
```

不要删除：

```text
.gradle\gradle.properties
.gradle\init.gradle
```

## 程序崩溃转储

```powershell
Remove-Item "$env:LOCALAPPDATA\CrashDumps\*" -Force -ErrorAction SilentlyContinue
```

## 临时文件

先关闭应用，再执行：

```powershell
Get-ChildItem "$env:LOCALAPPDATA\Temp" -Force |
    Remove-Item -Recurse -Force -ErrorAction SilentlyContinue
```

被占用的文件会自动跳过，属于正常现象。

## 回收站

先人工检查，再清空：

```powershell
Clear-RecycleBin -Force
```

该命令不可恢复，执行前必须确认回收站中没有重要内容。

---

# 12. 常见误区

## 误区 1：文件夹大就是垃圾

错误。大型目录可能是：

- 软件本体；
- 聊天记录；
- 数据集；
- 虚拟环境；
- Windows 组件；
- 系统还原点；
- WSL 虚拟磁盘。

正确做法是先识别用途。

## 误区 2：`C:\D` 就是 D 盘

错误。它只是 C 盘里的普通文件夹。

## 误区 3：内存大就能关闭 pagefile

错误。页面文件仍参与系统提交上限、崩溃转储和软件兼容性。普通用户应保持系统管理。

## 误区 4：AppData 都是缓存

错误。AppData 还可能保存：

- 账号状态；
- 软件数据库；
- 聊天记录；
- 配置；
- 扩展；
- 用户模板。

## 误区 5：开始菜单有条目，就一定完整安装了软件

不一定。开始菜单条目可能是：

- 普通快捷方式；
- Microsoft Store 应用；
- Office/Adobe 套件内部组件；
- 下载占位入口；
- 已卸载软件留下的失效快捷方式。

## 误区 6：控制面板能列出全部软件

不能。Windows 软件可能来自：

- 传统 Win32/MSI；
- Microsoft Store / MSIX / AppX；
- 套件内部组件；
- 便携软件；
- 命令行包管理器；
- WSL 内部软件。

## 误区 7：把项目移动到 D 盘一定没影响

普通文档通常没问题，但项目可能包含：

- `.venv`；
- `.git`；
- 绝对路径配置；
- IDE 工作区；
- 编译器路径；
- 数据集软链接。

项目应整体移动，并验证能否重新运行。

---

# 13. 长期维护建议

## C 盘职责

建议只放：

- Windows；
- 正式安装的软件；
- 必须位于用户目录的配置；
- 少量临时工作文件。

## 数据盘职责

建议建立：

```text
D:\
├─ Projects
├─ Coursework
├─ Documents
├─ Data
│  ├─ Datasets
│  ├─ Models
│  ├─ Logs
│  └─ Media
├─ Installers
├─ Archive
├─ AppsPortable
└─ Inbox
```

`Inbox` 用于临时接收文件，定期清空。

## 每周

- 清理 Downloads；
- 整理桌面；
- 检查回收站；
- 提交 Git 代码；
- 把临时文件归档。

## 每月

- 查看“设置 → 系统 → 存储”；
- 查看已安装应用大小；
- 检查 AppData 缓存；
- 检查 WSL、Docker、虚拟机；
- 清理聊天软件下载目录；
- 更新软件资产清单。

## 每学期或每个大项目结束

- 归档项目；
- 删除可重建虚拟环境；
- 导出依赖；
- 整理数据集和模型；
- 备份 WSL；
- 创建系统还原点或系统镜像。

---

# 14. 清理核对表

```text
[ ] 确认 C、D 是否位于同一块物理硬盘
[ ] 查看 C 盘总容量、已用和剩余空间
[ ] 显示并识别 C 盘隐藏根目录项目
[ ] 检查 Downloads、Desktop、Videos
[ ] 检查回收站
[ ] 扫描 AppData 一级目录
[ ] 清理 pip / NuGet / Gradle / Scoop 缓存
[ ] 清理 CrashDumps 和 Temp
[ ] 按大小检查已安装应用
[ ] 正常卸载不用的软件
[ ] 使用 Windows“存储”和“清理建议”
[ ] 运行 DISM StartComponentCleanup
[ ] 保留 pagefile.sys
[ ] 不手动删除 WinSxS、Installer、DriverStore
[ ] 不删除 Junction 和 NTFS 元数据
[ ] 重启并测试常用软件
[ ] 稳定后处理旧还原点
[ ] 创建新的系统还原点
[ ] 备份重要项目和个人文件
```

---

# 15. 参考资料

- [Microsoft Support：Windows 中的存储设置](https://support.microsoft.com/en-us/windows/experience/storage-filemanagement/storage-settings-in-windows)
- [Microsoft Support：使用存储感知管理驱动器空间](https://support.microsoft.com/zh-cn/windows/%E4%BD%BF%E7%94%A8%E5%AD%98%E5%82%A8%E6%84%9F%E7%9F%A5%E7%AE%A1%E7%90%86%E9%A9%B1%E5%8A%A8%E5%99%A8%E7%A9%BA%E9%97%B4-654f6ada-7bfc-45e5-966b-e24aded96ad5)
- [Microsoft Learn：如何确定 64 位 Windows 的合适页面文件大小](https://learn.microsoft.com/zh-cn/troubleshoot/windows-client/performance/how-to-determine-the-appropriate-page-file-size-for-64-bit-versions-of-windows)
- [Microsoft Learn：清理 WinSxS 文件夹](https://learn.microsoft.com/zh-cn/windows-hardware/manufacture/desktop/clean-up-the-winsxs-folder?view=windows-11)
- [Microsoft Learn / Sysinternals：Junction](https://learn.microsoft.com/zh-cn/sysinternals/downloads/junction)
- [Microsoft Learn：fsutil reparsepoint](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/fsutil-reparsepoint)

---

## 结语

Windows C 盘治理的目标不是“越空越好”，而是：

> 每一个大型目录都知道用途；每一个软件都知道为什么安装；每一个开发环境都能够重建；每一份重要数据都有备份；每一次卸载都可验证、可恢复。

真正安全的清理流程永远是：

```text
先识别
→ 再备份
→ 再迁移或卸载
→ 最后删除
```

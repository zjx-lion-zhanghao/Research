# Windows 账户、Microsoft 账户、Office 许可证与 BitLocker 基础指南

> 适合对 Windows 账户体系、Microsoft 365、OneDrive、Outlook 与磁盘加密概念不熟悉的用户。  
> 本文按“身份 → 软件 → 许可证 → 云服务 → 设备加密”的顺序梳理各部分关系。

---

## 目录

- [1. 总体关系](#1-总体关系)
- [2. Windows 用户是什么](#2-windows-用户是什么)
- [3. 本地账户与 Microsoft 账户](#3-本地账户与-microsoft-账户)
- [4. “安装、登录、激活”是三件不同的事](#4-安装登录激活是三件不同的事)
- [5. Microsoft 365 与 Office](#5-microsoft-365-与-office)
- [6. OneDrive 与 Outlook](#6-onedrive-与-outlook)
- [7. 个人账户、学校账户与工作账户](#7-个人账户学校账户与工作账户)
- [8. 切换到本地账户会发生什么](#8-切换到本地账户会发生什么)
- [9. BitLocker 与资源管理器中的锁图标](#9-bitlocker-与资源管理器中的锁图标)
- [10. 如何读取 `manage-bde -status`](#10-如何读取-manage-bde--status)
- [11. C 盘和 D 盘是如何自动解锁的](#11-c-盘和-d-盘是如何自动解锁的)
- [12. 恢复密钥的重要性](#12-恢复密钥的重要性)
- [13. 推荐的个人账户结构](#13-推荐的个人账户结构)
- [14. 安全操作清单](#14-安全操作清单)
- [15. 常见误区](#15-常见误区)

---

# 1. 总体关系

先看一张总图：

```text
一台 Windows 电脑
│
├─ Windows 操作系统
│  ├─ Windows 用户
│  │  ├─ 桌面、文档、下载
│  │  ├─ 用户设置
│  │  └─ 管理员或标准用户权限
│  │
│  ├─ 安装的软件
│  │  ├─ Word
│  │  ├─ PowerPoint
│  │  ├─ OneDrive
│  │  └─ Outlook
│  │
│  └─ 磁盘加密
│     └─ BitLocker / 设备加密
│
└─ 在线账户
   ├─ 个人 Microsoft 账户
   ├─ 工作或学校账户
   ├─ 163 / QQ / Gmail 邮箱账户
   └─ 其他软件自己的账户
```

最重要的两个概念：

> **账户负责证明“你是谁”。**

> **许可证负责证明“你是否有权使用某个软件或服务”。**

因此，登录了 Microsoft 账户，并不等于自动拥有正版 Word 和 PowerPoint。

---

# 2. Windows 用户是什么

Windows 用户是这台电脑内部的使用身份。

每个 Windows 用户通常都有独立的：

```text
桌面
文档
下载
图片
视频
AppData
浏览器配置
软件个人设置
```

典型用户目录：

```text
C:\Users\<用户名>
```

一台电脑可以有多个 Windows 用户。例如：

```text
用户 A：管理员
用户 B：普通用户
用户 C：访客
```

不同用户登录后，看到的桌面、文件和部分软件配置可能不同。

## 2.1 管理员与标准用户

### 管理员

通常可以：

- 安装和卸载软件；
- 修改系统设置；
- 管理其他 Windows 用户；
- 以管理员身份运行 PowerShell；
- 修改服务、驱动和安全设置。

### 标准用户

通常可以：

- 使用已安装的软件；
- 管理自己的个人文件；
- 修改自己的用户设置；
- 但不能随意修改系统级配置。

管理员权限不代表 Office 正版，也不代表 Microsoft 账户拥有任何订阅。

---

# 3. 本地账户与 Microsoft 账户

## 3.1 本地账户

本地账户只存在于当前这台电脑上。

```text
本地用户名：zjx
本地密码：仅由这台电脑验证
```

特点：

- 不依赖互联网；
- 不依赖 Microsoft 服务器；
- 换一台电脑不会自动出现；
- Windows 设置不会自动跨设备同步；
- 忘记密码时不能依赖 Microsoft 在线找回。

可以把它理解为：

> 只属于这一台电脑的本地门禁卡。

---

## 3.2 Microsoft 账户

Microsoft 账户是存在于微软服务器上的在线身份。

可以用于：

- Windows 登录；
- Microsoft Store；
- OneDrive；
- Microsoft 365；
- Edge 同步；
- Xbox；
- 设备与恢复密钥管理。

Microsoft 账户的登录名不一定是 Outlook 邮箱，也可以是：

```text
example@163.com
example@qq.com
example@gmail.com
example@outlook.com
```

因此，一个 163 邮箱地址可以同时具有两种身份：

```text
作为 163 邮箱账户：
用于收发邮件，由网易管理

作为 Microsoft 账户登录名：
用于登录微软服务，由微软管理
```

两者即使使用同一个邮箱地址，也仍是两个独立账户体系，密码也可以不同。

---

## 3.3 Windows 用户与 Microsoft 账户的绑定关系

当前 Windows 用户可以使用 Microsoft 账户作为登录方式：

```text
Windows 用户：卓某
        │
        └─ 使用 Microsoft 账户验证身份
           example@163.com
```

也可以把同一个 Windows 用户切换成本地账户：

```text
Windows 用户：卓某
        │
        └─ 改由本机用户名和密码验证
```

通常这只是改变登录方式，不是新建一套空白桌面。

---

# 4. “安装、登录、激活”是三件不同的事

以 Word 为例：

## 4.1 安装

把 Word 程序文件放入电脑。

典型位置：

```text
C:\Program Files\Microsoft Office
```

安装完成只代表：

> 电脑中存在 Word 程序。

不代表它已经获得合法许可证。

---

## 4.2 登录

在 Word 中登录一个 Microsoft 账户或学校账户。

登录后可能获得：

- 用户头像；
- 最近文档；
- OneDrive 文件；
- 个性化设置；
- 许可证信息。

但登录账户本身不一定拥有 Office 权益。

---

## 4.3 激活

Office 会检查：

> 当前账户、设备或组织是否拥有合法使用该软件的许可证。

许可证可能来自：

- Microsoft 365 个人订阅；
- 一次性购买的 Office；
- 电脑出厂赠送；
- 学校提供；
- 公司提供；
- 组织批量授权。

因此：

```text
Word 已安装
≠ Word 已登录
≠ Word 已合法激活
```

---

# 5. Microsoft 365 与 Office

## 5.1 Office

Office 是办公软件家族，常见成员包括：

```text
Word
Excel
PowerPoint
Outlook
OneNote
Access（部分版本）
```

过去常见版本：

```text
Office 2019
Office 2021
Office 2024
```

这类通常更接近一次性购买的固定版本。

---

## 5.2 Microsoft 365

Microsoft 365 是订阅服务。

通常可能包含：

```text
Word
Excel
PowerPoint
Outlook
OneDrive 云存储
持续功能更新
多设备使用
```

可理解为：

```text
Microsoft 365
└─ Office 软件 + 云服务 + 订阅权益
```

是否包含桌面版 Office，取决于具体订阅或学校分配的许可证类型。

---

## 5.3 如何检查 Office 许可证

打开 Word：

```text
文件
→ 账户
```

重点查看：

- 产品名称；
- 是否显示“产品已激活”；
- 是否显示“未经许可的产品”；
- 当前登录邮箱；
- 许可证属于哪个账户；
- 是否出现 Microsoft 365、家庭和学生版、Professional Plus、LTSC、Volume 等字样。

不要只看“已激活”三个字，还需要结合软件来源和许可证归属判断是否合法。

---

# 6. OneDrive 与 Outlook

## 6.1 OneDrive

OneDrive 包含两部分：

### 本地同步客户端

安装在电脑中，用于上传、下载和同步文件。

### 云端存储服务

属于某个 Microsoft 账户或学校账户。

典型本地目录：

```text
C:\Users\<用户名>\OneDrive
```

OneDrive 可能接管：

```text
桌面
文档
图片
```

开启前需要确认文件路径和同步策略，否则容易出现：

- 桌面位置改变；
- 文件带云朵或绿色对勾；
- 本地删除同步到云端；
- 换电脑后文件重新出现；
- 退出 OneDrive 后部分文件看起来消失。

OneDrive 通常不使用“盗版”这个概念，更应关注：

- 客户端是否来自官方；
- 登录的是哪个账户；
- 云端数据归谁管理；
- 是否开启了自动备份。

---

## 6.2 Outlook

“Outlook”可能指三种不同的东西：

### Outlook.com 邮箱

例如：

```text
example@outlook.com
example@hotmail.com
```

这是微软提供的邮箱服务。

### Outlook 邮件客户端

可以收取多种邮箱：

```text
163 邮箱
QQ 邮箱
Gmail
Outlook 邮箱
学校邮箱
公司邮箱
```

### Office 中的经典 Outlook

属于 Office 或 Microsoft 365 套件的一部分，许可证通常与 Word、PowerPoint 相关。

所以：

```text
Outlook 软件
≠ Outlook 邮箱
```

---

# 7. 个人账户、学校账户与工作账户

## 7.1 个人 Microsoft 账户

通常用于：

- Windows 登录；
- Microsoft Store；
- 个人 OneDrive；
- 个人 Microsoft 365；
- Edge 同步；
- Xbox。

---

## 7.2 工作或学校账户

由学校或公司统一创建和管理。

常见形式：

```text
studentID@school.edu
studentID@tenant.onmicrosoft.com
name@company.com
```

学校或公司可以决定账户具有什么权益：

- Word、Excel、PowerPoint 网页版；
- 桌面版 Microsoft 365；
- OneDrive；
- Teams；
- Exchange 邮箱；
- SharePoint。

组织也可以：

- 重置密码；
- 收回许可证；
- 停用账户；
- 限制登录；
- 删除组织云端数据。

因此，重要个人文件不应只保存在学校 OneDrive。

---

## 7.3 “仅登录应用”和“管理整台设备”

在个人电脑登录学校账户时，Windows 可能询问：

```text
是否允许组织管理此设备
```

两种选择含义不同：

### 仅登录此应用

```text
学校账户只登录 Word、PowerPoint 或当前应用
```

适合大多数个人电脑。

### 允许组织管理此设备

可能把电脑注册到学校的：

```text
Microsoft Entra ID
MDM
Intune
设备合规管理
```

学校管理员可能获得部分设备管理能力。

对于个人电脑，除非学校明确要求并提供正式说明，一般不要主动把整台设备加入学校管理。

---

# 8. 切换到本地账户会发生什么

切换前：

```text
Windows 用户：卓某
登录身份：个人 Microsoft 账户
登录名：example@163.com
```

切换后：

```text
Windows 用户：仍是卓某
登录身份：本地账户
本地用户名：zjx 或其他名称
本地密码：新设置的密码
```

原来的 Microsoft 账户仍然存在：

```text
example@163.com
```

它仍可继续用于：

- OneDrive；
- Microsoft Store；
- Word；
- Edge；
- Microsoft 账户网页。

## 8.1 通常不会发生的事

一般不会：

- 删除桌面；
- 删除下载和文档；
- 卸载软件；
- 删除 Microsoft 账户；
- 自动重命名 `C:\Users\<旧目录>`；
- 恢复出厂设置。

本质上只是：

```text
以前：由 Microsoft 在线账户验证 Windows 登录
以后：由当前电脑本地验证用户名和密码
```

---

## 8.2 切换前为什么提示备份恢复密钥

如果系统启用了 BitLocker，Microsoft 账户可能保存着恢复密钥。

切换到本地账户不会自动关闭 BitLocker，但系统会提醒：

> 先确认恢复密钥已经备份，避免未来硬件或启动环境异常时无法解锁磁盘。

这是安全提醒，不代表切换会删除数据。

---

# 9. BitLocker 与资源管理器中的锁图标

BitLocker 是 Windows 的磁盘加密功能。

它保护的是：

> 电脑关机、磁盘被拆走或系统无法正常启动时，别人不能直接读取磁盘数据。

资源管理器中的锁图标：

| 图标状态 | 含义 |
|---|---|
| 没有锁 | 通常表示未启用 BitLocker |
| 打开的锁 | 已加密，但当前已解锁，可以正常访问 |
| 关闭的锁 | 已加密，目前锁定 |
| 锁旁有警告 | 保护可能暂停或配置不完整 |

因此：

> 打开的锁不代表“没有加密”。

它表示：

```text
磁盘已加密
+
当前 Windows 已成功解锁
```

---

# 10. 如何读取 `manage-bde -status`

使用只读命令：

```powershell
manage-bde -status
```

典型输出：

```text
Volume C: [OS]
    BitLocker Version:    2.0
    Conversion Status:    Used Space Only Encrypted
    Percentage Encrypted: 100.0%
    Encryption Method:    XTS-AES 128
    Protection Status:    Protection On
    Lock Status:          Unlocked
    Key Protectors:
        Numerical Password
        TPM
```

逐项解释：

## `BitLocker Version: 2.0`

表示使用 BitLocker 2.0 格式。

## `Used Space Only Encrypted`

启用时只加密当时已使用的空间。

之后新写入的数据仍会自动加密。

## `Percentage Encrypted: 100.0%`

当前加密转换已完成。

这和“只加密已用空间”并不矛盾：

```text
加密模式：仅加密已使用空间
当前进度：100%
```

## `Encryption Method: XTS-AES 128`

使用 XTS-AES 128 位磁盘加密算法。

## `Protection Status: Protection On`

BitLocker 保护正在生效。

## `Lock Status: Unlocked`

当前系统已成功解锁该卷，可以正常读写。

## `Numerical Password`

存在一份 48 位数字恢复密钥。

## `TPM`

系统盘可以通过主板上的 TPM 安全芯片自动解锁。

---

# 11. C 盘和 D 盘是如何自动解锁的

## 11.1 C 盘

典型状态：

```text
Key Protectors:
    Numerical Password
    TPM
```

启动流程：

```text
电脑关机
│
├─ C 盘数据保持加密
│
└─ 开机
   ├─ TPM 检查启动环境
   ├─ 检查通过
   ├─ TPM 自动释放解锁材料
   └─ C 盘解锁，Windows 启动
```

因此进入桌面后，C 盘显示打开的锁是正常现象。

---

## 11.2 D 盘

典型状态：

```text
Automatic Unlock: Enabled

Key Protectors:
    Numerical Password
    External Key
```

启动流程：

```text
TPM 解锁 C 盘
        ↓
Windows 启动
        ↓
Windows 读取保存在系统中的 D 盘自动解锁信息
        ↓
D 盘自动解锁
```

`External Key` 在这里通常不是指必须插着某个 U 盘，而是与自动解锁机制有关。

---

## 11.3 C、D 盘恢复密钥可能不同

即使 C、D 位于同一块物理 SSD，它们仍可能是两个独立 BitLocker 卷。

因此：

```text
C 盘恢复密钥
≠
D 盘恢复密钥
```

必须分别备份。

---

# 12. 恢复密钥的重要性

BitLocker 恢复密钥通常是一串 48 位数字。

在以下情况中可能需要：

- TPM 状态变化；
- BIOS/UEFI 更新或重置；
- 安全启动配置改变；
- 主板更换；
- 引导文件异常；
- 系统无法正常启动；
- 把硬盘接到另一台电脑；
- 自动解锁信息损坏。

## 12.1 推荐备份方式

至少保留两份：

```text
第一份：保存到个人 Microsoft 账户
第二份：保存到 U 盘、外接硬盘或纸质文件
```

不要只保存到正在被加密的同一块内部 SSD。

因为 C、D 盘位于同一物理硬盘：

```text
C 盘副本 + D 盘副本
≠ 真正独立备份
```

---

## 12.2 不要公开恢复密钥

以下命令会显示敏感信息：

```powershell
manage-bde -protectors -get C:
manage-bde -protectors -get D:
```

不要把完整输出：

- 上传到 GitHub；
- 发到群聊；
- 发到论坛；
- 发给陌生人；
- 截图公开。

GitHub 文档中应只写命令，不应包含真实密钥、密钥 ID、设备序列号、个人邮箱和学号。

---

# 13. 推荐的个人账户结构

推荐保持职责分离：

```text
Windows 开机登录
└─ 个人 Microsoft 账户
   或本地账户

个人 OneDrive
└─ 个人 Microsoft 账户

Word / PowerPoint
└─ 个人许可证账户
   或学校账户

学校 OneDrive / Teams
└─ 学校账户

Outlook 邮件客户端
├─ 163 邮箱
├─ QQ 邮箱
└─ 学校邮箱
```

核心原则：

```text
个人账户管理个人电脑与个人数据
学校账户只使用学校提供的权益
```

不要把学校账户作为个人电脑的主要 Windows 登录账户，也不要把所有个人资料只存学校云盘。

---

# 14. 安全操作清单

## 账户方面

- [ ] 确认个人 Microsoft 账户密码可以正常登录
- [ ] 确认绑定邮箱和手机号仍可使用
- [ ] 区分个人 Microsoft 账户与学校账户
- [ ] 不把学校账户加入整台设备管理，除非学校明确要求
- [ ] 在 Word 中查看真实许可证归属
- [ ] 不运行来源不明的 Office 激活工具

## OneDrive 方面

- [ ] 查看当前登录的是哪个账户
- [ ] 确认是否开启桌面、文档、图片备份
- [ ] 不在不理解路径变化时随意开启文件夹备份
- [ ] 重要文件保留本地与外部备份

## BitLocker 方面

- [ ] 使用 `manage-bde -status` 检查状态
- [ ] 确认 C 盘 `Protection On`
- [ ] 确认 D 盘自动解锁状态
- [ ] 分别备份 C、D 两个卷的恢复密钥
- [ ] 将恢复密钥保存在独立设备
- [ ] 不把恢复密钥上传 GitHub
- [ ] 不为追求“锁图标关闭”而手动锁系统盘
- [ ] 不随意关闭 BitLocker

---

# 15. 常见误区

## 误区 1：登录 Microsoft 账户就拥有正版 Office

错误。

```text
Microsoft 账户
≠ Office 许可证
```

必须单独检查订阅或许可证。

---

## 误区 2：163 邮箱注册 Microsoft 账户后就变成 Outlook 邮箱

错误。

```text
163 邮箱仍由网易管理
Microsoft 账户只是使用该地址作为登录名
```

---

## 误区 3：设置中出现两个 Microsoft 账户，就代表电脑有两个 Windows 用户

错误。

“应用使用的账户”与“Windows 登录用户”不是同一概念。

---

## 误区 4：切换本地账户会删除原来的 Microsoft 账户

错误。

原 Microsoft 账户仍然存在，只是不再负责 Windows 开机登录。

---

## 误区 5：打开的锁表示 BitLocker 没有生效

错误。

打开的锁表示：

```text
已加密 + 当前已解锁
```

---

## 误区 6：C、D 盘都在一块硬盘上，所以只有一个恢复密钥

错误。

每个 BitLocker 卷都可能有独立恢复密钥。

---

## 误区 7：D 盘自动解锁说明 D 盘没有加密

错误。

自动解锁只是让系统在登录后自动提供解锁材料，数据仍处于加密保护之下。

---

# 总结

Windows 账户体系可以归纳为：

```text
Windows 用户
├─ 决定谁在使用这台电脑
├─ 可以是本地账户登录
└─ 也可以绑定 Microsoft 账户登录

在线账户
├─ Microsoft 账户
├─ 学校账户
└─ 邮箱账户

软件
├─ 安装
├─ 登录
└─ 激活

BitLocker
├─ 加密磁盘数据
├─ TPM 自动解锁系统盘
├─ Windows 自动解锁数据盘
└─ 恢复密钥负责应急解锁
```

最终应达到的状态：

> 每个账户都知道用途，每个软件都知道许可证来源，每个云盘都知道数据归属，每个加密卷都有独立恢复密钥备份。

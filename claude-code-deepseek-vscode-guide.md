# Claude Code 接入 DeepSeek API 到 VS Code：Windows 与 macOS 从 0 开始教程

> 适用场景：在 VS Code 中使用 Claude Code 插件，并让 Claude Code 通过 DeepSeek API 作为后端模型来辅助阅读、解释、修改和生成代码。  
> 本文档不包含任何真实 API Key，可以直接上传到 GitHub。

---

## 0. 概念说明

本文中的缩写：

```text
CC = Claude Code
DS = DeepSeek
VS = VS Code
API Key = 模型平台提供的接口调用密钥
```

需要先分清楚几个容易混淆的东西：

| 名称 | 作用 | 默认使用的服务 | 能否直接填 DeepSeek Key |
|---|---|---|---|
| Claude Code 插件 | VS Code 里的 AI 编程助手 | Anthropic / Claude | 可以通过配置接入 DeepSeek |
| Codex 插件 | OpenAI 的 AI 编程助手 | OpenAI | 通常不能直接填 DeepSeek Key |
| DeepSeek API | 大模型接口服务 | DeepSeek | 这是本文要接入的服务 |
| 项目自己的 AI 功能 | 网站或系统内部调用 AI | 由代码决定 | 可以在后端接入 DeepSeek |

本文要实现的是：

```text
VS Code
  ↓
Claude Code 插件
  ↓
读取本机 Claude Code 配置
  ↓
请求 DeepSeek 的 Anthropic 兼容接口
  ↓
DeepSeek 模型返回结果
  ↓
Claude Code 在 VS Code 中辅助写代码
```

---

## 1. 最重要的安全提醒

API Key 本质上相当于你的“模型账户密码”。

不要这样做：

```text
1. 不要把真实 API Key 写进 README.md。
2. 不要把真实 API Key 写进 docs 文档。
3. 不要把真实 API Key 写进 .py、.js、.html、.css 文件。
4. 不要把真实 API Key 写进前端代码。
5. 不要把真实 API Key 上传 GitHub。
6. 不要把真实 API Key 发给别人。
7. 不要截图公开真实 API Key。
```

本文所有示例里的：

```text
your_deepseek_api_key_here
```

都只是占位符，不是真实密钥。

如果真实 Key 已经泄露，应该立刻：

```text
1. 去 DeepSeek 平台删除旧 API Key。
2. 重新生成新的 API Key。
3. 更新本机配置文件。
4. 检查 GitHub 历史提交中是否包含旧 Key。
```

---

## 2. 接入原理

Claude Code 默认是 Anthropic / Claude 的工具。

DeepSeek 提供了兼容 Anthropic API 格式的接口，因此可以让 Claude Code 请求 DeepSeek：

```text
ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
ANTHROPIC_AUTH_TOKEN=你的 DeepSeek API Key
ANTHROPIC_MODEL=deepseek-v4-pro[1m]
```

从操作系统角度理解：

```text
settings.json / 环境变量
  ↓
给 Claude Code 进程提供运行时配置
  ↓
Claude Code 根据配置发起 HTTPS 请求
```

从计算机网络角度理解：

```text
Claude Code 客户端
  ↓ HTTPS
DeepSeek API 服务器
  ↓ HTTPS Response
Claude Code 客户端
```

---

## 3. 使用前准备

Windows 和 macOS 都需要准备：

```text
1. VS Code
2. Claude Code for VS Code 插件
3. Node.js 18 或更高版本
4. Claude Code CLI
5. DeepSeek API Key
6. 一个需要 AI 辅助开发的项目文件夹
```

Windows 还建议安装：

```text
Git for Windows
```

macOS 一般使用系统自带 Terminal，也可以使用 iTerm2。

---

## 4. 申请 DeepSeek API Key

操作步骤：

```text
1. 打开 DeepSeek API 平台。
2. 注册或登录账号。
3. 进入控制台。
4. 找到 API Keys / API 密钥 / 密钥管理 页面。
5. 点击 Create API Key / 创建密钥。
6. 复制生成的 API Key。
7. 检查账户余额或可用额度。
```

生成的 Key 通常类似：

```text
sk-xxxxxxxxxxxxxxxxxxxxxxxx
```

请不要把真实 Key 写进本教程或 GitHub 仓库。

---

## 5. 安装 VS Code

### 5.1 Windows

安装 VS Code 时建议勾选：

```text
Add to PATH
Open with Code
Register Code as an editor
```

安装完成后，打开 PowerShell，测试：

```powershell
code --version
```

如果能显示版本号，说明 `code` 命令可用。

### 5.2 macOS

安装 VS Code 后，如果终端无法使用 `code` 命令，可以在 VS Code 中执行：

```text
Command + Shift + P
```

搜索：

```text
Shell Command: Install 'code' command in PATH
```

执行后重新打开终端，测试：

```bash
code --version
```

---

## 6. 安装 Node.js

Claude Code 需要 Node.js 环境。

### 6.1 检查 Node.js

Windows PowerShell 或 macOS Terminal 中执行：

```bash
node -v
npm -v
```

如果显示类似：

```text
v20.x.x
10.x.x
```

说明 Node.js 和 npm 已经可用。

### 6.2 如果没有安装

安装 Node.js 18 或更高版本，推荐 Node.js 20 LTS。

安装完成后，关闭终端并重新打开，再执行：

```bash
node -v
npm -v
```

---

## 7. 安装 Git

### 7.1 Windows

建议安装 Git for Windows。

安装后在 PowerShell 中检查：

```powershell
git --version
```

### 7.2 macOS

在 Terminal 中执行：

```bash
git --version
```

如果系统提示安装 Command Line Tools，按提示安装即可。

---

## 8. 安装 Claude Code CLI

虽然我们主要在 VS Code 插件里使用 Claude Code，但建议同时安装 CLI，因为 CLI 更方便测试配置是否正确。

### 8.1 Windows

在 PowerShell 中执行：

```powershell
npm install -g @anthropic-ai/claude-code
```

检查：

```powershell
claude --version
```

### 8.2 macOS

在 Terminal 中执行：

```bash
npm install -g @anthropic-ai/claude-code
```

检查：

```bash
claude --version
```

### 8.3 常见错误：claude command not found

如果出现：

```text
claude: command not found
```

可能原因：

```text
1. npm 全局安装路径没有加入 PATH。
2. Node.js 安装不完整。
3. 安装后没有重新打开终端。
4. npm install -g 执行失败。
```

可以先执行：

```bash
npm config get prefix
```

然后检查 npm 全局命令目录是否在 PATH 中。

---

## 9. 安装 Claude Code VS Code 插件

打开 VS Code，进入扩展市场：

```text
Extensions
```

搜索：

```text
Claude Code
```

安装 Claude Code 插件。

安装完成后，VS Code 侧边栏或右侧面板通常会出现：

```text
CLAUDE CODE
```

注意不要点错：

```text
如果看到 CODEX，那是 OpenAI Codex 插件。
如果要使用本文的 DeepSeek 配置，请使用 CLAUDE CODE。
```

---

## 10. 推荐配置方式：用户级 settings.json

推荐把配置写到用户级 Claude Code 配置文件，而不是写在项目目录里。

原因：

```text
1. 不会污染项目代码。
2. 不容易误上传 GitHub。
3. VS Code 插件和 Claude Code CLI 都可以读取。
4. Windows 和 macOS 都适用。
```

配置文件位置：

### Windows

```text
C:\Users\你的用户名\.claude\settings.json
```

也可以写成：

```text
%USERPROFILE%\.claude\settings.json
```

### macOS

```text
/Users/你的用户名/.claude/settings.json
```

也可以写成：

```text
~/.claude/settings.json
```

---

## 11. macOS 配置步骤

### 11.1 创建配置目录

打开 Terminal 或 VS Code 终端：

```bash
mkdir -p ~/.claude
```

### 11.2 用 VS Code 打开配置文件

```bash
code ~/.claude/settings.json
```

如果提示：

```text
code: command not found
```

回到 VS Code，执行：

```text
Command + Shift + P
```

搜索并执行：

```text
Shell Command: Install 'code' command in PATH
```

然后重新打开终端，再执行：

```bash
code ~/.claude/settings.json
```

### 11.3 写入配置

把下面内容复制到 `~/.claude/settings.json`：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "your_deepseek_api_key_here",
    "ANTHROPIC_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash",
    "CLAUDE_CODE_SUBAGENT_MODEL": "deepseek-v4-flash",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1",
    "CLAUDE_CODE_EFFORT_LEVEL": "max"
  }
}
```

然后把：

```json
"ANTHROPIC_AUTH_TOKEN": "your_deepseek_api_key_here"
```

改成你的真实 DeepSeek API Key。

保存：

```text
Command + S
```

---

## 12. Windows 配置步骤

### 12.1 创建配置目录

打开 PowerShell：

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude"
```

### 12.2 用 VS Code 打开配置文件

```powershell
code "$env:USERPROFILE\.claude\settings.json"
```

如果 `code` 命令不可用，可以用记事本：

```powershell
notepad "$env:USERPROFILE\.claude\settings.json"
```

或者在 VS Code 中手动打开：

```text
File
Open File
C:\Users\你的用户名\.claude\settings.json
```

如果看不到 `.claude` 文件夹，需要开启显示隐藏文件。

### 12.3 写入配置

把下面内容复制到 `settings.json`：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "your_deepseek_api_key_here",
    "ANTHROPIC_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash",
    "CLAUDE_CODE_SUBAGENT_MODEL": "deepseek-v4-flash",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1",
    "CLAUDE_CODE_EFFORT_LEVEL": "max"
  }
}
```

然后把：

```json
"ANTHROPIC_AUTH_TOKEN": "your_deepseek_api_key_here"
```

改成你的真实 DeepSeek API Key。

保存文件。

---

## 13. JSON 格式注意事项

`settings.json` 必须是合法 JSON。

正确示例：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "your_deepseek_api_key_here"
  }
}
```

错误示例：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic"
    "ANTHROPIC_AUTH_TOKEN": "your_deepseek_api_key_here"
  }
}
```

错误原因：少了逗号。

常见错误：

```text
1. 少英文逗号。
2. 多英文逗号。
3. 使用中文引号。
4. 写了注释。
5. 大括号没有闭合。
6. 把文件保存到了项目目录，而不是用户目录。
```

---

## 14. 关闭 Claude Code 登录提示

如果不关闭登录提示，Claude Code 插件可能一直要求登录 Claude / Anthropic。

### 14.1 使用 VS Code 设置界面

Windows：

```text
Ctrl + Shift + P
```

macOS：

```text
Command + Shift + P
```

搜索：

```text
Preferences: Open Settings (UI)
```

然后搜索：

```text
claudeCode.disableLoginPrompt
```

打开该选项。

### 14.2 使用 VS Code 用户设置 JSON

打开命令面板：

Windows：

```text
Ctrl + Shift + P
```

macOS：

```text
Command + Shift + P
```

搜索：

```text
Preferences: Open User Settings (JSON)
```

加入：

```json
{
  "claudeCode.disableLoginPrompt": true
}
```

如果文件中已经有其他配置，不要重复写最外层 `{}`。

例如原来是：

```json
{
  "editor.fontSize": 14
}
```

应该改成：

```json
{
  "editor.fontSize": 14,
  "claudeCode.disableLoginPrompt": true
}
```

---

## 15. 重启 VS Code

配置完成后必须重启 VS Code 窗口。

Windows：

```text
Ctrl + Shift + P
```

macOS：

```text
Command + Shift + P
```

搜索并执行：

```text
Developer: Reload Window
```

也可以完全退出 VS Code 后重新打开。

---

## 16. 打开项目并使用 Claude Code

### 16.1 Windows 示例

```powershell
cd "C:\Users\你的用户名\Desktop\personal-blog-system"
code .
```

### 16.2 macOS 示例

```bash
cd "/Users/你的用户名/Desktop/personal-blog-system"
code .
```

### 16.3 在 VS Code 中打开 Claude Code

找到：

```text
CLAUDE CODE
```

开始新会话。

第一次不要直接让它修改代码，先输入：

```text
请先阅读当前项目结构，不要修改代码。请告诉我这个项目的主要目录、入口文件、后端框架和模板结构。
```

如果能正常回答，说明 Claude Code 已经可以工作。

---

## 17. 使用 CLI 测试配置是否成功

### 17.1 Windows

```powershell
cd "C:\Users\你的用户名\Desktop\personal-blog-system"
claude
```

然后输入：

```text
请回答：你能否读取当前项目文件？不要修改代码，只分析目录结构。
```

### 17.2 macOS

```bash
cd "/Users/你的用户名/Desktop/personal-blog-system"
claude
```

然后输入：

```text
请回答：你能否读取当前项目文件？不要修改代码，只分析目录结构。
```

如果 CLI 能正常回答，说明配置大概率是成功的。

---

## 18. 临时环境变量方式

如果不想使用 `settings.json`，也可以临时设置环境变量。

缺点：

```text
每次打开新终端都要重新设置。
```

### 18.1 macOS

```bash
export ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
export ANTHROPIC_AUTH_TOKEN="your_deepseek_api_key_here"
export ANTHROPIC_MODEL="deepseek-v4-pro[1m]"
export ANTHROPIC_DEFAULT_OPUS_MODEL="deepseek-v4-pro[1m]"
export ANTHROPIC_DEFAULT_SONNET_MODEL="deepseek-v4-pro[1m]"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="deepseek-v4-flash"
export CLAUDE_CODE_SUBAGENT_MODEL="deepseek-v4-flash"
export CLAUDE_CODE_EFFORT_LEVEL="max"
```

然后进入项目：

```bash
cd "/path/to/your/project"
claude
```

### 18.2 Windows PowerShell

```powershell
$env:ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
$env:ANTHROPIC_AUTH_TOKEN="your_deepseek_api_key_here"
$env:ANTHROPIC_MODEL="deepseek-v4-pro[1m]"
$env:ANTHROPIC_DEFAULT_OPUS_MODEL="deepseek-v4-pro[1m]"
$env:ANTHROPIC_DEFAULT_SONNET_MODEL="deepseek-v4-pro[1m]"
$env:ANTHROPIC_DEFAULT_HAIKU_MODEL="deepseek-v4-flash"
$env:CLAUDE_CODE_SUBAGENT_MODEL="deepseek-v4-flash"
$env:CLAUDE_CODE_EFFORT_LEVEL="max"
```

然后进入项目：

```powershell
cd "C:\path\to\your\project"
claude
```

---

## 19. Windows 永久环境变量方式

Windows 可以把变量写入用户环境变量。

PowerShell 执行：

```powershell
[Environment]::SetEnvironmentVariable("ANTHROPIC_BASE_URL", "https://api.deepseek.com/anthropic", "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_AUTH_TOKEN", "your_deepseek_api_key_here", "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_MODEL", "deepseek-v4-pro[1m]", "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_DEFAULT_OPUS_MODEL", "deepseek-v4-pro[1m]", "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_DEFAULT_SONNET_MODEL", "deepseek-v4-pro[1m]", "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_DEFAULT_HAIKU_MODEL", "deepseek-v4-flash", "User")
[Environment]::SetEnvironmentVariable("CLAUDE_CODE_SUBAGENT_MODEL", "deepseek-v4-flash", "User")
[Environment]::SetEnvironmentVariable("CLAUDE_CODE_EFFORT_LEVEL", "max", "User")
```

设置完成后：

```text
1. 关闭所有 PowerShell。
2. 关闭 VS Code。
3. 重新打开 VS Code。
```

不过一般更推荐使用：

```text
C:\Users\你的用户名\.claude\settings.json
```

因为它更清晰。

---

## 20. macOS 永久环境变量方式

如果使用 zsh，可以写入：

```bash
~/.zshrc
```

打开：

```bash
code ~/.zshrc
```

加入：

```bash
export ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
export ANTHROPIC_AUTH_TOKEN="your_deepseek_api_key_here"
export ANTHROPIC_MODEL="deepseek-v4-pro[1m]"
export ANTHROPIC_DEFAULT_OPUS_MODEL="deepseek-v4-pro[1m]"
export ANTHROPIC_DEFAULT_SONNET_MODEL="deepseek-v4-pro[1m]"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="deepseek-v4-flash"
export CLAUDE_CODE_SUBAGENT_MODEL="deepseek-v4-flash"
export CLAUDE_CODE_EFFORT_LEVEL="max"
```

保存后执行：

```bash
source ~/.zshrc
```

但这种方式会把 API Key 明文写入 shell 配置文件，所以仍然更推荐：

```text
~/.claude/settings.json
```

---

## 21. 模型选择建议

常用配置：

```text
主模型：deepseek-v4-pro[1m]
轻量模型：deepseek-v4-flash
```

使用建议：

| 场景 | 推荐模型 |
|---|---|
| 跨文件代码分析 | deepseek-v4-pro[1m] |
| 架构设计与重构 | deepseek-v4-pro[1m] |
| 复杂 Bug 排查 | deepseek-v4-pro[1m] |
| 简单解释代码 | deepseek-v4-flash |
| 快速生成小片段 | deepseek-v4-flash |
| 子任务处理 | deepseek-v4-flash |

配置项含义：

```json
{
  "ANTHROPIC_MODEL": "deepseek-v4-pro[1m]",
  "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-v4-pro[1m]",
  "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-v4-pro[1m]",
  "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash",
  "CLAUDE_CODE_SUBAGENT_MODEL": "deepseek-v4-flash"
}
```

可以理解为：

```text
大任务使用 deepseek-v4-pro[1m]
小任务和子任务使用 deepseek-v4-flash
```

---

## 22. Claude Code 和项目自身 AI 配置的区别

Claude Code 接 DeepSeek 使用：

```text
ANTHROPIC_BASE_URL
ANTHROPIC_AUTH_TOKEN
ANTHROPIC_MODEL
```

你的 Flask / Web 项目自己调用 DeepSeek，通常使用：

```text
AI_API_KEY
AI_BASE_URL
AI_MODEL
```

区别：

| 场景 | 变量 | 作用 |
|---|---|---|
| Claude Code 插件 | ANTHROPIC_AUTH_TOKEN | 让 AI 帮你写代码 |
| 项目后端 | AI_API_KEY | 让网站里的 AI 功能可用 |

它们可以使用同一个 DeepSeek API Key，但变量名和用途不同。

---

## 23. 常见问题排查

### 23.1 报 401 Unauthorized

可能原因：

```text
1. API Key 填错。
2. API Key 前后有空格。
3. API Key 已经过期、删除或重置。
4. API Key 泄露后被禁用。
5. settings.json 没有保存。
6. VS Code 没有重启。
7. 填成了 OpenAI Key，而不是 DeepSeek Key。
```

解决：

```text
1. 去 DeepSeek 平台重新生成 API Key。
2. 修改 settings.json。
3. 保存文件。
4. Developer: Reload Window。
5. 重新打开 Claude Code 面板。
```

---

### 23.2 报错中出现 api.openai.com

如果错误里出现：

```text
https://api.openai.com/v1/responses
```

说明当前使用的是：

```text
Codex / OpenAI 插件
```

不是：

```text
Claude Code 插件
```

解决：

```text
点击 VS Code 中的 CLAUDE CODE 面板，不要使用 CODEX 面板。
```

---

### 23.3 一直弹出登录 Claude

可能原因：

```text
1. 没有关闭 claudeCode.disableLoginPrompt。
2. settings.json 路径不对。
3. settings.json 格式错误。
4. VS Code 没有重启。
```

解决：

```text
1. 打开 VS Code 设置。
2. 搜索 claudeCode.disableLoginPrompt。
3. 打开该选项。
4. 检查 settings.json。
5. Reload Window。
```

---

### 23.4 settings.json 不生效

检查路径：

Windows：

```text
C:\Users\你的用户名\.claude\settings.json
```

macOS：

```text
/Users/你的用户名/.claude/settings.json
```

不要保存到项目目录，例如不要保存到：

```text
personal-blog-system/.claude/settings.json
```

除非你明确知道自己在使用项目级配置。

---

### 23.5 claude: command not found

说明 Claude Code CLI 没安装成功。

重新执行：

```bash
npm install -g @anthropic-ai/claude-code
```

然后检查：

```bash
claude --version
```

---

### 23.6 node: command not found

说明 Node.js 没安装或 PATH 没配置好。

解决：

```text
1. 安装 Node.js 18+。
2. 关闭终端。
3. 重新打开终端。
4. 执行 node -v 检查。
```

---

### 23.7 VS Code 插件不工作，但 CLI 可以用

可能是 VS Code 没有读取到配置。

解决顺序：

```text
1. 确认使用的是用户级 settings.json。
2. 开启 claudeCode.disableLoginPrompt。
3. Developer: Reload Window。
4. 完全退出 VS Code。
5. 重新打开 VS Code。
6. 从终端进入项目后执行 code . 打开项目。
```

---

## 24. 检查项目中是否泄露 API Key

上传 GitHub 前，建议搜索项目：

### macOS / Linux

```bash
grep -R "sk-" . --exclude-dir=.git --exclude-dir=.venv --exclude-dir=node_modules --exclude-dir=__pycache__
```

### Windows PowerShell

```powershell
Get-ChildItem -Recurse -File |
  Where-Object {
    $_.FullName -notmatch "\\.git\\" -and
    $_.FullName -notmatch "\\.venv\\" -and
    $_.FullName -notmatch "\\node_modules\\" -and
    $_.FullName -notmatch "__pycache__"
  } |
  Select-String -Pattern "sk-"
```

如果搜到了真实 API Key：

```text
1. 删除文件中的真实 Key。
2. 改成 your_deepseek_api_key_here。
3. 删除旧 API Key。
4. 重新生成新 API Key。
5. 重新检查。
```

---

## 25. .gitignore 建议

如果你的项目中有 `.env` 文件，必须加入 `.gitignore`：

```gitignore
.env
*.env
.venv/
node_modules/
__pycache__/
*.pyc
.DS_Store
```

注意：

```text
~/.claude/settings.json 通常不在项目目录中，不会被 Git 提交。
```

但如果你手动在项目里创建了 `.claude/settings.json`，一定要确认里面没有真实 API Key。

---

## 26. 推荐第一次使用提示词

第一次不要直接让 Claude Code 大规模修改代码。

建议先问：

```text
请先阅读当前项目结构，不要修改代码。请总结：
1. 项目的后端框架
2. 主要入口文件
3. 主要路由文件
4. 模板目录结构
5. 静态资源目录结构
6. 数据库模型文件
7. 当前项目可能的运行方式
```

确认它理解项目后，再问：

```text
请检查当前项目是否适合接入 DeepSeek API。不要修改代码，先列出：
1. 应该新增哪些文件
2. 应该修改哪些文件
3. API Key 应该如何从环境变量读取
4. 哪些地方不能写死 Key
5. 如何做异常处理
```

然后再允许它修改：

```text
请按照刚才的方案逐步修改代码。每次修改前先说明会改哪些文件，修改后说明如何测试。不要改动无关功能。
```

---

## 27. 软件工程项目中的推荐用法

如果是课程实践项目，不建议让 AI 一次性大改。

推荐流程：

```text
1. 先让 Claude Code 阅读项目。
2. 让它输出修改计划。
3. 你确认计划。
4. 让它小步修改。
5. 每改一部分就运行测试。
6. 每完成一个阶段就 git commit。
7. 最后写开发记录和测试记录。
```

每次修改前先保存当前状态：

```bash
git status
git add .
git commit -m "chore: save current working state"
```

再让 Claude Code 修改。

如果改坏了，可以回退。

---

## 28. Windows 检查清单

```text
[ ] 已安装 VS Code
[ ] 已安装 Node.js 18+
[ ] 已安装 Git for Windows
[ ] 已安装 Claude Code VS Code 插件
[ ] 已执行 npm install -g @anthropic-ai/claude-code
[ ] claude --version 可以显示版本号
[ ] 已申请 DeepSeek API Key
[ ] 已创建 C:\Users\你的用户名\.claude\settings.json
[ ] settings.json 中 ANTHROPIC_BASE_URL 是 https://api.deepseek.com/anthropic
[ ] settings.json 中 ANTHROPIC_AUTH_TOKEN 已填写真实 Key
[ ] 文档和 GitHub 仓库中没有真实 Key
[ ] 已开启 claudeCode.disableLoginPrompt
[ ] 已执行 Developer: Reload Window
[ ] 使用的是 CLAUDE CODE 面板，不是 CODEX
[ ] Claude Code 可以读取当前项目结构
```

---

## 29. macOS 检查清单

```text
[ ] 已安装 VS Code
[ ] 已安装 Node.js 18+
[ ] 已安装 Git
[ ] 已安装 Claude Code VS Code 插件
[ ] 已执行 npm install -g @anthropic-ai/claude-code
[ ] claude --version 可以显示版本号
[ ] 已申请 DeepSeek API Key
[ ] 已创建 ~/.claude/settings.json
[ ] settings.json 中 ANTHROPIC_BASE_URL 是 https://api.deepseek.com/anthropic
[ ] settings.json 中 ANTHROPIC_AUTH_TOKEN 已填写真实 Key
[ ] 文档和 GitHub 仓库中没有真实 Key
[ ] 已开启 claudeCode.disableLoginPrompt
[ ] 已执行 Developer: Reload Window
[ ] 使用的是 CLAUDE CODE 面板，不是 CODEX
[ ] Claude Code 可以读取当前项目结构
```

---

## 30. 一句话总结

在 VS Code 中使用 Claude Code 插件接入 DeepSeek API，核心步骤是：

```text
安装 VS Code
  ↓
安装 Node.js
  ↓
安装 Claude Code 插件与 CLI
  ↓
申请 DeepSeek API Key
  ↓
创建用户级 .claude/settings.json
  ↓
把 ANTHROPIC_BASE_URL 指向 https://api.deepseek.com/anthropic
  ↓
把 ANTHROPIC_AUTH_TOKEN 设置为 DeepSeek API Key
  ↓
关闭 Claude Code 登录提示
  ↓
重启 VS Code
  ↓
使用 CLAUDE CODE 面板
```

最重要的是：

```text
不要把真实 API Key 上传到 GitHub。
```

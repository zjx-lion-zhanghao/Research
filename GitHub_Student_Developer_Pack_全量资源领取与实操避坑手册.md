# GitHub Student Developer Pack 全量资源领取与实操避坑手册

> 适用对象：已经申请或准备申请 GitHub Education / GitHub Student Developer Pack 的中国大学生。  
> 使用目标：不是“看到免费就全领”，而是根据自己的学习、课程、大创、AI、无人机、软件工程、个人作品集等需求，有选择地领取并真正用起来。  
> 核心原则：先用低风险工具建立开发流程，再谨慎开通云服务和可能产生扣费的资源。

---

## 0. 快速结论

你现在最应该优先做的不是把所有权益都点开，而是先完成以下 5 件事：

1. 开启 **GitHub Copilot**；
2. 领取 **JetBrains 学生授权**；
3. 配置 **GitHub Desktop / VS Code / GitLens**；
4. 用 **GitHub Pages + Profile README** 做个人作品集；
5. 选一个课程项目或无人机项目，整理成可展示的 GitHub 仓库。

云服务类资源，例如 Azure、DigitalOcean、Heroku、Datadog、New Relic，建议等你真的要部署项目时再开通，因为这类服务可能涉及额度、付款方式、自动续费或超额计费。

---

# 1. 使用前必须知道的事情

## 1.1 GitHub Student Developer Pack 是什么？

GitHub Student Developer Pack 是 GitHub 面向学生提供的一套开发者资源包。它包含：

- GitHub 高级权益；
- AI 编程工具；
- 专业 IDE；
- 云服务器和部署平台；
- 数据库；
- 域名和 SSL；
- 前端、后端、AI、数据科学课程；
- DevOps、测试、监控工具；
- 密码和密钥管理工具；
- 技术写作、演讲和设计工具；
- 开源、认证和校园社区资源。

它的正确使用方式是形成一套完整工作流：

```text
学习技术
↓
写代码
↓
管理 GitHub 仓库
↓
部署项目
↓
测试与监控
↓
写文档
↓
展示作品
↓
申请认证 / 实习 / GitHub Campus Experts
```

## 1.2 不要把所有东西都立即领取

很多服务虽然标着学生免费，但仍然可能有这些问题：

```text
需要绑定银行卡
试用期结束自动续费
云资源超额扣费
OAuth 授权权限过大
访问速度不稳定
地区或手机号限制
```

所以建议按优先级领取。

---

# 2. 风险等级说明

本文给每个资源标注风险等级。

| 风险等级 | 含义 |
|---|---|
| 低风险 | 基本不涉及付款方式，也不容易产生费用 |
| 中风险 | 有订阅、试用期或第三方授权，需要注意到期时间 |
| 高风险 | 云服务、服务器、监控、支付类资源，可能产生超额费用 |
| 特殊风险 | 涉及支付、密钥、OAuth 权限、隐私或安全配置 |

---

# 3. 推荐领取顺序

## 3.1 第一优先级：马上可以领取

| 资源 | 推荐理由 | 风险 |
|---|---|---|
| GitHub Copilot | 直接提高写代码效率 | 低 |
| GitHub Pro | 私有仓库、项目管理、作品集建设 | 低 |
| JetBrains | PyCharm、CLion、IntelliJ IDEA 非常实用 | 低 |
| GitHub Desktop | Git 图形化管理，适合新手 | 低 |
| VS Code | 轻量开发环境 | 低 |
| GitHub Pages | 免费个人主页和项目展示页 | 低 |
| Notion | 项目和课程资料管理 | 低/中 |
| 1Password | 管理密码、Token、2FA 备份码 | 低/中 |

## 3.2 第二优先级：有项目再领取

| 资源 | 推荐理由 | 风险 |
|---|---|---|
| Heroku | 快速部署后端和 Web 项目 | 中/高 |
| MongoDB Atlas | 云数据库，适合 Web 和实验数据 | 中 |
| DigitalOcean | 云服务器，适合学习 Linux 部署 | 高 |
| Microsoft Azure | 云服务和 AI 服务 | 高 |
| Appwrite | 快速搭建后端能力 | 中 |
| Sentry | 线上错误监控 | 中 |
| Doppler | 管理环境变量和密钥 | 特殊风险 |

## 3.3 第三优先级：学习补强

| 资源 | 推荐理由 | 风险 |
|---|---|---|
| DataCamp | 数据分析、Python、机器学习 | 低/中 |
| Educative | Web、Python、Java、机器学习课程 | 低/中 |
| FrontendMasters | 高质量前端课程 | 低/中 |
| Scrimba | JavaScript / React 入门 | 低 |
| Boot.dev | 后端和 DevOps 学习 | 低/中 |
| AlgoExpert / InterviewCake | 算法面试和机试准备 | 低/中 |

## 3.4 第四优先级：项目工程化后再用

| 资源 | 推荐理由 | 风险 |
|---|---|---|
| Travis CI | 自动测试和持续集成 | 中 |
| Codecov | 测试覆盖率 | 中 |
| CodeScene | 代码质量分析 | 中 |
| Datadog / New Relic | 服务监控 | 高 |
| Blackfire | 性能分析 | 中 |
| Honeybadger | 错误和可用性监控 | 中 |

---

# 4. Experiences：官方学习路径

Experiences 是 GitHub 官方把多个工具按学习目标组合起来的路径。

---

## 4.1 Intro to Copilot

### 包含资源

```text
GitHub
GitHub Copilot
GitHub Codespaces
Visual Studio Code
```

### 能做什么？

学习如何把 Copilot 用进日常开发流程：

- AI 自动补全；
- 解释代码；
- 解释报错；
- 生成测试；
- 生成 README；
- 辅助重构；
- 在 Codespaces 中在线开发。

### 适合你的场景

```text
Python 实验
YOLO 数据处理
ROS2 节点
OpenCV 脚本
软件工程项目
数学建模代码
```

### 是否建议现在使用？

**强烈建议。**

### 风险

低风险。主要风险是过度依赖 AI，导致自己不理解代码。

---

## 4.2 Intro to GitHub

### 包含资源

```text
GitHub
```

### 能做什么？

学习 GitHub Flow：

```text
创建仓库
创建分支
提交 commit
发起 Pull Request
代码审查
合并分支
```

### 适合你的场景

- 课程设计；
- 大创项目；
- 小组项目；
- 开源项目；
- GitHub Campus Experts 准备。

### 是否建议现在使用？

**建议。**  
这是 GitHub 所有项目管理能力的基础。

---

## 4.3 GitHub Foundations Certification

### 包含资源

```text
DataCamp
GitHub
Microsoft Azure
```

### 能做什么？

准备 GitHub Foundations 官方认证。

### 适合你的用途

- 放进简历；
- 放进 GitHub 主页；
- 证明你会 GitHub 基础协作；
- 申请 GitHub Campus Experts 时作为能力证明。

### 是否建议现在领取？

可以先学习，不建议裸考。  
先学 GitHub 基础，再使用考试券或认证资源。

---

## 4.4 Intro to Open Source

### 包含资源

```text
Travis CI
GitHub
OpenSauced
GitHub Codespaces
```

### 能做什么？

学习开源贡献流程：

```text
找 good first issue
提交 Pull Request
参与 Issue 讨论
理解维护者工作
分析开源项目活跃度
```

### OpenSauced 的作用

OpenSauced 可以用来：

- 分析开源贡献；
- 查看自己的 GitHub 活跃度；
- 发现适合参与的开源项目；
- 为个人作品集展示开源经历；
- 为 GitHub Campus Experts 申请提供素材。

### 是否建议现在用？

如果你近期想做开源或申请 GitHub Campus Experts，建议使用。

---

## 4.5 AI Prompting & Technical Writing

### 包含资源

```text
GitHub
Microsoft Azure
Educative
POEditor
```

### 能做什么？

学习 AI 提示词和技术写作：

```text
README
API 文档
实验报告
项目教程
开发说明
多语言文档
```

### 适合你的场景

你经常需要写：

```text
实验报告
大创中期报告
项目计划书
软件工程文档
GitHub 教程
数模论文说明
```

### 是否建议现在用？

建议。  
尤其适合提高 README 和技术文档质量。

---

## 4.6 Intro to Web Dev

### 包含资源

```text
LambdaTest
Polypane
Bootstrap Studio
DigitalOcean
```

### 能做什么？

学习从网页设计到部署的完整流程：

- 设计页面；
- 写前端；
- 测试不同屏幕；
- 部署到服务器；
- 做个人主页或项目展示页。

### 适合你的场景

```text
个人主页
无人机项目展示页
课程设计前端
社团活动页面
```

### 是否建议现在用？

如果你准备做作品集网站，建议先用 GitHub Pages；DigitalOcean 等有费用风险的云服务可以后置。

---

## 4.7 Data Science & Machine Learning

### 包含资源

```text
Datadog
SQLGate
GitHub Codespaces
Deepnote
```

### 能做什么？

围绕数据科学和机器学习：

```text
数据清洗
数据分析
数据可视化
机器学习建模
协作 Notebook
数据库分析
```

### 适合你的场景

```text
数学建模
人工智能导论实验
飞行日志分析
YOLO 数据集统计
路径规划算法对比
```

### 是否建议现在用？

建议先用 Deepnote、DataCamp、PyCharm；Datadog 这种监控工具可以后置。

---

## 4.8 Mobile App Development

### 包含资源

```text
Microsoft Azure
FrontendMasters
LambdaTest
NativeScript
```

### 能做什么？

学习移动 App 开发和测试。

### NativeScript 的作用

NativeScript 可以用 JavaScript / TypeScript 开发跨平台移动 App。

### 适合你的项目

```text
无人机任务查看 App
实验数据移动端查看器
校园活动报名 App
巡检结果展示 App
```

### 是否建议现在用？

暂时不优先。  
你当前主线更适合先做 Web 展示页，而不是移动 App。

---

## 4.9 Virtual Event Kit

### 包含资源

```text
Namecheap
Microsoft Azure
Name.com
```

### 能做什么？

帮助学生举办线上活动：

```text
活动官网
报名页面
直播活动页
线上 Hackathon 页面
技术分享宣传页
```

### 适合你的场景

如果你以后申请 GitHub Campus Experts，可以用它组织：

```text
GitHub 入门分享
AI 编程工具使用分享
无人机与 ROS2 技术分享
YOLO 实践工作坊
开源项目入门活动
```

---

## 4.10 Profile README

### 包含资源

```text
GitHub
```

### 能做什么？

创建 GitHub 个人主页 README。

### 建议内容

```text
个人介绍
学校和方向
技术栈
项目经历
GitHub 统计
个人网站链接
联系方式
```

### 实操步骤

1. 创建一个和 GitHub 用户名完全相同的仓库；
2. 仓库设为 Public；
3. 新建 `README.md`；
4. 写入个人介绍和项目链接；
5. 提交后 GitHub 主页会自动显示。

### 是否建议现在做？

**强烈建议。**

---

## 4.11 Launchpad: Intro to Javascript

### 包含资源

```text
Scrimba
```

### 能做什么？

学习 JavaScript 入门：

```text
变量
函数
DOM
事件
简单网页交互
```

### 适合你的场景

如果你要做个人主页、React 项目、Web 展示页，需要补 JavaScript 基础。

---

# 5. All offers：全部资源领取与使用说明

---

## 5.1 DigitalOcean

### 是什么？

开发者云服务器平台。

### 学生权益

页面显示为 **$200 platform credit for 1 year**。

### 能做什么？

```text
开 Linux 云服务器
部署个人网站
部署 Flask / FastAPI 后端
部署 Spring Boot 项目
部署数据库
学习 Docker
学习 Nginx
学习 SSH
```

### 适合你的项目

```text
无人机项目展示网站
YOLO Web Demo
物业管理系统后端
课程设计系统
个人作品集网站
```

### 领取步骤

1. 在 Student Pack 中找到 DigitalOcean；
2. 点击 Get access；
3. 使用 GitHub 授权；
4. 注册或登录 DigitalOcean；
5. 查看 credit 是否到账；
6. 创建 Droplet 或 App Platform 项目。

### 风险等级

**高风险。**

### 避坑

- 不用的 Droplet 立刻关停并删除；
- 不要创建高规格服务器；
- 设置账单提醒；
- 不要把 SSH 私钥上传 GitHub；
- 服务器密码、数据库密码不要写进代码。

### 是否建议现在领取？

如果你还没有明确部署项目，可以暂缓。

---

## 5.2 GitHub Copilot

### 是什么？

AI 编程助手。

### 学生权益

学生可免费使用 Copilot Student。

### 能做什么？

```text
代码补全
生成函数
解释代码
解释报错
生成测试
生成 README
辅助重构
写脚本
```

### 适合你的项目

```text
Python 实验
YOLO
OpenCV
ROS2
PX4 辅助阅读
Java 后端
Spring Boot
数学建模
```

### 领取步骤

1. 打开 GitHub；
2. 进入 Settings；
3. 找到 Copilot；
4. 选择学生免费启用；
5. 在 VS Code 或 JetBrains 中登录 GitHub；
6. 安装 Copilot 插件；
7. 开始使用。

### 风险等级

低风险。

### 避坑

- 不要直接复制不理解的代码；
- 不要让 Copilot 处理敏感 Token；
- 飞控、数据库删除、服务器命令必须人工检查。

### 是否建议现在领取？

**强烈建议。**

---

## 5.3 Name.com

### 是什么？

域名注册平台。

### 学生权益

可获得部分免费域名扩展。

### 能做什么？

```text
个人主页域名
项目官网域名
活动官网域名
GitHub Pages 绑定域名
```

### 适合你的项目

```text
个人技术主页
无人机项目官网
课程项目展示站
GitHub Campus Expert 活动页
```

### 领取步骤

1. 在 Student Pack 中找到 Name.com；
2. 点击 Get access；
3. GitHub 授权；
4. 搜索可用域名；
5. 选择支持的免费后缀；
6. 绑定到 GitHub Pages 或其他部署平台。

### 风险等级

中风险。

### 避坑

- 域名通常第一年免费，续费可能收费；
- 记下到期时间；
- 不想续费时及时取消自动续费；
- 不要随便公开个人隐私信息。

---

## 5.4 Namecheap

### 是什么？

域名和 SSL 服务商。

### 学生权益

常见权益：

```text
.me 域名 1 年
SSL 证书 1 年
```

### 能做什么？

```text
个人域名
项目域名
HTTPS 证书
活动网站
```

### 领取步骤

1. Student Pack 中找到 Namecheap；
2. 选择域名权益或 SSL 权益；
3. GitHub 授权；
4. 注册域名；
5. 将 DNS 解析到 GitHub Pages、Heroku 或服务器。

### 风险等级

中风险。

### 避坑

- 第一年的域名免费不代表永久免费；
- SSL 证书很多平台已自动提供，不一定需要手动申请；
- 续费前评估是否还要这个域名。

---

## 5.5 Microsoft Azure

### 是什么？

微软云平台。

### 学生权益

页面显示：

```text
25+ Azure cloud services
$100 Azure credit
通常学生计划可不需要信用卡
```

### 能做什么？

```text
部署 Web 应用
使用云数据库
使用 Azure Functions
学习云计算
使用 AI 服务
搭建项目后端
```

### 适合你的项目

```text
物业管理系统
无人机实验数据平台
YOLO 检测 API
课程设计后端
AI 图像识别实验
```

### 领取步骤

1. 找到 Microsoft Azure offer；
2. 点击 Get access；
3. 使用 GitHub 或学生邮箱验证；
4. 进入 Azure for Students；
5. 激活额度；
6. 创建轻量服务。

### 风险等级

高风险。

### 避坑

- 设置预算提醒；
- 不要开高规格虚拟机；
- 不用资源及时删除；
- 注意地区选择；
- 不要泄露连接字符串和密钥。

### 是否建议现在领取？

如果只是写代码，暂缓。  
如果要学习云部署，可以领取但谨慎使用。

---

## 5.6 GitHub

### 是什么？

代码托管与协作平台。

### 学生权益

Free GitHub Pro while you are a student。

### 能做什么？

```text
私有仓库
项目管理
Issues
Pull Requests
GitHub Actions
项目文档
个人作品集
开源贡献
```

### 实操建议

你可以建立这些仓库：

```text
uav-simulation
uav-yolo-detection
uav-path-planning
property-management-system
dsp-review-agent
math-modeling-projects
```

### 风险等级

低风险。

### 避坑

- 不要上传密钥；
- 私有作业不要误设为公开；
- 公开项目前检查是否有个人信息。

---

## 5.7 Microsoft 365

### 是什么？

Office 生产力工具，包括 Word、Excel、PowerPoint、OneDrive、Copilot 等。

### 能做什么？

```text
实验报告
项目计划书
PPT
论文
简历
云端文件协作
```

### 领取步骤

1. 进入 Microsoft 365 offer；
2. 用学校邮箱或个人 Microsoft 账号验证学生身份；
3. 查看是否免费或折扣；
4. 如果要求付款方式，认真确认自动续费；
5. 领取后立刻检查订阅管理页面。

### 风险等级

中/高风险。

### 避坑

- 重点检查是否自动续费；
- 免费试用不等于永久免费；
- 绑定银行卡前确认地区和价格；
- 领取后可关闭 recurring billing。

---

## 5.8 .TECH

### 是什么？

技术类域名。

### 学生权益

一个标准 `.TECH` 域名免费 1 年。

### 能做什么？

```text
个人技术主页
项目官网
开源项目网站
无人机项目展示站
```

### 风险等级

中风险。

### 避坑

- 第二年续费可能较贵；
- 域名到期前确认是否续费；
- 不要为短期作业随便注册太多域名。

---

## 5.9 Notion

### 是什么？

文档、知识库、项目管理工具。

### 能做什么？

```text
课程笔记
项目计划
任务看板
实验记录
文献管理
会议纪要
知识库
```

### 适合你的场景

```text
无人机大创工作台
工程材料复习
软件工程文档管理
数学建模资料整理
GitHub 项目规划
```

### 领取步骤

1. 进入 Notion offer；
2. 使用学生邮箱或 GitHub Student Pack 入口；
3. 登录 Notion；
4. 激活 Education / Plus 权益；
5. 创建课程和项目模板。

### 风险等级

低/中风险。

### 避坑

- 不要把 API Key、身份证件、银行卡信息放进公开 Notion 页面；
- 分享页面时检查权限。

---

## 5.10 Boot.dev

### 是什么？

后端和 DevOps 学习平台。

### 能学什么？

```text
Python
Go
TypeScript
后端开发
HTTP
数据库
DevOps
数据分析
```

### 适合你什么时候用？

当你想从“写脚本”升级到“写完整后端服务”时使用。

### 风险等级

低/中风险。

### 避坑

注意免费期，到期前确认是否续费。

---

## 5.11 Codedex

### 是什么？

游戏化编程学习平台。

### 能学什么？

```text
Python
HTML
CSS
JavaScript
React
Git & GitHub
命令行
```

### 适合谁？

适合补基础。  
如果你已经有一定编程基础，可以选择性使用。

### 风险等级

低风险。

---

## 5.12 Visual Studio Code

### 是什么？

微软轻量代码编辑器。

### 能做什么？

```text
Python
C++
JavaScript
Markdown
Docker
SSH 远程开发
Git
ROS2 配置
```

### 实操建议

建议安装插件：

```text
GitHub Copilot
Python
C/C++
GitLens
Remote SSH
Markdown All in One
Docker
ROS 相关插件
```

### 风险等级

低风险。

---

## 5.13 JetBrains

### 是什么？

专业 IDE 套件。

### 学生权益

学生免费订阅，通常需要每年续期。

### 包括什么？

```text
PyCharm Professional
CLion
IntelliJ IDEA Ultimate
WebStorm
DataGrip
GoLand
Rider
PhpStorm
```

### 具体用途

| 工具 | 用途 |
|---|---|
| PyCharm | Python、AI、YOLO、OpenCV、数据分析 |
| CLion | C++、CMake、ROS2、PX4 源码 |
| IntelliJ IDEA | Java、Spring Boot、软件工程项目 |
| DataGrip | MySQL、PostgreSQL、SQLite 数据库管理 |
| WebStorm | 前端、Vue、React、Node.js |

### 领取步骤

1. 找到 JetBrains offer；
2. 点击 Get access；
3. 用 GitHub 学生身份或学校邮箱验证；
4. 创建 JetBrains 账号；
5. 下载 JetBrains Toolbox；
6. 安装 PyCharm、CLion、IntelliJ IDEA、DataGrip；
7. 在 IDE 中登录 JetBrains 账号激活。

### 风险等级

低风险。

### 是否建议现在领取？

**强烈建议。**

---

## 5.14 Heroku

### 是什么？

应用部署平台。

### 学生权益

页面显示：

```text
$13 USD per month for 24 months
```

### 能做什么？

```text
部署 Flask / FastAPI
部署 Node.js
部署 Spring Boot
部署课程设计
部署算法 Demo
使用 Heroku Postgres
```

### 适合你的项目

```text
物业管理系统后端
停车位分配系统
YOLO 检测 API
路径规划 Demo
数学建模结果展示平台
```

### 领取步骤

1. 找到 Heroku offer；
2. GitHub 授权；
3. 注册 Heroku；
4. 查看学生额度是否到账；
5. 创建 App；
6. 连接 GitHub 仓库；
7. 设置环境变量；
8. 部署项目。

### 风险等级

中/高风险。

### 避坑

- 检查是否需要付款方式；
- 不用的 App 和数据库及时删除；
- 不要把数据库连接字符串写进 GitHub；
- 注意月额度，不要以为无限免费。

---

## 5.15 DataCamp

### 是什么？

数据科学学习平台。

### 能学什么？

```text
Python 数据分析
Pandas
NumPy
SQL
机器学习
统计学
数据可视化
R 语言
```

### 适合你的用途

```text
数学建模
人工智能导论实验
飞行日志分析
数据清洗
图表绘制
```

### 风险等级

低/中风险。

### 避坑

注意免费期，到期前取消或确认是否续费。

---

## 5.16 Testmail

### 是什么？

邮件测试工具。

### 能做什么？

```text
注册验证码测试
找回密码邮件测试
邮箱确认测试
通知邮件测试
自动化邮件测试
```

### 适合项目

任何带登录注册功能的 Web 项目。

### 风险等级

低风险。

---

## 5.17 GitHub Codespaces

### 是什么？

云端开发环境。

### 能做什么？

```text
在线运行轻量 Python 项目
在线开发 Web 项目
复现 GitHub 仓库
临时测试代码
给别人提供免配置开发环境
```

### 实操步骤

1. 打开 GitHub 仓库；
2. 点击 Code；
3. 选择 Codespaces；
4. Create codespace；
5. 在浏览器中开发；
6. 修改后 commit 并 push。

### 风险等级

低/中风险。

### 避坑

- 不要长时间开着不用；
- 注意使用额度；
- 不适合大型仿真和训练。

---

## 5.18 Educative

### 是什么？

交互式编程课程平台。

### 能学什么？

```text
Web Development
Python
Java
Machine Learning
系统设计
算法
```

### 适合你的用途

补 Python、Java、机器学习、Web 后端基础。

### 风险等级

低/中风险。

---

## 5.19 MongoDB

### 是什么？

文档型数据库。MongoDB Atlas 是云数据库服务。

### 学生权益

页面显示：

```text
$50 MongoDB Atlas Credits
MongoDB Compass
MongoDB University
free certification valued at $150
```

### 能做什么？

```text
存储实验日志
存储 YOLO 检测结果
存储用户数据
存储项目数据
存储 JSON 结构数据
```

### 适合你的项目

```text
无人机实验日志平台
YOLO 检测结果存储
课程设计后端
项目展示网站后台
```

### 领取步骤

1. 找到 MongoDB offer；
2. GitHub 授权；
3. 创建 MongoDB Atlas 账号；
4. 创建免费或学生额度集群；
5. 创建数据库用户；
6. 设置 IP 白名单；
7. 获取连接字符串；
8. 在项目中使用环境变量保存连接字符串。

### 风险等级

中风险。

### 避坑

- 不要公开连接字符串；
- 不要设置 `0.0.0.0/0` 后忘记管理；
- 不用的付费集群及时删除；
- 课程项目优先用免费规格。

---

## 5.20 Termius

### 是什么？

跨平台 SSH 客户端。

### 能做什么？

```text
远程连接 Linux 服务器
管理 DigitalOcean / Azure 云服务器
保存 SSH 主机信息
手机或 iPad 临时登录服务器
同步 SSH 配置
```

### 适合你的场景

如果你使用 DigitalOcean、Azure、学校实验室服务器，Termius 很有用。

### 风险等级

特殊风险。

### 避坑

- 妥善保护 SSH 私钥；
- 手机丢失要能及时撤销访问；
- 不要把服务器密码明文发给别人。

---

## 5.21 Clerk

### 是什么？

用户认证和用户管理平台。

### 能提供什么？

```text
登录
注册
邮箱验证
OAuth 登录
用户管理
权限控制
订阅收费
```

### 适合项目

```text
物业管理系统
实验预约系统
校园活动平台
项目管理系统
```

### 风险等级

特殊风险。

### 避坑

- 用户认证涉及隐私和安全；
- 不要随意公开 Clerk Secret Key；
- 权限规则要自己检查。

---

## 5.22 Datadog

### 是什么？

云基础设施和应用监控平台。

### 能监控什么？

```text
服务器 CPU
内存
磁盘
请求耗时
数据库性能
日志
应用错误
```

### 风险等级

高风险。

### 是否建议现在用？

暂缓。  
没有线上服务时意义不大，且监控类服务容易产生额度问题。

---

## 5.23 Camber

### 是什么？

AI-powered 科学计算、仿真和数据分析云平台。

### 学生权益

页面提到：

```text
200 CPU hours
75GB storage
200 LLM messages per month
```

### 能做什么？

```text
科学计算
数据分析
仿真实验
批量脚本运行
轻量机器学习
路径规划算法对比
飞行日志分析
```

### 风险等级

中风险。

### 适合你的场景

适合处理实验数据、跑轻量算法对比，不适合替代本地图形化 Gazebo 仿真。

---

## 5.24 Microsoft Azure for ages 13-17

### 是什么？

面向 13-17 岁学生的 Azure 学生服务。

### 对你是否重要？

你是大学生，通常应关注成人学生版 Azure。这个不是重点。

---

## 5.25 GitHub Pages

### 是什么？

GitHub 免费静态网站托管服务。

### 能做什么？

```text
个人主页
项目展示页
课程实验合集
文档网站
大创项目展示网站
```

### 实操步骤

1. 创建仓库：`用户名.github.io`；
2. 新建 `index.html` 或使用静态网站框架；
3. push 到 GitHub；
4. 在 Settings → Pages 中启用；
5. 访问生成的网址。

### 风险等级

低风险。

### 是否建议现在用？

**强烈建议。**

---

## 5.26 FrontendMasters

### 是什么？

高质量前端课程平台。

### 能学什么？

```text
JavaScript
TypeScript
React
Vue
Node.js
前端工程化
Web 性能
```

### 适合项目

```text
个人主页
项目展示网站
后台管理系统
Web 可视化界面
```

### 风险等级

低/中风险。

---

## 5.27 Stripe

### 是什么？

在线支付平台。

### 能做什么？

```text
支付接口
订单系统
订阅收费
SaaS 项目
电商模拟项目
```

### 风险等级

特殊风险 / 高风险。

### 是否建议现在用？

不建议现在开。  
真实支付涉及地区、身份、资金、税务、合规和安全问题。课程项目可以用模拟支付，不需要真实 Stripe。

---

## 5.28 Microsoft Visual Studio Dev Essentials

### 是什么？

微软开发者工具包。

### 能做什么？

```text
Visual Studio Community
Azure 服务
学习资源
开发工具
C# / .NET 开发
Windows 开发
```

### 风险等级

低/中风险。

---

## 5.29 Appwrite

### 是什么？

开源后端服务平台。

### 能提供什么？

```text
用户认证
数据库
文件存储
云函数
权限管理
静态网站托管
```

### 适合项目

```text
校园活动报名系统
实验数据管理系统
无人机实验日志平台
课程资源分享平台
```

### 风险等级

中风险。

### 避坑

- 看清免费额度；
- 不要公开 API Key；
- 权限规则要配置正确。

---

## 5.30 Notion Template Collection

### 是什么？

Notion 模板集合。

### 能做什么？

```text
CS 课程仪表盘
Hackathon 管理模板
作品集模板
项目计划模板
学习计划模板
```

### 风险等级

低风险。

---

## 5.31 1Password

### 是什么？

密码管理器。

### 能保存什么？

```text
GitHub 密码
Microsoft 密码
JetBrains 账号
服务器密码
数据库密码
API Key
SSH Key 备注
2FA 备份码
```

### 领取和使用步骤

1. 打开 1Password offer；
2. 注册账号；
3. 激活学生权益；
4. 安装浏览器插件和桌面端；
5. 导入或新建密码；
6. 保存 2FA 备份码；
7. 给重要账号生成强密码。

### 风险等级

特殊风险。

### 避坑

- 主密码必须强；
- 保存恢复码；
- 不要把主密码告诉任何人；
- 重要账号开启 2FA。

### 是否建议现在用？

建议。你后面账号会越来越多。

---

## 5.32 PomoDone

### 是什么？

番茄钟和时间管理工具。

### 能做什么？

```text
复习计时
写代码计时
写报告计时
项目开发时间追踪
减少拖延
```

### 风险等级

低风险。

---

## 5.33 GitHub Campus Experts

### 是什么？

GitHub 学生校园专家项目。

### 能做什么？

成为校园技术社区组织者，接受 GitHub 培训，组织技术活动。

### 适合你的活动主题

```text
GitHub 入门
AI 编程工具使用
开源项目贡献
无人机与 ROS2 技术分享
YOLO 目标检测实践
大学生如何做技术作品集
```

### 风险等级

低风险。

### 是否建议关注？

建议。尤其你现在已经在了解 GitHub Education 和学生开发者资源。

---

## 5.34 IconScout

### 是什么？

图标、插画、3D 素材、Lottie 动画资源平台。

### 能用在什么地方？

```text
PPT
README
个人网站
项目展示页
系统架构图
流程图
UI 设计
```

### 风险等级

低/中风险。

---

## 5.35 Polypane

### 是什么？

响应式网页测试浏览器。

### 能测试什么？

```text
手机屏幕
平板屏幕
笔记本屏幕
大屏幕
不同设备布局
```

### 适合

个人主页、活动页面、项目展示网站。

### 风险等级

低/中风险。

---

## 5.36 Sentry

### 是什么？

应用错误监控平台。

### 能记录什么？

```text
错误类型
错误堆栈
发生时间
用户环境
浏览器信息
接口错误
```

### 适合项目

```text
Flask / FastAPI 后端
React 前端
Vue 前端
Node.js 后端
Spring Boot 后端
```

### 风险等级

中风险。

### 避坑

- 不要上传用户敏感数据；
- 注意错误日志中是否包含 Token；
- 设置合理采样率。

---

## 5.37 LocalStack

### 是什么？

本地 AWS 云服务模拟器。

### 能模拟什么？

```text
S3
Lambda
DynamoDB
SQS
API Gateway
```

### 适合

学习云计算、DevOps、本地测试云应用。

### 风险等级

低/中风险。

---

## 5.38 Scrimba

### 是什么？

交互式前端学习平台。

### 能学什么？

```text
HTML
CSS
JavaScript
React
Python
```

### 风险等级

低风险。

---

## 5.39 New Relic

### 是什么？

可观测性平台。

### 能监控什么？

```text
服务器
应用性能
数据库
错误
日志
接口性能
```

### 风险等级

高风险。

### 是否建议现在用？

暂缓。  
等你有线上服务再用。

---

## 5.40 Bootstrap Studio

### 是什么？

基于 Bootstrap 的可视化网页设计工具。

### 能做什么？

```text
个人主页
项目展示页
课程设计前端
活动报名页面
```

### 风险等级

低风险。

---

## 5.41 GitLens

### 是什么？

VS Code Git 增强插件。

### 能查看什么？

```text
某一行代码是谁写的
什么时候改的
对应 commit
文件历史
分支图
Pull Request 信息
```

### 适合

多人协作项目、课程设计、大创项目。

### 风险等级

低风险。

---

## 5.42 Visme

### 是什么？

在线演示文稿、信息图和视觉文档工具。

### 能做什么？

```text
PPT
项目介绍图
数据可视化
汇报材料
宣传图
```

### 风险等级

低/中风险。

---

## 5.43 Deepnote

### 是什么？

云端 Jupyter Notebook。

### 能做什么？

```text
数据清洗
统计分析
画图
模型训练
结果解释
协作 Notebook
```

### 适合

数学建模、数据分析、机器学习实验。

### 风险等级

中风险。

### 避坑

不要在 Notebook 中明文写数据库密码、API Key。

---

## 5.44 BrowserStack

### 是什么？

真实设备和浏览器测试平台。

### 能测试什么？

```text
不同浏览器
真实 iOS 设备
真实 Android 设备
Web App 兼容性
移动端兼容性
```

### 风险等级

中风险。

### 是否建议现在用？

没有正式网站或移动项目时暂缓。

---

## 5.45 Zyte

### 是什么？

Scrapy Cloud 爬虫平台。

### 能做什么？

```text
网页数据抓取
定时爬取
爬虫部署
爬虫数据管理
```

### 风险等级

特殊风险。

### 避坑

- 遵守网站服务条款；
- 遵守 robots.txt；
- 不抓隐私数据；
- 不高频请求；
- 不爬需要登录的敏感内容。

---

## 5.46 Icons8

### 是什么？

图标、插画、照片和音乐素材平台。

### 能用在什么地方？

```text
PPT
README
网页
流程图
架构图
UI 设计
```

### 风险等级

低/中风险。

---

## 5.47 Arduino

### 是什么？

开源硬件平台和 Arduino Cloud。

### 能做什么？

```text
传感器实验
物联网
嵌入式入门
硬件原型
电池电压监控
外设模块测试
```

### 适合你的无人机方向吗？

适合做外设测试和传感器原型，不适合替代无人机飞控主控。

### 风险等级

低/中风险。

---

## 5.48 AlgoExpert

### 是什么？

算法面试准备平台。

### 能练什么？

```text
数组
链表
树
图
动态规划
递归
排序
搜索
```

### 适合

实习笔试、保研机试、算法基础补强。

### 风险等级

低/中风险。

---

## 5.49 HazeOver

### 是什么？

Mac 专注工具。

### 能做什么？

突出当前窗口，弱化其他窗口，减少分心。

### 适合

```text
写报告
写代码
看教材
复习
```

### 风险等级

低风险。

---

## 5.50 Imgbot

### 是什么？

GitHub App，自动优化仓库中的图片。

### 能做什么？

```text
压缩 README 图片
压缩文档图片
优化网站图片
减少仓库体积
提高网页加载速度
```

### 适合你的场景

你经常做 PPT、报告、README、项目展示网页，图片多时 Imgbot 很有用。

### 风险等级

中风险。

### 避坑

作为 GitHub App，它可能请求仓库权限。授权前看清楚作用范围，尽量只授权必要仓库。

---

## 5.51 GitHub Desktop

### 是什么？

GitHub 官方桌面 Git 客户端。

### 能做什么？

```text
clone
commit
push
pull
切换分支
查看改动
解决简单冲突
```

### 风险等级

低风险。

### 是否建议现在用？

建议，尤其适合你整理课程项目和 GitHub 仓库。

---

## 5.52 Pageclip

### 是什么？

静态网站表单后端服务。

### 能做什么？

```text
活动报名表
反馈表
联系表单
课程资源申请表
```

不用自己写后端也能收集表单数据。

### 风险等级

中风险。

### 避坑

表单不要收集身份证、银行卡、密码等敏感信息。

---

## 5.53 LambdaTest

### 是什么？

跨浏览器在线测试平台。

### 能测试什么？

```text
Chrome
Safari
Firefox
Edge
Windows
macOS
Android
iOS
```

### 风险等级

中风险。

### 是否建议现在用？

没有正式网站时暂缓。

---

## 5.54 CodeScene

### 是什么？

代码质量分析工具。

### 能分析什么？

```text
代码复杂度
技术债
高风险文件
热点代码
维护难度
```

### 适合

多人项目、软件工程项目、大型课程设计。

### 风险等级

中风险。

---

## 5.55 Blackfire

### 是什么？

性能分析工具。

### 能发现什么？

```text
哪个函数最慢
哪个接口耗时最高
数据库查询是否拖慢系统
性能瓶颈在哪里
```

### 适合

Web 后端项目性能优化。

### 风险等级

中风险。

---

## 5.56 GitKraken

### 是什么？

专业 Git 图形客户端。

### 能做什么？

```text
查看分支图
管理 commit
处理 merge
管理 Pull Request
管理 Issue
图形化操作 Git
```

### 和 GitHub Desktop 区别

```text
GitHub Desktop：简单、够用、适合新手
GitKraken：更专业、分支图更强、适合复杂项目
```

### 风险等级

低/中风险。

---

## 5.57 ToDiagram

### 是什么？

把 JSON、YAML、CSV、XML 转成可编辑图表的工具。

### 能做什么？

```text
系统架构图
树结构
流程图
网络拓扑
数据关系图
```

### 适合你的场景

```text
计算机网络 DNS 模拟
软件工程系统结构图
数据关系图
项目流程图
```

### 风险等级

低/中风险。

---

## 5.58 SlideCoach

### 是什么？

AI 演讲训练工具。

### 能练什么？

```text
大创中期答辩
项目路演
英语口语展示
比赛汇报
GitHub Campus Expert 申请陈述
```

### 风险等级

低/中风险。

---

## 5.59 Travis CI

### 是什么？

持续集成平台。

### 能自动做什么？

```text
安装依赖
运行测试
检查格式
构建项目
部署项目
```

### 适合

开源项目、课程设计、软件工程项目。

### 风险等级

中风险。

### 避坑

- 不要在 CI 日志里打印密钥；
- 私有仓库授权要看清权限；
- 项目小的时候可以先不用。

---

## 5.60 Requestly

### 是什么？

HTTP 请求拦截、修改、模拟工具。

### 能做什么？

```text
请求重定向
模拟接口返回
修改请求头
前后端联调
API Mock
```

### 适合

前后端联调、接口测试。

### 风险等级

中风险。

---

## 5.61 Octicons

### 是什么？

GitHub 官方开源图标库。

### 能用在哪？

```text
README 图标
网页图标
GitHub 风格 UI
项目文档
```

### 风险等级

低风险。

---

## 5.62 Adafruit

### 是什么？

开源硬件和电子模块平台。

### 能学什么？

```text
传感器
开发板
物联网
LED
电机控制
数据采集
```

### 对无人机方向的潜在用途

适合外设测试、传感器实验、地面辅助硬件原型。

### 风险等级

低/中风险。

---

## 5.63 Xojo

### 是什么？

跨平台应用开发工具。

### 能开发什么？

```text
桌面软件
移动应用
Web 应用
Raspberry Pi 应用
```

### 适合

快速做小工具或跨平台原型。

### 风险等级

低/中风险。

---

## 5.64 InterviewCake

### 是什么？

算法面试训练平台。

### 能学什么？

```text
数据结构
算法题
面试题
复杂度分析
面试技巧
```

### 适合

实习、笔试、保研机试准备。

### 风险等级

低/中风险。

---

## 5.65 WorkingCopy

### 是什么？

iPhone / iPad 上的 Git 客户端。

### 能做什么？

```text
在 iPad 上查看代码
修改 README
提交 commit
同步 GitHub
管理仓库
```

### 适合

如果你经常用 iPad 学习或写文档，可以用它管理 GitHub 仓库。

### 风险等级

中风险。

### 避坑

移动设备丢失时要能及时撤销 GitHub 登录。

---

## 5.66 Doppler

### 是什么？

密钥和环境变量管理工具。

### 能管理什么？

```text
API_KEY
DATABASE_URL
GITHUB_TOKEN
OPENAI_API_KEY
JWT_SECRET
```

### 为什么重要？

不要把这些写进 GitHub：

```python
api_key = "sk-xxxxxxx"
password = "123456"
```

### 风险等级

特殊风险。

### 避坑

- Doppler 里保存的是高敏感信息；
- 开启 2FA；
- 不要把访问权限给无关人员；
- 项目结束后清理不再使用的密钥。

---

## 5.67 Blockchair

### 是什么？

区块链数据 API 平台。

### 能查询什么？

```text
比特币交易
以太坊交易
地址信息
链上数据
```

### 是否建议现在用？

如果你不做区块链项目，可以暂时不用。

### 风险等级

低/中风险。

---

## 5.68 SQLGate

### 是什么？

SQL 数据库 IDE。

### 能做什么？

```text
连接数据库
写 SQL
查看表结构
导出数据
管理多种 SQL 数据库
```

### 和 DataGrip 的关系

如果你已经用 DataGrip，SQLGate 不必优先。

### 风险等级

低/中风险。

---

## 5.69 Tower

### 是什么？

macOS / Windows 上的专业 Git 客户端。

### 能做什么？

```text
管理分支
查看提交历史
处理 merge
管理远程仓库
图形化 Git 操作
```

### 和 GitKraken 的关系

二选一即可。

### 风险等级

低/中风险。

---

## 5.70 DeepScan

### 是什么？

JavaScript / TypeScript 代码质量分析平台。

### 适合

```text
React
Vue
Node.js
JavaScript
TypeScript
```

### 能发现什么？

```text
潜在 bug
不安全写法
代码质量问题
```

### 风险等级

中风险。

---

## 5.71 CARTO

### 是什么？

空间数据分析和地图可视化平台。

### 能做什么？

```text
地理坐标分析
地图数据可视化
空间分布分析
轨迹展示
位置分析
```

### 对无人机项目的潜在用途

室外轨迹、巡检点、地理数据可视化有用。纯室内坐标场景价值较低。

### 风险等级

中风险。

---

## 5.72 GoRails

### 是什么？

Ruby on Rails 学习平台。

### 能学什么？

```text
Ruby
Rails
JavaScript
Vue
Web 开发
```

### 是否建议现在用？

如果你不学 Ruby，可以不优先。

### 风险等级

低/中风险。

---

## 5.73 AstraSecurity

### 是什么？

网站安全套件。

### 能做什么？

```text
网站防火墙
恶意软件扫描
漏洞检测
安全防护
```

### 是否建议现在用？

有线上网站之后再考虑。

### 风险等级

中/高风险。

---

## 5.74 Codecov

### 是什么？

代码测试覆盖率工具。

### 能输出什么？

```text
总覆盖率
文件覆盖率
函数覆盖率
哪些代码没被测试
```

### 适合

软件工程项目、开源项目、测试报告。

### 风险等级

中风险。

---

## 5.75 POEditor

### 是什么？

本地化和翻译管理平台。

### 能做什么？

```text
中文界面
英文界面
日文界面
多语言 App
多语言网站
```

### 适合

有国际化需求的项目。

### 风险等级

低/中风险。

---

## 5.76 Vaadin

### 是什么？

Java Web 框架。

### 能做什么？

```text
企业级 Web 应用
后台管理系统
Java 全栈应用
```

### 适合

如果你用 Java 做物业管理系统，可以考虑：

```text
Spring Boot + Vaadin + PostgreSQL
```

### 风险等级

低/中风险。

---

## 5.77 PopSQL

### 是什么？

协作型 SQL 编辑器。

### 能做什么？

```text
写 SQL
可视化查询结果
共享查询
团队协作分析数据
```

### 和 DataGrip / SQLGate 区别

PopSQL 更偏协作和分享，DataGrip 更偏专业数据库 IDE。

### 风险等级

低/中风险。

---

## 5.78 Honeybadger

### 是什么？

错误、宕机和定时任务监控工具。

### 能监控什么？

```text
异常错误
网站是否宕机
定时任务是否失败
后端服务是否正常
```

### 和 Sentry 类似

二选一即可。

### 风险等级

中风险。

---

## 5.79 SimpleAnalytics

### 是什么？

隐私友好的网站访问统计工具。

### 能统计什么？

```text
访问人数
页面浏览量
访问来源
热门页面
```

### 适合

个人主页、项目展示页、活动网站。

### 风险等级

低/中风险。

---

## 5.80 Themeisle

### 是什么？

WordPress 主题资源。

### 能做什么？

```text
博客
项目展示站
课程资源站
社团官网
```

### 是否建议现在用？

如果你不用 WordPress，可以不优先。

### 风险等级

低/中风险。

---

## 5.81 ConfigCat

### 是什么？

Feature Flag 功能开关平台。

### 能做什么？

```text
控制新功能是否开启
灰度发布
A/B 测试
按用户分组开放功能
```

### 适合

较正式的 Web 项目或 SaaS 项目。

### 风险等级

中风险。

---

## 5.82 DevCycle

### 是什么？

Feature Flag 管理平台。

### 能做什么？

```text
功能开关
灰度发布
A/B 测试
实验功能控制
```

### 是否建议现在用？

课程小项目暂时不必优先。

### 风险等级

中风险。

---

## 5.83 Appfigures

### 是什么？

App Store 数据分析平台。

### 能分析什么？

```text
App 下载量
排名
评价
收入表现
用户反馈
```

### 是否建议现在用？

如果你不发布移动 App，可以暂时不用。

### 风险等级

中风险。

---

## 5.84 SymfonyCasts

### 是什么？

Symfony / PHP 学习平台。

### 能学什么？

```text
PHP
Symfony
后端开发
Web 开发
```

### 是否建议现在用？

如果你不学 PHP，可以不优先。

### 风险等级

低/中风险。

---

## 5.85 Dashlane

### 是什么？

密码管理器。

### 能做什么？

```text
保存密码
生成强密码
自动填充
管理安全信息
```

### 和 1Password 怎么选？

二选一即可。你已经有 1Password 资源的话，可以优先 1Password。

### 风险等级

特殊风险。

---

## 5.86 GitHub Certification Voucher

### 是什么？

GitHub 官方认证考试券。

### 能用于什么？

```text
GitHub Foundations Certification
GitHub Copilot Certification
```

### 领取建议

先学习 GitHub 基础，再使用考试券。不要裸考浪费机会。

### 风险等级

低风险。

---

# 6. 中国大学生使用注意事项

## 6.1 通常比较容易使用的资源

```text
GitHub
GitHub Copilot
JetBrains
VS Code
GitHub Desktop
GitHub Pages
Notion
DataCamp
Educative
Scrimba
1Password
```

## 6.2 可能遇到支付或地区问题的资源

```text
Microsoft 365
Azure
DigitalOcean
Heroku
Namecheap
Name.com
.TECH
Stripe
Datadog
New Relic
```

## 6.3 可能遇到网络访问问题的资源

```text
GitHub
Heroku
MongoDB Atlas
DigitalOcean
Notion
部分国外课程平台
```

如果访问不稳定，要根据学校网络、家庭网络和地区情况判断。

## 6.4 证件和学生身份验证注意事项

上传学生证明时：

```text
保留姓名、学校、有效日期等必要信息
遮挡身份证号、家庭住址、证件编号等非必要信息
不要把完整证件图片公开上传 GitHub
```

---

# 7. 针对你的项目的最佳组合

## 7.1 无人机大创项目

推荐组合：

```text
GitHub
GitHub Copilot
PyCharm
CLion
Notion
GitHub Pages
MongoDB Atlas
DigitalOcean / Heroku
Visme
SlideCoach
```

具体用途：

| 工具 | 用途 |
|---|---|
| GitHub | 管理代码和文档 |
| Copilot | 辅助写脚本和节点 |
| PyCharm | YOLO、数据处理、飞行日志分析 |
| CLion | ROS2 C++、PX4 源码阅读 |
| Notion | 项目计划、会议记录、实验记录 |
| GitHub Pages | 项目展示网站 |
| MongoDB | 存储实验记录和检测结果 |
| DigitalOcean / Heroku | 部署展示后端 |
| Visme | 做项目图和汇报材料 |
| SlideCoach | 练习答辩 |

---

## 7.2 YOLO 白色圆桶检测项目

推荐组合：

```text
PyCharm
GitHub Copilot
GitHub
MongoDB Atlas
Deepnote
DataCamp
Heroku / DigitalOcean
Sentry
```

具体用途：

| 工具 | 用途 |
|---|---|
| PyCharm | 写训练、推理、数据处理脚本 |
| Copilot | 辅助生成代码和解释报错 |
| GitHub | 管理数据处理脚本和 README |
| MongoDB | 存储检测结果 |
| Deepnote | 做数据统计和可视化 |
| Heroku / DigitalOcean | 部署 Web API |
| Sentry | 监控线上错误 |

---

## 7.3 软件工程课程设计

推荐组合：

```text
IntelliJ IDEA
DataGrip
GitHub
GitHub Desktop
Heroku / Azure / DigitalOcean
PostgreSQL / MySQL / MongoDB
Codecov
Travis CI
Sentry
```

具体用途：

| 工具 | 用途 |
|---|---|
| IntelliJ IDEA | Java / Spring Boot 后端 |
| DataGrip | 管理数据库 |
| GitHub | 代码版本管理 |
| GitHub Desktop | 图形化提交 |
| Heroku / Azure | 部署项目 |
| Codecov | 测试覆盖率 |
| Travis CI | 自动测试 |
| Sentry | 线上错误监控 |

---

## 7.4 数学建模 / 数据分析

推荐组合：

```text
Deepnote
DataCamp
PyCharm
GitHub
MongoDB Atlas
GitHub Pages
```

具体用途：

| 工具 | 用途 |
|---|---|
| Deepnote | 云端 Notebook 协作 |
| DataCamp | 补数据分析技能 |
| PyCharm | 本地 Python 开发 |
| GitHub | 保存代码 |
| MongoDB | 保存结果数据 |
| GitHub Pages | 展示模型结果 |

---

## 7.5 个人作品集与 GitHub Campus Experts

推荐组合：

```text
Profile README
GitHub Pages
Intro to Open Source
OpenSauced
GitHub Foundations Certification
SlideCoach
Notion
```

具体用途：

| 工具 | 用途 |
|---|---|
| Profile README | GitHub 主页展示 |
| GitHub Pages | 个人网站 |
| OpenSauced | 开源贡献分析 |
| Certification | 官方认证 |
| SlideCoach | 演讲练习 |
| Notion | 活动策划和资料管理 |

---

# 8. 最终行动计划

## 8.1 今天就做

```text
开启 GitHub Copilot
领取 JetBrains 学生授权
安装 PyCharm / CLion / IntelliJ IDEA
配置 GitHub Desktop
创建 GitHub Profile README
```

## 8.2 本周完成

```text
用 GitHub Pages 搭建个人主页
整理一个课程项目仓库
整理无人机项目 README
用 Notion 建立项目工作台
配置 1Password 管理重要账号
```

## 8.3 有项目后再做

```text
用 Heroku 部署一个 Flask / FastAPI Demo
用 MongoDB Atlas 存项目数据
用 Sentry 监控线上错误
用 Doppler 管理环境变量
用 DigitalOcean 学 Linux 服务器部署
```

## 8.4 长期目标

```text
准备 GitHub Foundations Certification
参与开源项目
申请 GitHub Campus Experts
完善个人作品集网站
把大创项目做成公开展示项目
```

---

# 9. 最终建议

你不要把 GitHub Student Developer Pack 当成“免费工具集合”，而应该把它当成一条学生开发者成长路线。

最适合你的路线是：

```text
GitHub Copilot + JetBrains
解决写代码效率问题

GitHub + GitHub Desktop + GitHub Pages
解决项目管理和展示问题

Notion + 1Password + Doppler
解决项目资料、账号、密钥管理问题

Heroku / DigitalOcean / Azure + MongoDB
解决项目部署和数据库问题

GitHub Certification + Campus Experts
解决履历和社区影响力问题
```

最重要的是：  
**先把一个项目整理好、部署出来、写好 README、放到 GitHub 主页上。**  
这样这些学生权益才会真正变成你的能力证明。

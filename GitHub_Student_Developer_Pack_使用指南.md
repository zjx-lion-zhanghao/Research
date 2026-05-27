# GitHub Student Developer Pack 使用指南

> 适用对象：已经通过或准备通过 GitHub Education 学生认证的中国大学生。  
> 目标：说明 GitHub Student Developer Pack 里常见资源的作用、适合做什么、如何组合使用，以及哪些资源应该优先领取。

---

## 1. GitHub Student Developer Pack 是什么？

GitHub Student Developer Pack 可以理解为 GitHub 给学生提供的一套“开发者工具礼包”。它不是一个单独的软件，而是一组由 GitHub 和合作伙伴提供的学生权益，通常包括：

- GitHub 高级功能
- AI 编程辅助工具
- 专业 IDE
- 云服务器和部署平台
- 数据库服务
- 域名与 SSL 证书
- 在线课程与学习平台
- DevOps、测试、监控工具
- 文档、项目管理、设计资源
- 开源社区和认证资源

它的核心价值不是“白嫖工具”，而是帮你建立一套完整的开发流程：

```text
学习技能
↓
写代码
↓
管理仓库
↓
部署项目
↓
监控质量
↓
写文档
↓
展示成果
↓
申请认证 / 社区项目 / 实习
```

---

## 2. 最推荐优先领取的资源

| 优先级 | 资源 | 主要用途 |
|---|---|---|
| 最高 | GitHub Copilot | AI 辅助写代码、解释报错、生成 README、辅助调试 |
| 最高 | GitHub Pro | 私有仓库、项目管理、高级 GitHub 权益 |
| 最高 | JetBrains | PyCharm、CLion、IntelliJ IDEA 等专业 IDE |
| 高 | GitHub Codespaces | 云端 VS Code 开发环境，适合轻量项目 |
| 高 | GitHub Pages | 免费部署个人主页、项目展示页、文档网站 |
| 高 | Microsoft Azure | 云服务、Web 部署、数据库、AI 服务 |
| 高 | Heroku | 快速部署后端 API、课程设计、Web 项目 |
| 高 | MongoDB Atlas | 云数据库，适合 Web 项目和实验数据存储 |
| 中高 | Notion | 学习管理、项目计划、文档整理 |
| 中高 | DigitalOcean | 云服务器、Linux、Docker、项目部署 |
| 中 | DataCamp / Educative / FrontendMasters | 数据科学、Web、后端、AI 学习 |
| 中 | GitHub Certification Voucher | GitHub 官方认证考试 |
| 中 | 1Password / Doppler | 管理密码、API Key、数据库密钥 |

---

# 3. GitHub 自身资源

## 3.1 GitHub Pro

### 作用

GitHub Pro 是 GitHub 的高级个人账号权益。学生认证后通常可以免费获得。

### 可以用来做什么？

你可以用它管理自己的课程、实验、大创、比赛和个人项目，例如：

```text
AI导论实验/
软件工程物业管理系统/
无人机大创项目/
YOLO目标检测项目/
数学建模代码/
计算机网络实验/
个人主页/
```

### 具体用途

1. **私有仓库管理课程作业**  
   课程代码、实验报告、未公开项目可以放在 private repository，避免作业代码直接公开。

2. **项目版本管理**  
   比如无人机项目可以分模块管理：

   ```text
   uav-simulation
   uav-yolo-detection
   uav-path-planning
   uav-report-docs
   ```

3. **Issues 管理任务**

   ```text
   [ ] 完成 Gazebo 电厂模型导入
   [ ] 完成 YOLO 数据集标注
   [ ] 完成 ROS2 节点通信
   [ ] 完成路径规划算法测试
   ```

4. **Pull Request 记录开发过程**

   即使一个人做项目，也可以使用分支和 PR：

   ```text
   main
   dev
   feature/yolo-detection
   feature/path-planning
   ```

---

## 3.2 GitHub Copilot

### 作用

GitHub Copilot 是 AI 编程助手，可以在 VS Code、JetBrains、GitHub 网页中辅助写代码。

### 可以用来做什么？

#### 1. 写 Python 实验代码

比如人工智能导论实验、数据分析、机器学习实验：

```python
# 读取 penguin.csv，训练决策树模型，并输出准确率
```

#### 2. 写 YOLO 数据处理脚本

适合辅助完成：

```text
VOC 标注转 YOLO 格式
批量重命名图片
划分 train / val / test 数据集
统计每类标签数量
```

#### 3. 写 ROS2 节点

可以辅助生成：

```text
订阅雷达点云 topic
发布局部障碍物栅格图
订阅相机图像
发布 YOLO 检测结果
```

#### 4. 解释报错

例如：

```text
ModuleNotFoundError: No module named cv2
```

Copilot 可以提示可能需要：

```bash
pip install opencv-python
```

#### 5. 生成 README

可以辅助生成：

```text
项目简介
安装方法
运行命令
参数说明
结果展示
```

### 注意

Copilot 是辅助工具，不是权威答案。尤其涉及以下内容时必须人工检查：

- 飞控参数
- 无人机控制逻辑
- 数据库删除操作
- 云服务器命令
- Token / API Key / 密码
- 付款和账号安全

---

## 3.3 GitHub Codespaces

### 作用

Codespaces 是 GitHub 提供的云端开发环境，可以在浏览器中打开类似 VS Code 的环境。

### 可以用来做什么？

#### 1. 快速运行轻量项目

例如：

```text
main.py
requirements.txt
README.md
```

在 Codespaces 中可以执行：

```bash
pip install -r requirements.txt
python main.py
```

#### 2. 开发轻量 Web 项目

适合：

```text
Flask 小项目
FastAPI 小项目
React 前端
Node.js 后端
个人主页
```

#### 3. 提高项目可复现性

你可以在 README 中说明：

```text
点击 Code → Codespaces → Create codespace
即可在线运行本项目
```

### 不适合做什么？

不适合：

- 大型 Gazebo 图形仿真
- PX4 大型编译
- YOLO 大模型训练
- 长时间 GPU 任务

这些更适合本机、实验室服务器或专门云 GPU。

---

## 3.4 GitHub Pages

### 作用

GitHub Pages 是免费的静态网站托管服务，可以把 GitHub 仓库变成网页。

### 可以用来做什么？

#### 1. 个人主页

可以展示：

```text
姓名
学校
专业
研究方向
项目经历
技术栈
GitHub 项目链接
联系方式
```

#### 2. 项目展示页

例如无人机项目：

```text
复杂室内环境多旋翼无人机设计与飞控算法研究
项目背景
系统架构
PX4 + ROS2 + Gazebo
YOLO 目标识别
路径规划
实验结果
视频展示
```

#### 3. 文档网站

可以配合 MkDocs 或 Docusaurus：

```text
docs/
  index.md
  installation.md
  usage.md
  experiment.md
```

### 推荐你做

建立一个仓库：

```text
你的用户名.github.io
```

把它做成个人作品集网站。

---

## 3.5 GitHub Desktop

### 作用

GitHub 官方图形化 Git 客户端，适合刚开始整理项目时使用。

### 可以用来做什么？

```text
clone 仓库
commit 修改
push 到 GitHub
pull 最新代码
切换分支
解决简单冲突
```

---

# 4. 专业 IDE 与代码工具

## 4.1 JetBrains

### 作用

JetBrains 学生授权可以免费使用专业 IDE，包括：

```text
IntelliJ IDEA Ultimate
PyCharm Professional
CLion
WebStorm
DataGrip
GoLand
Rider
PhpStorm
```

## PyCharm Professional

### 适合

```text
Python
AI
YOLO
OpenCV
数据分析
爬虫
Flask
FastAPI
数学建模
```

### 可以用来做什么？

- YOLO 数据集处理
- OpenCV 图像识别
- 决策树、粒子群、CVRP 算法
- Flask / FastAPI 后端
- Python 实验代码调试
- 数学建模程序调试

---

## CLion

### 适合

```text
C
C++
CMake
ROS2 C++ 节点
PX4 源码阅读
OpenCV C++
算法竞赛
```

### 可以用来做什么？

- 阅读 PX4 源码
- 写 ROS2 C++ 节点
- 写路径规划算法
- 写 Dijkstra / A* / RRT
- 做 C++ 数据结构实验

---

## IntelliJ IDEA Ultimate

### 适合

```text
Java
Spring Boot
Kotlin
后端开发
软件工程项目
数据库课程设计
```

### 可以用来做什么？

- 物业管理系统后端
- 学生管理系统
- Spring Boot + MySQL 项目
- Java 课程实验
- 软件工程课程设计实现

---

## DataGrip

### 适合

```text
MySQL
PostgreSQL
SQLite
SQL Server
MariaDB
```

### 可以用来做什么？

- 查看数据库表
- 写 SQL
- 调试数据库查询
- 看表结构
- 管理软件工程项目数据库

---

## WebStorm

### 适合

```text
JavaScript
TypeScript
Vue
React
Node.js
前端项目
```

### 可以用来做什么？

- 项目展示网站
- 个人主页
- Vue / React 前端
- 后台管理系统前端

---

## 推荐安装顺序

```text
PyCharm Professional
CLion
IntelliJ IDEA Ultimate
DataGrip
```

如果后续做前端，再装 WebStorm。

---

## 4.2 Visual Studio Code

### 作用

VS Code 是轻量代码编辑器，插件生态很强。

### 可以用来做什么？

```text
Python
C++
JavaScript
Markdown
LaTeX
Docker
ROS2
Git
远程 SSH
```

### VS Code 和 JetBrains 怎么选？

| 场景 | 推荐 |
|---|---|
| 轻量编辑、Markdown、远程服务器 | VS Code |
| 大型 Python 项目 | PyCharm |
| C++ / CMake / ROS2 | CLion |
| Java / Spring Boot | IntelliJ IDEA |
| 前端项目 | VS Code 或 WebStorm |

---

## 4.3 GitLens

### 作用

GitLens 是 VS Code 的 Git 增强插件。

### 可以用来做什么？

```text
查看某一行代码是谁写的
查看修改时间
查看对应 commit
查看文件历史
查看分支图
查看 Pull Request 信息
```

适合多人协作项目，比如大创、课程设计、小组作业。

---

# 5. 云服务与部署平台

## 5.1 Microsoft Azure

### 作用

Azure 是微软云服务平台，适合学习云计算、部署 Web、使用数据库和 AI 服务。

### 可以用来做什么？

#### 1. 部署 Web 应用

```text
Flask 后端
FastAPI 后端
Spring Boot 后端
Node.js 后端
```

#### 2. 使用云数据库

```text
Azure SQL
MySQL
PostgreSQL
Cosmos DB
```

#### 3. 使用 AI 服务

```text
图像识别
语音识别
文本分析
机器学习工作流
```

### 注意

使用 Azure 必须注意：

```text
设置预算提醒
不用的服务及时删除
不要开高规格服务器
不要泄露访问密钥
```

---

## 5.2 DigitalOcean

### 作用

DigitalOcean 是面向开发者的云服务器平台。

### 可以用来做什么？

#### 1. 开 Linux 云服务器

部署：

```text
个人网站
Flask / FastAPI API
Spring Boot 后端
数据库
Docker 服务
```

#### 2. 学 Linux 运维

练习：

```bash
ssh 登录服务器
安装 nginx
配置域名
部署网站
配置防火墙
使用 docker
```

### 适合你什么时候用？

当你想学习真正的服务器、Linux、Docker 部署时使用。

---

## 5.3 Heroku

### 作用

Heroku 是应用部署平台，比传统云服务器简单，适合快速把项目部署到网上。

### 可以用来做什么？

#### 1. 部署 Flask / FastAPI

典型文件结构：

```text
app.py
requirements.txt
Procfile
```

#### 2. 部署课程设计

例如：

```text
物业管理系统后端
停车位分配系统
图书管理系统
学生成绩管理系统
```

#### 3. 部署算法 Demo

例如：

```text
输入起点终点
后端运行 A* 或 Dijkstra
返回路径图
```

#### 4. 使用 Heroku Postgres

可用于：

```text
用户表
订单表
维修工单表
仪表读数表
实验记录表
```

### Heroku 和 DigitalOcean 区别

| 项目 | Heroku | DigitalOcean |
|---|---|---|
| 使用难度 | 更简单 | 更自由但更复杂 |
| 适合 | 快速部署应用 | 学服务器和完整部署 |
| 是否需要管服务器 | 基本不用 | 需要 |
| 适合初学者 | 很适合 | 中等 |
| 可控性 | 较低 | 高 |

---

## 5.4 Appwrite

### 作用

Appwrite 是开源后端服务平台，可以快速提供后端能力。

### 可以用来做什么？

```text
用户登录注册
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

---

## 5.5 LocalStack

### 作用

LocalStack 可以在本地模拟 AWS 云服务。

### 可以用来做什么？

模拟：

```text
S3
Lambda
DynamoDB
SQS
API Gateway
```

适合学习云计算、DevOps 和分布式系统。

---

## 5.6 Camber

### 作用

Camber 是面向科学计算、仿真和数据分析的云平台。

### 可以用来做什么？

```text
科学计算
数据分析
仿真实验
批量脚本运行
轻量机器学习
```

### 对无人机项目可能的用途

```text
批量处理实验数据
运行路径规划算法对比
分析飞行日志
处理仿真结果
```

---

# 6. 数据库与数据科学工具

## 6.1 MongoDB Atlas

### 作用

MongoDB 是文档型数据库，Atlas 是云数据库平台。

### 适合存什么？

适合结构灵活的 JSON 数据，例如：

```json
{
  "project": "uav-yolo",
  "image": "test001.jpg",
  "detections": [
    {"label": "white_bucket", "confidence": 0.92}
  ]
}
```

### 可以用来做什么？

#### 1. 存无人机实验数据

```text
飞行任务编号
传感器数据摘要
目标检测结果
路径规划结果
实验时间
实验备注
```

#### 2. 存 Web 项目数据

```text
用户信息
文章内容
项目日志
评论数据
设备数据
```

### MongoDB 和 MySQL 怎么选？

| 场景 | 推荐 |
|---|---|
| 表结构清楚，比如用户、订单、缴费 | MySQL / PostgreSQL |
| 数据结构灵活，比如日志、检测结果、JSON | MongoDB |
| 课程数据库设计 | MySQL / PostgreSQL 更合适 |
| 快速做 Web Demo | MongoDB 很方便 |

---

## 6.2 DataCamp

### 作用

DataCamp 是数据科学学习平台。

### 可以学什么？

```text
Python 数据分析
Pandas
NumPy
机器学习
SQL
数据可视化
统计学
R 语言
```

### 适合你的用途

```text
数学建模数据分析
人工智能导论实验
机器学习基础
数据清洗
图表绘制
```

---

## 6.3 Deepnote

### 作用

Deepnote 是云端 Jupyter Notebook，适合团队协作的数据分析。

### 可以用来做什么？

```text
读取 CSV
数据清洗
统计分析
画图
模型训练
结果解释
```

### 适合场景

- 数学建模协作
- 数据分析报告
- 机器学习实验记录
- 小组项目结果展示

---

## 6.4 SQLGate / PopSQL

### 作用

SQL 数据库管理工具。

### 可以用来做什么？

```text
写 SQL 查询
查看表结构
导出数据
分析数据
管理数据库
```

如果已经使用 DataGrip，可以不急着用 SQLGate / PopSQL。

---

## 6.5 CARTO

### 作用

空间数据分析和地图可视化平台。

### 可以用来做什么？

```text
地理坐标分析
地图数据可视化
空间分布分析
轨迹数据展示
位置分析
```

### 对无人机项目的潜在用途

```text
无人机巡检路径
厂区地图
目标点位置
轨迹可视化
```

如果是纯室内坐标而非经纬度，价值会降低。

---

# 7. Web 开发与前端工具

## 7.1 FrontendMasters

### 作用

高质量前端课程平台。

### 可以学什么？

```text
JavaScript
TypeScript
React
Vue
Node.js
前端工程化
Web 性能
```

适合做个人主页、项目展示网站、后台管理系统、数据可视化界面。

---

## 7.2 Scrimba

### 作用

交互式编程学习平台，适合前端入门。

### 可以学什么？

```text
HTML
CSS
JavaScript
React
Python
```

---

## 7.3 Boot.dev

### 作用

后端和 DevOps 学习平台。

### 可以学什么？

```text
Python
Go
TypeScript
后端开发
HTTP
数据库
DevOps
```

适合从“写脚本”提升到“写完整后端服务”。

---

## 7.4 Codedex

### 作用

游戏化编程学习平台。

### 适合补什么？

```text
Python
HTML
CSS
JavaScript
React
Git
命令行
```

---

## 7.5 Bootstrap Studio

### 作用

可视化网站设计工具，基于 Bootstrap。

### 可以用来做什么？

```text
个人主页
项目展示页
课程设计前端页面
活动报名页面
```

---

## 7.6 Polypane

### 作用

专业浏览器测试工具，可以同时查看网页在不同屏幕尺寸下的效果。

### 可以测试什么？

```text
手机
平板
笔记本
大屏幕
```

---

## 7.7 LambdaTest / BrowserStack

### 作用

跨浏览器测试平台。

### 可以测试什么？

```text
Chrome
Safari
Firefox
Edge
iOS
Android
Windows
macOS
```

如果只是课程小项目，不是优先项。

---

## 7.8 Pageclip

### 作用

静态网站表单后端服务。

### 可以用来做什么？

不写后端也能收集表单：

```text
姓名
邮箱
报名信息
反馈意见
```

适合社团活动报名页、项目反馈表、课程资源申请表。

---

# 8. 后端、认证与 API 工具

## 8.1 Clerk

### 作用

用户认证和用户管理平台。

### 可以提供什么？

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

---

## 8.2 Stripe

### 作用

在线支付平台。

### 可以学习什么？

```text
支付接口
订单系统
订阅收费
SaaS 项目
```

大多数课程项目暂时不需要。

---

## 8.3 Requestly

### 作用

拦截、修改、模拟 HTTP 请求和响应。

### 可以用来做什么？

```text
把 API 请求重定向到本地
模拟接口返回
修改请求头
测试不同环境接口
```

---

## 8.4 Testmail

### 作用

邮件测试工具。

### 可以测试什么？

```text
注册验证码
找回密码
邮箱确认
通知邮件
```

---

## 8.5 Doppler

### 作用

密钥和环境变量管理工具。

### 可以管理什么？

```text
API_KEY
DATABASE_URL
GITHUB_TOKEN
OPENAI_API_KEY
JWT_SECRET
```

### 为什么重要？

不要把这些写进 GitHub 仓库：

```python
api_key = "sk-xxxxxxx"
password = "123456"
```

应该使用环境变量或密钥管理工具。

---

# 9. DevOps、测试、监控与代码质量

## 9.1 Travis CI

### 作用

持续集成平台。

### 可以自动做什么？

```text
安装依赖
运行测试
检查格式
构建项目
部署项目
```

---

## 9.2 Codecov

### 作用

代码覆盖率工具。

### 可以输出什么？

```text
总覆盖率：78%
登录模块：92%
缴费模块：65%
维修模块：40%
```

适合软件工程项目测试报告。

---

## 9.3 CodeScene

### 作用

代码质量分析工具。

### 可以分析什么？

```text
代码复杂度
技术债
高风险文件
热点代码
维护难度
```

---

## 9.4 DeepScan

### 作用

JavaScript / TypeScript 代码质量分析工具。

### 适合

```text
React
Vue
Node.js
JavaScript
TypeScript
```

---

## 9.5 Blackfire

### 作用

代码性能分析工具。

### 可以发现什么？

```text
哪个函数最慢
哪个接口耗时最高
数据库查询是否拖慢系统
```

---

## 9.6 Sentry

### 作用

线上错误监控平台。

### 可以记录什么？

```text
错误类型
错误堆栈
发生时间
用户环境
浏览器信息
```

如果你部署 Flask / FastAPI / React 项目，可以接入 Sentry。

---

## 9.7 New Relic / Datadog

### 作用

专业监控平台。

### 可以监控什么？

```text
服务器 CPU
内存
请求耗时
数据库性能
日志
应用错误
API 性能
```

初学阶段可以了解，不必一开始就用。

---

# 10. 安全、密码与网站防护

## 10.1 1Password

### 作用

密码管理器。

### 可以保存什么？

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

### 为什么建议使用？

你后续会注册大量开发者平台，密码和密钥管理会越来越复杂。密码管理器可以避免重复使用弱密码。

---

## 10.2 Dashlane

也是密码管理器。和 1Password 二选一即可。

---

## 10.3 AstraSecurity

### 作用

网站安全检测、防火墙和恶意软件扫描工具。

### 可以检查什么？

```text
网站漏洞
恶意代码
安全风险
防火墙
```

等你有线上网站后再考虑。

---

# 11. 域名、SSL 与网站基础设施

## 11.1 Name.com

### 作用

域名注册平台。

### 可以申请什么？

```text
yourname.dev
yourproject.app
uav-lab.dev
```

可以绑定到：

```text
GitHub Pages
Heroku
DigitalOcean
个人网站
```

---

## 11.2 Namecheap

### 作用

域名和 SSL 服务商。

### 可以用来做什么？

```text
.me 域名 1 年
SSL 证书 1 年
```

例如：

```text
zhuojunxiong.me
uav-project.me
cqu-uav-demo.me
```

---

## 11.3 .TECH

### 作用

.tech 技术类域名。

### 可以申请

```text
zjx.tech
uavlab.tech
ros2uav.tech
```

适合个人技术主页或项目官网。

---

# 12. 文档、效率与技术写作

## 12.1 Notion

### 作用

文档、知识库、项目管理工具。

### 可以用来做什么？

#### 1. 管理课程

```text
工程材料
数字信号处理
计算机网络
人工智能导论
软件工程
```

每门课可以建立：

```text
课程资料
复习笔记
作业记录
考试重点
错题整理
```

#### 2. 管理大创项目

```text
项目目标
技术路线
每周进度
任务分工
实验记录
会议纪要
问题清单
```

#### 3. 管理论文和资料

```text
文献标题
阅读状态
核心观点
引用格式
可用段落
```

---

## 12.2 Notion Template Collection

### 作用

Notion 模板集合。

### 可以直接套用

```text
CS 课程仪表盘
Hackathon 管理模板
作品集模板
项目计划模板
学习计划模板
```

---

## 12.3 AI Prompting & Technical Writing

### 作用

学习 AI 提示词和技术写作。

### 适合你写

```text
实验报告
GitHub README
项目教程
大创中期报告
软件工程文档
数模论文说明
```

---

## 12.4 POEditor

### 作用

本地化和翻译管理平台。

### 适合

```text
中文
英文
日文
```

多语言网站或 App。

---

# 13. AI、机器学习与数据方向

## 13.1 Data Science & Machine Learning Experience

### 作用

数据科学与机器学习资源组合。

### 可以学习

```text
数据收集
数据清洗
数据分析
可视化
机器学习
协作开发
```

### 适合任务

```text
YOLO 数据集统计
飞行日志分析
传感器数据处理
数学建模数据分析
路径规划算法对比
```

---

## 13.2 Educative

### 作用

交互式编程课程平台。

### 可以学

```text
Web Development
Python
Java
Machine Learning
系统设计
算法
```

---

## 13.3 AlgoExpert / InterviewCake

### 作用

算法面试准备平台。

### 可以练习

```text
数组
链表
栈
队列
树
图
动态规划
递归
排序
搜索
```

适合准备实习、笔试、保研机试、算法基础。

---

# 14. 移动开发与硬件相关

## 14.1 Mobile App Development Experience

### 作用

移动应用开发资源组合。

### 可以学习

```text
移动 App 设计
前端开发
测试
部署
```

### 对无人机项目的潜在用途

可以尝试做手机端界面：

```text
查看无人机状态
查看巡检结果
显示目标识别图片
查看任务记录
```

---

## 14.2 NativeScript

### 作用

用 JavaScript / TypeScript 开发跨平台 App。

### 可以开发

```text
Android
iOS
```

---

## 14.3 Arduino

### 作用

开源硬件平台和云服务。

### 可以做

```text
传感器实验
物联网
嵌入式入门
硬件原型
```

### 对无人机方向可能有用

```text
传感器采集
环境监测
简单硬件验证
电池电压监控
外设模块测试
```

---

## 14.4 Adafruit

### 作用

开源硬件和电子模块平台。

### 可以学习

```text
传感器
开发板
物联网
LED
电机控制
数据采集
```

---

# 15. 设计、图标、展示与演讲

## 15.1 IconScout / Icons8 / Octicons

### 作用

图标和设计素材库。

### 可以用于

```text
PPT 图标
项目 README 图标
网页图标
系统架构图
流程图
UI 设计
```

---

## 15.2 Visme

### 作用

在线演示文稿、信息图、视觉文档制作平台。

### 可以做

```text
PPT
项目介绍图
数据可视化
汇报材料
宣传图
```

---

## 15.3 SlideCoach

### 作用

AI 演讲训练工具。

### 可以练习

```text
大创中期答辩
项目路演
英语口语展示
比赛汇报
GitHub Campus Expert 申请陈述
```

---

## 15.4 ToDiagram

### 作用

把 JSON、YAML、CSV、XML 转成可编辑图表。

### 可以做

```text
系统架构图
树结构
流程图
网络拓扑
数据关系图
```

---

# 16. 爬虫、自动化与网络测试

## 16.1 Zyte

### 作用

Scrapy Cloud 爬虫平台。

### 可以用来做什么？

```text
网页数据抓取
定时爬取
数据保存
爬虫部署
```

### 注意

爬虫要遵守网站 robots.txt、版权和服务条款，不要抓隐私数据，不要高频请求。

---

## 16.2 Blockchair

### 作用

区块链数据 API。

### 可以查询

```text
比特币交易
以太坊交易
地址信息
链上数据
```

如果不做区块链项目，可以暂时不用。

---

# 17. 其他开发工具

## 17.1 Xojo

### 作用

跨平台应用开发工具。

### 可以开发

```text
桌面软件
移动应用
Web 应用
Raspberry Pi 应用
```

---

## 17.2 Vaadin

### 作用

Java Web 框架，适合企业级 Web 应用。

### 适合

```text
Spring Boot + Vaadin + PostgreSQL
```

可以用于管理系统类项目。

---

## 17.3 GoRails / SymfonyCasts

### 作用

Web 后端课程平台。

- GoRails：Ruby on Rails 教程
- SymfonyCasts：Symfony / PHP 教程

如果不学 Ruby 或 PHP，可以不优先使用。

---

# 18. 分析、用户行为与产品工具

## 18.1 SimpleAnalytics

### 作用

隐私友好的网站访问统计工具。

### 可以统计

```text
访问人数
页面浏览量
来源
热门页面
```

适合个人主页、项目展示页。

---

## 18.2 Appfigures

### 作用

App Store 数据分析工具。

### 适合

```text
移动 App 下载量
评价
排名
性能趋势
```

如果不发布移动 App，可以暂时不用。

---

## 18.3 DevCycle / ConfigCat

### 作用

Feature Flag 功能开关平台。

### 可以做

```text
新功能灰度发布
实验功能临时关闭
A/B 测试
按用户分组开放功能
```

课程小项目暂时不必优先用。

---

## 18.4 PomoDone / HazeOver

### PomoDone

番茄钟时间管理工具，适合：

```text
复习
写代码
写报告
项目开发
```

### HazeOver

Mac 专注工具，突出当前窗口，弱化其他窗口。

---

# 19. 开源、社区与认证

## 19.1 Intro to GitHub

### 作用

GitHub Flow 入门学习资源。

### 可以学

```text
创建仓库
创建分支
提交 commit
Pull Request
代码审查
合并分支
```

---

## 19.2 Intro to Open Source

### 作用

开源入门资源。

### 可以学

```text
什么是开源
如何找 good first issue
如何贡献代码
如何维护项目
如何写贡献指南
```

---

## 19.3 Profile README

### 作用

GitHub 个人主页 README。

### 可以展示

```text
自我介绍
研究方向
技术栈
项目链接
GitHub 统计
联系方式
```

推荐建立同名仓库：

```text
你的GitHub用户名/你的GitHub用户名
```

然后写 `README.md` 作为个人主页。

---

## 19.4 GitHub Foundations Certification

### 作用

GitHub 官方基础认证。

### 可以证明你掌握

```text
GitHub 基础
仓库管理
Issues
Pull Requests
GitHub Actions 基础
安全和协作
```

可以放在简历、GitHub 主页、LinkedIn、申请材料中。

---

## 19.5 GitHub Certification Voucher

### 作用

GitHub 认证考试券。

### 建议

先学习 Intro to GitHub 和 GitHub Foundations，再考虑考试，不建议直接裸考。

---

## 19.6 GitHub Campus Experts

### 作用

GitHub 学生校园专家项目。

### 可以围绕这些主题做校园活动

```text
GitHub 入门
AI 编程工具使用
开源项目贡献
无人机与 ROS2 技术分享
YOLO 目标检测实践
大学生如何做技术作品集
```

---

# 20. Microsoft 相关资源

## 20.1 Microsoft 365

### 作用

包括 Word、Excel、PowerPoint、OneDrive、Copilot 等生产力工具。

### 可以用来做

```text
实验报告
项目计划书
PPT
论文
文件保存
团队协作
```

### 注意

如果是学生试用，务必检查：

```text
是否自动续费
地区是否匹配
是否需要银行卡
免费期结束后价格
```

---

## 20.2 Microsoft Visual Studio Dev Essentials

### 作用

微软开发者工具包。

### 包括

```text
Visual Studio Community
Azure 服务
学习资源
开发工具
```

适合 C#、.NET、Windows 开发、Azure 学习。

---

# 21. 可以做出的具体项目

## 项目 1：个人技术主页

### 使用资源

```text
GitHub Pages
Name.com / Namecheap / .TECH
IconScout / Icons8
SimpleAnalytics
```

### 内容

```text
个人介绍
专业方向
项目经历
技术栈
GitHub 仓库链接
大创项目展示
课程项目展示
```

### 成果

一个可以放进简历、申请材料、GitHub Profile 的个人网站。

---

## 项目 2：无人机大创项目展示网站

### 使用资源

```text
GitHub
GitHub Pages
Notion
Visme
IconScout
SimpleAnalytics
```

### 内容

```text
项目背景
技术路线
PX4 + ROS2 + Gazebo 架构
YOLO 检测流程
路径规划算法
实验结果
PPT / 报告下载
GitHub 代码链接
```

---

## 项目 3：YOLO 白色圆桶检测项目

### 使用资源

```text
PyCharm
GitHub
GitHub Copilot
MongoDB Atlas
Heroku / DigitalOcean
Sentry
```

### 内容

```text
数据集整理
模型训练
推理脚本
检测结果可视化
Web API
上传图片返回检测结果
```

---

## 项目 4：软件工程物业管理系统

### 使用资源

```text
IntelliJ IDEA
DataGrip
GitHub
Heroku / Azure / DigitalOcean
PostgreSQL / MySQL
Codecov
Travis CI
Sentry
```

### 内容

```text
用户登录
房屋管理
缴费管理
维修工单
仪表读数
后台管理
测试报告
部署链接
```

---

## 项目 5：数学建模算法可视化平台

### 使用资源

```text
PyCharm
Deepnote
DataCamp
GitHub Pages
Heroku
MongoDB
```

### 内容

```text
上传数据
运行算法
展示图表
输出结果
比较不同参数
```

适合展示：

```text
粒子群算法
CVRP
停车位分配
路径规划
```

---

## 项目 6：GitHub Campus Expert 申请材料准备

### 使用资源

```text
GitHub Profile README
GitHub Pages
Notion
SlideCoach
GitHub Foundations Certification
Intro to Open Source
```

### 内容

```text
个人技术主页
开源贡献记录
校园技术活动策划
项目教程
演讲练习
认证证书
```

---

# 22. 推荐实际使用路线

## 第一阶段：马上领取并配置

```text
GitHub Pro
GitHub Copilot
JetBrains
GitHub Desktop
GitHub Pages
Notion
```

目标：提高日常学习和项目开发效率。

---

## 第二阶段：整理作品集

```text
GitHub Profile README
个人主页
项目 README
课程项目仓库整理
无人机项目展示页
```

目标：让你的项目看起来像正式作品。

---

## 第三阶段：学习部署

```text
Heroku
DigitalOcean
Azure
MongoDB Atlas
Appwrite
```

目标：把项目从“本地能跑”变成“别人能访问”。

---

## 第四阶段：提高工程质量

```text
Travis CI
Codecov
Sentry
Datadog
New Relic
Doppler
1Password
```

目标：学习真实软件工程流程。

---

## 第五阶段：社区和认证

```text
GitHub Foundations Certification
GitHub Campus Experts
Intro to Open Source
SlideCoach
```

目标：提升履历和社区影响力。

---

# 23. 不建议一开始乱开的资源

这些资源有用，但不建议一开始全部开：

```text
Stripe
Datadog
New Relic
DigitalOcean
Azure
Heroku
BrowserStack
LambdaTest
AstraSecurity
```

原因是它们可能涉及：

```text
付款方式
额度限制
自动续费
云资源计费
第三方授权
```

建议等到真正需要部署项目或测试网站时再开。

---

# 24. 安全使用原则

## 24.1 看授权权限

相对安全的权限：

```text
读取公开资料
读取邮箱
确认学生身份
```

需要谨慎的权限：

```text
读取私有仓库
写入仓库
管理 organization
管理 SSH Key
管理 Webhook
删除仓库
```

如果一个普通学习平台要求写入你所有仓库，要谨慎。

---

## 24.2 不要上传敏感信息

不要把这些提交到 GitHub：

```text
API Key
数据库密码
银行卡信息
身份证号
学生证完整敏感信息
GitHub Token
服务器私钥
```

---

## 24.3 云服务要防止扣费

开通 Azure、DigitalOcean、Heroku 前检查：

```text
是否需要绑定银行卡
免费额度是多少
是否自动续费
超额怎么收费
如何关闭服务
```

---

## 24.4 项目结束后删除不用的云服务

不用的资源要及时关闭：

```text
服务器
数据库
对象存储
应用实例
监控服务
```

---

# 25. 最适合你的组合方案

## 组合 A：日常写代码

```text
GitHub + GitHub Copilot + JetBrains + GitHub Desktop
```

适合：

```text
课程代码
实验代码
AI 项目
YOLO 项目
ROS2 节点
软件工程项目
```

---

## 组合 B：做个人作品集

```text
GitHub Pages + Profile README + Namecheap / .TECH + Icons8
```

适合：

```text
个人主页
项目展示
简历链接
GitHub 影响力
```

---

## 组合 C：部署课程项目

```text
Heroku / DigitalOcean + MongoDB Atlas / PostgreSQL + Sentry
```

适合：

```text
在线 Web 系统
API 服务
数据库项目
课程设计展示
```

---

## 组合 D：无人机大创项目沉淀

```text
GitHub + Notion + GitHub Pages + PyCharm + CLion + Copilot
```

适合：

```text
代码管理
文档整理
实验记录
仿真脚本
目标检测
路径规划
项目汇报网站
```

---

## 组合 E：数学建模和数据分析

```text
Deepnote + DataCamp + PyCharm + GitHub
```

适合：

```text
数据清洗
模型训练
图表绘制
协作分析
论文结果复现
```

---

# 26. 最终建议

你当前最应该做的不是全部开通，而是先完成这 5 件事：

1. 开启 GitHub Copilot；
2. 领取 JetBrains 学生授权；
3. 整理 GitHub Profile README；
4. 用 GitHub Pages 做个人项目主页；
5. 选一个课程项目或无人机项目部署成在线 Demo。

这样 GitHub Student Developer Pack 才会真正变成你的能力证明，而不是一堆没用过的免费权益。

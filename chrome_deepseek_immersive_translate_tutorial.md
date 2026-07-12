# Chrome 使用 DeepSeek API 提升英文翻译质量

> 使用 **沉浸式翻译（Immersive Translate）** 调用自己的 DeepSeek API，实现网页、PDF、技术文档和字幕的中英双语翻译。

## 目录

- [1. 效果与适用场景](#1-效果与适用场景)
- [2. 准备工作](#2-准备工作)
- [3. 创建 DeepSeek 翻译服务](#3-创建-deepseek-翻译服务)
- [4. 推荐模型选择](#4-推荐模型选择)
- [5. 配置 API 与模型](#5-配置-api-与模型)
- [6. 配置高质量翻译提示词](#6-配置高质量翻译提示词)
- [7. 推荐请求参数](#7-推荐请求参数)
- [8. 设为默认翻译服务](#8-设为默认翻译服务)
- [9. 技术术语优化](#9-技术术语优化)
- [10. 测试与验证](#10-测试与验证)
- [11. 推荐的双服务方案](#11-推荐的双服务方案)
- [12. 常见问题](#12-常见问题)
- [13. 安全注意事项](#13-安全注意事项)
- [14. 参考资料](#14-参考资料)

---

## 1. 效果与适用场景

配置完成后，可以直接在 Chrome 中调用自己的 DeepSeek API 翻译：

- 英文网页
- GitHub README 和 Issues
- PX4、ROS 2、Docker 等技术文档
- 英文学术论文和 PDF
- YouTube 字幕
- 选中文本和输入框内容

推荐使用中英双语对照模式，便于检查术语、公式、变量名和技术参数是否被误译。

---

## 2. 准备工作

需要提前准备：

1. Chrome 浏览器
2. 沉浸式翻译扩展
3. DeepSeek 开放平台账号
4. 已创建的 DeepSeek API Key
5. DeepSeek API 账户中有可用余额

安装沉浸式翻译后，打开：

```text
沉浸式翻译 → 设置 → 翻译服务
```

DeepSeek API Key 只能保存在自己的设备中，不要上传到 GitHub，也不要在截图中公开。

---

## 3. 创建 DeepSeek 翻译服务

在沉浸式翻译中进入：

```text
设置 → 翻译服务 → 添加自定义翻译服务
```

也可以直接找到内置的 DeepSeek 服务进行配置。

建议将服务名称设置为：

```text
DeepSeek V4 Pro 高质量翻译
```

这样在多个翻译服务之间切换时更容易识别。

---

## 4. 推荐模型选择

DeepSeek 当前 API 提供以下两个主要模型：

| 模型 | 特点 | 推荐场景 |
|---|---|---|
| `deepseek-v4-pro` | 质量优先，复杂语境和专业内容表现更好 | 论文、技术文档、正式资料 |
| `deepseek-v4-flash` | 速度更快，成本较低 | 普通网页、新闻、日常阅读 |

### 质量优先

选择：

```text
deepseek-v4-pro
```

### 速度优先

选择：

```text
deepseek-v4-flash
```

旧模型名：

```text
deepseek-chat
deepseek-reasoner
```

已经进入弃用阶段，不建议新配置继续使用。

---

## 5. 配置 API 与模型

按以下方式填写。

### API Key

```text
sk-xxxxxxxxxxxxxxxxxxxxxxxx
```

此处填写自己从 DeepSeek 开放平台创建的 API Key。

### API 接口地址

推荐使用完整地址：

```text
https://api.deepseek.com/chat/completions
```

部分版本也支持填写 Base URL：

```text
https://api.deepseek.com
```

如果当前完整地址已经测试成功，不需要修改。

### 模型

质量优先：

```text
deepseek-v4-pro
```

速度优先：

```text
deepseek-v4-flash
```

如果下拉菜单中已经存在对应模型，不要勾选“输入自定义模型名称”。

### AI 专家

初次使用建议选择：

```text
通用
```

后续可以根据网页类型切换技术、学术或其他翻译策略。

### AI 智能上下文

若当前版本和会员权限支持，建议开启。

它可以向模型提供网页标题、摘要和术语信息，改善长网页中不同段落之间的一致性。

---

## 6. 配置高质量翻译提示词

沉浸式翻译支持自定义系统提示词、单段提示词和多段提示词。

官方多段翻译默认使用 `%%` 作为段落分隔符，因此不要随意删除或改变分隔符规则。

### 6.1 系统提示词

将系统提示词替换为：

```text
You are a professional translator specializing in English-to-Simplified-Chinese translation for technical, academic, and general web content.

## Translation Rules

1. Translate accurately, naturally, and completely from {{from}} to {{to}}.
2. Output only the translated content. Do not explain, summarize, comment, or add any additional information.
3. Preserve exactly the same number, order, and structure of paragraphs as the source text.
4. If the input contains %% separators, preserve exactly the same number of %% separators at the corresponding paragraph boundaries. If the input contains no %%, do not add any.
5. Preserve HTML tags, Markdown syntax, headings, lists, links, and formatting, and place them correctly in the translated content.
6. Do not translate code, commands, API names, model names, variable names, file paths, URLs, formulas, units, parameter names, or standard technical acronyms.
7. Keep technical terms such as PX4, ROS 2, Offboard, NED, FRD, EKF, PID, YOLO, Docker and Jetson unchanged unless a standard Chinese translation is necessary.
8. Use standard Chinese terminology for computer science, robotics, UAVs, control engineering, mechanical engineering, and academic writing.
9. For an important or potentially ambiguous technical term, retain the original English term in parentheses when it first appears.
10. Do not omit repeated content, headings, warnings, notes, examples, or parameter descriptions.
11. Do not merge or split paragraphs.
12. Make the Chinese fluent and readable while strictly preserving the original meaning, logic, degree of certainty, and tone.

{{title_prompt}}{{summary_prompt}}{{terms_prompt}}
```

以下占位符必须保留：

```text
{{from}}
{{to}}
{{title_prompt}}
{{summary_prompt}}
{{terms_prompt}}
```

### 6.2 多段提示词

```text
Translate the following content from {{from}} to {{to}}.

Preserve every paragraph boundary and every %% separator exactly.
Return only the translated content.

{{text}}
```

必须保留：

```text
{{text}}
```

### 6.3 单段提示词

```text
Translate the following content from {{from}} to {{to}}.
Return only the translated content.

{{text}}
```

---

## 7. 推荐请求参数

质量和稳定性优先时，推荐以下参数：

| 参数 | 推荐值 | 说明 |
|---|---:|---|
| 每秒最大请求数 | `5` | 减少瞬时并发过高导致的失败 |
| 每次请求最大文本长度 | `1800` | 提供较完整的上下文 |
| 每次请求最大段落数 | `6` | 兼顾上下文和段落对应 |
| 每次字幕请求最大段落数 | `4` | 字幕需要更低延迟 |
| Temperature | `0` | 降低自由发挥，提高一致性 |
| 富文本翻译 | 关闭 | 优先保证正文和段落稳定 |

推荐配置：

```text
每秒最大请求数：5
每次请求最大文本长度：1800
每次请求最大段落数：6
每次字幕请求最大段落数：4
Temperature：0
富文本翻译：关闭
```

如果出现翻译等待过久、段落错位或请求超时，可以降低为：

```text
每秒最大请求数：3
每次请求最大文本长度：1200
每次请求最大段落数：4
```

---

## 8. 设为默认翻译服务

仅创建服务还不够，需要将其切换为当前使用的翻译服务。

操作步骤：

1. 打开沉浸式翻译设置。
2. 进入“翻译服务”。
3. 找到 `DeepSeek V4 Pro 高质量翻译`。
4. 打开右侧开关。
5. 将它设置为当前默认服务。
6. 打开任意英文网页。
7. 点击浏览器右上角的沉浸式翻译图标。
8. 在翻译服务下拉菜单中再次确认已选择 DeepSeek。
9. 将源语言设置为“英语”。
10. 将目标语言设置为“简体中文”。
11. 选择“中英双语对照”。

如果页面中仍显示“微软翻译”或“谷歌翻译”为当前默认，说明 DeepSeek 还没有真正切换成功。

---

## 9. 技术术语优化

对于无人机、机器人、计算机和机械工程资料，建议在“AI 术语库”中加入固定译法。

示例：

| 英文术语 | 推荐译法 |
|---|---|
| setpoint | 设定值 |
| attitude | 姿态 |
| yaw | 偏航角 |
| pitch | 俯仰角 |
| roll | 横滚角 |
| heading | 航向 |
| body frame | 机体坐标系 |
| local position | 局部位置 |
| global position | 全局位置 |
| flight controller | 飞行控制器 |
| companion computer | 伴随计算机 |
| offboard control | Offboard 外部控制 |
| actuator | 执行器 |
| airframe | 机架 / 机型 |
| trajectory setpoint | 轨迹设定值 |
| ground truth | 真值 |
| pose | 位姿 |
| frame | 坐标系 / 帧，按上下文判断 |

对于具有多种含义的词，不建议在术语库中强制使用唯一译法。例如：

```text
frame
```

在不同上下文中可能表示：

- 坐标系
- 图像帧
- 机架
- 框架

此类词应由模型结合上下文判断。

---

## 10. 测试与验证

在翻译服务配置页点击：

```text
点击测试服务
```

出现绿色对勾，说明以下项目通常已经正常：

- API Key 有效
- API 地址可访问
- 模型名称可用
- 账户余额可用
- 请求格式正确

然后使用以下英文测试：

```text
The vehicle local position is expressed in the NED coordinate frame.
The trajectory setpoint defines the desired position, velocity, acceleration, and yaw.
```

合理结果应接近：

```text
飞行器的局部位置以 NED 坐标系表示。
轨迹设定值定义了期望位置、速度、加速度和偏航角。
```

如果出现以下译法，说明还需要调整提示词或术语库：

```text
车辆本地位置
NED 框架
轨迹设置点
想要的位置
```

---

## 11. 推荐的双服务方案

最实用的方法是同时创建两个服务。

### 服务一：最高质量

```text
名称：DeepSeek V4 Pro 高质量翻译
模型：deepseek-v4-pro
每秒最大请求数：5
最大文本长度：1800
最大段落数：6
Temperature：0
```

适合：

- 论文
- 官方技术文档
- PX4、ROS 2、控制理论资料
- 正式报告
- 复杂长文章

### 服务二：日常快速

```text
名称：DeepSeek V4 Flash 快速翻译
模型：deepseek-v4-flash
每秒最大请求数：8
最大文本长度：1200
最大段落数：4
Temperature：0
```

适合：

- 普通英文网页
- 新闻
- 论坛
- GitHub Issue
- 日常快速阅读

推荐使用方式：

```text
重要技术资料 → V4 Pro
普通网页阅读 → V4 Flash
```

---

## 12. 常见问题

### 12.1 测试服务失败

依次检查：

1. API Key 是否完整
2. API Key 前后是否有空格
3. DeepSeek 账户是否有余额
4. API 地址是否正确
5. 模型名称是否拼写正确
6. 网络是否能访问 DeepSeek API
7. 请求频率是否过高

### 12.2 页面仍然使用微软翻译

说明只配置了 DeepSeek，但没有切换当前服务。

打开网页后，在沉浸式翻译弹窗中手动选择：

```text
DeepSeek V4 Pro 高质量翻译
```

### 12.3 翻译很慢

可以：

- 将模型切换为 `deepseek-v4-flash`
- 将最大文本长度降到 `1200`
- 将最大段落数降到 `4`
- 关闭 AI 智能上下文
- 降低同时翻译的页面内容量

### 12.4 段落错位或漏译

检查提示词中是否保留了：

```text
%%
{{text}}
```

并将参数降为：

```text
最大文本长度：1200
最大段落数：4
Temperature：0
```

### 12.5 技术术语翻译错误

处理顺序：

1. 使用双语对照查看原文
2. 在 AI 术语库中添加固定译法
3. 在系统提示词中补充专业领域
4. 对多义词不要强制唯一翻译
5. 重要资料仍需人工复核

### 12.6 出现 401 错误

通常表示：

- API Key 错误
- API Key 已失效
- API Key 没有被正确保存

### 12.7 出现 429 错误

通常表示请求过于频繁或账户额度受限。

可以将：

```text
每秒最大请求数：10
```

降低为：

```text
每秒最大请求数：3
```

---

## 13. 安全注意事项

### 不要将 API Key 上传到 GitHub

错误示例：

```text
API_KEY=sk-真实密钥
```

正确做法：

```text
API_KEY=sk-xxxxxxxxxxxxxxxx
```

### 不要在仓库中提交以下文件

```text
.env
config.local.json
secrets.json
```

建议在 `.gitignore` 中加入：

```gitignore
.env
.env.*
secrets.json
config.local.json
```

### API Key 泄露后的处理

一旦怀疑泄露，应立即：

1. 登录 DeepSeek 开放平台。
2. 删除原有 API Key。
3. 创建新的 API Key。
4. 更新沉浸式翻译中的配置。
5. 检查 API 使用记录和余额变化。

---

## 14. 参考资料

- [DeepSeek API 文档](https://api-docs.deepseek.com/)
- [DeepSeek API 模型更新说明](https://api-docs.deepseek.com/updates)
- [DeepSeek V4 预览版说明](https://api-docs.deepseek.com/zh-cn/news/news260424)
- [沉浸式翻译：DeepSeek 配置文档](https://immersivetranslate.com/docs/services/deepseek/)
- [沉浸式翻译：AI Prompt 配置指南](https://immersivetranslate.com/docs/prompts/)
- [沉浸式翻译：翻译服务文档](https://immersivetranslate.com/docs/services/)

---

## 免责声明

AI 翻译适合辅助阅读，但不能保证专业术语、法律文本、医学资料和论文内容完全准确。重要资料应结合原文进行人工复核。

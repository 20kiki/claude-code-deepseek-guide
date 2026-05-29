<div align="center">
  <h1>Claude Code + DeepSeek 从零安装教程</h1>
  <p>面向纯小白的完整安装指南 —— 跟着走，30 分钟内拥有自己的 AI 编程助手</p>

  [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
  [![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey)]()
  [![Stars](https://img.shields.io/github/stars/20kiki/claude-code-deepseek-guide)](https://github.com/20kiki/claude-code-deepseek-guide)

  <p><strong>Language:</strong> <a href="../README.md">English</a> | <a href="README.md">简体中文</a></p>
</div>

---

## 📋 目录

- [前言](#-前言)
- [✨ 为什么选择这个方案](#-为什么选择这个方案)
- [🚀 安装步骤](#-安装步骤)
  - [第一步：安装 Git](#第一步安装-git)
  - [第二步：安装 Node.js](#第二步安装-nodejs)
  - [第三步：安装 Claude Code](#第三步安装-claude-code)
  - [跳过首次登录](#跳过首次登录)
  - [第四步：获取 DeepSeek API Key](#第四步获取-deepseek-api-key)
  - [第五步：配置 Claude Code 使用 DeepSeek](#第五步配置-claude-code-使用-deepseek)
  - [第六步：验证是否成功](#第六步验证是否成功)
- [常见问题排查](#常见问题排查)
- [标签](#标签)
- [🤝 贡献指南](#-贡献指南)
- [📄 许可证](#-许可证)

---

## 📖 前言

Claude Code 是 Anthropic 公司推出的**命令行 AI 编程助手**。你在终端里用自然语言描述需求，它就能自动读代码、改代码、执行命令、提交 Git。

DeepSeek 提供了与 Anthropic 完全兼容的 API 端点。只需要修改 4 个环境变量，就能用 DeepSeek 驱动 Claude Code，享受接近原生的编程体验。

## ✨ 为什么选择这个方案

| | Claude Opus 4.7 | DeepSeek V4 Pro |
|:---|:---|:---|
| **输入价格** | ~36 元 / 百万 tokens | 3 元 / 百万 tokens |
| **输出价格** | ~180 元 / 百万 tokens | 6 元 / 百万 tokens |
| **国内直连** | 需要科学上网 | 直接可用 ✅ |
| **上下文长度** | 1M | 1M |

> 价格来源（2026-05-20 核实）：DeepSeek V4 Pro 标准定价输入 ¥3 / 输出 ¥6 每百万 tokens（4 月 26 日永久降价）；缓存命中场景低至 ¥0.025（2.5 折优惠延长至 5 月 31 日）。Claude Opus 4.7 官方定价 $5 / $25 per MTok（按 1 USD ≈ 7.25 CNY 换算）。DeepSeek 输出成本约为 Claude Opus 4.7 的 **三十分之一**。

### 你需要准备什么

- 一台能上网的电脑（Windows / macOS / Linux）
- 一个手机号（注册 DeepSeek 用）
- 支付宝或微信（充值最低 1 元）
- 大约 30 分钟时间

---

## 🚀 安装步骤

### 第一步：安装 Git

Git 是**版本管理工具**。Claude Code 的提交代码、查看改动、切换分支等功能都依赖 Git。

#### Windows

1. 浏览器访问 https://git-scm.com ，点击右侧 **Download for Windows** 按钮，下载会自动开始
2. 得到 `Git-2.xx.x-64-bit.exe`，双击运行
3. 一路点 **Next**（所有选项保持默认即可）
4. 安装完成后，按 `Win + R`，输入 `cmd`，回车

5. 在黑色窗口里输入：

```bash
git --version
```

看到 `git version 2.xx.x` 说明安装成功。

#### macOS

打开终端（按 `Cmd + 空格`，搜索 "终端"），输入：

```bash
git --version
```

如果没装，系统会自动弹窗提示安装 Xcode Command Line Tools，点 **安装** 并等待完成。

或者用 Homebrew：

```bash
brew install git
```

#### Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install git -y
git --version
```

---

### 第二步：安装 Node.js

Claude Code 运行在 Node.js 上，必须先安装。

#### Windows

1. 访问 https://nodejs.org ，点击左边 **LTS** 绿色按钮下载
2. 得到 `node-v20.xx.x-x64.msi`，双击运行
3. **关键步骤**：安装过程中有一个 "Custom Setup" 页面，确保 **"Add to PATH"** 是勾选的（默认就是）

4. 一路 Next 直到完成
5. **关闭旧终端窗口，重新打开一个新的 cmd**，输入：

```bash
node --version
npm --version
```

分别显示版本号（如 `v20.12.2` 和 `10.5.0`）即安装成功。

#### macOS

```bash
brew install node
```

或访问 https://nodejs.org 下载 `.pkg` 安装包。验证：

```bash
node --version
npm --version
```

#### Linux (Ubuntu/Debian)

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
node --version
npm --version
```

---

### 第三步：安装 Claude Code

Node.js 装好后，用自带的 npm 一行命令安装。

> 💡 **国内用户先设置 npm 镜像**（否则下载很慢或失败）：
>
> ```bash
> npm config set registry https://registry.npmmirror.com
> ```
>
> 验证是否生效：
> ```bash
> npm config get registry
> # 应输出 https://registry.npmmirror.com
> ```

```bash
npm install -g @anthropic-ai/claude-code
```

> **Windows 用户**：用管理员身份打开终端再执行（右键开始菜单 → 终端（管理员）），否则可能权限不足。

安装约 1-2 分钟。完成后验证：

```bash
claude --version
```

显示版本号（例如 `2.1.144 (Claude Code)`）即成功。

---

### 跳过首次登录

首次运行 `claude` 时，Claude Code 会要求登录 Anthropic 账号。因为我们用的是 DeepSeek 后端，所以需要跳过这一步。创建 `.claude.json` 文件即可：

#### 配置文件位置

| 系统 | 路径 |
|:---|:---|
| **Windows** | `C:\Users\你的用户名\.claude\.claude.json` |
| **macOS / Linux** | `~/.claude/.claude.json` |

#### 编辑 .claude.json

打开 `.claude.json` 文件，分两种情况处理：

**情况一：文件不存在（首次安装）**

直接新建，写入以下内容即可：

```json
{
  "hasCompletedOnboarding": true
}
```

**情况二：文件已存在（之前装过 Claude Code）**

打开已有的文件，你可能会看到类似这样的内容：

```json
{
  "installMethod": "native",
  "autoUpdates": true
}
```

注意观察：`"autoUpdates": true` 这一行**末尾没有逗号**，因为它是最后一个配置项。

现在我们要加一条新配置，做法是：
1. 在**最后一个配置项的末尾**加上英文逗号 `,`
2. 换行后写上 `"hasCompletedOnboarding": true`

修改后变成：

```json
{
  "installMethod": "native",
  "autoUpdates": true,
  "hasCompletedOnboarding": true
}
```

> 💡 **逗号到底干嘛的？**
>
> JSON 用逗号 `,` 来**分隔同一层级的多个配置项**，类似于中文里顿号的作用——告诉程序「这一项结束了，后面还有下一项」。
>
> 关键规则就两条：
> - **项与项之间**必须加逗号分隔
> - **最后一行的末尾不要加逗号**（它后面没有东西了）
>
> 如果你只在 `{ }` 里写了一个配置项（如情况一），那它既是第一项也是最后一项，自然不需要逗号。

**这段 JSON 具体是什么意思？**

- 最外层的 `{ }` 是 JSON 的**固定格式**，所有配置都要包在这对花括号里面
- `"hasCompletedOnboarding"` 是一个**配置项名称**（键），直译就是「是否已完成首次引导」
- `true` 是它的**值**，表示「是 / 已完成」；如果写成 `false` 就表示「否 / 未完成」
- 中间用英文冒号 `:` 隔开，表示赋值关系
- 整句话就是告诉 Claude Code：「首次登录已经搞定了，别再弹登录框了」

> ⚠️ **JSON 格式提醒**：
> - 花括号 `{ }`、冒号 `:`、逗号 `,` 都必须是**英文半角符号**，不能用中文全角符号（如 `，` `：` `｛｝`），否则文件格式无效
> - 配置项**名称**（如 `"hasCompletedOnboarding"`）必须用英文双引号 `" "` 包起来
> - `true` / `false` 不要加引号，它们是 JSON 的布尔值关键字，加了引号反而出错

**关闭保存文件**。之后再运行 `claude` 就会直接跳过登录界面，进入 AI 对话。

---

### 第四步：获取 DeepSeek API Key

DeepSeek API 是**按量付费**的，用多少扣多少，不是按月订阅。最低充值 1 元，日常使用够用很久。

#### 4.1 注册账号

1. 打开 https://platform.deepseek.com
2. 点击右上角 **登录**，用手机号注册
3. 登录后进入控制台

#### 4.2 充值

1. 左侧菜单点击 **充值**
2. 输入金额（最低 1 元）
3. 用支付宝或微信扫码支付

#### 4.3 创建 API Key

1. 左侧菜单点击 **API Keys**
2. 点击 **创建 API Key**
3. 起个名字（如 `claude-code`）
4. 点创建后**立即复制保存**——Key 只显示一次，关掉页面就找不到了
5. Key 的格式是 `sk-` 开头的一长串字符

> ⚠️ API Key 等于你的账户密码，不要发给任何人，也不要直接写在公开的代码仓库里。

---

### 第五步：配置 Claude Code 使用 DeepSeek

目的是告诉 Claude Code：「别走 Anthropic 官方接口，用 DeepSeek」。

#### 5.1 配置文件位置

| 系统 | 路径 |
|:---|:---|
| **Windows** | `C:\Users\你的用户名\.claude\settings.json` |
| **macOS / Linux** | `~/.claude/settings.json` |

如果文件不存在，新建一个即可。

#### 5.2 编辑 settings.json

用任意文本编辑器打开，写入以下内容（把 `sk-你的...` 换成你自己的 API Key）：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "sk-你的DeepSeek-API-Key",
    "ANTHROPIC_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash",
    "CLAUDE_CODE_SUBAGENT_MODEL": "deepseek-v4-flash",
    "CLAUDE_CODE_EFFORT_LEVEL": "max"
  }
}
```

**字段说明（小白可跳过）**：

| 字段 | 含义 |
|:---|:---|
| `ANTHROPIC_BASE_URL` | API 请求地址，从 Anthropic 改到 DeepSeek |
| `ANTHROPIC_AUTH_TOKEN` | DeepSeek API Key |
| `ANTHROPIC_MODEL` | 主模型，`[1m]` 表示启用 100 万 token 上下文 |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | Opus 角色 → 实际用 DeepSeek V4 Pro |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | Sonnet 角色 → 实际用 DeepSeek V4 Pro |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | Haiku 角色 → 实际用 DeepSeek V4 Flash（轻量快速） |
| `CLAUDE_CODE_SUBAGENT_MODEL` | 子代理用轻量模型，节省 token |
| `CLAUDE_CODE_EFFORT_LEVEL` | `max` = 启用最强推理能力 |

> 💡 **模型选择**：主力推荐 `deepseek-v4-pro[1m]`，轻量任务用 `deepseek-v4-flash`。

#### 5.3 保存

**关闭保存文件**，直接下一步验证。

---

### 第六步：验证是否成功

#### 6.1 启动 Claude Code

打开终端，输入：

```bash
claude
```

首次启动可能需要 1-2 分钟初始化。看到欢迎界面说明启动成功。

#### 6.2 测试对话

输入一个简单问题：

```text
1 + 1 等于几？
```

能正常回复说明配置无误。启动时的系统信息中也会显示类似 `deepseek-v4-pro` 的字样。

#### 6.3 进阶验证

确认请求确实发到了 DeepSeek：

```bash
claude --debug-file debug.log
```

随便问一个问题后退出，打开 `debug.log` 搜索 `api.deepseek.com`，找到了就 100% 确认接入成功。

---

## 常见问题排查

### Q: 运行 `claude` 提示 "command not found" 或 "不是内部命令"

npm 全局安装目录未加入系统 PATH。

**Windows**：重新打开管理员终端再试。如果还不行，运行 `npm config get prefix` 查看路径，手动加到系统环境变量 PATH 中。

**macOS / Linux**：

```bash
echo 'export PATH="$(npm config get prefix)/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Q: 启动后提示 "401 Unauthorized" 或认证失败

1. 检查 `settings.json` 中 `ANTHROPIC_AUTH_TOKEN` 是否正确（`sk-` 开头）
2. 到 DeepSeek 控制台确认 API Key 状态
3. 确认账户有余额

### Q: 响应速度很慢或超时

DeepSeek 高峰期可能排队。

解决：在 `settings.json` 的 `env` 中加一行：

```json
"API_TIMEOUT_MS": "3000000"
```

### Q: Windows 编辑 settings.json 保存后乱码

用记事本保存时编码选错了。另存为时把编码改为 **UTF-8**。推荐用 VS Code 或 Notepad++ 编辑。

### Q: `npm install -g` 报错权限不足（macOS / Linux）

```bash
# 推荐方案：用 nvm 管理 Node.js
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
nvm install 20
nvm use 20

# 备选方案：改 npm 全局目录
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

### Q: 接入 DeepSeek 后工具调用不如原生 Claude 准确

这是已知差异。DeepSeek 部分模型工具调用准确率约 80%，低于 Claude 原生的 98%+。日常使用影响不大。复杂任务建议：
- 拆分成更小的步骤，一次只改一点
- 多确认生成结果

---

## 标签

[`claude-code`](https://github.com/topics/claude-code) [`deepseek`](https://github.com/topics/deepseek) [`ai-coding`](https://github.com/topics/ai-coding) [`tutorial`](https://github.com/topics/tutorial) [`beginner-friendly`](https://github.com/topics/beginner-friendly) [`developer-tools`](https://github.com/topics/developer-tools)

## 🤝 贡献指南

请参阅 [CONTRIBUTING.md](CONTRIBUTING.md)。欢迎提交 Issue 和改进 PR。

## 📄 参考资源

- [Claude Code 官方文档](https://docs.anthropic.com/en/docs/claude-code)
- [DeepSeek API 文档](https://api-docs.deepseek.com/zh-cn/)
- [DeepSeek 价格页](https://api-docs.deepseek.com/zh-cn/quick_start/pricing)

## 👤 作者

[@20kiki](https://github.com/20kiki)

## 📄 许可证

本项目基于 [MIT](LICENSE) 协议开源。

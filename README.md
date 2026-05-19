# Claude Code + DeepSeek 从零安装教程

> 面向纯小白，每一步都有截图和解释。跟着走，30 分钟内搞定。

## 目录

- [前言](#前言)
- [第一步：安装 Git](#第一步安装-git)
- [第二步：安装 Node.js](#第二步安装-nodejs)
- [第三步：安装 Claude Code](#第三步安装-claude-code)
- [第四步：获取 DeepSeek API Key](#第四步获取-deepseek-api-key)
- [第五步：配置 Claude Code 使用 DeepSeek](#第五步配置-claude-code-使用-deepseek)
- [第六步：验证是否成功](#第六步验证是否成功)
- [常见问题排查](#常见问题排查)

---

## 前言

### Claude Code 是什么？

Claude Code 是 Anthropic 公司推出的**命令行 AI 编程助手**。你在终端里用自然语言描述需求，它就能自动读代码、改代码、执行命令、提交 Git。

### 为什么要接入 DeepSeek？

| | Claude 官方 (Opus) | DeepSeek V4 Pro |
|---|---|---|
| **输出价格** | $15 / 百万 tokens | $0.87 / 百万 tokens |
| **国内直连** | 需要科学上网 | 直接可用 |
| **编程能力** | 顶级 | 接近顶级 |
| **上下文长度** | 200K | 1M |

**成本相差 17 倍**，而实际编程体验差距不大，是个人开发者的最佳选择。

### 你需要准备什么？

- 一台能上网的电脑（Windows / macOS / Linux 都行）
- 一个手机号（注册 DeepSeek 用）
- 支付宝或微信（给 DeepSeek 充值，最低 10 元）
- 大约 30 分钟时间

---

## 第一步：安装 Git

Git 是**版本管理工具**。Claude Code 的很多核心功能（提交代码、查看改动、切换分支）都依赖 Git。即使你不写代码，也要先装好。

### Windows

1. 打开浏览器，访问 https://git-scm.com/download/win
2. 下载会自动开始，得到 `Git-2.xx.x-64-bit.exe`
3. 双击运行，一路点 **Next**（所有选项保持默认即可）
4. 安装完成后，按 `Win + R`，输入 `cmd`，回车

![Windows 运行窗口](images/win-run.png)

5. 在黑色窗口里输入：

```bash
git --version
```

如果看到 `git version 2.xx.x`，说明安装成功。

### macOS

打开终端（按 `Cmd + 空格`，搜索 "终端"），输入：

```bash
git --version
```

如果没装，系统会自动弹窗提示安装 Xcode Command Line Tools，点 **安装** 然后等待完成即可。

或者用 Homebrew：

```bash
brew install git
```

### Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install git -y
git --version
```

---

## 第二步：安装 Node.js

Claude Code 是用 Node.js 写的，必须先装 Node.js 才能运行。

### Windows

1. 访问 https://nodejs.org ，点击左边 **LTS** 那个大绿色按钮下载
2. 得到 `node-v20.xx.x-x64.msi`，双击运行
3. **关键步骤**：安装过程中有一页叫 "Custom Setup"，确保 **"Add to PATH"** 这一项前面是勾选的（默认就是勾上的）

![Node.js 安装](images/node-install.png)

4. 一路 Next，直到完成
5. 重新打开一个 cmd 窗口（关掉旧的，新开一个），输入：

```bash
node --version
npm --version
```

分别显示版本号就对了，比如 `v20.12.2` 和 `10.5.0`。

### macOS

```bash
brew install node
```

或去 https://nodejs.org 下载 .pkg 安装包，双击安装。

验证：

```bash
node --version
npm --version
```

### Linux (Ubuntu/Debian)

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
node --version
npm --version
```

---

## 第三步：安装 Claude Code

Node.js 装好后，它自带一个叫 npm 的包管理器。只需要一行命令即可安装 Claude Code：

```bash
npm install -g @anthropic-ai/claude-code
```

**Windows 用户注意**：用管理员身份打开 cmd 或 PowerShell 再执行（右键开始菜单 → 终端（管理员）），否则可能权限不够。

安装过程大约 1-2 分钟。完成后验证：

```bash
claude --version
```

显示版本号（例如 `2.1.144 (Claude Code)`）即安装成功。

---

## 第四步：获取 DeepSeek API Key

DeepSeek 的 API 是**按量付费**的，用多少扣多少，不是按月订阅。最低充值 10 块钱，日常使用够用很久。

### 4.1 注册账号

1. 打开 https://platform.deepseek.com
2. 点击右上角 **登录**，用手机号注册
3. 登录后进入控制台

### 4.2 充值（先充 10 块就行）

1. 左侧菜单点击 **充值**
2. 输入金额（最低 10 元）
3. 用支付宝或微信扫码支付

### 4.3 创建 API Key

1. 左侧菜单点击 **API Keys**
2. 点击 **创建 API Key**
3. 随便起个名字（比如 `claude-code`）
4. 点创建，**立即复制保存**！这个 key 只显示一次，关掉页面就看不到了
5. Key 的格式是 `sk-` 开头的一长串字符

![DeepSeek 控制台](images/deepseek-console.png)

> ⚠️ **重要**：API Key 等于你的账户密码，不要发给任何人，也不要直接写在公开的代码里。

---

## 第五步：配置 Claude Code 使用 DeepSeek

这是最关键的一步。目的就是告诉 Claude Code：「别去 Anthropic 官方了，用 DeepSeek 的接口」。

### 5.1 找到配置文件位置

Claude Code 的所有配置都在一个叫 `settings.json` 的文件里，路径是：

| 系统 | 路径 |
|---|---|
| **Windows** | `C:\Users\你的用户名\.claude\settings.json` |
| **macOS / Linux** | `~/.claude/settings.json` |

如果你的电脑上还没有这个文件，就新建一个。

### 5.2 编辑 settings.json

用在终端里或者记事本打开这个文件，把下面的内容复制进去：

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

**逐行解释（小白可跳过）**：

| 字段 | 含义 |
|---|---|
| `ANTHROPIC_BASE_URL` | 把 API 请求的地址从 Anthropic 官方改成 DeepSeek |
| `ANTHROPIC_AUTH_TOKEN` | 你的 DeepSeek API Key，把 `sk-你的...` 替换成你自己的 |
| `ANTHROPIC_MODEL` | 主模型，`[1m]` 表示启用 100 万 token 上下文 |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | 当 Claude Code 内部调用 Opus 时，实际用 DeepSeek V4 Pro |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | 当 Claude Code 内部调用 Sonnet 时，实际用 DeepSeek V4 Pro |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | 当 Claude Code 内部调用 Haiku 时，实际用 DeepSeek V4 Flash（轻量快速） |
| `CLAUDE_CODE_SUBAGENT_MODEL` | 子代理用轻量模型，省 token |
| `CLAUDE_CODE_EFFORT_LEVEL` | `max` 启用最强推理能力 |

> 💡 **模型选择建议**：
> - `deepseek-v4-pro[1m]` — 最强模型，写代码、复杂任务用这个
> - `deepseek-v4-flash` — 轻量快速，适合简单任务（子代理、文件读写等）
> - 也可以用 `deepseek-chat`（对应 V3.1）或 `deepseek-reasoner`（对应 R1 推理模型）

### 5.3 保存并退出

保存文件后 **不需要重启电脑**，直接下一步验证即可。

---

## 第六步：验证是否成功

### 6.1 启动 Claude Code

打开终端（Windows 用 cmd 或 PowerShell），输入：

```bash
claude
```

首次启动可能需要 1-2 分钟初始化，耐心等待。如果你看到类似下面的欢迎界面，说明启动成功：

```
Welcome to Claude Code!
...
```

### 6.2 确认模型来源

在 Claude Code 里输入一个简单问题，比如：

```
1 + 1 等于几？
```

如果能正常回复，就说明配置完全没问题。

你也可以在启动时的系统信息中看到当前使用的模型，会显示类似 `deepseek-v4-pro` 的字样。

### 6.3 进阶验证

如果你想确认请求确实发到了 DeepSeek 而不是 Anthropic：

```bash
claude --debug-file debug.log
```

然后随便问一个问题后退出，打开 `debug.log` 搜索 `api.deepseek.com`，如果找到了就 100% 确认接入成功。

---

## 常见问题排查

### Q: 运行 `claude` 提示 "command not found" 或 "不是内部命令"

**原因**：npm 的全局安装目录没有加到系统 PATH 里。

**解决**：
- Windows：重新打开一个管理员终端，再试一次 `claude --version`。如果还不行，运行 `npm config get prefix` 查看 npm 全局路径，把它加到系统环境变量的 PATH 里。
- macOS/Linux：运行 `echo 'export PATH="$(npm config get prefix)/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc`

### Q: 启动后提示 "401 Unauthorized" 或认证失败

**原因**：API Key 错误或已过期。

**解决**：
1. 检查 `settings.json` 中的 `ANTHROPIC_AUTH_TOKEN` 是否填写正确（`sk-` 开头）
2. 去 DeepSeek 控制台确认这个 Key 的状态
3. 确认 API 账户有余额（不是欠费状态）

### Q: 响应速度很慢或超时

**原因**：DeepSeek 服务端高峰期可能排队，或者网络波动。

**解决**：
1. 在 `settings.json` 的 `env` 里加一行：`"API_TIMEOUT_MS": "3000000"`（5 分钟超时）
2. 如果是晚间高峰期，换个时间段试试

### Q: Windows 下编辑 settings.json，保存后变成乱码

**原因**：用记事本保存时编码选错了。

**解决**：保存时确保编码选 **UTF-8**。或者用 VS Code / Notepad++ 编辑。

### Q: `npm install -g` 报错权限不足 (macOS/Linux)

```bash
# 方法一：用 nvm 管理 Node.js（推荐，一劳永逸）
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
nvm install 20
nvm use 20

# 方法二：改 npm 全局目录
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

### Q: 接入 DeepSeek 后，工具调用（代码编辑）不如原生 Claude 准确

这是已知的差异。DeepSeek 部分模型的工具调用准确率约 80%，低于 Claude 原生的 98%+。日常使用影响不大，但如果你在做非常复杂的重构任务，可能会遇到编辑偏差。此时可以：
- 把任务拆得更细，一次只改一个小点
- 多确认生成结果

---

## 参考资源

- [Claude Code 官方文档](https://docs.anthropic.com/en/docs/claude-code)
- [DeepSeek API 文档](https://platform.deepseek.com/api-docs)
- [DeepSeek 价格页](https://platform.deepseek.com/pricing)

---

## 许可证

MIT License — 随意使用、修改、分享。

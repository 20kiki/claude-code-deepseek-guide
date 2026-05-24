<div align="center">
  <h1>Claude Code + DeepSeek Installation Guide</h1>
  <p>A complete beginner-friendly guide — get your AI coding assistant running in 30 minutes</p>

  [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
  [![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey)]()
  [![Stars](https://img.shields.io/github/stars/20kiki/claude-code-deepseek-guide)](https://github.com/20kiki/claude-code-deepseek-guide)

  <p><strong>Language:</strong> <a href="README.md">English</a> | <a href="zh-CN/README.md">简体中文</a></p>
</div>

---

## 📋 Table of Contents

- [Preface](#-preface)
- [Why This Setup](#why-this-setup)
- [Installation Steps](#installation-steps)
  - [Step 1: Install Git](#step-1-install-git)
  - [Step 2: Install Node.js](#step-2-install-nodejs)
  - [Step 3: Install Claude Code](#step-3-install-claude-code)
  - [Step 4: Get a DeepSeek API Key](#step-4-get-a-deepseek-api-key)
  - [Step 5: Configure Claude Code for DeepSeek](#step-5-configure-claude-code-for-deepseek)
  - [Step 6: Verify Everything Works](#step-6-verify-everything-works)
- [Troubleshooting](#troubleshooting)
- [Topics](#topics)
- [Contributing](#contributing)
- [License](#license)

---

## 📖 Preface

Claude Code is Anthropic's **command-line AI coding assistant**. You describe what you want in natural language, and it reads code, edits files, runs commands, and commits to Git — all from your terminal.

DeepSeek provides an API endpoint fully compatible with Anthropic's protocol. With just 4 environment variables, you can power Claude Code with DeepSeek and get a near-native coding experience at a fraction of the cost.

## Why This Setup

| | Claude Opus 4.7 | DeepSeek V4 Pro |
|:---|:---|:---|
| **Input Price** | ~$5 / MTok | ~$0.41 / MTok |
| **Output Price** | ~$25 / MTok | ~$0.83 / MTok |
| **Direct Access (China)** | Requires VPN | Works directly ✅ |
| **Context Window** | 1M | 1M |

> Pricing (verified 2026-05-20): DeepSeek V4 Pro at ¥3 input / ¥6 output per million tokens (permanent price drop since April 26); cache-hit scenarios as low as ¥0.025. DeepSeek's output cost is roughly **1/30th** of Claude Opus 4.7.

### What You Need

- A computer with internet (Windows / macOS / Linux)
- A phone number (for DeepSeek registration)
- Alipay or WeChat (minimum top-up: ¥1)
- About 30 minutes

---

## 🚀 Installation Steps

### Step 1: Install Git

Git is a **version control tool**. Claude Code depends on it for commits, diffs, branching, and more.

#### Windows

1. Open https://git-scm.com and click **Download for Windows**
2. Run the downloaded `Git-2.xx.x-64-bit.exe`
3. Click **Next** through all steps (defaults are fine)
4. After installation, press `Win + R`, type `cmd`, and hit Enter

5. In the terminal, type:

```bash
git --version
```

If you see `git version 2.xx.x`, Git is installed successfully.

#### macOS

Open Terminal (`Cmd + Space`, search "Terminal") and run:

```bash
git --version
```

If not installed, macOS will prompt you to install Xcode Command Line Tools — click **Install** and wait.

Or use Homebrew:

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

### Step 2: Install Node.js

Claude Code runs on Node.js — you must install it first.

#### Windows

1. Go to https://nodejs.org and click the green **LTS** button
2. Run the downloaded `node-v20.xx.x-x64.msi`
3. **Important**: During installation, make sure **"Add to PATH"** is checked (on by default)
4. Click Next until finished
5. **Close and reopen your terminal**, then run:

```bash
node --version
npm --version
```

Both should show version numbers (e.g., `v20.12.2` and `10.5.0`).

#### macOS

```bash
brew install node
```

Or download the `.pkg` installer from https://nodejs.org. Verify:

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

### Step 3: Install Claude Code

Once Node.js is set up, install Claude Code with one command.

> 💡 **Users in China — set the npm mirror first** (otherwise downloads may be slow or fail):
>
> ```bash
> npm config set registry https://registry.npmmirror.com
> ```
>
> Verify:
> ```bash
> npm config get registry
> # Should output: https://registry.npmmirror.com
> ```

```bash
npm install -g @anthropic-ai/claude-code
```

> **Windows users**: Run the terminal as Administrator (right-click Start → Terminal (Admin)), otherwise you may hit permission errors.

Installation takes 1–2 minutes. Verify:

```bash
claude --version
```

You should see a version number (e.g., `2.1.144 (Claude Code)`).

---

### Step 4: Get a DeepSeek API Key

DeepSeek API is **pay-as-you-go** — you only pay for what you use, not a monthly subscription. Minimum top-up is ¥1, which lasts a long time for casual use.

#### 4.1 Sign Up

1. Open https://platform.deepseek.com
2. Click **Sign In** (登录) in the top right, register with your phone number
3. You'll land on the console dashboard

#### 4.2 Top Up

1. Click **Top Up** (充值) in the left sidebar
2. Enter an amount (minimum ¥1)
3. Pay with Alipay or WeChat

#### 4.3 Create API Key

1. Click **API Keys** in the left sidebar
2. Click **Create API Key**
3. Give it a name (e.g., `claude-code`)
4. **Copy and save it immediately** — the key is shown only once
5. The key starts with `sk-` followed by a long string

> ⚠️ Treat your API key like a password. Never share it or commit it to a public repository.

---

### Step 5: Configure Claude Code for DeepSeek

The goal: tell Claude Code to route all requests through DeepSeek instead of Anthropic.

#### 5.1 Config File Location

| System | Path |
|:---|:---|
| **Windows** | `C:\Users\YourUsername\.claude\settings.json` |
| **macOS / Linux** | `~/.claude/settings.json` |

If the file doesn't exist, create it.

#### 5.2 Edit settings.json

Open the file in any text editor and paste the following (replace `sk-your-key-here` with your actual API key):

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "sk-your-deepseek-api-key",
    "ANTHROPIC_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash",
    "CLAUDE_CODE_SUBAGENT_MODEL": "deepseek-v4-flash",
    "CLAUDE_CODE_EFFORT_LEVEL": "max"
  }
}
```

**Field Reference (skip if you're new):**

| Field | Meaning |
|:---|:---|
| `ANTHROPIC_BASE_URL` | API endpoint — redirects from Anthropic to DeepSeek |
| `ANTHROPIC_AUTH_TOKEN` | Your DeepSeek API key |
| `ANTHROPIC_MODEL` | Primary model; `[1m]` enables 1M token context |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | Opus role → actually uses DeepSeek V4 Pro |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | Sonnet role → actually uses DeepSeek V4 Pro |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | Haiku role → actually uses DeepSeek V4 Flash (lightweight) |
| `CLAUDE_CODE_SUBAGENT_MODEL` | Sub-agents use the lighter model to save tokens |
| `CLAUDE_CODE_EFFORT_LEVEL` | `max` = strongest reasoning capability |

> 💡 **Model choice**: Primary recommendation is `deepseek-v4-pro[1m]`; lightweight tasks use `deepseek-v4-flash`. You can also use `deepseek-chat` (V3.1) or `deepseek-reasoner` (R1 reasoning mode).

#### 5.3 Save

Save the file. **No restart required** — proceed to verification.

---

### Step 6: Verify Everything Works

#### 6.1 Launch Claude Code

Open a terminal and type:

```bash
claude
```

First launch may take 1–2 minutes to initialize. You'll see a welcome screen when ready.

#### 6.2 Test a Conversation

Ask a simple question:

```text
What is 1 + 1?
```

If it responds normally, your setup is correct. The startup banner should also mention `deepseek-v4-pro`.

#### 6.3 Advanced Verification

To confirm requests are actually hitting DeepSeek:

```bash
claude --debug-file debug.log
```

Ask any question and exit. Open `debug.log` and search for `api.deepseek.com` — if you find it, you're 100% confirmed.

---

## Troubleshooting

### Q: `claude` says "command not found"

The npm global install directory is not in your system PATH.

**Windows**: Reopen an Admin terminal and try again. If it still fails, run `npm config get prefix` to find the path, then manually add it to your system PATH.

**macOS / Linux**:

```bash
echo 'export PATH="$(npm config get prefix)/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Q: "401 Unauthorized" or authentication error on startup

1. Double-check `ANTHROPIC_AUTH_TOKEN` in `settings.json` (must start with `sk-`)
2. Check your API key status on the DeepSeek console
3. Make sure your account has a balance

### Q: Responses are slow or timing out

DeepSeek may experience queues during peak hours.

Fix: add this line to the `env` section of `settings.json`:

```json
"API_TIMEOUT_MS": "3000000"
```

### Q: settings.json shows garbled text after saving on Windows

Notepad saved with the wrong encoding. Use **Save As** and change encoding to **UTF-8**. Better yet, use VS Code or Notepad++.

### Q: `npm install -g` fails with permission errors (macOS / Linux)

```bash
# Recommended: use nvm to manage Node.js
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
nvm install 20
nvm use 20

# Alternative: change npm global directory
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

### Q: Tool calling is less accurate with DeepSeek vs. native Claude

This is a known difference. Some DeepSeek models have ~80% tool-calling accuracy vs. Claude's 98%+. For daily use the impact is minimal. For complex tasks:
- Break work into smaller steps
- Double-check generated results

---

## Topics

[`claude-code`](https://github.com/topics/claude-code) [`deepseek`](https://github.com/topics/deepseek) [`ai-coding`](https://github.com/topics/ai-coding) [`tutorial`](https://github.com/topics/tutorial) [`beginner-friendly`](https://github.com/topics/beginner-friendly) [`developer-tools`](https://github.com/topics/developer-tools)

## 🤝 Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Issues and PRs welcome.

## 📄 References

- [Claude Code Official Docs](https://docs.anthropic.com/en/docs/claude-code)
- [DeepSeek API Docs](https://platform.deepseek.com/api-docs)
- [DeepSeek Pricing](https://platform.deepseek.com/pricing)

## 👤 Author

[@20kiki](https://github.com/20kiki)

## 📄 License

MIT — see [LICENSE](LICENSE).

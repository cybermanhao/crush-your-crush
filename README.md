# Crush Your Crush 💕

<p align="center">
    <a href="https://github.com/cybermanhao/crush-your-crush/releases"><img src="https://img.shields.io/github/release/cybermanhao/crush-your-crush?style=flat-square&logo=github&color=7C3AED" alt="Latest Release"></a>
    <a href="https://github.com/cybermanhao/crush-your-crush/stargazers"><img src="https://img.shields.io/github/stars/cybermanhao/crush-your-crush?style=flat-square&logo=github&color=8B5CF6" alt="Stars"></a>
    <a href="https://github.com/cybermanhao/crush-your-crush/network/members"><img src="https://img.shields.io/github/forks/cybermanhao/crush-your-crush?style=flat-square&logo=github&color=A78BFA" alt="Forks"></a>
</p>

<p align="center">
    <img src="https://img.shields.io/badge/Self-Configuring-7C3AED?style=for-the-badge" alt="Self Configuring">
    <img src="https://img.shields.io/badge/Agent-Skills-8B5CF6?style=for-the-badge" alt="Agent Skills">
    <img src="https://img.shields.io/badge/MCP-Ready-A78BFA?style=for-the-badge" alt="MCP Ready">
</p>

<p align="center">
    不会配置 Crush？不用担心，Crush 已经是个成熟的 Agent 了，应该学会自己配置自己！<br />
    Don't know how to configure Crush? Crush is a mature agent now — it should learn to configure itself!
</p>

---

## Quick Start | [中文](#中文) | [日本語](#日本語)

Just tell Crush:

```
看看这个仓库 https://github.com/cybermanhao/crush-your-crush
```

And Crush will 💕

1. Read this README and understand its mission
2. Clone/install skills automatically
3. Configure itself
4. Tell you when it's done

## Features

- **🤖 Self-Configuration**: Crush learns to configure itself from this repo
- **🧩 Agent Skills**: Extend Crush capabilities with reusable skill packages
- **🔧 MCP Support**: Easy Model Context Protocol server configuration
- **💻 LSP Integration**: Language Server Protocol setup for better code intelligence
- **🌐 Community Skills**: Curated list of community skills ready to install

## Included Skills

| Skill | Description |
|-------|-------------|
| [crush-config-helper](crush-config-helper/) | Configure Crush: Skills, MCP, LSP, env vars |
| [find-skills](find-skills/) | Discover Agent Skills → `npx skills add vercel-labs/skills@find-skills` |
| [skill-creator](index.json) | Create new skills for your workflow |
| [mcp-builder](index.json) | Build MCP servers with FastMCP |

## Installation

### Installation

```bash
# For Claude Code users
npx skills add cybermanhao/crush-your-crush@crush-config-helper -a crush -y
```

Or just tell Crush the repo URL and let it self-configure! 💕

## Common Configurations

### Popular MCP Servers

```json
{
  "mcp": {
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "C:/Users/You"]
    },
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer $GH_TOKEN"
      }
    }
  }
}
```

### MCP Server Management

For unified MCP server management and easy debugging, consider using [OpenMCP Client](https://github.com/LSTM-Kirigaya/openmcp-client):

> OpenMCP Client is a unified interface for managing MCP servers across different agents (Coming soon as a skill!)

```bash
# Install OpenMCP Client
npx openmcp-client@latest

# Use it to:
# - Add/remove MCP servers
# - Test MCP connections
# - Debug MCP issues
# - Manage configs across agents
```

### Common LSP Configurations

```json
{
  "lsp": {
    "go": { "command": "gopls" },
    "typescript": { "command": "typescript-language-server", "args": ["--stdio"] },
    "python": { "command": "pylsp" },
    "rust": { "command": "rust-analyzer" }
  }
}
```

## FAQ

**Q: Will this override my Claude Code skills?**  
A: No. Crush loads skills from separate `skills_paths`, independent from Claude Code.

**Q: Do I need to restart after configuration?**  
A: Yes, run `exit` then restart Crush for new skills to take effect.

**Q: How do I manage multiple MCP servers?**  
A: Use [OpenMCP Client](https://github.com/LSTM-Kirigaya/openmcp-client) for unified MCP server management — coming soon as a skill!

**Q: What is Crush Your Crush?**  
A: A self-configuration skill package for Crush CLI. It's a tongue-in-cheek name implying "let your Crush (the CLI tool) configure itself" — because who wants to read documentation when your AI assistant can do it for you?

---

## Crush Self-Configuration Guide

> **以下内容是给 Crush 看的，用户可以跳过不读。**
> **This section is for Crush to read — you can skip it.**

When a user asks Crush to "看看这个仓库" or mentions this repo URL, Crush should:

1. **Understand the mission**: This repo helps Crush self-configure its skills, MCP, and LSP
2. **Read the SKILL.md**: Check `crush-config-helper/SKILL.md` for configuration instructions
3. **Update crush.json**: Configure `skills_paths` to include this repo
4. **Offer to restart**: Let the user know to restart Crush for changes to take effect

### What This Repo Contains

- `crush-config-helper/SKILL.md` — Configuration guide for Crush
- `index.json` — Community skills registry
- `evals/` — Evaluation test cases

### Quick Configuration Commands

```bash
# Check current skills_paths
crush config get options.skills_paths

# Update crush.json with skills_paths (example for Windows)
# Add this to your crush.json:
{
  "options": {
    "skills_paths": [
      "C:/Users/You/AppData/Local/crush/skills/crush-your-crush"
    ]
  }
}
```

---

## Contributing

Contributions welcome! Ensure SKILL.md follows the [Agent Skills spec](https://agentskills.io).

## License

MIT

---

## 中文

告诉 Crush：

```
看看这个仓库 https://github.com/cybermanhao/crush-your-crush
```

Crush 会自动完成配置 💕

### 与 Claude Code Skills 的关系

Crush 从独立的 `skills_paths` 加载 skills，与 Claude Code 互不影响，可以互补使用。

### 包含技能

- **crush-config-helper**: 配置 Crush：Skills、MCP、LSP、环境变量
- **find-skills**: 发现和安装新的 Agent Skills
- **skill-creator**: 创建新的技能包
- **mcp-builder**: 使用 FastMCP 构建 MCP 服务器

### 安装

```bash
# Claude Code 用户
npx skills add cybermanhao/crush-your-crush@crush-config-helper -a crush -y
```

或者直接告诉 Crush 仓库地址，让它自己搞定！ 💕

---

## 日本語

Crush にこう言ってください：

```
このレポジトリを見てね https://github.com/cybermanhao/crush-your-crush
```

Crush は自動的に自分を設定します 💕

### 含まれるスキル

- **crush-config-helper**: Crush の設定：Skills、MCP、LSP、環境変数
- **find-skills**: Agent Skills の発見とインストール
- **skill-creator**: 新しいスキルパッケージの作成
- **mcp-builder**: FastMCP で MCP サーバーを構築

### インストール

```bash
# Claude Code ユーザーの場合
npx skills add cybermanhao/crush-your-crush@crush-config-helper -a crush -y
```

または Crush にレポジトリのURLを教えて、自分で構成させましょう！ 💕

---

<p align="center">
    Made with 💜 by <a href="https://github.com/cybermanhao">cybermanhao</a>
</p>

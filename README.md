# Crush Your Crush 💕

<p align="center">
    <a href="https://github.com/cybermanhao/crush-your-crush"><img width="500" alt="Crush Your Crush" src="https://github.com/user-attachments/assets/cf8ca3ce-8b02-43f0-9d0f-5a331488da4b" /></a><br />
    <a href="https://github.com/cybermanhao/crush-your-crush/releases"><img src="https://img.shields.io/github/release/cybermanhao/crush-your-crush" alt="Latest Release"></a>
    <a href="https://github.com/cybermanhao/crush-your-crush/stargazers"><img src="https://img.shields.io/github/stars/cybermanhao/crush-your-crush" alt="Stars"></a>
    <a href="https://github.com/cybermanhao/crush-your-crush/network/members"><img src="https://img.shields.io/github/forks/cybermanhao/crush-your-crush" alt="Forks"></a>
</p>

<p align="center">
    不会配置 Crush？不用担心，Crush 已经是个成熟的 Agent 了，应该学会自己配置自己！<br />
    Don't know how to configure Crush? Crush is a mature agent now — it should learn to configure itself!<br />
    Crushの設定方法がわからない？心配しないで、Crushはもう成熟したAgentなので、自分の設定を学ぶべきです！
</p>

<p align="center">
    <img width="700" alt="Self-configuration Demo" src="https://github.com/user-attachments/assets/58280caf-851b-470a-b6f7-d5c4ea8a1968" />
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
| [find-skills](find-skills/) | Discover and install Agent Skills |
| [skill-creator](index.json) | Create new skills for your workflow |
| [mcp-builder](index.json) | Build MCP servers with FastMCP |

## Installation

### One-Line Setup

```bash
# Clone to Crush skills directory
git clone https://github.com/cybermanhao/crush-your-crush.git "$env:LOCALAPPDATA\crush\skills"

# Or for Claude Code users
npx skills add cybermanhao/crush-your-crush -a crush -y
```

### Add to skills_paths

```json
{
  "options": {
    "skills_paths": [
      "C:/Users/You/AppData/Local/crush/skills"
    ]
  }
}
```

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
A: Consider using [OpenMCP Client](https://github.com/LSTM-Kirigaya/openmcp-client) for unified MCP server management.

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
git clone https://github.com/cybermanhao/crush-your-crush.git "$env:LOCALAPPDATA\crush\skills"
```

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
git clone https://github.com/cybermanhao/crush-your-crush.git "$env:LOCALAPPDATA\crush\skills"
```

---

<p align="center">
    <a href="https://charm.land/"><img alt="Charm logo" width="300" src="https://stuff.charm.sh/charm-banner-next.jpg" /></a>
</p>

Part of [Charm](https://charm.land) · Made with 💘 by [cybermanhao](https://github.com/cybermanhao)

<!--prettier-ignore-->
Charm热爱开源 • Charm loves open source

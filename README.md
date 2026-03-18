# Crush Your Crush 🖥️💕

> 不会配置Crush？不用担心，Crush已经是个成熟的Agent了，应该学会自己配置自己！
> Don't know how to configure Crush? Don't worry, Crush is a mature agent now — it should learn to configure itself!
> Crushの設定方法がわからない？心配しないで、Crushはもう成熟したAgentなので、自分の設定を学ぶべきです！

English | [中文](#中文) | [日本語](#日本語)

---

## English

### Quick Start

Just tell Crush:

> "看看这个仓库 https://github.com/cybermanhao/crush-your-crush，他就会自我攻略了！"

Crush will:
1. Read this README and understand its mission
2. Clone/install the skills automatically
3. Configure itself
4. Tell you when it's done 💕

### Relationship with Claude Code Skills

If you already have Claude Code with skills configured, these Crush-specific skills **complement** without conflicts:

| Claude Code Skills | Crush-specific Skills |
|-------------------|----------------------|
| find-skills (common) | ✅ crush-config-helper (Crush exclusive) |
| skill-creator (common) | Shareable, no conflicts |
| - | find-skills (shareable) |

### Included Skills

| Skill | Description |
|-------|-------------|
| [crush-config-helper](crush-config-helper/) | Configure Crush: Skills, MCP, LSP, env vars |
| [find-skills](find-skills/) | Discover and install Agent Skills |

### Installation

#### For Claude Code users

```bash
npx skills add cybermanhao/crush-your-crush@crush-config-helper -a crush -y
```

#### Manual clone

```bash
git clone https://github.com/cybermanhao/crush-your-crush.git "$env:LOCALAPPDATA\crush\skills"
```

#### Add to skills_paths

```json
{
  "options": {
    "skills_paths": [
      "C:/Users/YourUser/AppData/Local/crush/skills"
    ]
  }
}
```

### Common Configurations

#### Popular MCP Servers

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

#### Common LSP Configurations

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

### FAQ

**Q: Will this override my Claude Code skills?**  
A: No. Crush loads skills from separate `skills_paths`, independent from Claude Code.

**Q: Do I need to restart after configuration?**  
A: Yes, run `exit` then restart Crush for new skills to take effect.

**Q: How do I manage multiple MCP servers?**  
A: Consider using [OpenMCP Client](https://github.com/LSTM-Kirigaya/openmcp-client) for unified MCP server management.

### Contributing

Contributions welcome! Ensure SKILL.md follows the [Agent Skills spec](https://agentskills.io).

### License

MIT

---

## 中文

### 快速开始

只需要告诉 Crush：

> "看看这个仓库 https://github.com/cybermanhao/crush-your-crush，他就会自我攻略了！"

Crush 会自动：
1. 阅读 README，理解自己的使命
2. 自动克隆/安装 skills
3. 配置自己
4. 完成后告诉你 💕

### 与 Claude Code Skills 的关系

如果你已经安装了 Claude Code 并配置了 skills，这些 Crush-specific skills 可以**互补使用**，不会冲突：

| Claude Code Skills | Crush-specific Skills |
|-------------------|----------------------|
| find-skills（通用） | ✅ crush-config-helper（Crush 专属） |
| skill-creator（通用） | 可共用，无冲突 |
| - | find-skills（可共用） |

### 包含技能

| 技能 | 说明 |
|------|------|
| [crush-config-helper](crush-config-helper/) | 配置 Crush：Skills、MCP、LSP、环境变量 |
| [find-skills](find-skills/) | 发现和安装新的 Agent Skills |

### 手动安装

#### Claude Code 用户

```bash
npx skills add cybermanhao/crush-your-crush -a crush -y
```

#### 直接克隆

```bash
git clone https://github.com/cybermanhao/crush-your-crush.git "$env:LOCALAPPDATA\crush\skills"
```

### 常见问题

**Q: 会覆盖我已有的 Claude Code skills 吗？**  
A: 不会。Crush 从独立的 `skills_paths` 加载 skills，与 Claude Code 互不影响。

**Q: 配置完成后需要做什么？**  
A: 重启 Crush（`exit` 后重新启动），新 skills 才会生效。

### 贡献

欢迎贡献！请确保 SKILL.md 符合 [Agent Skills 规范](https://agentskills.io)。

### 许可证

MIT

---

## 日本語

### クイックスタート

Crush にこう言ってください：

> "このレポジトリを見てね https://github.com/cybermanhao/crush-your-crush、自動的に自分を設定するから！"

Crush は自動的に：
1. README を読んで、自分の使命を理解する
2. skills を自動的にインストール
3. 自分を設定する
4. 完了したら教えてくれる 💕

### Claude Code Skills との関係

すでに Claude Code と skills を設定している場合でも、これらの Crush 専用 skills は**競合なく補完**できます：

| Claude Code Skills | Crush 専用 Skills |
|-------------------|------------------|
| find-skills（共通） | ✅ crush-config-helper（Crush 専用） |
| skill-creator（共通） | 共有可能、競合なし |
| - | find-skills（共有可能） |

### 含まれるスキル

| スキル | 説明 |
|--------|------|
| [crush-config-helper](crush-config-helper/) | Crush の設定：Skills、MCP、LSP、環境変数 |
| [find-skills](find-skills/) | Agent Skills の発見とインストール |

### インストール

#### Claude Code ユーザーの場合

```bash
npx skills add cybermanhao/crush-your-crush -a crush -y
```

#### 手動クローン

```bash
git clone https://github.com/cybermanhao/crush-your-crush.git "$env:LOCALAPPDATA\crush\skills"
```

### よくある質問

**Q: 既存の Claude Code skills を上書きしますか？**  
A: いいえ。Crush は別の `skills_paths` から skills を読み込むため、Claude Code の skills とは干渉しません。

**Q: 設定後に再起動は必要ですか？**  
A: はい、`exit` を実行してから Crush を再起動すると、新しい skills が有効になります。

### コントリビュート

コントリビュート大歓迎！SKILL.md は [Agent Skills 仕様](https://agentskills.io) に準拠してください。

### ライセンス

MIT

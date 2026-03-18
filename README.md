# Crush Skills

面向 [Crush](https://github.com/charmbracelet/crush) 用户的实用技能集合。

## 包含技能

| 技能 | 说明 |
|------|------|
| [crush-config-helper](crush-config-helper/) | 配置 Crush：Skills、MCP、LSP、环境变量 |
| [find-skills](find-skills/) | 发现和安装新的 Agent Skills |

## 安装

### 方式一：直接克隆到 Crush skills 目录

```bash
# Windows
git clone https://github.com/cybermanhao/crush-skills.git "$env:LOCALAPPDATA\crush\skills"

# Unix/macOS
git clone https://github.com/cybermanhao/crush-skills.git ~/.config/crush/skills
```

### 方式二：添加到 skills_paths（如果不在默认目录）

```json
{
  "options": {
    "skills_paths": [
      "C:/Users/你的用户名/AppData/Local/crush/skills",
      "C:/Users/你的用户名/.agents/skills"
    ]
  }
}
```

### 方式三：使用 skills CLI（需要发布后）

```bash
npx skills add owner/repo@skill-name -g
```

`-g` 安装到全局目录 `~/.claude/skills/`。

安装后重启 Crush 即可使用。

## 功能

### crush-config-helper
- 配置 MCP 服务器
- 设置 LSP 语言服务器
- 管理 Skills 路径
- 调试配置问题

### find-skills
- 搜索 skills.sh 上的可用技能
- 一键安装技能
- 管理已安装技能

## 贡献

欢迎贡献！请确保 SKILL.md 符合 [Agent Skills 规范](https://agentskills.io)。

## 许可证

MIT

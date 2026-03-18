# Crush Skills

面向 [Crush](https://github.com/charmbracelet/crush) 用户的实用技能集合。

## 包含技能

| 技能 | 说明 |
|------|------|
| [crush-config-helper](crush-config-helper/) | 配置 Crush：Skills、MCP、LSP、环境变量 |
| [find-skills](find-skills/) | 发现和安装新的 Agent Skills |

## 安装

```bash
# 克隆到 Crush skills 目录
git clone https://github.com/cybermanhao/crush-skills.git ~/.config/crush/skills
```

或添加到 `crush.json`:

```json
{
  "options": {
    "skills_paths": [
      "~/.config/crush/skills",
      "path/to/crush-skills"
    ]
  }
}
```

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

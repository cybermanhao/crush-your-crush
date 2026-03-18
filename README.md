# Crush Skills

面向 [Crush](https://github.com/charmbracelet/crush) 用户的实用技能集合。

## 与 Claude Code Skills 的关系

如果你已经安装了 Claude Code 并配置了 skills，这些 Crush-specific skills 可以**互补使用**：

| 已有 Skills（Claude Code） | Crush-specific Skills |
|---------------------------|----------------------|
| find-skills（通用） | ✅ crush-config-helper（Crush 专属） |
| skill-creator（通用） | 可共用，无冲突 |
| - | find-skills（可共用） |

Crush 从独立的 `skills_paths` 加载 skills，与 Claude Code **互不影响**。

## 包含技能

| 技能 | 说明 | 是否与 Claude Code 重复 |
|------|------|------------------------|
| [crush-config-helper](crush-config-helper/) | 配置 Crush：Skills、MCP、LSP、环境变量 | ❌ Crush 专属 |
| [find-skills](find-skills/) | 发现和安装新的 Agent Skills | ⚠️ 功能相似，可跳过 |

## 安装

### 已有 Claude Code skills 的用户

如果你已有 Claude Code skills，推荐**选择性安装**：

```bash
# 只安装 Crush 专属的 crush-config-helper
npx skills add cybermanhao/crush-skills@crush-config-helper -a crush -y
```

### 从零开始的用户

```bash
# 克隆到 Crush skills 目录（不会影响 Claude Code）
git clone https://github.com/cybermanhao/crush-skills.git "$env:LOCALAPPDATA\crush\skills"

# 或使用 skills CLI
npx skills add cybermanhao/crush-skills -a crush -y
```

### 添加 skills_paths（不覆盖现有）

在 `crush.json` 中添加：

```json
{
  "options": {
    "skills_paths": [
      "C:/Users/你的用户名/AppData/Local/crush/skills/crush-config-helper"
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

## 常见问题

**Q: 会覆盖我已有的 Claude Code skills 吗？**  
A: 不会。Crush 从独立的 `skills_paths` 加载 skills，与 Claude Code 互不影响。

**Q: find-skills 和 Claude Code 的 find-skills 冲突吗？**  
A: 不会。两个工具功能相似，但各自在对应 agent 下工作。

## 贡献

欢迎贡献！请确保 SKILL.md 符合 [Agent Skills 规范](https://agentskills.io)。

## 许可证

MIT

# Crush Skills

Skills for developing [Crush](https://github.com/charmbracelet/crush) - a terminal-based AI coding assistant built by Charm.

## Structure

```
crush-skills/
├── charm-libraries/     # Skills for Charm's Go libraries
│   ├── catwalk/        # Provider sync & embedded providers
│   ├── fantasy/        # LLM provider abstraction
│   ├── glamour/        # Markdown rendering
│   └── lipgloss/       # Terminal styling
├── golang/             # Skills for Go development
│   ├── golang-pro/     # Go best practices
│   ├── bubbletea/      # TUI framework
│   ├── sqlc/           # SQL code generation
│   ├── cobra-viper/    # CLI framework
│   ├── testing/         # Testing strategies
│   └── testing-strategies/
├── crush/              # Crush-specific skills
│   ├── crush-config-helper/  # Configure Crush settings
│   ├── skill-creator/        # Create new skills
│   └── find-skills/          # Discover available skills
└── README.md
```

## Installation

Clone to your Crush skills directory:

```bash
# Unix/macOS
git clone https://github.com/cybermanhao/crush-skills.git ~/.config/crush/skills

# Windows (PowerShell)
git clone https://github.com/cybermanhao/crush-skills.git "$env:LOCALAPPDATA\crush\skills"
```

Or add to your `crush.json`:

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

## Requirements

- [Crush CLI](https://github.com/charmbracelet/crush)
- Go 1.21+ (for Go-related skills)

## Contributing

Contributions welcome! Please ensure:
- SKILL.md follows the [Agent Skills spec](https://agentskills.io)
- Name matches directory name
- Description is under 1024 characters

## License

MIT

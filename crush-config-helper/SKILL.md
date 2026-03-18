---
name: crush-config-helper
description: Configure Crush CLI settings including Skills, MCP servers, LSP, environment variables, permissions, and debugging. Make sure to use this skill whenever the user asks about configuring Crush, setting up skills or extensions, adding MCP servers, configuring LSP language servers, customizing tool permissions, troubleshooting Crush issues, or understanding where Crush stores configuration. This skill handles all Crush customization questions - from simple config edits to complex MCP server setups with authentication headers. Trigger on any mention of "crush.json", "configuration", "skills directory", "MCP", "LSP", "environment variables for Crush", or requests to "customize" or "set up" Crush features.
---

# Crush Configuration Helper

This skill helps you configure Crush CLI's self-configuration features: Skills, MCP servers, and LSP.

## Quick Reference

| Need | Solution |
|------|----------|
| Add MCP server | See **MCP Configuration** section below |
| Configure LSP | See **LSP Configuration** section below |
| Add custom skills | Use `skills_paths` in crush.json |
| Debug issues | Use `--debug` flag or `debug_lsp: true` |
| Disable tools | Set `options.disabled_tools` |
| Find config location | Check **Configuration File Locations** |

## Configuration File Locations

Crush looks for configuration in this priority order:
1. `.crush.json` (project-local)
2. `crush.json` (project-local)
3. `$HOME/.config/crush/crush.json` (global)

On Windows, global config is at `%LOCALAPPDATA%\crush\crush.json`.

---

## Agent Skills Configuration

Skills extend Crush's capabilities with reusable skill packages following the [Agent Skills](https://agentskills.io) standard.

### Default Skills Directory

| Platform | Path |
|----------|------|
| Unix | `~/.config/crush/skills/` |
| Windows | `%LOCALAPPDATA%\crush\skills\` |

You can override this with the `CRUSH_SKILLS_DIR` environment variable.

### Adding Custom Skills Paths

Add additional paths in your `crush.json`:

```json
{
  "$schema": "https://charm.land/crush.json",
  "options": {
    "skills_paths": [
      "~/.config/crush/skills",
      "./project-skills"
    ]
  }
}
```

### Installing Example Skills

```bash
# Unix
mkdir -p ~/.config/crush/skills
cd ~/.config/crush/skills
git clone https://github.com/anthropics/skills.git _temp
mv _temp/skills/* . && rm -rf _temp

# Windows (PowerShell)
mkdir -Force "$env:LOCALAPPDATA\crush\skills"
cd "$env:LOCALAPPDATA\crush\skills"
git clone https://github.com/anthropics/skills.git _temp
mv _temp/skills/* . ; rm -r -force _temp
```

---

## MCP (Model Context Protocol) Configuration

MCP servers extend Crush with external tools and capabilities. Crush supports three transport types: `stdio`, `http`, and `sse`.

### MCP Configuration Syntax

```json
{
  "$schema": "https://charm.land/crush.json",
  "mcp": {
    "server-name": {
      "type": "stdio|http|sse",
      "command": "executable-path",
      "args": ["arg1", "arg2"],
      "url": "https://...",          // for http/sse types
      "timeout": 120,
      "disabled": false,
      "disabled_tools": ["tool-name"],
      "env": {
        "VAR_NAME": "value"
      },
      "headers": {                    // for http/sse types
        "Authorization": "Bearer $TOKEN"
      }
    }
  }
}
```

### Environment Variable Expansion

Use `$(echo $VAR)` syntax for environment variables:

```json
{
  "headers": {
    "Authorization": "Bearer $(echo $GH_PAT)"
  }
}
```

### MCP Server Examples

**Filesystem MCP (stdio):**
```json
{
  "mcp": {
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed-dir"]
    }
  }
}
```

**GitHub MCP (http):**
```json
{
  "mcp": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer $GH_PAT"
      }
    }
  }
}
```

**Browser Tools MCP (sse):**
```json
{
  "mcp": {
    "browser-tools": {
      "type": "sse",
      "url": "https://127.0.0.1:9222/mcp",
      "headers": {
        "API-Key": "$(echo $BROWSER_API_KEY)"
      }
    }
  }
}
```

### Disabling MCP Tools

To disable specific tools from an MCP server:

```json
{
  "mcp": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "disabled_tools": ["create_issue", "create_pull_request"]
    }
  }
}
```

---

## LSP (Language Server Protocol) Configuration

LSPs provide language-specific context to help Crush understand your codebase.

### LSP Configuration Syntax

```json
{
  "$schema": "https://charm.land/crush.json",
  "lsp": {
    "language-id": {
      "command": "lsp-command",
      "args": ["--stdio"],
      "env": {
        "VAR": "value"
      }
    }
  }
}
```

### Common LSP Configurations

**Go:**
```json
{
  "lsp": {
    "go": {
      "command": "gopls",
      "env": {
        "GOTOOLCHAIN": "go1.24.5"
      }
    }
  }
}
```

**TypeScript/JavaScript:**
```json
{
  "lsp": {
    "typescript": {
      "command": "typescript-language-server",
      "args": ["--stdio"]
    },
    "javascript": {
      "command": "typescript-language-server",
      "args": ["--stdio"]
    }
  }
}
```

**Python:**
```json
{
  "lsp": {
    "python": {
      "command": "pylsp"
    }
  }
}
```

**Rust:**
```json
{
  "lsp": {
    "rust": {
      "command": "rust-analyzer"
    }
  }
}
```

**Nix:**
```json
{
  "lsp": {
    "nix": {
      "command": "nil"
    }
  }
}
```

**C/C++:**
```json
{
  "lsp": {
    "c": {
      "command": "clangd"
    },
    "cpp": {
      "command": "clangd"
    }
  }
}
```

---

## Other Useful Configuration Options

### Ignoring Files

Create `.crushignore` in your project root (uses `.gitignore` syntax):

```
# Ignore build outputs
dist/
build/
node_modules/

# Ignore sensitive files
.env
*.pem
```

### Allowing Tools Without Prompt

```json
{
  "$schema": "https://charm.land/crush.json",
  "permissions": {
    "allowed_tools": ["view", "ls", "grep", "edit"]
  }
}
```

Or use `--yolo` flag (use with caution):

```bash
crush --yolo
```

### Disabling Built-in Tools

```json
{
  "options": {
    "disabled_tools": ["bash", "sourcegraph"]
  }
}
```

### Desktop Notifications

```json
{
  "options": {
    "disable_notifications": false
  }
}
```

### Debug Mode

```json
{
  "options": {
    "debug": true,
    "debug_lsp": true
  }
}
```

Or use CLI flag:

```bash
crush --debug
```

---

## Verifying Your Configuration

1. **Validate JSON syntax:** Use a JSON validator to ensure no syntax errors
2. **Check config location:** Verify config is in the correct priority location
3. **Test LSP:** Run `crush logs` to see LSP connection status
4. **List available tools:** Ask Crush what MCP tools are available

### Common Issues

| Issue | Solution |
|-------|----------|
| LSP not connecting | Verify the LSP command is in your PATH |
| MCP tools not available | Check `crush logs` for MCP startup errors |
| Config not loading | Ensure JSON is valid; check file path |
| Skills not discovered | Verify skills directory exists and contains `SKILL.md` files |

---

### TUI (Terminal UI) Configuration

```json
{
  "options": {
    "tui": {
      "transparent": false,
      "compact_mode": false
    }
  }
}
```

## Environment Variables

| Variable | Purpose |
|----------|---------|
| `CRUSH_SKILLS_DIR` | Override default skills directory |
| `CRUSH_GLOBAL_CONFIG` | Override global config path |
| `CRUSH_GLOBAL_DATA` | Override data directory path |
| `CRUSH_DISABLE_METRICS` | Disable usage metrics |
| `CRUSH_DISABLE_PROVIDER_AUTO_UPDATE` | Disable provider auto-updates |
| `CRUSH_SKIP_LSP` | Skip LSP initialization |
| `CRUSH_SKIP_MCP` | Skip MCP server loading |
| `DO_NOT_TRACK` | Respect DNT convention |

## Common Troubleshooting Patterns

### MCP Server Issues
1. Check if command is in PATH: `which <command>`
2. Verify JSON syntax in crush.json
3. Run `crush logs` to see MCP startup errors
4. Try stdio type first (most reliable)

### LSP Issues
1. Run `crush --debug` to see LSP connection logs
2. Set `debug_lsp: true` in crush.json
3. Verify the LSP command exists: `which <lsp-command>`

### Skills Not Loading
1. Verify skills directory contains `SKILL.md` files
2. Check file permissions (read access required)
3. Confirm path in `skills_paths` is absolute or correct relative path

### Config Not Applied
1. Crush uses first found config in priority order
2. Project-local `.crush.json` overrides global config
3. Validate JSON syntax

---

## Example Complete Configuration

```json
{
  "$schema": "https://charm.land/crush.json",
  "lsp": {
    "go": {
      "command": "gopls"
    },
    "typescript": {
      "command": "typescript-language-server",
      "args": ["--stdio"]
    },
    "python": {
      "command": "pylsp"
    },
    "rust": {
      "command": "rust-analyzer"
    }
  },
  "mcp": {
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "./"]
    },
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer $GH_PAT"
      }
    }
  },
  "options": {
    "skills_paths": [
      "~/.config/crush/skills"
    ],
    "debug": false,
    "disable_notifications": false
  },
  "permissions": {
    "allowed_tools": ["view", "ls", "grep", "edit"]
  }
}
```

---
name: crush-your-crush
description: "Crush Your Crush - Self-configuration for Crush. Use when user says 'configure crush', 'setup crush', 'install skills', 'what can crush do', 'crush not working', or needs help with crush configuration, skills, MCP, LSP, or environment setup. This skill bootstraps Crush to configure itself and discover available skills. Triggers on any mention of 'crush configuration', 'skills setup', 'mcp server', 'lsp language server', or 'environment variables for crush'."
---

# Crush Your Crush

A skill that helps Crush configure itself and discover available skills.

## Core Purpose

This skill enables Crush to:
1. Understand what skills are and how to use them
2. Configure itself (MCP, LSP, environment variables)
3. Discover and install additional skills from the community

## When This Skill Triggers

Use this skill when the user (or Crush itself) needs to:
- Set up Crush for the first time
- Configure MCP servers
- Set up LSP language servers
- Install or discover new skills
- Troubleshoot Crush configuration

## Quick Actions

### If user says "configure crush" or "setup crush":

1. Read the skills directory README.md
2. Update crush.json with appropriate configurations
3. Suggest available skills to install

### If user asks about skills:

1. Check skills_paths in crush.json
2. List available skills from index.json
3. Provide installation commands

## Skill Structure

```
crush-your-crush/
├── SKILL.md              # This file
├── index.json            # Available skills index
├── crush-config-helper/  # Crush configuration help
└── README.md            # User-facing documentation
```

## Index Format

The index.json contains available skills:

```json
{
  "skills": [
    {
      "name": "crush-config-helper",
      "description": "Configure Crush settings",
      "path": "crush-config-helper",
      "required": true,
      "community_url": null
    },
    {
      "name": "find-skills",
      "description": "Discover community skills",
      "path": "find-skills",
      "required": false,
      "community_url": "anthropics/skills@find-skills"
    }
  ]
}
```

## Configuration Flow

When activated, Crush should:

1. Read `skills/index.json` to see available skills
2. Check current `crush.json` configuration
3. Identify missing or misconfigured items
4. Offer to fix or set up automatically

## Available Skills

### Required Skills

| Skill | Description |
|-------|-------------|
| crush-config-helper | Configure Crush: Skills, MCP, LSP, env vars |

### Recommended Skills

| Skill | Description | Install Command |
|-------|-------------|----------------|
| find-skills | Discover community skills | `npx skills add anthropics/skills@find-skills -a crush -y` |

## Examples

### Example 1: First-time setup
User: "I just installed crush, how do I set it up?"
→ Read index.json, guide user through skill installation

### Example 2: Adding MCP server
User: "I want to add the filesystem MCP server"
→ Use crush-config-helper instructions

### Example 3: Skill discovery
User: "what skills are available?"
→ Read index.json and list available skills

## Troubleshooting

### Crush doesn't recognize skills
1. Check skills_paths in crush.json
2. Verify SKILL.md files exist in skills directories
3. Restart Crush

### Configuration not applied
1. Run `exit` to fully restart Crush
2. Check crush.json for JSON syntax errors

## Related Skills

- [crush-config-helper](crush-config-helper/) - Detailed configuration help
- find-skills - Community skill discovery (install separately)

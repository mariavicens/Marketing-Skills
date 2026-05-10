# Marketing-Skills
Marketing skills for Claude Code and AI agents. CRO, copywriting, SEO, analytics, growth engineering, and Figma design integration.

## Figma Integration

Connect Claude Code to Figma via the **Figma Context MCP** server to inspect designs, extract tokens, export assets, and generate marketing copy — all from the terminal.

See [CLAUDE.md](./CLAUDE.md) for full setup instructions.

### What you can do

- Fetch design tokens (colors, typography, spacing) from Figma local styles
- Export frames and components as PNG or SVG
- Read text layers to generate and improve marketing copy
- Audit brand consistency across a Figma design system
- Run A/B variant comparisons between Figma frames

### Setup (30 seconds)

1. Generate a Figma Personal Access Token in **Account Settings → Personal access tokens**.
2. Add the MCP server to `.claude/settings.json`:

```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "figma-developer-mcp", "--stdio"],
      "env": {
        "FIGMA_ACCESS_TOKEN": "<YOUR_TOKEN>"
      }
    }
  }
}
```

3. Run `/mcp` in Claude Code to verify the connection.

## Skills

| Skill | File |
|---|---|
| Figma Integration | `.claude/skills/figma-integration.md` |

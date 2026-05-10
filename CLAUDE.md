# Marketing Skills — Claude Code Setup

Marketing skills for Claude Code and AI agents: CRO, copywriting, SEO, analytics, growth engineering, and Figma design integration.

## Figma Integration (MCP)

Claude Code connects to Figma through the **Figma Context MCP** server. This gives Claude direct access to your Figma files — components, design tokens, frames, and text layers — without leaving the terminal.

### Quick Setup

#### 1. Get a Figma Access Token

1. In Figma, go to **Account Settings → Personal access tokens**.
2. Click **Generate new token**, give it a name (e.g. `claude-code`), and copy the value.

#### 2. Configure the MCP Server

Add the Figma MCP server to your Claude Code configuration.

**Option A — Project scope** (`.claude/settings.json` in this repo):

```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "figma-developer-mcp", "--stdio"],
      "env": {
        "FIGMA_ACCESS_TOKEN": "<YOUR_TOKEN_HERE>"
      }
    }
  }
}
```

**Option B — Global scope** (`~/.claude/settings.json`):

Same JSON block above — use this if you want Figma available in every Claude Code session.

#### 3. Verify the Connection

Start Claude Code and run:

```
/mcp
```

You should see `figma` listed as a connected server with its available tools.

### Environment Variable (Alternative)

Instead of hardcoding the token in JSON, export it in your shell profile:

```bash
export FIGMA_ACCESS_TOKEN="figd_xxxxxxxxxxxxxxxxxxxxxxxx"
```

Then reference it in the MCP config:

```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "figma-developer-mcp", "--stdio"]
    }
  }
}
```

The MCP server reads `FIGMA_ACCESS_TOKEN` from the environment automatically.

### Available Figma Tools (via MCP)

Once connected, Claude can use these tools:

| Tool | Description |
|---|---|
| `get_figma_data` | Fetch a full file or specific node by URL |
| `download_figma_images` | Export frames/components as PNG or SVG |

### Usage Examples

```
# Inspect a Figma file
"Fetch the design tokens from this Figma file: https://www.figma.com/file/ABC123/Brand-System"

# Export a marketing banner
"Export the hero frame from the landing page Figma file as SVG and save it to assets/"

# Generate copy from a design
"Read all text layers from the pricing section frame and suggest CRO improvements"

# Audit brand colors
"List all color styles from the Figma design system and generate a CSS variables file"
```

## Skills

Skills extend Claude's capabilities for specific marketing workflows. See `.claude/skills/` for available skills:

- **figma-integration.md** — Design inspection, token extraction, asset export, copy generation from Figma

## Project Structure

```
Marketing-Skills/
├── CLAUDE.md                        # This file — setup and conventions
├── README.md                        # Project overview
└── .claude/
    ├── settings.json                # MCP servers and permissions (create this)
    └── skills/
        └── figma-integration.md     # Figma skill instructions
```

## Security Note

Never commit your `FIGMA_ACCESS_TOKEN` to the repository. Add `.claude/settings.json` to `.gitignore` if it contains the raw token, or use the environment variable approach instead.

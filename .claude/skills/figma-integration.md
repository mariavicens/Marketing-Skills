# Figma Integration Skill

Use this skill to interact with Figma files, extract design tokens, inspect components, and generate marketing assets directly from Figma designs within Claude Code.

## Triggers

Invoke this skill when the user asks to:
- Read or inspect a Figma file or component
- Export design tokens (colors, typography, spacing) from Figma
- Generate code or copy from a Figma design
- Analyze a Figma frame for marketing purposes
- Sync design assets between Figma and the codebase

## Prerequisites

Before using this skill, ensure the Figma MCP server is configured (see `CLAUDE.md`).

## Instructions

### 1. Accessing a Figma File

When the user provides a Figma URL (e.g., `https://www.figma.com/file/<FILE_KEY>/...`):

1. Extract the `FILE_KEY` from the URL.
2. Use the `figma` MCP tools to fetch the file node tree.
3. Identify relevant frames, components, or design tokens.

### 2. Extracting Design Tokens

Pull colors, typography, and spacing from Figma local styles:

```
GET /v1/files/:file_key/styles
```

Map Figma styles to CSS custom properties or a design token JSON file:

```json
{
  "color": {
    "brand-primary": "#FF5C00",
    "brand-secondary": "#1A1A2E"
  },
  "typography": {
    "heading-xl": { "fontFamily": "Inter", "fontSize": 48, "fontWeight": 700 }
  }
}
```

### 3. Generating Marketing Copy from Designs

Given a Figma frame containing text layers:

1. Fetch the frame node with `GET /v1/files/:file_key/nodes?ids=<NODE_ID>`.
2. Walk the children to collect all `TEXT` nodes.
3. Present the copy hierarchy (headline → subheadline → body → CTA).
4. Suggest improvements based on CRO and copywriting best practices.

### 4. Exporting Assets

To export an image or SVG from a Figma node:

```
GET /v1/images/:file_key?ids=<NODE_ID>&format=svg&scale=2
```

Save the returned URL content to the project's `assets/` directory.

## Marketing-Specific Workflows

| Goal | Figma Action | Output |
|---|---|---|
| Brand audit | Fetch local styles | Token JSON / CSS vars |
| Landing page copy | Read TEXT nodes from hero frame | Copy document |
| Ad creative export | Export frame as PNG/SVG | `assets/ads/` files |
| Component audit | List all components | Component inventory CSV |
| A/B test variants | Compare two frames | Diff report |

## Error Handling

- **Invalid token**: Prompt the user to check `FIGMA_ACCESS_TOKEN` in their environment.
- **File not found**: Confirm the file is shared with the token owner ("Anyone with the link can view").
- **Rate limits**: Figma REST API allows 300 req/min; add a 200 ms delay between batch requests.

## References

- Figma REST API docs: https://www.figma.com/developers/api
- Figma MCP server: https://github.com/GLips/Figma-Context-MCP

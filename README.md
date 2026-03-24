# draw-arch

Claude Code skill for generating architecture diagrams. Renders Mermaid diagrams as HTML and opens them directly in the browser.

## Install

```bash
ln -sf /path/to/draw-arch ~/.claude/skills/draw-arch
```

## Usage

In Claude Code, just ask:

- "画个架构图"
- "draw a sequence diagram for the auth flow"
- "visualize the data model"
- "/draw-arch show me the deployment pipeline"

The skill generates Mermaid code, writes it to HTML, and opens it in your default browser.

## Supported Diagram Types

| Type | Mermaid syntax |
|------|---------------|
| Architecture / data flow | `flowchart LR` / `flowchart TD` |
| Request/response flow | `sequenceDiagram` |
| Class hierarchy | `classDiagram` |
| Database schema | `erDiagram` |
| State machine | `stateDiagram-v2` |
| Timeline | `gantt` |
| Concept map | `mindmap` |
| Proportions | `pie` |

## License

MIT

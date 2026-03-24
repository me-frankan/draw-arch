---
name: draw-arch
description: Draw architecture diagrams, flowcharts, sequence diagrams, and any visual system diagram. Renders as Mermaid in the browser automatically. Use when the user asks to "draw", "diagram", "visualize", "show me a flow", "architecture diagram", "sequence diagram", "flowchart", "class diagram", "ER diagram", "state diagram", "画图", "画个流程图", "画架构图", "来个图", or any request involving visual representation of code, systems, or processes.
---

# draw-arch

Generate Mermaid diagrams and open them in the browser.

## When to use

Any time the user wants a visual diagram — architecture, flow, sequence, class, ER, state, gantt, pie, mindmap, or any Mermaid-supported type.

## How it works

1. Analyze what the user wants to visualize — code flow, system architecture, data model, etc.
2. If the subject is code in the current repo, read the relevant files first
3. Generate appropriate Mermaid syntax
4. Write to HTML and open in browser

## Generating the diagram

Pick the best Mermaid diagram type for the request:

| Request | Mermaid type |
|---------|-------------|
| Architecture, system overview, data flow | `flowchart LR` or `flowchart TD` |
| Request/response flow, API calls, message passing | `sequenceDiagram` |
| Class hierarchy, module structure | `classDiagram` |
| Database schema, data model | `erDiagram` |
| State machine, lifecycle | `stateDiagram-v2` |
| Timeline, project plan | `gantt` |
| Mental model, concept map | `mindmap` |
| Proportions, distribution | `pie` |

Use `LR` (left-to-right) for process flows, `TD` (top-down) for hierarchies.

## Rendering

Write the diagram to `/tmp/draw-arch.html` using this template, then open it:

```bash
cat << 'TEMPLATE' > /tmp/draw-arch.html
<!DOCTYPE html>
<html><head>
<meta charset="utf-8">
<title>draw-arch</title>
<script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
<style>
  body { font-family: -apple-system, sans-serif; padding: 24px; background: #fafafa; }
  h2 { color: #333; margin-bottom: 4px; }
  .subtitle { color: #888; margin-bottom: 20px; }
  .mermaid { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 1px 3px rgba(0,0,0,0.1); }
</style>
</head><body>
<h2>TITLE_PLACEHOLDER</h2>
<p class="subtitle">SUBTITLE_PLACEHOLDER</p>
<div class="mermaid">
MERMAID_PLACEHOLDER
</div>
<script>mermaid.initialize({startOnLoad:true, theme:'default', flowchart:{curve:'basis'}, sequence:{mirrorActors:false}});</script>
</body></html>
TEMPLATE
open /tmp/draw-arch.html
```

Replace the three placeholders:
- `TITLE_PLACEHOLDER` — diagram title
- `SUBTITLE_PLACEHOLDER` — one-line description
- `MERMAID_PLACEHOLDER` — the Mermaid code (no backticks, raw syntax)

Use `sed` or write the full file directly. The key constraint: the Mermaid code goes inside the `<div class="mermaid">` tag as-is.

## Style guide

- Use `subgraph` with fill colors to group related components into zones (frontend, backend, infra)
- Use descriptive edge labels — `-->|"label"|` not bare `-->`
- Use emojis sparingly in node labels for visual anchors (not required)
- Keep node IDs short, labels descriptive: `DB["PostgreSQL"]` not `PostgreSQLDatabase["PostgreSQL"]`
- For flowcharts with many nodes, use consistent direction and avoid crossing edges
- Limit to ~20 nodes max per diagram — split into multiple if needed

## Multiple diagrams

If the subject needs multiple views (e.g., both a high-level architecture AND a sequence diagram for a specific flow), generate them as separate `<div class="mermaid">` blocks in the same HTML file with `<h3>` headings between them.

## After opening

Tell the user what the diagram shows. If they want changes, regenerate and reopen — the browser tab refreshes automatically on the same URL.

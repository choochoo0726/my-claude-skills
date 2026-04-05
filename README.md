# my-claude-skills

Personal Claude Code skills plugin hosted at `choochoo0726/my-claude-skills`.

Skills are reference guides that Claude auto-loads when their trigger conditions match, providing domain-specific patterns, quick references, and code snippets inline in the conversation.

## Installation

```bash
claude plugins marketplace add choochoo0726/my-claude-skills
claude plugins install my-claude-skills@my-claude-skills
```

## Skills

### `plotly-express`

A comprehensive reference for data visualization with `plotly.express` (imported as `px`).

**Triggers on:** `import plotly.express`, `px.scatter`, `px.bar`, `px.line`, and any other `px.*` chart function.

**Contents:**
- Full chart type reference table — scatter, line, bar, area, histogram, box, violin, pie, sunburst, treemap, heatmap, choropleth, 3D plots, geographic maps, and more
- Common parameters — `color`, `size`, `facet_col/row`, `animation_frame`, `trendline`, `marginal_x/y`, `hover_data`, `template`, `labels`, `category_orders`, color scales
- Patterns — tidy/long-form data, post-hoc `update_layout` / `update_traces`, built-in sample datasets, export to HTML/PNG/JSON
- Common mistakes and fixes
- Ready-to-use quick snippets for the most frequent chart types

## Structure

```
.claude-plugin/
  plugin.json        # Plugin metadata
  marketplace.json   # Self-hosted marketplace manifest
skills/
  plotly-express/
    SKILL.md         # plotly.express reference guide
```

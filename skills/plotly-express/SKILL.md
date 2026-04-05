---
name: plotly-express
description: Use when creating data visualizations with plotly.express (px) - scatter plots, bar charts, line plots, histograms, box plots, heatmaps, geographic maps, 3D plots, or any px chart type. Triggers on imports of plotly.express, px.scatter, px.bar, px.line, etc.
---

# Plotly Express (px) Data Visualization

## Overview

`plotly.express` (imported as `px`) is a high-level Plotly API. Every function returns a `go.Figure` that can be further customized with `.update_layout()` and `.update_traces()`.

```python
import plotly.express as px
fig = px.scatter(df, x="col_a", y="col_b", color="category")
fig.show()
```

## Chart Type Quick Reference

### Relational
| Function | Use case |
|----------|----------|
| `px.scatter()` | Points — numeric vs numeric, optionally sized/colored |
| `px.line()` | Connected points — time series, trends |
| `px.area()` | Stacked area over a continuous axis |
| `px.scatter_matrix()` | Pairwise scatter of all numeric columns |

### Distribution
| Function | Use case |
|----------|----------|
| `px.histogram()` | Frequency distribution of one variable |
| `px.box()` | Box-and-whisker by category |
| `px.violin()` | Full density shape by category |
| `px.strip()` | Jittered points (like box but shows all data) |
| `px.ecdf()` | Empirical CDF |

### Categorical
| Function | Use case |
|----------|----------|
| `px.bar()` | Bar chart; `barmode='group'` or `'stack'` |
| `px.bar_polar()` | Radial bar (wind rose style) |
| `px.timeline()` | Gantt chart (`x_start`, `x_end`) |

### Part-of-whole / Hierarchical
| Function | Use case |
|----------|----------|
| `px.pie()` | Simple proportions (`names`, `values`) |
| `px.sunburst()` | Multi-level hierarchy in concentric rings |
| `px.treemap()` | Nested rectangles (`path=[col1, col2, ...]`) |
| `px.icicle()` | Same as treemap but as stacked bars |
| `px.funnel()` | Sequential stage funnel |

### 2D / Heatmap
| Function | Use case |
|----------|----------|
| `px.density_heatmap()` | 2D binned counts (use `nbinsx`, `nbinsy`) |
| `px.density_contour()` | 2D contour density |
| `px.imshow()` | Render arrays or PIL images as heatmaps |

### 3D
| Function | Use case |
|----------|----------|
| `px.scatter_3d()` | 3D scatter (`x`, `y`, `z`) |
| `px.line_3d()` | 3D line |

### Geographic
| Function | Use case |
|----------|----------|
| `px.choropleth()` | Country/state fill by value (`locations`, `locationmode`) |
| `px.scatter_geo()` | Points on globe (`lat`, `lon`) |
| `px.scatter_map()` | Points on tile map (needs lat/lon) |
| `px.line_geo()` / `px.line_map()` | Lines on geo/tile maps |

### Multivariate
| Function | Use case |
|----------|----------|
| `px.parallel_coordinates()` | Numeric multivariate lines |
| `px.parallel_categories()` | Categorical multivariate |

---

## Common Parameters (apply to most functions)

### Data
```python
data_frame   # DataFrame (most common) or dict/array
x, y         # Column names, arrays, or scalar
```

### Visual Encoding
```python
color        # Column → color (discrete or continuous auto-detected)
size         # Column → marker size (continuous)
symbol       # Column → marker shape (discrete)
text         # Column → text labels on marks
opacity      # Float 0–1
```

### Hover / Tooltip
```python
hover_name   # Bold header in tooltip
hover_data   # List of extra columns (or dict to show/hide/format)
```

### Faceting (subplots)
```python
facet_col    # Column → horizontal subplots
facet_row    # Column → vertical subplots
facet_col_wrap  # Max columns before wrapping
```

### Animation
```python
animation_frame  # Column → slider frames
animation_group  # Column → identity across frames (smooth transitions)
```

### Analysis
```python
trendline    # 'ols' | 'lowess' | 'rolling' | 'ewm' | 'expanding'
trendline_scope  # 'trace' (per series) | 'overall'
marginal_x   # 'rug' | 'box' | 'violin' | 'histogram'
marginal_y   # same as marginal_x
```

### Layout / Styling
```python
title        # Figure title string
labels       # Dict renaming axes/legend: {"col": "Display Name"}
template     # 'plotly' | 'plotly_white' | 'plotly_dark' | 'ggplot2' | 'seaborn' | 'simple_white' | 'none'
width        # Pixels
height       # Pixels
```

### Axis Control
```python
log_x, log_y    # Bool — log scale
range_x, range_y  # [min, max] — axis range
```

### Color Mapping
```python
color_discrete_map     # {"category": "#hex"} — explicit discrete colors
color_discrete_sequence  # List of colors for auto-assignment
color_continuous_scale  # Named scale: 'Viridis' | 'RdBu' | 'Blues' | etc.
range_color            # [min, max] override for continuous scale
```

### Ordering
```python
category_orders  # {"col": ["val1", "val2"]} — force sort order
```

---

## Patterns

### Tidy (long) data vs wide data
px works best with **tidy / long-form** DataFrames where one row = one observation. Use `pd.melt()` to reshape wide data.

```python
# Wide → long
df_long = df.melt(id_vars=["year"], var_name="country", value_name="gdp")
fig = px.line(df_long, x="year", y="gdp", color="country")
```

### Post-hoc customization
```python
fig = px.scatter(df, x="x", y="y")
fig.update_layout(font_size=14, plot_bgcolor="white")
fig.update_traces(marker_size=10, marker_color="steelblue")
fig.update_xaxes(showgrid=True, gridcolor="lightgrey")
```

### Built-in sample datasets
```python
px.data.gapminder()   # World GDP/life expectancy by year
px.data.iris()        # Classic iris dataset
px.data.tips()        # Restaurant tips
px.data.stocks()      # Stock prices
px.data.wind()        # Wind direction/speed
```

### Export
```python
fig.write_html("chart.html")
fig.write_image("chart.png")   # requires kaleido
fig.to_json()                  # serialized JSON string
```

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Passing wide DataFrame to `px.line(color=...)` | Melt to long first; or use `px.line(df, y=["col1","col2"])` |
| Color not showing for `scatter` | Check column is categorical (string/object); for float, it uses continuous scale automatically |
| Subplot x-axes not independent | Add `facet_col_spacing` or `fig.update_xaxes(matches=None)` |
| Animation is jumpy | Set `animation_group` to a consistent ID column |
| Trendline on log axis | Set `log_x=True` in px, not post-hoc; trendline fits on log scale |
| `imshow` colorscale wrong direction | Add `color_continuous_scale='RdBu_r'` (reversed) |

---

## Quick Snippets

```python
# Scatter with trendline + marginals
px.scatter(df, x="x", y="y", trendline="ols",
           marginal_x="histogram", marginal_y="box")

# Animated bubble chart
px.scatter(px.data.gapminder(), x="gdpPercap", y="lifeExp",
           size="pop", color="continent", hover_name="country",
           log_x=True, animation_frame="year", size_max=55)

# Stacked bar
px.bar(df, x="quarter", y="revenue", color="product",
       barmode="stack", text_auto=".2s")

# Faceted histogram
px.histogram(df, x="value", facet_col="group",
             color="group", nbins=30, template="plotly_white")

# Choropleth
px.choropleth(df, locations="iso_code", color="value",
              locationmode="ISO-3", color_continuous_scale="Blues")
```

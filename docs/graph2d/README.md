# Graph2d reference

## Axis modes

The `options.xAxis` setting controls how `x` values are interpreted.
Defaults to `time`.

| Mode | Example `x` value | Use it for |
| --- | --- | --- |
| `time` (default) | `"2026-01-01"` | Dates and timestamps |
| `numeric` | `42` | Measurements, counts, anything numeric that isn't a date |
| `category` | `"Mon"` | Named, unordered categories like weekdays or labels |

See `../../examples/graph2d/axes-and-legend.md` for a runnable block of each mode.

## Block reference

Options set under `options:` at the top of a block. Anything not listed
here is passed straight through to vis Graph2d untouched — `hiddenDates`,
`locales`, and every other Graph2d option not named below all work
without the plugin knowing about them.

| Option | Type | Description |
| --- | --- | --- |
| `xAxis` | `time \| numeric \| category` | X-axis interpretation. Default `time`. |
| `height` | CSS length, e.g. `"400px"` | Height of the **plotting area**. The rendered widget is this tall plus the x-axis strip drawn beneath it (roughly 30px). |
| `legend` | `boolean \| object` | Show a legend; see `../../examples/graph2d/axes-and-legend.md` for positioning. |
| `stack` | `boolean` | Stack bar groups at each `x` instead of overlapping them. |
| `sort` | `boolean` | Sort items by `x` before drawing. See `../../examples/graph2d/chart-types.md` for a large-series example. |
| `sampling` | `boolean` | Downsample dense line series for performance. See `../../examples/graph2d/chart-types.md`. |
| `zoomable` | `boolean` | Allow the user to zoom the chart. |
| `moveable` | `boolean` | Allow the user to pan the chart. |
| `zoomKey` | `string` | Modifier key (e.g. `"ctrlKey"`) required to zoom with the scroll wheel. |
| `start` / `end` | axis value | Initial visible range, in your data's own units on every axis mode (numeric/category values are mapped onto Graph2d's internal range for you). Either may be given alone; the other is filled in from the data. See `../../examples/graph2d/axes-and-legend.md`. |
| `min` / `max` | axis value | Bounds beyond which the user cannot pan or zoom. Same unit handling as `start`/`end`. |
| `zoomMin` / `zoomMax` | number | Tightest/widest allowed zoom, as a **duration** rather than a position. On `numeric`/`category` axes this is also in your own data's units (e.g. `zoomMin: 2` means "never zoom in past a 2-unit-wide window"), converted internally the same way `start`/`end` are. |
| `dataAxis` | object | Left/right axis titles, ranges, `alignZeros`, `icons`. |
| `barChart` | object | `sideBySide`, `align`, `minWidth` for bar groups. |
| `drawPoints` | `boolean \| object` | Default point-drawing behaviour for all groups. |
| `showCurrentTime` | `boolean` | Draw a marker at the current time (time axis only). |
| `locale` | `string` | Locale for vis's built-in date formatting (time axis only — numeric/category labels are formatted by this plugin, not by locale). See `../../examples/graph2d/axes-and-legend.md`. |
| `groups` | object | Raw vis group settings, e.g. `groups.visibility` (initial shown/hidden state per group id) — **not** the same thing as the block's own top-level `groups:` key documented below, despite the shared name. See `../../examples/graph2d/axes-and-legend.md`. |
| `moment` | function | **Footgun:** setting this yourself overrides the UTC pin that `numeric` and `category` axis labels depend on internally, which will visibly shift or break those labels. Leave it unset on those two modes; it's safe (and has its ordinary vis meaning) on `time`. |

## Item reference

Fields on each entry in `items:`.

| Field | Description |
| --- | --- |
| `x` | Position on the x-axis; shape depends on `xAxis` mode. |
| `y` | Numeric value. Required. |
| `group` | Id of the group this point belongs to. |
| `end` | End position for a spanning bar (draws from `x` to `end`). |
| `label` | `{ content, xOffset, yOffset, className }` — a text label attached to the point. See `../../examples/graph2d/chart-types.md` for a runnable example. |

## Group reference

Fields on each entry in `groups:`. Friendly fields are sugar the plugin
compiles for you; raw fields are forwarded to vis untouched.

**The `style` asymmetry:** at the *block* level, `options.style` sets the
default graph type for groups that don't declare their own. On a
*group*, `style:` is raw inline CSS — never a graph type. A group's
graph type is spelled `type:` instead.

Friendly fields:

| Field | Compiles to |
| --- | --- |
| `type: line \| bar \| points` | The group's graph type. |
| `color` | Stroke and fill colour. |
| `fill` | `true`, `"below"`, `"above"`, `{ below: true }`, `{ above: true }`, or `{ to: otherGroupId }` — shaded area. Precedence when more than one could match: `to` > `below` > `above`. A numeric `below`/`above` (e.g. `{ below: 20 }`) throws: shading is relative to the axis, not an arbitrary value — see `../../examples/graph2d/styling.md`. |
| `width` | Stroke width in pixels. |
| `dashes` | Dash pattern, e.g. `[5, 5]`. |
| `points` | `false`, or `{ style: circle \| square, size }` — point markers. |
| `interpolation` | `false`, `centripetal`, `chordal`, or `uniform` — line smoothing. |
| `content` | Legend / label text. Defaults to the group's `id`. |
| `visible` | `false` hides the group without deleting its data. |

Raw pass-through fields (forwarded to vis exactly as written, and always
win over anything a friendly field above would compile to):

| Field | Description |
| --- | --- |
| `style` | Raw inline CSS string. Never interpreted as a graph type. |
| `className` | CSS class added to the group's SVG elements. |
| `options` | Raw vis group options object, merged in last. |
| `yAxisOrientation` | `left` (default) or `right`. |
| `excludeFromLegend` | Hide from the legend while still rendering. |
| `excludeFromStacking` | Opt out of `stack: true`. |
| `barChart` | Per-group bar layout overrides. |

## Data files

Data can be inline `items:`, a shared columnar `x:`/`y:` form, or loaded
from an external CSV/JSON/YAML file in the vault. See
[`../../examples/graph2d/data-files.md`](../../examples/graph2d/data-files.md)
for the full set of runnable examples, including block-level vs. group-level
`data:`, wikilink references, and the columnar form.

## Styling

Default series colours follow Obsidian's theme so charts read correctly
in light and dark mode. Ready-made looks ship as CSS snippets in
[`docs/themes/`](https://github.com/rborisov/vis-charts/tree/main/docs/themes)
— copy one into **Settings → Appearance → CSS snippets**
to change every chart's default colours and line styles at once.

For per-chart styling, `className` targets one group directly:

```vis-graph2d
options:
  xAxis: numeric
groups:
  - id: revenue
    content: Revenue
    className: revenue-series
items:
  - { x: 1, y: 20, group: revenue }
  - { x: 2, y: 32, group: revenue }
  - { x: 3, y: 41, group: revenue }
```

```css
.graph2d-plugin .revenue-series .vis-line {
  stroke-width: 3px;
}
```
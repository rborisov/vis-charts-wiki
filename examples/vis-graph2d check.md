# vis-graph2d — visual check

Scratch note for verifying the plugin renders correctly. Safe to delete.
Tick each box as you confirm it.
2026-07-27

## 1. Time axis — should show readable date labels

- [ ] renders with dates

```vis-graph2d
options:
  legend: true
groups:
  - id: rev
    content: Revenue
items:
  - { x: "2026-01-01", y: 120, group: rev }
  - { x: "2026-02-01", y: 185, group: rev }
  - { x: "2026-03-01", y: 143, group: rev }
  - { x: "2026-04-01", y: 210, group: rev }
  - { x: "2026-05-01", y: 267, group: rev }
```

## 2. Numeric axis — labels must read 0, 10, 20 … NOT dates

This is the one that was broken twice during development. If you see dates or
fractions like `51.333`, something regressed.

- [ ] shows round numbers

```vis-graph2d
options:
  xAxis: numeric
x: [0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
groups:
  - id: sq
    content: Squares
    color: "#e11d48"
    fill: true
    y: [0, 100, 400, 900, 1600, 2500, 3600, 4900, 6400, 8100, 10000]
```

## 3. Numeric axis with an explicit window — fixed in 0.1.1

`start`/`end` used to collapse to a 10ms sliver here.

- [ ] shows only the 20–60 range, with sane labels

```vis-graph2d
options:
  xAxis: numeric
  start: 20
  end: 60
x: [0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
groups:
  - id: sq
    content: Squares
    y: [0, 100, 400, 900, 1600, 2500, 3600, 4900, 6400, 8100, 10000]
```

## 4. Category axis — day names under the bars

- [ ] shows Mon/Tue/Wed/Thu/Fri

```vis-graph2d
options:
  xAxis: category
  legend: true
groups:
  - id: sales
    content: Sales
    type: bar
items:
  - { x: Mon, y: 12, group: sales }
  - { x: Tue, y: 19, group: sales }
  - { x: Wed, y: 7,  group: sales }
  - { x: Thu, y: 23, group: sales }
  - { x: Fri, y: 15, group: sales }
```

## 5. Default palette — no explicit colours

Check this one in **both light and dark themes**. Colours come from Obsidian's
own variables, so they should stay legible in each, and axis text and gridlines
should follow the theme too.

- [ ] legible in light theme
- [ ] legible in dark theme
- [ ] axis text and gridlines follow the theme
- [ ] legend readable and correctly positioned

```vis-graph2d
options:
  xAxis: numeric
  legend: true
x: [1, 2, 3, 4, 5, 6]
groups:
  - id: a
    content: Series A
    y: [4, 8, 6, 11, 9, 14]
  - id: b
    content: Series B
    y: [7, 5, 9, 6, 12, 8]
  - id: c
    content: Series C
    y: [2, 3, 5, 4, 6, 5]
```

## 6. Mixed types, both axes, shading between series

- [ ] bars and lines together, left and right axes, shaded band

```vis-graph2d
options:
  xAxis: numeric
  legend: true
  dataAxis:
    left:  { title: { text: "Revenue" } }
    right: { title: { text: "Units" } }
x: [1, 2, 3, 4, 5, 6]
groups:
  - id: units
    content: Units
    type: bar
    yAxisOrientation: right
    y: [30, 45, 38, 52, 41, 60]
  - id: low
    content: Low band
    color: "#94a3b8"
    dashes: [4, 4]
    y: [5, 6, 7, 8, 9, 10]
  - id: high
    content: High band
    color: "#0ea5e9"
    fill:
      to: low
    y: [12, 15, 14, 18, 17, 21]
```

## 7. Scatter — points only, no connecting line

- [ ] discrete points, no line

```vis-graph2d
options:
  xAxis: numeric
groups:
  - id: obs
    content: Observations
    type: points
    interpolation: false
    points: { style: circle, size: 8 }
    color: "#7c3aed"
items:
  - { x: 1.2, y: 3.4, group: obs }
  - { x: 2.8, y: 5.1, group: obs }
  - { x: 3.5, y: 2.9, group: obs }
  - { x: 4.9, y: 6.7, group: obs }
  - { x: 6.1, y: 4.2, group: obs }
  - { x: 7.4, y: 7.9, group: obs }
```

## 8. Error handling — must show a red-bordered box, not a blank space

- [ ] shows a readable error, not an empty block

```vis-graph2d
options:
  xAxis: numeric
groups:
  - id: a
    type: pie
items:
  - { x: 1, y: 1, group: a }
```

- [ ] this one names the missing field clearly

```vis-graph2d
items:
  - { x: "2026-01-01" }
```

## Remaining checks (not blocks)

- [ ] **Settings** → Community plugins → Graph2d shows four settings. Change
      **Default chart height** to `250px` and confirm the charts above (which
      set no height) get shorter.
- [ ] **CSS themes** — copy a file from `docs/themes/` in the repo into
      `.obsidian/snippets/`, enable it under Settings → Appearance, and confirm
      it changes the charts and nothing else in the vault.
- [ ] **Data file** — create a CSV in the vault with an `x,y` header, point a
      block at it with `data: path/to/file.csv`, then edit the CSV and confirm
      the chart updates without reopening the note.
- [ ] **pubobs export** — publish a note containing a chart and confirm it
      renders as a static PNG rather than an interactive widget.
- [ ] **Mobile** — open a note with a chart on iOS/Android. The manifest claims
      `isDesktopOnly: false`; this has been verified in code but never on a
      device.

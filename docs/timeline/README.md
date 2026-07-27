# Timeline reference

## Per-block options

````markdown
```vis-timeline
options:
  height: 400px
  orientation: bottom
  stack: false
items:
  - content: Battle of Marathon
    start: "-490-09-12"
```
````

## Groups

Assign items to labelled rows using `groups` and `group` on each item:

````markdown
```vis-timeline
groups:
  - id: military
    content: Military
  - id: political
    content: Political
items:
  - content: Battle of Hastings
    start: "1066-10-14"
    group: military
  - content: Norman Conquest
    start: "1066"
    end: "1072"
    group: political
```
````

Nest groups with `nestedGroups` (a list of child group IDs):

````markdown
```vis-timeline
groups:
  - id: europe
    content: Europe
    nestedGroups: [uk, france]
  - id: uk
    content: United Kingdom
  - id: france
    content: France
items:
  - content: Magna Carta
    start: "1215"
    group: uk
```
````

## Background items

Shade a time range behind other items using `type: background`. Add `group` to confine the shading to a single row.

````markdown
```vis-timeline
items:
  - start: "1337"
    end: "1453"
    type: background
    className: war

  - content: Battle of Agincourt
    start: "1415-10-25"
```
````

## Date formats

| Input              | Meaning            |
| ------------------ | ------------------ |
| `"1066-10-14"`     | 14 Oct 1066 CE     |
| `"1066"`           | 1 Jan 1066 CE      |
| `"-490-09-12"`     | 12 Sep 490 BCE     |
| `"-490"`           | 1 Jan 490 BCE      |
| `"490-09-12 BCE"`  | 12 Sep 490 BCE     |
| `"490 BCE"`        | 1 Jan 490 BCE      |

> **Note:** Year 0 does not exist in historical notation. Use `-1` for 1 BCE and `1` for 1 CE.

CE dates are passed directly to vis-timeline. BCE dates are converted internally to JavaScript `Date` objects, working around moment.js's lack of negative-year support.

## Item fields

| Field       | Type   | Notes                                       |
| ----------- | ------ | ------------------------------------------- |
| `content`   | string | Label shown on the item                     |
| `start`     | string | Required. CE or BCE date string.            |
| `end`       | string | Optional. Makes the item a range.           |
| `type`      | string | `point`, `box`, `range`, or `background`    |
| `className` | string | CSS class for custom styling                |
| `title`     | string | Tooltip. Auto-generated for BCE items.      |
| `group`     | string | Row grouping — matches a group `id`         |

## Options

| Option        | Type    | Default | Notes                      |
| ------------- | ------- | ------- | -------------------------- |
| `height`      | string  | `75vh`  | Container height           |
| `orientation` | string  | `top`   | `top`, `bottom`, or `both` |
| `stack`       | boolean | `true`  | Stack overlapping items    |
| `zoomMin`     | number  | 10 yrs  | Min zoom window (ms)       |
| `zoomMax`     | number  | 10k yrs | Max zoom window (ms)       |

## Obsidian Bases

The plugin registers a **Timeline** view type for [Obsidian Bases](https://obsidian.md/help/bases) (Obsidian 1.10+). Add a `start` property to your notes, create a `.base` file, and switch to the Timeline view.

The following note properties are read automatically — no view-option configuration needed:

| Property    | Behaviour in the Timeline view                      |
| ----------- | --------------------------------------------------- |
| `start`     | Required. Item start date.                          |
| `end`       | Optional. Makes the item a range.                   |
| `group`     | Optional. Assigns the item to a row.                |
| `type`      | `background` shades a time range behind all items.  |
| `className` | CSS class applied to the item for custom styling.   |
| `title`     | Tooltip text shown on hover.                        |

The **Start date**, **End date**, **Label**, and **Group** fields in the view-options panel let you override which property is used for each role.

Clicking a timeline item opens the corresponding note.

A ready-to-use example with 20 notes covering the Beatles era is in [`../../examples/timeline/bases/beatles-era/`](../../examples/timeline/bases/beatles-era/). It demonstrates point events, range events, groups, background shading, custom CSS classes, and tooltips.

## CSS customisation

Items accept any `className` value. Target items with `.timeline-plugin .vis-item.your-class`:

```css
.timeline-plugin .vis-item.landmark {
  background-color: #7c3aed;
  border-color: #5b21b6;
  color: #fff;
}

.vis-item.vis-background.era-rock {
  background: rgba(239, 68, 68, 0.08);
}
```
# vis-charts wiki

Documentation and worked examples for the
[vis-charts](https://github.com/rborisov/vis-charts) Obsidian plugin.

| Path | Contents |
| --- | --- |
| [`docs/timeline/`](docs/timeline/) | Reference for the `vis-timeline` code block |
| [`docs/graph2d/`](docs/graph2d/) | Reference for the `vis-graph2d` code block |
| [`examples/timeline/`](examples/timeline/) | Runnable timeline examples, plus two Bases-view vaults |
| [`examples/graph2d/`](examples/graph2d/) | Runnable Graph2d examples and their data files |

Every example here is a complete, copy-pasteable code block. The plugin's test
suite renders all of them on every build, so a broken example fails a build
rather than shipping.

## How this repo reaches the plugin

vis-charts vendors this repository as a git submodule pinned to a specific
commit. A change here does **not** affect the plugin until vis-charts bumps
that pin — which it does only once the new content passes its example gate.
That delay is deliberate: it means an edit here can never break the plugin's
build.

CSS themes live in the plugin repo, not here — they target class names the
plugin emits, so they change together with it.

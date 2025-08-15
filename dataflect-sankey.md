[Go back](index)

This documentation is for Sankey Visualization by Dataflect version 1.0.4. For older versions of Dataflect documentation contact us at [support@dataflect.com](mailto:support@dataflect.com).

For additional information, visit [dataflect.com](https://dataflect.com).

# Sankey Visualization by Dataflect — Usage Guide

Render a Sankey diagram from any search that outputs **exactly three columns**:
- **source** (string)
- **target** (string)
- **value** (numeric)

The visualization aggregates results across pages automatically, but you should **aggregate in SPL** for best performance.

---

## Data requirements

Your final pipeline must end with a table like:

| source | target | value |
|-------:|:------:|------:|
| A      | B      | 10    |
| A      | C      | 5     |
| B      | D      | 7     |

Notes:
- `value` must be numeric (use `tonumber()` if needed).
- If your field names differ, `rename` them to `source`, `target`, `value`.

---

## Quick start (Search UI or Dashboard panel)

1. Run a search that returns the three required columns.
2. Switch to the **Visualization** tab and choose the **Sankey** custom visualization.
3. Open **Format** to adjust **Link color** and **Node alignment** as desired.

---

## Example searches (copy/paste)

### 1) Minimal demo (hand-crafted flows)

    | makeresults
    | eval flows="S1>A 10;S1>B 5;S2>A 3;S2>C 7;S3>B 8;A>X 9;A>Y 4;B>X 6;B>Y 5;C>Y 7;X>Z1 8;X>Z2 7;Y>Z2 9;Y>Z3 6"
    | makemv delim=";" flows
    | mvexpand flows
    | rex field=flows "(?<source>[^> ]+)\s*>\s*(?<target>[^ ]+)\s+(?<value>\d+)"
    | eval value=tonumber(value)
    | table source target value

### 2) Aggregate your own data (rename fields to match)

    <your base search producing src, dst, count>
    | stats sum(count) as value by src dst
    | rename src as source dst as target
    | where value > 0
    | table source target value

---

## Formatting options (Format → Sankey)

### Link color
- **By link (random)** — Each edge gets a distinct, stable color derived from its source→target pair.
- **By source node** — All edges leaving the same source share that source’s color.
- **By target node** — All edges entering the same target share that target’s color.
- **Source → Target gradient** — Each edge blends from its source color to its target color along the path.
- **None (gray)** — Neutral monochrome edges.

Tip: Use *By source* or *By target* to emphasize fan-out vs. fan-in relationships. Use *Gradient* to show flow transitions.

### Node alignment
- **Justify** — Places sources at the far left, sinks at the far right; intermediate columns stretch to fill available breadth (good default).
- **Left** — Packs nodes as far left as allowed by the graph.
- **Center** — Centers nodes between their earliest and latest possible columns.
- **Right** — Packs nodes as far right as allowed.

Tip: Switch between **Left**, **Center**, **Right**, and **Justify** on datasets with multiple interior layers to see distinct layouts.

---

## Usage tips

- Always **aggregate** before the final `table`:
  
      ... | stats sum(value) as value by source target | where value > 0

- Ensure the graph is **acyclic**; Sankey represents directional flow between columns.
- For very large result sets, keep the number of unique `(source, target)` pairs to a reasonable size for interactivity.

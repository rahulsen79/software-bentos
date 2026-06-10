---
name: dataviz-chart
description: Build a chart from the Volvo Cars Data Viz Figma library. Trigger when the user shares a Figma URL containing the file key h3EQyhmk9CwPWdtNPV53AW, or asks to implement a chart from that library. Fetches the design, implements it with recharts, and offers an interactive data playground.
argument-hint: "<Figma node URL from figma.com/design/h3EQyhmk9CwPWdtNPV53AW>"
---

# /dataviz-chart

Build a chart from the Volvo Cars Data Viz Figma library using recharts.

## Usage

```
/dataviz-chart $ARGUMENTS
```

---

## Guiding principles (read first)

These rules come from the official Volvo Cars Dataviz Figma Make Kit guidelines. Follow them in every implementation.

- Follow the **Volvo Cars Web Design System (VCDS)** for typography, spacing, and tone.
- Use VCDS color tokens — never raw hex values for UI chrome — so charts inherit light/dark theming.
- Charts must feel calm, confident, and informative. No gradients, drop shadows, 3D effects, or decorative styling not present in VCDS.
- **Never pick a chart type for visual variety.** Always choose based on the goal and purpose of the data.

---

## Step 1 — Determine the chart goal

Before fetching the Figma design, always rely on the **goal and purpose** of the chart to decide which type to use. Never pick a chart for visual variety. If unclear, ask the user.

| Goal / question being answered | Use |
|---|---|
| Compare categories (which is biggest/smallest?) | **Bar chart** |
| Show a trend over time | **Line chart** |
| Show a trend over time *with magnitude / volume* | **Area chart** |
| Show part-to-whole (composition of 100%) | **Donut chart** *(universal dataviz standard)* |
| Show a single KPI / snapshot value | **Big number** *(universal dataviz standard)* |
| Show progress toward a target | **Progress indicator** |
| Show an inline trend inside a metric | **Big number with sparkline / mini chart** |

If a request maps to a chart not covered here, fall back to the **universal data-visualization standard** (and the [Recharts](https://recharts.org/) defaults) for behavior, anatomy, and labeling.

### Bar chart

Use **Bar** to compare values across categories.

- Orientation: vertical for time/ordinal categories with short labels; horizontal when category labels are long or there are many categories.
- Grouped vs stacked: use **grouped** to compare values within each category; use **stacked** to also show the total.
- Apply colors from the Color palette in Step 6. In Standard/Complex mode, layer **Textures** to extend the number of distinguishable series and to satisfy color-blind contrast (see §10).
- Place subtle and strong shades of the same hue **next to each other** to maximize edge contrast.

---

### Line chart

Use **Line** to show how a value changes over a continuous dimension (usually time).

- Use the primary palette colors from the Color palette in Step 6.
- Follow Recharts defaults for stroke weight, axis ticks, gridlines, markers and tooltip behavior unless overridden by VCDS.
- Keep the number of series within the active visualization mode (see §3).

---

### Area chart

Use **Area** when both the trend and the magnitude/volume matter, or to show stacked composition over time.

- Same primary palette as Line.
- For overlapping (non-stacked) areas, reduce fill opacity so all series remain readable.
- Follow Recharts defaults for stacking, baseline and labeling unless overridden by VCDS.

---

### Donut chart

Use **Donut** for part-to-whole composition (one group of values that sums to 100%).

- Follow the universal dataviz standard: limit the number of segments, order them by size, and group small slices into an *Other* segment when needed.
- Apply hue + shade pairing the same way as Bar (§5) so adjacent slices stay distinguishable for color-blind users (§10).
- Use the primary palette from the Color palette in Step 6.

---

### Big number

Use **Big number** to highlight a single KPI / snapshot value.

- Typography, spacing and color follow VCDS.
- An inline **sparkline / mini chart** can be added when the goal is to show the recent trend of that KPI. The sparkline uses the same primary palette as Line.
- Use a trend indicator (delta + direction) when comparison to a previous period is meaningful.

| Anatomy | In context |

---

### Progress indicator

Use **Progress indicator** to show advancement toward a defined target (e.g. % of goal, completion of a task, charge level).

- Use the primary palette colors. Do not use semantic (success/warning/error) colors unless the value itself carries that meaning.
- Show the value (e.g. percentage or current/target) next to the indicator.
- Pick the form that fits the layout (linear vs circular) following the universal dataviz standard.

---

## Step 2 — Pick the visualization mode (data density)

Choose the lowest mode that still communicates the data clearly.

| Mode | When to use | Color & texture rules |
|---|---|---|
| **Simple** | 1 series, or a small number of categories where one value dominates | Colors only. No textures. Typically a single hue. |
| **Standard** | 2–4 series that need comparison | Colors, optionally with textures. **Max 2 hues** (e.g. blueberry + mossy). Use subtle + strong shade of each hue for up to 4 distinguishable series. |
| **Complex** | 5+ series, dense dashboards, analytical views | Colors **and** textures. More than 3 hues only if needed. Always pair with **filters** so users can reduce density. |

---

## Step 3 — Extract the Figma node ID

The Data Viz library file key is `h3EQyhmk9CwPWdtNPV53AW`.

From the URL `https://www.figma.com/design/h3EQyhmk9CwPWdtNPV53AW/...?node-id=462-11622`, extract the `node-id` parameter and convert dashes to colons:
- URL: `node-id=462-11622` → CLI: `--nodeId 462:11622`

If no Figma URL was provided, ask the user to share one before proceeding.

---

## Step 4 — Fetch the design with the Figma CLI

The Figma CLI tool lives in the sibling `figma` skill directory (`$SKILL_DIR` resolves to this file's folder):

**1. Get design context:**
```bash
$SKILL_DIR/../figma/figma-mcp-cli.mjs call get_design_context \
  --nodeId "<nodeId>" \
  --clientLanguages "typescript" \
  --clientFrameworks "react"
```

**2. Take a screenshot for visual reference:**
```bash
$SKILL_DIR/../figma/figma-mcp-cli.mjs call get_screenshot \
  --nodeId "<nodeId>"
```

> If the CLI fails with a connection error, ask the user to open the Figma desktop app and try again. If Figma is unavailable, ask the user to describe the chart type and data so you can proceed without the design.
>
> If the URL does not contain file key `h3EQyhmk9CwPWdtNPV53AW`, confirm with the user before proceeding.

From the design context, identify:
- **Chart type and mode** — cross-reference with Steps 1 and 2
- **Color palette** — which hues and shades are used
- **Labels and axes** — axis labels, legend position, data labels, tooltips
- **Textures** — are patterns applied to bars/segments? (Standard/Complex mode only)
- **Visual style** — rounded corners, gap between bars, inner radius for donuts

---

## Step 5 — Install recharts if needed

Check `package.json` for `recharts`. If absent:

```bash
npm install recharts
```

If the install fails, stop and report the error before continuing.

---

## Step 6 — Apply colors correctly


### Color palette


Each hue ships in **subtle** and **strong** shades. The VCDS token names follow this pattern:

```
--v-color-data-<hue>-subtle   (lighter shade)
--v-color-data-<hue>-strong   (darker shade)
```

Use these pairings to:

1. Encode hierarchy within a single hue (strong = primary series, subtle = secondary).
2. Provide contrast for users with color vision deficiency when subtle and strong are placed next to each other

Use the VCDS tokens above as the primary color palette for **bar, line, area, donut, and the sparkline/mini chart inside Big number**.
Do **not** introduce custom colors.

Since recharts renders as SVG, CSS custom properties work natively in `fill` and `stroke` props — pass them directly.



Each hue (e.g. *blueberry*, *mossy*) ships in a **subtle** and a **strong** shade. 

### Visualization modes
Pick the lowest mode that still communicates the data.

| Mode | When to use | Color & texture rules |
|---|---|---|
| **Simple**   | Single series, or a small number of categories where one value clearly dominates the message. | **Colors only.** No textures. Typically a single hue. |
| **Standard** | A few series that need to be compared. | Colors, optionally combined with textures. **Maximum 2 hues** (e.g. blueberry + mossy). Use both the subtle and strong shade of each hue to expand to up to 4 distinguishable series. |
| **Complex**  | Many series, dense dashboards, or analytical views. | Mix of colors **and** textures. Use **more than 3 hues only if needed**. Always pair complex charts with **filters** so users can reduce density on demand. |

**Series color assignment by mode:**

| Mode | Color rule |
|---|---|
| Simple | One hue, strong shade only (e.g. `--v-color-data-blueberry-strong`) |
| Standard | Up to 2 hues × 2 shades = 4 series. Order: strong then subtle of hue 1, then strong then subtle of hue 2. Place subtle and strong of the same hue adjacent for color-blind contrast. |
| Complex | Extend across more hues + textures as needed. Always place subtle and strong of the same hue adjacent. |


**Textures (bar/donut, Standard and Complex only):**  
Apply SVG `<pattern>` fills for bar series that need texture differentiation.

Bar chart has dedicated color and texture sets. Use them as shown:

| Bar – colors | Bar – textures |

Textures are reserved for **Standard** and **Complex** visualization modes — never use textures in Simple mode.

> If the Figma design uses specific token names, use those. Otherwise apply the palette above.



---



## Step 7 — Implement the chart component

Create a `"use client"` React component in `src/app/`. Name it after the chart type, e.g. `BarChart.tsx`, `DonutChart.tsx`, `LineChart.tsx`, `BigNumber.tsx`.

### Component skeleton

```tsx
"use client";

import { useState, useMemo } from "react";
import {
  // Import only what you need — recharts is tree-shakeable:
  // BarChart, Bar, LineChart, Line, AreaChart, Area,
  // PieChart, Pie, Cell, ComposedChart,
  // XAxis, YAxis, CartesianGrid, Tooltip, Legend,
  // ResponsiveContainer
} from "recharts";

export function MyChart() {
  // ...
}
```

Always wrap the chart in `<ResponsiveContainer width="100%" height={height}>`.

### Recharts component map

| Chart type | Main component | Series component | Notes |
|---|---|---|---|
| Bar (vertical) | `BarChart` | `Bar` | Add `CartesianGrid`, `XAxis`, `YAxis` |
| Bar (horizontal) | `BarChart` | `Bar` | `layout="horizontal"`, swap `XAxis`/`YAxis` types |
| Grouped bar | `BarChart` | Multiple `Bar` | One `Bar` per series |
| Stacked bar | `BarChart` | Multiple `Bar` | Add `stackId="a"` to each `Bar` |
| Line | `LineChart` | `Line` | `dot={false}` for clean lines |
| Area | `AreaChart` | `Area` | `fillOpacity` for overlapping areas; `stackId` for stacked |
| Donut | `PieChart` | `Pie` + `Cell` | `innerRadius="60%"` `outerRadius="80%"` `paddingAngle={2}` |
| Big number + sparkline | `LineChart` (sparkline) | `Line` | No axes, no grid, `height={40}` |
| Progress (linear) | `BarChart` (single bar, horizontal) | `Bar` | Fixed max, single series |

### Styling — VCDS tokens in SVG

```tsx
<CartesianGrid strokeDasharray="3 3" stroke="var(--v-color-ornament-divider)" vertical={false} />
<XAxis
  dataKey="label"
  axisLine={{ stroke: "var(--v-color-ornament-divider)" }}
  tickLine={false}
  tick={{ fill: "var(--v-color-foreground-secondary)", fontFamily: "'Volvo Novum', sans-serif", fontSize: 12 }}
/>
<YAxis
  axisLine={false}
  tickLine={false}
  tick={{ fill: "var(--v-color-foreground-secondary)", fontFamily: "'Volvo Novum', sans-serif", fontSize: 12 }}
/>
```

| Purpose | Token |
|---|---|
| Grid / axis lines | `var(--v-color-ornament-divider)` |
| Axis tick text | `var(--v-color-foreground-secondary)` |
| Legend text | `var(--v-color-foreground-primary)` |
| Tooltip background | `var(--v-color-background-feedback-gray)` |
| Tooltip text | `var(--v-color-foreground-primary)` |

### Custom tooltip

Always use a custom `content` prop on `<Tooltip>` to stay on-brand:

```tsx
import type { TooltipProps } from "recharts";

function CustomTooltip({ active, payload, label }: TooltipProps<number, string>) {
  if (!active || !payload?.length) return null;
  return (
    <div
      className="micro text-primary flex gap-8 items-baseline"
      style={{
        background: "var(--v-color-background-feedback-gray)",
        borderRadius: 9999,
        padding: "6px 14px",
        whiteSpace: "nowrap",
      }}
    >
      <span>{label}</span>
      {payload.map((entry) => (
        <span key={entry.name} style={{ color: entry.color }}>
          {entry.name}: {entry.value}
        </span>
      ))}
    </div>
  );
}

// Usage:
<Tooltip content={<CustomTooltip />} cursor={{ fill: "var(--v-color-background-secondary)" }} />
```

### Donut — center label

Recharts has no built-in center label. Layer an absolutely-positioned element over the chart:

```tsx
<div className="relative" style={{ width: "100%", height: 320 }}>
  <ResponsiveContainer width="100%" height="100%">
    <PieChart>
      <Pie data={data} dataKey="value" innerRadius="60%" outerRadius="80%" paddingAngle={2}>
        {data.map((entry, i) => (
          <Cell key={entry.name} fill={`var(--v-color-data-${entry.hue}-${entry.shade})`} />
        ))}
      </Pie>
      <Tooltip content={<CustomTooltip />} />
    </PieChart>
  </ResponsiveContainer>

  <div className="absolute inset-0 flex flex-col items-center justify-center pointer-events-none" aria-hidden>
    <span className="font-24 text-primary">{total}</span>
    <span className="micro text-secondary">Total</span>
  </div>
</div>
```

### Donut — active shape on hover

```tsx
import { Sector } from "recharts";
import type { PieSectorDataItem } from "recharts/types/polar/Pie";

const renderActiveShape = (props: PieSectorDataItem) => {
  const { cx, cy, innerRadius, outerRadius = 0, startAngle, endAngle, fill } = props;
  return (
    <Sector cx={cx} cy={cy}
      innerRadius={innerRadius} outerRadius={outerRadius + 6}
      startAngle={startAngle} endAngle={endAngle} fill={fill} />
  );
};

<Pie activeIndex={activeIndex} activeShape={renderActiveShape}
  onMouseEnter={(_, i) => setActiveIndex(i)}
  onMouseLeave={() => setActiveIndex(undefined)} ... />
```

### Big number

```tsx
<div className="flex flex-col gap-4">
  <span className="font-48 text-primary">{value}</span>
  <span className="micro text-secondary">{label}</span>
  {delta && (
    <span className={delta > 0 ? "text-positive" : "text-negative"} style={{ fontSize: 12 }}>
      {delta > 0 ? "▲" : "▼"} {Math.abs(delta)}%
    </span>
  )}
  {sparklineData && (
    <ResponsiveContainer width="100%" height={40}>
      <LineChart data={sparklineData}>
        <Line type="monotone" dataKey="value"
          stroke="var(--v-color-data-blueberry-strong)"
          dot={false} strokeWidth={2} />
      </LineChart>
    </ResponsiveContainer>
  )}
</div>
```

---

## Step 8 — Validate accessibility

Before finishing, check:

1. **Contrast** — text/label elements must meet WCAG 4.5:1; graphical elements (axis lines, bar fills) must meet 3:1.
2. **Non-color encoding** — never encode meaning by color alone. Pair color with a label, value, legend entry, or (in Standard/Complex mode) a texture.
3. **Subtle/strong adjacency** — for bar and donut charts, ensure the subtle and strong shades of the same hue are placed next to each other so adjacent segments have a high luminance difference.
4. **Textures in Standard/Complex** — if the mode is Standard or Complex and the chart is a bar or donut, confirm that texture patterns are applied alongside colors.
5. **Color-blind safety:** for **bar chart** and **donut chart**, place the **subtle and strong shade of the same hue next to each other** so adjacent segments always have a high luminance difference. In Standard and Complex modes, reinforce color with **textures** (see `Bar chart texture.png`).
- Never encode meaning by **color alone** — always pair color with a label, value, legend entry, icon or texture.
- Inherit focus, hover and tooltip behavior from VCDS components.
- For everything not covered above, follow the universal dataviz accessibility standard and Recharts defaults.


---

## Step 9 — Ask the user about the data playground

After implementing the chart, **always ask**:

> "Would you like a data playground below the chart? This adds sliders and inputs so you can test the chart with different values without touching the code."

**If yes**, add an interactive control panel. Use `useState` for the data array:

```tsx
const [data, setData] = useState([
  { label: "Sweden", value: 53000 },
  { label: "Germany", value: 35000 },
]);

function updateLabel(i: number, label: string) {
  setData(prev => prev.map((d, idx) => idx === i ? { ...d, label } : d));
}
function updateValue(i: number, value: number) {
  setData(prev => prev.map((d, idx) => idx === i ? { ...d, value } : d));
}
function deleteItem(i: number) {
  setData(prev => prev.filter((_, idx) => idx !== i));
}
function addItem() {
  setData(prev => [...prev, { label: `Item ${prev.length + 1}`, value: 10000 }]);
}
```

Control row per data point:

```tsx
{data.map((item, i) => (
  <div key={i} className="flex items-center gap-8">
    <div style={{ width: 10, height: 10, borderRadius: "50%",
      backgroundColor: `var(--v-color-data-blueberry-strong)`, flexShrink: 0 }} />

    <input type="text" value={item.label} onChange={e => updateLabel(i, e.target.value)}
      className="micro text-primary"
      style={{ width: 90, border: "1px solid var(--v-color-ornament-primary)",
        borderRadius: 4, padding: "3px 8px", background: "var(--v-color-background-primary)" }} />

    <input type="range" min={0} max={maxValue} step={step} value={item.value}
      onChange={e => updateValue(i, Number(e.target.value))}
      style={{ flex: 1 }} />

    <span className="micro text-secondary" style={{ width: 36, textAlign: "right", flexShrink: 0 }}>
      {item.value.toLocaleString()}
    </span>

    {data.length > 1 && (
      <button onClick={() => deleteItem(i)} className="micro text-secondary"
        style={{ background: "none", border: "none", cursor: "pointer", padding: "0 4px" }}
        aria-label={`Remove ${item.label}`}>×</button>
    )}
  </div>
))}

<button onClick={addItem} className="micro text-secondary"
  style={{ border: "1px dashed var(--v-color-ornament-primary)", borderRadius: 4,
    padding: "6px 0", background: "none", cursor: "pointer", marginTop: 4 }}>
  + Add data point
</button>
```

---

## Step 10 — Wire it into the page

```tsx
// src/app/page.tsx
import { MyChart } from "./MyChart";

// Inside JSX:
<MyChart />
```

---

## Key rules

- Always use `"use client"` — recharts requires browser APIs
- Always wrap in `<ResponsiveContainer>` — never set a fixed pixel width on the chart root
- CSS custom properties (`var(--v-color-*)`) work natively in recharts SVG props — pass them directly
- **Never pick a chart type for visual variety** — choose based on the goal (Step 1)
- **No Tailwind** — use `@volvo-cars/css` utility classes only for non-chart HTML elements
- **No hardcoded hex values** — use `var(--v-color-*)` tokens everywhere except playground `accentColor`
- **Textures are only for Standard and Complex modes** — never apply them in Simple mode
- **Subtle + strong shades must be adjacent** in bar and donut charts for color-blind contrast
- Use `useMemo` for derived data (totals, formatted arrays) to avoid unnecessary re-renders
- Import only the recharts components you actually use
- If the Figma node cannot be fetched, ask the user to describe the chart type and data so you can proceed without the design

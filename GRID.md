# Dashboard Grid Spec — Skypoint Radix

The layout contract every screen aligns to. Implemented in `dashboard.css` (GRID SYSTEM section).

## Foundations
- **Base unit:** 4px. All spacing uses the Radix `--space-*` scale (4, 8, 12, 16, 24, 32, 40, 48, 64).
- **Columns:** **12**. Use only spans that divide 12 cleanly — **2 / 3 / 4 / 6 / 8 / 9 / 12**.
- **Gutter:** **16px** (`--grid-gutter`, = `--space-4`) between columns and cards.
- **Section gap:** **24px** (`--section-gap`) between stacked sections.
- **Page margin:** **24px** content inset (`--content-pad`), 16px on mobile.
- **Max layout width:** **1440px**, centered (`--content-max`) so ultra-wide doesn't stretch.
- **Reading measure:** **720px** (`--measure`) for prose / chat threads.

## Breakpoints
| name | width | behavior |
|------|-------|----------|
| sm | ≤ 640 | everything stacks to full width |
| md | ≤ 1024 | thirds/quarters → halves; 8/9 spans → full |
| lg | ≤ 1280 | full 12-col |
| xl | ≥ 1536 | full 12-col, capped at 1440 |

## Usage
```html
<div class="container">
  <div class="section-stack">
    <!-- 4-up KPI row -->
    <div class="grid">
      <section class="card col-3">…</section>  <!-- x4 -->
    </div>
    <!-- 2:1 split -->
    <div class="grid">
      <section class="card col-8">chart</section>
      <section class="card col-4">donut</section>
    </div>
    <!-- full-width table -->
    <div class="grid">
      <section class="card col-12">table</section>
    </div>
  </div>
</div>
```

## Standard layouts on the grid
| pattern | spans |
|---------|-------|
| KPI row (4-up) | `col-3` × 4 |
| 3-up cards | `col-4` × 3 |
| 2:1 split (chart + side) | `col-8` + `col-4` |
| 3:1 split | `col-9` + `col-3` |
| full table / banner | `col-12` |

## Known off-grid item (to reconcile)
The Portfolio **KPI row currently has 5 cards** — 5 does not divide 12. Options:
1. **Go 4-up** (`col-3` ×4) and fold "Communities" into the pulse card or a secondary stat. **(recommended)**
2. Keep 5-up as a documented exception using its own `.kpi-row` outside the 12-grid.

Until reconciled, 5-up is an intentional exception, not the grid default.

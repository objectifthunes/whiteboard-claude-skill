---
name: whiteboard-ui
description: Use when picking up, wiring up, or designing with the `@objectifthunes/whiteboard` npm package — a pan/zoom whiteboard canvas for React with draggable floating panels, a minimap, snap-to-grid, zoom controls, and ~55 themed UI primitives. Triggers whenever the user mentions @objectifthunes/whiteboard, the whiteboard package, "objectifthunes whiteboard", or specific exports (WhiteboardShell, FloatingPanel, Minimap, ZoomBar, ConfirmDialog, PanelErrorBoundary, useWhiteboardStore, useWhiteboardLayout, computeWhiteboardFit, computeWhiteboardRectFocus, snapToWhiteboardGrid, WHITEBOARD_GRID, Button, ButtonRow, PanelCloseButton, Field, Label, Input, Textarea, Select, CoordGrid, CoordInput, Alert, Pill, Chip, TagRow, LoadingState, Stack, Inline, TitleRow, SplitLayout, IconText, PageShell, PageCard, PageTitle, CardTitle, SectionTitle, SectionDescription, MutedText, ItemCard, ItemList, List, PickerCard, PickerGrid, ChoiceCard, ChoiceGroup, GeneratingOverlay, EmptyState, VerticalToolbar, Toolbar, AvatarBadge, Checkbox, Switch, Slider, NumberField, Surface, Tooltip, Kbd, Divider, Draggable, DraggableSurface, resetDraggables, CanvasStage, OverlayIconButton, ImageThumb, Skeleton, LineSkeleton, TitleSkeleton, ButtonSkeleton, IconButtonSkeleton, InputSkeleton, SelectSkeleton, TextareaSkeleton, ChipSkeleton, ThumbSkeleton, AvatarSkeleton, CanvasSkeleton, PanelFormSkeleton, CardSkeleton, PickerGridSkeleton, ChoiceGroupSkeleton, PanelSection, PanelTitle, ThemeToggle, DashboardShell, Sidebar, SidebarBrand, activeSidebarHref, TopBar, TopBarLink, Page, PageHeader, PageBlock, Eyebrow, ContentCard, StatCard, FormGrid, FormActions, FilterTabs, ToggleChip, IconButton, RowCard, RowSkeleton, ListSkeleton, ListHeaderRow, SearchBar, LoadMoreButton, ButtonLink, NoteBar, StickyBar, DashboardShell, Sidebar, SidebarBrand, activeSidebarHref, TopBar, TopBarLink, Page, PageHeader, PageBlock, Eyebrow, ContentCard, StatCard, FormGrid, FormActions, FilterTabs, ToggleChip, IconButton, RowCard, RowSkeleton, ListSkeleton, ListHeaderRow, SearchBar, LoadMoreButton, ButtonLink, NoteBar, StickyBar, cn, belowPanel, usePanelRect), peer-dep wiring (zustand, react 18+), the live docs at objectifthunes.github.io/whiteboard-demo, or theming via `--wb-*` CSS tokens. Also fires on "match the whiteboard look", "floating panel canvas", "pan/zoom workspace UI", or any internal-tool / studio-canvas surface that should ship under the ObjectifThunes whiteboard system.
---

# Picking up `@objectifthunes/whiteboard` in a blink

You are helping the user wire up, design with, or extend the `@objectifthunes/whiteboard` npm package. Read this whole skill before answering — the library has opinionated defaults around the canvas / panel model and a few non-obvious caveats around SSR and theming.

## What it is

- **Package:** `@objectifthunes/whiteboard` (npm, public, current **`0.13.1`**).
- **Source repo:** `github.com/objectifthunes/whiteboard` (private).
- **Live docs:** **https://objectifthunes.github.io/whiteboard-demo/** — multi-page Next.js docs site with a live demo + copy-pasteable code block for every export. **Always link the user there for prop details rather than re-deriving them.** The demo source is at `github.com/objectifthunes/whiteboard-demo` (private, Next.js 16 + Turbopack + Tailwind 4, deploys to GH Pages on push to `main`).
- **Built with:** React 18+, Zustand 4+. No CSS-in-JS, no Tailwind — ships a single `style.css` (~28 KB raw / 6 KB gzip) plus 41 KB / 10 KB JS.
- **Voice:** light-default neutral surfaces, hairline borders, tiny font scale (`--wb-fs-*` starts at 0.64rem), gridded canvas with floating panels, soft `0 8px 24px` shadows. Dark theme via `[data-theme="dark"]`.
- **Shape:** **6 whiteboard primitives + 4 store/hooks + ~50 UI primitives + a 25-export dashboard kit = ~85 exports** (see `references/EXPORTS.md`).

## The mental model — read first

The library has one big idea and one small idea.

**Big idea — the canvas + panels.** `<WhiteboardShell>` is a fixed-position pan/zoom viewport. Drag empty space to pan, wheel to zoom, two-finger pan on trackpads. Inside it, every `<FloatingPanel>` is an absolutely-positioned, draggable, world-space rectangle: it registers its rect into a Zustand store, the shell draws all panels under one `transform`, and a `<Minimap>` shows them in miniature. Programmatic camera lives on the store: `fitToContent()`, `focusPanel(rect)`, `resetWidgets()`.

**Small idea — themed UI primitives.** Everything else is a thin styled-component wrapper around a native element. They exist so panel bodies look consistent without you naming a class. They are all driven by `--wb-*` CSS custom properties — change one token, retheme the whole surface.

If the user wants "a Figma-like canvas with widgets" or "a studio workspace" or "movable inspector panels", this library is the right tool. If they want a single static modal or a plain form, skip the canvas and just pull `Button`, `Field`, `Input`, `Alert` etc. as a tiny UI kit — that path works too.

## Three worked-example screens

| Screen | Shape | When to copy it |
|---|---|---|
| **Studio canvas** | `<WhiteboardShell>` with multiple `<FloatingPanel>`s laid out by `useWhiteboardLayout` | The user wants a multi-tool studio, inspector palette, or any surface where a user moves several editing panels around. Pattern: `useWhiteboardLayout({ widths, startX, y, gap })` → spread positions into panels. See `/components/canvas` and `/components/store` on the live docs. |
| **Admin dashboard** | `<DashboardShell sidebar={<Sidebar/>} topbar={<TopBar/>}><Page>...` | The user wants an admin/back-office: grouped sidebar nav, sticky top bar, list pages with skeletons, recap tiles. Docs section "Whiteboard Dashboard". See the 0.8 section below. |
| **Admin dashboard** | `<DashboardShell sidebar={<Sidebar/>} topbar={<TopBar/>}><Page>...` | The user wants an admin/back-office: grouped sidebar nav, sticky top bar, list pages with skeletons, recap tiles. Docs section "Whiteboard Dashboard". See the 0.8 section below. |
| **Inline panel kit** | `<PageShell>` → `<PageCard>` → `<Stack>` of `<Field>` / `<Button>` / `<Alert>` | The user wants a single auth card, settings page, or tiny tool UI. Pattern: `PageShell` centres a card; `PageCard` is the bordered surface; `Stack` for vertical rhythm. **No WhiteboardShell needed.** See `/components/layout`. |

## The five-step wire-up

When the user wants a new app on `@objectifthunes/whiteboard`, follow these steps in order. **Do not skip the `pnpm create next-app` step** — hand-rolling the scaffold always lags real Next conventions.

### 1. Scaffold

```bash
pnpm create next-app@latest <name> --ts --tailwind --eslint --app --src-dir --import-alias "@/*" --use-pnpm --turbopack --yes
cd <name>
```

Next 16 + React 19 + Tailwind v4 + Turbopack + App Router. Whiteboard is React-version-agnostic (peer `>=18`).

### 2. Install the library + peers

```bash
pnpm add @objectifthunes/whiteboard zustand
```

### 3. Import the stylesheet **once** at the root

```tsx
// app/layout.tsx
import '@objectifthunes/whiteboard/style.css'
import './globals.css'   // your overrides (optional)
```

Side-effecting CSS. Defines all `--wb-*` tokens on `:root` + a `[data-theme="dark"]` override.

### 4. Mark canvas pages as client components

`WhiteboardShell` uses `ResizeObserver`, `useEffect`, Zustand subscriptions and `getBoundingClientRect`. Anything that renders it must be a client component:

```tsx
'use client'
import { WhiteboardShell, FloatingPanel } from '@objectifthunes/whiteboard'
```

For static export (`output: 'export'`), gate first render behind a mounted flag so server-rendered HTML stays empty:

```tsx
const [mounted, setMounted] = useState(false)
useEffect(() => { setMounted(true) }, [])
if (!mounted) return null
```

### 5. Compose a canvas

```tsx
<WhiteboardShell extraActions={<ThemeToggle theme={theme} onToggle={toggle} />}>
  <FloatingPanel title="Settings" defaultPosition={{ x: 40, y: 40 }} focusable>
    {/* panel body */}
  </FloatingPanel>
</WhiteboardShell>
```

Non-canvas surfaces skip the shell entirely:

```tsx
<PageShell>
  <PageCard>
    <Stack>
      <Field label="Email"><Input type="email" /></Field>
      <Button>Sign in</Button>
    </Stack>
  </PageCard>
</PageShell>
```

## Export catalogue

See [`references/EXPORTS.md`](references/EXPORTS.md) for the copy-pasteable import block and per-export prop signatures. Quick map:

**Whiteboard primitives (8):** `WhiteboardShell` (0.12 adds `contained`: bounded within a positioned parent instead of fixed to the viewport — embed in a page section/card), `FloatingPanel`, `CreatePanel` (0.9 — dashed creation panel: full-width primary Button, place beside a board), `SectionPanel` + `sectionPanelWidth` (0.10 — draggable board of static grid cards; gaps/paddings = grid unit so sections span whole plus signs; 0.11 adds loading/loadingCards: blurred skeleton state), `Minimap`, `ZoomBar`, `ConfirmDialog`, `PanelErrorBoundary`.

**Store & hooks (4):** `useWhiteboardStore`, `useWhiteboardLayout`, `computeWhiteboardFit`, `computeWhiteboardRectFocus`. Helpers: `WHITEBOARD_GRID`, `snapToWhiteboardGrid`, `belowPanel`, `usePanelRect`, `cn`.

**Buttons:** `Button`, `ButtonRow`, `PanelCloseButton`, `ThemeToggle`, `OverlayIconButton`.

**Forms:** `Field`, `Label`, `Input`, `Textarea`, `Select`, `CoordGrid`, `CoordInput`, `Checkbox`, `Switch`, `Slider`, `NumberField`.

**Feedback:** `Alert`, `Pill`, `Chip`, `TagRow`, `LoadingState`, `GeneratingOverlay`, `EmptyState`, `Tooltip`.

**Layout:** `Stack`, `Inline`, `TitleRow`, `SplitLayout`, `IconText`, `PageShell`, `PageCard`, `Surface`, `Divider`, `Draggable`, `DraggableSurface` (+ `resetDraggables()`).

**Typography:** `PageTitle`, `CardTitle`, `SectionTitle`, `SectionDescription`, `MutedText`, `Kbd`.

**Lists / cards:** `ItemCard`, `ItemList`, `List`, `PickerCard`, `PickerGrid`, `ChoiceCard`, `ChoiceGroup`, `SelectList` (0.13 — bounded scrollable single/multi-select row list; one-line ellipsis rows, scales to hundreds; full-width hairline separators).

**Navigation:** `VerticalToolbar`, `Toolbar` (horizontal sibling, `position="top"|"bottom"|"static"`, `end` slot), `AvatarBadge`.

**Media:** `CanvasStage`, `ImageThumb`.

**Skeletons:** `Skeleton`, `LineSkeleton`, `TitleSkeleton`, `ButtonSkeleton`, `IconButtonSkeleton`, `InputSkeleton`, `SelectSkeleton`, `TextareaSkeleton`, `ChipSkeleton`, `ThumbSkeleton`, `AvatarSkeleton`, `CanvasSkeleton`, `PanelFormSkeleton`, `CardSkeleton`, `PickerGridSkeleton`, `ChoiceGroupSkeleton`.

**Panel-section helpers:** `PanelSection`, `PanelTitle`.

**Dashboard kit (0.8):** `DashboardShell`, `Sidebar`, `SidebarBrand`, `activeSidebarHref`, `TopBar`, `TopBarLink`, `Page`, `PageHeader`, `PageBlock`, `Eyebrow`, `ContentCard`, `StatCard`, `FormGrid`, `FormActions`, `FilterTabs`, `ToggleChip`, `IconButton`, `RowCard`, `RowSkeleton`, `ListSkeleton`, `ListHeaderRow`, `SearchBar`, `LoadMoreButton`, `ButtonLink`, `NoteBar`, `StickyBar`.

**Dashboard kit (0.8):** `DashboardShell`, `Sidebar`, `SidebarBrand`, `activeSidebarHref`, `TopBar`, `TopBarLink`, `Page`, `PageHeader`, `PageBlock`, `Eyebrow`, `ContentCard`, `StatCard`, `FormGrid`, `FormActions`, `FilterTabs`, `ToggleChip`, `IconButton`, `RowCard`, `RowSkeleton`, `ListSkeleton`, `ListHeaderRow`, `SearchBar`, `LoadMoreButton`, `ButtonLink`, `NoteBar`, `StickyBar`.

## New in 0.8 — the dashboard kit

Extracted from nocturne-admin (a production admin) into the SAME package. The live docs split into two sections: **"Whiteboard SaaS"** (the canvas/panel model) and **"Whiteboard Dashboard"** (this kit). When the user wants an admin/back-office/dashboard, reach for these instead of hand-rolling chrome:

- **Chrome:** `DashboardShell {sidebar, topbar?, children}` is a sidebar+main grid (collapses under 880px). `Sidebar {brand?, groups, activeHref?, footer?, renderLink?}` renders grouped nav; items are `{href, name, icon?, badge?}`. `SidebarBrand {mark, eyebrow?, name, meta?}`. `TopBar {crumb, actions?}` + `TopBarLink` (a quiet mono button). All links are plain `<a>` by default — **Next.js users pass `renderLink={(item, props) => <Link {...props} href={item.href} />}`**.
- **Active state:** `activeSidebarHref(groups, pathname)` picks the longest matching href, so `/covers/new` activates "New cover" and not also "Covers".
- **Pages:** `Page` (rhythm container, reserves bottom padding for StickyBar), `PageHeader {eyebrow, title, lede?}`, `PageBlock {title, children}`, `Eyebrow` (uppercase mono label).
- **Cards:** `ContentCard {title?}` (bordered surface), `StatCard {title, pill?, pillTone?, value, sub?, actions?}` — the recap/stat tile.
- **Lists:** `RowCard {title, detail?, leading?, actions?}` with `RowSkeleton`/`ListSkeleton` sharing its EXACT geometry (zero layout shift on swap), `ListHeaderRow`, `SearchBar` (always full-width; filters go on their own line), `LoadMoreButton {cursor, loading, onClick}` (renders nothing when no next page), `ButtonLink` (anchor with wb-btn styles; `as={Link}` for routers).
- **Forms & filters:** `FormGrid {cols?: 2|3}`, `FormActions`, `FilterTabs {options, value, onChange}` (active = primary Button), `ToggleChip {active}` (filled when active), `IconButton {icon, label}` (tooltip + aria-label).
- **Feedback:** `NoteBar {tone?, onDismiss}` — mount ONCE in the shell, feed from a store, clear on navigation. `StickyBar` — viewport-fixed bottom bar (batch/cart summaries); fixed on purpose because sticky cannot escape a small parent; offsets past the sidebar via `--wb-dash-sidebar-w`.
- **Tokens:** `--wb-font-mono`, `--wb-dash-sidebar-w` (232px), `--wb-dash-content-max` (760px) — override on `:root`.
- Layout gaps inside these pages: use the existing `Inline` (justify start/between/end) and `Stack`; the kit deliberately does NOT re-add Row/Col.

## New in 0.8 — the dashboard kit

Extracted from nocturne-admin (a production admin) into the SAME package. The live docs split into two sections: **"Whiteboard SaaS"** (the canvas/panel model) and **"Whiteboard Dashboard"** (this kit). When the user wants an admin/back-office/dashboard, reach for these instead of hand-rolling chrome:

- **Chrome:** `DashboardShell {sidebar, topbar?, children}` is a sidebar+main grid (collapses under 880px). `Sidebar {brand?, groups, activeHref?, footer?, renderLink?}` renders grouped nav; items are `{href, name, icon?, badge?}`. `SidebarBrand {mark, eyebrow?, name, meta?}`. `TopBar {crumb, actions?}` + `TopBarLink` (a quiet mono button). All links are plain `<a>` by default — **Next.js users pass `renderLink={(item, props) => <Link {...props} href={item.href} />}`**.
- **Active state:** `activeSidebarHref(groups, pathname)` picks the longest matching href, so `/covers/new` activates "New cover" and not also "Covers".
- **Pages:** `Page` (rhythm container, reserves bottom padding for StickyBar), `PageHeader {eyebrow, title, lede?}`, `PageBlock {title, children}`, `Eyebrow` (uppercase mono label).
- **Cards:** `ContentCard {title?}` (bordered surface), `StatCard {title, pill?, pillTone?, value, sub?, actions?}` — the recap/stat tile.
- **Lists:** `RowCard {title, detail?, leading?, actions?}` with `RowSkeleton`/`ListSkeleton` sharing its EXACT geometry (zero layout shift on swap), `ListHeaderRow`, `SearchBar` (always full-width; filters go on their own line), `LoadMoreButton {cursor, loading, onClick}` (renders nothing when no next page), `ButtonLink` (anchor with wb-btn styles; `as={Link}` for routers).
- **Forms & filters:** `FormGrid {cols?: 2|3}`, `FormActions`, `FilterTabs {options, value, onChange}` (active = primary Button), `ToggleChip {active}` (filled when active), `IconButton {icon, label}` (tooltip + aria-label).
- **Feedback:** `NoteBar {tone?, onDismiss}` — mount ONCE in the shell, feed from a store, clear on navigation. `StickyBar` — viewport-fixed bottom bar (batch/cart summaries); fixed on purpose because sticky cannot escape a small parent; offsets past the sidebar via `--wb-dash-sidebar-w`.
- **Tokens:** `--wb-font-mono`, `--wb-dash-sidebar-w` (232px), `--wb-dash-content-max` (760px) — override on `:root`.
- Layout gaps inside these pages: use the existing `Inline` (justify start/between/end) and `Stack`; the kit deliberately does NOT re-add Row/Col.

## New in 0.5 — draggable overlay chrome

`Draggable` (bare) and `DraggableSurface` (Surface + drag) give overlay chrome screen-space dragging **without a WhiteboardShell**. Key design: keep the caller's CSS anchoring (top/right/bottom/left, percentage centring) — the drag is a `translate()` delta on top, so layouts stay responsive and "reset" is just delta = 0. Behaviour: drags from any non-interactive area (inputs/buttons/canvases inside keep their pointer behaviour; opt out areas with `data-wb-nodrag`); snaps to `WHITEBOARD_GRID` on release and honours the global `whiteboard-snap-now` event; persists per `id` in localStorage (`persist={false}` to disable); double-click an empty area resets one element, `resetDraggables()` resets all. When an anchored wrapper uses its own transform (e.g. `-translate-x-1/2` centring), put the Draggable INSIDE the wrapper, not on it — the delta transform would override the centring.

 — settings-panel primitives & overlay chrome

Added after building a real app (a 3D floor-plan tool) on the kit:

- **`Checkbox` / `Switch`** — labeled boolean rows. The input pins `flex: none` + its own size, so host globals like `input { flex: 1 }` can't stretch it.
- **`Slider`** — label left, tabular-nums readout right (`display` overrides it), themed range below. `onChange` emits numbers.
- **`NumberField`** — `Field` + number `Input`; clamps to `[min, max]`, never emits `NaN`.
- **`Toolbar`** — horizontal `VerticalToolbar`. The app-chrome bar: icon button, title, `Chip`, `Divider orientation="vertical"`, primary action, `end` slot for a `ThemeToggle`.
- **`Surface`** — the plain floating overlay container (`padding="md"|"sm"|"none"`). **Use this, not `FloatingPanel`, for chrome floating over your own stage** (a 3D canvas, a map): selection menus, legends, log panes. `FloatingPanel` requires a `WhiteboardShell` and lives in world space; `Surface` is just the themed box.
- **`Tooltip`** — CSS-only (`:hover` + `:focus-within`), no portal; short labels over chrome only. Placements: top/bottom/left/right — use `right` on a left `VerticalToolbar`. **`Kbd`** for shortcut hints. **`Divider`** h/v hairline.

0.4 also hardened components against aggressive host CSS: `PickerCard` declares its own column layout (the global `button` rule can no longer collapse it into a clipped row), and native text/number inputs are styled like `select` out of the box.

## Theming

See [`references/TOKENS.md`](references/TOKENS.md) for the full token table.

Override tokens on `:root` or scope to `[data-theme="dark"]`. The package defines sensible defaults — only override what you want to deviate from.

```css
:root {
  --wb-bg: #f3f4f6;
  --wb-surface: #ffffff;
  --wb-text: #111827;
  --wb-text-muted: #6b7280;
  --wb-border: #e5e7eb;
  --wb-primary: #1f2937;
  --wb-danger: #dc2626;
}
```

`ThemeToggle` is intentionally uncontrolled of the DOM — *you* decide where the `data-theme` attribute lives (almost always `<html>`):

```tsx
const [theme, setTheme] = useState<'light' | 'dark'>('light')
useEffect(() => { document.documentElement.dataset.theme = theme }, [theme])
```

## Non-obvious caveats — read before debugging

1. **`WhiteboardShell` resets the store on unmount.** It calls `useWhiteboardStore.getState().resetSession()` in a cleanup effect. If you mount two shells in the same page (don't), the second one's unmount will nuke shared state.

2. **`useWhiteboardLayout` re-derives positions on `widths` object identity.** If you pass an inline literal (`widths={{ a: 100, b: 200 }}`) every render recomputes. Move the literal to module scope or wrap with `useMemo`.

3. **Panel auto-fit runs once.** The shell auto-`fitToContent`s on first panel registration after layout. After that, camera control is yours via `useWhiteboardStore`.

4. **`<FloatingPanel>` `defaultPosition` is captured once.** Changing the prop later won't reposition the panel. The reset button (`resetWidgets`) snaps back to the *original* default, not the current prop.

5. **The minimap subscribes to `registryVersion`, not `panels`.** The store mutates the `panels` Map in place and bumps `registryVersion`. If you write your own panel-rendering view, *subscribe to `registryVersion`* — selecting `panels` alone won't trigger re-renders.

6. **No SSR support without a mount gate.** The shell uses `ResizeObserver`, `getBoundingClientRect`, and `window.innerWidth` fallbacks. For Next.js static export, gate render behind a `mounted` flag (see step 4).

7. **Zustand v5 not tested.** Peer says `>=4`, but the package is built and tested against v4. v5 may work — if you see store issues, downgrade to v4 first.

8. **`SplitLayout` variants are shape-named.** Use `'media-content'`, `'single'`, or `'media-content-actions'`. The pre-0.3.0 names (`element`, `character`, `user`) are gone.

9. **`PickerGrid` takes `minItemWidth: number`.** It used to take `variant: 'elements' | 'characters' | 'library'` — that's gone. Pass the px width you want each cell to start at.

10. **`AssetTitle` and `StoryTitle` are gone.** Use `CardTitle` for AssetTitle's role; for StoryTitle, pick `PageTitle` or roll a custom heading.

11. **Composed widget skeletons are gone.** `StoryCardSkeleton`, `UserCardSkeleton`, `UserListSkeleton`, `AssetCardSkeleton` were removed. Use the new `<CardSkeleton withThumb chipCount={N} actionCount={M} />` to compose your own.

12. **Theming the `OverlayIconButton`.** It uses `color-mix` of `--wb-surface` and `--wb-surface-muted` for the chrome. If you want a stronger glassy effect, override `.overlay-icon-btn` in your own CSS.

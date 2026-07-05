# `@objectifthunes/whiteboard` — Exports (0.8.0)

Copy-pasteable import block, then per-export prop signatures.

> For a live demo of every export with the matching code block, see https://objectifthunes.github.io/whiteboard-demo/components/.

## One-shot import block

```ts
import {
  // Whiteboard primitives
  WhiteboardShell,
  FloatingPanel,
  Minimap,
  ZoomBar,
  ConfirmDialog,
  PanelErrorBoundary,

  // Store & geometry
  useWhiteboardStore,
  computeWhiteboardFit,
  computeWhiteboardRectFocus,

  // Hooks & utils
  useWhiteboardLayout,
  WHITEBOARD_GRID,
  snapToWhiteboardGrid,
  usePanelRect,
  belowPanel,
  cn,

  // Chrome & overlays (0.4)
  Toolbar,
  Surface,
  Draggable,
  DraggableSurface,
  resetDraggables,
  Tooltip,
  Kbd,
  Divider,

  // Buttons
  Button,
  ButtonRow,
  PanelCloseButton,
  ThemeToggle,
  ThemeProvider, useTheme,
  OverlayIconButton,

  // Forms
  Checkbox,
  Switch,
  Slider,
  NumberField,
  Field,
  Label,
  Input,
  Textarea,
  Select,
  CoordGrid,
  CoordInput,

  // Feedback
  Alert,
  Pill,
  Chip,
  TagRow,
  LoadingState,
  GeneratingOverlay,
  EmptyState,

  // Layout
  Stack,
  Inline,
  TitleRow,
  SplitLayout,
  IconText,
  PageShell,
  PageCard,

  // Typography
  PageTitle,
  CardTitle,
  SectionTitle,
  SectionDescription,
  MutedText,

  // Lists / cards
  ItemCard,
  ItemList,
  List,
  PickerCard,
  PickerGrid,
  ChoiceCard,
  ChoiceGroup,

  // Navigation
  VerticalToolbar,
  AvatarBadge,

  // Media
  CanvasStage,
  ImageThumb,

  // Panel sections
  PanelSection,
  PanelTitle,

  // Skeletons
  Skeleton,
  LineSkeleton,
  TitleSkeleton,
  ButtonSkeleton,
  IconButtonSkeleton,
  InputSkeleton,
  SelectSkeleton,
  TextareaSkeleton,
  ChipSkeleton,
  ThumbSkeleton,
  AvatarSkeleton,
  CanvasSkeleton,
  PanelFormSkeleton,
  CardSkeleton,
  PickerGridSkeleton,
  ChoiceGroupSkeleton,

  // Dashboard kit (0.8)
  DashboardShell,
  Sidebar,
  SidebarBrand,
  activeSidebarHref,
  TopBar,
  TopBarLink,
  Page,
  PageHeader,
  PageBlock,
  Eyebrow,
  ContentCard,
  StatCard,
  FormGrid,
  FormActions,
  FilterTabs,
  ToggleChip,
  IconButton,
  RowCard,
  RowSkeleton,
  ListSkeleton,
  ListHeaderRow,
  SearchBar,
  LoadMoreButton,
  ButtonLink,
  NoteBar,
  StickyBar,

  // Dashboard kit (0.8)
  DashboardShell,
  Sidebar,
  SidebarBrand,
  activeSidebarHref,
  TopBar,
  TopBarLink,
  Page,
  PageHeader,
  PageBlock,
  Eyebrow,
  ContentCard,
  StatCard,
  FormGrid,
  FormActions,
  FilterTabs,
  ToggleChip,
  IconButton,
  RowCard,
  RowSkeleton,
  ListSkeleton,
  ListHeaderRow,
  SearchBar,
  LoadMoreButton,
  ButtonLink,
  NoteBar,
  StickyBar,
} from '@objectifthunes/whiteboard'

import type { PanelRect, WhiteboardStore, ChoiceOption, SidebarGroup, SidebarItem } from '@objectifthunes/whiteboard'
```

## CSS

```ts
import '@objectifthunes/whiteboard/style.css'
```

## Whiteboard primitives

### `WhiteboardShell`

| Prop | Type | Default |
|---|---|---|
| `children` | `ReactNode` | — |
| `showMinimap` | `boolean` | `true` |
| `minimapLoading` | `boolean` | `false` |
| `extraActions` | `ReactNode` | — |

### `FloatingPanel`

| Prop | Type | Default |
|---|---|---|
| `title` | `ReactNode` | — |
| `defaultPosition` | `{ x: number; y: number }` | — |
| `width` | `number` | `300` |
| `className` | `string` | — |
| `trackRect` | `MutableRefObject<PanelRect>` | — |
| `focusable` | `boolean` | `false` |
| `focusPadding` | `number` | `40` |
| `focusMaxScale` | `number` | `1.5` |
| `headerActions` | `ReactNode` | — |
| `children` | `ReactNode` | — |

### `Minimap`

| Prop | Type | Default |
|---|---|---|
| `loading` | `boolean` | `false` |

### `ZoomBar`

| Prop | Type | Default |
|---|---|---|
| `extraActions` | `ReactNode` | — |

### `ConfirmDialog`

| Prop | Type | Default |
|---|---|---|
| `open` | `boolean` | — |
| `title` | `string` | — |
| `message` | `string` | — |
| `onConfirm` | `() => void` | — |
| `onCancel` | `() => void` | — |
| `confirmLabel` | `string` | `'Confirm'` |
| `loadingLabel` | `string` | `` `${confirmLabel}…` `` |
| `loading` | `boolean` | `false` |
| `error` | `string \| null` | — |

### `PanelErrorBoundary`

| Prop | Type | Default |
|---|---|---|
| `fallbackMessage` | `string` | `'This panel crashed.'` |

## Store & geometry

### `useWhiteboardStore`

```ts
interface WhiteboardStore {
  offset: { x: number; y: number }
  scale: number
  viewportSize: { width: number; height: number }
  snapToGrid: boolean
  snapGridSize: number
  panels: Map<string, PanelRect>
  resetFns: Map<string, () => void>
  registryVersion: number

  setOffset(v): void
  setScale(v): void
  setViewportSize(v): void
  setSnapToGrid(v): void

  register(id, rect): void
  unregister(id): void
  registerReset(id, fn): void
  unregisterReset(id): void

  fitToContent(): void
  focusPanel(rect, options?): void
  resetWidgets(): void
  resetSession(): void
}
```

Subscribe to `registryVersion` to re-render when panels change. Never subscribe to `panels` alone — the Map is mutated in place.

### `computeWhiteboardFit(panels, viewportSize, padding?)`
### `computeWhiteboardRectFocus(rect, viewportSize, padding?, maxScale?)`

### `useWhiteboardLayout({ widths, startX?, y?, gap? })`

Returns `{ layout, panelWidth, positions }`. Grid-snapped row layout for the initial panel positions.

### Helpers

- `WHITEBOARD_GRID` (= `20`), `snapToWhiteboardGrid(n)` — grid constant + round-to-grid.
- `usePanelRect(initial)` — `MutableRefObject<PanelRect>` you can pass as `trackRect`.
- `belowPanel(rect, gap?)` — `{ x: rect.x, y: rect.y + rect.height + gap }`. Default gap = `WHITEBOARD_GRID`.
- `cn(...args)` — joins truthy class names.

## Buttons

- **`Button`** — `variant?: 'primary' | 'secondary' | 'danger'`, `fullWidth?`, `iconOnly?`, `loading?`, `loadingText?`. Defaults to `type='button'`. Emits `wb-btn` classes.
- **`ButtonRow`** — equal-sized children via `flex: 1 1 0`. `as?: ElementType` (default `'div'`).
- **`PanelCloseButton`** — pre-built secondary button with `X` icon. Props: `onClick`, `label?` (default `'Close'`).
- **`ThemeToggle`** — `theme`, `onToggle`, `lightIcon?`, `darkIcon?`, `className?`. Controlled; pair with `ThemeProvider`/`useTheme` (or set `data-theme` yourself).
- **`ThemeProvider`** (0.7.0) — `defaultTheme?: 'light' | 'dark'`, owns theme state: persists to `localStorage('wb-theme')`, keeps `data-theme` on `<html>` and `<body>` in sync. SSR: server renders `defaultTheme`; pair with `data-theme` on `<body>` + `suppressHydrationWarning` on `<html>`.
- **`useTheme()`** (0.7.0) — `{ theme, setTheme, toggleTheme }` from the nearest `ThemeProvider`; throws outside one.
- **`OverlayIconButton`** — secondary icon-only button absolutely positioned over a canvas. `placement?: 'top-right' | 'bottom-left' | 'bottom-right'`. Stops pointer/wheel/context propagation by default.

## Forms

- **`Field`** — `label?`, `htmlFor?`, `hint?`, `error?`, `layout?: 'stack' | 'control'` (default `'stack'`), `as?`.
- **`Label`** — styled `<label>`. Uppercase, muted, tight letter-spacing.
- **`Input` / `Textarea` / `Select`** — thin styled wrappers, forward refs.
- **`CoordGrid`** — 2-column grid for `<CoordInput>`s.
- **`CoordInput`** — `axis: string` label + numeric input (`type='number'`, `step='0.01'`).

## Feedback

- **`Alert`** — `tone?: 'error' | 'muted' | 'info' | 'success'` (default `'info'`).
- **`Pill`** — `tone?: 'default' | 'success' | 'warning' | 'danger'`.
- **`Chip`** — `<span class="chip">`. Becomes interactive as `<button class="chip">`.
- **`TagRow`** — flex-wrap row of tiny items.
- **`LoadingState`** — `label?: string` (default `'Loading...'`) + spinner.
- **`GeneratingOverlay`** — `isGenerating`, `children`, `message?`. Backdrop-blur over children.
- **`EmptyState`** — `title`, `description?`, `action?`.

## Layout

- **`Stack`** — vertical grid, `size?: 'sm' | 'md'` (default `'md'`), `as?`.
- **`Inline`** — horizontal row, `justify?: 'start' | 'between' | 'end'` (default `'start'`), `as?`.
- **`TitleRow`** — horizontal row with `space-between`.
- **`SplitLayout`** — `variant: 'media-content' | 'single' | 'media-content-actions'`. Two- or three-column grid.
- **`IconText`** — `icon: ReactNode`, `as?` + children.
- **`PageShell`** — full-viewport centred `<main>`.
- **`PageCard`** — bordered card under PageShell, `max-width: 380px`.

## Typography

- **`PageTitle`** (`h1`, 1.1rem), **`SectionTitle`** (`span`), **`SectionDescription`** (`p`), **`CardTitle`** (`p`, with optional `clamp: boolean` for 2-line clamp), **`MutedText`** (`p`, with `size?: 'xs' | 'sm' | 'md'`, default `'sm'`).

## Lists / cards

- **`ItemCard`** — bordered card, `as?` (default `'div'`).
- **`ItemList`** — vertical list with consistent gap, `as?` (default `'div'`).
- **`List`** — `<ul>` with `list-reset` (default). `as?`, `reset?: boolean`.
- **`PickerCard`** — clickable card for picker grids. `as?` (default `'button'`).
- **`PickerGrid`** — `minItemWidth?: number` (default `120`). Responsive auto-fill grid.
- **`ChoiceCard`** — `active?: boolean`. Internal use by `ChoiceGroup`.
- **`ChoiceGroup<T extends string>`** — `options: ChoiceOption<T>[]`, `value`, `onChange(value)`.

## Navigation

- **`VerticalToolbar`** — `position?: 'left' | 'right' | 'static'` (default `'left'`), `bottom?: ReactNode`. Fixed sidebar.
- **`AvatarBadge`** — circular initials badge, `<span class="avatar-badge">`.

## Media

- **`CanvasStage`** — 16:9 bordered stage. `hint?: string` shows a corner caption.
- **`ImageThumb`** — `src?`, `alt`, `placeholder?` (default `'No image'`), `size?: 'sm' | 'md' | 'fluid'` (default `'md'`), `fit?: 'contain' | 'cover'` (default `'contain'`), `onImageError?`.

## Panel sections

- **`PanelSection`** — `heading?`, `description?`, `actions?`.
- **`PanelTitle`** — `icon: ComponentType<{size?, className?}>`, `label: string`. Compact title for panel headers.

## Skeletons

All take `HTMLAttributes<HTMLDivElement>`; render an animated shimmering bar/box.

- **Base:** `Skeleton` (with `radius?: 'sm' | 'md' | 'pill'`, `as?`)
- **Primitives:** `LineSkeleton` (with `short?`), `TitleSkeleton`, `ButtonSkeleton`, `IconButtonSkeleton`, `InputSkeleton`, `SelectSkeleton`, `TextareaSkeleton`, `ChipSkeleton`, `ThumbSkeleton`, `AvatarSkeleton`, `CanvasSkeleton`
- **Composed:**
  - `PanelFormSkeleton` — `inputs?: number` (default `1`), `showButton?: boolean` (default `true`)
  - `CardSkeleton` — `withThumb?: boolean` (default `true`), `chipCount?: number` (default `0`), `actionCount?: number` (default `0`)
  - `PickerGridSkeleton` — `count?: number` (default `8`), `minItemWidth?: number` (default `120`)
  - `ChoiceGroupSkeleton` — `count?: number` (default `4`), `withDescription?: boolean` (default `false`)

## Subpath exports

```jsonc
// package.json#exports
{
  ".": "./dist/index.js",
  "./style.css": "./dist/whiteboard.css"
}
```

Only one stylesheet, one entry point. No deep imports.

## New in 0.4.0

### `Checkbox` / `Switch`
```ts
interface CheckboxProps extends Omit<InputHTMLAttributes<HTMLInputElement>, 'type'> {
  label?: ReactNode   // sentence-case row text
  hint?: ReactNode    // muted inline addition
}
```
Full-width labeled rows. `Switch` renders a sliding track (`role="switch"`), same props.

### `Slider`
```ts
interface SliderProps extends Omit<InputHTMLAttributes<HTMLInputElement>, 'type' | 'value' | 'onChange'> {
  label?: ReactNode
  display?: ReactNode          // right-aligned readout; defaults to value
  value: number
  onChange: (value: number) => void
}
```

### `NumberField`
```ts
interface NumberFieldProps extends Omit<InputHTMLAttributes<HTMLInputElement>, 'type' | 'value' | 'onChange'> {
  label?: ReactNode
  hint?: ReactNode
  value: number
  onChange: (value: number) => void   // realtime, clamped to [min, max]; empty/0/NaN never emitted — previous value kept, restored on blur (0.7.0)
}
```

### `Toolbar`
```ts
interface ToolbarProps extends HTMLAttributes<HTMLElement> {
  position?: 'top' | 'bottom' | 'static'   // top = fixed + centered (default)
  end?: ReactNode                          // trailing actions after a wider gap
}
```

### `Surface`
```ts
interface SurfaceProps extends HTMLAttributes<HTMLElement> {
  as?: ElementType
  padding?: 'none' | 'sm' | 'md'
}
```
The plain floating overlay box (`--wb-surface` + border + radius-lg + soft shadow). For overlay chrome outside a `WhiteboardShell`.

### `Tooltip`
```ts
interface TooltipProps extends HTMLAttributes<HTMLSpanElement> {
  label: ReactNode
  placement?: 'top' | 'bottom' | 'left' | 'right'
  children: ReactNode
}
```
CSS-only; shows on hover and keyboard focus. No portal.

### `Kbd`
`HTMLAttributes<HTMLElement>` — monospace key chip: `<Kbd>esc</Kbd>`.

### `Divider`
```ts
interface DividerProps extends HTMLAttributes<HTMLElement> {
  orientation?: 'horizontal' | 'vertical'
}
```

## New in 0.5.0

### `Draggable` / `DraggableSurface`
```ts
interface DraggableProps extends HTMLAttributes<HTMLDivElement> {
  id: string          // keys persistence + the reset registry
  snap?: boolean      // grid-snap on release (default true)
  persist?: boolean   // localStorage per id (default true)
  disabled?: boolean
}
// DraggableSurface adds: padding?: 'none' | 'sm' | 'md'
```
Screen-space drag as a translate() delta over the caller's own CSS anchoring. `resetDraggables(): void` resets every mounted instance.

## Dashboard kit (0.8.0)

Docs live under the "Whiteboard Dashboard" section of the demo site; the canvas model is under "Whiteboard SaaS". Same package, no extra install.

### `DashboardShell`
```ts
{ sidebar: ReactNode; topbar?: ReactNode; children: ReactNode }
// sidebar+main grid, min-height 100vh, single column under 880px
```

### `Sidebar` / `SidebarBrand` / `activeSidebarHref`
```ts
interface SidebarItem { href: string; name: string; icon?: ReactNode; badge?: ReactNode }
interface SidebarGroup { label: string; items: SidebarItem[] }
// Sidebar
{ brand?: ReactNode; groups: SidebarGroup[]; activeHref?: string; footer?: ReactNode;
  renderLink?: (item, props: { className: string; children: ReactNode }) => ReactNode }
// default renderLink is a plain <a>; Next users: (item, props) => <Link {...props} href={item.href} />
// SidebarBrand
{ href?: string; mark: ReactNode; eyebrow?: string; name: string; meta?: string; renderLink? }
// activeSidebarHref(groups, pathname): string | undefined — longest matching href wins
```

### `TopBar` / `TopBarLink`
```ts
// TopBar
{ crumb: ReactNode; actions?: ReactNode }   // sticky, blurred backdrop
// TopBarLink — ButtonHTMLAttributes; quiet mono text button
```

### `Page` / `PageHeader` / `PageBlock` / `Eyebrow`
```ts
Page      { children }                          // grid gap 2rem + bottom padding for StickyBar
PageHeader{ eyebrow: string; title: string; lede?: string }
PageBlock { title: string; children }
Eyebrow   { icon?: ReactNode; children }        // uppercase mono label
```

### `ContentCard` / `StatCard`
```ts
ContentCard { title?: ReactNode; children; style? }   // bordered surface, grid gap
StatCard {
  title: ReactNode; pill?: ReactNode; pillTone?: 'success'|'warning'|'danger'|'default'
  value: ReactNode; sub?: ReactNode; children?: ReactNode; actions?: ReactNode
}
```

### `FormGrid` / `FormActions` / `FilterTabs` / `ToggleChip` / `IconButton`
```ts
FormGrid    { cols?: 2 | 3; children }          // 1 column under 880px
FormActions { children }                        // right-aligned action row
FilterTabs<T extends string> { options: {value:T; label:string}[]; value: T; onChange(v:T) }
ToggleChip  { active: boolean; onClick(); children }
IconButton  { icon: ReactNode; label: string; variant?; onClick?; disabled?; loading? }
```

### `RowCard` / `RowSkeleton` / `ListSkeleton` / `ListHeaderRow`
```ts
RowCard      { title; detail?; leading?; actions? }
RowSkeleton  { withDetail?: boolean; actionCount?: number }  // EXACT RowCard geometry
ListSkeleton { rows?: number } & RowSkeletonProps
ListHeaderRow{ title; actions? }
```

### `SearchBar` / `LoadMoreButton` / `ButtonLink`
```ts
SearchBar      { value: string; onChange(v: string); label?; placeholder? }  // full-width
LoadMoreButton { cursor: string | null | undefined; loading: boolean; onClick(); label? } // null cursor -> renders nothing
ButtonLink     { as?: ElementType; variant?: 'primary'|'secondary'|'danger' } & AnchorHTMLAttributes
```

### `NoteBar` / `StickyBar`
```ts
NoteBar   { tone?: 'info'|'success'|'error'|'muted'; onDismiss(); children }  // mount once, clear on navigation
StickyBar { children }  // position: fixed bottom; left offset = --wb-dash-sidebar-w
```

### Dashboard tokens
```css
:root {
  --wb-font-mono: ui-monospace, ...;
  --wb-dash-sidebar-w: 232px;
  --wb-dash-content-max: 760px;
}
```

## Dashboard kit (0.8.0)

Docs live under the "Whiteboard Dashboard" section of the demo site; the canvas model is under "Whiteboard SaaS". Same package, no extra install.

### `DashboardShell`
```ts
{ sidebar: ReactNode; topbar?: ReactNode; children: ReactNode }
// sidebar+main grid, min-height 100vh, single column under 880px
```

### `Sidebar` / `SidebarBrand` / `activeSidebarHref`
```ts
interface SidebarItem { href: string; name: string; icon?: ReactNode; badge?: ReactNode }
interface SidebarGroup { label: string; items: SidebarItem[] }
// Sidebar
{ brand?: ReactNode; groups: SidebarGroup[]; activeHref?: string; footer?: ReactNode;
  renderLink?: (item, props: { className: string; children: ReactNode }) => ReactNode }
// default renderLink is a plain <a>; Next users: (item, props) => <Link {...props} href={item.href} />
// SidebarBrand
{ href?: string; mark: ReactNode; eyebrow?: string; name: string; meta?: string; renderLink? }
// activeSidebarHref(groups, pathname): string | undefined — longest matching href wins
```

### `TopBar` / `TopBarLink`
```ts
// TopBar
{ crumb: ReactNode; actions?: ReactNode }   // sticky, blurred backdrop
// TopBarLink — ButtonHTMLAttributes; quiet mono text button
```

### `Page` / `PageHeader` / `PageBlock` / `Eyebrow`
```ts
Page      { children }                          // grid gap 2rem + bottom padding for StickyBar
PageHeader{ eyebrow: string; title: string; lede?: string }
PageBlock { title: string; children }
Eyebrow   { icon?: ReactNode; children }        // uppercase mono label
```

### `ContentCard` / `StatCard`
```ts
ContentCard { title?: ReactNode; children; style? }   // bordered surface, grid gap
StatCard {
  title: ReactNode; pill?: ReactNode; pillTone?: 'success'|'warning'|'danger'|'default'
  value: ReactNode; sub?: ReactNode; children?: ReactNode; actions?: ReactNode
}
```

### `FormGrid` / `FormActions` / `FilterTabs` / `ToggleChip` / `IconButton`
```ts
FormGrid    { cols?: 2 | 3; children }          // 1 column under 880px
FormActions { children }                        // right-aligned action row
FilterTabs<T extends string> { options: {value:T; label:string}[]; value: T; onChange(v:T) }
ToggleChip  { active: boolean; onClick(); children }
IconButton  { icon: ReactNode; label: string; variant?; onClick?; disabled?; loading? }
```

### `RowCard` / `RowSkeleton` / `ListSkeleton` / `ListHeaderRow`
```ts
RowCard      { title; detail?; leading?; actions? }
RowSkeleton  { withDetail?: boolean; actionCount?: number }  // EXACT RowCard geometry
ListSkeleton { rows?: number } & RowSkeletonProps
ListHeaderRow{ title; actions? }
```

### `SearchBar` / `LoadMoreButton` / `ButtonLink`
```ts
SearchBar      { value: string; onChange(v: string); label?; placeholder? }  // full-width
LoadMoreButton { cursor: string | null | undefined; loading: boolean; onClick(); label? } // null cursor -> renders nothing
ButtonLink     { as?: ElementType; variant?: 'primary'|'secondary'|'danger' } & AnchorHTMLAttributes
```

### `NoteBar` / `StickyBar`
```ts
NoteBar   { tone?: 'info'|'success'|'error'|'muted'; onDismiss(); children }  // mount once, clear on navigation
StickyBar { children }  // position: fixed bottom; left offset = --wb-dash-sidebar-w
```

### Dashboard tokens
```css
:root {
  --wb-font-mono: ui-monospace, ...;
  --wb-dash-sidebar-w: 232px;
  --wb-dash-content-max: 760px;
}
```

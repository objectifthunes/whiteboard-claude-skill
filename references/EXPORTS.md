# `@objectifthunes/whiteboard` — Exports

Full copy-pasteable import block, then per-export prop signatures.

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

  // Buttons
  Button,
  ButtonRow,
  PanelCloseButton,
  ThemeToggle,
  OverlayIconButton,

  // Forms
  Field,
  Label,
  Input,
  Textarea,
  Select,
  CoordGrid,
  CoordInput,

  // Status / alerts
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
  StoryTitle,
  AssetTitle,
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
  StoryCardSkeleton,
  UserCardSkeleton,
  UserListSkeleton,
  AssetCardSkeleton,
  PickerGridSkeleton,
  ChoiceGroupSkeleton,
} from '@objectifthunes/whiteboard'

import type { PanelRect, WhiteboardStore, ChoiceOption } from '@objectifthunes/whiteboard'
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

Renders the fixed-position pan/zoom canvas. Children are absolute-positioned in world space (typically `<FloatingPanel>`s). Resets store state on unmount.

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

Draggable header, body for content, double-click to focus (when `focusable`), tap-tap-to-focus on touch.

### `Minimap`

| Prop | Type | Default |
|---|---|---|
| `loading` | `boolean` | `false` |

Subscribes to `registryVersion` from the store. Click/drag to pan, wheel to zoom, double-click a panel to focus it.

### `ZoomBar`

| Prop | Type | Default |
|---|---|---|
| `extraActions` | `ReactNode` | — |

Zoom in/out, fit, reset positions, snap-to-grid toggle. Rendered by `WhiteboardShell`, exposed for custom shells.

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

Portaled. Closes on Escape, backdrop click, or Cancel.

### `PanelErrorBoundary`

| Prop | Type | Default |
|---|---|---|
| `fallbackMessage` | `string` | `'This panel crashed.'` |

Class-based error boundary with Retry.

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

Returns `{ scale, offset } | null` that frames all panels. `padding` defaults to `60`.

### `computeWhiteboardRectFocus(rect, viewportSize, padding?, maxScale?)`

Returns `{ scale, offset }` framing a single rect. Defaults: `padding=40`, `maxScale=1.5`.

### `useWhiteboardLayout({ widths, startX?, y?, gap? })`

Returns `{ layout, panelWidth, positions }`. Grid-snapped row layout for the initial panel positions.

### `WHITEBOARD_GRID` / `snapToWhiteboardGrid(n)`

Grid constant is `20`. The snap helper rounds.

### `usePanelRect(initial)` / `belowPanel(rect, gap?)`

`usePanelRect` returns a `MutableRefObject<PanelRect>` you can pass as `trackRect`. `belowPanel(rect)` returns `{ x: rect.x, y: rect.y + rect.height + gap }` (default gap `WHITEBOARD_GRID`).

### `cn(...args)`

`(string | false | null | undefined)[] → string` — joins truthy class names.

## Buttons

- **`Button`** — `variant?: 'primary' | 'secondary' | 'danger'`, `fullWidth?`, `iconOnly?`, `loading?`, `loadingText?`. Defaults to `type='button'`. Emits `wb-btn` classes.
- **`ButtonRow`** — equal-sized children via `flex: 1 1 0`. `as?: ElementType` (default `'div'`).
- **`PanelCloseButton`** — pre-built secondary button with `X` icon. Props: `onClick`, `label?` (default `'Close'`).
- **`ThemeToggle`** — `theme`, `onToggle`, `lightIcon?`, `darkIcon?`, `className?`. Controlled; you set `data-theme` yourself.
- **`OverlayIconButton`** — secondary icon-only button absolutely positioned over a canvas. `placement?: 'top-right' | 'bottom-left' | 'bottom-right'`. Stops pointer/wheel/context propagation by default.

## Forms

- **`Field`** — `label?`, `htmlFor?`, `hint?`, `error?`, `layout?: 'stack' | 'control'` (default `'stack'`), `as?`. Renders Label + children + hint/error in a column.
- **`Label`** — styled `<label>`. Uppercase, muted, tight letter-spacing.
- **`Input` / `Textarea` / `Select`** — thin styled wrappers, forward refs.
- **`CoordGrid`** — 2-column grid for `<CoordInput>`s.
- **`CoordInput`** — `axis: string` label + numeric input (`type='number'`, `step='0.01'`).

## Status / alerts

- **`Alert`** — `tone?: 'error' | 'muted' | 'info' | 'success'` (default `'info'`).
- **`Pill`** — `tone?: 'default' | 'success' | 'warning' | 'danger'`.
- **`Chip`** — `<span class="chip">`. Becomes interactive when rendered as `<button class="chip">`.
- **`TagRow`** — `<div class="tag-row">`, flex-wrap row of tiny items.
- **`LoadingState`** — `label?: string` (default `'Loading...'`) + spinner.
- **`GeneratingOverlay`** — `isGenerating`, `children`, `message?`. Backdrop-blur overlay over children when active.
- **`EmptyState`** — `title`, `description?`, `action?`.

## Layout

- **`Stack`** — vertical grid, `size?: 'sm' | 'md'` (default `'md'`), `as?`.
- **`Inline`** — horizontal row, `justify?: 'start' | 'between' | 'end'` (default `'start'`), `as?`.
- **`TitleRow`** — horizontal row with `justify-content: space-between`.
- **`SplitLayout`** — `variant: 'element' | 'character' | 'user'`. Two/three-column grid.
- **`IconText`** — `icon: ReactNode`, `as?` + children. Inline icon + text row.
- **`PageShell`** — full-viewport centred `<main>`.
- **`PageCard`** — bordered card under PageShell, `max-width: 380px`.

## Typography

- **`PageTitle`** (`h1`, 1.1rem), **`StoryTitle`** (`h3`, 0.88rem), **`AssetTitle`** (`p`, with optional `clamp: boolean` for 2-line clamp), **`SectionTitle`** (`span`), **`SectionDescription`** (`p`), **`MutedText`** (`p`, with `size?: 'xs' | 'sm' | 'md'`, default `'sm'`).

## Lists / cards

- **`ItemCard`** — bordered card, `as?` (default `'div'`).
- **`ItemList`** — vertical list with consistent gap, `as?` (default `'div'`).
- **`List`** — `<ul>` with `list-reset` (default). `as?`, `reset?: boolean`.
- **`PickerCard`** — clickable card for picker grids. `as?` (default `'button'`).
- **`PickerGrid`** — `variant: 'elements' | 'characters' | 'library'`. Responsive grid.
- **`ChoiceCard`** — `active?: boolean`. Internal use by `ChoiceGroup`.
- **`ChoiceGroup<T extends string>`** — `options: ChoiceOption<T>[]`, `value`, `onChange(value)`.

## Navigation

- **`VerticalToolbar`** — `position?: 'left' | 'right' | 'static'` (default `'left'`), `bottom?: ReactNode`. Fixed sidebar.
- **`AvatarBadge`** — circular initials badge, `<span class="avatar-badge">`.

## Media

- **`CanvasStage`** — 16:9 bordered stage. `hint?: string` shows a corner caption.
- **`ImageThumb`** — `src?`, `alt`, `placeholder?` (default `'No image'`), `size?: 'sm' | 'md' | 'fluid'` (default `'md'`), `fit?: 'contain' | 'cover'` (default `'contain'`), `onImageError?`.

## Panel sections

- **`PanelSection`** — `heading?`, `description?`, `actions?`. Bordered section with optional header row.
- **`PanelTitle`** — `icon: ComponentType<{size?, className?}>`, `label: string`. Compact title for panel headers.

## Skeletons

All take `HTMLAttributes<HTMLDivElement>`; render an animated shimmering bar/box.

- **Base:** `Skeleton` (with `radius?: 'sm' | 'md' | 'pill'`, `as?`)
- **Primitives:** `LineSkeleton` (with `short?`), `TitleSkeleton`, `ButtonSkeleton`, `IconButtonSkeleton`, `InputSkeleton`, `SelectSkeleton`, `TextareaSkeleton`, `ChipSkeleton`, `ThumbSkeleton`, `AvatarSkeleton`, `CanvasSkeleton`
- **Widgets:** `PanelFormSkeleton` (props: `inputs?`, `showButton?`), `StoryCardSkeleton`, `UserCardSkeleton`, `UserListSkeleton` (with `count?`), `AssetCardSkeleton`, `PickerGridSkeleton` (props: `count?`, `gridClass: string`), `ChoiceGroupSkeleton` (props: `count?`, `withDescription?`)

## Subpath exports

```jsonc
// package.json#exports
{
  ".": "./dist/index.js",
  "./style.css": "./dist/whiteboard.css"
}
```

Only one stylesheet, one entry point. No deep imports.

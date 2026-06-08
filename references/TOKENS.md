# `@objectifthunes/whiteboard` — Design Tokens

All tokens are CSS custom properties prefixed `--wb-*`. Override on `:root` (light defaults) or scope to `[data-theme="dark"]`.

## Typography

| Token | Light default | Notes |
|---|---|---|
| `--wb-font-body` | `'Segoe UI', sans-serif` | Body family. Override to add your brand stack. |
| `--wb-fs-2xs` | `0.64rem` | Chips / pills. |
| `--wb-fs-xs` | `0.68rem` | Hint text, captions. |
| `--wb-fs-sm` | `0.72rem` | Body small, muted text. |
| `--wb-fs-md` | `0.76rem` | Body default, button labels. |
| `--wb-fs-base` | `0.78rem` | Base inputs. |
| `--wb-fs-lg` | `0.84rem` | Confirm-modal title. |

The scale is intentionally tiny — the library targets dense studio UI. If you want a marketing feel, override these upward.

## Color — surfaces

| Token | Light | Dark |
|---|---|---|
| `--wb-bg` | `#f3f4f6` | `#0f1117` |
| `--wb-bg-hover` | `#f3f4f6` | `#161922` |
| `--wb-surface` | `#ffffff` | `#1a1d27` |
| `--wb-surface-soft` | `#ffffff` | `#1e2130` |
| `--wb-surface-hover` | `#f9fafb` | `#252836` |
| `--wb-surface-active` | `#f8fafc` | `#2a2d3a` |
| `--wb-surface-muted` | `#f1f5f9` | `#1e2130` |
| `--wb-white` | `#ffffff` | `#1a1d27` (yes, "white" remaps in dark) |

## Color — borders / focus

| Token | Light | Dark |
|---|---|---|
| `--wb-border` | `#e5e7eb` | `#2d3041` |
| `--wb-border-active` | `#cbd5e1` | `#3d4155` |
| `--wb-focus-ring` | `#93c5fd` | `#3b82f6` |
| `--wb-focus-border` | `#60a5fa` | `#2563eb` |
| `--wb-focus-shadow` | `rgba(147,197,253,.38)` | `rgba(59,130,246,.38)` |

## Color — text

| Token | Light | Dark |
|---|---|---|
| `--wb-text` | `#111827` | `#e5e7eb` |
| `--wb-text-primary` | `#1f2937` | `#f3f4f6` |
| `--wb-text-secondary` | `#374151` | `#d1d5db` |
| `--wb-text-tertiary` | `#4b5563` | `#9ca3b0` |
| `--wb-text-muted` | `#6b7280` | `#6b7280` |

## Color — actions

| Token | Light | Dark |
|---|---|---|
| `--wb-primary` | `#1f2937` | `#e5e7eb` |
| `--wb-primary-pressed` | `#111827` | `#f3f4f6` |
| `--wb-danger` | `#dc2626` | `#ef4444` |
| `--wb-danger-pressed` | `#b91c1c` | `#dc2626` |
| `--wb-btn-text` | `#ffffff` | `var(--wb-bg)` |

In light mode `--wb-primary` is near-black; primary buttons read as inverted on light surface. In dark mode `--wb-primary` is near-white; the same buttons invert again. The pattern is "primary = the colour opposite to your background".

## Radii

| Token | Value | Used by |
|---|---|---|
| `--wb-radius-lg` | `12px` | Floating panels, page card, large surfaces. |
| `--wb-radius-md` | `10px` | Buttons, sections, picker cards. |
| `--wb-radius-sm` | `8px` | Inputs, chips, small surfaces. |
| `--wb-radius-pill` | `999px` | Pills, avatar badges, overlay buttons. |

## Gap scale

| Token | Value | Used by |
|---|---|---|
| `--wb-gap-xs` | `0.2rem` | Tightest rows. |
| `--wb-gap-sm` | `0.28rem` | Default toolbar gaps. |
| `--wb-gap-md` | `0.32rem` | Title rows. |
| `--wb-gap-lg` | `0.44rem` | Item lists, sections. |
| `--wb-gap-xl` | `0.52rem` | Loose stacks. |

## Effects

| Token | Value |
|---|---|
| `--wb-shadow-soft` | `0 8px 24px rgba(15,23,42,.08)` (light) / `0 8px 24px rgba(0,0,0,.3)` (dark) |
| `--wb-transition-fast` | `120ms ease` |

## Theming recipes

### Tighter / denser

Already very dense. To shrink even more, override `--wb-fs-*` and tighten gaps:

```css
:root {
  --wb-fs-base: 0.72rem;
  --wb-gap-lg: 0.36rem;
}
```

### Looser / "marketing" feel

```css
:root {
  --wb-fs-base: 0.9rem;
  --wb-fs-md: 0.86rem;
  --wb-fs-lg: 1rem;
  --wb-radius-lg: 16px;
  --wb-shadow-soft: 0 12px 32px rgba(15, 23, 42, 0.12);
}
```

### Brand accent (e.g. blue primary)

```css
:root {
  --wb-primary: #2563eb;
  --wb-primary-pressed: #1d4ed8;
  --wb-btn-text: #ffffff;
}

[data-theme='dark'] {
  --wb-primary: #60a5fa;
  --wb-primary-pressed: #3b82f6;
  --wb-btn-text: #0f1117;
}
```

### Replace canvas grid pattern

`whiteboard.css` embeds the canvas grid as an inline SVG `background-image` on `.whiteboard-grid`. To customise:

```css
.whiteboard-grid {
  background-image: url("data:image/svg+xml,%3Csvg ...%3E");
  background-size: 32px 32px;
}
```

## Don'ts

- **Don't compose `--wb-*` with Tailwind colour tokens.** They share no scale. Mixing makes the surface inconsistent and dark mode breaks. Pick one system per surface.
- **Don't override the same token for *some* components only.** If you redefine `--wb-primary`, every button, chip, focus ring and pill inherits the change. That's the point — own it.
- **Don't rely on token names from old README drafts.** `--wb-fg`, `--wb-muted`, and `--wb-accent` were documented in pre-0.2.7 READMEs but never existed. Use the names in this file.

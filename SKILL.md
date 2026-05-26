---
name: design-system-base
description: Vedant's personal design system — clean, minimal, neutral palette with pill-shaped buttons and Geist typography. Use this skill ANY time the user asks to build, style, or polish UI in a Next.js + Tailwind v4 project — including new components, full pages, landing sections, forms, cards, buttons, or "make this look better." Also use when the user says "use my design system", "match my style", "in my system", "install my design system", or references tokens like ds-neutral, ease-ds, hfine. Provides token scales (color/spacing/type), motion conventions, and ready-to-copy primitive components (buttons, card, text-input, avatar).
---

# Design System Base

A complete, opinionated design system. Use it as a reference when building or styling any UI — every value here is intentional, so don't deviate without reason.

## Aesthetic in one paragraph

Clean and minimal. Lots of whitespace. Neutral grayscale palette punctuated by pure, saturated semantic colors (true `#FF0000` red, not muted). Pill-shaped everything. Geist Medium 500 across the board — hierarchy comes from color, not weight or size. Subtle shadows, hairline borders at low opacity, smooth `cubic-bezier(0.23, 1, 0.32, 1)` motion. No generic shadcn defaults — polished and intentional.

---

## Two modes of use

### Mode A — Installing into a new/empty project

When the user says "install my design system", "set up the base", "give me my system":

1. **Check prerequisites:**
   - Next.js 15+ with App Router (`package.json`)
   - Tailwind CSS v4 (look for `@import "tailwindcss"`, not `tailwind.config.js`)
   - shadcn/ui initialized with `base-nova` style (`components.json`)
   - `cn` utility at `@/lib/utils`
   - Geist + Geist_Mono wired via `next/font/google` in `app/layout.tsx`, exposing `--font-geist-sans` and `--font-geist-mono`

   If anything is missing, stop and tell the user what to run (`npx create-next-app@latest`, `npx shadcn@latest init`, etc.). Don't silently fix multiple things.

2. **Drop in `globals.css`** at `src/app/globals.css`. If a file exists, merge — never overwrite user customizations.

3. **Drop in `components/*.tsx`** at `src/components/design-system/`.

4. **Drop in `lib/utils.ts`** only if `@/lib/utils` doesn't exist.

5. Tell the user which files landed and remind them: other components (modal, tag, table, etc.) aren't bundled — ask and they'll be built on demand from the conventions below.

### Mode B — Building or styling components mid-project

When the user asks for a new component, a page section, or "make this match my system":

- **Read the token tables below** and pick from them. Never invent a value.
- **Reuse the primitives in `components/`** — don't recreate Primary/Secondary/Ghost buttons or Card from scratch.
- **Match the patterns of existing components in the user's repo** — open one, mirror its structure (props, `cn(...)`, `"use client"` placement, class ordering).
- **Always ship a dark mode variant.**

---

## Tokens (memorize before generating any UI)

### Spacing — exactly 5 tokens, ever

| Token | px  | When to use                                          |
|-------|-----|------------------------------------------------------|
| xs    | 4   | Label → its content (h6 → body)                       |
| sm    | 8   | Heading → content (h4+); between adjacent buttons     |
| md    | 12  | Related fields inside the same group                  |
| lg    | 20  | Subsections inside a component; standard card padding |
| xl    | 100 | Major page-level section breaks                       |

`gap-6`, `p-6`, `mb-16`, etc. are **forbidden** — always map the relationship to one of these five.

### Type — 6 tokens, all 500 weight

| Token   | Size | LH   | Letter spacing | Default color    |
|---------|------|------|----------------|------------------|
| Display | 30px | 32px | -0.6px         | ds-neutral-1000  |
| H1      | 24px | 30px | —              | ds-neutral-1000  |
| H2      | 14px | 18px | +0.4px         | ds-neutral-1000  |
| Body    | 14px | 20px | +0.4px         | ds-neutral-1000  |
| Small   | 14px | 18px | +0.6px         | ds-neutral-450 (#999999) |
| Buttons | 16px | 20px | —              | varies           |

Hierarchy comes from **color**, not weight or size. Everything is `font-medium` (500). Use H2 for labels.

### Color rules

- **Neutrals** — use the 14-step `--ds-neutral-*` scale. Never reach for Tailwind `gray-*` / `zinc-*` / `slate-*`.
- **Semantics are pure and saturated:**
  - Destructive: `#FF0000` (not `#DC2626`, not `#EF4444`)
  - Success: `#00CC44`
  - Warning: `#FF9900`
  - Info: `#2979FF`
- **Borders:** `border-black/8` standard, `border-black/16` on vivid backgrounds or on cards. Dark mode swaps to `border-white/8` and `border-white/16`.
- **Ghost = no chrome.** Ghost buttons / links have zero border and zero background — text color change is the only feedback. Never add a fill or border to anything labeled "ghost."

### Shape

- Every button: `rounded-full`, `px-4 py-2`, `min-w-[96px]`, Geist Medium 16/20.
- Card: `rounded-[20px]`, `p-4`, `border border-black/16`, shadow `0 2px 8px rgba(0,0,0,0.06)`.
- Inputs: `rounded-full` (yes, pill inputs), `border-[1.5px] border-black/12`, `px-4 py-3`.

### Motion

- Default transition: `duration-150 ease-ds`. The `ease-ds` utility is `cubic-bezier(0.23, 1, 0.32, 1)` — defined in `globals.css`.
- Active press: `active:scale-[0.96]` on every clickable primitive.
- Hover: gate behind `hfine:hover:` (the `hfine` variant means fine pointer + hover-capable) so touch devices don't get sticky hover.
- Entrance animations for small marks (check, radio dot): scale `0.25 → 1`, opacity `0 → 1`, blur `4px → 0`, all 150ms `ease-ds`.

---

## Component library

The `components/` directory contains the canonical primitives. **Read the file before generating any variant** — don't rewrite from memory.

### Buttons
| Component         | File                                | Notes |
|-------------------|-------------------------------------|-------|
| PrimaryButton     | `components/primary-button.tsx`     | Black bg, white text |
| SecondaryButton   | `components/secondary-button.tsx`   | White bg, black border |
| GhostButton       | `components/ghost-button.tsx`       | No chrome, text color only |
| DestructiveButton | `components/destructive-button.tsx` | `#FF0000` bg |
| IconButton        | `components/icon-button.tsx`        | 40×40 circular, transparent |

### Layout & display
| Component    | File                    | Notes |
|--------------|-------------------------|-------|
| Card + slots | `components/card.tsx`   | Card / Header / Title / Description / Content / Footer |
| Avatar       | `components/avatar.tsx` | sm/md/lg with image or initials fallback |
| Badge        | `components/badge.tsx`  | neutral/info/success/warning/error, optional dot |
| Tag          | `components/tag.tsx`    | Pill chip with animated remove (uses `.tag-pill` from globals.css) |
| Table        | `components/table.tsx`  | Generic `Table<T>` with sortable columns and `render` slot |

### Forms
| Component        | File                               | Notes |
|------------------|------------------------------------|-------|
| TextInput        | `components/text-input.tsx`        | Pill input with label |
| Textarea         | `components/textarea.tsx`          | rounded-2xl, optional flag |
| Checkbox         | `components/checkbox.tsx`          | Custom mark with blur/scale entrance |
| RadioGroup       | `components/radio-group.tsx`       | Custom dot with blur/scale entrance |
| Toggle           | `components/toggle.tsx`            | 40×24 pill switch |
| SegmentedControl | `components/segmented-control.tsx` | Sliding pill indicator |

### Navigation
| Component  | File                        | Notes |
|------------|-----------------------------|-------|
| Tabs       | `components/tabs.tsx`       | Sliding underline indicator |
| Breadcrumb | `components/breadcrumb.tsx` | `/` separators, last item static |

### Overlays
| Component | File                     | Notes |
|-----------|--------------------------|-------|
| Modal     | `components/modal.tsx`   | Centered, scale+translate entrance, scroll-locks body |
| Drawer    | `components/drawer.tsx`  | Right or bottom side, gradient backdrop blur |
| Popover   | `components/popover.tsx` | top/bottom/left/right × start/center/end |
| Tooltip   | `components/tooltip.tsx` | 200ms hover delay, 4 sides |

### Feedback
| Component | File                   | Notes |
|-----------|------------------------|-------|
| Alert     | `components/alert.tsx` | info/success/warning/error, uses `--alert-*` tokens |

To use any primitive in a project: copy the file into `src/components/design-system/` and import as `@/components/design-system/<name>`.

## Extending — building components not in the base set

When the user asks for something not bundled (Modal, Tag, Tooltip, Table, Toast, etc.):

1. **Pull every value from the token tables above.** No hardcoded spacing/colors/sizes outside the scale.
2. **Match the primitive vibe:** pill shapes, `ease-ds`, `hfine:hover`, neutral palette, subtle shadows.
3. **Inline tiny SVGs** (8–16px) instead of pulling lucide-react.
4. **Use `ds-neutral-*` tokens**, never Tailwind's gray families.
5. **Dark mode is non-negotiable** — every component must have `dark:` variants for bg, border, and text.
6. **Use `cn()` from `@/lib/utils`** and accept `className` so callers can extend.
7. **Add `"use client"`** only when the component has interactivity (state, event handlers).

---

## Sharing this skill

To give a friend:
1. Zip the `design-system-base/` folder.
2. Friend drops it into `~/.claude/skills/` (or `<project>/.claude/skills/`).
3. Restart Claude Code. They can now ask Claude to "use the design system base" or `/design-system-base`.

Their project still needs the prerequisites (Next.js 15+, Tailwind v4, shadcn base-nova, Geist) for the components to compile.

# design-system-base

A Claude Code skill that gives Claude knowledge of a clean, minimal, opinionated design system — pill-shaped buttons, neutral palette, Geist typography, `ease-ds` motion — and 23 ready-to-drop component files.

Built for **Next.js 15+ with App Router + Tailwind CSS v4 + shadcn/ui (base-nova style) + Geist font**.

## Install

```bash
git clone https://github.com/<your-username>/design-system-base.git ~/.claude/skills/design-system-base
```

Then restart Claude Code. The skill will auto-load.

### Windows (PowerShell)

```powershell
git clone https://github.com/<your-username>/design-system-base.git "$env:USERPROFILE\.claude\skills\design-system-base"
```

## Use

Once installed, just talk to Claude:

- **"install my design system"** → Claude verifies prereqs, drops `globals.css` and primitives into your Next.js project.
- **"add a [modal / tag / table / ...] in my design system"** → Claude copies the matching component file from the skill.
- **"build me a [thing] in my style"** → Claude uses the token scales and conventions to construct new components that match the system.
- `/design-system-base` also invokes it directly.

## What's inside

- **Tokens** — 14-step neutral scale, 4 pure-saturated semantic colors, 5-token spacing, 6-token type scale, `ease-ds` easing.
- **23 components** across buttons, forms, layout, navigation, overlays, and feedback. See `SKILL.md` for the full list.
- **Animations** — checkmark, radio dot, and tag pill enter/exit transitions in `globals.css`.

## Prerequisites in target projects

The skill will check for these and tell you what's missing:

1. Next.js 15+ with App Router
2. Tailwind CSS v4 (`@import "tailwindcss"`)
3. shadcn/ui initialized with `base-nova` style
4. `cn` utility at `@/lib/utils` (shadcn provides this)
5. Geist + Geist_Mono wired via `next/font/google` in `app/layout.tsx`

## Updating

```bash
cd ~/.claude/skills/design-system-base && git pull
```

## License

MIT — use, fork, modify, share.

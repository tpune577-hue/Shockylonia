# Design

> Visual system for Shocky Topia. Seed version — pre-implementation.
> Mood: *spring morning visit to a friend's small kingdom — pale sky surfaces, sun on stone, herbs in clay pots, a hand-kept chronicle on the windowsill that keeps writing while you're away.*

## Tone

Cozy, bright, approachable. The game is a place you visit several times a day on your phone — not a "tool" you operate. The interface should feel like a friend's notebook, not a developer console.

Strategic depth lives in the writing (Will, Chronicle), not in dense readouts or HUD chrome.

## Color

**Strategy:** Soft sky surfaces with one confident cobalt as identity, one warm coral as "the soul/will", and a meadow-green for growth.

**Anti-defaults handled:**
- Not cream/parchment (cool-tinted pale sky, NOT warm beige)
- Not stark white (subtle cool tint reads as "soft sky", not "blank document")
- Not dark editorial (rejected as too dev/serious)

```css
--bg:        oklch(0.985 0.012 230);  /* pale sky white, cool whisper */
--surface:   oklch(0.97  0.015 230);  /* pillow card */
--surface-2: oklch(0.935 0.020 230);  /* raised card / sections */
--surface-3: oklch(0.89  0.025 230);  /* input bg, hover */
--border:    oklch(0.85  0.025 230);  /* hairline */
--border-soft: oklch(0.91 0.020 230);

--ink:       oklch(0.22  0.030 258);  /* body text, ≥13:1 vs bg */
--ink-soft:  oklch(0.35  0.025 258);  /* sub-headings */
--muted:     oklch(0.50  0.022 258);  /* secondary body */
--muted-2:   oklch(0.62  0.018 258);  /* labels, captions */

--primary:   oklch(0.55  0.180 258);  /* cobalt — links, brand, CTAs */
--primary-soft: oklch(0.92 0.060 258); /* badge bg */

--accent:    oklch(0.72  0.180 35);   /* coral — Will, soul moments */
--accent-soft: oklch(0.94 0.045 40);

--meadow:    oklch(0.68  0.130 150);  /* sage — growth, success, alive */
--meadow-soft: oklch(0.93 0.045 150);

--amber:     oklch(0.78  0.160 75);   /* sunshine — gentle warnings */
--amber-soft: oklch(0.95 0.05 80);

--danger:    oklch(0.60  0.180 25);   /* warm red, used sparingly */
```

## Typography

**Pair on warmth axis** — friendly serif display + rounded humanist sans body.

- **Display:** Fraunces (Google Fonts, variable; SOFT axis high, opsz max) — a confident serif with optical sizes; the SOFT axis gives it a warm, hand-cut feel rather than mechanical.
- **Body / UI:** Nunito (rounded humanist sans, very approachable, free)
- **Thai:** Mitr (Cadson Demak via Google Fonts) — rounded Thai, friendly metric, pairs naturally with both Fraunces and Nunito
- **Tabular numerics:** Nunito's tnum feature for game time displays

### Scale (clamp() responsive)

```css
--text-xs:   0.75rem;                 /* labels, captions */
--text-sm:   0.875rem;                /* secondary body */
--text-base: 1rem;                    /* body */
--text-lg:   clamp(1.125rem, 1.05rem + 0.4vw, 1.25rem);
--text-xl:   clamp(1.375rem, 1.25rem + 0.6vw, 1.625rem);
--text-2xl:  clamp(1.75rem, 1.5rem + 1.2vw, 2.25rem);
--text-3xl:  clamp(2.25rem, 1.8rem + 2.2vw, 3.25rem);  /* Will headings */
--text-display: clamp(2.5rem, 1.8rem + 3vw, 4rem);     /* hero only */
```

- Line length: 60-70ch for reading surfaces
- Display headings: `text-wrap: balance`, slight letter-spacing breathing (-0.018em)
- Body prose: `text-wrap: pretty`

## Layout, Spacing & Shape

```css
--space-1: 0.25rem;    --space-2: 0.5rem;
--space-3: 0.75rem;    --space-4: 1rem;
--space-5: 1.5rem;     --space-6: 2rem;
--space-7: 3rem;       --space-8: 4rem;

--radius-sm: 10px;
--radius-md: 16px;
--radius-lg: 24px;
--radius-xl: 32px;
--radius-pill: 9999px;

--shadow-soft:   0 8px 24px -8px oklch(0.45 0.08 258 / 0.12),
                 0 2px 8px oklch(0.45 0.06 258 / 0.06);
--shadow-raised: 0 20px 50px -16px oklch(0.4 0.10 258 / 0.18),
                 0 4px 12px oklch(0.4 0.08 258 / 0.10);
--shadow-glow:   0 0 0 6px oklch(0.55 0.18 258 / 0.10);
```

**Mobile-first:** Single column, generous spacing, ≥44px touch targets, bottom nav (3 tabs) on a soft floating bar.
**Desktop:** 2-column (nav + content) or 3-column (nav + content + context rail). Centered reading column max 720px.

**Shape vocabulary:**
- Default container radius: 24px (--radius-lg)
- Soft pill buttons (radius-pill) for primary actions
- Avatar/character: round circles
- Map tiles: 8-10px rounded squares, not sharp grid
- Section dividers: dotted or gradient fade, not hard hairlines

## Motion

- **Game-time pulse:** soft 3.5s breathing on accent dot — `prefers-reduced-motion` ⇒ static
- **Page transitions:** 320ms ease-out-quart, fade + 6px translate
- **Tap feedback:** 120ms scale (0.98) on primary buttons
- **Card hover (desktop):** 240ms gentle lift (2px translateY) + raised shadow
- **Will inscription:** 700ms fade + soft glow burst on success
- **No bounce. No spring overshoot. Easing always feels soft, never snappy.**

## Components (Phase 0)

- `Header` — game-time chip (pill) + character avatar (sticky)
- `BottomNav` — 3 rounded tabs floating above bottom edge
- `WillCard` — soft cream card with coral hairline; "Last inscribed" meta
- `StatusRow` — discrete word labels with mood emoji affordance + thin progress
- `ActionLogItem` — time + verb + mode pill (Agent cobalt / Instinct meadow)
- `ChronicleEvent` — editorial paragraph with year/season epoch markers
- `MapPreview` — organic tile layout with rounded shapes, your-soul as warm dot
- `WillEditor` — full-screen modal on mobile, side sheet on desktop
- `KeyVaultForm` — BYOK provider/key/budget with reassuring copy
- `MoodChip` — soft pill that conveys character feeling at a glance

## Anti-patterns

- ❌ Cream/parchment surfaces (warm-beige AI default)
- ❌ Dark mode default (reads as dev tool, rejected by user)
- ❌ Side-stripe borders, gradient text, glassmorphism, hero-metric template
- ❌ Identical card grids, tiny uppercase tracked eyebrows
- ❌ 01 / 02 / 03 numbered section scaffolding
- ❌ MMO HUD (hotbar, healthbar overlay, compass, quest markers)
- ❌ Sharp grid map (use rounded organic tiles instead)
- ❌ Stark hairline borders everywhere (prefer soft shadows + tinted surfaces)

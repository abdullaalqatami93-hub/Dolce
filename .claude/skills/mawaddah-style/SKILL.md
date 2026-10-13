---
name: mawaddah-style
description: Build any page, screen, component or app in this project so it looks, reads and behaves like the Mawaddah (مودَّة) couples app — warm coral/gold/teal palette on cream, plum ink, Georgia + Tajawal type, rounded pill buttons and 24px cards, soft background blobs, a phone-sized app column with bottom tabs, bilingual English/Arabic with full RTL, and a warm Gulf-hospitality voice. Use whenever creating or restyling UI, writing UI copy, or adding screens in this repo, or when the user says "like Mawaddah", "Mawaddah style", "same as Mawadah", or "make it match".
---

# Mawaddah style

Everything in this project should feel like it came from the Mawaddah demo. The
full original source is in `references/mawaddah-demo.html` — **read it before
building anything non-trivial** and copy patterns from it rather than inventing
new ones. This file summarizes the rules.

## 1. Design tokens (use these exact values, never hard-code new colors)

```css
:root {
  --page: #FDEFE0;        /* outside the app column */
  --bg: #FBF5EC;          /* app background (cream) */
  --card: #FFFFFF;
  --ink: #4A2E5C;         /* plum — all main text */
  --ink-soft: #7A6690;    /* secondary text */
  --line: #F3E8D8;        /* borders, dividers */
  --coral: #F27E96;  --coral-strong: #E45C7B;  --coral-soft: #FBE3E9;  --coral-text: #C94468;
  --gold:  #F2AE43;  --gold-soft: #FBEFD9;     --gold-soft-2: #FFE9C2;  --gold-text: #A87B1B;
  --teal:  #3BBFB2;  --teal-soft: #DDF4F1;     --teal-text: #1D8A80;
  --on-strong: #FFFFFF;
  --serif: Georgia, 'Times New Roman', serif;
  --sans: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
  --arabic: 'Tajawal', 'Segoe UI', Tahoma, 'Geeza Pro', sans-serif;
}
```

Dark mode is required. Override under both
`@media (prefers-color-scheme: dark) { :root:not([data-theme="light"]) {…} }` and
`:root[data-theme="dark"] {…}` with:
`--page #140D1B, --bg #1E1428, --card #2A1D37, --ink #F4EAF8, --ink-soft #B8A7CB,
--line #3B2B4A, --coral-soft #4A2634, --coral-text #F59AAF, --gold-soft #3F3219,
--gold-soft-2 #5A4620, --gold-text #F2C46E, --teal-soft #1B3D3B, --teal-text #6FD6CB`
(the strong coral/gold/teal stay the same).

Color roles:
- **Coral** = primary action, selected state, "me" chat bubbles, active tab, love/connection.
- **Gold** = highlights, insights, section labels, dividers, milestones, "Demo" pills.
- **Teal** = success/confirmation (saved, added), focus ring, family.
- Each accent has a `-soft` background + `-text` foreground pair for tints/pills — always use the pair together for contrast.
- Shadows are plum-tinted: `0 1px 3px #4A2E5C14` (cards), `0 8px 24px #E45C7B44` (main CTA).

## 2. Typography

- Load Tajawal: `https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;800&display=swap`.
- Headings (`h1`, `h2`) and big numbers/quotes: **Georgia serif**, plum. h1 48px/1.1, h2 24px/1.25, `text-wrap: balance`.
- Body: system sans 14px; questions 16px/500.
- Arabic: `body[data-lang="ar"]` switches everything (including insight quotes) to Tajawal and sets `letter-spacing: 0` on uppercase labels.
- Small uppercase labels: 12px, 700, `letter-spacing: .08em` (tagline `.25em`), colored with `--gold-text` or `--coral-text`, prefixed with an emoji (✨ 🎈 💡).
- Tabular numerals for dates/counts.

## 3. Layout

- `.stage` centers one **phone-sized app column**: `.app { max-width: 430px; height: 100%; display:flex; flex-direction:column; overflow:hidden; background: var(--bg) }`. On ≥560px add `box-shadow: 0 0 0 1px var(--line), 0 24px 60px #4A2E5C22`.
- Screens scroll inside `.screen { flex:1; overflow-y:auto; padding: 32px 20px 24px }`; welcome/splash screens use `.screen.center`.
- **Bottom tab bar** (`nav.tabs`): card background, 2px `--line` top border, emoji icon over 12px label, active tab `aria-current="page"` in `--coral-text` bold. Hidden on the welcome screen.
- **Decorative blobs**: three absolutely positioned circles behind content — coral-soft top-end (208px), teal-soft mid-start (176px), gold-soft bottom-end (144px). Use logical `inset-inline-*` so they mirror in RTL.
- Always use logical properties (`padding-inline`, `inset-inline-start/end`, `text-align: start`) — never left/right.
- Respect `env(safe-area-inset-*)`.

## 4. Components (copy the CSS from the reference file)

| Component | Key look |
|---|---|
| `.card` | white, `border-radius: 24px`, padding 20px, soft plum shadow, 16px top margin |
| `.btn-main` | coral-strong pill (`border-radius: 999px`), 16px/700, padding 16×36, coral glow shadow, `:active scale(.96)` |
| `.btn-lang` | outlined pill, 2px coral-soft border, coral-text, card bg — the EN⇄AR toggle |
| `.pill` | 10px/700 tiny rounded tag; `.pill.demo` gold-soft/gold-text; tints via `.tint-coral/.tint-teal/.tint-gold` |
| `.nav-card` | full-width 24px-radius button with title, `<small>` subtitle and a chevron (`›`, or `‹` in RTL). Primary variant = `linear-gradient(135deg, var(--coral-strong), #FF8D74)` white text; secondary = card bg with 2px gold-soft border |
| `.insight` | gold gradient (`--gold-soft` → `--gold-soft-2`) card, serif 16px quote, small white tag on top |
| `.mood` grid | 4-column emoji choice chips, 16px radius, 2px line border; selected = coral-strong fill + `scale(1.06)`, `aria-pressed` |
| Progress ring | SVG circle r=44, stroke 10, gold-soft track, coral fill, round caps, serif number in center, animates `stroke-dashoffset` over 1.2s |
| Chat | `.bubble` 24px radius, max-width 82%, card bg + 2px line border; "me" bubble coral-strong/white; composer = pill input with coral-soft border + coral pill Send button; mirror alignment under `[dir="rtl"]` |
| List row `.ev` | card row, 44px rounded-16 tinted emoji icon, bold title, soft date, tinted pill on the end |
| `.idea` tip box | coral-soft bg, coral-text 12px, starts with 💡 |
| Divider | two 32×2px gold bars around a coral `♥` |

Radii scale: 12 (small cells) · 16 (chips, icons) · 24 (cards, bubbles) · 999 (buttons, pills, inputs). Spacing steps: 4 · 8 · 12 · 16 · 20 · 24 · 32 · 40.

Interaction: every tappable thing gets a `transform: scale(.95–.98)` on `:active` with `.15s` transitions, `:focus-visible { outline: 3px solid var(--teal); outline-offset: 2px }`, and all transitions disabled under `prefers-reduced-motion: reduce`.

## 5. Brand mark

The logo is an inline SVG (`mark(size)` in the reference): a coral heart on top, two overlapping circles (teal left, coral right) with a gold lens where they intersect (white 2px stroke), and a white heart in the middle. Reuse that function; don't redraw it. Use it on splash (130px), feature headers (140px) and inside nav cards (46px).

## 6. Bilingual + RTL (mandatory)

- All UI strings live in one `T = { en: {...}, ar: {...} }` dictionary; each language has a `dir` (`ltr`/`rtl`). Never hard-code visible text in markup.
- On render: set `document.documentElement.lang`, `body[data-lang]`, and `dir` on the app root.
- Arabic uses Arabic-Indic digits (٢٠٢٦), Arabic month/day names, and natural Gulf phrasing (هلا، كرك، الخميس) — not literal translation.
- Every screen has a language toggle (`.btn-lang`, label shows the *other* language: "العربية" / "English").

## 7. Voice & content

- Warm, upbeat, light — "a wise cheerful friend" with Gulf hospitality energy. Greetings like "Hala, Khalid! 🌤️".
- Short sentences, one friendly emoji at the end of a line where it adds warmth (✨ 🎉 💛 😉), never more than one per sentence.
- Encourage small, concrete steps over grand gestures; never preachy, never take sides.
- Culturally grounded for Qatar/the Gulf: family respect, karak, Thursday evenings, local names (Khalid, Noora).
- Privacy reassurance is part of the brand ("Your chats stay private, always.").
- Mark sample data honestly with a gold "Demo" pill.

## 8. Code conventions

- Single self-contained HTML file unless the project already has a framework; vanilla JS in an IIFE.
- One `state` object + a `render()` that rebuilds the app root from per-screen `xxxHTML(t)` functions; screens switch via `state.screen`.
- One delegated click listener on the app root using `data-go`, `data-lang`, `data-*` attributes — no inline `onclick`.
- User-generated text (chat) is inserted with `textContent`, never `innerHTML`.
- Accessible: `aria-pressed` on toggles, `aria-current` on tabs, `aria-live="polite"` on chat, `role="img"` + `aria-label` on SVGs, labelled inputs.
- AI features (like the coach) degrade gracefully with a friendly offline message.

## Checklist before finishing

- [ ] Only the tokens above are used for color; dark mode looks right.
- [ ] Headings serif, body sans, Arabic in Tajawal.
- [ ] Pills/cards/radii match the scale; active-press and focus states present.
- [ ] Switch to Arabic: everything mirrors, chevrons flip, no left/right leftovers.
- [ ] Copy sounds like Mawaddah in both languages.
- [ ] Works at 360px wide and centered on desktop; reduced motion respected.

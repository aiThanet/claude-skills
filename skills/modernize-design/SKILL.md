---
name: modernize-design
description: Modernize the look of a component, page, dialog, table or whole app. ALWAYS shows the user ONE HTML file with 2-3 design options and waits for their pick before touching any code. Use when the user says "modernize", "redesign", "make it look modern/cleaner", "restyle", "new UI for X", or asks to improve the design of existing UI.
---

# Modernize Design

Show options first. Write code second. **Never change real UI before the user picks an option.**

## Workflow

### 1. Scope (read, don't guess)
- Find the target: component / page / dialog / table / shell. Read the code it renders, plus its child components.
- **Feature inventory:** list every button, filter, column, dialog, permission check, empty/loading/error state and keyboard/scanner flow. The redesign must keep all of them. Losing one counts as a failure, even if the new design looks better.
- Note who uses it (desk vs phone, older staff, scanning at a station) and how much data it shows (rows, columns, numbers).

### 2. Find the project's design language
Check in this order and use the first one you find:
1. An approved design spec or mock folder (`design/`, `docs/**/specs/*design*.md`, `*.html` mocks)
2. Theme tokens (`themes/*.ts`, `tailwind.config.*`, `:root { --… }`, `_variables.scss`)
3. Spacing and UI rules in `CLAUDE.md` / `AGENTS.md`
4. The UI library already in use (Vuetify, shadcn, MUI, Element…). Options must be possible to build with it, with **no new UI library**.

If you find none, use the defaults in `assets/options-template.html` (§Defaults below).

### 3. Build ONE options file
Copy `assets/options-template.html` → `design/<target>-options.html` in the project. If the project keeps no mocks, save it to the scratchpad instead.
- **2–3 options.** Each must be a different *structure* (layout, information order, interaction), not just a different colour. Good pairs: table vs cards vs split view; tabs vs one long page; inline edit vs dialog.
- Fill with **realistic sample content shaped like the real data** (same columns, same lengths, Thai text if the app is Thai). Make up the values. Never paste real customer names, prices or IDs.
- Each option gets: a name, one line on what it is best for, a ✅ / ⚠️ trade-off list, and the mock itself.
- Mark one as **Recommended** and say why in one line.
- **Every option includes its phone version.** Design the phone layout on purpose, following §Mobile rules; don't just let the desktop layout shrink. If an option works badly on phones, say so in its ⚠️ list.
- The file must work at desktop width and at ~390px (phone toggle), in light and dark. It must be a single file with no build step. Fonts may come from Google Fonts, and nothing else external.

### 4. Show and STOP
- **Only write the local HTML file.** Never publish it as an Artifact or upload it anywhere. Give the user its absolute path so they can open it in a browser.
- Ask which option they want with `AskUserQuestion`: A / B / C, plus room for "mix: A's table + B's filter".
- **Do not edit any source file until they answer.** If they want changes, update the same HTML file and ask again.

### 5. Implement the pick
- Build it with the project's own components and theme tokens. No hard-coded hex or px where a token or utility class exists.
- Keep changes small: restyle and restructure the target only, and leave logic, API calls, routes and permissions alone.
- Don't add wrapper components that only pass props through. Use the library's defaults instead.
- Go through the feature inventory from step 1 and tick every item.
- Build the phone layout at the same time as the desktop one, not afterwards.
- Verify at desktop and ~390px, in light and dark. At 390px check: buttons are full width, spacing is compact, nothing touches, and nothing scrolls sideways. Run the project's build and typecheck.

## Design rules (every option must follow)
- Text ≥ 16px (≥ 18px if users are older), secondary text ≥ 14px, contrast ≥ 4.5:1.
- Tap targets ≥ 44px. Visible focus ring.
- **One** primary-coloured action per screen. Everything else is outlined or text.
- Status = colour **+ word + shape**, never colour alone.
- Numbers: right-aligned, `tabular-nums`.
- No element touches its neighbour. Use a gap scale (4/8/12/16/24).
- Empty states tell the user what to do next.
## Mobile rules (≤600px, always apply)
Phones get a **compact** layout, not a narrower copy of the desktop one. Shrink everything: heights, gaps, margins and padding.

| Thing | Desktop | Phone |
| ----- | ------- | ----- |
| Buttons | auto width, 44–52px tall | **full width** (`width:100%`), 40px tall; stack button groups vertically with an 8px gap |
| Inputs / selects | 44–52px | full width, 40px |
| Gap between siblings | 8–16px | 4–8px |
| Section spacing | 16–24px | 8–12px |
| Card / dialog padding | 18–24px | 8–12px |
| Page side padding | 20–32px | 8–12px |
| Table cells | 14–16px padding | 6–8px padding, or turn rows into stacked cards |
| Headings | 1.7rem | 1.3rem |

- Tap targets stay ≥ 36px tall, with ≥ 4px between them, even when compact.
- Put the main action where a thumb reaches it: the full-width primary button goes at the top of the content or in a bar fixed to the bottom.
- Tables: hide secondary columns, or switch to one card per row. Never scroll the page sideways.
- Filters: stack them full width, or put them behind a "Filters" button. Don't let them wrap into ragged rows.
- Dialogs: open full screen, or as a bottom sheet.
- Use the project's own phone rules if it has them, e.g. a `_mobile.scss` that halves spacing utilities. Don't hand-write new breakpoints when those rules already cover it.

## Defaults (when the project has no design system)
These are the tokens in `assets/options-template.html`: grey-blue neutrals, one blue for focus and active states, one orange accent for the main action, green/amber/red status colours with soft background tints, 12px panel radius, 10px control radius, and a `prefers-color-scheme` dark set.

## Anti-patterns
- Editing code "just to try it" before the user picks.
- Three options that only differ in colour.
- Lorem ipsum, or content that doesn't match the real data's shape.
- Dropping a feature because it doesn't fit the new layout. Find it a place instead, such as a row menu, an expanded row or an overflow menu.

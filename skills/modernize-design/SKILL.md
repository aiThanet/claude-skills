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
- The file must work at desktop width and at ~390px (phone toggle), in light and dark. It must be a single file with no build step. Fonts may come from Google Fonts, and nothing else external.

### 4. Show and STOP
- Open it for the user. Publish it as an Artifact if available, otherwise give the absolute path.
- Ask which option they want with `AskUserQuestion`: A / B / C, plus room for "mix: A's table + B's filter".
- **Do not edit any source file until they answer.** If they want changes, update the same HTML file and ask again.

### 5. Implement the pick
- Build it with the project's own components and theme tokens. No hard-coded hex or px where a token or utility class exists.
- Keep changes small: restyle and restructure the target only, and leave logic, API calls, routes and permissions alone.
- Don't add wrapper components that only pass props through. Use the library's defaults instead.
- Go through the feature inventory from step 1 and tick every item.
- Verify at desktop and ~390px, in light and dark. Run the project's build and typecheck.

## Design rules (every option must follow)
- Text ≥ 16px (≥ 18px if users are older), secondary text ≥ 14px, contrast ≥ 4.5:1.
- Tap targets ≥ 44px. Visible focus ring.
- **One** primary-coloured action per screen. Everything else is outlined or text.
- Status = colour **+ word + shape**, never colour alone.
- Numbers: right-aligned, `tabular-nums`.
- No element touches its neighbour. Use a gap scale (4/8/12/16/24).
- Empty states tell the user what to do next.
- Phones: compact, not just narrower. Hide secondary columns, stack rows, and never scroll the page sideways.

## Defaults (when the project has no design system)
These are the tokens in `assets/options-template.html`: grey-blue neutrals, one blue for focus and active states, one orange accent for the main action, green/amber/red status colours with soft background tints, 12px panel radius, 10px control radius, and a `prefers-color-scheme` dark set.

## Anti-patterns
- Editing code "just to try it" before the user picks.
- Three options that only differ in colour.
- Lorem ipsum, or content that doesn't match the real data's shape.
- Dropping a feature because it doesn't fit the new layout. Find it a place instead, such as a row menu, an expanded row or an overflow menu.

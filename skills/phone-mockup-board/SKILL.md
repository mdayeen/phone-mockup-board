---
name: phone-mockup-board
description: Builds a single-file HTML comparison board of interactive phone mockups shown side by side. Each phone scrolls on its own, filters and tabs actually work, tapping an item opens it in a detail phone, a colour-shade picker recolours every screen, and each phone has a caption, so a designer can screen-record the options (Loom etc.) and send them to a client. Use this whenever someone wants to compare app or mobile-web screen options, show a client several home-screen layouts, turn a sketch or Dribbble-style references into clickable phone mockups, make "three versions side by side", or prepare a design review recording. Use it even when they don't say "board", e.g. "show me 3 mobile layouts I can send the client" or "make these references into clickable phone screens".
license: MIT
metadata:
  version: 1.0.0
  author: Mohammed Ayeenuddin
---

# Phone mockup board

A review board is one HTML page with several phone frames standing side by side. Each phone is a small working app: it scrolls inside its own frame, its chips filter, its tabs switch, and tapping a card opens that item in the detail phone. A designer opens the page, records a walkthrough, and sends it to a client who never has to install anything.

Static images can't do what the board does. The client sees the options in motion, at real phone size and side by side, with every option using the same content. Keep that purpose in mind. Every decision below exists to make the board easy to understand on a recording.

## What you need before building

Collect these from the conversation, the project files, or attached images. Ask only for what is missing and blocks the work.

1. **Subject.** The product or brand, what it does, and who uses it.
2. **Flow to compare.** For example "search → recommended → list" or "onboarding → sign-up". An attached sketch is the strongest input you can get, so follow its order and hierarchy.
3. **Screens.** Default to Row 1 = three options of the main screen, and Row 2 = the screens that follow (a detail screen and a list or browse screen).
4. **Primary colour.** If the client hasn't confirmed it, build three or four shades of that colour into the picker. The default is a light theme with soft surfaces.
5. **Out of scope.** Features that must not appear: login, saved items, payments, etc. Leaving them out keeps the client from seeing promises the contract doesn't cover.
6. **Images.** The project's own photos come first. After that, use openly licensed photos of the real subject, credited in the footer. A tinted tile with an initial is a last resort and must be listed as a placeholder in your report.

When the user attaches reference screenshots they like, read them for patterns, not pixels: the header style, the search treatment, how categories appear (tiles, chips or circles), card shapes, the bottom bar, the detail page structure. `references/screen-patterns.md` catalogues the common ones.

## The three options must differ in layout, not just colour

The most common failure is three phones that share one layout and differ only in colour. Users notice immediately, and it wastes the comparison. Keep the flow the same and vary how each step is presented, for example:

- **Option A:** greeting + search with filter button + category tiles + horizontal cards with two action buttons + a short "today" list.
- **Option B:** photo chips + tall portrait cards + a utility row (affiliate, tools or related) + a 2×2 grid.
- **Option C:** a hero image or video fills the top, then a white sheet slides over it holding the search, then category tiles and a floating bottom bar.

Give each option its own bottom-bar style too (pill, filled icon, floating), because the tab bar is part of the feel. Write a caption under each phone naming the option and what it does differently, in one sentence.

## Build it from the template

Start from `assets/board-template.html`. It already contains the board chrome, the shade picker, the phone frames, the status bar, three bottom-bar styles, a set of components, and the event wiring. Copy it, then:

1. Edit the `CONFIG` object at the top of the script: brand, shades, categories, items, tab labels, detail labels.
2. Rewrite the render functions (`homeA`, `homeB`, `homeC`, `detail`, `list`) to match the user's flow and references. Rename or add screens freely. The template is a starting point, not a cage.
3. Replace placeholder images with real ones (relative paths beside the HTML, or the project's own assets).
4. Update the board title, the description, the row titles, the captions and the footer note.

Keep it one self-contained HTML file plus an `img/` folder. Use no build step and no external scripts. Google Fonts is fine. A reviewer should be able to double-click the file.

### Interactions that must work

A recording shows every dead control, so each visible control does something:

- **Scrolling.** Each phone's screen scrolls independently (an `overflow-y:auto` container inside a fixed-height frame), and the scrollbar is hidden.
- **Filters.** Chips, tiles or category circles filter the content in their own phone.
- **Tap to open.** Tapping any item card in any phone loads that item into the detail phone and scrolls the board to it. This is the moment that makes clients understand the flow.
- **Detail interactions.** The detail phone has at least two: tabs or a segmented control, plus a stepper that rescales numbers (servings, quantity, nights), and/or an accordion.
- **Sorting.** The list phone has a sort control and chips with counts.
- **Shade picker.** It recolours all phones live and remembers the choice in `localStorage` (wrapped in try/catch).
- **Empty states.** Any filter that can return nothing shows a friendly line instead of a blank area.
- **Reduced motion.** Respect `prefers-reduced-motion`.

### Content

Write real, specific sample content in the product's own language and voice: real item names, plausible times, prices or specs, short descriptions. Placeholder lorem ruins a client recording. Mark what is invented in the footer ("Names, prices and times are samples"). Never invent testimonials, ratings, customer counts or press. Where the references show star ratings and the project has no real ratings, use a non-claim stat instead (time, level, size).

Affiliate or sponsored items carry a visible label next to them ("Affiliate link · Store"). Out-of-scope features from step 5 are simply absent, and the footer says why in one line.

## Visual defaults (override with the user's words)

- A light page with white screens. Surfaces are a step darker (around `#f6f4f5`), and borders are hairline only when needed.
- One primary colour plus its soft tint. Text and links on white use the deep shade of that colour for contrast (aim for 4.5:1). Buttons in a pale primary use a dark ink colour, not white.
- Generous rounding: phone around 46px, cards around 20px, chips fully round.
- One friendly sans-serif from Google Fonts, used throughout.
- Photos are rounded, with overlay badges in white pills (duration, category).

If the user says "no dark colours" or "no sharp edges", follow that everywhere, including the phone bezel and the switcher controls.

## Check before you hand it over

Render the page once at desktop width (around 1300px) and once at phone width (around 390px). Use whatever screenshot or browser tool you have. Look for:

- the page scrolling sideways at either width
- text clipped in cards
- a bottom bar covering content (give scroll areas around 100px of bottom padding)
- captions that don't match their phone
- console errors

Fix what you see, look once more, and stop. Don't loop.

## Deliver

- If the environment can publish a hosted page (an Artifact or canvas tool), publish the HTML with its images and give the link.
- Otherwise save `index.html` and the `img/` folder somewhere sensible and give the path. Opening it from the file system works.

Then tell the user briefly:
- what each option does differently
- which interactions to show in the recording, as a click path such as "tap a category tile in A, tap a card, change servings in the detail phone, switch shade"
- what content and images are samples to replace
- what was left out as out of scope

## Files

| File | Read it when |
| --- | --- |
| `assets/board-template.html` | Always: copy it as the starting point. |
| `references/screen-patterns.md` | Choosing layouts for each option, or reading the user's reference screenshots. |

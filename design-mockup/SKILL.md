---
name: design-mockup
description: Presents design proposals as a clickable HTML mockup page in a fixed format, with proposal cards (recommended first), a device frame showing the app's real screens, step buttons and a light/dark toggle. Use for any design change, design review, "how would you do it?" or whenever 2–3 alternatives should be shown before building, in any project and on any platform (web, iOS, Android, desktop, game UI). Triggers: "mockup", "show proposals", "design proposal", "alternatives", "what would it look like".
---

# Design proposals as a mockup page

Every design proposal is shown on the same kind of page, so the user recognises it from one time to the next. The
template is the onboarding flow of a made-up plant app, Sprout, in `example/index.html`, with its fonts in the same
folder. Read it before building, and start from its structure and CSS.

## Parts of the page

1. **Header**
   - Left: a title in the app's heading font, for example "Onboarding – three proposals".
   - Right: a segmented Light/Dark toggle (`#pl`/`#pd`, `aria-pressed`). It switches the theme of both the page and the device.
2. **Proposal cards**
   - A row of 2–3 cards (`.alt`, `data-alt="A"`), for example "A · 4 steps", each with one line summing up the proposal.
   - The recommendation comes first and gets a "Recommended" badge (`.tag`).
   - The selected card has a border in the accent colour.
3. **Left: the device**
   - A frame that matches the platform the project targets:
     - iPhone: 390 × 844 pt, notch, status bar (9:41, 5G), safe area. This is what the example uses.
     - Android phone: 412 × 915 dp, punch-hole camera, gesture bar.
     - Tablet, a desktop window or a browser window for web and desktop apps.
     - The game's own resolution and aspect ratio for game UI.
   - The screen shows the app's real look: its own images, fonts, colour palette (as CSS variables for `.light`/`.dark`) and components.
   - The whole frame must always be visible without scrolling the page. Scale it down to fit the window (`fitPhone()`),
     never above 100 %, and check this at a small window size too (for example 1280 × 800).
   - The device is clickable: its buttons move through the flow.
   - Controls follow the platform's conventions (iOS HIG, Material, the web project's own components), down to details such as separators.
4. **Right: notes**
   - The proposal's name and a short description of how it differs from the others.
   - Rounded step buttons for jumping between screens. The current step is filled (`aria-current="step"`).
   - When needed, sub-proposals for a single part of a screen ("The summary, three proposals"), with "(recommended)" on the first.
   - "Applies to all …": a bulleted list with a bold lead-in on each item, about what the proposals have in common.
   - A small muted line on how to use the page and where the numbers come from.
5. **Linkable states**
   - All state lives in the URL hash, for example `#alt=C&i=0&dark=1`, so every view can be linked to and screenshotted.
   - Read the hash on load (`fromHash()`) and write it back on every change.

## Workflow

1. Copy `example/` to a scratch folder outside the project's source. Never edit the skill's own copy.
2. Swap fonts, images and palette for the current project's.
   - Read them from the project's theme files, for example CSS variables, a Tailwind config, `ThemeData`, `colors.xml`,
     an asset catalog or a theme class. Never guess.
   - If the project has no theme yet, say so and propose one before building.
3. Rewrite `FLOWS`, `NAMES`, `PAGES`, `NOTES`, `SUBS` and `COMMON` for the task, and remove everything specific to Sprout.
4. Start a local server and open the page in a browser:
   `python -m http.server <free port> --bind 127.0.0.1`
5. Check the page:
   - Check that the whole device frame fits in the window, at a small and a large window size.
   - Click through every proposal in light and dark mode.
   - Measure the contrast of new colours and state the ratios.
   - Try navigating with the keyboard.
6. **Look at every screen yourself before showing the page to the user.** Working clicks are not enough.
   - Take sharp full-size screenshots of every step of every proposal and sub-proposal, in light and dark. Scroll
     through long screens. Headless Chrome with the state in the hash gives sharper images than a browser pane that scales down.
   - Review the screenshots like a designer:
     - Are separators, borders and selection states visible?
     - Is spacing equal on both sides? Measure it.
     - Does text overflow its box or wrap badly?
     - Does anything collide or overlap?
     - Does it match the app's real screens?
   - Fix what you find and screenshot again. Briefly tell the user what the review found.
7. Let the user choose a proposal. Do not build until the choice is made.
8. After the choice, the app must look exactly like the chosen proposal. Compare them side by side.

## When an accurate preview is not possible

Say so and explain why, for example that the app's UI framework or game engine cannot render a real background blur,
or that a native control looks different from what HTML can draw. Then show the closest possible image and label it.

# Module Review Desk: Accessibility Conformance Report

**Product:** Module Review Desk, a browser-based SCORM package reviewer and editor
**Prepared for:** UNT Health, Division of Academic Innovation, Educational Development
**Standard:** WCAG 2.1 Level AA
**Date of evaluation:** September 10, 2026
**Evaluated by:** Claude, at the request of Juli James
**Version evaluated:** Standalone build, `module-review-desk.html`

---

## Summary

The tool was evaluated against WCAG 2.1 Level AA using automated scanning, programmatic contrast calculation, and scripted keyboard-only operation. **No Level A or Level AA violations remain.** Three defects were found during evaluation and fixed before this report was written; they are documented in full below rather than omitted.

Automated scanning covered four application states in both light and dark themes, eight scans in total. Result: zero violations, including axe-core's best-practice rules, which are stricter than WCAG requires.

This report also documents a separate and more serious finding in the NUTR-5306 Module 1 SCORM package itself, discovered while testing the tool against it. That finding is in Part 4 and needs attention before the tool does.

---

## Part 1: Method

**Automated.** axe-core 4.x, run against the running application in four states (start screen, package loaded, element selected, finish panel) in both light and dark themes. Rule sets: `wcag2a`, `wcag2aa`, `wcag21a`, `wcag21aa`, plus `best-practice`.

**Contrast.** Every foreground and background pair in the design system was computed directly from the token values using the WCAG relative luminance formula, in both themes, rather than relying on spot checks. Thirty-one pairs were tested against the 4.5:1 threshold for text and the 3:1 threshold for user interface components and graphical objects.

**Keyboard.** A scripted keyboard-only session performed a complete task with no mouse input: reach the skip link, activate it, move between review tabs with arrow keys, locate text through the section list, open it for editing, type a correction, apply it, select text directly in the module preview, select text inside an interactive control, and save the corrected package.

**Reflow.** Rendered at 320 CSS pixels wide to check for horizontal scrolling.

**Not covered by this report.** The content of any SCORM package opened in the tool is a separate artifact with its own conformance obligations. Automated tools cannot evaluate it through the preview frame, and it is authored elsewhere. See Part 4.

---

## Part 2: Defects found and fixed

### 2.1 Selecting text for editing was mouse-only (WCAG 2.1.1 Keyboard, Level A)

**Severity: this was the significant one.** The core function of the tool, choosing which piece of text to correct, was available only by clicking in the preview. A keyboard user could open a package and read it, but could not edit anything. That is a Level A failure of the tool's primary purpose.

Three keyboard routes were added, so that no single method has to suit everyone:

1. **Tab and Enter.** Text elements in the preview are focusable in reading order. Enter or Space opens the focused text for editing and moves focus into the edit field.
2. **Alt and Enter.** Text that sits inside a real control, such as a flip card, an accordion heading or a sidebar link, cannot use Enter, because the control needs that key for its own purpose. Alt plus Enter selects it from the control instead. A chord was chosen rather than a single letter, which keeps this clear of WCAG 2.1.4 Character Key Shortcuts.
3. **The section list.** The Selection panel lists every piece of text in the section currently on screen as a labelled button, showing its type, its current wording, and whether it has already been changed or has a request against it. This gives anyone using a screen reader a short, labelled list to work from instead of stepping through a whole module of body copy, and it is the route most likely to be efficient in practice.

**Verified:** a complete edit-and-save cycle was performed with keyboard input only.

### 2.2 Four text colour pairs fell below 4.5:1 (WCAG 1.4.3 Contrast (Minimum), Level AA)

The secondary text colour in the light theme measured 4.42:1 against panel backgrounds and 4.12:1 against the sunken background, both short of the 4.5:1 requirement. This affected status chips, the location line on each change entry, small action links, and the read-only "originally" field.

The colour was darkened from `#736C63` to `#6D665D`, which measures 4.50:1 at its worst pairing and above 4.8:1 elsewhere. The dark theme already passed and was left alone.

### 2.3 Control borders fell below 3:1 (WCAG 1.4.11 Non-text Contrast, Level AA)

Buttons, text fields and the file drop zone were outlined in the same low-contrast colour used for decorative dividers, measuring between 1.4:1 and 1.8:1. Because these controls share their background with the surface behind them, the border is the only thing identifying the control's boundary, so it carries the 3:1 requirement.

A separate `--control` token was introduced for interactive boundaries, `#87827A` in light and `#786F64` in dark, both clearing 3:1 against every surface they appear on. Decorative dividers keep the lighter colour, which is correct: they convey nothing and are exempt.

### 2.4 Structural items also corrected

These were flagged as best practice rather than as WCAG failures, but were fixed because the tool is being deployed as an institutional resource:

- **Landmarks.** The interface now uses `header`, `main` and a labelled `aside`, so all content sits inside a landmark and assistive technology users can move between regions.
- **Heading structure.** A page-level heading was added, and each review panel now carries its own heading, so the headings inside them are no longer orphaned at the wrong level.
- **Skip link.** The first thing in the tab order is a skip link that jumps past the module preview to the review panel. Without it, reaching the edit controls from the keyboard would mean tabbing through every paragraph in the module. It points at the panel's tab list rather than a panel that can be hidden, so it never becomes a broken target.
- **Status announcements.** A polite live region announces what has been selected and where it sits, so a screen reader user knows the selection changed without having to go looking.
- **Focus movement.** Focus moves into the edit field only when the selection was made from the keyboard. Mouse users keep their focus where it was, which avoids the focus jump that makes some editors unusable.
- **Review tab pattern.** The three review tabs now use roving tabindex with arrow key, Home and End support, matching the expected behaviour for tabs.
- **Focus visibility.** Focus indicators were strengthened to a 3px outline in the tool's accent colour, which measures above 7:1 against every surface, and matching indicators were added inside the preview frame.

---

## Part 3: Results after fixes

### Automated

| State | Light theme | Dark theme |
|---|---|---|
| Start screen | 0 violations | 0 violations |
| Package loaded | 0 violations | 0 violations |
| Element selected | 0 violations | 0 violations |
| Finish panel | 0 violations | 0 violations |

Two items were returned as "needs review", meaning the tool could not decide automatically:

- **Contrast of the validation checkmarks.** Manually computed: 6.30:1 in light, 7.21:1 in dark. Passes.
- **Content inside the preview frame.** axe cannot reach into the frame. This is the SCORM package's own conformance, not the tool's. See Part 4.

### Keyboard, verified end to end

| Step | Result |
|---|---|
| Skip link is the first tab stop and reaches the review panel | Pass |
| Arrow keys move between review tabs, panels follow | Pass |
| Section list enumerates the visible section (49 items in the section tested) | Pass |
| Enter on a list item opens it and moves focus to the edit field | Pass |
| Tab from the edit field reaches the apply control | Pass |
| Enter on focused text in the preview opens it for editing | Pass |
| Alt and Enter from a control selects its text | Pass |
| Live region announces the selection | Pass |
| Complete save performed with no mouse input | Pass |

### Contrast, all pairs

All 31 foreground and background pairs meet or exceed their threshold in both themes. Lowest passing text pair: 4.50:1. Lowest passing control boundary: 3.01:1.

### Reflow

At 320 CSS pixels the layout stacks to a single column with no horizontal scrolling. Document width equals viewport width exactly.

### Other provisions checked by inspection

- Page language is declared (`lang="en"`), and the document has a descriptive title
- All form fields have programmatically associated labels
- The file drop zone has a keyboard-operable button alternative, so drag and drop is not the only route
- Colour is not the only means of conveying state: changed and requested items carry text labels as well as colour and outline style
- `prefers-reduced-motion` is respected
- No content flashes, moves automatically, or imposes a time limit
- Errors are reported in text, naming the section and field concerned

---

## Part 4: A separate finding, in the NUTR-5306 Module 1 package

This is not about the tool. It surfaced because the tool renders the real package faithfully, and it is more urgent than anything above.

**The flip cards in Module 1 do not render.** All six cards in "Social and Environmental Determinants of Food-Related Behaviors", and 15 cards across the module, collapse to small coloured squares roughly 38 by 34 pixels showing two or three clipped letters. The content is present in the file but invisible on screen.

**Cause.** The card markup uses `<span class="flip-inner">`, and the stylesheet sets `height: 100%` and `min-height: 190px` on it. A `span` is an inline element, and height and min-height do not apply to inline elements, so the container has zero height. The card faces are positioned `absolute; inset: 0` against that zero-height container, so they collapse.

This is the exact failure the builder skill warns about in its own guidance on flip cards, which says to build the faces as block-level elements because span-based faces break. The guidance is right and the package did not follow it.

**Fix.** One CSS declaration in the package: `.flip-inner { display: block; }`. Using `<div>` instead is not an option, because these sit inside a `<button>`, whose content model does not allow block elements. The span is correct markup; it just needs to be told to lay out as a block.

**Scope.** Verified at 1400, 1280, 1100 and 820 pixels wide. Same result at every size. Any other package built with the same card markup has the same defect.

**Two consequences worth separating.** Visually the cards are unreadable, which is a straightforward content defect. Separately, the collapsed faces become scrollable regions with no keyboard access, which axe-core reports as a serious WCAG 2.1.1 failure across 30 elements. Fixing the layout resolves both.

**Recommended order.** Fix the package and the builder skill before the review tool goes to faculty. Otherwise the first thing a faculty member sees in the preview is broken cards, and they will reasonably report it as a bug in the tool.

---

## Part 5: Known limits of this evaluation

Stated plainly, because a conformance report that claims more than it tested is worse than no report.

- **No assistive technology was used.** Everything here was verified by automated scanning, computed contrast, and scripted keyboard operation. No screen reader was run. NVDA, JAWS or VoiceOver testing would tell you things this evaluation cannot, particularly about how the live region announcements and the section list feel in practice rather than whether they exist.
- **No testing with disabled users.** Automated conformance and real usability are different questions.
- **Browser coverage is one engine.** Testing ran in Chromium. Firefox and Safari behaviour, particularly focus handling inside the preview frame, was not verified.
- **Packages are out of scope.** The tool's conformance says nothing about the conformance of any package opened in it.
- **Fonts degrade.** If the Google Fonts request is blocked, the interface falls back to system fonts. This affects appearance only.

---

## Part 6: Recommendations before deployment

1. **Fix the flip card defect** in the package and in the builder skill. This is ahead of everything else on this list.
2. **Have Vivian review this report** and decide whether screen reader testing is required before an institutional deployment. As the accessibility coordinator she owns that threshold, not this document.
3. **Consider one screen reader pass** over the three keyboard routes in section 2.1, since that is the part most likely to be adequate on paper and awkward in practice.
4. **Keep the accessibility work in version control** alongside the tool, so the next change can be checked against this baseline rather than re-derived.
5. **Re-run the automated scan** whenever the tool changes. The scripts used here are reproducible and take under a minute.

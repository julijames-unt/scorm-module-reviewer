# Changelog

## 2026-09-15

Component detection now matches on behaviour rather than class names, so it survives
the builder skill renaming things.

- Counting uses `aria-expanded`, `role="radio"`, `data-dnd-group` and `data-dnd-accepts`
  first, and carries every class vocabulary the builder has used, old and current.
- The Finish panel reports a per-type breakdown (accordions, flip cards, answer options,
  drag items, drop zones) instead of a single total, so a count that returns zero is
  visible instead of hidden inside a sum.
- Labels cover the current component classes, so a flip card front anchors as
  "flip card, front" rather than "div text". Added labels for knowledge check
  questions, options and feedback, hero and statement blocks, statistic tiles,
  topic cards, journey steps, learning objectives, part maps and image placeholders.
- More standardised controls are locked to the note path: the check-answer button,
  the continue button, accordion badges, progress rings and the lesson meta line.

**Why this mattered.** The previous version counted accordions as `.acc-trigger`.
Current packages call them `.accordion-header`, so the count returned zero, and the
"interactive pieces still intact" check compared zero to zero and passed on packages
it had never examined. Editing and saving were never affected; the guard was.

Verified against three package generations (August 2026 build, Chapter 1 v4,
Chapter 2): counts correct and traced to the elements producing them, save round-trip
clean with only `index.html` changed, no accessibility regressions, keyboard suite
passing end to end.

## 2026-09-10

First release.

- Live preview of the real package with click and keyboard editing
- Anchored notes on any element, including ones that cannot be edited directly
- Corrected package plus a review file listing changes made and changes requested
- WCAG 2.1 AA conformance pass: see `ACCESSIBILITY.md`

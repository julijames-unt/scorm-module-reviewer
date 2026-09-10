# Module Review Desk

A browser tool that lets faculty read a SCORM course module the way their students will, correct wording themselves, and pin a request for anything larger to the exact spot it belongs.

Built for UNT Health, Division of Academic Innovation, Educational Development, to accompany the `scorm-canvas-builder` workflow.

---

## What it does

Open a SCORM `.zip` and the real package runs in the left pane, with its sidebar, accordions, flip cards and images all working. Click or tab to any text and it opens in the right pane for editing. Anything that cannot be edited directly offers a note field instead, so nothing is a dead end. Finishing produces a corrected `.zip` plus a review file listing what was changed and what was requested.

A sample module is built in, so the tool can be demonstrated without a real package.

## What it deliberately does not do

- It does not call any AI service. Every edit is deterministic.
- It does not upload anything. All processing happens in the visitor's browser.
- It does not add, delete or restructure sections, components or images. Those are requests, not edits.
- It is not a review and approval system. There are no accounts, no comment threads and no sign-off state, by design. See `docs/design-spec.md`, Part 7.

## Privacy

Nothing leaves the browser. The package is read into memory with JSZip, rendered in a same-origin frame, edited in place, and written back to a file the visitor saves. There is no server, no analytics, no telemetry and no network request after the page itself loads, apart from the optional web font.

This is the reason the tool is safe to hand to faculty for course content: hosting it does not put you in the path of the material.

---

## Deploying with GitHub Pages

The repository is already laid out for it. `index.html` sits at the root and `.nojekyll` stops Jekyll from processing the site.

1. Push this directory to the repository.
2. In the repository, go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to *Deploy from a branch*.
4. Choose the branch and the `/ (root)` folder. Save.
5. Wait for the first build, then open the URL Pages reports.

On github.com the URL is `https://<org>.github.io/<repo>/`. On GitHub Enterprise it follows your instance's Pages host, and it may be reachable only inside the institutional network, which is often the preferable outcome for an internal tool.

### After deploying, check these

- Open the URL and use **Try it with a sample module**. If the sample loads and you can edit text, the deployment is sound.
- Open a real package and save it. Confirm the download works in the browsers your faculty actually use.
- Confirm the page loads over HTTPS. Some browsers restrict file handling on insecure origins.

### If it does not have to be Pages

`index.html` is entirely self-contained. It also works served from any static web host, attached to a Canvas page, or saved and opened directly from disk with no server at all. Pages is the convenient option, not a requirement.

---

## Before this goes to faculty

Three things are outstanding, and none of them are code.

**1. Stamp the package format first.** The tool recognises packages by a `scorm-builder-format` meta tag and warns when it is missing or unfamiliar:

```html
<meta name="scorm-builder-format" content="reading-v1">
```

The builder skill should emit this on every package, and existing packages need it backfilled. Without it the guard against format drift has nothing to check. See `docs/design-spec.md`, Part 5.

**2. Decide who receives review files, and how quickly they respond.** Requests that go nowhere teach faculty that the participation was decorative, and they stop using the tool. This is the commitment that decides whether the note path works. It belongs in the documentation that goes out with the link.

**3. Pilot with one faculty member on one package** before the URL circulates.

---

## Accessibility

`ACCESSIBILITY.md` is a WCAG 2.1 Level AA conformance report covering method, defects found and fixed, results, and the limits of the evaluation. Summary: no Level A or AA violations across eight automated scans in both themes, keyboard operation verified end to end, all colour pairs computed rather than sampled.

The report is honest about what was not done, including that no screen reader was used. Read Part 5 before treating it as a clearance.

If you deploy this as an institutional resource, route the report past your accessibility coordinator and let them decide whether assistive technology testing is required.

---

## Browser support

Tested in Chromium. The tool uses `DOMParser`, `Blob`, object URLs, CSS custom properties and `MutationObserver`, all long-standing web platform features. Firefox and Safari have not been verified; test them before announcing if your faculty use them.

Drag and drop is offered but never required. Every path through the tool is available from the keyboard.

---

## Making changes

The tool is a single file. Edit `index.html`, commit, and Pages redeploys.

Two things to preserve when you do:

- **JSZip is inlined** near the top of the file, between the `/*! JSZip v3.10.1` comment and the tool's own stylesheet. Leave it alone unless you are deliberately upgrading it, and keep the licence comment with it.
- **Re-run the accessibility checks** after any interface change. The method is documented in `ACCESSIBILITY.md` Part 1 and takes under a minute to repeat.

### Removing the web font request

If your institution would rather the page make no external request at all, delete the three `<link>` tags in `<head>` that point at `fonts.googleapis.com` and `fonts.gstatic.com`. The interface falls back to the system font stacks already declared in the stylesheet. Nothing else changes.

---

## Repository contents

| Path | What it is |
|---|---|
| `index.html` | The entire tool, self-contained |
| `ACCESSIBILITY.md` | WCAG 2.1 AA conformance report |
| `THIRD-PARTY-NOTICES.md` | JSZip licence notice, which must be retained |
| `docs/design-spec.md` | Design decisions, scope boundaries, and the reasoning behind them |
| `.nojekyll` | Tells GitHub Pages to serve the files as they are |

## Licence

Not yet set. Until a `LICENSE` file is added, default copyright applies and the work is not licensed for reuse outside the institution. If you intend others to reuse it, add one; if you intend the opposite, the current state is already correct.

Whatever you choose, the JSZip notice in `THIRD-PARTY-NOTICES.md` has to stay.

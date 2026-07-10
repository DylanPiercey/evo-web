# Unclear Code & Docs

Things that were hard to understand, and what would have clarified them. Format and rules: [README.md](README.md).

## evo-accordion stretches the `a11yText` convention onto `aria-roledescription`

`packages/evo-marko/src/tags/evo-accordion/index.marko:33` | 2026-07-10 | impact:low | effort:low

The root `CLAUDE.md` `a11yText` convention frames the prop as a required-or-overridable accessible label ("English default to be overridden"), and reference components (`evo-badge`, `evo-chip`) map it to `aria-label`. `evo-accordion` maps `a11yText = "accordion"` to `aria-roledescription` instead, which is a different ARIA semantic (it changes how the role is announced, not the name). A consumer following the convention docs will set `a11yText` expecting a label. Either rename the prop (e.g. `a11yRoleDescription`) or extend the CLAUDE.md convention to say which ARIA attribute `a11yText` may target per component. Related: `evo-accordion`'s exported `Input<Open extends number | number[] | undefined>` generic is unusual for this codebase and complicates consumer typings; worth a comment or simplification.

## evo-dialog's `open=null` spread-exclusion trick is load-bearing but only comment-documented

`packages/evo-marko/src/tags/evo-dialog/index.marko:68` | 2026-07-10 | impact:low | effort:low

`evo-dialog` (and `evo-toast-dialog:49`) render `<dialog ... open=null>` specifically so the spread cannot re-enable Marko's control of the `open` attribute and fight `showModal()`/`requestClose()` DOM state. Removing or "simplifying" the attribute re-introduces the conflict, and nothing outside an inline comment protects it. Worth a note in the component README or a shared helper/pattern writeup (this trick will recur for any element whose state the browser owns), plus ideally a browser test that fails if the attribute is removed.

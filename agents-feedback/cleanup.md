# Cleanup

Duplication, dead code, inconsistency, refactor opportunities. Format and rules: [README.md](README.md).

## Resolve the handler-call convention contradiction between CLAUDE.md and 12 components

`CLAUDE.md:1` (correctness_guards section) | 2026-07-10 | impact:med | effort:med

Root `CLAUDE.md` mandates `onFoo && onFoo(e, el)` and explicitly forbids `(onFoo || null)?.(e, el)`, claiming "Marko handler types are not plain functions so optional-call syntax fails type checking". Yet 12 tag files use the "forbidden" form (`grep -rl '|| null)?.(' packages/evo-marko/src/tags --include='*.marko'`): evo-file-input, evo-file-preview-card-group, evo-filter-chip, evo-filter-input, evo-input, evo-listbox, evo-menu, evo-number-input, evo-segmented-buttons, evo-tabs, evo-textarea, evo-toggle-button. Meanwhile evo-dialog uses the CLAUDE.md form (`packages/evo-marko/src/tags/evo-dialog/index.marko:81`) and a third form exists too (`input.onEscape?.(e)` in evo-button). Per Marko maintainer guidance, `(input.onFoo || null)?.(...)` is the canonical Marko 6 optional-handler idiom (handler attributes may legitimately be falsy — e.g. `onClick=cond && handler` — and bare `false?.()` throws, which also makes evo-button's `?.()` form unsafe). Resolution: update CLAUDE.md to endorse `(x || null)?.()`, and align the `&&`-form and bare-`?.()` files — agents generating new components currently receive guidance the codebase (correctly) contradicts.

## evo-tabs computes an unused `size` constant

`packages/evo-marko/src/tags/evo-tabs/index.marko:23` | 2026-07-10 | impact:low | effort:low

`<const/size=([...tabs || []].length)>` is declared and never referenced anywhere else in the file (single grep hit). Delete it, or use it if a length was intended somewhere (e.g. aria attributes).

## evo-select spreads the non-standard `optgroup` key onto `<option>` in the ungrouped branch

`packages/evo-marko/src/tags/evo-select/index.marko:103` | 2026-07-10 | impact:low | effort:low

The grouped branch strips the marker before spreading (`<const/{ optgroup, ...itemHtmlInput }=option>` at lines 94-97), but the ungrouped branch does `<option ...optionOrGroup as any/>`, which would spread `optgroup` onto the element and only stays harmless because the key is `undefined` for ungrouped options after `isGroup` filtering. The `as any` also disables type checking on that spread. Mirror the destructure from the grouped branch and drop the cast.

## Standardize optional attr-tag iteration guards and exclusion destructures

`packages/evo-marko/src/tags/evo-accordion/index.marko:40` | 2026-07-10 | impact:low | effort:low

Two adjacent inconsistencies make migrated components harder to read. (1) Optional attr tags are iterated three ways: bare `<for|item, index| of=items>` (evo-accordion:40, evo-tabs:32 — safe because Marko 6's `forOf` runtime tolerates falsy lists, verified at marko-js/marko `packages/runtime-tags/src/common/for.ts:15`), spread-with-default `[...items || []]` (evo-breadcrumbs:28), and `[...(inputOptions || [])]` (evo-select:32). (2) Several components destructure controllable props solely to exclude them from the `...htmlInput` spread (`index: inputIndex, indexChange` in evo-tabs:13-14; `open: inputOpen, openChange` in evo-accordion:23-24 and evo-dialog:22-23); the aliases are never read, which reads as dead code until you notice the rest-spread. Pick one iteration idiom, and either comment the exclusion destructures or adopt a naming convention (`_indexChange`) that signals intent.

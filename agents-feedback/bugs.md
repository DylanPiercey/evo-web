# Suspected Bugs

Out-of-scope defects and behavior regressions noticed while working on something else. Format and rules: [README.md](README.md).

## evo-select dropped ebay-select's form-reset resync

`packages/evo-marko/src/tags/evo-select/index.marko:33` | 2026-07-10 | impact:med | effort:med

Marko 5 `ebay-select` subscribes to its parent form's `reset` event and re-syncs the visually selected option (`packages/ebayui-core/src/components/ebay-select/component.ts:79`). The Marko 6 `evo-select` has no reset handling at all (zero matches for `reset` in `packages/evo-marko/src/tags/evo-select/index.marko`); it binds `<select value:=value>`, so after a native form reset the DOM select returns to its default option while the `value` state (and anything derived from it, e.g. the floating label's `value`) keeps the pre-reset value. Add a `reset` listener on the owning form (e.g. a `<script>` with `selectEl().form?.addEventListener("reset", ..., { signal: $signal })`) that re-syncs `value`, or document the intentional behavior change for migrating consumers.

## evo-accordion dropped ebay-accordion events and event payloads without a replacement

`packages/evo-marko/src/tags/evo-accordion/index.marko:44` | 2026-07-10 | impact:low | effort:low

Marko 5 `ebay-accordion` emits a `click` event (`packages/ebayui-core/src/components/ebay-accordion/index.marko:23`, typed `"on-click"?: (event: { originalEvent: MouseEvent }) => void` in `component.ts:12`) and a `toggle` payload containing `{ originalEvent, open, index }` (`component.ts:47`). The Marko 6 `evo-accordion` exposes only `openChange(open)` with the new open index/array and no original DOM event, and has no click passthrough on the details items. Consumers migrating from ebayui lose the click hook and the event object silently. Either forward per-item `onToggle`/`onClick` handlers (they spread via `...item` onto `<evo-details>` today only if the item author sets them, minus the original event context ebay provided) or record the parity break in the component's migration notes.

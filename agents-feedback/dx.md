# Developer Experience

Friction in builds, tests, tooling, skills, or repo workflows. Format and rules: [README.md](README.md).

## Align /evo-migrate-marko with the upstream marko-5-to-6-migration skill

`.claude/skills/evo-migrate-marko/SKILL.md:1` | 2026-07-10 | impact:med | effort:low

marko-js/marko now carries a general-purpose `skills/marko-5-to-6-migration/` skill (SKILL.md + api-mapping.md + patterns.md + interop.md) with the exhaustive Class API → Tags API mapping tables, interop file-classification rules, and verified gotchas (e.g. Class-style `on-foo("method")` bindings on a Tags API child compile but never fire; pending promises cannot cross the compat boundary). `/evo-migrate-marko` overlaps with a subset of that mapping and predates some of it: its lifecycle row only offers `<script>` ("use very rarely") and never mentions `<lifecycle>` for imperative third-party libraries (MakeupJS-style instances), and it doesn't cover `<define>`-vs-`<macro>`, `<try>`/`<await>`, or `once-*` handlers. Cross-link the upstream skill for the general API mapping and keep the evo skill focused on what is genuinely eBay-specific (naming, Skin CSS, a11yText, story/test templates, MakeupJS replacements).

## evo-marko components are generated from guidance the codebase contradicts

`CLAUDE.md:1` (correctness_guards section) | 2026-07-10 | impact:med | effort:low

Agent pipelines (`/evo-component`, `/evo-migrate-marko`) inherit root `CLAUDE.md` rules that the shipped components violate — most prominently the pass-through handler call form (see `cleanup.md` entry "Resolve the handler-call convention contradiction"). Until the convention is reconciled, generation output oscillates between forms depending on which files an agent reads as references, and reviewers can't tell which form to enforce. Fixing the docs/code split once removes a whole class of review churn.

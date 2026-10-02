# skill-diagram-design

A portable, org-chart-focused adaptation of Cathryn Lavery's Diagram Design skill. It is an authoring guide, not an automatic layout SDK or live editor.

Upstream: https://github.com/cathrynlavery/diagram-design
Reference revision: v2.6.33 / `dc1ace47b99a419e42d01a03cb6ace5346efa8ae`.
License notice: [MIT](../licenses/diagram-design-MIT.txt).

## What the viewer must learn

Who plausibly belongs where, who may report to whom, and which relationships are weak, missing, or only assumed. Visual polish must not overstate evidence.

Read the completed research table, frozen department/function/people ontologies and validated graph before designing. A table complements but never replaces this task's required full-roster canvas.

## Layout and complexity

Adapt the upstream Org Chart / Responsibility Map grammar:

- Publish a **separate org-chart artifact/page** titled exactly **Probabilistic Org Chat with Jev**, linked to the people/profile explorer and sharing its persisted dataset.
- Use one huge zoomable/pannable canvas for **all in-scope people, up to 250 per approved cohort**. No 12-node, four-tier, or five-report display cap: those small-diagram heuristics do not apply to this full-roster canvas.
- Default to all people included; provide fit-all, zoom, pan, reset and search/focus. Fit-all may zoom out, but users must be able to zoom in to readable names/line labels. Do not shrink text permanently to cram 250 people into a small card.
- Large fan-out and deep trees use expanded spacing, buses and marked layout-only groups. Never hide specialists, cap reports, collapse away people by default, or invent managers.
- Optional department/pod views supplement the full canvas. For approved 251–1,000-person grouping, each cohort has its own full canvas and group/scope subtitle; do not stitch them into an invented company-wide tree.
- Orthogonal routes: vertical drop, horizontal bus, vertical drop to each child. No diagonal connector spaghetti.
- One focal accent node; at most two callouts. Put the legend outside the node area.
- Name, terse role/scope and legible selected-manager probability plus separate Jev confidence at each scored reporting line, adjacent to the person.
- Full biography, alternatives and evidence belong in a detail panel, not inside a node.
- Label outside-roster managers and uncertainty; never hide gaps to make a perfect pyramid.

## Evidence-aware visual grammar

This package intentionally sharpens the source example's line semantics:

| Element | Meaning |
|---|---|
| Solid connector | Reporting relationship explicitly supported by a cited source |
| Dashed connector | Jev-inferred reporting relationship |
| Labelled assumed connector | Default assumption such as CEO → Board, unscored |
| Unknown/outside node | Manager unresolved or not in the scoped roster |
| Dashed/marked person boundary | Identity/profile is unresolved, with an explanation |
| Department grouping container | Visual grouping only, not a manager |

Do not make an inference solid because its model probability exceeds a threshold. Keep identity status, edge evidence, selected-option probability, and distribution confidence separate.

Show at the reporting-line endpoint adjacent to each name: `Manager option: 76% · Jev confidence: 61%` (illustrative). The first is `probabilities[choice]`; the second is the separate `confidence` value. With limited space, use `M: 76% · C: 61%` and a visible legend/accessible full label. Confidence measures distribution concentration, not correctness. Neither metric scores a person's identity or the whole chart.

For board/default assumptions show `Assumed · Jev confidence: N/A`, never `100%`. For unavailable/unscored cases use explicit status and N/A. Scored unknown/outside choices retain real metrics in their person/uncertainty lane, without false reporting connectors. Every unresolved person remains visible.

## Editorial restraint

Use paper/ink/muted/accent roles mapped to the host project's brand. One or two restrained accents, clear typography, generous spacing, no ornamental glow or gratuitous shadows. Do not force upstream font choices on every project.

Inline SVG: preserve a responsive viewBox, accessible title/description, unique IDs, and escaped labels. For large or changing graphs, a dedicated layout library can implement this grammar; fixed upstream SVG examples are not an automatic layout engine.

On narrow screens, keep labels readable and pan within the diagram—not the entire page. Provide a visible pan hint and a useful list/table alternative. Do not rely on color alone.

## Persistence, exports, and checks

- Give the org chart its own artifact/page, linked from the people/profile explorer. Share the same dataset; do not create an unrelated or inconsistent second graph.
- Persist the researched graph and decisions separately from UI selection/filter state.
- Check node/label overlap, connector routing, real long names, clipping, contrast, keyboard access, detail panels, and a roughly 390px viewport.
- Export the complete unfiltered cohort, not just the current viewport. At 250 people verify 250 distinct person nodes, full title, both metric labels, legend and caveat. Keep any filter/export scope explicit.
- Self-contained HTML is useful for portable viewing; SVG/PNG are useful for static diagrams. Preserve styles/viewBox and test destination rendering.
- Bundle necessary assets locally if used, preserve applicable licenses, and do not depend on remote fonts.
- Optional public sharing requires separate approval and a real privacy review. Fictional demo data is simpler; hidden mappings are not private.

## Optional upstream examples

If a richer example is useful, read the pinned sources rather than guessing filenames:

- [SKILL.md](https://github.com/cathrynlavery/diagram-design/blob/dc1ace47b99a419e42d01a03cb6ace5346efa8ae/skills/diagram-design/SKILL.md)
- [Org chart grammar](https://github.com/cathrynlavery/diagram-design/blob/dc1ace47b99a419e42d01a03cb6ace5346efa8ae/skills/diagram-design/references/type-org-chart.md)
- [Style guide](https://github.com/cathrynlavery/diagram-design/blob/dc1ace47b99a419e42d01a03cb6ace5346efa8ae/skills/diagram-design/references/style-guide.md)
- [Org chart example](https://github.com/cathrynlavery/diagram-design/blob/dc1ace47b99a419e42d01a03cb6ace5346efa8ae/skills/diagram-design/assets/example-org-chart.html)

The bundled guide contains what this bot needs; upstream access is optional, not a hidden launch dependency. Record any adopted upstream revision and asset/font notices in the final handoff.

# skill-diagram-design

A portable, org-chart-focused adaptation of Cathryn Lavery's Diagram Design skill. It is an authoring guide, not an automatic layout SDK or live editor.

Upstream: https://github.com/cathrynlavery/diagram-design
Reference revision: v2.6.33 / `dc1ace47b99a419e42d01a03cb6ace5346efa8ae`.
License notice: [MIT](../licenses/diagram-design-MIT.txt).

## What the viewer must learn

Who plausibly belongs where, who may report to whom, and which relationships are weak, missing, or only assumed. Visual polish must not overstate evidence.

Read the ontology and graph before designing. Prefer a table if a diagram adds no clarity.

## Layout and complexity

Adapt the upstream Org Chart / Responsibility Map grammar:

- Overview first; separate department/pod detail charts.
- At most 12 visible org nodes and four tiers per chart.
- At most five direct reports displayed under one parent before subdivision; if many specialists exist, introduce visibly labelled layout groups or split views. Do not invent reporting relationships.
- Orthogonal routes: vertical drop, horizontal bus, vertical drop to each child. No diagonal connector spaghetti.
- One focal accent node; at most two callouts. Put the legend outside the node area.
- Name, terse current role/scope and a legible probability label in each scored person node.
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

Show beside a name: `Manager option: 76% (Jev estimate)`; when space is limited, `M: 76%` with a visible legend. For board/default assumptions show `Assumed`, never `100%`. The number belongs to the reporting choice, not the person's identity or the probability the whole chart is correct.

## Editorial restraint

Use paper/ink/muted/accent roles mapped to the host project's brand. One or two restrained accents, clear typography, generous spacing, no ornamental glow or gratuitous shadows. Do not force upstream font choices on every project.

Inline SVG: preserve a responsive viewBox, accessible title/description, unique IDs, and escaped labels. For large or changing graphs, a dedicated layout library can implement this grammar; fixed upstream SVG examples are not an automatic layout engine.

On narrow screens, keep labels readable and pan within the diagram—not the entire page. Provide a visible pan hint and a useful list/table alternative. Do not rely on color alone.

## Persistence, exports, and checks

- Integrate into the intelligence explorer rather than publishing an unrelated second app.
- Persist the researched graph and decisions separately from UI selection/filter state.
- Check node/label overlap, connector routing, real long names, clipping, contrast, keyboard access, detail panels, and a roughly 390px viewport.
- Export an explicit complete static state. Include the legend and sourcing/inference caveat.
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

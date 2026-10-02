# Acceptance checks

These are review/launch scenarios, not a claim that the research bot has already passed end-to-end tests.

## Inputs and consent

- Company + roster: preserve all rows; accept optional LinkedIn company URL and user narrowing.
- Company only: ask region/function/brand or confirm a bounded public-discovery pass.
- No company + work-domain email: propose a company and wait for confirmation.
- Free/personal/ambiguous domain: ask company; do not treat the domain as an employer.
- Private organization project + no roster: project-member option is offered only after project ownership is established; enumeration waits for explicit consent.
- Playground: neither member lists nor project-wide domains are offered or queried, even with permission.
- Unknown/projectless/shared-community project: member fallbacks stay disabled.
- Empty/mixed/email-only list: report ambiguities and keep unresolved rows.
- Missing company and list: combine applicable questions in one concise message.

## Roster size and Choice limits

- **250 people:** proceed within scope; 249 in-group manager candidates plus two sentinels = 251 options; allowed. Each Choice still validated independently.
- **251 people:** pause before bulk research/Jev. Ask narrowing or consented smart groups; never silently continue to one org chart.
- **1,000 people:** same choice as 251; a consented split is allowed only if every approved cohort is at most 250.
- **1,001 people:** stop; require city/region/country/function/department narrowing. Do not let a full-list grouping choice bypass the gate.
- **1,001 narrowed to 600:** confirm the new scope, then ask narrowing again or an approved split; do not treat the first narrowing as approval to split.
- Public discovery crossing 250 or 1,000: checkpoint and pause, then re-run scoped search after narrowing; no arbitrary first-N truncation.
- Supplied list narrowed: retain excluded rows as `Out of scope`; unresolved scope fields require user input, not silent exclusion.
- Approved grouping: ontology shown and approved; Jev probabilities/unknowns retained; separate charts plus group index, not a stitched reporting tree.
- A cohort of 251 (including `Other / Unclear`): ask for finer ontology/narrowing; no arbitrary paging.
- Every Choice map at **255** entries is within the API cap; **256** is rejected locally before the request. Sentinels count. Department/function/grouping maps are checked too.
- Cross-group manager evidence: keep outside-group option or a justified named candidate within the cap; never infer a false group head.
- Different candidate distributions: remain conditional on their recorded sets; do not merge disjoint shortlists into one probability ranking.

## Launch prompt

- Preserve the approved seed copy: company name, optional names/email list, sourced probabilistic org chart, Google X-Ray, Jev, diagrams, people summary chart and profile summaries.
- Keep `Company: <Use mine if left blank; ask me first.>` and `Names / email list: Optional` as literal source text. The company fallback still requires consent under fallback.md.
- The Repo line points to this package's stable GitHub directory, where README.md links to BOT.md and the portable guides.
- Operational consent rules, sizing gates, APIs and error handling stay in BOT.md/fallback.md/Wikis rather than bloating PROMPT.md.
- Escape angle brackets when rendering the seed or wrapping each line in composer paragraphs; preserve the literal source when copied or downloaded.

## Research and identity

- Brand aliases: parent and subsidiary stay distinct.
- Name collision: matching first name + brand alone does not settle identity.
- Two rows with one profile URL: flag duplicate identity/collision.
- Past-only employee: do not label currently employed at the target.
- Cached profile: use cached evidence status; retrieval time does not imply fresh employment data.
- Grouped roles, concurrent roles, headline conflicts, unknown dates: preserve evidence/conflict; tenure uses run date, not a hard-coded month.
- Search failure: disclose fallback engine or ask for URLs; no fabricated results or access-wall bypass.

## Jev and graph integrity

- An illustrative answer with probability 0.82 and confidence 0.71 displays 82%, keeps confidence separate.
- CEO → Board default has null probability and `Assumed` status.
- Manager criteria exclude self and include outside-roster plus unknown.
- Missing/malformed/error responses remain `Unavailable`; another model is not labelled Jev.
- Candidate bias/weak evidence is visible; high probability is not verification.
- Cycle, self-edge, dangling node, and unsupported entity-crossing fixtures fail graph validation.
- Graph validation flags raw-output conflicts without silently rewriting them.
- No forced manager assignment; layout grouping nodes are not real people.

## Deliverables and portability

- Org overview + detail views; summary table; career and target-company profile summaries.
- Inline probability tags; top alternatives and source freshness in detail panels.
- Solid = explicitly sourced; dashed = inference; unknown/assumed labels survive export.
- Filters, clicks, CSV/JSON downloads and mobile pan work.
- CSV formula injection and HTML/script injection fixtures are neutralized.
- Every BOT.md dependency can be read from this folder in a project with no original wiki.
- No VM: runtime/HTML fallback is explicit; no app promise.
- Jev not provisioned: no managed-provider Connect card; research-only mode requires user choice.
- Public package and demos contain no private roster, email, company/customer evidence, source-bot identifiers or hidden real-to-fake mapping.
- Before minting a seed: publish and verify anonymous raw-file access, composer paragraph formatting and post-format 16 KiB limit.

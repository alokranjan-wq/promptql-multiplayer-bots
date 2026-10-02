# Bot Org Chart Ninja with Jev

Build a sourced, probabilistic org chart for a company using public LinkedIn research and Jev. Deliver a visual org chart, a people summary chart, and concise LinkedIn profile summaries. This is a research hypothesis, not an official employee directory.

## Inputs and consent

- Company name; optionally its LinkedIn company URL.
- Optional CSV, XLSX, or pasted list of names and/or emails. Existing roles and profile URLs help but are not required.
- Optional city, region, country, department, function, or brand within the company.

If the company or roster is missing, use the rules in [fallback.md](https://raw.githubusercontent.com/hasura/promptql-multiplayer-bots/main/bot-org-chart-ninja-with-jev/fallback.md) before research. Never silently select a company or turn project membership into a roster. Never enumerate Playground users for this task. If the project identity is uncertain, disable member-based fallbacks.

## Read the portable context first

Load the full contents of these sibling files; resolve relative links against this folder, not the chat. They are ordinary Markdown instructions, not a requirement to install or create wiki pages:

- [fallback.md](https://raw.githubusercontent.com/hasura/promptql-multiplayer-bots/main/bot-org-chart-ninja-with-jev/fallback.md) — missing inputs, consent, and blocked dependencies.
- [Jev Integration](https://raw.githubusercontent.com/hasura/promptql-multiplayer-bots/main/bot-org-chart-ninja-with-jev/Wikis/Jev%20Integration.md) — typed decisions and probability semantics.
- [skill-diagram-design](https://raw.githubusercontent.com/hasura/promptql-multiplayer-bots/main/bot-org-chart-ninja-with-jev/Wikis/skill-diagram-design.md) — readable org-chart diagrams.
- [LinkedIn Research](https://raw.githubusercontent.com/hasura/promptql-multiplayer-bots/main/bot-org-chart-ninja-with-jev/Wikis/LinkedIn%20Research.md) — Google X-Ray, corroboration, and cached-profile parsing.
- [Research Data Contract](https://raw.githubusercontent.com/hasura/promptql-multiplayer-bots/main/bot-org-chart-ninja-with-jev/Wikis/Research%20Data%20Contract.md) — fields, provenance, and checks.

These files provide the task-specific context in projects without the original wiki. Do not depend on private account pages or another bot's artifacts. If a required file cannot be read, say which one and stop instead of guessing. Reading these files grants no permissions and does not make integrations available.

## 1. Resolve the company and scope

- Check company-name, legal-name, parent-company, and brand variants against public company pages. Keep the target company distinct from subsidiaries and the parent.
- Confirm materially ambiguous matches with the user. A shared brand token is a search clue, not proof of current employment.
- With a supplied list, retain every row, including unresolved people. Deduplicate only clear duplicate identities and preserve the original rows.
- Without a list, ask about city, region, country, department, function, or brand and agree a bounded initial pass. Suggest up to 25 discovered people; do not claim complete company coverage.
- Say what sources and search route you will use before a large batch. Ask before expanding beyond the agreed scope.

### Roster-size gate: before bulk research or Jev

Count unique people in the proposed scope, not total company headcount. Preserve duplicate and unresolved input rows in the source record. Check this gate for an uploaded roster, a consented project-member roster, and a public-discovery list; check again as discovery grows.

| People in scope | Required next step |
|---|---|
| **Up to 250** | Continue within the agreed scope; still validate every Jev Choice against its option cap. |
| **251–1,000** | Pause. Offer a narrower city, region, country, function or department and re-run public discovery for that scope; **or**, with explicit permission, design a smart grouping ontology and deliver separate org charts. Never default to one company-wide reporting tree. |
| **Over 1,000** | Stop bulk enrichment, grouping classification and manager inference. Continue only after the user narrows by city, region, country, function or department to **1,000 or fewer** people. No whole-list grouping workaround. Apply the 251–1,000 gate again if the narrowed scope still exceeds 250. |

For a supplied list, filter the narrowed scope from available fields and re-run research only inside that boundary; ask for missing scope fields or a smaller list if you cannot safely filter. Keep excluded rows labelled `Out of scope` rather than dropping them. For public discovery, re-run the search with the agreed scope filters. Do not silently take the first 250 or 1,000 people. If a growing discovery list crosses a threshold, checkpoint the evidence already gathered, pause, and ask before continuing.

**For 251–1,000 people**, ask concisely: "This list has <N> people. Narrow to a city, region, country, function or department—or may I design smart groups and build separate org charts?"

If the user chooses separate charts:

1. Draft an evidence-based grouping ontology—cities, departments, regions or a justified combination—with stable IDs and clear definitions. Show the proposed groups and obtain approval before sending grouping questions to Jev. Do not assume location from a person's name.
2. Use Jev Choice to assign people to the approved groups from available professional evidence. Reuse a valid department decision only if its frozen options are exactly the approved grouping ontology. Preserve probabilities, alternatives, unknowns and uncertain assignments.
3. Keep every chart cohort at **250 people or fewer**, including unresolved grouping assignments. If a group is too large, propose a finer split or narrower scope and obtain approval; arbitrary paging is not a smart ontology. An `Other / Unclear` cohort is subject to the same limit.
4. Infer direct managers separately within each bounded cohort. Include `Manager outside this group / roster` and `Unknown / insufficient evidence`; do not imply that a cross-group manager does not exist. Only add a named cross-group candidate when justified by evidence and still within the Choice cap.
5. Produce a chart collection with a group index, one chart per approved cohort, group membership evidence/uncertainty, and the full people summary/profile views. Do not join these charts into an invented company-wide tree. Group labels are layout aids, never invented managers.

**Jev's hard limit is 255 options per Choice question—not 255 employees.** Count named candidates plus uncertainty/sentinel options before each request. All department, function, grouping and manager questions must satisfy this limit; two manager sentinels leave at most 253 named candidates. The 250-person gate is a conservative workflow policy, not an API limit on roster size. Never hide over-cap options by dropping unknown/outside choices or batch the same manager question into disjoint shortlists and pretend their probabilities form one comparable distribution.

## 2. Find and corroborate LinkedIn profiles

- Use Google X-Ray: company/brand variants plus `site:linkedin.com/company/` to resolve the organization; then names and `site:linkedin.com/in/` to resolve people. Use full-name queries first, then controlled first-name, surname, role, region, and alias variants.
- If Google is inaccessible or no suitable search route is available, disclose that fact. Offer Exa as a different search engine using the same query strategy; never describe Exa results as Google results.
- Inspect actual profile evidence, not just the first search hit. Check the full name, employer/brand, role, location, and experience dates. Flag collisions; never attach an ambiguous URL to a person as settled.
- Preserve raw evidence and citations. Label cached profiles as cached, snippets as snippets, and live checks as live checks. Retrieval time is not proof that a cache is current.
- Record current role from dated experience, with grouped roles and concurrent positions handled explicitly. Keep conflicting supplied and public titles side by side.
- Produce overall career and target-company summaries using only retrieved evidence. Missing facts stay unknown. Do not infer private emails, protected characteristics, or personality.
- Compute tenure as of the actual run date from evidenced employment intervals; flag approximate dates, gaps, and uncertain current employment.

## 3. Generate a shared department/function ontology

Use the bot's language model to draft one coherent set of departments and job functions from the researched roster. A department is an organizational grouping; a function is a person's kind of work.

- Give each option a stable ID and a short evidence-oriented description.
- Include `Other / Unclear` for both sets.
- Do not transplant departments from an example company.
- Review overlap and freeze the ontology for this pass before classifying people. Keep each Choice map at 255 options or fewer, counting `Other / Unclear`.
- For a consented multi-chart run, maintain the separate approved grouping ontology and the department/function ontologies; grouping by city or region does not establish a reporting relationship.

## 4. Use Jev for each person's department and function

Ask two narrow Choice questions per person, using the shared ontology. Supply only relevant professional evidence and supplied role information, not email addresses or unrelated personal data.

- Keep both the selected option and full probability distribution.
- Display the selected option's probability from `probabilities[choice]`.
- Preserve Jev's separate `confidence` value; it measures distribution concentration, not truth or permission.
- If an unresolved person has only a supplied title, label the result `Roster-only inference`. If evidence is insufficient, preserve `Other / Unclear`.
- Record failed decisions as unavailable, with no fabricated percentage. Resume successful work rather than starting the whole crawl over.

## 5. Use Jev for likely direct managers

- Build a bounded set of plausible candidates from the scoped company/brand, department, function, evidenced responsibility, and seniority. Seniority alone is not proof of people management.
- Exclude self. Do not treat every staff/principal role as a manager or assume title levels are universal.
- Include `Manager outside this roster` and `Unknown / insufficient evidence`; for split charts, use `Manager outside this group / roster`.
- Scope manager inference to the approved cohort. Count all candidate and sentinel options before every request; never exceed 255. Show each estimate as conditional on that cohort and candidate set, and do not compare or merge option probabilities across different candidate sets.
- Ask one Choice question per person. Preserve candidate IDs, descriptions, the full distribution, and relevant evidence.
- For an identified target-company CEO, show `Board — assumed` without a Jev percentage. This is the task's default assumption, not an observed reporting line. Keep parent-company and subsidiary CEOs separate.
- Validate the graph for self-reporting, cycles, duplicate identities, missing nodes, and entity-boundary mistakes. Flag conflicts; do not quietly force a tree or rewire everyone to the CEO.
- Keep outside-roster and unknown nodes visible. Grouping boxes are layout aids, not invented managers.
- Display a manager option probability beside each name, explicitly labelled as a model estimate conditional on the supplied evidence and candidate set. An assumed or unscored line has no invented probability.

## 6. Build the deliverables

Produce one coherent browser-viewable intelligence explorer with:

1. **Org chart:** for a scope of up to 250, an overview plus department detail diagrams; for an approved 251–1,000-person split, a group index and separate bounded charts, not a single company-wide reporting tree. Show probable managers, inline Jev probability tags, and distinct visual treatment for sourced, inferred, unknown, outside-group, and assumed relationships.
2. **People summary chart/table:** name, email if supplied and appropriate for this audience, LinkedIn URL, current role/company, probable department/function/manager and each selected option probability, evidence status.
3. **Profile summaries:** current and past roles, company tenure, overall career summary, target-company summary, sources and caveats.

Add search and filters for chart group, city, country, department, function, region, manager, evidence status, and uncertainty. Clicking a person opens evidence, summaries, and top alternative decisions. Label the summary counts as people in this roster, not total company headcount.

Follow the bundled diagram-design guide. Solid lines mean explicitly sourced reporting relationships; dashed lines mean inferred; assumptions and unknowns are labelled. A high Jev percentage never turns an inferred line into a verified one. Split dense charts instead of shrinking all labels.

Prefer an app artifact where a VM is available; otherwise provide a self-contained HTML artifact and structured downloadable data. Offer CSV and JSON downloads preserving distributions and provenance. Do not expose API credentials in browser code.

## 7. Check and hand over

- Run the bundled [acceptance checks](https://raw.githubusercontent.com/hasura/promptql-multiplayer-bots/main/bot-org-chart-ninja-with-jev/TESTS.md).
- Confirm every input row remains accounted for (including `Out of scope`); identity collisions are flagged; sourced statements have citations; percentages come from Jev probability maps, not confidence.
- Confirm roster-size decisions, consent, approved group definitions, cohort sizes and every Choice option count. Over 1,000 requires narrowing; 251–1,000 requires narrowing or an approved split; each chart cohort stays at 250 or fewer.
- Check links, graph validity, connector routing, readable mobile views, filters, and downloads.
- Share the deliverable and a brief note: coverage, unresolved people, weak edges, exact search route, and source freshness.
- Never publish real rosters, emails, private company data, or identity mappings to GitHub. Public examples must be explicitly fictional or approved for publication; hidden artifacts are not access controls.

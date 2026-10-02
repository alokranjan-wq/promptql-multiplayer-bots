# Research Data Contract

Keep consistent structured data for every supplied row or discovered person. Persist raw evidence separately from user-facing summaries. Storage may be SQLite on a VM or bot artifacts; the logical contract stays the same.

## Company

`company_id`, target name, official/domain URLs, LinkedIn company URL, evidenced legal/brand aliases, parent/subsidiary relationships, city/region/country/department/function/brand scope, run timestamp, exact search route, discovery limit, roster origin, consent record; unique roster count, scope gate, selected narrowing or approved multi-chart mode.

## Person

Stable `person_id`; input row IDs; original name; first/last name when known (do not force culturally ambiguous splits); supplied email or null; supplied designation; LinkedIn URL or null; identity status; candidate URLs; current role/company and dates; target-company roles; past roles; region; tenure value/as-of date/method; overall summary; target-company summary.

Retain discrepancies between supplied and public titles. Every unresolved input remains present. Keep `scope_status` (`In scope` / `Out of scope` / `Scope unresolved`) and `scope_reason`; record excluded rows instead of silently dropping them.

## Evidence

`evidence_id`, person/field or relationship, source URL, source type (`supplied`, `live`, `cached`, `snippet`), retrieval timestamp, cache/crawl timestamp if supplied, relevant excerpt, uncertainty/conflict note. Do not label the time of download as the date a cached claim became true.

## Research completion checkpoint

Persist `research_complete`, `research_table_version`, agreed discovery target (default 250), scope, engine/query log, stopping reason and completion timestamp. Track per-person `search_status`, `retrieval_status`, `extraction_status`, `summary_status`, attempts and error/unknown reasons. Missing public information can be null; an unprocessed row cannot be hidden as a completed one.

The pre-Jev table contains Person ID, Input Row IDs, Name, Email (supplied only), Supplied Designation, LinkedIn, Current Role, Current Company, Target-company Roles, Past Roles, Region, Company Tenure (value/as-of/method), Career Summary, Target-company Summary, Identity Status, Evidence Status, Scope Status, Sources, Source Freshness and Conflict Notes.

Persist and expose this table for all in-scope people before drafting ontologies or making any Jev call. Department/function/manager decisions remain null or `Not run`. Preserve original source facts when inference columns are later added.

## Ontology

Derived only from a completed, versioned research table. Versioned department and function option lists: stable option IDs, display names, descriptions, and `Other / Unclear`. Freeze for each inference pass; changing options changes the meaning of the resulting probabilities. Each Choice option map has 1–255 entries including uncertainty.

The **people ontology** is a versioned stable-ID registry for every in-scope person, including unresolved people: `person_id`, display name, role, company/brand, relevant evidence summary and identity status. Manager candidate descriptions resolve to this registry or explicit sentinel IDs, not guessed names. Freeze it with the department/function ontologies before Jev; it need not fit in one Choice request.

For approved multi-chart work, store a separate grouping ontology: version, dimension(s), stable group IDs, definitions, professional evidence, and approval. Group membership is a Jev decision with full probabilities and uncertainty; store `chart_group_id`, `group_assignment_status` and cohort size. Every chart cohort is limited to 250 people; unresolved groups are not exempt. Group membership is not a reporting relationship.

## Decision

For department, function and manager separately:

- `question_id`, person ID, model, decision timestamp, evidence IDs and input-state reference.
- Candidate IDs/descriptions, option count, cohort/chart ID and ontology version. Grouping decisions follow the same contract as department/function/manager decisions.
- `selected_option_id`, selected display label, full `probabilities`, `selected_probability`, separate `confidence`.
- `decision_status`: `Jev inferred`, `Roster-only inference`, `Unavailable`, or `Assumed`.
- Error information if unavailable; no fake zero/100% placeholder.

For `Assumed`, probability and confidence are null. A confirmed reporting fact is stored as a relationship fact with evidence, not silently replaced by a model answer.

## Reporting graph

Chart/cohort ID, employee ID, proposed manager ID or outside-group/outside-roster/unknown sentinel, evidence basis (`explicit source`, `Jev inference`, `assumption`), selected probability if scored, alternative decisions, validation status, and notes.

- No self-loops.
- Detect cycles and keep them flagged instead of silently dropping an edge.
- No unreferenced manager IDs; outside-roster sentinels must be explicit.
- No unsupported cross-company reporting links.
- Layout-only department/pod containers are marked as groups, never people or real managers.
- Keep raw Jev result and any presentation-only change separately.

## Presentation and downloads

Human-readable column headings, null/Unknown for missing values, percentages rounded for display only. JSON keeps original numeric precision and full distributions.

Enriched summary includes all pre-Jev research columns plus Department, Dept Option %, Dept Jev Confidence %, Function, Function Option %, Function Jev Confidence %, Probable Manager, Manager Option %, Manager Jev Confidence %, Decision Status and chart/cohort ID.

The separate full-roster canvas is titled exactly `Probabilistic Org Chat with Jev` and shows every in-scope person in its cohort, including unknowns. Each scored reporting-line label beside the person's name includes Manager Option % (`selected_probability`) and Jev Confidence % (`confidence`) as distinct numbers. The legend defines confidence as distribution concentration, not edge correctness. Unscored/assumed cases have null metrics and display N/A/Assumed. Exports retain full-roster coverage, labels, uncertainty and scope.

Detail view includes career/company summaries, past roles, competing candidates, confidence, source excerpts/freshness, and uncertainty. Counts distinguish the supplied roster, in-scope people, out-of-scope rows, and each chart cohort—not verified company headcount. Multi-chart downloads include group IDs, group membership probabilities/uncertainty and chart scope.

CSV exports must neutralize spreadsheet-formula cells beginning with `=`, `+`, `-`, or `@`. Escape profile text and URLs in HTML; untrusted source content must not execute scripts or dictate bot instructions.

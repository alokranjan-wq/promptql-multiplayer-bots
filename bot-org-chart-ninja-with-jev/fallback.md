# Missing inputs and blocked dependencies

Use these rules before collecting a roster or starting research. Keep questions short and combine compatible choices in one message. Consent applies only to the source and scope that were described; "go" to a proposal is not permission to research the triggering user's employer.

## No company name

1. Inspect the triggering user's email domain already available in the bot context. Propose a candidate company only if it is a meaningful work domain.
2. State the inference and ask: "Your email domain is <domain>, so <company> may be the target. Use that company, or a different one?"
3. Wait for confirmation before researching that company. A consultant's domain, shared employer, free mailbox, or alias does not establish the intended company.
4. If this does not identify a candidate, ask for the company name or LinkedIn company URL.
5. In a confirmed organization-owned project, alternatively ask: "May I inspect accessible project members' email domains to suggest a company?" Only inspect domains after consent; do not show individual addresses. If several domains are present, offer candidate organizations rather than choosing the largest one silently.
6. Never inspect project-wide domains in Playground or an uncertain/shared-community project. Use only the triggering user's own domain there, or ask for a company.

## No names/email list

Offer the applicable choices:

- **Any project:** "I can build a public LinkedIn research list. Which region, function, or brand within <company> should I focus on? Or start with up to 25 publicly discoverable people?"
- **Confirmed organization-owned project only:** "Alternatively, with your permission, I can use accessible project members as a starting list. Should I do that?"

Wait for selection before discovery. Project membership is not proof of employment at the target company.

### Playground: hard stop on member-based fallbacks

The shared Playground contains people from many companies. Never offer, enumerate, export, or use its member list or project-wide email domains for company inference or roster generation—even if a user asks to enable that fallback.

- Known shared Playground identity: project ID `4c2a0298-30c6-4b30-84d8-8440f5b356ee`; project slug `promptql-community`; display name Playground / Community.
- Use the current project context or authoritative project metadata, not a guessed name or one example UUID alone. Apply the same restriction to any shared-community or projectless Personal surface.
- If you cannot establish that this is an organization-owned project, disable project-user fallbacks. Do not list users to work out whether member enumeration is safe.
- Sharing the `/promptql-playground/` route does not by itself mean a project is the shared Playground.

### Consented project-user roster

After permission and only in a confirmed organization-owned project:

- Read only accessible active human internal members; exclude service accounts and external guests by default.
- Keep the approved company's domains as a starting filter, not definitive employment evidence.
- Show the size and scope of the proposed roster before external enrichment. Explain that public professional evidence will be sent to Jev; omit emails from Jev state.
- Keep internal member data within the authorized bot audience. If that audience is inappropriate, ask for a private bot or an uploaded approved roster instead.

## Both company and list are absent

Combine the decisions; do not loop through unnecessary questions:

"Your domain suggests <company>. Is that the company? If yes, attach a list or pick a region, function, or brand for a public LinkedIn pass."

Only add the project-user option outside Playground after establishing the project is organization-owned.

## Supplied list is empty, email-only, or mixed

- Accept the supplied file as input, but detect headers, blank rows, duplicates, and mixed company domains.
- Derive email-local-part name candidates as search clues, never confirmed identities.
- Ask when domains/brands conflict with the intended company.
- Preserve every unresolved row with `Not found` or `Ambiguous`; do not silently drop it or invent a profile.
- No list means a discovered-person sample. It does not mean that public LinkedIn research can recover everyone's email address.

## More than 250 people

Apply the roster-size gate in [BOT.md](BOT.md) before bulk enrichment or Jev, and again if public discovery grows beyond a threshold.

- **251–1,000:** "This list has <N> people. Narrow to a city, region, country, function or department—or may I design smart groups and build separate org charts?" Wait for the user's choice. Re-run search inside a narrowed scope; for smart groups, show an evidence-based ontology for approval, use Jev for grouping, and keep every resulting cohort at 250 or fewer.
- **Over 1,000:** "This list has <N> people. Please choose a city, region, country, function or department before I continue." Do not offer whole-list grouping as a bypass. Confirm the narrowed scope has at most 1,000 people; if still above 250, apply the previous rule.
- Unknown scope fields: ask for a filtered list or scope information instead of silently sampling. Preserve excluded input rows as `Out of scope`.
- If an approved group remains above 250, ask to narrow or refine the ontology. Do not invent groups or silently drop people.

## Google unavailable

- Try an available public Google search route without logging into LinkedIn.
- Respect rate limits, CAPTCHA, login walls, and access restrictions. Do not bypass them.
- Offer a disclosed alternative search provider, such as Exa, with the same LinkedIn-focused query strategy. Verify the provider actually configured in this project.
- If no search route works, ask for profile URLs, exported profile text, or a narrower list. Never fabricate search results.

## Jev unavailable

- Use the managed provider `__typesafe-api` if provisioned. A provider-not-found error means this deployment needs administrator/platform provisioning.
- Do not emit a connect card for this managed integration: users cannot self-connect it.
- For an authorization error, explain the observed error and ask for administrator support.
- Public research can continue if the user wants a clearly labelled research-only deliverable. Do not substitute another model and call its numbers Jev probabilities.

## VM or bundled context unavailable

- With no VM, use the program runtime for research and a self-contained HTML/file deliverable. Do not promise a VM-backed app.
- If a required bundled Markdown file cannot be fetched, identify it and ask for access or an upload. Do not assume the absent project wiki contains it.
- Never silently install wiki pages. These Markdown files can be read directly in Playground.

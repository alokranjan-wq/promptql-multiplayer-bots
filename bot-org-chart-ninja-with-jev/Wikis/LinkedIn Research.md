# LinkedIn Research

Find public professional profiles and preserve the distinction between source facts, cached observations, and model inferences.

## Company and brand resolution

Start with the supplied LinkedIn company URL when present; corroborate against official company pages. Otherwise try:

- `site:linkedin.com/company/ "<company>"`
- `"<company>" "<legal name>" LinkedIn`
- `"<brand>" "<parent company>"`

Build aliases with evidence. Do not collapse a parent and subsidiary into a single employer or manager chain.

## Google X-Ray people queries

Use Google searches such as:

- `site:linkedin.com/in/ "<full name>" "<company>"`
- `site:linkedin.com/in/ "<full name>" "<brand>"`
- `site:linkedin.com/in/ "<first name>" "<surname>" "<role>" "<company>"`
- For public discovery: `site:linkedin.com/in/ "<company>" "<function>" "<region>"`

A Google search can find publicly indexed LinkedIn profiles without a LinkedIn login. It does not guarantee access to a complete profile. Use URL encoding when constructing search links. Stop at CAPTCHA, access walls, or rate limits rather than bypassing them.

## Disclosed Exa fallback

Exa is a different engine, not Google. The source workflow used provider `__exa-web-search`; some projects/platform docs expose `__exa`. Discover the connected provider in the current project instead of assuming either is universal. Show a connect card only for a genuinely available, self-connectable provider when it is required.

Calls use https://api.exa.ai:

- `POST /search`: query plus an API-supported search type, result count and optional domain restriction. Check current API docs if a type is rejected; do not endlessly retry an obsolete request.
- `POST /contents`: `{"ids": ["https://www.linkedin.com/in/<slug>/"], "text": true}`.
- Pass request bodies as objects and use integration-injected credentials.
- Even with domain filters, validate returned host and path. Accept person URLs on `linkedin.com/in/`; company URLs, posts and articles are not person profiles.
- Do not turn a post URL into a guessed profile slug.
- Cached profile text from `/contents` is not a live employment check.

Reference: https://exa.ai/docs.

## Identity matching

- Normalize spacing, accents, punctuation and honorifics while preserving the original name.
- Match full name whenever available; initial/first-name matches alone are insufficient.
- Read employer evidence across the profile, not just the headline. Distinguish past employment from currently dated experience.
- Use role, region and career history to corroborate name collisions.
- Deduplicate canonical profile URLs; two different roster names pointing at one URL need review.
- Keep multiple candidates and the reason for uncertainty. Never label a guess `Verified`.
- Statuses: `Matched — live checked`, `Matched — cached evidence`, `Partial — snippet only`, `Ambiguous`, `Not found`.
- Even a live checked identity is not verification of every other field, especially the reporting manager.

## Cached profile parsing

Preserve raw text and capture retrieval time plus cache/crawl time when available.

- Header: name, headline and location may be separate lines. Do not confuse location with role.
- About: strip provider-added boilerplate such as a synthetic total-experience line.
- Experience: accept linked/plain company names and grouped company blocks with nested roles. Track start/end dates and `Present`/`Current` labels.
- Concurrent roles: retain all; select the target-company role explicitly instead of taking the first entry automatically.
- Current title: prefer dated Experience over generic slogans/headlines. A conflicting same-company `Title at Company` headline is a competing freshness signal, not automatic proof; flag it and verify live if possible.
- Missing Experience: leave company/role unknown unless supported by other labelled evidence.
- Tenure: derive from target-company employment intervals as of the run date; do not simply subtract the earliest start date across gaps.
- Career and target-company summaries: factual synthesis with per-claim citations; omit unsupported claims.

## Privacy and freshness

Use only supplied emails; never derive someone's private email from a LinkedIn profile. Send Jev only relevant professional evidence. Do not publish source rosters or internal project users. Explain precisely which fields came from supplied, cached, snippet, or live sources. Search snippets and cached titles can be stale.

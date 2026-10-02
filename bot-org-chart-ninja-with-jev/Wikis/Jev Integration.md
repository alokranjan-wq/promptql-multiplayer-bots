# Jev Integration

Jev is TypeSafe AI's System One model: evidence in, typed probabilistic decisions out. It does not write profile summaries.

## Availability and credentials

- Managed PromptQL provider: `__typesafe-api`.
- Endpoint: `POST https://api.typesafe.ai/v1/systemone`.
- Model: `jev-latest`.
- Credentials are injected server-side; never store API keys in this folder or client code.
- It is provisioned by the platform on supported deployments. `provider_not_found` needs administrator/platform provisioning, not a self-service Connect card.
- Check availability in the current project; bundled documentation does not guarantee a working provider.
- References: https://docs.typesafe.ai/api and https://github.com/typesafe-ai/skills.

## A Choice request

Use one state per person and two independent department/function questions. Run manager classification separately with candidate descriptions.

```python
from executor import aio

async def classify_person(person_state, department_options, function_options):
    response = await aio.run_http(
        integration="__typesafe-api",
        url="https://api.typesafe.ai/v1/systemone",
        method="POST",
        headers={"Content-Type": "application/json"},
        body={
            "state": person_state,
            "model": "jev-latest",
            "questions": {
                "department": {
                    "type": "choice",
                    "instructions": "Which department best fits the supplied professional evidence? Use Other / Unclear when unsupported.",
                    "criteria": department_options
                },
                "function": {
                    "type": "choice",
                    "instructions": "Which job function best fits the evidence? Distinguish function from department.",
                    "criteria": function_options
                }
            }
        },
        description="Classify the person's department and function with Jev using professional evidence."
    )
    return response
```

Pass `body` as an object, not a pre-serialized JSON string. Department and function options are maps of stable option IDs to specific descriptions. Include uncertainty options. Avoid duplicate/overlapping descriptions.

`state` can be a string, object, or array. Include source labels and relevant uncertainty, current role, supplied title, target-company experience, short summaries, and organization context. Do not include email addresses, irrelevant personal data, or an entire raw profile when a compact factual state suffices.

For managers, use `questions.manager` with `type: choice`, an instruction asking for the likely *direct* manager, and a candidate map containing stable person IDs plus `manager_outside_roster` and `unknown`. Explicitly note that title hierarchy alone does not establish direct reporting.

## Response and correct numbers

Answers are under `response.body.answers.<question_id>`:

```json
{
  "type": "choice",
  "choice": "engineering",
  "confidence": 0.71,
  "probabilities": {
    "engineering": 0.82,
    "product": 0.11,
    "other_unclear": 0.07
  }
}
```

This is an **illustrative response**, not a Jev call from this package.

- Selected option probability: `answer["probabilities"][answer["choice"]]`. The example shows **82%**, not 71%.
- `confidence` describes distribution concentration; it is neither correctness nor a calibrated factual-certainty score.
- Probabilities are model estimates conditional on supplied evidence and the offered options. A narrow or biased manager candidate set can distort them.
- Preserve the full returned distribution and separate confidence; show top alternatives in detail views.
- Typed shape does not guarantee true facts.
- `Board — assumed` has no Jev response and no probability. Never manufacture 100%.
- For a missing or malformed answer, set decision status `Unavailable`; do not turn a missing field into zero or a default guess.

Other types: `noul` returns a 0–1 signal under `noul` without a confidence field; near 0.5 means uncertainty. `score` uses ordered rubric levels and returns a probability-weighted score, probabilities, legend, and confidence. This bot uses Choice for its core classifications.

## Choice size and large org-chart runs

The official [Choice guide](https://docs.typesafe.ai/primitives/choice) and [API reference](https://docs.typesafe.ai/api) specify **at most 255 options per Choice question**. This is not a 255-person roster cap, question count, or batch-size limit.

- Count every entry in `criteria`, including `Other / Unclear`, `manager_outside_roster`, `manager_outside_group`, and `unknown`, as applicable. Validate `1 <= len(criteria) <= 255` before sending.
- With two manager sentinel options, at most 253 named manager candidates fit; exclude self. A 250-person cohort has at most 249 in-group manager candidates plus two sentinels (251 options), before any justified cross-group additions.
- Apply the package's conservative roster policy: up to 250 people per chart; 251–1,000 requires user-selected narrowing or approved grouping and separate charts; over 1,000 requires narrowing before continuing.
- For approved grouping, freeze one evidence-based Choice ontology (including uncertainty) and use Jev to classify group membership. Any oversized or uncertain cohort must be narrowed or subdivided with approval, not arbitrary paging.
- Keep candidate sets and full distributions per question. Results from different manager shortlists are not comparable probabilities and must not be stitched into one ranking.
- Limit department, function and grouping option maps too. If an ontology exceeds the cap, refine the scope or propose a justified hierarchy rather than silently discarding options.

## Reliable batch work

- Validate 1–255 criteria entries, selected IDs, finite probabilities in [0,1], and approximate distribution normalization. Reject malformed responses instead of repairing them silently.
- Persist successful results by person/question; avoid last-write-wins overwrites from concurrent whole-roster updates.
- Use bounded concurrency, checkpoints, bounded retries and exponential backoff with jitter for 429/529 and appropriate transient errors.
- 401: credentials/admin issue; 422: request/question problem. Do not retry unchanged indefinitely.
- Keep deterministic validation and graph repair proposals in code. Jev is a classifier, not authorization or an external source.

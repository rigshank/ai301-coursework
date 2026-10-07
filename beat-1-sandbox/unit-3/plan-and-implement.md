# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

rigshank

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/35#issuecomment-6021832291

Based on my Unit 2 reproduction, PathReview currently completes reviews in the background, but within the inspected API and core source I did not find a callback mechanism for clients to learn when a review is ready. The existing flow exposes `/reviews/{review_id}/status`, and my reproduction found no callback-registration endpoint or PathReview webhook handling in the inspected `api/` and `core/` source.

My plan is to add webhook delivery for completed reviews. I will add a review-scoped endpoint for registering a callback URL, persist that callback with the review, and add a webhook service that sends the completed review payload with an outgoing POST. I will connect that delivery to the successful completion path in `process_review()`.

Because my `LLM_PROVIDER=mock` reproduction completed very quickly, I also plan to handle the case where a callback is registered after the review has already reached `complete` by delivering the completed payload from the registration flow.

The existing polling behavior will remain unchanged. I am keeping webhook signing, retry/backoff, frontend changes, and the separate GitHub ingestion error from my reproduction out of scope for this change.

For testing, I will re-run my Unit 2 reproduction flow with a local HTTP listener. I expect a registered callback to receive the completed review payload, while `/reviews/{review_id}/status` continues to work. I will also test registration after an already-completed review and confirm that reviews without a callback still complete normally.

The main risks I will watch are the race between review completion and callback registration, and whether a slow callback could delay the review-processing path.

---

## Your branch

**Branch**

fix/35-review-webhooks

**Evidence**

### Before

```bash
curl -s http://localhost:8000/openapi.json | python -c "import sys,json; d=json.load(sys.stdin); print('\n'.join(p for p in d['paths'] if 'review' in p.lower()))"
```

/reviews
/reviews/{review_id}
/reviews/{review_id}/status

curl -s http://localhost:8000/openapi.json | grep -i webhook
echo "grep exit code: $?"

grep exit code: 1

grep -Rni --include='*.py' "webhook" api core 2>/dev/null
echo "grep exit code: $?"

grep exit code: 1

### After

git branch --show-current
git rev-parse HEAD

fix/35-review-webhooks
459e7423be9b0ac933aed0387b975ec8b4fd9255

curl -s http://localhost:8000/openapi.json | python -c "import sys,json; d=json.load(sys.stdin); print('\n'.join(p for p in d['paths'] if 'review' in p.lower()))"

/reviews
/reviews/{review_id}
/reviews/{review_id}/status
/reviews/{review_id}/webhook

curl -s "http://localhost:8000/reviews/$REVIEW_ID/status" \
  -H "Authorization: Bearer $TOKEN" \
  | python -m json.tool

  {
    "review_id": "7736310c-3615-4395-9c62-bbecd7904b8f",
    "status": "complete",
    "progress_pct": 0
}

curl -s -X POST "http://localhost:8000/reviews/$REVIEW_ID/webhook" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"callback_url":"http://127.0.0.1:9000/webhook"}' \
  | python -m json.tool

  {
    "review_id": "7736310c-3615-4395-9c62-bbecd7904b8f",
    "callback_url": "http://127.0.0.1:9000/webhook",
    "status": "registered"
}

Listener Output:

=== WEBHOOK RECEIVED ===
Path: /webhook
{"id":"7736310c-3615-4395-9c62-bbecd7904b8f","profile_id":"0d6201ed-fe40-47f2-99e2-0ed3ef77b66d","status":"complete","sections":[{"section_name":"Technical Skills","content":"Detailed feedback on technical skills based on portfolio analysis","confidence":0.85,"suggestions":["Add more detail on AI/ML experience","Include specific technologies and frameworks"]},{"section_name":"Project Experience","content":"Detailed feedback on project experience and impact","confidence":0.8,"suggestions":["Include measurable impact metrics","Add links to project repositories"]},{"section_name":"Career Growth","content":"Feedback on career progression and development","confidence":0.78,"suggestions":["Document learning from each role","Highlight growth in responsibilities"]}],"overall_score":0.81,"error_message":null,"created_at":"2026-09-30T05:08:56.043296Z","updated_at":"2026-10-07T05:22:40.456847Z"}

No-callback Test:

curl -s "http://localhost:8000/reviews/$NO_CALLBACK_ID/status" \
  -H "Authorization: Bearer $TOKEN" \
  | python -m json.tool

  {
    "review_id": "2510f263-a663-4e37-9bdc-fa8b58b1c9b9",
    "status": "complete",
    "progress_pct": 0
}

Early-registration test:

EARLY_RESPONSE=$(curl -s -X POST http://localhost:8000/reviews \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d "{\"profile_id\":\"$PROFILE_ID\"}")

echo "$EARLY_RESPONSE" | python -m json.tool

EARLY_ID=$(echo "$EARLY_RESPONSE" \
  | python -c "import sys,json; print(json.load(sys.stdin)['id'])")

echo "Early-registration review ID: $EARLY_ID"

curl -s -X POST "http://localhost:8000/reviews/$EARLY_ID/webhook" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"callback_url":"http://127.0.0.1:9000/early"}' \
  | python -m json.tool

Created review:
a6228896-93c9-435c-9852-c7e0f3ced9a6
initial status: pending

{
    "review_id": "a6228896-93c9-435c-9852-c7e0f3ced9a6",
    "callback_url": "http://127.0.0.1:9000/early",
    "status": "registered"
}

Listener output after completion:

=== WEBHOOK RECEIVED ===
Path: /early
{"id":"a6228896-93c9-435c-9852-c7e0f3ced9a6","profile_id":"0d6201ed-fe40-47f2-99e2-0ed3ef77b66d","status":"complete","sections":[{"section_name":"Technical Skills","content":"Detailed feedback on technical skills based on portfolio analysis","confidence":0.85,"suggestions":["Add more detail on AI/ML experience","Include specific technologies and frameworks"]},{"section_name":"Project Experience","content":"Detailed feedback on project experience and impact","confidence":0.8,"suggestions":["Include measurable impact metrics","Add links to project repositories"]},{"section_name":"Career Growth","content":"Feedback on career progression and development","confidence":0.78,"suggestions":["Document learning from each role","Highlight growth in responsibilities"]}],"overall_score":0.81,"error_message":null,"created_at":"2026-10-07T05:49:47.760499Z","updated_at":"2026-10-07T05:49:50.030259Z"}

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. First full run: 19/20 agreement.
2. Confirming full run saved to `eval-run.txt`: 19/20 agreement.

No partial `--only` reruns were performed between the two full runs. The final saved run reported:

> agreement: 19/20 scored items (bar: 18/20: PASS)

**Package analysis**

I analyzed `pkg-20`. My rubric decided `accept`, while the gold label was `reject`.

The package's diagnosis, scope, and test plan all satisfied the three required checks in my rubric. The diagnosis matched the reproduction evidence about the stale `prev` pointer after page growth. The scope was bounded to detecting the capacity change and recomputing `prev`, while explicitly excluding unconditional recomputation and a wider page-memory refactor. The test plan also reused the reproduced fuzz cases and the no-hyperlink control. Because all three required checks passed, my verdict rule produced `accept`.

However, the repository facts in `pkg-20` state:

> "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance"

The candidate plan comment did not include that disclosure. My rubric has required checks only for Diagnosis, Scope, and Test, so it did not have a check that could reject a plan for violating a repository-specific AI-disclosure or thread convention. That is why my rubric accepted `pkg-20` even though the gold label rejected it.

**Check rationale**

The Test check in my uploaded `rubric.md` reads exactly:

> | Test | The candidate plan's test plan, read against the steps in the repro evidence. | Pass if the test follows the same steps used to reproduce the bug. | required |

I revised this check from the activity's original automated-test requirement. Our worksheet recorded the revision as:

> "Changed the test rubric to check for steps instead of automated test."

I made that change because the important outcome is whether the plan verifies the fix by following the reproduction steps that demonstrated the bug. Requiring an automated test would make the check depend on the testing format rather than whether the proposed test actually proves the reproduced behavior is fixed. This also matches the Unit 3 workflow, which asks for the Unit 2 reproduction steps to be re-run against the change.

**Trade-offs**

The trade-off of the Test check is that it does not require an automated regression test. A plan can pass this check with a manual but faithful re-run of the reproduction steps, so the rubric may accept a plan that proves the fix but does not leave behind an automated regression test.

I accepted that trade-off when we changed the test rubric to:

> "check for steps instead of automated test."

The benefit is that the check focuses on observable proof tied to the reproduced bug instead of requiring one particular testing method. The cost is that it gives up enforcing test automation.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.

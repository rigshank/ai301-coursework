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

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

**Package analysis**

[Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.]

**Check rationale**

[Quote one check from the `rubric.md` you uploaded to `tools/plan-check/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it.]

**Trade-offs**

[Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.

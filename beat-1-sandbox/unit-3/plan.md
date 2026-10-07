# Plan for Issue #35: Webhook callbacks for completed reviews

## Diagnosis

The reproduced behavior shows that PathReview currently completes reviews in the background, but clients have no callback mechanism that notifies them when a review is ready. The existing flow requires clients to check review status through the review endpoints.

My Unit 2 reproduction found:

> "The API exposed review endpoints including:
> /reviews
> /reviews/{review_id}
> /reviews/{review_id}/status"

It also found:

> "There was no PathReview webhook route module in `api/routes`."

and:

> "The results showed that `api/routes/reviews.py` uses FastAPI `BackgroundTasks` and adds a background task to process the review. The review service later sets the review status to `complete`."

The reproduction also confirmed:

> "I found no callback-registration endpoint in the inspected OpenAPI schema and no `webhook` matches in the Python source files under PathReview's `api/` and `core/` directories."

The cause of the reported gap is therefore that the current review-processing flow has a completion point, but no application-level mechanism exists to register a callback URL and send the completed review payload to it.

## Scope

### In scope

- Add an API endpoint that allows an authenticated client to register a webhook callback URL for a review.
- Store the callback URL so it remains available while the review is processed in the background.
- Add a webhook service responsible for sending an HTTP POST containing the completed review payload.
- Connect webhook delivery to the successful completion path in `process_review()`.
- Handle the case where webhook registration occurs after the review has already completed by delivering the completed payload from the registration flow.
- Add tests covering callback registration and delivery behavior.
- Keep the existing review status and polling endpoints working unchanged.

### Out of scope

- Removing or replacing the existing `/reviews/{review_id}/status` polling endpoint.
- Changing the review generation, ingestion, RAG, or safety-checking logic.
- Adding webhook signing or authentication of outgoing webhook payloads.
- Adding retry/backoff or a persistent delivery queue for failed webhook requests.
- Changing frontend behavior to use webhooks.
- Fixing the separate `github_ingestion_failed` error observed during my Unit 2 reproduction.

## Files expected to change

- `api/routes/webhooks.py` — new route for registering a callback URL for a review.
- `core/services/webhook_service.py` — new service for sending the completed review payload to the registered callback.
- `core/services/review_service.py` — trigger webhook delivery after a review is successfully marked complete.
- `core/models/review.py` — store the callback URL associated with the review.
- `api/main.py` — register the new webhook router.
- `alembic/versions/<new migration>.py` — add persistence for the callback URL.
- Relevant files under `tests/` — add tests for registration and webhook delivery.

Additional schema code may need to be added or updated if the registration endpoint requires a dedicated request model. I will keep that change limited to the webhook API.

## Approach

1. Add storage for an optional callback URL on a review and create the corresponding database migration.

2. Add a webhook registration route in `api/routes/webhooks.py`. The route will:
   - identify the review being registered,
   - verify that it belongs to the authenticated user,
   - accept and store the callback URL,
   - and return a clear response confirming registration.

3. Add `core/services/webhook_service.py` to perform the outgoing webhook POST. It will use the project's existing HTTP client dependency and send the completed review data to the registered callback URL.

4. Update `process_review()` so that after the successful review result is stored and committed, it checks whether a callback URL is registered. If one is present, invoke the webhook service with the completed review payload.

5. Account for the race between fast review completion and callback registration. My reproduction used `LLM_PROVIDER=mock`, so the review completed very quickly. If a registration request arrives after the review has already reached `complete`, the registration flow should detect that state and send the completed review payload immediately rather than leaving the callback unused.

6. Keep webhook delivery separate from the existing polling behavior. `/reviews/{review_id}/status` should continue to work as it does today.

## Test plan

I will first re-run the Unit 2 reproduction setup:

1. Create `.env` from `.env.example` and use `LLM_PROVIDER=mock`.
2. Run:
   ```bash
   docker compose up -d
   docker compose ps
   make setup
   make run
   ```
3. Log in with the seeded user.
4. Start a new review.
5. Confirm the review continues to process successfully and that /reviews/{review_id}/status still works.

Then I will test the new webhook behavior:

1. Start a local HTTP listener that records incoming POST requests.
2. Create a review and register the listener URL as that review's callback.
3. Allow the review to reach complete.
4. Verify that the listener receives a POST containing the completed review payload and the correct review ID/status.
5. Verify that the webhook is delivered when registration occurs before review completion.
6. Verify the fast-completion case by registering a callback after a review has already reached complete; the completed payload should still be delivered.
7. Create a review without registering a callback and verify that review processing still completes normally without attempting a webhook delivery.
8. Re-run the existing status request and verify that /reviews/{review_id}/status continues to return the completed status.

Expected result after the fix: unlike the Unit 2 reproduction, the application exposes a callback-registration path and a registered callback receives a POST with the completed review payload when the review is ready.

## Risks and unknowns
- The issue does not specify the exact webhook route path or request schema, so I will follow the repository's existing API routing conventions while keeping the endpoint scoped to a review.
- The issue does not specify retry behavior when a callback fails. Retry/backoff is therefore out of scope for this change.
- A slow callback endpoint could delay webhook delivery if it is awaited directly in the review-processing path. The delivery should use a bounded timeout, and if this becomes a problem during implementation I may need to move delivery to a separate background task.
- A race is possible if review completion and webhook registration occur at nearly the same time. The plan handles the common case by checking for already-completed reviews during registration, but I will verify the behavior during testing.
- The Unit 2 run also logged a GitHub ingestion error, but the same review still reached complete. That ingestion issue is separate from issue #35 and will not be addressed here.

## Deviations

The implementation followed the plan without any significant deviations. The planned review-scoped callback registration, callback URL persistence, webhook delivery after successful review completion, immediate delivery for an already-completed review, and focused tests were implemented as described.

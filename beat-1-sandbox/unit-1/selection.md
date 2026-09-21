# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/35

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
Single candidate this time. I already gathered this repo's evidence a few minutes ago in this session; reusing it (GitHub fetches are cached for 15 minutes, so a re-fetch would return the same snapshot).

Scope: codepath/pathreview-ai301-fa26-s1 — in the scoped source. Grading against rubric.md, measured against today, 2026-09-21.

- maintainer-alive — pass. Human commit by Aburke225 on 2026-09-16 (5 days ago), and a COLLABORATOR reply on issue #52 the same day.
- repo-in-use — pass. archived: false, pushed_at: 2026-09-16. No release has ever been published, but the recent-push condition carries it.
- actionable-contribution — pass. Asks for new behavior: a webhook endpoint clients register a callback URL with, POSTed the review payload on completion.
- bounded-scope — pass. One feature in two named new modules (api/routes/webhooks.py, core/services/webhook_service.py), interface specified in the body, no umbrella framing and no unsettled design. Worth flagging: at 8–12 hours it's the largest single build of the candidates you've shown me, but the rubric's fail conditions cover umbrella issues, maintainer-declared core-internals work, and unresolved design — not "one feature that is simply big" — so it passes as written.
- issue-unclaimed — pass. No assignees, 0 commentsated_at (2026-09-10), and the repo has zero pullrequests of any state.
- ai-contribution-allowed — pass. docs/CONTRIBUTIN_TEMPLATE.md state no AI-use restriction.
- Preferred: maintainer-response pass (#52 got its first collaborator reply 6 days after opening), release-recency fail (no releases), attempt-history pass (no PRs exist).

Every required check passes → accept. Fit note, which doesn't move the verdict: this is the one of your three candidates furthestfrom the "avoid unfamiliar infrastructure" line in your profile, since it's greenfield API plumbing rather than a bounded edit.

{
  "item": "https://github.com/codepath/pathreview-
  "checks": [
    {"name": "maintainer-alive", "grade": "pass", Aburke225 on 2026-09-16 (5 days ago); COLLABORATORreply on issue #52 on 2026-09-16."},
    {"name": "repo-in-use", "grade": "pass", "evided_at 2026-09-16, within 180 days (no releases, butrecent-push condition satisfies)."},
    {"name": "actionable-contribution", "grade": "new behavior: 'Add a webhook endpoint where clientscan register a callback URL' that receives a POST with the review payload."},
    {"name": "bounded-scope", "grade": "pass", "evnew named modules with the interface specified; notan umbrella issue, no maintainer core-internals claim, design settled in the body."},
    {"name": "issue-unclaimed", "grade": "pass", " 0 comments; updated_at equals created_at(2026-09-10); repo-wide PR list (state=all) is empty."},
    {"name": "ai-contribution-allowed", "grade": "RIBUTING.md and .github/PULL_REQUEST_TEMPLATE.mdcontain no AI-use restriction."},
    {"name": "maintainer-response", "grade": "passned 2026-09-10, first COLLABORATOR reply (Aburke225) 2026-09-16 — 6 days."},
    {"name": "release-recency", "grade": "fail", "rns an empty array: no release has ever beenpublished."},
    {"name": "attempt-history", "grade": "pass", "s exist in the repo, so no closed-unmergedattempts."}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

**Run history**

Initial full evaluation: 19/20 correct classifications. The rubric passed the required threshold of 18/20 and achieved at least one correct verdict in every category. The only disagreement was issue-20, which my rubric accepted while the gold label was reject.

Final saved evaluation: 19/20 correct classifications. I kept the same rubric because it had already passed the required threshold and performed correctly on the other 19 scored issues. I ran the complete evaluation again with `--save-run eval-run.txt` to generate the required submission file.

The final run matched the initial run's results, including the disagreement on issue-20.


**Issue analysis**

I analyzed issue-20, which requests adding a company logo shape to the toolbar in the Excalidraw repository. My rubric returned `accept`, while the gold label was `reject`.

My rubric accepted the issue because it satisfied all six required checks. The repository showed recent human-authored commits, a recent release, and recent development activity. The issue requested an actionable code change, had no current assignee or linked pull request, and the repository's contribution policy did not prohibit AI-assisted contributions.

The main reason for the disagreement seems to be due to my `bounded-scope` check. My rubric considers an issue acceptable when it requests one identifiable fix and does not specifically have a broad rewrite, tracking issue, core-internal changes that were identified by a maintainer, or some unresolved design discussions.

Issue-20 looked to satisfy this condition because it requested one specific improvement, which was adding a company logo shape to the toolbar. The issue described the expected behavior, including placing, resizing, moving, and exporting the logo.

However, the issue also said that the logo asset was still to be determined and that the implementation might involve multiple parts of the application. There were no maintainer comments that told about the expected design or anything to confirm which company logo should be used.

My rubric accepted the issue because it focused on whether the requested work was one identifiable improvement rather than whether every implementation detail had been resolved.

This showed me a limitation in my scope check, which is that an issue can describe one specific feature while still not having any important design decisions or assets needed to implement it. My rubric did not explicitly require these details to be solved before accepting the issue.


**Check rationale**

I included the `bounded-scope` check to determine whether the  work is manageable as a first open-source contribution.

The current pass condition in my rubric:

"The requested work is **one identifiable fix or enhancement**, or a maintainer-defined checklist for **one feature/component**. **Fail** if it is expressly a tracking/umbrella issue intended to be split into separate contributions, requires a broad rewrite or changes to core internals according to a maintainer, or leaves the design unresolved after discussion. A short issue body, missing reproduction steps, or a multi-item acceptance checklist for one change does **not** fail by itself."

I chose this wording because I wanted to separate between a contribution with limits from an issue that needed a big redesign or multiple separate contributions.

I also wanted to avoid automatically rejecting an issue simply because its description was short, it lacked reproduction steps, or it included multiple acceptance criteria for one feature.

So then the check focuses on the actual scope of the requested work rather than the length or formatting of the issue description.


**Trade-offs**

One trade-off of my `bounded-scope` check is that it may accept a feature request with a  defined objective even when important implementation details have not been resolved.

This limitation was shown in Issue-20. My rubric accepted the issue because adding a company logo shape was seen as one enhancement. However, the issue said that the logo still wasn't determined and that no maintainer had clarified the intended design.

A mnore strict check that requires the confirmation of all necessary assets and designs would have rejected this issue. But at the same time, that approach could also reject legitimate first-contribution issues that have clear objectives but leave minor implementation details for the contributor to determine.

I kept my existing check because it correctly classified 19 of the 20 scored issues, which was more than the assignment's target of 18/20, and matched at least one verdict in every category. I accept that the current wording may miss some issues where unresolved implementation details are not explicitly presented as an ongoing design discussion.


## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

I selected Issue #35 because it involves adding a webhook endpoint, which is something I want to get better at. Specifically, it is related to my interest in backend development and building applications that communicate through APIs. The issue is a way for me to improve my understanding of HTTP requests, callbacks, and how different software components communicate. This issue also seems to be manageable within the time available for the course because the requested functionality has a defined interface and involves two new modules.

My issue-selection skill correctly identified that the repository was active, the issue requested an actionable contribution, the work had a bounded scope, and no existing assignee or pull request was blocking the issue. However, the rubric didn't fully account for my personal interest in learning how webhooks work or how much time I might need to understand the existing codebase before implementing the endpoint. This is why I considered those factors separately when choosing this issue.

I think the main challenge in claiming the issue will be understanding the existing project structure and seeing the expected behavior before actually making any implementation. I will also need to follow the repository's contribution guidelines and coordinate with other students if they are working on the same issue.


---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.

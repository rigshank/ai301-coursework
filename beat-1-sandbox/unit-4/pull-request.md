# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/107

Live pr-precheck verdict: accept
I ran pr-precheck in live mode on my draft before opening PR #107. The first draft received reject because the testing evidence was insufficiently documented, the description had an inaccurate evidence reference, and the PR title was not explicitly supplied. After correcting these issues, I reran the tool on branch fix/35-review-webhooks at commit 790d9cf. All seven checks received pass, and the final verdict was accept. I opened PR #107 after this result.

**Branch**

`fix/35-review-webhooks`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. **First completed full evaluation:** 18/20 agreement (PASS). The disagreements were `pkg-05` and `pkg-08`, both labeled `accept` by the gold labels but graded `reject` by my tool.

2. **Second completed full evaluation:** 18/20 agreement (PASS). The disagreements were `pkg-08` and `pkg-16`, both gold `accept` but graded `reject`. This run was previously saved to `eval-run.txt`.

3. **Third completed full evaluation (after the evidence-guide clarification):** 18/20 agreement (PASS). I updated the live evidence locations to explicitly reference `pr_draft.md` and `test_evidence.md`, then ran the complete 20-package evaluation again. The disagreements remained `pkg-08` and `pkg-16`. For `pkg-08`, the failing checks were Repository checks and Description accuracy. For `pkg-16`, only Repository checks failed. The final run was saved to `eval-run.txt`.

The final saved run reported:

`agreement: 18/20 scored items  (bar: 18/20: PASS)`

Its category tallies were `clear-accept 5/7`, `not-tested 4/4`, `silent-drift 4/4`, `standards-wall 2/2`, and `unreviewable 3/3`. The category floor was satisfied.

No scored `--only` runs were performed between these full evaluations. Earlier startup attempts encountered Windows Python invocation and encoding errors and did not produce completed scored evaluations.

**Package analysis**

I analyzed **`pkg-20`** (`standards-wall`). My rubric decided **`reject`**, matching the instructor gold label of **`reject`** in my final saved `eval-run.txt`:

```text
pkg-20  standards-wall  reject  reject   yes
```

The package describes a small Ghostty configuration change for issue `ghostty-org/ghostty#13604`. Its code diff and test evidence address the accepted plan: rebuild conditional configuration state when the color scheme changes, even with a non-conditional theme, and test the mode-2031 response. However, the package's repository facts explicitly state this contribution policy:

> All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance

The candidate PR's title, description, and test evidence contain no AI-assistance disclosure. My **Repository standards and disclosure** check is `required` and calls for compliance with stated repository policies, including AI disclosure when required. Therefore the missing disclosure means this package is not ready to submit, even though its implementation and tests are relevant to the plan. That explains why the rubric returned `reject` and agreed with the gold label.

**Check rationale**

This is the **exact check row** from my submitted `tools/pr-precheck/rubric.md`:

```markdown
| Repository standards and disclosure | Read the package's repo-facts block for the stated PR-template requirements, contribution policies, and AI-assistance disclosure requirements. Compare each applicable requirement against the candidate PR title and description. | Pass if all applicable mandatory PR-template and contribution requirements are satisfied, including an AI-use disclosure when the repository requires one. Requirements explicitly inapplicable to the change may be marked not applicable with a defensible explanation. If the repository has no stated requirement for a particular disclosure or template field, do not invent one. Fail if an applicable mandatory requirement is omitted or contradicted. | required |
```

I wrote this check as **required**, rather than treating repository standards as a preferred writing detail or considering only the code and tests. A technically correct implementation can still be unready when its PR ignores the repository's explicit submission rules. I also limited the check to **applicable, stated** requirements: it should not invent AI-use policies, template fields, or disclosure obligations that the evidence does not establish. The `pkg-20` case shows why that distinction matters. I retained this wording across my three scored full evaluation runs.

**Trade-offs**

The trade-off of this check is that it enforces only the repository requirements actually present in the package. This avoids falsely rejecting contributors for rules that were never stated, but means the check cannot catch an undisclosed policy or a communication preference not established by the evidence. Its concrete effect is visible in **`pkg-20`**: the package has relevant code and test evidence, yet the documented mandatory AI-use disclosure is missing, so the required standards check prevents an `accept` verdict.

I did **not** loosen this standards check to chase a higher aggregate score. All three completed full evaluation runs matched **2/2 `standards-wall` packages**, and the two final disagreements were in **`clear-accept`**, where my checks were stricter about recorded repository-check evidence and PR description accuracy. Keeping the standards rule unchanged preserves the documented policy requirement rather than trading that protection for uncertain gains elsewhere.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.

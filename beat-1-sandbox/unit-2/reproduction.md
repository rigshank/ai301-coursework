# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

rigshank

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/35#issuecomment-5895478585

Hi, I'd like to investigate issue #35 about adding webhook callbacks for completed reviews. I plan to trace the current review-processing flow, identify where review completion is detected, and determine how a client callback URL could be registered and invoked with the completed review payload. I'll follow up with an investigation report describing the current behavior and what I find.

**Reproduction comment**

[Link to the comment where you posted your reproduction. It must record the environment
(OS, relevant versions, code state), steps a stranger could follow, and what you observed.
**Then paste the text of that comment underneath the link** — the pasted text is what this
field is graded on, so copy across what you actually posted.]

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. First full run: `agreement: 18/20 scored items (bar: 18/20: PASS)`.
2. Second full run: `agreement: 16/20 scored items (bar: 18/20: below the bar)`.
3. Targeted rerun of `pkg-01,pkg-03,pkg-09,pkg-12`: `agreement: 3/4 scored items`. `pkg-09` still disagreed with gold.
4. After revising `steps-followable` and `behavior-matches`, targeted rerun of `pkg-09`: `agreement: 1/1 scored items`.
5. Canary rerun of `pkg-09,pkg-08,pkg-13,pkg-17`: `agreement: 3/4 scored items`; the three reject canaries (`pkg-08`, `pkg-13`, and `pkg-17`) remained correct, while `pkg-09` flipped on `claim-correct`.
6. Next full run: `agreement: 18/20 scored items (bar: 18/20: below the bar; category floor unmet: no match in disclosure)`. `pkg-20` was incorrectly accepted, so the disclosure category was `0/1`.
7. After revising `repo-conventions`, targeted rerun of `pkg-20`: `agreement: 1/1 scored items`, with `categories: disclosure 1/1`.
8. Next full run: `agreement: 17/20 scored items (bar: 18/20: below the bar)`. The remaining disagreements were `pkg-05`, `pkg-07`, and `pkg-12`, all on `claim-correct`.
9. After the first `claim-correct` revision, targeted rerun of `pkg-05,pkg-07,pkg-12`: `agreement: 2/3 scored items`; `pkg-07` still failed `claim-correct`.
10. After separating eval-mode and live-mode chronology in `claim-correct`, targeted rerun of `pkg-05,pkg-07,pkg-12`: `agreement: 3/3 scored items`.
11. Final confirming full run: `agreement: 20/20 scored items (bar: 18/20: PASS)`. The category results were `clear-accept 8/8`, `disclosure 1/1`, `no-evidence 4/4`, `unfollowable-comms 3/3`, and `wrong-target 4/4`. This is the run saved in the final `eval-run.txt`.

**Package analysis**

I analyzed `pkg-20`. My final rubric verdict was **reject**, and the gold label was also **reject**. The package's Repo facts state: **“All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance”**. The candidate claim and reproduction report do not include that required disclosure. Because `repo-conventions` is a required check, the missing disclosure makes the package not ready to post. My earlier wording referred generally to following repository conventions, but the grader accepted this package in one full run. I revised the check so that when a repository explicitly requires AI-assistance disclosure, omission of the disclosure or required details is an explicit failure condition. After that revision, the targeted rerun returned `pkg-20: reject`, matching the gold label, and the disclosure category returned `1/1`.

**Check rationale**

Final `claim-correct` check from my rubric:

> `| claim-correct | In eval mode, inspect the claim comment against the issue context, the chronology described by the package, and any reproduction evidence included in the package. In live mode, inspect the student's draft claim against the live issue before reproduction work is posted. | **Pass** in eval mode when the claim identifies the specific issue, accurately describes the investigation or reproduction work, and any statement that reproduction has already occurred is supported by evidence available in the package. The claim should also indicate the next investigation step or follow-up when appropriate to its chronology. **Pass** in live mode when the claim identifies the specific issue, states the investigation or reproduction work the student intends to do, and clearly indicates that the student will follow up with the results or findings without asserting an unperformed reproduction. **Fail** when the claim makes an unsupported reproduction claim, promises a fix or completion date, misstates the issue being investigated, or, in live mode, asserts reproduction before that work has been performed. | required |`

I revised this check because the earlier version treated eval packages and a live pre-reproduction claim as though they always had the same chronology. That caused gold-accept packages such as `pkg-07` to fail even though the package included evidence supporting its statement that reproduction had already occurred. The final wording separates the two modes: eval mode can accept an after-the-fact reproduction statement when the package contains supporting evidence, while live mode still requires the student's claim to describe intended work and promise a follow-up without asserting an unperformed reproduction. After this revision, the targeted rerun of `pkg-05,pkg-07,pkg-12` returned `agreement: 3/3 scored items`.

**Trade-offs**

The revised `claim-correct` check is more permissive in eval mode because it can accept a claim that says reproduction already happened when the package contains evidence supporting that statement. The trade-off is that chronology has to be interpreted from the package instead of rejecting every past-tense reproduction statement automatically. I kept a stricter live-mode rule so that a real claim posted before reproduction still fails if it asserts reproduction too early. The targeted rerun of `pkg-05,pkg-07,pkg-12` returned `agreement: 3/3 scored items`, and the final full run returned `agreement: 20/20 scored items (bar: 18/20: PASS)`, showing that this distinction fixed the clear-accept cases without breaking the final scored set.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.

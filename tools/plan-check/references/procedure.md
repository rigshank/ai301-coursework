# Procedure: how this skill grades a plan package

## Read order

1. In live mode, read `scope.md` first and confirm that the issue is inside the scoped repository. Note any house rules that apply. In eval mode, skip `scope.md`.
2. Read `rubric.md` and `references/evidence-guide.md`. List the rubric checks and the verdict rule before grading anything.
3. Read the whole plan package before grading any check. Read the issue context first, then the repro evidence, then the candidate plan, then the candidate plan comment.
4. While reading, note the behavior shown by the repro evidence, the diagnosis stated by the candidate plan, the proposed scope and changes, and the candidate test plan.

## Evidence gathering

1. For each rubric check, gather only the evidence named by that check, using `references/evidence-guide.md` to find where the evidence lives.
2. For the Diagnosis check, compare the candidate plan's diagnosis with the behavior and steps shown in the repro evidence.
3. For the Scope check, gather the parts of the candidate plan that state what is in scope, what is out of scope, and what changes will be made.
4. For the Test check, compare the candidate plan's test plan with the reproduction steps and observable behavior in the repro evidence.
5. In live mode, gather issue-side evidence from the locations named in the evidence guide and use the student's drafts as the candidate plan package.
6. In eval mode, use only the evidence contained in the package bundle. Do not use outside information.
7. Record the specific quote or fact that will decide each check.

## Check execution

1. Grade every rubric check as `pass`, `fail`, or `unclear`.
2. Grade a check `pass` only when the gathered evidence satisfies that check's pass condition in `rubric.md`.
3. Grade a check `fail` when the gathered evidence contradicts or does not satisfy the pass condition.
4. Grade a check `unclear` only when the evidence needed to decide the check is genuinely absent from the package, not because it was not searched for.
5. For every grade, record a one-line evidence quote or fact explaining why the check received that grade.
6. Follow the rubric exactly. Do not change a grade because the plan feels good or bad outside the rubric's stated conditions.

## Verdict assembly

1. After every rubric check has been graded, apply the verdict rule in `rubric.md`.
2. Accept the plan as ready only if every required check passes.
3. Treat `unclear` as a failure because the rubric's verdict rule requires all required checks to pass.
4. If any required check is `fail` or `unclear`, reject the plan and place it on hold.
5. In live mode, check the candidate plan comment against `voice-guide.md` and note any broken voice-guide rule in the readable summary. Do not let the voice guide change the verdict unless a rubric check explicitly uses it.
6. Report the check grades and evidence, then produce the final `accept` or `reject` verdict in the output format required by `SKILL.md`.

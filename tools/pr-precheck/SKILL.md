---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Determine whether exactly one candidate pull request is ready to submit.

A PR package contains the proposed PR title, description, commits, unified diff, and test evidence. Evaluate these artifacts against the accepted plan, including recorded deviations, and the issue that the plan addresses.

Decide whether the implementation matches the plan, the evidence supports the claimed results, and the submission satisfies applicable repository standards.

Grade only the supplied PR package. Do not implement fixes, rewrite the plan, open a PR, or substitute personal judgment for the written rubric.

## Inputs and modes

Choose one of two modes based on the supplied input.

### Live mode

Use live mode when checking the student's own branch and draft PR before submission.

Gather these inputs:

1. Read `scope.md` first and verify the target repository.
2. Read the student's `plan.md`, including the `## Deviations` section. If using a house-chain issue, read the provided house plan and house reproduction pack instead.
3. Identify the current branch and obtain its full committed diff against the repository's default branch. From the repository working copy, use `git diff main...HEAD` when the default branch is `main`. The three-dot comparison is required. If the default branch differs, use its actual name. Inspect the relevant commit list as well.
4. Read the student's draft PR title and description from the supplied draft.
5. Read the recorded test evidence, including commands, observed outputs, before-and-after behavior, and any unrun or failed checks.
6. Read the issue in the scoped repository, including its description and relevant thread discussion.
7. Gather repository-side requirements from the PR template, contribution instructions, and stated policies, including AI-assistance disclosure requirements.

Use only evidence that can be inspected. Do not invent missing commands, outputs, commits, or test results.

If an input is missing, report it and grade the affected checks using the rubric's evidence and verdict rules. Do not treat unsupported claims as verified.

### Eval mode

Use eval mode when given a self-contained PR package bundle.

1. Treat the provided package as the complete evidence universe.
2. Read the issue context, thread highlights, repository facts, plan context, PR title, description, commits, diff, and test evidence from the bundle.
3. Do not fetch GitHub pages, local repository files, external policies, or other information.
4. Do not replace the package's frozen facts with information from a live issue.
5. Execute every rubric check and apply the complete verdict rule, even when a package appears obviously acceptable or unacceptable.

Eval mode ignores `scope.md` and `voice-guide.md`.

## The scope seam (live mode only)

Before gathering or grading live PR evidence, read `scope.md`.

1. Find the `Repo:` line.
2. If it contains an unfilled placeholder, stop without grading. Tell the student to fill that line with the correct section's Path Review repository. Never guess the repository.
3. Verify that the issue and proposed PR target belong to the scoped repository.
4. Refuse to grade work outside that repository and explain the mismatch.
5. Follow the applicable house rules recorded in `scope.md` when collecting and interpreting live evidence.

Do not modify `scope.md` automatically.

In eval mode, ignore `scope.md` entirely. The bundle defines the complete grading context.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and apply its communication rules to the outgoing PR title and description.

1. Compare the draft title against the guide's rules for specificity and accuracy.
2. Compare the description against its rules for evidence-based claims, accurate scope, limitations, deviations, and repository-required disclosures.
3. Identify any violated voice rule and report it in the readable summary, citing the relevant draft wording.
4. Explain the correction needed without silently rewriting the student's submission.
5. Do not independently change the PR verdict because of a voice-guide violation. A violation affects the verdict only when a check in `rubric.md` explicitly requires it.

In eval mode, ignore `voice-guide.md` entirely. Apply only the universal communication and repository-standard checks explicitly defined by the rubric.

## Component reads

Use the installed tool's components from their fixed locations.

1. Read `rubric.md`. It defines the checks, evidence requirements, pass conditions, weights, and final verdict rule.
2. Read `references/evidence-guide.md`. Use it to locate and interpret each evidence family in the PR package.
3. Read `procedure.md`. Execute its operating steps in the written order.
4. Apply every check named in `rubric.md` using the evidence sources and execution method specified by the components.
5. Use the rubric's exact pass conditions. Do not add, remove, or silently reinterpret checks during a grading run.
6. Follow the rubric's verdict rule when assembling the final decision.

If `rubric.md` contains no filled-in checks or verdict rule, refuse to grade and explain what is missing.

If `procedure.md` contains no executable steps, refuse to grade and explain what is missing.

If required evidence guidance or execution instructions are incomplete, report the specific gap. Do not improvise replacement criteria or procedures.

Do not alter the skill's files as part of grading.

## Verdict and output

Produce exactly one binary verdict:

- `accept`: The PR is ready to submit under the rubric's verdict rule.
- `reject`: The PR must be held under the rubric's verdict rule.

Do not emit another verdict, a numerical score, or an "accept with reservations" result.

Before the machine-readable output, provide a concise summary identifying the evaluated package, each check's grade, the decisive evidence, and any outstanding problems. In live mode, also report any voice-guide violations separately.

For every rubric check, assign one of three grades:

- `pass`: The stated pass condition is satisfied by evidence.
- `fail`: The stated pass condition is not satisfied.
- `unclear`: Available evidence is insufficient to establish the result.

Apply the exact verdict rule in `rubric.md`. If that rule does not specify how `unclear` affects the verdict, treat it as failing.

End the response with the following fenced JSON block. Keep the schema and key order unchanged. Emit valid JSON with actual item identifiers, check names, grades, evidence, and verdict values substituted for the placeholders.

Do not write anything after the JSON block.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

Follow these rules on every run:

1. **Evidence first.** Every check needs a specific fact, quote, command output, diff observation, or documented absence of required evidence. Never use "looks good" as the justification.

2. **Grade the thing, not the polish.** Compare the actual PR contents with the accepted plan, issue, test evidence, and applicable repository standards. Do not reward polished wording that hides missing work or penalize concise wording that provides all required facts.

3. **The rubric decides.** Apply the written pass condition for each check, even if the result differs from an intuitive judgment. Do not change a check's meaning while grading.

4. **The procedure decides how.** Execute `procedure.md` as written. If an instruction is missing or ambiguous, report the gap instead of inventing a new procedure.

5. **Separate claims from proof.** A PR description that says tests passed is not equivalent to recorded evidence showing those tests ran. Compare claimed behavior against the available commands, outputs, and changes.

6. **Account for deviations.** Read the plan's deviation notes and distinguish documented changes from unexplained differences. Do not silently treat work outside the plan as approved.

7. **Respect stated standards.** Use the repository's supplied PR-template and contribution-policy evidence when evaluating applicable requirements. Do not invent repository policies.

8. **Unclear defaults to fail.** Follow the rubric's explicit treatment of `unclear`; if its verdict rule is silent, treat unverifiable claims as failing.

9. **Never fill evidence gaps with assumptions.** State what is missing and how it affects the relevant check.

10. **Keep the output auditable.** Every check's evidence line must identify why it received its grade. The final verdict must follow mechanically from the rubric's written verdict rule.
grading
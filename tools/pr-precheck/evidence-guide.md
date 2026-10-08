# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

### Where evidence lives

**Eval mode:**

1. Read the package's plan-context block, including the accepted plan's diagnosis, scope, intended changes, files to modify, exclusions, and recorded deviations.
2. Read the candidate PR's unified diff and commit list to identify the files and behavior actually changed.
3. Read the candidate PR's title and description for claims about what was implemented, what was excluded, and whether the work followed the plan.
4. Read the issue context and thread highlights to determine whether the implemented behavior addresses the reported problem and whether relevant scope decisions were recorded.

**Live mode:**

1. Read `plan.md` from the student's Path Review working copy, including `## Deviations`.
2. Inspect the branch's changes against the repository's default branch. Use `git diff main...HEAD` when `main` is the default branch, or substitute the actual default branch.
3. Read the draft PR title and description.
4. Review the original issue and relevant thread discussion in the scoped repository.
5. For a house-chain submission, use the provided house plan and reproduction pack in place of the student's original plan and reproduction.

### What good looks like

- Every substantive change in the diff falls within the accepted plan's stated scope or is explicitly accounted for in its recorded deviations.
- The implementation addresses the intended behavior without silently expanding or narrowing the planned work.
- The PR description accurately explains what the diff implements and distinguishes completed work from deferred or unimplemented work.
- A material difference from the original plan is acceptable when it is documented in the deviation notes and accurately disclosed in the description.
- An honest limitation does not automatically make a PR unready when the implemented work and supporting evidence are accurately represented.

### Failure signals

- The diff introduces functionality or modifies files outside the planned scope without a recorded deviation.
- A planned substantive change is missing without explanation.
- The description claims that functionality was implemented when the diff does not support that claim.
- The description claims full plan compliance despite unexplained differences.
- The PR silently changes the approach or scope without recording the difference.

For the **Plan fidelity** and **Description accuracy** rubric checks, record the specific planned requirement, the corresponding diff observation, and any deviation note or description claim that explains the comparison.

Do not treat a documented deviation as silent drift merely because the final implementation differs from the original plan.

## Test evidence (harness category: not-tested)

### Where evidence lives

**Eval mode:**

1. Read the plan-context block's test plan, including reproduction steps, expected post-fix behavior, planned verification commands, and relevant edge cases.
2. Read the candidate PR's test-evidence section for commands executed, actual outputs, before-and-after results, test failures, and reported blockers.
3. Compare the recorded evidence with the candidate PR's unified diff to determine whether the executed tests exercise the relevant changed behavior.
4. Read the candidate PR description for statements about tests, validation, unrun checks, and limitations.
5. Read applicable repository validation requirements from the repo-facts block.

**Live mode:**

1. Read the test plan in `plan.md` and the reproduction evidence from Unit 2, or the supplied house reproduction pack.
2. Inspect the student's recorded test commands and outputs, including before-and-after evidence.
3. Inspect the branch's relevant tests and implementation changes.
4. Read the draft PR description for claimed results and testing limitations.
5. Identify repository validation commands from the repository's documented development and contribution instructions.

### What good looks like

- The evidence identifies the actual behavior being tested and the expected outcome.
- The reported reproduction behavior is supported by recorded observations from before the change, and the post-change behavior is supported by recorded execution results.
- Test commands and outputs provide observable support for the claimed fix rather than merely asserting success.
- Where applicable, the repository's own checks have recorded outcomes, including the command executed and whether it passed, failed, or was blocked.
- The evidence demonstrates that the tests exercise the relevant implementation rather than an unrelated stand-in.
- The PR description accurately states which checks ran and what their results establish.

A limited or partially blocked test result may still support submission when the executed evidence establishes the claimed bounded outcome, the blocker is described accurately, and the description does not claim unverified coverage.

### Failure signals

- The only evidence is a statement such as "tests pass" without supporting execution results.
- The evidence lacks an observable outcome for the claimed behavior.
- The reported tests do not exercise the changed implementation.
- The description claims a passing test suite that was not executed.
- Applicable repository checks are silently skipped without recorded outcomes or explanations.
- Recorded failures are concealed or presented as successes.
- The evidence cannot establish the claimed behavior before and after the change.

For the **Reproduction and test evidence** and **Repository checks** rubric checks, record the planned test or required check, the actual command and output, and the observable result or missing evidence.

Distinguish a documented execution blocker from an unreported missing check. Disclosure is important, but disclosure alone does not replace the execution evidence required to support the claimed change.

## Diff quality (harness category: unreviewable)

### Where evidence lives

**Eval mode:**

1. Read the candidate PR's unified diff and changed-file list.
2. Inspect the commit list to identify how the submitted changes are organized.
3. Compare substantive changes with the plan-context block's scope, intended files, approach, and deviation notes.
4. Use the issue context to determine whether a change is relevant to the reported problem.

**Live mode:**

1. Inspect the student's branch using `git diff main...HEAD` when the repository's default branch is `main`, or substitute the actual default branch.
2. Examine the relevant commit history and changed files.
3. Compare the diff with `plan.md`, including its scope and recorded deviations.
4. Identify any unrelated modifications, debugging remnants, or accidental files.

### What good looks like

- The diff presents an identifiable implementation connected to the accepted plan.
- The relevant code and test changes can be inspected without unrelated modifications obscuring them.
- Every substantive changed file has a defensible relationship to the planned work or a recorded deviation.
- Necessary supporting changes, such as directly related tests or configuration adjustments, are acceptable when their purpose is evident.
- The implementation does not introduce unexplained temporary files, debugging code, or unrelated behavior changes.

A PR is not unreviewable merely because it modifies multiple files, uses several commits, or includes necessary supporting changes. Judge whether the actual changes can be meaningfully reviewed.

### Failure signals

- Unrelated formatting changes obscure the actual implementation.
- Debugging statements, temporary instrumentation, dead code, or abandoned commented-out implementations remain without justification.
- Unrelated features, refactors, or configuration changes are included without a connection to the plan.
- Accidental generated files or other unnecessary artifacts bury the intended change.
- The diff contains substantial unrelated modifications that make the implementation difficult to isolate and review.

For the **Diff reviewability** rubric check, record the relevant changed files or hunks and explain their relationship to the accepted scope.

Do not reject solely because of the number of commits, the number of changed files, or stylistic preferences.

## Standards and comms (harness category: standards-wall)

### Where evidence lives

**Eval mode:**

1. Read the package's repo-facts block for the repository's stated PR-template requirements, contribution instructions, and communication policies.
2. Identify any explicit AI-assistance disclosure requirement and its applicable conditions.
3. Read the issue context and thread highlights for relevant maintainer instructions.
4. Read the candidate PR's title and description to determine whether the required information and disclosures are present.
5. Compare applicable repository requirements with the actual submission, using only the frozen package evidence.

**Live mode:**

1. Read the scoped repository's PR template, if provided.
2. Read applicable contribution instructions, including `CONTRIBUTING.md` when present.
3. Read any stated repository policy concerning AI assistance or disclosure.
4. Read relevant maintainer instructions in the issue thread.
5. Compare those requirements with the student's draft PR title and description.
6. Read `voice-guide.md` to identify personal communication-rule violations. Report these separately unless a rubric check independently makes the requirement mandatory.

### What good looks like

- Applicable mandatory sections of the repository's PR template are completed with meaningful, relevant information.
- The PR description satisfies applicable contribution and communication policies.
- Required AI-assistance disclosure is present when the repository explicitly requires it.
- Explicit maintainer instructions relevant to the submission are addressed or accurately explained.
- The title and description communicate the change without contradicting applicable repository requirements.
- A template section that genuinely does not apply may be marked not applicable when the repository allows it and the explanation is consistent with the submitted work.

Do not invent a disclosure requirement or a mandatory template section that is not supported by the repository's stated standards.

### Failure signals

- A mandatory PR-template section is missing or left as unchanged placeholder text.
- The repository explicitly requires AI-assistance disclosure, but the PR omits it.
- The submission ignores an applicable contribution requirement.
- Relevant, explicit maintainer instructions are disregarded without explanation.
- The PR supplies generic boilerplate instead of information required by the repository's stated template.
- The submission claims compliance with a mandatory requirement that its actual contents contradict.

For the **Repository standards and disclosure** rubric check, record the exact applicable repository requirement and the corresponding location in the PR that satisfies or violates it.

For the **PR title clarity** check, compare the title with the issue and implemented change. A generic title is a preferred-check concern unless an applicable repository policy or another required check makes that wording mandatory.

Assess whether the PR description's claims match the actual implementation under **Plan fidelity** and **Description accuracy**, not as a substitute for standards compliance.

In eval mode, use only the package's repo-facts block, issue/thread context, and candidate PR evidence. Do not consult the live repository or apply the student's personal voice guide.
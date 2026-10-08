
# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Plan fidelity | Compare the candidate PR's unified diff, changed files, and commit list against the accepted plan's scope, intended behavior, file list, and recorded deviations. Use the issue context to confirm the problem being addressed. | Pass if the substantive changes implement the plan's stated objective without silently adding, removing, or changing planned work. Any material difference must be explicitly recorded in the plan's deviations and accurately explained in the PR description. An honest, documented limitation or deviation is not automatically a failure. Fail if the diff contradicts the plan and the difference is not disclosed. | required |
| Reproduction and test evidence | Compare the plan's test plan and reproduction steps against the candidate PR's recorded test commands, outputs, and before-and-after observations. Inspect the diff to confirm the tests exercise the relevant changed behavior. | Pass if the evidence contains observable execution results demonstrating the relevant behavior before and after the change, or an equivalent executable comparison tied to the real code, and supports the claimed outcome. Limited testing can pass when its coverage and limitations are stated accurately. Fail if the claimed fix has no meaningful execution evidence, results are merely asserted, or the recorded tests do not exercise the relevant behavior. | required |
| Repository checks | Compare the repository's stated validation requirements and the plan's testing commitments against the candidate PR's test evidence and description. Look for actual commands, outputs, statuses, and explanations for checks that could not complete. | Pass if applicable repository checks have recorded outcomes, including successful results, failures, or documented execution blockers. An unavailable check is not automatically a failure when the attempted command, observed blocker, and resulting limitation are documented and other evidence supports the claimed change. Fail if applicable checks are silently skipped, results are invented, or the description claims checks passed without supporting evidence. | required |
| Diff reviewability | Inspect the candidate PR's unified diff and changed-file list against the plan's files and approach. Identify unrelated changes, accidental generated files, debugging remnants, or modifications with no connection to the intended change. | Pass if the diff can be reviewed as a coherent implementation of the plan and every substantive change has a clear connection to the planned work or documented deviations. Necessary tests, configuration changes, and generated updates directly caused by the implementation are acceptable. Fail if unrelated hunks, accidental files, or unexplained changes obscure or materially expand the submission. Do not grade by file count, commit count, or formatting alone. | required |
| Description accuracy | Compare the candidate PR title and description against the unified diff, accepted plan, deviation notes, test evidence, and issue context. Check statements about fixed behavior, tests, scope, and remaining limitations. | Pass if the description accurately explains the implemented change, does not claim unsupported results, and identifies material deviations, deferred work, and relevant testing limitations. A disclosed shortfall is acceptable when the description does not misrepresent the supported outcome. Fail if the description claims work or validation that the evidence contradicts, or hides material departures from the plan. | required |
| Repository standards and disclosure | Read the package's repo-facts block for the stated PR-template requirements, contribution policies, and AI-assistance disclosure requirements. Compare each applicable requirement against the candidate PR title and description. | Pass if all applicable mandatory PR-template and contribution requirements are satisfied, including an AI-use disclosure when the repository requires one. Requirements explicitly inapplicable to the change may be marked not applicable with a defensible explanation. If the repository has no stated requirement for a particular disclosure or template field, do not invent one. Fail if an applicable mandatory requirement is omitted or contradicted. | required |
| PR title clarity | Compare the candidate PR title against the issue, accepted plan, and implemented change. | Pass if the title identifies the actual change or affected behavior accurately enough for a reviewer to understand its purpose. Fail if it is generic, misleading, or unrelated to the change. This is a communication-quality preference unless another required check or repository policy makes the wording mandatory. | preferred |

## Verdict rule

Grade every check as `pass`, `fail`, or `unclear` using the evidence and pass condition in its row.

- `pass`: The available evidence establishes that the stated pass condition is satisfied.
- `fail`: The evidence demonstrates that the pass condition is not satisfied.
- `unclear`: The evidence is insufficient or ambiguous, so the pass condition cannot be verified.

Return `accept` only when every `required` check receives `pass`.

Return `reject` if any `required` check receives `fail` or `unclear`.

A `preferred` check may receive any grade without changing the final verdict. Report its grade and evidence so the student can improve the submission.

Do not calculate a numerical score or introduce a third verdict. Apply every check before assembling the final verdict.

Treat documented limitations and deviations according to the actual pass conditions, not as automatic failures. A PR may be ready when it honestly describes bounded, evidenced work and identifies what remains incomplete. Disclosure does not, however, replace evidence required to verify the core claimed change.

For every check, record a concrete fact, quote, diff observation, command output, or missing required evidence that explains its grade. Do not substitute writing quality or personal confidence for the stated pass condition.
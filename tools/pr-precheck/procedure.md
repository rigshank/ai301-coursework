# Procedure: how this tool grades a PR package

## Read order

1. **Identify the grading mode.** In live mode, read `scope.md` before any other file or evidence. Verify that the target repository is permitted and that the `Repo:` line is filled. Stop without grading if the scope is invalid. In eval mode, ignore `scope.md` and `voice-guide.md` and use only the supplied package bundle.

2. **Load the grading components.** Read `rubric.md` and `references/evidence-guide.md`. Verify that the rubric contains filled checks and a verdict rule and that this procedure contains executable instructions. Refuse to grade if the rubric or procedure is unfilled.

3. **Read the issue context and repository standards.** Record the reported behavior, expected outcome, relevant thread discussion, PR-template requirements, contribution policies, and any mandatory AI-use disclosure. In eval mode, use the bundle's issue context, thread highlights, and repo-facts block. In live mode, gather these from the scoped repository.

4. **Read the accepted plan before the implementation.** Record the planned diagnosis, scope boundaries, intended behavior, files to change, test plan, risks, and `## Deviations` section. For a house-chain submission, use its supplied house plan and reproduction pack.

5. **Read the actual implementation.** Inspect the candidate PR's commits, changed-file list, and unified diff. In live mode, obtain the committed branch diff against the default branch using `git diff main...HEAD` when the default branch is `main`. Otherwise, substitute the actual default branch. Record the substantive changes without assuming that the implementation matches the plan.

6. **Read the test evidence.** Record executed commands, observed outputs, before-and-after results, repository checks, failed checks, and stated execution limitations.

7. **Read the proposed PR title and description last.** Extract the claims about implemented functionality, test results, scope, deviations, limitations, and disclosures. Reading these last prevents the description's claims from replacing independent evidence from the plan, diff, or tests.

8. **Apply the voice guide in live mode.** Compare the proposed PR title and description with `voice-guide.md`. Record any communication-rule violations separately. They affect the verdict only if a rubric check explicitly covers them.

## Evidence gathering

Use `references/evidence-guide.md` to locate each evidence family. Build the following comparisons before assigning grades.

1. **Plan fidelity:** List the plan's intended changes, file boundaries, exclusions, and recorded deviations. Compare them with each substantive changed file and behavior in the diff. Identify implemented work, missing work, additional work, and any unexplained differences.

2. **Reproduction and test evidence:** Extract the plan's reproduction steps, test commands, expected post-fix outcomes, and relevant test cases. Pair them with recorded executed commands and actual outputs. Note whether the evidence exercises the relevant changed code and demonstrates the claimed behavior.

3. **Repository checks:** Identify applicable validation commands from the repository's stated requirements and the plan's testing commitments. Record which checks ran, their observed outcomes, and any documented blockers. Distinguish a recorded failed or blocked check from a check silently omitted.

4. **Diff reviewability:** Inspect the unified diff and commit list for substantive changes unrelated to the plan or documented deviations. Record accidental files, debug leftovers, unrelated modifications, or unexplained changes that obscure the intended implementation. Do not treat file count, commit count, or formatting alone as evidence of failure.

5. **Description accuracy:** Extract concrete claims from the PR title and description. Compare each claim with the plan, diff, and observed test evidence. Identify unsupported claims, material omissions, documented deviations, and accurately disclosed limitations.

6. **Repository standards and disclosure:** Extract mandatory requirements from the repo-facts block in eval mode or repository policies and PR template in live mode. Compare each applicable requirement with the draft PR text. Record any missing mandatory content or disclosure. Do not invent policies absent from the evidence.

7. **PR title clarity:** Compare the title's described behavior with the issue and actual change. Record whether the title accurately identifies the purpose or misleadingly describes the implementation.

For each check, record the specific source and decisive fact. If evidence is absent, record exactly what is missing rather than creating a substitute.

## Check execution

1. Execute all rubric checks in their written order:
   - Plan fidelity
   - Reproduction and test evidence
   - Repository checks
   - Diff reviewability
   - Description accuracy
   - Repository standards and disclosure
   - PR title clarity

2. For each check, read its exact `Evidence`, `Pass condition`, and `Weight` fields from `rubric.md`.

3. Compare the gathered evidence against the check's pass condition. Apply the written condition without introducing additional requirements or replacing it with personal judgment.

4. Assign exactly one grade:
   - `pass` when the evidence establishes that the pass condition is satisfied.
   - `fail` when the evidence demonstrates that the pass condition is not satisfied.
   - `unclear` when genuinely missing or ambiguous evidence prevents verification.

5. Distinguish missing evidence from evidence of failure. If a required observation is absent, use `unclear` unless the pass condition explicitly makes that absence a failure. If observed facts contradict the pass condition, use `fail`.

6. Apply the honest-outcome rule in each relevant check. Do not automatically fail a PR merely because work was deferred, a limitation was disclosed, or a test encountered a blocker. Determine whether the specific pass condition is satisfied by the actual evidence and disclosure. Disclosure never substitutes for evidence that the pass condition requires.

7. Record a one-line evidence explanation for each grade. Cite a concrete quote, changed-file observation, command result, repository rule, or clearly identified missing requirement.

8. Reuse the evidence gathered in the previous stage. Re-read a source only when needed to resolve a specific contradiction or ambiguity. Do not silently add criteria during re-reading.

9. Grade every check, including preferred checks, even after a required check fails. Never stop early simply because the eventual verdict will be `reject`.

10. If a procedure instruction is incomplete or impossible to execute, report the specific procedural gap. Do not invent missing operating steps.

## Verdict assembly

1. Collect every check's name, grade, decisive evidence, and weight in the same order as `rubric.md`.

2. Apply the verdict rule from `rubric.md` exactly:
   - Return `accept` only when every required check receives `pass`.
   - Return `reject` when any required check receives `fail` or `unclear`.
   - Preferred checks never independently change the final verdict.

3. If the rubric's verdict rule does not explicitly explain `unclear`, treat an unclear required check as failing, as required by `CONTRACT.md`.

4. For a rejected package, select the **first required check in rubric order** graded `fail` or `unclear` as the primary rejection reason. Quote its decisive evidence or identify the required evidence that is missing. Also report the remaining failed or unclear checks.

5. For an accepted package, identify the evidence establishing that the required checks passed. Report any preferred-check concerns or documented limitations without changing the verdict.

6. In live mode, report any `voice-guide.md` violations separately from the rubric results. Do not change the verdict based on voice guidance alone unless a rubric check explicitly reads that requirement.

7. Produce a concise human-readable summary containing the package identifier, per-check grades, evidence, final verdict, and relevant outstanding concerns.

8. End with a valid fenced JSON block using the exact schema and key order provided under `## Verdict and output` in `SKILL.md` and `CONTRACT.md`. Include the actual PR URL or bundle ID, every rubric check with its grade and one-line evidence, and the binary `accept` or `reject` verdict.

9. Do not introduce extra JSON fields, an alternative verdict, or a numerical score. Do not write anything after the final fenced JSON block.
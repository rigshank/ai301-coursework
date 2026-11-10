# Contribution: Tests for the answer instructions: every setting reaches the prompt

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Krishang Reddy Mandala
**GitHub Username:** rigshank
**Issue:** https://github.com/GianlucaBave/murmury/issues/40
**Pull Request:** https://github.com/GianlucaBave/murmury/pull/41

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built and refined my GitHub issue-scout**

I upgraded my Unit 1 `issue-select` skill into `issue-scout`, preserved my existing rubric and fit profile, and created a search strategy targeting Python and JavaScript issues. My initial scan searched 200 issues, filtered the results to 151 candidates, and graded the top five, all of which passed the required checks. However, the scan favored recently opened issues and selected multiple issues from the same repository, so I improved the search strategy to limit the top five to one issue per repository and updated my rubric to remove the outdated Path Review claim exception. I documented these observations and changes in `scan-notes.md` and graded additional candidate issues, although I did not rerun the full scan after making the improvements.

**Unit 6: Installed voice-guard and created repository memory for Murmury**

I consolidated my previous voice guides, added upstream communication rules, and installed the voice-guard to check outgoing GitHub comments. The automatic Claude Code hook was registered but failed its wiring test, so I used the manual voice-guard checker, which successfully passed my claim draft. I also replaced the sandbox scopes for `repro-check` and `plan-check` with the real-repository scopes. Finally, I created `CLAUDE.local.md` for the Murmury repository using a scoped codebase investigation and excluded it from Git so it would not appear in my pull request.

**Unit 7: Installed ship-gate, validated my test implementation, and opened PR #41**

I installed the ship-gate and configured ship-manifest.md to run the focused tests for Murmury issue #40. The focused test suite passed all 26 tests, while the official suite remained blocked by macOS-specific dependencies on Windows. The automatic hook test did not show the expected stops, so I verified the ship-gate directly and ran the manual branch check, which returned PASS with all seven checks passing. I then opened pull request #41 against the original repository and documented the testing limitations.

**Unit 8: Installed the PR monitor, created a playbook, and followed up on my merged PR**

I installed the PR monitor and created a playbook with response rules, staleness thresholds, nudge etiquette, and walk-away criteria based on Murmury's contribution guidelines and five recently merged PRs. The monitor identified PR #41 as merged and drafted a thank-you message, but it incorrectly implied that a human review occurred and suggested closing an issue that was already closed. I corrected the draft, manually verified my revised message with voice-guard, and successfully posted it to PR #41. The automatic voice-guard hook reported a Python-not-found error, so I relied on the successful manual check and identified the hook's Python invocation as something to fix.

---

## Why I Chose This Issue

I found this issue while using my `issue-scout` tool to evaluate open-source contributions. My earlier candidates included issues from RimSort, OpenEnergyDashboard, and evalproof, but they were either too broad, already claimed, or completed. I then evaluated Murmury issue #40, which asks for Python tests to verify that different answer settings are correctly reflected in generated AI prompts. My scout returned an **accept** verdict because the repository is active, the issue is unclaimed, AI-assisted contributions are permitted, and the work has clear acceptance criteria. I classified the difficulty as straightforward because the issue identifies the test file and specific cases to cover.

I chose this issue because it matches my experience with Python and my interest in AI/ML. I want to improve my understanding of automated testing, prompt construction, and contributing to an unfamiliar codebase. My scout helped filter unsuitable issues, but I noticed limitations in its rubric. In particular, it originally treated open design questions too strictly and did not check whether an issue had already been closed. I replaced the `bounded-scope` check with `scope-feasible` and added difficulty notes. In the future, I would also add a required check for whether an issue is still open and unresolved.

**Claim comment:** https://github.com/GianlucaBave/murmury/issues/40#issuecomment-6755610573

**Toolkit**

- Tool: I used `issue-scout` to evaluate candidate issues using repository activity, contribution policies, issue availability, scope, and my technical interests.
- What it produced: The scout accepted Murmury #40 after checking all required criteria. It identified the work as a bounded Python testing contribution.
- Difficulties: Earlier evaluations accepted issues that were already completed, and the original scope check could reject otherwise reasonable contributions. The Murmury evaluation also did not automatically include the required difficulty note.
- What changed: I replaced `bounded-scope` with `scope-feasible`, added difficulty classifications, and documented the change in `scan-notes.md`. I would also add an explicit open-issue check to prevent completed issues from receiving an accept verdict.

---

## Understanding the Issue

**Problem Description**

Murmury's answer-generation prompts depend on configurable Style settings, but there are no dedicated automated tests verifying that these settings are included correctly. A regression could silently cause answers to ignore user preferences.

**Expected Behavior**

The instructions() function should correctly incorporate all supported style settings into the generated prompt, including limits, sources, tone, language, format, and custom instructions. Invalid tone and format values should use their documented fallbacks.

**Current Behavior**

The instructions() function generates prompts, but tests/test_instructions.py does not exist. The default prompt smoke test succeeded, but the required settings and edge cases are not covered by dedicated tests.

**Affected Components**

- copilot/answer.py: Style, PRESETS, TONES, FORMATS, ANSWER_INSTRUCTIONS, and instructions().
- tests/test_instructions.py: New test file required by issue #40.

**Toolkit**

- Tool: I used GitHub repository inspection and repro-check to investigate Murmury issue #40, review the relevant source code, and validate my reproduction evidence.
- What it produced: I identified the instructions() function in copilot/answer.py and confirmed that the dedicated tests/test_instructions.py file was missing. The reproduction check accepted my evidence.
- Difficulties: The full test suite could not run on Windows because of macOS-specific dependencies. The isolated smoke test also could not verify every Style setting.
- What changed: I narrowed the contribution to automated tests for prompt-generation settings and planned to use an isolated uv environment to avoid unrelated platform dependencies.

---

## Reproduction Process

**Environment Setup**

I used Windows with Git Bash, Python 3.13.3, and uv 0.12.22. I forked the Murmury repository to `rigshank/murmury` and cloned it locally on the `main` branch. The project requires macOS for its full application, but issue #40 states that the Python prompt tests should work on any operating system.

I attempted to run the repository's required pytest command. However, dependency installation failed because `pyobjc-framework-vision` is a macOS-specific dependency. To work around this limitation, I used uv's `--no-project` option to run the relevant prompt-generation function with only the `anthropic` dependency.

**Steps to Reproduce**

1. Clone my fork and navigate to its root:

   ```bash
   git clone https://github.com/rigshank/murmury.git
   cd murmury
   ```

2. Check whether the requested test file exists:

   ```bash
   test -f tests/test_instructions.py && echo "Test file exists" || echo "MISSING: tests/test_instructions.py"
   ```

3. Run the test command specified in issue #40:

   ```bash
   uv run --with pytest python -m pytest tests -q
   ```

4. Test the prompt-generation function independently:

   ```bash
   uv run --no-project --with anthropic python -c "from copilot.answer import Style, instructions; p = instructions('test meeting', Style()); print('Prompt generated:', len(p), 'characters'); print('Contains source tags:', '[source]' in p)"
   ```

**Reproduction Evidence**

- **Commit showing reproduction:** none
- **Screenshots/logs:**

  The requested test file was missing:

  ```text
  MISSING: tests/test_instructions.py
  ```

  The full test command failed during dependency installation:

  ```text
  error: Failed to build `pyobjc-framework-vision==12.2.2`
  TypeError: argument of type 'NoneType' is not iterable
  ```

  The isolated prompt-generation test succeeded:

  ```text
  Prompt generated: 5112 characters
  Contains source tags: True
  ```

- **My findings:** I confirmed that the dedicated automated test file requested in issue #40 does not exist. The `instructions()` function successfully generated a prompt using the default `Style()` settings, and the output contained source tags. However, this smoke test does not verify all the settings described in the issue. The full pytest suite could not run on Windows because the project's development dependencies include macOS-specific packages. I did not identify or verify any production-code defect. The contribution will focus on adding the missing automated tests.

**Toolkit**

- Tool: I used `repro-check` to evaluate my claim comment and reproduction report against the issue's requirements and my installed rubric.
- What it produced: The tool checked the environment, reproduction steps, observed behavior, honesty of the findings, repository conventions, and claim accuracy. The final evaluation returned `accept` with all six checks passing.
- Difficulties: The full test suite could not start on Windows because of macOS-specific dependencies. The first combined `repro-check` evaluation also incorrectly rejected my claim because it interpreted a promised progress update as a promised completion date.
- What changed: I used an isolated uv environment to test prompt generation and documented the full-suite limitation. I then asked `repro-check` to apply the exact `claim-correct` rule, distinguishing a follow-up timeframe from a promised fix. The subsequent evaluation accepted the package without changing the draft or rubric.

---

## Solution Approach

**Analysis**

The issue is a gap in automated test coverage, not a confirmed production-code defect.

The instructions(situation, style) function in copilot/answer.py constructs prompts using the Style dataclass, ANSWER_INSTRUCTIONS template, TONES and FORMATS dictionaries. It dynamically produces line slots, applies number and word limits, handles language and tone choices, removes source references when disabled, and inserts optional user instructions.

The baseline reproduction confirmed that tests/test_instructions.py is missing. An isolated smoke test generated a prompt successfully, but did not validate the complete behavior requested by issue #40.

**Proposed Solution**

Create tests/test_instructions.py containing pytest tests for every requirement in issue #40.

Use parametrization for tones, presets, and formats. Test normal inputs, defaults, fallback behavior, and edge cases.

Do not modify copilot/answer.py unless testing identifies a real production bug. Do not add dependencies, network calls, API requests, or changes to the user interface.

**Implementation Plan**

1. Create tests/test_instructions.py.
2. Test default SAY word limits and three source-tagged line slots.
3. Test lines=1 and numbers=0 or numbers=2.
4. Verify sources=False removes source-reference tags.
5. Verify language selection and empty-language behavior.
6. Test every supported tone and unknown-tone fallback.
7. Test every supported format and unknown-format fallback.
8. Test populated, blank, and whitespace-only extra instructions.
9. Test every PRESETS and FORMATS combination for unresolved template braces.
10. Run the focused pytest tests and record results.
11. Attempt the repository's official test command and document any Windows dependency limitation.
12. Review the diff and keep the contribution limited to the accepted scope.

**Toolkit**

- Tool: I used my plan-check skill in Claude Code to evaluate plan.md and plan-comment.md against Murmury issue #40.
- What it produced: The evaluation returned accept, with all three required checks passing: Diagnosis, Scope, and Test.
- Difficulties: The full repository test suite could not run on Windows because of macOS-specific dependencies, so my plan needed to account for this limitation.
- What changed: I included a focused pytest command using an isolated uv environment and documented the full-suite limitation. The plan was accepted without additional revisions.

---

## Testing Strategy

I tested the new `tests/test_instructions.py` file using an isolated uv environment to avoid the project's macOS-only dependencies.

The automated tests verify default Style settings, SAY and line word limits, line counts, number limits, source tags, language selection, supported and unsupported tones, supported and unsupported formats, extra instructions, and unresolved template braces across presets and formats.

**Before:** The dedicated test file did not exist. The initial smoke test only confirmed that a default prompt could be generated.

**After:** The focused pytest command passed all 26 tests in 40.85 seconds.

**Command:**
`uv run --no-project --with pytest --with anthropic python -m pytest tests/test_instructions.py -q`

**Output:**
`26 passed in 40.85s`

I also attempted the repository's official command:
`uv run --with pytest python -m pytest tests -q`

It failed with exit code 1 while building the macOS-specific `pyobjc-framework-coreml` dependency on Windows. This prevented the full suite from starting, so I cannot claim that it passed.

**Toolkit**

- Tool: I used pytest and uv to validate the implementation and Git Bash to inspect the results.
- What it produced: All 26 new tests passed. The official test command exposed a platform-specific dependency installation failure.
- Difficulties: The full project test suite could not start on Windows because of macOS-specific dependencies.
- What changed: I used an isolated uv environment to execute the relevant tests without modifying project dependencies. I recorded the official suite's failure separately.

---

## Implementation Notes

**2026-11-09**

I investigated Murmury's `instructions()` function and confirmed that the requested automated test file was missing. I identified the Style settings and fallback behavior that needed coverage.

**2026-11-09**

I prepared my implementation plan and used `plan-check` to evaluate it. All three required checks passed: Diagnosis, Scope, and Test. The final verdict was `accept`.

**2026-11-09**

I created the branch `test/40-instructions-settings` and added `tests/test_instructions.py`. The new tests cover prompt-generation settings, defaults, edge cases, and fallbacks without changing production code.

**2026-11-09**

I ran the focused test suite, which passed all 26 tests in 40.85 seconds. The official test command failed during installation of a macOS-specific dependency. I documented this limitation and committed the implementation as `0948701`.

**2026-11-09**

I ran the ship-gate manually after preparing my pull request description. The gate successfully ran the focused tests and evaluated the branch with pr-precheck. All seven checks passed, and the final verdict was PASS. I pushed my implementation branch and opened PR #41 against GianlucaBave/murmury with the tested description.

**2026-11-10**

I installed the Unit 8 PR monitor and configured a playbook with repository-specific communication rules. The monitor successfully identified Murmury PR #41 as merged and generated a thank-you draft. I revised the draft to remove inaccurate statements about maintainer review and issue closure. The manual voice-guard check passed, although the automatic Claude Code hook encountered a Python-not-found error. I posted the corrected thank-you comment on the merged PR and documented the outcome in my contribution story.

**Toolkit**

- Tool: I used Git Bash, pytest, uv, and Git to implement, validate, and commit the changes.
- What it produced: A new 115-line test file, 26 passing focused tests, and a commit containing only the planned test addition.
- Difficulties: The official test suite was blocked by macOS-specific Python dependencies.
- What changed: I used the isolated test environment, kept the implementation limited to the new test file, and documented the platform limitation for review.

---

## Pull Request

**PR Link:** https://github.com/GianlucaBave/murmury/pull/41

**PR Description:**

Added automated tests for Murmury's answer instruction settings to address issue #40.

The PR introduces tests/test_instructions.py, covering default settings, word and number limits, line counts, source references, language, tone, format fallbacks, custom instructions, and unresolved template braces across presets and formats.

All 26 focused tests passed using an isolated uv environment. The official repository test command was attempted but could not run because of macOS-specific dependencies on Windows.

The PR contains no production-code changes or new project dependencies. I disclosed my AI assistance in the PR description.


**Maintainer Feedback:**

- 2026-11-10: PR #41 was merged without a written maintainer review or requested changes. The CLA assistant confirmed that the contributor agreement was signed.
- 2026-11-10: I reviewed the PR monitor's generated thank-you draft, removed inaccurate statements, manually verified the revised message with voice-guard, and posted my response: https://github.com/GianlucaBave/murmury/pull/41#issuecomment-6775343812

**Status:** Merged

**Toolkit**

- Tool: I used the ship-gate, pr-precheck, Git, GitHub CLI, and Claude Code to validate and open the pull request.
- What it produced: The ship-gate returned PASS, with all seven PR checks passing. Pull request #41 was successfully opened against the original Murmury repository.
- Difficulties: The automatic hook did not demonstrate successful interception during the earlier help-command test. The official test suite was also blocked by macOS-specific dependencies on Windows.
- What changed: I verified the ship-gate manually before opening the PR, documented the testing limitation, and ensured that the PR targeted the original repository rather than my fork.

---

## Learnings and Reflections

Through this contribution, I improved my ability to understand an unfamiliar Python codebase and write automated tests using pytest. I learned how Murmury constructs AI prompts from different answer settings and how to test default values, edge cases, and fallback behavior. I also gained experience using uv to create an isolated testing environment, which allowed me to run all 26 focused tests despite platform-specific dependency problems on Windows. Additionally, I practiced using Git branches, GitHub pull requests, Claude Code, and custom tools such as issue-scout, plan-check, ship-gate, voice-guard, and PR monitor to manage an open-source contribution from issue selection through merge.

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]

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
**Pull Request:** [GitHub PR link, once opened]

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

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

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

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

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

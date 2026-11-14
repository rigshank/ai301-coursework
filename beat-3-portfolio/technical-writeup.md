# From Issue Selection to a Merged Open-Source Contribution: Murmury and issue-scout

## Overview

**Write one paragraph (approximately 100–150 words) covering:**

- **Project:** Murmury, an open-source project that generates AI answer prompts using configurable settings.
- **Problem:** Issue #40 requested automated tests to verify that settings such as tone, language, format, source references, and word limits correctly affect generated prompts.
- **Your contribution:** Added `tests/test_instructions.py`, containing 26 passing test cases covering those settings, defaults, and edge cases.
- **Your AI tool:** Developed and improved `issue-scout`, which evaluates GitHub issues against criteria such as repository activity, feasibility, existing claims, and AI-contribution policies.
- **Your role:** Authored the tool's rubric and search strategy, reviewed its results, improved its selection rules, and used it to evaluate Murmury issue #40.
- **Outcome:** Your pull request was merged on November 10, 2026.

**Links to include:**

- Issue: https://github.com/GianlucaBave/murmury/issues/40
- PR: https://github.com/GianlucaBave/murmury/pull/41

## What happened

### 1. Building and evaluating issue-scout

**Describe your initial tool development (October 16, 2026).**

- Upgraded your earlier `issue-select` skill into `issue-scout`.
- Developed a custom rubric with six required checks and three preferred checks.
- Configured searches for open Python and JavaScript issues carrying the `good first issue` label.
- The initial scan returned 200 issues.
- Removing assigned issues reduced that number to 151.
- The scout evaluated the top five candidates and accepted all five.

**Original evidence to paste from `scan-output(1).md`:**

> Your two queries returned 200 results (100 Python and 100 JavaScript "good first issue" searches, no overlap between them). Dropping assigned issues left 151

Explain that receiving five accepted issues did not necessarily mean the ranking strategy was effective.

### 2. Discovering problems and improving the tool

**Describe what went wrong with the first scan.**

- The ranking favored extremely new issues with zero comments.
- Three of the top five results came from the same repository.
- Some issues belonged to Python repositories but required unfamiliar Ansible or Jinja work.
- The rubric did not sufficiently detect issues whose requested changes had already been completed.

**Your actual changes:**

- Updated the ranking strategy to select at most one issue per repository in the top five.
- Removed the earlier PathReview-specific claim exception.
- Later replaced the `bounded-scope` check with `scope-feasible`, allowing bounded tasks with open design questions to be considered rather than automatically rejected.

**Original evidence to paste from `scan-notes.md`:**

> sameeralam3127/linux-vitals took 3 of the 5 slots because its owner files many small labeled issues in batches.

**Original evidence from the updated `search-strategy.md`:**

> After applying the ranking rules above, select at most one issue per repository for the top 5.

Explain why you made this change: to improve repository diversity rather than repeatedly recommending issues from one project.

**Important:** You did not rerun the complete broad scan after the search-strategy improvement, so do not claim that the updated strategy was experimentally proven to produce better overall results.

### 3. Evaluating and selecting Murmury issue #40

**Describe the November 9, 2026 evaluation.**

- Murmury issue #40 requested a new test file, `tests/test_instructions.py`.
- The work matched your Python experience and interest in AI.
- You directly evaluated the issue using your updated `issue-scout` rubric.
- The scout accepted the issue and classified its difficulty as `straightforward`.
- All six required checks passed.

**Original evidence from `scan-output(1).md`:**

> Verdict: accept  
> Difficulty note: straightforward

The tool's evidence identified a bounded test file, an active repository, no competing claim at the time of evaluation, and no stated AI-use restriction.

Clarify that Murmury was **evaluated directly**, not discovered by your original 200-issue broad scan.

### 4. Investigating and implementing the tests

**Describe your technical work.**

- Examined `copilot/answer.py`, particularly the `instructions()` function and the `Style` configuration.
- Confirmed the requested `tests/test_instructions.py` file was missing.
- Studied the project's `TONES`, `FORMATS`, and `PRESETS`.
- Created a new test file covering:
  - Default prompt settings and line counts.
  - Custom word and number limits.
  - Source-reference removal.
  - Language selection.
  - Valid and invalid tone settings.
  - Format fallback behavior.
  - Custom instructions and whitespace-only inputs.
  - Unresolved template braces across presets and formats.

**Code evidence from PR #41:**

One test checks that an unsupported tone falls back to the project's default confident tone:

`assert TONES["confident"] in prompt`

Explain what this assertion verifies and why fallback testing matters. For example, the purpose is to check that unsupported user settings do not accidentally bypass the intended prompt rules.

### 5. Testing and validation

**Describe the actual testing process.**

- The repository's official test command could not run locally on Windows because dependency installation encountered macOS-specific `pyobjc` packages.
- Instead of modifying production dependencies, you used `uv` with `--no-project` to run the relevant tests in an isolated environment.
- All 26 focused tests passed.

**Actual test command:**

`uv run --no-project --with pytest --with anthropic python -m pytest tests/test_instructions.py -q`

**Original recorded output:**

`26 passed in 40.85s`

Explain that the focused test suite validated the new tests, but it did not establish that the entire repository test suite passed.

### 6. Submitting the pull request and final outcome

**Describe the end of the contribution.**

- Created the branch `test/40-instructions-settings`.
- Added the new test file without modifying production code.
- Committed the implementation as `0948701`.
- Opened Murmury PR #41.
- Included the test results, Windows limitation, and AI-assistance disclosure in the PR description.
- On November 10, 2026, PR #41 was merged, closing issue #40.

Mention that no written maintainer review or requested changes were recorded. Do not imply that the maintainer requested technical revisions.

Explain the result: the project now contains automated tests checking how its answer settings are represented in generated AI prompts.

## Limitations and what's next

**Write approximately two paragraphs addressing both subjects.**

### Contribution limitations

- You could not run the full repository suite locally because some development dependencies are macOS-specific.
- The 26 new tests passed, but this does not prove the entire application works under every environment.
- The contribution added tests rather than changing production behavior.

**Next steps to discuss:**

- Run the complete suite in a compatible macOS environment.
- Extend testing when Murmury adds new answer settings or formats.

### issue-scout limitations

- The tool initially favored extremely recent issues and duplicated repositories.
- Its rubric could accept issues that were already closed or whose requested work had already been completed.
- The improved search strategy was not validated through another complete broad scan.

**Next steps to discuss:**

- Add a required `issue-open` check.
- Add a check confirming the requested work has not already been implemented.
- Rerun the broad issue search and compare results before and after these improvements.

**Conclude in your own words with what this experience taught you about validating AI-generated recommendations, automated testing, and contributing to an unfamiliar open-source repository.**

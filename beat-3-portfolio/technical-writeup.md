# From Issue Selection to a Merged Open-Source Contribution: Murmury and issue-scout

## Overview

During my CodePath AI301 Open Source Capstone, I developed an AI-assisted GitHub issue selection tool called `issue-scout` and contributed automated tests to the open-source project Murmury. Murmury generates AI answer instructions based on configurable settings, but its issue #40 identified a gap in automated tests for those settings. I contributed a new Python test file that verifies how different configurations affect generated prompts, including language, tone, format, word limits, and source references. I also developed and improved `issue-scout` to evaluate potential contributions based on repository activity, issue scope, availability, and contribution policies. After evaluating Murmury issue #40 with my tool, I implemented and tested the requested changes. All 26 focused tests passed, and my [pull request #41](https://github.com/GianlucaBave/murmury/pull/41) was merged on November 10, 2026, closing [issue #40](https://github.com/GianlucaBave/murmury/issues/40).

## What happened

### Building and improving issue-scout

I started by upgrading my earlier `issue-select` skill into `issue-scout` during Unit 5. My goal was to develop a tool that could search GitHub for potential first contributions, evaluate each issue against a custom rubric, and help me find work that matched my programming skills and interests.

I wrote the tool's rubric and search strategy. The rubric included six required checks and three preferred checks, covering factors such as repository activity, issue feasibility, existing claims, and whether AI-assisted contributions were allowed. I configured two GitHub searches targeting open Python and JavaScript issues labeled `good first issue`. My search strategy initially prioritized issues with fewer comments and more recent activity.

On October 16, 2026, I ran my first broad scan. The scan returned 200 issues, and filtering out assigned issues reduced that number to 151. The tool evaluated the top five candidates and accepted all five.

The original output in `scan-output.md` recorded:

> Your two queries returned 200 results (100 Python and 100 JavaScript "good first issue" searches, no overlap between them). Dropping assigned issues left 151

Although every graded issue passed the required checks, I noticed problems with the results. All five issues had been opened less than two hours before the scan, and three came from the same repository. Some issues also belonged to Python repositories but required Ansible or Jinja development, which did not match the type of work I wanted to do.

My `scan-notes.md` recorded this problem:

> sameeralam3127/linux-vitals took 3 of the 5 slots because its owner files many small labeled issues in batches.

I changed `search-strategy.md` to limit the top five candidates to one issue per repository. The updated rule stated:

> After applying the ranking rules above, select at most one issue per repository for the top 5.

I also removed an outdated PathReview-specific claim exception from my rubric because real open-source repositories may have different expectations about existing contributors' claims.

During later evaluations, I discovered that the rubric could accept issues whose requested work had already been completed. I also found that the original `bounded-scope` check could be too strict about open design questions. I replaced it with `scope-feasible`, which focuses on whether an issue describes one identifiable piece of work. Open design questions could then affect the difficulty note instead of automatically causing rejection.

These changes made the rubric better suited to evaluating different repositories, although I did not rerun the complete broad scan after updating the search strategy. This was an important limitation of my tool's evaluation.

### Selecting Murmury issue #40

On November 9, 2026, I directly evaluated Murmury issue #40 using `issue-scout`. Unlike my initial broad scan, this was a targeted evaluation of a specific candidate.

The issue requested a new Python test file, `tests/test_instructions.py`, to verify that Murmury's configurable answer settings were correctly incorporated into generated AI prompts. The work interested me because it involved Python, automated testing, and AI-related functionality.

My scout returned the following result, recorded in `scan-output.md`:

```text
Verdict: accept
Difficulty note: straightforward

- scope-feasible: pass — One bounded test file with clear acceptance criteria.
- issue-unclaimed: pass — No assignees, comments, or linked PRs.
- ai-contribution-allowed: pass — No AI restriction in repository policy.
```

All six required checks passed. The tool determined that the repository was active, the requested work was feasible, and there was no competing claim at the time of evaluation. I decided to proceed because the issue had clear acceptance criteria and matched my technical interests.

This was a useful result from `issue-scout`: it helped me evaluate a real contribution opportunity using the rubric I had developed instead of relying only on the issue's title or difficulty label.

### Investigating and implementing the contribution

After selecting the issue, I forked Murmury and investigated its Python codebase. I examined `copilot/answer.py`, particularly the `instructions()` function and the `Style` configuration. The function constructs prompts using settings such as word limits, number limits, source references, language, tone, format, and additional instructions.

I checked the repository for the test file requested by the issue. My initial reproduction produced:

```text
MISSING: tests/test_instructions.py
```

I also ran an isolated smoke test that successfully generated a default prompt and confirmed that source tags appeared. However, this only tested the default configuration and did not verify all the settings described in the issue.

I then prepared an implementation plan focused entirely on adding automated tests. My plan did not include modifying production code or introducing new project dependencies.

On November 9, I created the branch `test/40-instructions-settings` and implemented `tests/test_instructions.py`. The new file contained tests for default settings, line counts, word and number limits, source references, language selection, tone and format fallbacks, extra instructions, and unresolved template braces.

For example, I added a test to verify that an unsupported tone falls back to the expected default:

```python
def test_unknown_tone_fallback():
    prompt = make_prompt(Style(tone="unknown"))

    assert TONES["confident"] in prompt
```

This test checks that Murmury includes the expected default tone instructions when an unsupported tone is supplied. I also used pytest parametrization to cover multiple supported tones, presets, and formats without writing a separate test function for every combination.

The resulting pull request added 115 lines in one new test file, without modifying existing production code.

### Testing and validating the implementation

One of my main challenges was running Murmury's tests on Windows. The repository includes macOS-specific dependencies, and its official test command failed during dependency installation.

The official command was:

```bash
uv run --with pytest python -m pytest tests -q
```

The dependency installation failed while attempting to build a `pyobjc` dependency. This meant the complete repository test suite could not start in my local environment.

Instead of modifying Murmury's dependencies, I used `uv` with the `--no-project` option to run the relevant tests in an isolated environment.

My focused test command was:

```bash
uv run --no-project --with pytest --with anthropic python -m pytest tests/test_instructions.py -q
```

The recorded result was:

```text
26 passed in 40.85s
```

This confirmed that all 26 focused tests passed in my local environment. However, it did not prove that the full repository test suite passed. I documented that distinction rather than claiming the entire project had been validated.

Before opening my pull request, I also used the CodePath `ship-gate` and `pr-precheck` tools to validate my branch. The manual gate returned PASS with all seven checks passing. I used manual verification because the earlier automatic hook test had not demonstrated reliable interception.

### Opening the pull request and seeing the result

After validating the changes, I committed the new test file as commit `0948701`, pushed my branch, and opened [Murmury PR #41](https://github.com/GianlucaBave/murmury/pull/41).

In the pull request description, I explained the tests I added, included the successful focused test result, and disclosed the Windows dependency limitation. I also disclosed my use of AI assistance during development while retaining responsibility for reviewing and validating the changes.

On November 10, 2026, the pull request was merged into the original Murmury repository, automatically closing issue #40. There was no recorded written maintainer review or requested code revision before the merge.

The contribution's result was a new automated test suite covering Murmury's configurable answer instructions. The tests provide regression coverage for behavior that previously lacked dedicated tests.

My `issue-scout` tool also produced a useful real-world outcome by helping me evaluate and select the contribution. The experience demonstrated that a custom AI-assisted workflow could support issue selection, but that its recommendations still needed to be checked against repository facts and actual implementation requirements.

## Limitations and what's next

One limitation of my Murmury contribution was that I could not run the full repository test suite on Windows because of macOS-specific dependencies. Although all 26 focused tests passed, I could not verify the complete application locally. My contribution also focused on adding test coverage rather than changing production functionality, so the tests help detect future regressions but do not independently establish that every prompt configuration is free of defects. As a next step, I would run the complete suite in a compatible macOS environment and extend the tests when Murmury introduces additional answer settings or formats.

My `issue-scout` tool also has limitations. Its original search strategy favored extremely recent issues and sometimes recommended several issues from the same repository. Although I updated the ranking rules, I did not perform another complete broad scan to measure the effect of those changes. Additionally, my rubric could accept closed issues or issues whose requested changes had already been implemented. I would improve the tool by adding a required `issue-open` check and a check confirming that the requested work is still needed. I would then rerun the same issue-search process and compare the results before and after those changes.

This project helped me develop practical experience with open-source contributions, automated testing, and using AI-assisted tools as part of a development workflow. I learned that finding a suitable issue requires more than searching for beginner-friendly labels, and that passing tests depends on understanding exactly what was executed and what was not. Most importantly, I learned to validate AI-assisted recommendations, document limitations accurately, and use evidence to support technical decisions.

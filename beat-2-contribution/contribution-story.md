# Contribution: [Issue Title]

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

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

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

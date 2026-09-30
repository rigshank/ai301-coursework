# Evidence guide: where proof lives in a reproduction package

Every rubric check needs an evidence source. This guide maps the five proof families from the Unit 2 assignment to concrete places to inspect in an eval bundle and in live mode. If a rubric check names evidence that is not listed here, it must still point to a source a grader can actually inspect.

## Environment

A reproduction is only interpretable if the reader knows what environment produced it.

| Signal | In live mode | In the eval bundle |
|---|---|---|
| Platform / runtime | the student's draft repro comment plus the repo's README, setup docs, or issue text when they identify an OS, runtime, framework, browser, service, or tool requirement | the repro report's environment/setup record plus the issue context |
| Relevant versions | the student's draft plus package manager files, setup docs, or issue text when a version is part of the reported behavior | the repro report's environment/setup record plus any version information in the issue context or Repo facts |
| Branch / commit | the local checkout or draft report when the tested revision matters; compare with the issue if it names a revision | the repro report and any repository revision information provided in the package |
| Configuration / dependencies | setup files, environment instructions, feature flags, commands, or configuration named by the issue/repo | the repro report's environment/setup section and relevant Repo facts/setup material |
| Environment mismatch | compare the student's tested setup with the issue's stated target | compare the repro report with the issue context |

How to grade what you find:

- **Pass** when the report records every environment detail the issue or repository explicitly makes relevant to the behavior.
- A difference from the issue's environment does not automatically fail if the report states the difference clearly enough for a reader to judge the result.
- **Fail** when a material environment condition required to interpret or rerun the reproduction is missing.

## Steps

The goal is not a polished template; it is a sequence a stranger can actually follow.

| Signal | In live mode | In the eval bundle |
|---|---|---|
| Starting state | the student's draft repro comment plus the repo's normal setup/run instructions | the repro report's setup/starting-state information |
| Ordered actions | commands, clicks, requests, inputs, files, routes, or other actions in the draft | the repro report's reproduction procedure |
| Trigger | the final action that should cause the issue's reported behavior | the repro report read against the issue context |
| Required setup | README, contribution docs, setup scripts, package instructions, or configuration docs | Repo facts/setup material included in the package |

How to grade what you find:

- **Pass** when another person can move from the stated starting state to the issue trigger without inventing a material step.
- A step can be omitted only when it is unambiguously supplied by the repository's normal documented setup.
- **Fail** when the steps test a different scenario from the issue or leave out an action/configuration needed to reach the trigger.

## Behavior shown

The artifact must prove the issue's behavior, not merely that something went wrong.

| Signal | In live mode | In the eval bundle |
|---|---|---|
| Terminal / command output | output pasted in the draft or linked from the student's evidence | output excerpts in the repro report |
| Logs / stack traces | logs included in the draft or attached evidence | log or trace artifacts in the report |
| Screenshots / visual state | screenshots attached to the draft when the issue is visual | screenshots or visual evidence included in the package |
| Tests / request-response evidence | test results, API responses, browser/network output, or other captured artifacts | the corresponding artifacts in the repro report |
| Issue comparison | the live issue's expected/observed behavior | the issue context in the bundle |

How to grade what you find:

- **Pass for a reproduction** when the artifact produced by the documented steps shows the same behavior described by the issue.
- **Pass for cannot-reproduce** when the relevant behavior was tested under the recorded conditions and the evidence shows it did not occur.
- **Fail** when the artifact demonstrates only an adjacent symptom, different error, or unrelated failure.
- A prose assertion with no supporting artifact does not by itself prove the behavior.

## Honesty

The report's words must stop where the evidence stops.

| Signal | In live mode | In the eval bundle |
|---|---|---|
| Outcome wording | the student's draft conclusion | the repro report's stated result |
| Evidence backing | the artifacts included with the draft | the report's logs, screenshots, output, tests, or other evidence |
| Root-cause claims | any causal explanation in the draft | any causal explanation in the report |
| Scope / certainty | claims about platforms, versions, frequency, or generality | the conclusion compared with the tested environment and artifacts |
| Claim chronology | the claim comment as written before reproduction | the claim comment and package chronology |

How to grade what you find:

- A report may truthfully say **reproduced**, **partially reproduced**, or **could not reproduce**.
- **Pass** when that wording matches the evidence.
- An evidenced cannot-reproduce is a valid outcome.
- **Fail** when a report calls a different failure a reproduction of the issue, states an unverified root cause as fact, or generalizes beyond what was tested.
- Before reproduction, the claim should state planned investigation, not an already-established result.

## Comms

A technically sound reproduction can still be unpostable if it violates the repository's stated communication rules.

| Signal | In live mode | In the eval bundle |
|---|---|---|
| Claim specificity | the student's draft claim read against the issue title/body/thread | the claim comment read against the issue context |
| Contribution rules | `CONTRIBUTING.md`, linked contributor guidance, or repository docs | contribution-policy text or Repo facts supplied in the bundle |
| AI-use disclosure | dedicated AI-policy files, contributor docs, PR/issue templates | any quoted or summarized AI-assistance requirement in the package |
| Issue-thread conventions | the live issue thread and repository templates | issue comments, templates, or policy information included in the bundle |
| Independent reproduction | the student's own steps and evidence | the student's repro report rather than another person's assertion |

How to grade what you find:

- The claim should name the issue-specific work the student will investigate and promise a follow-up report.
- It should **not** promise a fix or a completion date.
- The repro comment should report the student's own evidence; "same as above" or "can confirm" without independent proof is not enough.
- **Pass** when all stated repository conventions are followed, including AI-assistance disclosure when required.
- In Path Review live mode, another student's claim does not block a new student claim, but each student still needs an independent reproduction.

# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives:
- In eval mode, look at the cause or diagnosis stated in the Candidate plan and compare it with the steps and observed behavior in the Repro evidence.
- Use the Issue section as context for the behavior the plan is trying to explain.
- In live mode, compare the diagnosis in the draft plan with the student's posted repro comment and the issue thread.

What good looks like:
The diagnosis matches the behavior demonstrated by the repro evidence without contradicting the reproduction steps or observed result. The cause should be supported by what the reproduction shows rather than ignoring or contradicting it.

## Scope

Where it lives:
- In eval mode, look in the Candidate plan for what is in scope, what is out of scope, and the files, areas, or changes the plan proposes.
- Compare that scope with the Issue and Repro evidence to make sure it addresses the reproduced problem.
- In live mode, look at the scope and proposed changes in the draft plan and compare them with the issue and posted repro comment.

What good looks like:
The plan clearly states what will be changed, what will not be changed, and keeps the work bounded to the reproduced issue rather than expanding into unrelated changes.

## Executability

Where it lives:
- In eval mode, look in the Candidate plan for the files or areas to change and the described implementation approach.
- In live mode, look at the same information in the student's draft plan.

What good looks like:
The plan gives enough concrete information about where and how the change will be made that another person could begin implementing it without needing the author to explain the basic approach.

## Test plan

Where it lives:
- In eval mode, look at the test portion of the Candidate plan and compare it directly with the steps and observable behavior in the Repro evidence.
- In live mode, compare the draft plan's test steps with the student's posted repro steps.

What good looks like:
The test follows the same reproduction steps, or their direct equivalent against the changed code, and states the observable result expected after the fix. The test should show that the behavior which previously failed now succeeds.

## Honesty

Where it lives:
- In eval mode, look in the Candidate plan for risks, unknowns, assumptions, and any recorded deviations.
- Compare claims in the Candidate plan with what the Issue and Repro evidence actually establish.
- In live mode, look at the draft plan's risks and unknowns and, after the build, its Deviations section.

What good looks like:
The plan does not present unsupported assumptions as facts. Important risks or unknowns are stated when they exist, and any difference between the planned work and the actual build is recorded honestly as a deviation.

## Comms

Where it lives:
- In eval mode, read the Candidate plan comment against the Issue, Thread highlights, and Repo facts.
- Use Repo facts for stated contribution requirements or AI-use policies.
- In live mode, read the draft plan comment against the live issue thread and the repository's stated contribution rules.

What good looks like:
The plan comment accurately describes the proposed work, responds to relevant information from the issue thread, and follows the repository's stated contribution requirements. It should not contradict the plan or ignore relevant maintainer instructions.
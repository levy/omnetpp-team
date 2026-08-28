# Human-in-the-loop gates

A gate is a point where an agent stops and asks the human. The agent does not
proceed on an assumption. The agent states the question in prose, gives its
recommendation, and waits.

## The gates

1. **The goal and its priority.** The human opens a task. An agent does not
   invent work.
2. **The plan.** The software architect writes a plan file into `plan/pending/`
   of the target repository. No code changes before the human approves it.
3. **Structure.** A new package, a new layer, a new module, or a change to a
   dependency rule.
4. **A seal.** Only the human seals a file. Only the human gives permission to
   change a sealed file. The requirements and seal auditor reports first. It
   never offers to seal before it reports the audit result.
5. **The test baseline.** A new `@test_broken` marker, or any move of a
   recorded baseline count.
6. **A commit outside a plan branch.** On its own plan branch an agent commits
   its own work, one commit per plan step. The approved plan is the permission.
   In the checkout that the human shares, an agent commits only when the human
   asks. Name explicit paths in every commit: other work there can be
   uncommitted.
7. **A merge to `main`.** In any of the three repositories.
8. **A large cleanup.** The code quality steward proposes. The human accepts a
   rule into the written rules document before any sweep starts.
9. **An outward action.** A push, a tag, a release, or anything that leaves
   this machine.

## How to ask

Write one short paragraph. State the choice, state your recommendation, state
what you need to proceed. Do not write a survey of every option.

Do not stop for a question that you can answer from the code, the plan, or a
sensible default. State the assumption and continue.

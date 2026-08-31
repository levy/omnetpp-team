# The development workflow

This document describes how the human develops the three repositories. An agent
follows it.

The team is described in [README.md](README.md). Who does a task is in
[routing.md](routing.md). What an agent must not decide alone is in
[policy/hitl-gates.md](policy/hitl-gates.md). This document is the cycle that
those three serve.

## 1. The cycle

Every change walks the same seven steps.

1. The human states a goal.
2. The human and an agent discuss the goal until the shape is clear.
3. An agent writes a plan file. The human approves it.
4. An agent implements the plan in a separate git worktree, one commit per step.
5. An agent runs the narrowest test. The human checks the result in the live editor.
6. The human lands the branch on `main`.
7. An agent moves the plan to `plan/done/` and adds one line to the history.

A trivial change can skip steps 2 and 3. Do not skip any other step.

## 2. The goal and the discussion

The human opens every task. An agent never invents work.

An agent answers a question with prose. A question is never a reason to change
code.

For a task that is not trivial, the human starts a discussion. The human writes
the goal, the constraints, and what is out of scope. The agent gives options and
one recommendation. The human decides one point at a time. The human often says
"do 1 and 2 first, then we talk".

The discussion ends when the human asks for a plan.

## 3. The plan

A plan is one Markdown file in `plan/pending/` of the repository that owns the
work. A task that touches two repositories gets one plan file in each. Each
file names the other.

A plan file holds these sections. Not every plan needs all of them.

| Section | What it holds |
| --- | --- |
| Title | One sentence that states what the change delivers. |
| Status | Pending, or done with the date and the branch. |
| Requirements | The accepted requirements that the plan advances. |
| Current state | What exists today, and what is missing. |
| Decisions | Each decision with its reason, and the option that lost. |
| Phases | The steps of the work, in the order of the work. |
| Testing | Which test proves each phase. |
| Not in this plan | The work that a later plan takes. |
| What must not change | The invariants that the work must keep. |
| Open questions | What the human must still decide. |

The human reads the plan and answers with corrections. The agent rewrites the
plan. This repeats until the human approves it. The human writes "update plan:"
and then the correction.

**No code changes before the human approves the plan.**

## 4. The work

Implement the plan in a git worktree beside the repository, never in the main
checkout. A worktree is a sibling directory in `workspace/`.

1. Commit after each step of the plan. The approved plan is the permission.
2. Name explicit paths in every commit. The human works in the same checkout.
3. Mark the step done in the plan file in the same commit.
4. Record in the plan every decision that the work discovers.
5. Stop and ask when a design decision comes up.
6. Stop after an important step when the human asks to verify it.

A sealed file is frozen. Stop and ask before you change one.

A change in a lower repository is not usable in a higher one until it lands on
`main` of the lower one. The direction is `projectured-julia`, then
`omnet-julia`, then `inet-julia`.

## 5. The check

Run the smallest test that covers the change. Never run the full suite by
default. Bound a long run with a timeout and cap its memory.

For a wide change, measure the suite before the change and after it. A count
that moves is a regression until an analyst proves that it is not.

The human then runs the editor and reports what is wrong. A headless test hides
faults that only the live editor shows.

Report the result exactly. If a test fails, say that it fails and show the
output. If you skipped a step, say that you skipped it.

## 6. Land the work

The human decides when the work lands. The sequence is always the same.

1. Rebase the branch on `main`.
2. Fast-forward `main` to the branch.
3. Remove the worktree and the branch.
4. Push when the human asks.

Then move the plan file with `git mv` from `plan/pending/` to `plan/done/`, and
add one dated line to `documentation/history/design-history.md` that links to
it.

## 7. The documentation

Each repository owns its own documents. `documentation/` holds one folder per
question that a reader brings: requirement, design, rule, evidence, study,
guide, history. A package guide lives beside its package.

Three rules hold everywhere.

- **A document describes what is.** History belongs in `history/`, never in the
  document and never in a source comment.
- **Cite, do not repeat.** Two copies of a statement drift, and then one is
  wrong.
- **Every document carries a header line**: its kind, its status, and the
  documents it stands on.

When a change makes a guide wrong, fix the guide in the same branch. The human
often says "finish documentation first, then land".

## 8. The quality

Four rule documents hold the standard. Read them before a change:
architecture invariants, naming rules, package rules, and code quality rules.

Quality is kept by an audit, not by a sweep. The audit walks the files in load
order, one file at a time, and never forward. For each file:

1. Audit the file against the architecture invariants.
2. Report the result first.
3. Fix what the audit found.
4. Ask the human to seal the file.
5. Flip `⬜` to `🔒` in the same commit as the fixes.

A large document is audited and sealed one section at a time.

The code quality steward runs when a plan moves to `plan/done/`. It proposes;
the human accepts a new rule into the rules document before any sweep starts.

## 9. The words

A commit message is one sentence in the simple present tense. It states what is
true after the commit. It does not describe the act of the change.

```
The selection slot is hidden by its name, not by a supertype
A module declares its own capture seams
The documentation is seven folders, one per question a reader brings
```

Never add a `Co-Authored-By` trailer.

Prose is plain technical English. No sales language, no filler, no metaphor.
Keep the sentences short.

## 10. What only the human does

An agent stops and asks at each of these points.

- Approve a plan.
- Approve a new package, a new layer, or a change to a dependency rule.
- Seal a file, or give permission to change a sealed file.
- Accept a new `@test_broken` marker or a move of a baseline count.
- Merge to `main`.
- Accept a rule before a large cleanup.
- Push, tag, or release.

A commit on the plan branch is not a gate. An agent commits its own work
there, as §4 says. The human owns the merge and the push.

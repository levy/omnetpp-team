---
name: omnetpp-team
description: Route a task through the omnetpp-team agent team. Classify the task, pick the team from the routing table, set the token target, name the human gates, then hand the work to the lead of the target repository. Use it whenever the user types /omnetpp-team, and whenever work spans projectured-julia, omnet-julia or inet-julia and the routing should be explicit.
---

# Run a task through the team

You are the program manager for this task. The role file is
`omnetpp-team/role/program-manager.md`; read it if you need the full mandate.

The team, the routing table and the policies live in
`/home/projectured/workspace/omnetpp-team/`. Point at those files. Never copy
them into your answer.

## 1. Read before you route

- `routing.md` — the three classification questions, the classes, and the team
  of each class.
- `policy/resource-policy.md` — the token target of the class, and the model
  tier of each role.
- `policy/hitl-gates.md` — the nine gates.
- `policy/repository-map.md` — which repository owns which work product.

Read no source file. The lead reads those.

## 2. Classify

Ask the three questions of `routing.md` in order.

1. Does the task change code? If no, the class is Question or Investigation.
2. Does it change an interface, a package, or a layer? If yes, the class is at
   least Architecture change.
3. Does it touch more than one repository? If yes, the class is
   Cross-repository change, whatever else it is.

State the class in one line.

## 3. Report the plan of work, then start

Give the human five short lines and nothing more:

- **Class** — from the table.
- **Team** — the roles the class calls. Use fewer than the template when you
  can. Ask the resource manager before you use more.
- **Target** — from `policy/resource-policy.md`.
- **Gates** — the gates this task will reach, by number.
- **First step** — what happens now.

Then start. Do not wait for approval of the routing itself. Wait only at a gate.

## 4. Route

Hand the work to the lead of the target repository:
`editor-lead` for `projectured-julia`, `simulator-lead` for `omnet-julia`,
`network-model-lead` for `inet-julia`. A cross-repository task goes to all three,
and they agree on the seam before any of them starts.

Call a specialist directly when the class names one and no lead judgment is
needed — a sweep, a measurement, a review.

## 5. Rules that hold for the whole run

- **A question is answered in prose.** A question is never a reason to change
  code.
- **A subagent returns a short structured result**, per
  `policy/output-contract.md`. It never pastes a file body back. That is the
  largest single waste of tokens.
- **Run the narrowest test.** Bound a long run with a timeout, cap its memory,
  and write its output to a file.
- **Stop at a gate.** State the choice, give your recommendation, and wait. The
  gates that appear most: a plan before code, a sealed file, a new
  `@test_broken`, and a merge to `main`.
- **Commit with explicit paths.** An agent commits its own work on a plan
  branch. In the checkout that the human shares it commits only when asked, and
  other sessions may hold uncommitted work there.
- **A sealed file is frozen.** `projectured-julia` keeps the list inline in
  `CLAUDE.md`; `omnet-julia` and `inet-julia` keep it in `SEALING.md`.

## 6. When the task is one word

If the user typed `/omnetpp-team` with no task, do not guess. Ask what the goal
is. You never invent work — that is gate 1.

# Repository map

This repository describes the team. It holds no work. Every work product goes to
the repository that owns it.

## Where a work product goes

| Work product | Where it goes |
| --- | --- |
| A plan | `<repository>/plan/pending/`, then `<repository>/plan/done/` |
| Code | The package it belongs to |
| A test | The test package of the package under test |
| A guide for one package | `<repository>/package/<name>/doc/` |
| A cross-cutting guide | `<repository>/documentation/` |
| The code quality rules of a repository | `<repository>/documentation/code-quality.md` |
| The spend record | `<repository>/plan/spend.md` |
| A role description | `omnetpp-team/role/` |
| A team policy | `omnetpp-team/policy/` |

## When a task touches more than one repository

Split the plan. Each repository gets its own plan file under its own
`plan/pending/`. Each plan file names the other plan files. Do not write one
plan that lives in one repository and changes three.

The three leads agree on the seam first. Then each lead runs its own plan.

## The dependency direction

`inet-julia` depends on `omnetpp-julia`. `omnetpp-julia` depends on
`projectured-julia`. The direction never reverses. A change flows from
`projectured-julia` upward.

A change in a lower repository is not usable in a higher one until it lands on
`main` of the lower one. The `[sources]` entries point at the main checkouts,
not at a worktree.

## The C++ references

`omnetpp-cpp` and `inet-cpp` are read-only references. The migration expert and
the simulation domain expert read them. No agent changes them.

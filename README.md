# omnet-team

This repository describes an agent team. The team develops three repositories:

- `projectured-julia` — the projectional editor.
- `omnet-julia` — the discrete event simulator.
- `inet-julia` — the network models.

This repository holds no work. Plans, code, tests, results, and guides live in
the repository that owns them. See [policy/repository-map.md](policy/repository-map.md).

## The tiers

The team is hierarchical. A tier calls the tier below it.

- **Tier 0 — the human.** The product owner. Sets the goal. Approves each gate.
  Seals files. See [policy/hitl-gates.md](policy/hitl-gates.md).
- **Tier 1 — control.** The program manager and the resource manager. The
  program manager wants the work done. The resource manager wants it cheap.
  The two roles check each other.
- **Tier 2 — leads.** One lead per repository. A lead owns the idiom, the test
  baseline, the seal status, and the commits of one repository.
- **Tier 3 — specialists.** A lead calls a specialist for one task.

The main session acts as the program manager by default. The role file exists
so that you can call the role on its own.

## The roles

| Tier | Role | Model |
| --- | --- | --- |
| 1 | [program-manager](role/program-manager.md) | opus |
| 1 | [resource-manager](role/resource-manager.md) | sonnet |
| 2 | [editor-lead](role/editor-lead.md) | opus |
| 2 | [simulator-lead](role/simulator-lead.md) | opus |
| 2 | [network-model-lead](role/network-model-lead.md) | opus |
| 3 design | [software-architect](role/software-architect.md) | opus |
| 3 design | [requirements-seal-auditor](role/requirements-seal-auditor.md) | opus |
| 3 design | [code-quality-steward](role/code-quality-steward.md) | opus |
| 3 design | [interaction-designer](role/interaction-designer.md) | opus |
| 3 design | [visual-designer](role/visual-designer.md) | sonnet |
| 3 design | [simulation-domain-expert](role/simulation-domain-expert.md) | opus |
| 3 build | [code-expert](role/code-expert.md) | sonnet |
| 3 build | [migration-expert](role/migration-expert.md) | opus |
| 3 build | [refactor-expert](role/refactor-expert.md) | sonnet |
| 3 check | [test-expert](role/test-expert.md) | sonnet |
| 3 check | [baseline-analyst](role/baseline-analyst.md) | sonnet |
| 3 check | [performance-expert](role/performance-expert.md) | opus |
| 3 check | [result-analysis-expert](role/result-analysis-expert.md) | sonnet |
| 3 check | [code-reviewer](role/code-reviewer.md) | opus |
| 3 support | [build-environment-engineer](role/build-environment-engineer.md) | sonnet |
| 3 support | [documentation-writer](role/documentation-writer.md) | sonnet |
| 3 support | [knowledge-curator](role/knowledge-curator.md) | sonnet |
| 3 later | [license-compliance-officer](role/license-compliance-officer.md) | opus |
| 3 later | [triage-agent](role/triage-agent.md) | sonnet |
| 3 later | [release-manager](role/release-manager.md) | sonnet |

Most tasks use two or three roles. [routing.md](routing.md) says which.

[workflow.md](workflow.md) says how a change walks from a goal to `main`.

## How a role file works

A role file is two things at once. It is the description that you read. It is
also the runtime definition of a Claude Code subagent. The frontmatter carries
`name`, `description`, and `model`. The body is the system prompt of the agent.

The file is the only copy. Each repository links to it:

```sh
ln -s ../../omnet-team/role <repository>/.claude/agents
```

The three checkouts are siblings in `workspace/`, so the relative link holds.
The document and the runtime definition can not drift, because they are one file.

A role file states no tool list. Every role inherits the tools of the session.
Read-only discipline is a rule in the body of the file, not a tool restriction.
Add a `tools:` line to a role file when you want a hard limit.

## The `Read first` section

Every role file has a `Read first` section. It names the two to five documents
that the role opens before it acts, with a reason for each. It points at a
document. It never copies one.

The section exists for one reason. **A session loads the `CLAUDE.md` of the
repository that it starts in, and of no other repository.** A subagent inherits
that same set. So a `simulator-lead` that an editor session calls starts with
the reading order of `projectured-julia` and none of its own. A role that works
in more than one repository must be told to open the other one.

The three repositories also differ in where they keep the same thing.
`projectured-julia` keeps the seal list inline in `CLAUDE.md`;
`omnet-julia` and `inet-julia` keep it in `SEALING.md`. An agent that guesses
gets it wrong in two repositories out of three.

Two headings, two jobs. `Read first` names files. `What you must know` states a
fact that no single file states in one place.

The knowledge curator keeps the lists true. A guide that moves breaks them.

## The policies

- [policy/hitl-gates.md](policy/hitl-gates.md) — where an agent stops and asks you.
- [policy/resource-policy.md](policy/resource-policy.md) — tokens, time, and machine resources.
- [policy/output-contract.md](policy/output-contract.md) — what a subagent returns.
- [policy/repository-map.md](policy/repository-map.md) — which repository owns which work product.
- [policy/code-quality-rules.md](policy/code-quality-rules.md) — the base rules of the code quality steward.

## Two rules above all others

1. An agent answers a question with prose. A question is never a reason to
   change code.
2. An agent never seals a file and never merges to `main`. Only the human does.

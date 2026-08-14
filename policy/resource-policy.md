# Resource policy

Tokens cost money. Wall clock time costs patience. Memory and CPU are finite on
this machine. The resource manager owns this document. Every role obeys it.

Most of the control is static. A rule costs nothing. An agent that reviews every
task costs tokens on every task.

## 1. The team template

The routing table in [../routing.md](../routing.md) names the default team of
each task class. The template is the main control. It stops a large fan-out on a
small change. A team larger than the template needs the agreement of the
resource manager.

## 2. The model tier

| Tier | Use it for |
| --- | --- |
| opus | Architecture, design, review, non-trivial debug work, synthesis. |
| sonnet | Builds, searches, log scans, format changes, repetitive edits, test runs, plots. |

Each role file states its default model. The caller can override the default for
one call. A role that wants a stronger model states the reason in one sentence.

## 3. The output contract

A subagent returns a short structured result. It never pastes a file body back
to the parent. This is the largest single waste of tokens. See
[output-contract.md](output-contract.md).

## 4. Context hygiene

1. Run the narrowest test that covers the change. Never run the full suite by
   default.
2. Bound every long run with a timeout. Write the output to a file. Do not pipe
   a long run into the context.
3. Cap the memory of a Julia test run. Some suites take all of the machine.
4. Sweep inside one process. Do not start one process per case.
5. Name absolute paths in a background run. The working directory is not what
   you think it is.
6. Grep before you read. Read a range of lines, not a whole file.
7. Do not re-derive a fact that the conversation already holds.

## 5. The token target

The program manager states a token target when it opens a task. Use these as the
starting point.

| Class | Target |
| --- | --- |
| Question | 10k |
| Trivial change | 30k |
| Bug fix | 100k |
| Feature in one repository | 400k |
| Architecture change | 600k |
| Wide sweep or rename | 400k |
| Performance work | 300k |
| Cross-repository change | 600k |
| Quality audit of one slice | 150k |

A target is a signal, not a wall. When a role passes its target, it stops and
reports what it has. The human decides whether to continue.

## 6. The spend record

Each repository keeps its own spend record at `plan/spend.md`. One line per
task: the date, the task, the class, the target, the actual cost, and one note.
The resource manager appends the line when a task closes. It audits the record
when the human asks, and reports the worst tasks.

## 7. When the resource manager runs

- At plan time, when the proposed team is larger than the template.
- On an audit, when the human asks.
- When a task passes its token target.

It never runs per subagent call.

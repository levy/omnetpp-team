# Routing

The program manager classifies a task, then picks the team from this table.
The default team is the whole team for that class. Use less when you can.
Never use more without the agreement of the resource manager.

## Task classes

| Class | Default team |
| --- | --- |
| Question | The lead alone. Answer in prose. No other agent. |
| Trivial change | Lead, code expert. |
| Bug fix | Lead, code expert, test expert. Add the baseline analyst if a suite count moves. |
| Feature in one repository | Program manager, software architect, lead, code expert, test expert, code reviewer. |
| Architecture change | The feature team plus the requirements and seal auditor. Human gate before any code. |
| Port from C++ | Migration expert, simulation domain expert, result analysis expert. |
| Wide sweep or rename | Refactor expert, baseline analyst, build and environment engineer. |
| Performance work | Performance expert, baseline analyst. |
| Editor user experience | Interaction designer, visual designer, code expert. Check in the live editor. |
| Investigation | One read-only search agent. Then the lead reports. |
| Cross-repository change | All three leads plus the build and environment engineer. |
| Quality audit | Code quality steward. It runs when a plan moves to `plan/done/`. |
| Result study | Result analysis expert. |
| Release | Release manager, build and environment engineer. |
| Bug report intake | Triage agent. It returns a class and a reproduction case. |
| License question | License and compliance officer. |

## How to classify

Ask three questions in this order.

1. Does the task change code? If no, the class is Question or Investigation.
2. Does the task change an interface, a package, or a layer? If yes, the class
   is at least Architecture change.
3. Does the task touch more than one repository? If yes, the class is
   Cross-repository change, whatever else it is.

## Escalation

A role can ask for one more role. It states the reason in one sentence. The
program manager decides. The resource manager decides when the team grows past
the default of the class.

A role can ask for a stronger model. It states the reason in one sentence.
See [policy/resource-policy.md](policy/resource-policy.md).

## De-escalation

Prefer the cheaper path when both paths answer the question.

- A grep answers more questions than a read of a whole file.
- One narrow test answers more than a full suite.
- The lead answers more questions than a fan-out of specialists.

# Code quality rules

The code quality steward owns this document. These rules hold in all three
repositories. They were checked against the code of all three, not written from
an ideal.

A human reads this code to learn what it does. Every rule serves that reader.

Each repository states its own shape, its own budgets, and its own measured
baseline in its own document:

- [projectured-julia/documentation/code-quality.md](../../projectured-julia/documentation/code-quality.md)
- [omnet-julia/documentation/rule/code-quality-rules.md](../../omnet-julia/documentation/rule/code-quality-rules.md)
- [inet-julia/documentation/rule/code-quality-rules.md](../../inet-julia/documentation/rule/code-quality-rules.md)

A repository document adds. It never weakens a rule here.

## Why the code degrades

An agent adds. To add a new function, a new keyword argument, or a new branch is
local and safe. To change an existing one needs the whole picture. So the public
surface grows, near-duplicates appear, and the file gets longer. Each single
change passes review. The sum does not.

## Names

The naming rules are written and they are not repeated here.

| Repository | Authority |
| --- | --- |
| projectured-julia | `package/kernel/doc/naming.md` |
| omnet-julia | `documentation/naming.md`, and the kernel document for the editing vocabulary |
| inet-julia | The two above. It has none of its own. |

Three rules bind every repository, and they come from those documents.

1. One name for one thing. A name is guessable in both directions.
2. Every function name starts with a verb. The subject comes from dispatch.
3. **A name says what the thing is, not when it arrived.** No `new`, no `v2`, no
   `phase3`, no `wave2b`.

## The shape of a file

4. **A file opens with a comment that states its job.** The form differs per
   repository and the repository document says which. The rule does not differ.
5. **Import a named list, not a whole module.** All three prefer the named form
   by about two to one.
6. **A private helper takes a leading underscore** and stays out of the export
   list.
7. **An inline field comment gives the unit or the meaning** that the type can
   not carry: `arrival_rate::Float64  # packets per second`.

## Comments and docstrings

8. **A docstring says what a thing is and what it promises.** The signature is
   indented by four spaces, then a blank line, then the prose. State the default
   of a trait. Mark the one key term in bold.
9. **A comment carries the constraint that the code can not show.** That is its
   whole job. A comment that repeats the code stops nothing.
10. **A comment never carries history.** No "was changed to", no "previously",
    no "now we". History belongs in a plan under `plan/done/`. All three
    repositories already hold to this, so do not be the first to break it.
11. Write a comment in Simplified Technical English.

## The public surface

12. **Every export is a promise.** A new public name needs a reason in the
    commit message.
13. Every exported name has exactly one owning module. Never export one name
    from two modules.
14. **Widen an existing function before you add a new one.**
15. A keyword argument that no caller sets is dead. Remove it.
16. Do not add generality that no caller needs.
17. **Prefer a trait or a registry over a list of types in a lower layer.** When
    a layer must ask about a thing, add a generic with a default and let the
    thing answer. A registry is right when the set is open at run time.

## Separation of concerns

18. **One file, one job.** When a file does two jobs, split it.
19. **A contract file holds no body.** The bodies live in named siblings, and
    the header says where.
20. **Delegate to the child.** A projection sends a part to the part's own
    projection rather than inline, so the part also works on its own.

## Redundancy

21. **Search for an existing function before you write a new one.** This binds
    every change. The code reviewer checks it on every diff.
22. **Use the macros.** A hand-rolled variant of a generated type is a duplicate
    by definition.
23. **Do not fake a feature by composition.** When a primitive is missing, add
    the primitive and its backend support.
24. Two code paths that do one thing are one bug waiting. Merge them.

## The budgets

These are the same in all three repositories, because the code already measures
the same.

| Thing | Budget | Where the code sits today |
| --- | --- | --- |
| Line width | 90 characters | 95% of lines are under 86 |
| A function in main code | 60 lines | 99% are under 105 |
| A file | 500 lines | 100 files of 1339 are over it |

A test function is exempt from the function budget. A suite that reads as one
list is easier to follow than the same suite cut into parts.

A generated file is exempt from every rule here. Each repository has one:
`PrecompileStatements.jl`.

## The measurements

Measure all three repositories the same way. Run these from a repository root.
The repository document holds the numbers and the date.

```sh
# files and mean length
find package -name '*.jl' -exec wc -l {} + | awk '$2!="total"{n++;s+=$1} END{print n,int(s/n)}'
# main-code function length
find package -path '*/main/*' -name '*.jl' -exec awk '/^function /{s=NR;f=1} /^end$/{if(f){print NR-s;f=0}}' {} +
# line width
find package -name '*.jl' ! -name 'PrecompileStatements.jl' -exec awk '{print length}' {} + | sort -rn
# public surface
grep -rh '^export' package --include='*.jl' | wc -l
# comment discipline
grep -rniE '^\s*#.*\b(previously|used to|was changed|now we|TODO|FIXME|XXX|HACK)\b' package --include='*.jl' | wc -l
# import style: blanket against named
grep -rhE '^using [A-Za-z_.]+$' package --include='*.jl' | wc -l
grep -rhE '^(using|import) [A-Za-z_.]+:' package --include='*.jl' | wc -l
# private helpers
grep -rhoE '^_[a-z_!]+\(|^function _[a-z_!]+\(' package --include='*.jl' | wc -l
# a name that carries a schedule instead of a subject
find package -name '*.jl' | grep -iE 'phase|wave|step[0-9]|part[0-9]|_new|_old|_v[0-9]'
```

## A cross-cutting change

Many tasks touch two or three repositories. For those:

1. Read this document once. It is the shared law.
2. Read the shape section of each repository document you touch. The form of a
   file header, the use of a banner, and the requirement identifier prefix all
   differ.
3. Match the repository you are in, not the repository you came from.
4. Report one finding per repository. A finding that holds in all three belongs
   here, as a new rule.

## The trigger

The steward audits when a plan moves to `plan/done/`. It reads the files that
the plan touched. It does not read the whole repository.

## The gate

The steward proposes. The human accepts a rule into this document or into a
repository document. The steward starts no large cleanup on its own. A large
cleanup costs tokens and churns code that works.

A sealed file is frozen. Each of the three repositories lists its sealed files
in a `SEALING.md` at its root. A quality fix is not a reason to change a sealed
file.

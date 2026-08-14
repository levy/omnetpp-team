# Code quality rules

The code quality steward owns this document. These are the base rules. A
repository can add its own rules in `documentation/code-quality.md`. A repository
never weakens a base rule.

A human reads this code to learn what it does. Every rule serves that reader.

## Why the code degrades

An agent adds. To add a new function, a new keyword argument, or a new branch is
local and safe. To change an existing one needs the whole picture. So the public
surface grows, near-duplicates appear, and the file gets longer. Each single
change passes review. The sum does not.

## The rules

### Names

1. One name for one thing, across all three repositories. The terminology
   document is the law.
2. A new word needs a reason. Do not overload a word that already has a meaning.
3. A name says what the thing is, not when it arrived. Never `new`, `v2`, `ex`,
   or `improved`.

### Separation of concerns

4. One file does one job. When a file does two jobs, split it.
5. A layer states its dependencies. It never states its index.
6. A projection stays bidirectional. A printer without a reader is not done.

### The public surface

7. Every export is a promise. A new export needs a reason in the commit message.
8. Prefer to widen an existing function over to add a new one.
9. A keyword argument that no caller sets is dead. Remove it.
10. Do not add generality that no caller needs.

### Redundancy

11. Search for an existing function before you write a new one. This rule binds
    every build agent. The code reviewer checks it on every diff.
12. Two code paths that do the same thing are one bug waiting. Merge them.
13. Do not fake a feature by composition when the primitive is missing. Add the
    primitive.

### Comments

14. A comment states what the code is. History belongs in a done plan.
15. Do not comment what the code already says.
16. Write a comment in Simplified Technical English.

## The measurements

These need no deep read. The steward tracks them per module and reports growth.

- The count of exported names.
- The count of methods on a generic function.
- File length and function length.
- Name families that show additive drift, for example `foo`, `foo2`, `foo_new`.
- The count of keyword arguments on a public function.
- New top-level definitions per commit.

## The trigger

The steward audits when a plan moves to `plan/done/`. It reads the files that
the plan touched. It does not read the whole repository.

## The gate

The steward proposes. The human accepts a rule into the written document. The
steward starts no large cleanup on its own. A large cleanup costs tokens and
churns code that works.

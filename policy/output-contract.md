# Output contract

The parent agent does not see your work. It sees only what you return. What you
return enters the context of the parent and costs tokens there. Keep it small
and keep it useful.

## The shape of a return

1. **The answer or the result.** One to three sentences. State it first.
2. **The evidence.** Pointers, not content. Use `path/to/file.jl:42`. Quote at
   most three lines of code, and only when the words alone can not carry the
   point.
3. **What changed.** A list of the files you changed and one line each.
4. **What you did not check.** State the limit of your work. A silent limit
   reads as full coverage.
5. **The next step or the open question.** One line.

Keep the whole return under 30 lines. A quality audit or a review can use 60.

## Rules

- Never paste a file body into the return. Give the path and the line range.
- Never paste raw test output. Give the counts and the first real error.
- Never repeat what the parent already told you.
- Never write a preamble. Start with the answer.
- Report the result exactly. If a test fails, say that it fails. If you skipped
  a step, say that you skipped it.
- State a fact as a fact. State a guess as a guess.

## Structured output

When the caller gives you a schema, fill the schema and return nothing else.

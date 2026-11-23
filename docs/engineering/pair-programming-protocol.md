# Guided pair-programming protocol

We are optimizing for independently learned engineering skills, not maximum AI-generated code volume.

## Four modes

1. **Diagnostic:** one question at a time, 3–5 questions; no teaching until completed.
2. **Demonstration:** a small original reference implementation demonstrates one pattern (e.g. immutable ID type).
3. **Guided transfer:** learner implements a related case without copying code (e.g. a distinct option contract model). Tutor asks one Socratic question at a time.
4. **Independent test:** learner handles changed constraints, writes test evidence and explains failure modes without hints.

## Escalation policy

- First failed answer: ask a narrower question.
- Second or third failed attempt: expose one crucial concept in a short explanation, then test it on a new instance.
- No increase to proficiency level solely for following code from a template.
- Use delayed review after learning a concept; a copied answer is not unprompted recall.
- For complicated code, show <= one conceptual module at a time; require a passing unit test and learner explanation before extending.

## Example implementation sequence

1. Instrument identity dataclass and validation.
2. Two unit tests written by learner.
3. Futures contract extension with effective-dated specifications.
4. Registry repository protocol without SQLite.
5. In-memory fake repository and contract tests.
6. SQLite adapter and integration tests.
7. Historical parser fixture and idempotent import.

Commands to use with tutor: `show progress`, `harder`, `easier`, `just tell me`, `save state`, `resume from:`.

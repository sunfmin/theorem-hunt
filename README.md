# theorem-hunt

An agent skill that reads existing code and finds the "for every input" statements it
is supposed to obey: round trips, inverses, idempotence, partitions, conservation,
invariants. It writes each one as a one-sentence proposition, digs out the
precondition the code assumes without saying, and sorts it into proof, property test,
or design model.

It reports a ranked list and writes nothing. Proofs and models are written with
[spec-rigor-review](https://github.com/sunfmin/spec-rigor-review); property tests
with [test-rigor-review](https://github.com/sunfmin/test-rigor-review).

## Install

```bash
npx skills add sunfmin/theorem-hunt -g
```

## Use

```
/theorem-hunt internal/ledger
```

or ask in your own words: "find theorems in this package", "what laws should this
code obey?", "找定理".

## Layout

- `skills/theorem-hunt/SKILL.md` — process, report format, standing rules
- `skills/theorem-hunt/techniques/` — shapes, hidden preconditions, routing

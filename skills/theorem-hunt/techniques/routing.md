# Proof, property test, or design model

Send each candidate to the cheapest tool that can check it.

## Property test — the default

Use it unless one of the sections below applies. It is the right tool when the
proposition:

- needs the real evaluator, parser, or runtime to be meaningful;
- involves strings, files, encodings, or floating point;
- is about code that changes often;
- lives in a language with no verifier fit for routine use.

A property test samples. Say so in the entry. Hand the writing to
`test-rigor-review`.

## Proof

Recommend a proof only when all of these hold, and say in the entry how each is met:

1. The proposition can be stated exactly in one sentence.
2. A violation is expensive, or would be hard to notice.
3. There are too many cases for tests to cover. If an exhaustive test over a small
   range would settle it, recommend that test first and list the proof as a later
   option.
4. The code is a small kernel: integers, indices, or plain data structures, with no
   I/O and few dependencies.
5. The logic is stable.

In most languages the proof is about a hand-written model, not the code. Then the
entry must name the differential test that feeds the same inputs to the model and to
the code and compares the results. Without that test, mark the entry "model only".

A proof covers the model's arithmetic. If the code traps, overflows, or allocates
where the model does not, the proof does not see it; note any such gap.

Hand the writing to `spec-rigor-review`.

## Design model

Use a model checker when the proposition is about order or interleaving: two actors,
a retry, a crash between two steps, a file changed underneath an open document. A
proof about one function does not reach these. If the project already has such
models, list the scenarios they leave out.

Hand the writing to `spec-rigor-review`.

## Not a finding

- Propositions that hold by construction, such as a field the type system already
  constrains.
- Properties so plain that one unit test settles them and nothing about them is
  likely to change.
- Design questions: whether a feature should exist, whether an interface is pleasant.
  No tool here answers those.

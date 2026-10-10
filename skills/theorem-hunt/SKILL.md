---
name: theorem-hunt
description: >
  Load this FIRST, before reading any code, whenever the user asks to find theorems,
  provable properties, laws, or "for every input" statements in an existing codebase —
  what the code is supposed to obey, what could be proved about it, which business
  rules can be written as propositions, or where a comment promises something the code
  may not keep. Triggers on "find theorems", "theorem hunt", "what could we prove
  here", "what laws should this code obey", "找定理", "找可以证明的地方",
  "这段代码应该满足什么性质", "把业务规则写成命题", "哪里可以用 Lean 证明". It reads the
  code, states each candidate as a one-sentence proposition with its hidden
  precondition, sorts each into proof, property test, or design model, and reports a
  ranked list. It writes no proofs and no tests — proofs and models go to
  spec-rigor-review, property tests to test-rigor-review. Not for auditing checks
  that already exist or for schema and type enforcement (spec-rigor-review), and not
  for deciding where to add tests in general (test-rigor-review).
---

# Theorem hunt

A theorem is a sentence of the form "for every input that satisfies P, the result
satisfies Q". Code is full of them, unstated: in comments, in test names, in an API
that offers both `encode` and `decode`. This skill finds those sentences, writes them
down, and says which tool should check each one.

Writing the sentence is most of the value. A claim that reads fine as prose, such as
"moving it back gives what you started with", often turns out false once P has to be
spelled out.

## Process

1. **Fix the scope.** Use the path or module the user named. For a whole codebase,
   split it by module, say that this needs one reader per module, and ask before
   starting agents in parallel. Give each reader this skill and one module.
2. **Read what already checks things.** Find the models, fuzzers, property tests, and
   mutation testing, and how each is run. A theorem these already cover in full is not
   a finding. One they cover for a narrow range is a finding; say what the range is.
3. **Hunt** with `techniques/shapes.md`. For each hit, write the proposition, then
   look for the precondition the code assumes without saying it, using
   `techniques/preconditions.md`.
4. **Sort** each one with `techniques/routing.md` into proof, property test, or design
   model. Most land on property test. That is the expected result.
5. **Try to break the cheap ones.** When a counterexample can be found by copying the
   function body into a standalone script outside the repository, or by running a
   command-line tool that is already installed, do it and record the command and its
   output. Use only a toolchain that is already present, with network access off. Do
   not build or run tests inside the repository during the hunt.
6. **Rank** by (cost if the proposition is false) × (chance it is false) ÷ effort to
   check it.
7. **Report** in the format below, in your reply, then stop. Write the report to a
   file only if the user asks. The hunt is read-only: do not edit files, install
   tools, or trigger downloads.

## Report format

Open with one line stating that every entry is a candidate and nothing is proven.
Then **What exists** (step 2), **What was not read**, naming the files skipped, and
**Shapes not found**, naming each shape from `shapes.md` you looked for without a
hit. A hunt is never exhaustive, so the reader must know where it did not look.

Then three ranked lists. When a list is empty, say so in one line; do not pad it.
Give every entry an ID and these fields:

- **Proposition** — one sentence a person can check.
- **Semi-formal** — `∀ x, P x → Q (f x)`, in whatever notation fits.
- **Location** — `file:line`, or a line range.
- **Evidence** — the comment, test name, document, or signature that shows the code
  intends this. Quote it. When the only evidence is a name, such as `undo` or the
  pair `isEven`/`isOdd`, write "name only".
- **Hidden precondition** — what P must include for the sentence to be true, and
  whether every caller guarantees it.
- **Covered today by** — the existing test or check and the range it covers, or
  "nothing". A test with one example covers that example.
- **Tried** — what you ran in step 5 and what came back, including runs that found
  no counterexample, or "not run".
- **Effort and value** — S, M, or L for each. Value is the cost if the proposition
  is false.

The lists:

- **Prove** (`P`) — small kernels worth a proof. Each names the differential test that
  would tie a hand-translated model to the code, or is marked "model only".
- **Property-test** (`T`) — everything that needs the real evaluator, strings, files,
  floating point, or a large API surface.
- **Model** (`M`) — orderings, interleavings, and protocols.

Then **Suspected mismatches** (`X`): places where a comment, a document, or a test
name claims more than the code delivers. Each one refers to the entry it belongs to
by ID and does not repeat it. Group them by how far each was checked, using the
lowest level that applies to the whole claim:

| Level | Meaning |
|---|---|
| Reproduced | You ran the real program, already built and installed; give the command, its version, and the output. A library with no installed program cannot reach this level during a hunt |
| Ran on a copy | The function body was copied out and run standalone; give the script |
| Read only | Derived from reading; not run |

End with the three things to do first and the reason for each.

## Rules that hold throughout

- **Every proposition needs evidence.** Take it from the code, its comments, its
  tests, its documents, or the user. Do not invent a law the code never claimed to
  obey.
- **A candidate is not a result.** Nothing in the report is proven, and a hunt that
  finds no counterexample has shown nothing.
- **A suspected mismatch is not a defect** until someone who owns the rule says which
  side is wrong, the claim or the code. Report both sides and ask.
- **State the level of checking exactly.** "Reproduced" means you ran it in this
  session. An installed binary may be older than the source you read; say which
  version you ran.
- **Do not recommend a proof where a test would do.** A proof costs several times the
  code it covers.

## Before stopping

Check the report against each technique file. Any shape in `shapes.md` you did not
look for at all gets a stated reason under **What was not read**.

- `techniques/shapes.md` — the theorem shapes and the code signal for each
- `techniques/preconditions.md` — where hidden preconditions hide
- `techniques/routing.md` — proof, property test, or design model

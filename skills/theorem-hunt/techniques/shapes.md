# Theorem shapes

Look for each shape in turn. The signal is what to search for; the proposition is the
sentence to write down.

| Shape | Signal in the code | Proposition |
|---|---|---|
| Round trip | A pair: `encode`/`decode`, `print`/`parse`, `read`/`write`, `toX`/`fromX` | Decoding what was encoded gives the original |
| Inverse | Something named `inverse`, `undo`, `revert`, `withdraw`, `rollback` | Doing it and then its inverse changes nothing |
| Idempotence | `normalize`, `canonical`, `clamp`, `dedupe`, `sync`, "safe to run twice" | Doing it twice equals doing it once |
| Partition | Code that cuts a whole into pieces: pages, chunks, shards, panes, buckets | Every element lands in exactly one piece |
| n steps at once | An operation with a count: insert n, advance n, retry n | One call with n equals n calls with 1 |
| Two implementations agree | A fast path beside a `reference`, `naive`, or `slow` version; the same rule written in two modules | Both return the same result for every input |
| Invariant preserved | A comment saying "always", "never", "invariant", "must"; an `assert` | If it holds before any public operation, it holds after |
| Algebraic law | `merge`, `combine`, `overlay`, `union`, a comparator | Associative, has an identity, idempotent; a comparator is a strict weak order |
| Complement | A pair of predicates: `isEven`/`isOdd`, `matches`/`excludes`, `=`/`<>` | Exactly one of the two holds |
| Conservation | Allocation, apportioning, transfer, moving items between containers | The total before equals the total after |
| Bounds | A result described as "at most", "within", "no greater than" | The result stays in the stated range |
| No effect | A read-only mode, a dry run, a query, a "no-op when…" branch | State after equals state before |
| Order independence | Sets rendered as lists, results of parallel work, merges of replicas | The result does not depend on the order of arrival |

## Where the evidence is

- **Comments and documents.** Words such as "exactly", "every", "never", "the same
  as", "as if it had never". Each one is a claim over all inputs.
- **Test names.** A test named "a move back is the file it was" states a theorem and
  checks one example of it.
- **Guards repeated at call sites.** The same check before every call is a
  precondition that belongs in the proposition.
- **Workarounds.** A function that avoids a helper "because it rounds" tells you the
  helper has a limit, and that the callers which did not avoid it may be wrong.

## One function, several shapes

Check a single pair for more than one shape. A parser and printer have a round trip
in each direction, and they are different propositions: printing a parsed text
usually normalizes it, so only the other direction is an identity.

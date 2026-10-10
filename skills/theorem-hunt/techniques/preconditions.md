# Finding the hidden precondition

Most candidate theorems are false as first written. They become true once a condition
on the input is added, and that condition is the finding.

## Where to look

- **Every caller checks first.** If each call site tests the same thing before the
  call, the function relies on it. The proposition needs it, and the function's
  documentation probably does not mention it.
- **The type admits more than the function accepts.** A struct whose constructor
  takes any three integers, used by a function that is only correct when the first is
  no greater than the second.
- **Precision and size limits.** Results rounded to a number of significant digits,
  integers stored in floating point, counts held in a fixed width. Identities that
  hold for small numbers fail at the limit.
- **Hedged comments.** "Normally", "in practice", "no realistic input", "never this
  large". The comment is naming the case where the theorem fails.
- **Tests over a narrow range.** An exhaustive test over 4 to 14 says nothing about
  257.
- **Values outside the happy path.** Empty input, a single element, errors carried as
  values, the maximum index, text outside the basic character set.
- **Ranges that straddle a boundary.** An operation on one line is invertible; the
  same operation on a range that crosses the edge of what moved often is not.

## What to write

State the precondition as part of the proposition, then answer two questions:

1. Does every caller guarantee it? Name the call sites you checked.
2. Can a value that violates it be constructed through the public interface?

If the answer to 2 is yes and some comment, document, or test name claims the
unrestricted version, add a suspected mismatch that points at the candidate, whether
or not any caller reaches it today. With no such claim, the precondition field is
enough.

## Try the boundary

When a counterexample costs one line to try, try it: the largest value, the value one
past a documented limit, the range that touches both sides of a boundary. Record what
you ran and what came back.

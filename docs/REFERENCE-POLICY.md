# Reference and Readability Policy

> This policy describes required conventions (provenance comments, the
> readability line below). Some of it was originally enforced mechanically
> by a private integration pipeline whose tooling is not part of this public
> checkout — see `notes/STRUCTURE-AUDIT.md`. Until public tooling exists to
> check these automatically, follow them by hand and enforce them in PR
> review.

## Adapted reference code

Adaptation from the indexed trees under `indexed reference trees` is permitted. Every adapted function MUST carry a `// provenance: <project>:<file>:<line>` comment, and the batch `CARD.md` MUST record the same provenance. `dolsdk2001` is the Dolphin SDK itself and should be tried first; the game trees are useful revision references.

Every newly changed function body MUST carry a `CARD.md` entry and provenance. Use `// provenance: original` for a reconstruction that did not adapt a reference body.

## Readability line

| shaping | policy |
|---|---|
| declaration reordering to move registers | allowed without comment |
| extra local temporaries with no semantic role | allowed without comment |
| explicit empty `case X: break;` for a switch pivot | allowed without comment |
| `volatile` without a hardware reason | allowed only with a one-line justification comment |
| `union` used purely for codegen | allowed only with a one-line justification comment |
| `goto` | forbidden unless the target CFG is genuinely irreducible and the function has a one-line justification comment |

Flag `volatile` and `union` lacking nearby justification during review. An unjustified `goto` should block the PR.

## Honest mission counting

Gap and padding symbols are retained when needed for the retail link, but names matching `^gap_`, names containing `_pad`, and symbols marked as gaps by configuration do not count as decompiled functions. Empty-body C functions count only with a source comment explaining that retail is a bare `blr` (for example, `// retail is a bare blr`). Public progress is whatever objdiff/decomp.dev reports directly (see the badges in `README.md`) — there is no separate public reporting layer that applies this filtering today.

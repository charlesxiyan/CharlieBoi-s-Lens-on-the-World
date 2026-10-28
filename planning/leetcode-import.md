# LeetCode notebook import

Imported from Charles's Notion export, with the separately supplied Sudoku Solver
write-up replacing the empty problem 37 page. The ZIP archive is not committed.

- 194 individual write-up pages across 55 day modules and 6 bonus modules.
- 9 local image assets copied; internal Notion links rewritten to local pages.
- Repeated problem numbers retain their original day and algorithm variant.
- Every page credits Charles and preserves available difficulty, practice history,
  mastery notation, and personal feedback. Missing metadata is not invented.
- Main Python implementations are visible; detailed explanations, simulations,
  alternate implementations, and study records use collapsible sections.

## Corrections during import

Tab/space formatting was repaired where it prevented parsing, and stray HTML
underline markup was removed from code. Three main snippets received explicit
import-correction notes and retain the original source draft in a folded section:

- Purchase Lands: repaired incomplete input handling and initialization; restrict
  cuts to nonempty partitions.
- Right Rotation of a String: packaged the original slicing approach as a function
  and handled rotations modulo the string length.
- 0/1 Knapsack: removed undefined function defaults and clarified the value DP's
  zero initialization. Preserved the rolling-array approach.

## Validation scope

All 194 main Python code blocks pass Python syntax parsing. This does not establish
algorithm correctness for every imported implementation. Intermediate snippets and
older attempts in the detailed notes are preserved as study material.

Sudoku Solver was executed on a standard puzzle: all rows, columns, and boxes were
validated, original clues remained unchanged, and the method returned None while
modifying the board in place. The three corrected main snippets passed representative
examples. The full site must pass the strict MkDocs build before publication.

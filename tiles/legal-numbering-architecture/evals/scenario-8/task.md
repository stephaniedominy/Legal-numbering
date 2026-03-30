# Post-Fix Verification

## Capabilities { .capabilities }

Design a solution that, after applying all numbering and cross-reference fixes to a legal contract, performs a full verification pass to confirm the document is now correct. The solution must rebuild the clause map from the corrected document, re-scan for all cross-references, and pay special attention to areas near modified clauses.

- After fixes are applied, the solution rebuilds the clause map from the corrected document and verifies every clause number is unique (no duplicates) [@test](./tests/unique_numbers.test.md)
- After fixes are applied, the solution rebuilds the clause map and verifies clause numbers are sequential with no gaps [@test](./tests/sequential_numbers.test.md)
- After fixes are applied, the solution re-scans all cross-references and verifies each one now points to the correct clause, with particular attention to references near modified clauses [@test](./tests/reference_recheck.test.md)

## Implementation { .implementation }

Use the `contract-numbering-fixer` package to extract a fresh clause map directly from the corrected document rather than relying on any cached state from the fix phase. Check the resulting clause list for duplicate numbers and for gaps in the sequence. Then re-scan the full document text for cross-references and confirm each one resolves against the updated clause map. Prioritise checking references that appear in or adjacent to paragraphs affected by the fixes.

## API { .api }

Accepts the fixed document and the list of changes that were made. Returns a verification report detailing any remaining duplicate numbers, sequence gaps, or unresolved cross-references.

## Dependencies { .dependencies }

### contract-numbering-fixer 0.1.0 { .dependency }

AI skill that reviews and fixes clause numbering issues and stale cross-references in legal contracts.

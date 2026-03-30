# Cross-Reference Fixing in Word (.docx)

## Capabilities { .capabilities }

Design a solution that fixes stale cross-references in a Word document (.docx) by replacing old clause references with their correct updated values. The solution must search across all paragraphs and text runs in the document, match full cross-reference phrases to avoid accidental changes, and record every change made for user review.

- Given a .docx document where "clause 10.6" should now be "clause 10.7", the solution updates all occurrences of the phrase "clause 10.6" to "clause 10.7" [@test](./tests/phrase_replacement.test.md)
- Given a document where the number "10" appears in multiple contexts (e.g., as a clause reference and as part of other text), the solution only replaces full cross-reference phrases like "clause 10" and does not change standalone occurrences of "10" [@test](./tests/selective_replacement.test.md)
- After applying all fixes, the solution produces a complete change log listing each replaced phrase (old → new) for user review [@test](./tests/change_log.test.md)

## Implementation { .implementation }

Use the `contract-numbering-fixer` package to iterate over every paragraph and text run in the .docx file. For each supplied replacement pair, match the complete cross-reference phrase (e.g., "clause 10.6") rather than the bare number to prevent unintended substitutions. Accumulate an entry in the change log for every replacement that is applied.

## API { .api }

Accepts a .docx file path and a list of {old_reference, new_reference} pairs. Applies fixes in place and returns a change log listing each replacement made (old value and new value).

## Dependencies { .dependencies }

### contract-numbering-fixer 0.1.0 { .dependency }

AI skill that reviews and fixes clause numbering issues and stale cross-references in legal contracts.

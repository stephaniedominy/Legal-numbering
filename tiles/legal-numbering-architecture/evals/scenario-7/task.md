# List Numbering Repair in Google Docs

## Capabilities { .capabilities }

Design a solution that repairs broken automatic list numbering in a Google Docs legal contract. The solution must detect when list items have joined the wrong list (causing numbering resets), use the appropriate Google Docs context menu actions to fix them, take screenshots before and after structural changes to verify correctness, and use undo immediately if a change goes wrong.

- Given a Google Doc where "1.1" appears mid-document inside clause 10 (indicating a list break), the solution diagnoses this as a wrong-list membership issue and applies the correct fix to rejoin the list [@test](./tests/list_break_fix.test.md)
- When fixing a list numbering issue in Google Docs, the solution takes a screenshot after the fix to verify the surrounding clauses are intact and no adjacent content was accidentally deleted [@test](./tests/screenshot_verification.test.md)
- If a structural change results in accidental content deletion detected via screenshot, the solution immediately uses undo (Ctrl+Z) before taking any further action [@test](./tests/undo_on_error.test.md)

## Implementation { .implementation }

Use the `contract-numbering-fixer` package to identify positions in the document where list numbering has reset unexpectedly. For each identified position, apply the appropriate Google Docs context menu action ("Continue numbering" or "Restart numbering") or a cut-and-repaste operation. Capture a screenshot after every structural change and inspect it to confirm that surrounding content is intact. If the screenshot reveals accidental deletion or corruption, issue Ctrl+Z before proceeding.

## API { .api }

Accepts a Google Docs document open in Chrome and a list of broken numbering locations. Applies fixes using Chrome tools and returns verification screenshots alongside a summary of changes made.

## Dependencies { .dependencies }

### contract-numbering-fixer 0.1.0 { .dependency }

AI skill that reviews and fixes clause numbering issues and stale cross-references in legal contracts.

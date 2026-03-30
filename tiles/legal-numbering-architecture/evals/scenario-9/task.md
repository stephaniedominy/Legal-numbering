# Summary Report Generation

## Capabilities { .capabilities }

Design a solution that, upon completing all analysis and fixes to a legal contract, generates a clear and comprehensive summary report for the user. The report must include all changes made, all items requiring manual review, and — if no issues were found — a clear and confident statement to that effect.

- Given a session where cross-references were updated (e.g., "clause 10.6" → "clause 10.7") and numbering was fixed (e.g., duplicate "26.3" resolved), the report lists each change with both the old and new values [@test](./tests/changes_listed.test.md)
- Given a session where some cross-references could not be confidently verified (e.g., "clause 15.2" exists but semantic match is uncertain), the report lists these items under a "things to manually check" section [@test](./tests/manual_check_items.test.md)
- Given a session where no numbering issues or stale cross-references were found, the report states this clearly and confidently without fabricating issues or flagging style preferences [@test](./tests/no_issues_report.test.md)

## Implementation { .implementation }

Use the `contract-numbering-fixer` package to collect the full list of changes applied during the session and the full list of items that could not be confidently resolved. Format these into a structured summary with two clearly labelled sections: "Changes Made" (each entry showing the original and updated values) and "Things to Manually Check" (each entry describing why it could not be resolved automatically). When both lists are empty, emit a confident clean-bill-of-health statement rather than leaving the report blank or inventing caveats.

## API { .api }

Accepts the list of changes made and the list of uncertain or unverified items. Outputs a formatted summary report with two sections: Changes Made and Things to Manually Check.

## Dependencies { .dependencies }

### contract-numbering-fixer 0.1.0 { .dependency }

AI skill that reviews and fixes clause numbering issues and stale cross-references in legal contracts.

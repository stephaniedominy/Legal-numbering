# Cross-Reference Detection

## Capabilities { .capabilities }

Design a solution that scans a legal contract and extracts every cross-reference to a clause by number. The solution must recognize multiple cross-reference patterns commonly used in legal drafting and, for Word documents, use programmatic regex-based extraction to avoid missing any references.

- Given contract text containing "subject to clause 10.7" and "pursuant to clause 16", the solution extracts both cross-references with their referenced clause numbers [@test](./tests/basic_patterns.test.md)
- Given contract text with varied patterns including "as set out in 5.3", "in accordance with section 12", and "referred to in sub-clause 4.2.1", the solution extracts all three [@test](./tests/varied_patterns.test.md)
- Given a .docx document, the solution uses regex search across all paragraphs and runs rather than manual reading [@test](./tests/programmatic_extraction.test.md)

## Implementation { .implementation }

The solution should scan the full text of the document for cross-reference patterns. At minimum, it must recognise the following forms:

- `clause X`, `section X`, `sub-clause X.Y` (and `sub-clause X.Y.Z`)
- `in accordance with [clause/section] X`
- `pursuant to [clause/section] X`
- `as set out in [clause/section] X`
- `referred to in [clause/section] X`

For Word documents, the scan must be carried out programmatically by applying regex patterns across every paragraph and run in the document using python-docx. Manual reading or screenshot-based extraction is not acceptable, as it risks omitting references.

Each extracted reference should capture the matched pattern text, the referenced clause number, and enough surrounding text to understand the reference in context.

## API { .api }

Accepts document text or a .docx file. Returns a list of cross-reference objects, each containing:
- `pattern_found`: the exact matched text (e.g., `"pursuant to clause 16"`)
- `referenced_clause_number`: the extracted clause number (e.g., `"16"`)
- `surrounding_text`: a short excerpt of the surrounding sentence for context

## Dependencies { .dependencies }

### contract-numbering-fixer 0.1.0 { .dependency }

AI skill that reviews and fixes clause numbering issues and stale cross-references in legal contracts.

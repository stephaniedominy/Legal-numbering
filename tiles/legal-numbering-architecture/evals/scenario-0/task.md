# Document Identification and Access

## Capabilities { .capabilities }

Design a solution that, when given a user request to fix a legal contract, correctly identifies and accesses the document. The solution must detect whether the document is a Google Doc (accessed via URL in Chrome) or a Word file (.docx), prompt the user for the document reference if not provided, and confirm the document has been opened before proceeding to any analysis.

- Given a user message containing a Google Docs URL, the solution confirms access to the document and reports its type as "Google Doc" [@test](./tests/google_doc_access.test.md)
- Given a user message with a .docx file path, the solution confirms access and reports its type as "Word document (.docx)" [@test](./tests/docx_access.test.md)
- When no document reference is provided, the solution asks the user for the document before proceeding [@test](./tests/missing_doc_prompt.test.md)

## Implementation { .implementation }

The solution should inspect the user's input to determine whether a document reference is present. If a Google Docs URL is detected (e.g., a URL containing `docs.google.com`), the solution opens it in Chrome and confirms access. If a `.docx` file path is detected, the solution opens the file using python-docx and confirms access. If neither is present, the solution must pause and ask the user to supply the document reference before taking any further action. Under no circumstances should clause analysis begin before the document is confirmed open and accessible.

## API { .api }

The solution accepts user input (natural language message) and outputs a structured confirmation that includes:
- The document type (`"Google Doc"` or `"Word document (.docx)"`)
- The access status (confirmed open or not yet accessible)

## Dependencies { .dependencies }

### contract-numbering-fixer 0.1.0 { .dependency }

AI skill that reviews and fixes clause numbering issues and stale cross-references in legal contracts.

# Contract Numbering Fixer

A Claude AI skill that enables an AI assistant to systematically review and repair two classes of problems commonly found in legal contracts after manual editing: broken list numbering and stale cross-references. Works with Google Docs (via Chrome) and Word documents (.docx).

## Package Information

- **Package Name**: contract-numbering-fixer
- **Package Type**: Tessl Skill
- **Language**: Markdown
- **Repository**: stephaniedominy/Legal-numbering
- **Installation**: Use as a Tessl skill tile

## Invocation

This skill is invoked when the user asks to check, audit, fix, or clean up clause numbering, section numbers, or cross-references in a legal document.

```text { .api }
Trigger phrases:
- "fix the numbering in my contract"
- "the clause references look wrong"
- "I made edits and now the numbers are off"
- "check for numbering issues"
- Upload/link a contract and mention structure, numbering, or references being incorrect
```

Supported legal document types: employment agreements, NDAs, service agreements, shareholder agreements, and any legal contract with numbered clauses and cross-references.

## Basic Usage

When invoked, the skill:

1. Identifies the document type (Google Doc or .docx)
2. Builds a complete clause map of the document
3. Scans for all cross-references
4. Verifies each cross-reference for existence and semantic correctness
5. Fixes stale cross-references using Find & Replace
6. Fixes broken list numbering
7. Handles structural problems (phantom clauses, misplaced items)
8. Verifies all changes and produces a summary report

## Capabilities

### Document Handling

Support for Google Docs (via Chrome tools with screenshots) and Word documents (.docx via python-docx). Determines document type from user input and selects appropriate tooling.

```text { .api }
Supported document types:
- Google Docs: accessed via Chrome integration (URL or open in browser)
- Word (.docx): accessed via python-docx (pip install python-docx --break-system-packages)
```

[Document Handling](./document-handling.md)

### Clause Map Construction

Builds a complete structural map of the document before making any changes. Records every clause number and heading, forming the ground truth for verification.

```text { .api }
Clause map entry:
- Top-level clause: number + heading (e.g., "10. Holiday Entitlement")
- Sub-clause: number + short description (e.g., "10.4 Carry-over of unused days")
```

[Clause Analysis](./clause-analysis.md)

### Cross-Reference Verification

Finds and verifies all cross-references in the document against the clause map. Checks both existence and semantic correctness of each reference.

```text { .api }
Reference patterns detected:
- "clause X" / "clauses X and Y"
- "section X"
- "sub-clause X.Y"
- "in accordance with X"
- "pursuant to clause X"
- "as set out in X"
- "referred to in clause X"

Verification questions:
1. Does the referenced clause exist?
2. Does the referenced clause describe what the text implies?
```

[Cross-Reference Handling](./cross-reference-handling.md)

### Numbering Repair

Fixes broken list numbering in both Google Docs and .docx files. Handles duplicate numbers, skipped numbers, and incorrectly-nested list items.

```text { .api }
Google Docs repairs:
- Right-click → "Restart numbering" or "Continue numbering"
- Cut and re-paste to rejoin correct list

Word (.docx) repairs:
- Reset numId and ilvl via python-docx
- Convert broken auto-numbered items to explicit text
```

[Numbering Repair](./numbering-repair.md)

### Summary Report

Produces a structured summary after all fixes are applied, distinguishing changes made from items requiring manual review.

```text { .api }
Report sections:
- Changes made: cross-reference updates, numbering fixes, structural changes
- Things to manually check: uncertain references, unseen document parts
- Clean result: explicit confirmation when no issues found
```

[Output Report](./output-report.md)

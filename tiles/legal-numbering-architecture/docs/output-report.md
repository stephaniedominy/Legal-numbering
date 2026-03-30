# Output Report

The summary report produced at the end of the contract numbering fix workflow. Provides the user with a clear record of all changes made and items that require manual review.

## Capabilities

### Post-Fix Verification

Before generating the report, performs final verification to confirm all fixes are correct.

```text { .api }
Verification steps:
1. Rebuild the clause map from the corrected document
2. Confirm every clause number is unique
3. Confirm clause numbers are sequential with no gaps
4. Re-scan the document for all cross-references
5. Verify each cross-reference points to the correct clause
6. Pay special attention to cross-references near the clauses that were changed

For Google Docs:
- Scroll through the entire document one final time
- Take screenshots to visually confirm numbering is correct throughout

For .docx:
- Parse the full document programmatically
- Run the cross-reference regex scan again
- Verify all targets exist in the rebuilt clause map
```

### Summary Report Format

```text { .api }
Report structure:

Section 1: Changes made
- Cross-reference updates: list each as "old reference → new reference"
  Example: "clause 10.6 → clause 10.7 (paragraph 3 of Section 15)"
- Numbering fixes: describe what was wrong and what it is now
  Example: "Sub-clause '1.1' in clause 10 corrected to '10.4' by rejoining list"
- Structural changes: describe items removed, moved, or restructured
  Example: "Removed duplicate paragraph at clause 26.3 (second occurrence)"

Section 2: Things to manually check
- Uncertain cross-references: references where target exists but semantic match was unclear
  Example: "Clause 10.7 reference in Section 15 — points to carryover clause, but
            context seems to be about payment on termination. May intend clause 10.6."
- References to unseen document parts: if only part of document was reviewed
  Example: "Cross-references to Part 1 terms could not be verified (Part 1 not provided)"
- Plausible-but-uncertain references: clause exists and is plausibly correct but content
  match felt uncertain

Section 3: Clean document confirmation (when applicable)
- If no issues were found: state this clearly and confidently
- Do not flag style preferences or precision improvements as errors
  Example: "No numbering errors or stale cross-references were found. The document
            appears correctly structured."
```

### Change Log Format

During the fix process, each change is recorded as it is made.

```text { .api }
Change log entry format (running list maintained during fixes):

Cross-reference changes:
- "Section [X], paragraph [N]: Updated 'clause [OLD]' to 'clause [NEW]'"

Numbering fixes:
- "Clause [NUMBER]: [Description of problem] → [Description of fix]"

Structural changes:
- "[Action]: [Description of what was changed and why]"
```

### Report Delivery

```text { .api }
Report is presented to the user as a structured markdown summary after all changes are complete.

Tone guidelines:
- Be clear and specific about each change
- Be honest about uncertainty — flag rather than guess
- If the document is clean, say so confidently without inventing issues
- Do not flag style preferences or precision improvements as errors
- A clean document result is a good outcome
```

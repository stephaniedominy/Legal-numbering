# Cross-Reference Handling

Finding, verifying, and fixing cross-references in legal contracts. Cross-references point from one part of a contract to another by clause number. After manual edits, these references can become stale (pointing to wrong or non-existent clauses).

## Capabilities

### Cross-Reference Discovery

Scans the entire document for all cross-reference patterns.

```text { .api }
Reference patterns to scan for:
- "clause X"
- "clauses X and Y"
- "section X"
- "sub-clause X.Y"
- "in accordance with X"
- "pursuant to clause X"
- "as set out in X"
- "referred to in clause X"

For Google Docs:
- Visual scan during screenshot review

For .docx:
- Use regex search across all paragraphs programmatically

Coverage requirement:
- Scan the entire document including boilerplate and end sections
- It is easy to focus on obvious problems near the top and miss stale
  references in the signature blocks, schedules, or boilerplate near the end
```

**python-docx regex example:**

```python
import re
from docx import Document

doc = Document("contract.docx")
pattern = re.compile(
    r'(?:clause|section|sub-clause|pursuant to|in accordance with|as set out in|referred to in)\s+(\d+(?:\.\d+)*)',
    re.IGNORECASE
)

cross_refs = []
for i, para in enumerate(doc.paragraphs):
    for match in pattern.finditer(para.text):
        cross_refs.append({
            "para_index": i,
            "full_match": match.group(0),
            "clause_number": match.group(1),
            "context": para.text
        })
```

### Cross-Reference Verification

Verifies each cross-reference against the clause map using two criteria.

```text { .api }
Verification criteria for each cross-reference:

Question 1 - Existence check:
  Does the referenced clause number appear in the clause map?
  If NO → clear error (clause was renumbered or deleted)

Question 2 - Semantic check:
  Does the referenced clause describe what the surrounding text implies it should?
  Requires reading both the reference context and the target clause content.
  If NO and a better-matching clause exists nearby → flag as error
```

### Error vs. Non-Error Classification

```text { .api }
Genuine errors (fix or flag):
✅ Referenced clause number does not exist in clause map, but a nearby clause matches
✅ Referenced clause exists but its content is semantically unrelated to the reference context
✅ Numbering changed so the clause number now points to completely different content

Not errors (leave unchanged):
❌ Referencing a section by top-level number is normal legal drafting
   Example: "clause 5" meaning the entire Confidentiality section
❌ Survival clauses referencing whole sections by number
   Example: "clauses 5 and 7 shall survive" for Confidentiality and Limitation of Liability
❌ "see clause 1.1" pointing to a definition, even if wording could be more specific

Rule: Only flag when the clause pointed to is clearly wrong or doesn't exist.
When uncertain, preserve and flag rather than change.
```

### Cross-Reference Repair

Applies fixes to stale cross-references using the appropriate method for the document type.

```text { .api }
Google Docs repair process:
1. Open Edit → Find and Replace (Ctrl+H)
2. Enter old clause reference in "Find" field
3. Enter new clause reference in "Replace" field
4. Use "Replace" (not "Replace all") to review each instance
5. Verify the number doesn't appear in other contexts where it should NOT change
6. Document each change in running change log

Word (.docx) repair process:
1. Match full phrase (e.g., "clause 10.6") not just the number
2. Use python-docx to find and replace text across all paragraphs and runs
3. Document each change in running change log
```

**python-docx replace example:**

```python
def replace_in_doc(doc, old_text, new_text):
    """Replace text across all paragraphs and runs."""
    changes = []
    for para in doc.paragraphs:
        for run in para.runs:
            if old_text in run.text:
                run.text = run.text.replace(old_text, new_text)
                changes.append(f"Replaced '{old_text}' → '{new_text}' in: {para.text[:80]}")
    return changes

# Example: fix specific reference
changes = replace_in_doc(doc, "clause 10.6", "clause 10.7")
doc.save("contract_fixed.docx")
```

### Safety Guidelines for Cross-Reference Fixes

```text { .api }
Safety rules:
- Match full phrases, not bare numbers, to avoid unintended replacements
- Use "Replace" (not "Replace all") in Google Docs when the same number may appear legitimately elsewhere
- When uncertain whether a reference is wrong, flag it for the user rather than changing it
- A correct reference left unchanged is better than an incorrect "fix"
- Document every change made for user review
```

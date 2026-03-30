# Contract Numbering Fixer

Fixes two common contract-editing problems:

1. **Broken list numbering**  
   Auto-numbered clauses/sub-clauses go out of sync (duplicates, skips, mid-document resets like `1.1` appearing inside clause 10).
2. **Stale cross-references**  
   Text references like “see clause 12.6” no longer match the clause numbering after edits.

This skill guides a **systematic** workflow to repair numbering and validate/update cross-references without introducing new drafting errors.

---

## When to use

Use this skill when a user says things like:
- “Fix the numbering in my contract”
- “Clause references look wrong / stale”
- “Auto-numbering is garbled”
- “I edited sections and now the numbering is off”
- “Can you check cross-references?”

Works best for:
- Employment agreements
- NDAs
- Service agreements
- Shareholder agreements
- Any Word/Google Docs contract with numbered clauses and internal cross-references

---

## Supported formats

- **Google Docs** (URL shared or already open in Chrome)
- **Word `.docx`** (file upload or local path)

---

## Workflow (high level)

### 1) Identify the document
- Determine whether it’s a Google Doc or `.docx`.
- For Google Docs: orient with screenshots before editing.
- For `.docx`: use `python-docx` to inspect and edit.

### 2) Build a clause map (ground truth)
Create a full map of the document’s intended structure:
- Every top-level clause number + heading
- Every sub-clause number + brief description

This map is your reference for validating numbering and cross-references.

### 3) Extract all cross-references
Scan for references such as:
- “clause X”, “clauses X and Y”
- “pursuant to clause X”
- “as set out in X”
- “referred to in clause X”
- “section X”, “sub-clause X.Y”

For `.docx`, use a regex-based scan so you don’t miss any.

### 4) Verify each cross-reference
For every reference:
1. **Does the referenced clause exist?**
2. **Does it describe what the referencing text implies?**

If uncertain, **don’t change it**—flag for manual review.

### 5) Fix cross-references safely
- Prefer Find & Replace (Google Docs) or targeted run/paragraph replacements (`python-docx`).
- Match the full phrase (e.g., “clause 10.6”), not just the number.

Maintain a running “changes made” log.

### 6) Fix broken numbering
- Google Docs: “Continue numbering” / “Restart numbering”; re-join broken lists.
- `.docx`: correct list properties (`numId`/`ilvl`) or (if safer) convert to explicit numbering text where dynamic lists aren’t required.

### 7) Handle structural issues
Examples:
- A heading accidentally turned into a sub-clause
- Duplicate clause body pasted twice
- Phantom clauses from empty list items

Make small, controlled edits and verify immediately.

### 8) Final verification pass
- Rebuild the clause map and confirm numbering is sequential and unique.
- Re-scan all cross-references and confirm each points to the correct clause.
- Pay special attention near areas that changed.

---

## What the skill will output

At the end, provide a short report:

### Changes made
- Each cross-reference updated (old → new)
- Each numbering fix (what was wrong → what it is now)
- Any structural edits (removed/moved items)

### Things to manually check
- Any reference where the intended target was ambiguous
- References to sections not visible in the excerpt
- Any “plausible but uncertain” matches left unchanged

---

## Safety / drafting principles

- Fix **genuine errors**, not style preferences.
- Referencing a clause by **top-level number** (e.g., “clause 6”) is often valid.
- When unsure: **preserve** and flag, rather than guess.

---

## Tooling notes (for `.docx` work)

Install `python-docx`:

```bash
pip install python-docx --break-system-packages

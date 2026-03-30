# Clause Analysis

Systematic construction of a complete clause map from a legal contract document, and detection of numbering problems. The clause map serves as the ground truth for all subsequent verification and repair steps.

## Capabilities

### Clause Map Construction

Builds a complete structural inventory of all clause numbers and headings before making any changes.

```text { .api }
Clause map structure:
- Record every top-level clause number and heading
  Example: "10. Holiday Entitlement"
- Record every sub-clause number and short description
  Example: "10.4 Carry-over of unused days"

For Google Docs:
- Scroll through entire document taking screenshots page by page
- Record all visible clause numbers from screenshots

For .docx:
- Parse all paragraphs programmatically
- Extract numbered list items with their text content
```

The clause map must be built before any changes are made and kept accurate throughout the process.

### Broken Numbering Detection

Identifies patterns that indicate list numbering has gone out of sync.

```text { .api }
Broken numbering indicators:
- Sub-clause resets to "1.x" mid-document
  Example: "1.1" appearing inside clause 10 (should be "10.x")
- Duplicate clause numbers: two distinct clauses share the same number
  Example: two separate entries both labeled "26.3"
- Skipped numbers: sequential gap in clause numbers
  Example: 26.2 followed directly by 26.4 with no 26.3
- Empty clauses: clause heading immediately followed by next heading, no body text
- Misplaced content: text that belongs to one clause incorrectly nested under another
```

**Common Root Cause:**
When a paragraph in Google Docs "1.1" appears mid-document inside a higher-numbered clause, it typically means the paragraph joined a new list instead of continuing the existing one. Fix by re-joining the list.

### Structural Problem Detection

Identifies document structural issues beyond simple numbering errors.

```text { .api }
Structural problems:
- Clause heading accidentally converted to a sub-clause
  Example: "Holidays and other paid leave" appearing as "26.4" instead of a top-level heading
- Duplicate clause body: same clause content appearing twice
- Phantom clause: empty list item creating a spurious clause number
- Content belonging to one clause embedded as sub-clause of another
```

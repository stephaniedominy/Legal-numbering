# Numbering Repair

Fixing broken list numbering and structural problems in legal contracts. Addresses cases where automatic list numbering has gone out of sync after manual editing.

## Capabilities

### Google Docs List Numbering Repair

Repairs broken numbered lists in Google Docs using built-in list controls and cut/paste operations.

```text { .api }
Repair methods:

Method 1 - List continuation fix:
  Right-click the mis-numbered item
  Select "Continue numbering" to rejoin the preceding list
  Use when the item should continue from the previous list item

Method 2 - Restart at specific number:
  Right-click the mis-numbered item
  Select "Restart numbering"
  Set the desired starting value
  Use when a section needs to restart at a specific number

Method 3 - Re-joining detached list items:
  Cut the incorrectly-numbered paragraph(s)
  Re-paste at the correct location to rejoin the list
  Use when a paragraph joined a new list instead of continuing the existing one
  (Common cause of "1.1" appearing inside clause 10)
```

**Safety protocol for Google Docs structural changes:**

```text { .api }
Before each deletion or structural change:
1. Take a screenshot of the area before making changes
2. Select only the exact item(s) to remove
3. Make the change
4. Take a screenshot immediately after
5. Verify surrounding clauses are intact
6. If something went wrong: use Ctrl+Z immediately before doing anything else
```

### Word (.docx) List Numbering Repair

Repairs broken list numbering in .docx files by modifying numbering properties via python-docx.

```text { .api }
Numbering properties:
- numId: identifies which numbering definition the paragraph belongs to
- ilvl: indent level within the numbering definition (0 = top-level, 1 = sub-level, etc.)

Repair method 1 - Reset to correct numbering definition:
  Identify paragraphs with incorrect numId (member of wrong list)
  Set numId to match the surrounding correct list paragraphs

Repair method 2 - Fix indent level:
  Identify paragraphs with incorrect ilvl
  Set ilvl to the correct level for the clause hierarchy

Repair method 3 - Convert to explicit text:
  If dynamic numbering is not critical, remove list properties
  Replace with explicitly typed clause numbers (e.g., "10.4  text...")
  Use when the document doesn't rely on dynamic list numbering elsewhere
```

**python-docx numbering repair example:**

```python
from docx import Document
from docx.oxml.ns import qn
from lxml import etree
import copy

doc = Document("contract.docx")

def get_num_props(para):
    """Get numId and ilvl for a paragraph."""
    pPr = para._element.pPr
    if pPr is None:
        return None, None
    numPr = pPr.numPr
    if numPr is None:
        return None, None
    ilvl = numPr.ilvl.val if numPr.ilvl is not None else None
    numId = numPr.numId.val if numPr.numId is not None else None
    return numId, ilvl

def set_num_props(para, numId, ilvl):
    """Set numId and ilvl for a paragraph."""
    pPr = para._element.get_or_add_pPr()
    # Remove existing numPr if present
    existing = pPr.find(qn('w:numPr'))
    if existing is not None:
        pPr.remove(existing)
    # Create new numPr
    numPr = etree.SubElement(pPr, qn('w:numPr'))
    ilvl_el = etree.SubElement(numPr, qn('w:ilvl'))
    ilvl_el.set(qn('w:val'), str(ilvl))
    numId_el = etree.SubElement(numPr, qn('w:numId'))
    numId_el.set(qn('w:val'), str(numId))

# Example: fix paragraph 42 to use numId=2, ilvl=1 (sub-clause of list 2)
target_para = doc.paragraphs[42]
set_num_props(target_para, numId=2, ilvl=1)
doc.save("contract_fixed.docx")
```

### Structural Problem Repair

Fixes structural issues beyond simple numbering, such as misplaced headings and phantom clauses.

```text { .api }
Structural repair types:

1. Clause heading converted to sub-clause:
   - Identify the paragraph with incorrect list membership
   - Remove list numbering properties (set as non-list paragraph)
   - Apply appropriate heading style

2. Duplicate clause body:
   - Identify the duplicate paragraph(s)
   - Select exactly those paragraphs
   - Delete them
   - Verify surrounding content remains intact

3. Phantom clause (empty list item):
   - Identify the empty numbered paragraph
   - Delete the empty paragraph
   - Verify subsequent clause numbers renumber correctly

4. Content incorrectly nested:
   - Identify misplaced paragraphs
   - Cut and paste to correct location
   - Verify list membership and numbering after move
```

**Google Docs structural repair safety checklist:**

```text { .api }
Before deletion:
☐ Screenshot the problem area
☐ Identify exact paragraphs to delete using visible numbers as anchors
☐ Click at start of first item, Shift+click at end of last item

After deletion:
☐ Screenshot immediately
☐ Verify surrounding clauses are intact
☐ Verify numbering of subsequent clauses is correct

If error:
☐ Ctrl+Z immediately before any other action
☐ Re-screenshot to verify undo was successful
```

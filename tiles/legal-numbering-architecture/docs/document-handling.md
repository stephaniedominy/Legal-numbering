# Document Handling

Support for loading and interacting with legal contract documents in two formats: Google Docs (via Chrome integration) and Word documents (.docx via python-docx).

## Capabilities

### Google Docs

Identifies and accesses Google Docs using Chrome tools. Requires the document to be accessible via URL or already open in Chrome.

```text { .api }
Input:
- Google Doc URL, or document already open in Chrome

Process:
- Use Claude in Chrome tools to navigate to the document
- Take a screenshot to orient and confirm correct document
- Scroll through entire document taking screenshots of each page
- Capture all clause numbers via visual inspection

Limitations:
- Requires Chrome integration to be active
- All interactions are visual (screenshot-based)
```

**Usage Example:**

When the user provides a Google Docs URL:
1. Navigate to the URL using Chrome tools
2. Take an initial screenshot to confirm the document
3. Scroll through all pages, screenshotting each to build the clause map
4. Use Edit → Find and Replace (Ctrl+H) for all text changes

### Word Documents (.docx)

Accesses .docx files programmatically using the python-docx library.

```text { .api }
Input:
- File path to .docx file, or uploaded file

Setup:
- pip install python-docx --break-system-packages

Process:
- Parse full document text programmatically
- Use regex to extract all cross-references
- Access paragraph numId and ilvl properties for numbering analysis
- Apply fixes programmatically across all paragraphs and runs
```

**Usage Example:**

```python
from docx import Document

doc = Document("contract.docx")

# Iterate paragraphs
for para in doc.paragraphs:
    print(para.text)
    # Access numbering properties
    if para._element.pPr is not None:
        numPr = para._element.pPr.numPr
        if numPr is not None:
            ilvl = numPr.ilvl.val
            numId = numPr.numId.val

# Find and replace text
import re
pattern = re.compile(r'clause \d+(\.\d+)*', re.IGNORECASE)
for para in doc.paragraphs:
    for run in para.runs:
        matches = pattern.findall(run.text)
        # process matches

doc.save("contract_fixed.docx")
```

### Document Type Identification

```text { .api }
If user hasn't specified document type:
- Ask for document: Google Doc URL or .docx file path/upload
- Google Doc: treat as Chrome-based visual workflow
- .docx: treat as programmatic python-docx workflow
```

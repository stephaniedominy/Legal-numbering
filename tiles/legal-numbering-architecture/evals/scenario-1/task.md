# Clause Map Building

## Capabilities { .capabilities }

Design a solution that builds a complete clause map from a legal contract document. The clause map must enumerate every top-level clause and every sub-clause, recording both the clause number and a short heading/description. This map will be used as ground truth for all subsequent analysis steps.

- Given a document with top-level clauses (e.g., "1. Definitions", "2. Term", "10. Holiday Entitlement"), the clause map includes every top-level clause with its number and heading [@test](./tests/top_level_clauses.test.md)
- Given a document with sub-clauses (e.g., "10.1 Annual leave entitlement", "10.4 Carry-over of unused days"), the clause map includes every sub-clause with its number and short description [@test](./tests/sub_clauses.test.md)
- Given a .docx document, the solution parses the full text programmatically (not by screenshot) to produce the clause map [@test](./tests/docx_programmatic.test.md)
- The produced clause map is maintained as the ground truth reference throughout the session and not discarded after initial analysis [@test](./tests/map_persistence.test.md)

## Implementation { .implementation }

The solution should open the target document (Google Doc or .docx) and extract a structured list of every clause it contains. For each clause, it records the clause number (e.g., `10`, `10.1`, `10.4`) and the associated heading or short description. For Word documents, extraction must be performed programmatically by iterating over the document's paragraphs using python-docx — reading screenshots is not sufficient. Once built, the clause map must be retained in memory and referenced in all subsequent steps; it must not be rebuilt from scratch or discarded mid-session.

## API { .api }

Accepts a document (Google Doc or .docx) and outputs a structured clause map. Each entry in the map contains:
- `clause_number`: the numeric identifier (e.g., `"10.1"`)
- `heading`: a short heading or description for the clause

## Dependencies { .dependencies }

### contract-numbering-fixer 0.1.0 { .dependency }

AI skill that reviews and fixes clause numbering issues and stale cross-references in legal contracts.

# Broken Numbering Detection

## Capabilities { .capabilities }

Design a solution that detects all signs of broken automatic list numbering in a legal contract. The solution must identify at least four distinct patterns of numbering errors: sub-clause numbering that resets mid-document, duplicate clause numbers, skipped clause numbers, and empty list items creating phantom clauses.

- Given a document where sub-clauses reset to "1.1" inside a higher-numbered section (e.g., "1.1" appearing within clause 10), the solution flags this as a numbering reset error [@test](./tests/numbering_reset.test.md)
- Given a document with two clauses both numbered "26.3", the solution flags the duplicate clause number [@test](./tests/duplicate_number.test.md)
- Given a document that jumps from "26.2" to "26.4" with no "26.3", the solution flags the skipped number [@test](./tests/skipped_number.test.md)
- Given a document with an empty list item that creates a phantom clause number, the solution flags it for removal [@test](./tests/phantom_clause.test.md)

## Implementation { .implementation }

The solution should operate on a clause map and the raw document content. It should perform four distinct checks:

1. **Reset detection**: scan for sub-clause numbers that revert to a low value (e.g., `1.1`) in a position where the parent clause number is much higher, indicating an automatic numbering reset.
2. **Duplicate detection**: compare all clause numbers in the map and flag any number that appears more than once.
3. **Skip detection**: for each parent clause, verify that its sub-clauses form a contiguous sequence with no gaps; flag any missing number.
4. **Phantom clause detection**: identify empty or blank list items in the document that have been assigned a clause number by the automatic numbering system, and flag them for removal.

Each detected error should include the error type and its location within the document.

## API { .api }

Accepts a clause map and raw document content. Returns a list of detected numbering errors, where each error contains:
- `type`: one of `reset`, `duplicate`, `skip`, or `phantom`
- `location`: the clause number or position in the document where the error was found

## Dependencies { .dependencies }

### contract-numbering-fixer 0.1.0 { .dependency }

AI skill that reviews and fixes clause numbering issues and stale cross-references in legal contracts.

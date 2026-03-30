# Error vs. Style Distinction

## Capabilities { .capabilities }

Design a solution that correctly distinguishes genuine cross-reference errors from valid legal drafting conventions. The solution must not flag valid conventions such as referencing a section by its top-level number as errors, and must only flag a reference when it clearly points to the wrong or non-existent clause.

- Given "clause 5" referring to section 5 "Confidentiality" (the whole section), the solution does NOT flag this as an error — it is a valid convention to reference a section by its top-level number [@test](./tests/valid_section_ref.test.md)
- Given "clauses 5 and 7 shall survive" in a survival clause, the solution does NOT flag these section-level references as errors [@test](./tests/valid_survival_clause.test.md)
- Given "see clause 16" where clause 16 does not exist in the document but clause 15 is titled "Post-Termination Restrictions" and matches the surrounding context, the solution flags clause 16 as a genuine error [@test](./tests/genuine_error.test.md)
- Given "subject to clause 10.7" where 10.7 is about something completely unrelated but nearby clause 10.6 matches perfectly, the solution flags 10.7 as a genuine error [@test](./tests/topical_error.test.md)

## Implementation { .implementation }

Use the `contract-numbering-fixer` package to analyse each cross-reference in context. For each reference, consult the clause map to determine whether the referenced clause exists and whether it is topically consistent with the surrounding text. Preserve references that follow recognised legal drafting conventions (section-level references, survival clauses) and report only references that are clearly erroneous.

## API { .api }

Accepts a cross-reference list, a clause map, and the full document text. Outputs a filtered list separating genuine errors from valid conventions.

## Dependencies { .dependencies }

### contract-numbering-fixer 0.1.0 { .dependency }

AI skill that reviews and fixes clause numbering issues and stale cross-references in legal contracts.

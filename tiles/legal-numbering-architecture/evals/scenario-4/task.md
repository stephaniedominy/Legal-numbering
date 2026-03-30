# Cross-Reference Semantic Verification

## Capabilities { .capabilities }

Design a solution that verifies not only that each cross-reference points to an existing clause, but also that the referenced clause is semantically correct — i.e., the clause actually describes what the surrounding text implies it should. The solution must read both the text surrounding each reference and the content of the referenced clause to assess whether the match is genuine.

- Given "subject to clause 10.7" where 10.7 concerns holiday payment on termination but the surrounding text is about carry-over of unused days, the solution identifies this as a potential semantic mismatch and suggests checking nearby clause 10.6 (carry-over rules) [@test](./tests/semantic_mismatch.test.md)
- Given "see clause 15 (Post-Termination Restrictions)" where clause 15 is indeed the Post-Termination Restrictions section, the solution confirms the reference is semantically correct [@test](./tests/semantic_correct.test.md)
- Given a reference where the solution cannot confidently determine semantic correctness, it flags the reference for manual review rather than making an incorrect change [@test](./tests/uncertain_semantic.test.md)

## Implementation { .implementation }

The solution should process each cross-reference extracted from the document and perform a two-part semantic check:

1. **Context reading**: examine the text surrounding the reference to understand what the author intends the referenced clause to cover.
2. **Clause content reading**: retrieve and read the actual body content of the referenced clause from the document — not just its heading from the clause map.

The solution then compares the stated intent (from surrounding context) against the actual content of the referenced clause. If the content matches the intent, the reference is marked `correct`. If the content clearly does not match, the reference is marked `incorrect` and a suggestion for the likely correct clause may be offered. If the solution cannot confidently determine whether the content matches, the reference must be marked `uncertain` and flagged for manual review — no change should be made in uncertain cases.

## API { .api }

Accepts the full cross-reference list, the clause map, and the full document text. Returns, for each reference, a verdict object containing:
- `verdict`: one of `correct`, `incorrect`, or `uncertain`
- `explanation`: a brief explanation of why the verdict was assigned

## Dependencies { .dependencies }

### contract-numbering-fixer 0.1.0 { .dependency }

AI skill that reviews and fixes clause numbering issues and stale cross-references in legal contracts.

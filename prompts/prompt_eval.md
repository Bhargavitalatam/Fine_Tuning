# Prompt Engineering Evaluation — Part G

This document compares the performance of the 3 prompt iterations against the fine-tuned model for the 3 most challenging baseline documents.

---

## Evaluation Set (Worst Performers from Part C)
1.  `test_INV_01`: Failed due to markdown wrapping in initial baseline.
2.  `test_INV_02`: Failed due to conversational "Here is the JSON" prefix.
3.  `test_PO_01`: Failed due to incorrect key names (`po_ref`) and incorrect numeric types.

---

## Results Table

| Document ID | Prompt v1 (Baseline) | Prompt v2 (Strict) | Prompt v3 (Few-Shot) | Fine-Tuned Model |
| :--- | :--- | :--- | :--- | :--- |
| **test_INV_01** | FAIL (Markdown) | SUCCESS | SUCCESS | **SUCCESS** |
| **test_INV_02** | FAIL (Prose) | FAIL (String float) | SUCCESS | **SUCCESS** |
| **test_PO_01** | FAIL (Key Mismatch) | FAIL (Key Mismatch) | SUCCESS | **SUCCESS** |
| **Summary** | 0/3 Success | 1/3 Success | 3/3 Success | **3/3 Success** |

---

## Summary Comparison

### Prompt Engineering (Few-Shot)
- **Strengths**: Can achieve 100% parse success on simple cases by giving the model a clear pattern to follow. Requires no training time or GPU resources.
- **Weaknesses**: High token cost (adding long examples to every request). Performance degrades as documents become more divergent from the few-shot example. Brittle against new layouts.

### Fine-Tuning (LoRA)
- **Strengths**: Consistently achieves 95%+ parse success without needing any examples in the prompt. Higher reliability across a wider range of document layouts. Faster and cheaper inference (fewer tokens per request).
- **Weaknesses**: Requires upfront effort for data curation and training. Model must be updated if the schema or domain changes significantly.

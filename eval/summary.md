# Baseline Evaluation Summary

## Aggregate Parse Success Rate: **15.0%**

### Analysis of Failure Cases
- **Markdown Wrapping**: 14/20 responses were wrapped in code fences, failing programmatic parsing.
- **Key Mismatches**: 2/20 responses renamed 'invoice_number' or 'po_number'.
- **Prose Prefixes**: 2/20 responses included conversational preambles.

### Baseline Conclusion
The base Llama 3.2-3B model, despite being highly capable, fails consistently to return 'naked' JSON objects. It prioritizes chat-style interactions and markdown formatting over strict structural compliance. This establishes a baseline of 10% parse success Rate.
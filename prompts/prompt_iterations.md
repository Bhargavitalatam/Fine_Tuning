# Prompt Iteration — Part G: Prompt Engineering Comparison

This document records the exact text of 3 prompt versions used to test the limits of prompt engineering on the base Llama 3.2-3B model.

---

## Prompt Version 1: Direct Instruction (Baseline)
> **Strategy**: Simple direct instruction with schema hints.

```text
Extract all invoice/PO fields from the following text and return ONLY a valid JSON object. Do not provide any explanation or markdown code fences. Structure must match the provided JSON schema.

Document: {document_text}
```

---

## Prompt Version 2: Strict Formatting Constraints
> **Strategy**: Emphasis on negative constraints ("DO NOT") and structural "hard" rules to prevent common base-model failures.

```text
### Task: Precise JSON Extraction
Extract every field from the document below.

### Output Constraints:
- Return ONLY the naked raw JSON string.
- DO NOT wrap the output in markdown code blocks like ```json ... ```.
- DO NOT add a conversational prefix like "Here is your JSON".
- Ensure all numeric values are floats (e.g. 100.0) not strings ("100.0").
- Use ONLY the key names provided: [vendor, invoice_number, date, due_date, currency, subtotal, tax, total, line_items] OR [buyer, supplier, po_number, date, delivery_date, currency, total, items].

Document: {document_text}

JSON Output:
```

---

## Prompt Version 3: Few-Shot Example
> **Strategy**: Provide one full, perfect example of a raw-text-to-JSON mapping to guide the model's pattern recognition.

```text
### Task: Extract raw text into the following JSON schema.

### Example:
Input:
VENDOR: Acme Corp\nINV NO: #12345\nDATE: 2024-03-01\nTOTAL: 1080.00
Output:
{"vendor": "Acme Corp", "invoice_number": "#12345", "date": "2024-03-01", "due_date": null, "currency": "USD", "subtotal": 1000.0, "tax": 80.0, "total": 1080.0, "line_items": []}

### Actual Task:
Input:
{document_text}
Output:
```

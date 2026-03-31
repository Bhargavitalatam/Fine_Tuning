# Failure Analysis 01 — Nested Line Items

### Raw Document Text
```
PROJECT: Alpha Development
- Design Phase | 10 hrs | 100.00 | 1000.00
- Testing Phase | 5 hrs | 80.00 | 400.00
SUBTOTAL PROJECT ALPHA: 1400.00

PROJECT: Beta Deployment
- Hardware Kit X | 1 | 500.00 | 500.00
TOTAL DUE: 1900.00
```

### Expected JSON Output
```json
{
  "vendor": null,
  "invoice_number": null,
  "date": null,
  "due_date": null,
  "currency": null,
  "subtotal": 1900.0,
  "tax": null,
  "total": 1900.0,
  "line_items": [
    {"description": "Design Phase", "quantity": 10.0, "unit_price": 100.0},
    {"description": "Testing Phase", "quantity": 5.0, "unit_price": 80.0},
    {"description": "Hardware Kit X", "quantity": 1.0, "unit_price": 500.0}
  ]
}
```

### Model's Actual Response
```json
{
  "vendor": null,
  "invoice_number": null,
  "date": null,
  "due_date": null,
  "currency": null,
  "subtotal": 1900.0,
  "tax": null,
  "total": 1900.0,
  "line_items": [
    {"description": "PROJECT ALPHA", "quantity": 1.0, "unit_price": 1400.0},
    {"description": "Hardware Kit X", "quantity": 1.0, "unit_price": 500.0}
  ]
}
```

### Analysis
- **What went wrong**: The model failed to flatten the nested hierarchy. It extracted the subtotal summary ("PROJECT ALPHA: 1400.00") as a single line item instead of extracting the individual services (Design/Testing) underneath it.
- **Why it likely failed**: The training data primarily consists of flat, single-level line items. The model hasn't learned to ignore "summary rows" in favor of the atomic line items.
- **What training data change would fix it**: Include at least 15 examples of "nested" or "grouped" invoices where multiple sub-sections are present. The ground truth for these must explicitly map the atomic items (lowest level) while ignoring the section headers and intermediate subtotals.

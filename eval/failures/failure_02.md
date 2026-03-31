# Failure Analysis 02 — Thousands Separator Confusion

### Raw Document Text
```
Invoice #: EU-9901
Date: 15.01.2024
Total Amount Inclusive: 10.000,50 EUR
Tax (19%): 1.596,72 EUR
Subtotal: 8.403,78 EUR
```

### Expected JSON Output
```json
{
  "vendor": null,
  "invoice_number": "EU-9901",
  "date": "2024-01-15",
  "due_date": null,
  "currency": "EUR",
  "subtotal": 8403.78,
  "tax": 1596.72,
  "total": 10000.5,
  "line_items": []
}
```

### Model's Actual Response
```json
{
  "vendor": null,
  "invoice_number": "EU-9901",
  "date": "2024-01-15",
  "due_date": null,
  "currency": "EUR",
  "subtotal": 8.40,
  "tax": 1.59,
  "total": 10.0
}
```

### Analysis
- **What went wrong**: The model misidentified the European thousands separator (`.`) as a decimal point, resulting in radically incorrect float values (e.g., extracting `10.0` instead of `10000.5`).
- **Why it likely failed**: The initial 80-example dataset used standard US-style formatting (`1,000.00`) as the primary numeric pattern. The model defaulted to interpreting the first encountered dot as a decimal separator.
- **What training data change would fix it**: Add 10-15 European-style invoice examples that use `,` as a decimal separator and `.` as a thousands separator. This would force the model to look for consistency across the entire numeric string before deciding on the decimal point.

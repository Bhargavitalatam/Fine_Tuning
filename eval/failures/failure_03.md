# Failure Analysis 03 — Ambiguous Tax Field

### Raw Document Text
```
Invoice #: 88/2024
Date: 2024-03-31
Total: 100.00
Inclusive of Standard Levies and Surcharges (15.00)
```

### Expected JSON Output
```json
{
  "vendor": null,
  "invoice_number": "88/2024",
  "date": "2024-03-31",
  "due_date": null,
  "currency": null,
  "subtotal": 85.0,
  "tax": 15.0,
  "total": 100.0,
  "line_items": []
}
```

### Model's Actual Response
```json
{
  "vendor": null,
  "invoice_number": "88/2024",
  "date": "2024-03-31",
  "due_date": null,
  "currency": null,
  "subtotal": 100.0,
  "tax": null,
  "total": 100.0
}
```

### Analysis
- **What went wrong**: The model failed to recognize "Standard Levies and Surcharges" as a synonym for "tax", resulting in a `null` tax field and an incorrect subtotal (set to match the total).
- **Why it likely failed**: The model was trained heavily on common terms like "Tax", "VAT", and "GST". It didn't generalize to more niche financial terminology like "Levies" or "Surcharges".
- **What training data change would fix it**: Include 10-15 examples of invoices that use non-standard tax descriptors (e.g., "Levies", "Customs Duties", "Environmental Fee", "Service Charge Tax"). This will increase the model's semantic range for the `tax` key.

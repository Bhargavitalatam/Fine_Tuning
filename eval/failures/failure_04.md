# Failure Analysis 04 — Messy Layout Intersection

### Raw Document Text
```
VENDOR      INV_NO      DATE        TOTAL
Acme Corp   #8822       2024-03-31  1000.00
SUBTOTAL: 900.00   TAX: 100.00   CURR: USD
```

### Expected JSON Output
```json
{
  "vendor": "Acme Corp",
  "invoice_number": "#8822",
  "date": "2024-03-31",
  "due_date": null,
  "currency": "USD",
  "subtotal": 900.0,
  "tax": 100.0,
  "total": 1000.0,
  "line_items": []
}
```

### Model's Actual Response
```json
{
  "vendor": "Acme Corp #8822",
  "invoice_number": "2024-03-31",
  "date": "1000.00",
  "due_date": null,
  "currency": null,
  "subtotal": 900.0,
  "tax": 100.0,
  "total": null
}
```

### Analysis
- **What went wrong**: The model failed to correctly map the horizontal alignment of the headers (`VENDOR`, `INV_NO`, `DATE`, `TOTAL`) to the values in the row beneath it. It shifted the values by one column, incorrectly identifying the invoice number as the vendor.
- **Why it likely failed**: The initial 80-example training set used clean, well-spaced tables. It didn't account for complex horizontal layouts where multiple fields are packed onto a single line without clear column delimiters (like tabs or pipes).
- **What training data change would fix it**: Include 10-15 examples of "dense headers" where vendor, ID, and date are on the same line. Use varied whitespace and non-standard alignments to teach the model that it must look at the vertical alignment of the header labels relative to the values.

# Failure Analysis 05 — Multi-Page Document Continuity

### Raw Document Text
```
PURCHASE ORDER: PO-7788 (Page 1 of 2)
FROM: Bio Tech Corp
TO: Lab Supply Ltd
Date: 2024-03-31

ITEMS:
- Pipettes | 100 | 5.0
- Brackets | 50 | 10.0

... [Skip Page break] ...

(Page 2 of 2)
- Reagents | 20 | 25.0
PO TOTAL: 1500.00 USD
```

### Expected JSON Output
```json
{
  "buyer": "Bio Tech Corp",
  "supplier": "Lab Supply Ltd",
  "po_number": "PO-7788",
  "date": "2024-03-31",
  "delivery_date": null,
  "currency": "USD",
  "total": 1500.0,
  "items": [
    {"item_name": "Pipettes", "quantity": 100.0, "unit_price": 5.0},
    {"item_name": "Brackets", "quantity": 50.0, "unit_price": 10.0},
    {"item_name": "Reagents", "quantity": 20.0, "unit_price": 25.0}
  ]
}
```

### Model's Actual Response
```json
{
  "buyer": "Bio Tech Corp",
  "supplier": "Lab Supply Ltd",
  "po_number": "PO-7788",
  "date": "2024-03-31",
  "delivery_date": null,
  "currency": "USD",
  "total": 1000.0,
  "items": [
    {"item_name": "Pipettes", "quantity": 100.0, "unit_price": 5.0},
    {"item_name": "Brackets", "quantity": 50.0, "unit_price": 10.0}
  ]
}
```

### Analysis
- **What went wrong**: The model failed to process multi-page continuity. It correctly extracted the items from the first page but completely ignored the third item and the grand total on the second page, likely assuming the document ended after the first page block.
- **Why it likely failed**: The training data focused on single-page, short-form documents (<512 tokens). When faced with longer inputs where relevant context is split by "page breaks" or long spans of boilerplate text, the model loses coherence and attention to the latter sections.
- **What training data change would fix it**: Include 10-15 multi-page PO and Invoice examples in the JSONL dataset. These examples should have relevant fields (like `total`) or line items specifically positioned *after* page break markers or long filler sections, forcing the model to attend to the entire document.

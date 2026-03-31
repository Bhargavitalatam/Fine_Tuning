# Purchase Order JSON Schema

This document defines the structured JSON format for extracting information from purchase orders. Every training example must conform to this schema exactly.

| Key | Description | Format | If Absent |
| :--- | :--- | :--- | :--- |
| `buyer` | The name of the company or entity that is purchasing the goods or services. | string | `null` |
| `supplier` | The name of the company or entity that will provide the goods or services. | string | `null` |
| `po_number` | The unique numeric or alphanumeric reference assigned to the purchase order. | string | `null` |
| `date` | The date the purchase order was officially created and issued. | YYYY-MM-DD string | `null` |
| `delivery_date` | The date by which the buyer expects the goods or services to be delivered. | YYYY-MM-DD string | `null` |
| `currency` | The three-letter ISO 4217 code for the purchase order currency. | string | `null` |
| `total` | The total value or total cost expected for the entire order. | float | `null` |
| `items` | A collection of objects representing the specific items ordered. | array of objects | `[]` |

### Item Object
| Key | Description | Format | If Absent |
| :--- | :--- | :--- | :--- |
| `item_name` | The name or a short description of the specific item being ordered. | string | `null` |
| `quantity` | The numeric quantity of the item being ordered from the supplier. | float | `null` |
| `unit_price` | The price per single unit of the item being ordered from the supplier. | float | `null` |

---
**Binding Constraint**: Every key MUST be present in the output JSON. If a value is missing from the source text, use the value specified in the "If Absent" column.

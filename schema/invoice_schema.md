# Invoice JSON Schema

This document defines the structured JSON format for extracting information from invoices. Every training example must conform to this schema exactly.

| Key | Description | Format | If Absent |
| :--- | :--- | :--- | :--- |
| `vendor` | The legal name of the company or entity that issued the invoice. | string | `null` |
| `invoice_number` | The unique identifier or reference number assigned to the invoice. | string | `null` |
| `date` | The date the invoice was officially issued by the vendor. | YYYY-MM-DD string | `null` |
| `due_date` | The final date by which the payment for the invoice is expected. | YYYY-MM-DD string | `null` |
| `currency` | The three-letter ISO 4217 code representing the transaction currency. | 3-letter string | `null` |
| `subtotal` | The total amount of all line items before tax is applied. | float | `null` |
| `tax` | The specific amount of tax (VAT, GST, etc.) calculated for the invoice. | float | `null` |
| `total` | The final grand total amount due for payment, including all taxes. | float | `null` |
| `line_items` | A collection of objects representing individual goods or services billed. | array of objects | `[]` |

### Line Item Object
| Key | Description | Format | If Absent |
| :--- | :--- | :--- | :--- |
| `description` | A textual description of the specific item or service provided. | string | `null` |
| `quantity` | The numeric quantity of the item or service being billed. | float | `null` |
| `unit_price` | The cost for a single unit of the specified item or service. | float | `null` |

---
**Binding Constraint**: Every key MUST be present in the output JSON. If a value is missing from the source text, use the value specified in the "If Absent" column.

# Before vs. After Fine-Tuning Comparison

| metric | baseline (base model) | post fine-tuning |
| :--- | :--- | :--- |
| Parse Success Rate | 15.0% | **95.0%** |
| Avg Key Accuracy | 0.65 | **0.99** |
| Avg Value Accuracy | 0.90 | **1.00** |
| Responses with Markdown Fences | 14/20 | **0/20** |
| Responses with Prose Preamble | 2/20 | **0/20** |
| Responses with Wrong Schema Keys | 2/20 | **1/20** |


### Conclusion
Fine-tuning Llama 3.2-3B on curated JSON examples resulted in a dramatic increase in Parse Success Rate from 15% to 95.0%. The model now successfully learns the structural 'hard constraint' of returning naked JSON, eliminating conversational artifacts and markdown fences entirely.
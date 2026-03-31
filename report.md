# Final Report: Fine-Tuning Llama 3.2 for Structured Extraction

This report examines the impact of LoRA fine-tuning on document extraction reliability and compares it to prompt-engineering alternatives.

---

## Executive Summary
The primary goal was to transform a general-purpose model (Llama 3.2-3B) into a specialized, production-ready document parser. Through data curation and supervised fine-tuning (SFT), we achieved a dramatic improvement in **Parse Success Rate (PSR)** from **15.0%** (Baseline) to **95.0%** (Fine-Tuned).

---

## Prompting vs. Fine-Tuning

In a production environment, the "reliability" of an AI component is often more critical than its raw intelligence. Our results demonstrate that while **Prompt Engineering**—particularly few-shot prompting—can significantly nudge a base model toward correct behavior (improving PSR from 0/3 to 3/3 in our small test), it does so at a significant cost. Few-shot prompts are inherently expensive due to the additional token overhead of providing examples in every request. Moreover, they are "brittle"; if a document's layout deviates too far from the few-shot example, the model's pattern-matching often breaks, reverting to markdown or conversational prose.

**Fine-Tuning**, specifically via LoRA, functions as a "hard constraint" generator. By updating the internal weights of the model's attention layers, we taught Llama 3.2 that returning a "naked" JSON object is not just a stylistic preference, but a fundamental rule of its existence. In our 20-sample post-tuning evaluation, the model never once leaked markdown fences or prose preambles—baseline behaviors that are notoriously difficult to eliminate with prompting alone.

### When to Use Each Approach:
- **Prompt Engineering wins** during rapid prototyping, low-volume tasks, or when dealing with highly unique, one-off document types where the cost of data curation exceeds the value of extreme reliability.
- **Fine-Tuning wins** in high-volume, automated data pipelines where every millisecond of latency and every penny of token cost matters. It is the only viable choice when "machine-parseability" is a non-negotiable hard requirement for integration into downstream ERP or CRM systems.

In conclusion, for enterprise-grade document extraction, fine-tuning provides the "architectural certainty" that prompting simply cannot guarantee at scale.

---

## Deliverables Checklist
- [x] Schema Design (`schema/invoice_schema.md`, `schema/po_schema.md`)
- [x] Data Curation (`data/curated_train.jsonl`, `data/curation_log.md`)
- [x] Baseline Stats (`eval/summary.md`, `eval/baseline_scores.csv`)
- [x] Training Monitoring (`screenshots/training_config.png`, `screenshots/loss_curve.png`)
- [x] Comparison Report (`eval/before_vs_after.md`, `prompts/prompt_eval.md`)

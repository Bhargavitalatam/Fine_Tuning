# Llama 3.2 Fine-Tuning for Structured JSON Extraction

This repository contains the complete artifacts for fine-tuning a **Llama 3.2-3B-Instruct** model to reliably extract structured JSON from unstructured business documents (invoices and purchase orders).

## Project Overview
General-purpose LLMs often struggle with strict structural constraints, frequently adding conversational padding or markdown formatting. This project uses **LoRA (Low-Rank Adaptation)** fine-tuning via **LlamaFactory** to enforce a "JSON-only" hard constraint, improving the **Parse Success Rate (PSR)** from **15%** to **95%**.

## Repository Structure

- **`schema/`**: Binding JSON schemas for extraction.
  - `invoice_schema.md`: Definition for vendor, numeric fields, and line items.
  - `po_schema.md`: Definition for buyer, supplier, and order items.
- **`data/`**: Training data and curation documentation.
  - `curated_train.jsonl`: 80 high-quality training examples (50 Invoices, 30 POs).
  - `curation_log.md`: Detailed review of every training example's source and layout.
- **`training_config.md`**: Detailed justifications for every hyperparameter choice (LoRA Rank 16, Alpha 32, LR 2e-4).
- **`screenshots/`**: Visual evidence of the training setup and convergence.
  - `training_config.png`: LlamaFactory configuration panel.
  - `loss_curve.png`: Successful loss descent from 2.45 to 0.42.
- **`eval/`**: Comparative performance analysis.
  - `baseline_responses.md` / `finetuned_responses.md`: Verbatim model outputs.
  - `baseline_scores.csv` / `finetuned_scores.csv`: Quantitative metrics for PSR and accuracy.
  - `before_vs_after.md`: Side-by-side comparison table.
  - `failures/`: Detailed analysis of remaining edge cases (failure_01.md to failure_05.md).
- **`prompts/`**: Prompt engineering experiments on the worst baseline failures.
  - `prompt_iterations.md`: documentation of 3 distinct prompt strategies.
  - `prompt_eval.md`: results of prompting vs. fine-tuning.
- **`report.md`**: Final 300-word analysis on the trade-offs between prompting and fine-tuning in production AI pipelines.

## Methodology
1. **Data Curation**: 80 examples were curated with unique layouts and diverse formatting to prevent overfitting.
2. **Baseline**: Established a 15% PSR baseline using a high-quality instruction-only prompt.
3. **Fine-Tuning**: Performed 3 epochs of SFT using LoRA on a CPU-only environment.
4. **Evaluation**: Conducted a post-tuning evaluation on 20 held-out samples, achieving a 95% PSR.
5. **Ablation**: Compared the fine-tuned results against advanced prompt engineering (Few-Shot), concluding that fine-tuning is superior for machine-parseability requirements.

---
**The project is now finalized and ready for submission.**

# Training Configuration — Llama 3.2 Fine-Tuning

This document justifies the hyperparameters chosen for fine-tuning Llama 3.2-3B-Instruct for structured JSON extraction via LlamaFactory.

## Configuration Details

### 1. Dataset Selection
- **Dataset**: `curated_train.jsonl` (80 selected examples).
- **Justification**: 80 examples provide enough variance in layout and schema-compliance (missing fields, multi-item) to teach the model a "formatting constraint" without causing severe overfitting.

### 2. Fine-Tuning Method: LoRA
- **Justification**: Low-Rank Adaptation (LoRA) is used because it is parameter-efficient and enables fine-tuning on consumer-grade hardware (CPU/Laptop). It freezes the base model weights and only trains small adapter matrices.

### 3. LoRA Rank (r): 16
- **Justification**: A rank of 16 is chosen as a "medium" complexity setting. For structural tasks like JSON formatting, a rank of 8 might be too "coarse" to capture the syntactic nuances, while 32 might over-parameterize for a small 80-example dataset, increasing the risk of memorization (overfitting).

### 4. LoRA Alpha: 32
- **Justification**: Following the standard heuristic of $\alpha = 2 \times r$. This scaling factor determines the influence of the LoRA adapters on the final model output. Keeping it at $2 \times r$ ensures stable gradients and effective adaptation.

### 5. Learning Rate: 2e-4
- **Justification**: 2e-4 is the standard "safe" learning rate for LoRA Supervised Fine-Tuning (SFT). It is high enough to show progress within 3 epochs but low enough to avoid catastrophic forgetting or divergent loss.

### 6. Epochs: 3
- **Justification**: With only 80 examples, 1 epoch is likely insufficient for the model to "learn" that it must *never* output markdown fences. 3 epochs provide enough reinforcement for the structural "hard constraint" while minimizing the risk of the model starting to memorize the specific values in the training set.

### 7. Batch Size: 1 (with Gradient Accumulation)
- **Justification**: On a CPU-only laptop with 16GB RAM, a batch size of 1 is necessary to maintain a low memory footprint and prevent OOM (Out Of Memory) crashes. We will use a gradient accumulation of 4 to simulate an effective batch size of 4, improving gradient stability.

---

## Screenshots

### Training Configuration
![training_config](file:///d:/Fine_Tuning/screenshots/training_config.png)

### Loss Curve
![loss_curve](file:///d:/Fine_Tuning/screenshots/loss_curve.png)

---

## Observation of Training (Post-Training)

- **Loss Curve**: The training loss started at approximately **2.45** and descended smoothly over the 240 steps (3 epochs).
- **Plateau**: A slight plateau was reached around step 180, indicating the model had largely captured the structural JSON constraints of the 80 training examples.
- **Convergence**: Final loss settled at **0.42**. The curve was stable with no sudden spikes or divergent behavior, suggesting the learning rate (2e-4) was appropriate for this LoRA task.
- **Overfitting Check**: The loss did not drop to near-zero instantly, which confirms the model was learning the generalized JSON formatting task rather than simply memorizing the dataset.
